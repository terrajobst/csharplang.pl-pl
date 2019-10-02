---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704085"
---
# <a name="expressions"></a><span data-ttu-id="bd998-101">Wyrażenia</span><span class="sxs-lookup"><span data-stu-id="bd998-101">Expressions</span></span>

<span data-ttu-id="bd998-102">Wyrażenie jest sekwencją operatorów i argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="bd998-103">Ten rozdział definiuje składnię, kolejność oceny argumentów operacji i operatorów oraz znaczenie wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="bd998-104">Klasyfikacje wyrażeń</span><span class="sxs-lookup"><span data-stu-id="bd998-104">Expression classifications</span></span>

<span data-ttu-id="bd998-105">Wyrażenie jest klasyfikowane jako jedno z następujących:</span><span class="sxs-lookup"><span data-stu-id="bd998-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="bd998-106">Wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-106">A value.</span></span> <span data-ttu-id="bd998-107">Każda wartość ma skojarzony typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="bd998-108">Zmienna.</span><span class="sxs-lookup"><span data-stu-id="bd998-108">A variable.</span></span> <span data-ttu-id="bd998-109">Każda zmienna ma skojarzony typ, a mianowicie zadeklarowany typ zmiennej.</span><span class="sxs-lookup"><span data-stu-id="bd998-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="bd998-110">Przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="bd998-110">A namespace.</span></span> <span data-ttu-id="bd998-111">Wyrażenie z tą klasyfikacją może występować tylko jako lewa strona *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="bd998-112">W każdym innym kontekście wyrażenie sklasyfikowane jako przestrzeń nazw powoduje błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="bd998-113">Typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-113">A type.</span></span> <span data-ttu-id="bd998-114">Wyrażenie z tą klasyfikacją może występować tylko jako lewa strona *member_access* ([dostęp do składowej](expressions.md#member-access)) lub jako operand dla operatora `as` ([operator as](expressions.md#the-as-operator)), operator `is` ([operator is](expressions.md#the-is-operator)) lub @no __t-6 — operator ([operator typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="bd998-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="bd998-115">W każdym innym kontekście wyrażenie sklasyfikowane jako typ powoduje błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="bd998-116">Grupa metod, która jest zestawem przeciążonych metod wynikających z odnośnika elementu członkowskiego ([wyszukiwanie elementu członkowskiego](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="bd998-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="bd998-117">Grupa metod może mieć skojarzone wyrażenie wystąpienia i listę powiązanych argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="bd998-118">Gdy wywoływana jest metoda wystąpienia, wynik oceny wyrażenia wystąpienia zmieni się na wystąpienie reprezentowane przez `this` ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="bd998-119">Grupa metod jest dozwolona w elemencie *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)), *delegate_creation_expression* ([wyrażenie tworzenia delegatów](expressions.md#delegate-creation-expressions)) i jako lewa strona operatora is i może być niejawnie konwertowane do zgodnego typu delegata ([konwersje grup metod](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="bd998-120">W każdym innym kontekście wyrażenie sklasyfikowane jako grupa metod powoduje błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="bd998-121">Literał o wartości null.</span><span class="sxs-lookup"><span data-stu-id="bd998-121">A null literal.</span></span> <span data-ttu-id="bd998-122">Wyrażenie z tą klasyfikacją może być niejawnie konwertowane na typ referencyjny lub typ dopuszczający wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="bd998-123">Funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="bd998-123">An anonymous function.</span></span> <span data-ttu-id="bd998-124">Wyrażenie z tą klasyfikacją może być niejawnie konwertowane na zgodny typ delegata lub typ drzewa wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="bd998-125">Dostęp do właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd998-125">A property access.</span></span> <span data-ttu-id="bd998-126">Każdy dostęp do właściwości ma skojarzony typ, a nie typ właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd998-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="bd998-127">Ponadto dostęp do właściwości może mieć skojarzone wyrażenie wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="bd998-128">Gdy wywoływana jest metoda dostępu (`get` lub `set`) właściwości wystąpienia, wynik oceny wyrażenia wystąpienia zmieni się na wystąpienie reprezentowane przez `this` ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="bd998-129">Dostęp do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-129">An event access.</span></span> <span data-ttu-id="bd998-130">Każdy dostęp do zdarzenia ma skojarzony typ, a nie typ zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="bd998-131">Ponadto dostęp do zdarzeń może mieć skojarzone wyrażenie wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="bd998-132">Dostęp do zdarzenia może być wyświetlany jako lewy argument operacji dla operatorów `+=` i `-=` ([przypisanie zdarzenia](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="bd998-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="bd998-133">W każdym innym kontekście wyrażenie sklasyfikowane jako dostęp do zdarzenia powoduje błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="bd998-134">Dostęp indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-134">An indexer access.</span></span> <span data-ttu-id="bd998-135">Każdy dostęp indeksatora ma skojarzony typ, a mianowicie typ elementu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="bd998-136">Ponadto dostęp indeksatora ma skojarzone wyrażenie wystąpienia i skojarzoną listę argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="bd998-137">Gdy wywoływany jest dostęp (`get` lub `set` bloku) dostępu indeksatora, wynik oceny wyrażenia wystąpienia zmieni się na wystąpienie reprezentowane przez `this` ([dostęp](expressions.md#this-access)), a wynik oceny listy argumentów zmieni się na Lista parametrów wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="bd998-138">Wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-138">Nothing.</span></span> <span data-ttu-id="bd998-139">Dzieje się tak, gdy wyrażenie jest wywołaniem metody z typem zwracanym `void`.</span><span class="sxs-lookup"><span data-stu-id="bd998-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="bd998-140">Wyrażenie sklasyfikowane jako Nothing jest prawidłowe tylko w kontekście *statement_expression* ([instrukcji wyrażenia](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="bd998-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="bd998-141">Końcowy wynik wyrażenia nigdy nie jest obszarem nazw, typem, grupą metod lub dostępem do zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="bd998-142">Zamiast tego, jak wspomniano powyżej, te kategorie wyrażeń są konstrukcjami pośrednimi, które są dozwolone tylko w określonych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="bd998-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="bd998-143">Dostęp do właściwości lub dostęp do indeksatora jest zawsze ponownie klasyfikowany jako wartość przez wykonanie wywołania *metody dostępu get* lub *set metody*dostępu.</span><span class="sxs-lookup"><span data-stu-id="bd998-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="bd998-144">Określony akcesor jest określany przez kontekst dostępu właściwości lub indeksatora: Jeśli dostęp jest obiektem docelowym przypisania, *metoda dostępu set* jest wywoływana, aby przypisać nową wartość ([przypisanie proste](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="bd998-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="bd998-145">W przeciwnym razie *metoda dostępu get* jest wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="bd998-146">Wartości wyrażeń</span><span class="sxs-lookup"><span data-stu-id="bd998-146">Values of expressions</span></span>

<span data-ttu-id="bd998-147">Większość konstrukcji obejmujących wyrażenie ostatecznie wymaga wyrażenia do określenia ***wartości***.</span><span class="sxs-lookup"><span data-stu-id="bd998-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="bd998-148">W takich przypadkach, jeśli wyrażenie rzeczywiste wskazuje przestrzeń nazw, typ, grupę metod lub wartość Nothing, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="bd998-149">Jeśli jednak wyrażenie oznacza dostęp do właściwości, dostęp indeksatora lub zmienną, wartość właściwości, indeksatora lub zmiennej jest niejawnie zastępowana:</span><span class="sxs-lookup"><span data-stu-id="bd998-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="bd998-150">Wartość zmiennej jest po prostu wartością przechowywaną obecnie w lokalizacji magazynu identyfikowanej przez zmienną.</span><span class="sxs-lookup"><span data-stu-id="bd998-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="bd998-151">Zmienna musi być traktowana jako czasowo przypisana ([przypisanie](variables.md#definite-assignment)), zanim będzie można uzyskać jej wartość lub w przeciwnym razie wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="bd998-152">Wartość wyrażenia dostępu do właściwości jest uzyskiwana przez wywołanie *metody dostępu get* właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd998-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="bd998-153">Jeśli właściwość nie ma *metody dostępu get*, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="bd998-154">W przeciwnym razie jest wykonywane wywołanie elementu członkowskiego ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a wynik wywołania będzie wartością wyrażenia dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd998-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="bd998-155">Wartość wyrażenia dostępu indeksatora jest uzyskiwana przez wywołanie *metody dostępu get* indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="bd998-156">Jeśli indeksator nie ma *metody dostępu get*, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="bd998-157">W przeciwnym razie wywołanie elementu członkowskiego funkcji ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jest wykonywane z listą argumentów skojarzoną z wyrażeniem dostępu indeksatora, a wynik wywołania będzie wartością dostępu indeksatora wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="bd998-158">Statyczne i dynamiczne powiązanie</span><span class="sxs-lookup"><span data-stu-id="bd998-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="bd998-159">Proces określania znaczenia operacji na podstawie typu lub wartości wyrażeń elementów (argumentów, operandów, odbiorników) jest często określany jako ***powiązanie***.</span><span class="sxs-lookup"><span data-stu-id="bd998-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="bd998-160">Na przykład znaczenie wywołania metody jest określane na podstawie typu odbiorcy i argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="bd998-161">Znaczenie operatora jest określane na podstawie typu jego operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="bd998-162">W C# znaczeniu operacji jest zwykle określane w czasie kompilacji na podstawie typu czasu kompilowania jego wyrażeń składowych.</span><span class="sxs-lookup"><span data-stu-id="bd998-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="bd998-163">Podobnie, jeśli wyrażenie zawiera błąd, błąd jest wykrywany i raportowany przez kompilator.</span><span class="sxs-lookup"><span data-stu-id="bd998-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="bd998-164">To podejście jest znane jako ***powiązanie statyczne***.</span><span class="sxs-lookup"><span data-stu-id="bd998-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="bd998-165">Jeśli jednak wyrażenie jest wyrażeniem dynamicznym (tj. ma typ `dynamic`), oznacza to, że wszystkie powiązania, w których uczestniczy, powinny być oparte na typie czasu wykonywania (tj. rzeczywisty typ obiektu, który jest w czasie wykonywania), a nie typ, który ma czas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="bd998-166">W związku z tym powiązanie takiej operacji jest odroczone do czasu, w którym operacja ma zostać wykonana podczas uruchamiania programu.</span><span class="sxs-lookup"><span data-stu-id="bd998-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="bd998-167">Nazywa się to ***dynamicznym wiązaniem***.</span><span class="sxs-lookup"><span data-stu-id="bd998-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="bd998-168">Gdy operacja jest powiązana dynamicznie, w kompilatorze nie jest wykonywane żadne sprawdzenie.</span><span class="sxs-lookup"><span data-stu-id="bd998-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="bd998-169">Zamiast tego, Jeśli powiązanie czasu wykonywania nie powiedzie się, błędy są raportowane jako wyjątki w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bd998-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="bd998-170">Następujące operacje w programie C# podlegają powiązaniu:</span><span class="sxs-lookup"><span data-stu-id="bd998-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="bd998-171">Dostęp do elementu członkowskiego: `e.M`</span><span class="sxs-lookup"><span data-stu-id="bd998-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="bd998-172">Wywołanie metody: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="bd998-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="bd998-173">Delegowanie wywołania: `e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="bd998-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="bd998-174">Dostęp do elementów: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="bd998-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="bd998-175">Tworzenie obiektu: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="bd998-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="bd998-176">Przeciążone operatory jednoargumentowe: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="bd998-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="bd998-177">Przeciążone operatory binarne: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, 0, 1, 2, 3, 4, 5, 6, 7, 8</span><span class="sxs-lookup"><span data-stu-id="bd998-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="bd998-178">Operatory przypisania: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, 0</span><span class="sxs-lookup"><span data-stu-id="bd998-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="bd998-179">Konwersje niejawne i jawne</span><span class="sxs-lookup"><span data-stu-id="bd998-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="bd998-180">Gdy nie są używane żadne wyrażenia dynamiczne C# , domyślnie jest to powiązanie statyczne, co oznacza, że typy wyrażeń elementów w czasie kompilacji są stosowane w procesie wyboru.</span><span class="sxs-lookup"><span data-stu-id="bd998-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="bd998-181">Jeśli jednak jeden z wyrażeń składowych w wymienionych powyżej operacjach jest wyrażeniem dynamicznym, operacja jest zamiast tego dynamicznie powiązana.</span><span class="sxs-lookup"><span data-stu-id="bd998-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="bd998-182">Czas powiązania</span><span class="sxs-lookup"><span data-stu-id="bd998-182">Binding-time</span></span>

<span data-ttu-id="bd998-183">Statyczne powiązanie odbywa się w czasie kompilacji, natomiast dynamiczne wiązanie odbywa się w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bd998-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="bd998-184">W poniższych sekcjach termin ***czas wiązania*** odnosi się do czasu kompilacji lub czasu wykonywania w zależności od tego, kiedy powiązanie ma miejsce.</span><span class="sxs-lookup"><span data-stu-id="bd998-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="bd998-185">Poniższy przykład ilustruje koncepcje statycznego i dynamicznego powiązania oraz czas powiązania:</span><span class="sxs-lookup"><span data-stu-id="bd998-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="bd998-186">Pierwsze dwa wywołania są statycznie powiązane: Przeciążenie `Console.WriteLine` jest wybierane w oparciu o typ czasu kompilacji ich argumentu.</span><span class="sxs-lookup"><span data-stu-id="bd998-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="bd998-187">W rezultacie czas powiązania to czas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="bd998-188">Trzecie wywołanie jest powiązane dynamicznie: Przeciążenie `Console.WriteLine` jest wybierane na podstawie typu czasu wykonywania tego argumentu.</span><span class="sxs-lookup"><span data-stu-id="bd998-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="bd998-189">Dzieje się tak, ponieważ argument jest wyrażeniem dynamicznym — typem czasu kompilacji jest `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="bd998-190">W rezultacie czas powiązania dla trzeciego wywołania to czas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bd998-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="bd998-191">Powiązanie dynamiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-191">Dynamic binding</span></span>

<span data-ttu-id="bd998-192">Celem powiązania dynamicznego jest umożliwienie C# programom współdziałania z ***obiektami dynamicznymi***, np. obiektów, które nie są zgodne z normalnymi C# regułami typu System.</span><span class="sxs-lookup"><span data-stu-id="bd998-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="bd998-193">Obiekty dynamiczne mogą być obiektami z innych języków programowania z różnymi typami systemów lub mogą być obiektami, które są programowo instalatorem do implementowania własnej semantyki powiązań dla różnych operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="bd998-194">Mechanizm, za pomocą którego obiekt dynamiczny implementuje swoją własną semantykę, ma zdefiniowaną implementację.</span><span class="sxs-lookup"><span data-stu-id="bd998-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="bd998-195">Określony interfejs — ponownie wdrożony — jest implementowany przez obiekty dynamiczne, aby sygnalizować czas C# wykonywania, który ma specjalną semantykę.</span><span class="sxs-lookup"><span data-stu-id="bd998-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="bd998-196">Z tego względu, gdy operacje na obiekcie dynamicznym są dynamicznie powiązane, ich własnej semantyki powiązania, a C# nie z określonymi w tym dokumencie, przejmowanie.</span><span class="sxs-lookup"><span data-stu-id="bd998-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="bd998-197">Chociaż celem powiązania dynamicznego jest umożliwienie współdziałania z obiektami dynamicznymi, C# zezwala na dynamiczne wiązanie dla wszystkich obiektów, niezależnie od tego, czy są one dynamiczne, czy nie.</span><span class="sxs-lookup"><span data-stu-id="bd998-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="bd998-198">Pozwala to na bezproblemowe integrację obiektów dynamicznych, ponieważ wyniki operacji na nich mogą nie być obiektami dynamicznymi, ale nadal są typu nieznanego dla programisty w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="bd998-199">Ponadto dynamiczne powiązanie może pomóc wyeliminować podatny na błędy kod oparty na odbiciu nawet wtedy, gdy żadne obiekty nie są obiektami dynamicznymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="bd998-200">W poniższych sekcjach opisano dla każdej konstrukcji w języku dokładnie, gdy jest stosowane dynamiczne wiązanie, jakie jest sprawdzanie czasu kompilacji — jeśli jest stosowana, a także wynik klasyfikacji wyników i wyrażeń w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="bd998-201">Typy wyrażeń składowych</span><span class="sxs-lookup"><span data-stu-id="bd998-201">Types of constituent expressions</span></span>

<span data-ttu-id="bd998-202">Gdy operacja jest statyczna, typ wyrażenia elementu (np. odbiorca, argument, indeks lub operand) jest zawsze uznawany za typ czasu kompilacji tego wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="bd998-203">Gdy operacja jest powiązana dynamicznie, typ wyrażenia elementu składowego jest określany na różne sposoby w zależności od typu czasu kompilacji w wyrażeniu składnika:</span><span class="sxs-lookup"><span data-stu-id="bd998-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="bd998-204">Wyrażenie elementu typu "Kompilacja" `dynamic` jest uznawane za posiadające typ wartości rzeczywistej, która jest wynikiem wyrażenia w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="bd998-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="bd998-205">Wyrażenie elementu składowego, którego typem czasu kompilacji jest parametr typu, jest uznawany za posiadający typ, z którym jest powiązany parametr typu w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="bd998-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="bd998-206">W przeciwnym razie wyrażenie składowe jest uznawane za mające typ czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="bd998-207">Operatory</span><span class="sxs-lookup"><span data-stu-id="bd998-207">Operators</span></span>

<span data-ttu-id="bd998-208">Wyrażenia są zbudowane z ***argumentów operacji*** i ***operatorów***.</span><span class="sxs-lookup"><span data-stu-id="bd998-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="bd998-209">Operatory wyrażenia wskazują operacje do zastosowania dla operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="bd998-210">Przykłady operatorów to `+`, `-`, `*`, `/`i `new`.</span><span class="sxs-lookup"><span data-stu-id="bd998-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="bd998-211">Przykładami operandów są literały, pola, zmienne lokalne i wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="bd998-212">Istnieją trzy rodzaje operatorów:</span><span class="sxs-lookup"><span data-stu-id="bd998-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="bd998-213">Operatory jednoargumentowe.</span><span class="sxs-lookup"><span data-stu-id="bd998-213">Unary operators.</span></span> <span data-ttu-id="bd998-214">Operatory jednoargumentowe przyjmują jeden operand i używają obu prefiksów (takich jak `--x`) lub notacji przyrostkowej (na przykład `x++`).</span><span class="sxs-lookup"><span data-stu-id="bd998-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="bd998-215">Operatory binarne.</span><span class="sxs-lookup"><span data-stu-id="bd998-215">Binary operators.</span></span> <span data-ttu-id="bd998-216">Operatory binarne przyjmują dwa operandy i wszystkie używają notacji wrostkowe (na przykład `x + y`).</span><span class="sxs-lookup"><span data-stu-id="bd998-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="bd998-217">Operator Trzyelementowy.</span><span class="sxs-lookup"><span data-stu-id="bd998-217">Ternary operator.</span></span> <span data-ttu-id="bd998-218">Tylko jeden operator Trzyelementowy, `?:`, istnieje; przyjmuje trzy operandy i używa notacji wrostkowe (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="bd998-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="bd998-219">Kolejność obliczania operatorów w wyrażeniu jest określana przez ***pierwszeństwo*** i ***łączność*** operatorów ([pierwszeństwo operatorów i łączność](expressions.md#operator-precedence-and-associativity)).</span><span class="sxs-lookup"><span data-stu-id="bd998-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="bd998-220">Argumenty operacji w wyrażeniu są oceniane od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="bd998-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="bd998-221">Na przykład w `F(i) + G(i++) * H(i)` Metoda `F` jest wywoływana przy użyciu starej wartości `i`, a następnie Metoda `G` jest wywoływana z starą wartością `i`, a wreszcie Metoda `H` jest wywoływana z nową wartością `i`.</span><span class="sxs-lookup"><span data-stu-id="bd998-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="bd998-222">Jest to niezależne od i niepowiązane z pierwszeństwem operatorów.</span><span class="sxs-lookup"><span data-stu-id="bd998-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="bd998-223">Niektóre operatory mogą być ***przeciążone***.</span><span class="sxs-lookup"><span data-stu-id="bd998-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="bd998-224">Przeciążanie operatora zezwala na określenie implementacji operatora zdefiniowanego przez użytkownika dla operacji, w których jeden lub oba operandy są typu klasy lub struktury zdefiniowanej przez użytkownika ([przeciążanie operatora](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="bd998-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="bd998-225">Pierwszeństwo operatorów i łączność</span><span class="sxs-lookup"><span data-stu-id="bd998-225">Operator precedence and associativity</span></span>

<span data-ttu-id="bd998-226">Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególne operatory.</span><span class="sxs-lookup"><span data-stu-id="bd998-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="bd998-227">Na przykład wyrażenie `x + y * z` jest oceniane jako `x + (y * z)`, ponieważ operator `*` ma wyższy priorytet niż binarny operator `+`.</span><span class="sxs-lookup"><span data-stu-id="bd998-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="bd998-228">Pierwszeństwo operatora jest ustanawiane przez definicję skojarzonej produkcji gramatyki.</span><span class="sxs-lookup"><span data-stu-id="bd998-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="bd998-229">Na przykład *additive_expression* składa się z sekwencji *multiplicative_expression*s oddzielonych operatorami `+` lub `-`, w związku z czym operatory `+` i `-` mają niższy priorytet niż `*`, `/` i @no__ Operatory t-8.</span><span class="sxs-lookup"><span data-stu-id="bd998-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="bd998-230">Poniższa tabela podsumowuje wszystkie operatory w kolejności od najwyższego do najniższego:</span><span class="sxs-lookup"><span data-stu-id="bd998-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="bd998-231">__Paragraf__</span><span class="sxs-lookup"><span data-stu-id="bd998-231">__Section__</span></span>                                                                                   | <span data-ttu-id="bd998-232">__Kategoria__</span><span class="sxs-lookup"><span data-stu-id="bd998-232">__Category__</span></span>                | <span data-ttu-id="bd998-233">__Operatory__</span><span class="sxs-lookup"><span data-stu-id="bd998-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="bd998-234">Wyrażenia podstawowe</span><span class="sxs-lookup"><span data-stu-id="bd998-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="bd998-235">Podstawowy</span><span class="sxs-lookup"><span data-stu-id="bd998-235">Primary</span></span>                     | <span data-ttu-id="bd998-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="bd998-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="bd998-237">Operatory jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="bd998-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="bd998-238">Jednostk</span><span class="sxs-lookup"><span data-stu-id="bd998-238">Unary</span></span>                       | <span data-ttu-id="bd998-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="bd998-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="bd998-240">Operatory arytmetyczne</span><span class="sxs-lookup"><span data-stu-id="bd998-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="bd998-241">Mnożeniowy</span><span class="sxs-lookup"><span data-stu-id="bd998-241">Multiplicative</span></span>              | <span data-ttu-id="bd998-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="bd998-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="bd998-243">Operatory arytmetyczne</span><span class="sxs-lookup"><span data-stu-id="bd998-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="bd998-244">Dana</span><span class="sxs-lookup"><span data-stu-id="bd998-244">Additive</span></span>                    | <span data-ttu-id="bd998-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="bd998-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="bd998-246">Operatory przesunięcia</span><span class="sxs-lookup"><span data-stu-id="bd998-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="bd998-247">Shift</span><span class="sxs-lookup"><span data-stu-id="bd998-247">Shift</span></span>                       | <span data-ttu-id="bd998-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="bd998-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="bd998-249">Operatory relacyjne i testowe typu</span><span class="sxs-lookup"><span data-stu-id="bd998-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="bd998-250">Testowanie relacyjne i typu</span><span class="sxs-lookup"><span data-stu-id="bd998-250">Relational and type testing</span></span> | <span data-ttu-id="bd998-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="bd998-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="bd998-252">Operatory relacyjne i testowe typu</span><span class="sxs-lookup"><span data-stu-id="bd998-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="bd998-253">Równości</span><span class="sxs-lookup"><span data-stu-id="bd998-253">Equality</span></span>                    | <span data-ttu-id="bd998-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="bd998-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="bd998-255">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="bd998-256">Logicznego AND</span><span class="sxs-lookup"><span data-stu-id="bd998-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="bd998-257">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="bd998-258">Logicznego XOR</span><span class="sxs-lookup"><span data-stu-id="bd998-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="bd998-259">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="bd998-260">Logicznego OR</span><span class="sxs-lookup"><span data-stu-id="bd998-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="bd998-261">Warunkowe operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="bd998-262">Warunkowego AND</span><span class="sxs-lookup"><span data-stu-id="bd998-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="bd998-263">Warunkowe operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="bd998-264">Warunkowego OR</span><span class="sxs-lookup"><span data-stu-id="bd998-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="bd998-265">Operator łączenia wartości null</span><span class="sxs-lookup"><span data-stu-id="bd998-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="bd998-266">Łączenia wartości null</span><span class="sxs-lookup"><span data-stu-id="bd998-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="bd998-267">Operator warunkowy</span><span class="sxs-lookup"><span data-stu-id="bd998-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="bd998-268">Warunkowe</span><span class="sxs-lookup"><span data-stu-id="bd998-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="bd998-269">[Operatory przypisania](expressions.md#assignment-operators), [wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="bd998-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="bd998-270">Przypisanie i wyrażenie lambda</span><span class="sxs-lookup"><span data-stu-id="bd998-270">Assignment and lambda expression</span></span> | <span data-ttu-id="bd998-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="bd998-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="bd998-272">Gdy operand występuje między dwoma operatorami o takim samym priorytecie, łączność operatorów kontroluje kolejność wykonywania operacji:</span><span class="sxs-lookup"><span data-stu-id="bd998-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="bd998-273">Z wyjątkiem operatorów przypisania i operatora łączenia wartości null, wszystkie operatory binarne są z ***lewej strony skojarzenia***, co oznacza, że operacje są wykonywane od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="bd998-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="bd998-274">Na przykład, `x + y + z` jest oceniane jako `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="bd998-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="bd998-275">Operatory przypisania, operator łączenia wartości null i operator warunkowy (`?:`) są ***z prawej strony skojarzenia***, co oznacza, że operacje są wykonywane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="bd998-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="bd998-276">Na przykład, `x = y = z` jest oceniane jako `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="bd998-277">Pierwszeństwo i asocjacyjność mogą być kontrolowane za pomocą nawiasów.</span><span class="sxs-lookup"><span data-stu-id="bd998-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="bd998-278">Na przykład `x + y * z` najpierw mnoży `y` przez `z` , a następnie dodaje wynik do `x`, ale `(x + y) * z` najpierw dodaje `x` i `y` i następnie wynik mnoży przez `z`.</span><span class="sxs-lookup"><span data-stu-id="bd998-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="bd998-279">Przeładowanie operatora</span><span class="sxs-lookup"><span data-stu-id="bd998-279">Operator overloading</span></span>

<span data-ttu-id="bd998-280">Wszystkie operatory jednoargumentowe i binarne mają wstępnie zdefiniowane implementacje, które są automatycznie dostępne w dowolnym wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="bd998-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="bd998-281">Oprócz wstępnie zdefiniowanych implementacji implementacje zdefiniowane przez użytkownika można wprowadzać przez uwzględnienie deklaracji `operator` w klasach i strukturach ([Operatory](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="bd998-282">Implementacje operatorów zdefiniowane przez użytkownika zawsze mają pierwszeństwo przed wstępnie zdefiniowanymi implementacjami operatorów: Tylko wtedy, gdy nie istnieją żadne odpowiednie implementacje operatorów zdefiniowane przez użytkownika, zostaną uznane wstępnie zdefiniowane implementacje operatora, zgodnie z opisem w [rozpoznawania przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [rozpoznawania przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="bd998-283">***Operatory jednoargumentowe*** z możliwością przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="bd998-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="bd998-284">Mimo że `true` i `false` nie są używane jawnie w wyrażeniach (i dlatego nie są uwzględnione w tabeli pierwszeństwa w [priorytecie operatora i łączność](expressions.md#operator-precedence-and-associativity)), są uznawane za operatory, ponieważ są wywoływane w kilku wyrażeniach Konteksty: wyrażenia logiczne ([wyrażenia logiczne](expressions.md#boolean-expressions)) i wyrażenia obejmujące warunkowe ([operator warunkowy](expressions.md#conditional-operator)) i warunkowe operatory logiczne ([warunkowe operatory logiczne](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="bd998-285">***Operatory binarne*** z możliwością przeciążenia są następujące:</span><span class="sxs-lookup"><span data-stu-id="bd998-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="bd998-286">Tylko operatory wymienione powyżej mogą być przeciążone.</span><span class="sxs-lookup"><span data-stu-id="bd998-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="bd998-287">W szczególności nie jest możliwe przeciążanie dostępu do składowych, wywoływanie metody lub `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, 0, 1 i 2 operatorów.</span><span class="sxs-lookup"><span data-stu-id="bd998-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="bd998-288">Gdy operator binarny jest przeciążony, odpowiedni operator przypisania, jeśli istnieje, również jest niejawnie przeciążony.</span><span class="sxs-lookup"><span data-stu-id="bd998-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="bd998-289">Na przykład Przeciążenie operatora `*` to również Przeciążenie operatora `*=`.</span><span class="sxs-lookup"><span data-stu-id="bd998-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="bd998-290">Opisano to dokładniej w [przypisaniu złożonym](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="bd998-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="bd998-291">Należy zauważyć, że sam operator przypisania (`=`) nie może być przeciążony.</span><span class="sxs-lookup"><span data-stu-id="bd998-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="bd998-292">Przypisanie zawsze wykonuje prostą kopię wartości w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="bd998-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="bd998-293">Operacje Cast, takie jak `(T)x`, są przeciążone przez dostarczanie zdefiniowanych przez użytkownika konwersji ([konwersje zdefiniowane przez użytkownika](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="bd998-294">Dostęp do elementu, taki jak `a[x]`, nie jest uważany za operatora z możliwością przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="bd998-295">Zamiast tego, zdefiniowane przez użytkownika indeksowanie jest obsługiwane za poorednictwem indeksatorów ([indeksatorów](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="bd998-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="bd998-296">W wyrażeniach operatory są wywoływane przy użyciu notacji operatora, a w deklaracjach operatory są odwoływane przy użyciu notacji funkcjonalnej.</span><span class="sxs-lookup"><span data-stu-id="bd998-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="bd998-297">W poniższej tabeli przedstawiono relacje między operatorami i notacjami funkcjonalnymi dla operatorów jednoargumentowych i binarnych.</span><span class="sxs-lookup"><span data-stu-id="bd998-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="bd998-298">W pierwszym wpisie *op* oznacza każdy jednoargumentowy operator prefiksu z możliwością przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="bd998-299">W drugim wpisie *op* oznacza przyrostk jednoargumentowy `++` i operatory `--`.</span><span class="sxs-lookup"><span data-stu-id="bd998-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="bd998-300">W trzecim wpisie *op* oznacza dowolny operator binarny z możliwością przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="bd998-301">__Notacja operatora__</span><span class="sxs-lookup"><span data-stu-id="bd998-301">__Operator notation__</span></span> | <span data-ttu-id="bd998-302">__Notacja funkcjonalna__</span><span class="sxs-lookup"><span data-stu-id="bd998-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="bd998-303">Deklaracje operatora zdefiniowane przez użytkownika zawsze wymagają, aby co najmniej jeden z parametrów był klasą lub typem struktury, który zawiera deklarację operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="bd998-304">Dlatego nie jest możliwe, aby operator zdefiniowany przez użytkownika miał taki sam podpis jak wstępnie zdefiniowany operator.</span><span class="sxs-lookup"><span data-stu-id="bd998-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="bd998-305">Deklaracje operatora zdefiniowane przez użytkownika nie mogą modyfikować składni, pierwszeństwa ani łącznośći operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="bd998-306">Na przykład operator `/` jest zawsze operatorem binarnym, zawsze ma poziom pierwszeństwa określony w [priorytecie operatora i łączność](expressions.md#operator-precedence-and-associativity), a zawsze jest w pełni asocjacyjny.</span><span class="sxs-lookup"><span data-stu-id="bd998-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="bd998-307">Chociaż jest możliwe, aby operator zdefiniowany przez użytkownika wykonywał jakiekolwiek obliczenia, należy zdecydowanie odradzać implementacje, które generują wyniki inne niż te, które są w sposób intuicyjny.</span><span class="sxs-lookup"><span data-stu-id="bd998-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="bd998-308">Na przykład implementacja `operator ==` powinna porównać dwa operandy dla równości i zwrócić odpowiedni wynik `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="bd998-309">Opisy poszczególnych operatorów w [wyrażeniach podstawowych](expressions.md#primary-expressions) za pomocą [warunkowych operatorów logicznych](expressions.md#conditional-logical-operators) określają wstępnie zdefiniowane implementacje operatorów i wszelkie dodatkowe reguły, które mają zastosowanie do każdego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="bd998-310">Opisy korzystają z warunków ***przeciążania przeciążenia operatora jednoargumentowego***, ***rozpoznawania przeciążania operatora binarnego***i ***promocji liczbowych***, definicji znalezionych w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="bd998-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="bd998-311">Rozpoznanie przeciążenia operatora jednoargumentowego</span><span class="sxs-lookup"><span data-stu-id="bd998-311">Unary operator overload resolution</span></span>

<span data-ttu-id="bd998-312">Operacja w formie `op x` lub `x op`, gdzie `op` jest operatorem jednoargumentowym, a `x` jest wyrażeniem typu `X`, jest przetwarzana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="bd998-313">Zestaw operatorów zdefiniowanych przez użytkownika kandydujących zapewnianych przez `X` dla operacji `operator op(x)` jest określany przy użyciu reguł [kandydujących operatorów zdefiniowanych przez użytkownika](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="bd998-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="bd998-314">Jeśli zestaw operatorów zdefiniowanych przez użytkownika nie jest pusty, zostanie to zestaw operatorów kandydujących dla operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="bd998-315">W przeciwnym razie wstępnie zdefiniowane jednoargumentowe implementacje `operator op`, w tym ich podniesione formularze, staną się zestawem operatorów kandydujących dla operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="bd998-316">Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([wyrażenia podstawowe](expressions.md#primary-expressions) i [operatory jednoargumentowe](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="bd998-317">Reguły [rozpoznawania przeciążenia są stosowane](expressions.md#overload-resolution) do zestawu operatorów kandydujących w celu wybrania najlepszego operatora w odniesieniu do listy argumentów `(x)`, a ten operator będzie wynikiem procesu rozpoznawania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="bd998-318">Jeśli nie można wybrać pojedynczego najlepszego operatora rozpoznawania przeciążenia, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="bd998-319">Rozpoznawanie przeciążenia operatora binarnego</span><span class="sxs-lookup"><span data-stu-id="bd998-319">Binary operator overload resolution</span></span>

<span data-ttu-id="bd998-320">Operacja `x op y`, gdzie `op` jest operatorem binarnym z możliwością przeciążenia, `x` jest wyrażeniem typu `X`, a `y` jest wyrażeniem typu `Y`, jest przetwarzana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="bd998-321">Zestaw operatorów zdefiniowanych przez użytkownika kandydujących zapewnianych przez `X` i `Y` dla operacji `operator op(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="bd998-322">Zestaw składa się z Unii operatorów kandydujących dostarczanych przez `X` i operatory kandydujące dostarczone przez `Y`, z których każda została określona przy użyciu reguł [kandydujących operatorów zdefiniowanych przez użytkownika](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="bd998-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="bd998-323">Jeśli `X` i `Y` są tego samego typu lub w przypadku, gdy `X` i `Y` pochodzą od wspólnego typu podstawowego, współdzielone operatory kandydujące występują tylko w połączonym zestawie.</span><span class="sxs-lookup"><span data-stu-id="bd998-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="bd998-324">Jeśli zestaw operatorów zdefiniowanych przez użytkownika nie jest pusty, zostanie to zestaw operatorów kandydujących dla operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="bd998-325">W przeciwnym razie wstępnie zdefiniowane, binarne implementacje `operator op`, w tym ich zniesione formularze, staną się zestawem operatorów kandydujących dla operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="bd998-326">Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([Operatory arytmetyczne](expressions.md#arithmetic-operators) za pomocą [warunkowych operatorów logicznych](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="bd998-327">Dla wstępnie zdefiniowanych typów wyliczeniowych i delegatów jedynymi uznanymi operatorami są te zdefiniowane przez typ wyliczeniowy lub delegata, który jest typem czasu powiązania jednego z operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="bd998-328">Reguły [rozpoznawania przeciążenia są stosowane](expressions.md#overload-resolution) do zestawu operatorów kandydujących w celu wybrania najlepszego operatora w odniesieniu do listy argumentów `(x,y)`, a ten operator będzie wynikiem procesu rozpoznawania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="bd998-329">Jeśli nie można wybrać pojedynczego najlepszego operatora rozpoznawania przeciążenia, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="bd998-330">Kandydujące Operatory zdefiniowane przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="bd998-330">Candidate user-defined operators</span></span>

<span data-ttu-id="bd998-331">W przypadku typu `T` i operacji `operator op(A)`, gdzie `op` jest operatorem przeciążania, a `A` jest listą argumentów, zestaw operatorów zdefiniowanych przez użytkownika w `T` dla `operator op(A)` jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="bd998-332">Określ typ `T0`.</span><span class="sxs-lookup"><span data-stu-id="bd998-332">Determine the type `T0`.</span></span> <span data-ttu-id="bd998-333">Jeśli `T` jest typem dopuszczającym wartość null, `T0` jest jego typem podstawowym, w przeciwnym razie `T0` jest równa `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="bd998-334">Wszystkie deklaracje `operator op` w `T0` i wszystkie zniesione formularze takich operatorów, jeśli ma zastosowanie co najmniej jeden operator ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)) w odniesieniu do listy argumentów `A`, a następnie zestaw operatorów kandydujących składa się ze wszystkich takich odpowiednie operatory w `T0`.</span><span class="sxs-lookup"><span data-stu-id="bd998-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="bd998-335">W przeciwnym razie, jeśli `T0` jest `object`, zestaw operatorów kandydujących jest pusty.</span><span class="sxs-lookup"><span data-stu-id="bd998-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="bd998-336">W przeciwnym razie zestaw operatorów kandydujących udostępniany przez `T0` jest zestawem operatorów kandydujących dostarczanych przez bezpośrednią klasę bazową `T0` lub obowiązującą klasę bazową `T0`, jeśli `T0` jest parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="bd998-337">Promocje numeryczne</span><span class="sxs-lookup"><span data-stu-id="bd998-337">Numeric promotions</span></span>

<span data-ttu-id="bd998-338">Promocja liczbowa składa się z automatycznego wykonywania niektórych niejawnych konwersji argumentów operacji dla wstępnie zdefiniowanych jednoargumentowych i binarnych operatorów liczbowych.</span><span class="sxs-lookup"><span data-stu-id="bd998-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="bd998-339">Promocja numeryczna nie jest odrębnym mechanizmem, ale raczej efektem zastosowania rozpoznawania przeciążenia do wstępnie zdefiniowanych operatorów.</span><span class="sxs-lookup"><span data-stu-id="bd998-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="bd998-340">Promocja liczbowa w specjalny sposób nie wpływa na ocenę operatorów zdefiniowanych przez użytkownika, chociaż Operatory zdefiniowane przez użytkownika mogą być implementowane w taki sposób, aby miały podobne skutki.</span><span class="sxs-lookup"><span data-stu-id="bd998-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="bd998-341">Przykładem promocji numerycznych jest rozważenie wstępnie zdefiniowanych implementacji operatora Binary `*`:</span><span class="sxs-lookup"><span data-stu-id="bd998-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="bd998-342">Gdy reguły rozpoznawania przeciążenia ([rozwiązanie przeciążenia](expressions.md#overload-resolution)) są stosowane do tego zestawu operatorów, efektem jest wybranie pierwszego z operatorów, dla których istnieją niejawne konwersje z typów operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="bd998-343">Na przykład dla operacji `b * s`, gdzie `b` jest `byte` i `s` jest `short`, rozpoznawanie przeciążenia wybiera `operator *(int,int)` jako najlepszy operator.</span><span class="sxs-lookup"><span data-stu-id="bd998-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="bd998-344">W związku z tym efektem jest to, że `b` i `s` są konwertowane na `int`, a typ wyniku jest `int`.</span><span class="sxs-lookup"><span data-stu-id="bd998-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="bd998-345">Podobnie dla operacji `i * d`, gdzie `i` jest `int` i `d` jest `double`, rozpoznawanie przeciążenia wybiera `operator *(double,double)` jako najlepszy operator.</span><span class="sxs-lookup"><span data-stu-id="bd998-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="bd998-346">Jednoargumentowe promocje liczbowe</span><span class="sxs-lookup"><span data-stu-id="bd998-346">Unary numeric promotions</span></span>

<span data-ttu-id="bd998-347">Jednoargumentowa promocja numeryczna występuje dla operandów wstępnie zdefiniowanych `+`, `-` i `~` jednoargumentowych operatorów.</span><span class="sxs-lookup"><span data-stu-id="bd998-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="bd998-348">Jednoargumentowa promocja liczbowa polega na przekonwertowaniu operandów typu `sbyte`, `byte`, `short`, `ushort` lub `char` do typu `int`.</span><span class="sxs-lookup"><span data-stu-id="bd998-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="bd998-349">Dodatkowo dla operatora jednoargumentowego `-` Jednoargumentowa promocja numeryczna konwertuje operandy typu `uint` na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="bd998-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="bd998-350">Binarne promocje liczbowe</span><span class="sxs-lookup"><span data-stu-id="bd998-350">Binary numeric promotions</span></span>

<span data-ttu-id="bd998-351">Binarne promocję liczbową występuje dla operandów wstępnie zdefiniowanych `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, 0, 1, 2 i 3 operatorów binarnych.</span><span class="sxs-lookup"><span data-stu-id="bd998-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="bd998-352">Binarne promocję liczbową niejawnie konwertuje oba operandy na wspólny typ, który, w przypadku operatorów nierelacyjnych, również staje się typem wyniku operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="bd998-353">Binarne promocje liczbowe polega na zastosowaniu następujących reguł w kolejności, w jakiej są wyświetlane w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="bd998-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="bd998-354">Jeśli oba operandy są typu `decimal`, drugi operand jest konwertowany na typ `decimal` lub wystąpi błąd w czasie powiązania, jeśli inny operand jest typu `float` lub `double`.</span><span class="sxs-lookup"><span data-stu-id="bd998-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="bd998-355">W przeciwnym razie, jeśli jeden z operandów jest typu `double`, drugi operand jest konwertowany na typ `double`.</span><span class="sxs-lookup"><span data-stu-id="bd998-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="bd998-356">W przeciwnym razie, jeśli jeden z operandów jest typu `float`, drugi operand jest konwertowany na typ `float`.</span><span class="sxs-lookup"><span data-stu-id="bd998-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="bd998-357">W przeciwnym razie, jeśli jeden z operandów jest typu `ulong`, drugi operand jest konwertowany na typ `ulong` lub wystąpi błąd w czasie powiązania, jeśli inny operand jest typu `sbyte`, `short`, `int` lub `long`.</span><span class="sxs-lookup"><span data-stu-id="bd998-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="bd998-358">W przeciwnym razie, jeśli jeden z operandów jest typu `long`, drugi operand jest konwertowany na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="bd998-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="bd998-359">W przeciwnym razie, jeśli jeden z operandów jest typu `uint`, a drugi operand jest typu `sbyte`, `short` lub `int`, oba operandy są konwertowane na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="bd998-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="bd998-360">W przeciwnym razie, jeśli jeden z operandów jest typu `uint`, drugi operand jest konwertowany na typ `uint`.</span><span class="sxs-lookup"><span data-stu-id="bd998-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="bd998-361">W przeciwnym razie oba operandy są konwertowane na typ `int`.</span><span class="sxs-lookup"><span data-stu-id="bd998-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="bd998-362">Należy zauważyć, że pierwsza reguła nie zezwala na wszystkie operacje, które mieszają typ `decimal` z typami `double` i `float`.</span><span class="sxs-lookup"><span data-stu-id="bd998-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="bd998-363">Reguła wynika z faktu, że nie ma żadnych niejawnych konwersji między typem `decimal` a typami `double` i `float`.</span><span class="sxs-lookup"><span data-stu-id="bd998-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="bd998-364">Należy również zauważyć, że nie jest możliwe, aby operand był typu `ulong`, gdy inny operand ma podpisany typ całkowity.</span><span class="sxs-lookup"><span data-stu-id="bd998-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="bd998-365">Przyczyną jest to, że nie istnieje żaden typ całkowity, który może reprezentować pełny zakres `ulong`, a także z podpisanymi typami całkowitymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="bd998-366">W obu powyższych przypadkach wyrażenie cast może służyć do jawnej konwersji jednego operandu na typ, który jest zgodny z drugim operandem.</span><span class="sxs-lookup"><span data-stu-id="bd998-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="bd998-367">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="bd998-368">Wystąpił błąd w czasie powiązania, ponieważ nie można pomnożyć wartości `decimal` przez `double`.</span><span class="sxs-lookup"><span data-stu-id="bd998-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="bd998-369">Ten błąd jest rozpoznawany przez jawne przekonwertowanie drugiego operandu na `decimal` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="bd998-370">Podniesione operatory</span><span class="sxs-lookup"><span data-stu-id="bd998-370">Lifted operators</span></span>

<span data-ttu-id="bd998-371">***Podnoszone operatory*** zezwalają wstępnie zdefiniowanym i zdefiniowanym przez użytkownika operatorom, którzy działają na typach wartości, które nie dopuszczają wartości null, również są używane z dopuszczaniem wartości null dla tych typów.</span><span class="sxs-lookup"><span data-stu-id="bd998-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="bd998-372">Podnoszone operatory są zbudowane ze wstępnie zdefiniowanych i zdefiniowanych przez użytkownika operatorów spełniających określone wymagania, zgodnie z opisem w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="bd998-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="bd998-373">Dla operatorów jednoargumentowych</span><span class="sxs-lookup"><span data-stu-id="bd998-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="bd998-374">podnieśna postać operatora istnieje, jeśli argument operacji i typ wyniku nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="bd998-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="bd998-375">Przekształcony formularz jest konstruowany przez dodanie pojedynczego `?` modyfikatora do operandu i typów wyników.</span><span class="sxs-lookup"><span data-stu-id="bd998-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="bd998-376">Podnieśny operator generuje wartość null, jeśli operand ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="bd998-377">W przeciwnym razie, podnieśny operator odwinie operand, stosuje operator podstawowy i otacza wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="bd998-378">Dla operatorów binarnych</span><span class="sxs-lookup"><span data-stu-id="bd998-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="bd998-379">podnieśna postać operatora istnieje, jeśli operand i typy wyników to wszystkie wartości niedopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="bd998-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="bd998-380">Przekształcony formularz jest konstruowany przez dodanie pojedynczego `?` modyfikatora do każdego operandu i typu wyniku.</span><span class="sxs-lookup"><span data-stu-id="bd998-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="bd998-381">Podnieśny operator tworzy wartość null, jeśli jeden lub oba operandy mają wartość null (wyjątkiem są operatory `&` i `|` typu `bool?`, zgodnie z opisem w [operatorach logicznych logicznej](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="bd998-382">W przeciwnym razie podnieśny operator odpakuje operandy, zastosuje operator podstawowy i otacza wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="bd998-383">Dla operatorów równości</span><span class="sxs-lookup"><span data-stu-id="bd998-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="bd998-384">podnieśna postać operatora istnieje, jeśli typy operandów to zarówno typ wartości niedopuszczający wartości null, jak i typ wyniku `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="bd998-385">Przekształcony formularz jest konstruowany przez dodanie pojedynczego `?` modyfikatora do każdego typu operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="bd998-386">Podnieśny operator traktuje dwie wartości null, a wartość null nie jest równa żadnej wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="bd998-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="bd998-387">Jeśli oba operandy są inne niż null, przenoszone operator odwinie operandy i stosuje operatora bazowego, aby wygenerować wynik `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="bd998-388">Dla operatorów relacyjnych</span><span class="sxs-lookup"><span data-stu-id="bd998-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="bd998-389">podnieśna postać operatora istnieje, jeśli typy operandów to zarówno typ wartości niedopuszczający wartości null, jak i typ wyniku `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="bd998-390">Przekształcony formularz jest konstruowany przez dodanie pojedynczego `?` modyfikatora do każdego typu operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="bd998-391">Podnieśny operator tworzy wartość `false`, jeśli jeden lub oba operandy mają wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="bd998-392">W przeciwnym razie podnieśny operator odwinie operandy i zastosuje operatora bazowego, aby wygenerować wynik `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="bd998-393">Wyszukiwanie elementów członkowskich</span><span class="sxs-lookup"><span data-stu-id="bd998-393">Member lookup</span></span>

<span data-ttu-id="bd998-394">Przeszukiwanie elementu członkowskiego jest procesem, w którym określa się znaczenie nazwy w kontekście typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="bd998-395">Wyszukiwanie elementu członkowskiego może odbywać się w ramach oceny elementu *simple_name* ([prostych nazw](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="bd998-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="bd998-396">Jeśli *simple_name* lub *member_access* występuje jako *primary_expression* elementu *invocation_expression* ([wywołania metody](expressions.md#method-invocations)), element członkowski jest określany jako wywoływany.</span><span class="sxs-lookup"><span data-stu-id="bd998-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="bd998-397">Jeśli element członkowski jest metodą lub zdarzeniem, lub jeśli jest to stała, pole lub właściwość typu delegata ([delegatów](delegates.md)) lub typ `dynamic` ([Typ dynamiczny](types.md#the-dynamic-type)), członek jest określany jako *wywoływać*.</span><span class="sxs-lookup"><span data-stu-id="bd998-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="bd998-398">Funkcja wyszukiwania elementów członkowskich traktuje nie tylko nazwę elementu członkowskiego, ale również liczbę parametrów typu, które ma element członkowski i czy element członkowski jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="bd998-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="bd998-399">Na potrzeby wyszukiwania elementów członkowskich, metod ogólnych i zagnieżdżonych typów ogólnych ma liczbę parametrów typu wskazanych w odpowiednich deklaracjach, a wszystkie inne elementy członkowskie mają parametry typu zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="bd998-400">Przeszukiwanie elementu członkowskiego o nazwie @ no__t-0 z `K` @ no__t-2type parametrów w typie @ no__t-3 jest przetwarzane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="bd998-401">Najpierw jest określany zestaw dostępnych składowych o nazwie @ no__t-0:</span><span class="sxs-lookup"><span data-stu-id="bd998-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="bd998-402">Jeśli `T` jest parametrem typu, zestaw jest Unią zestawów dostępnych składowych o nazwie @ no__t-1 w każdym typie określonym jako ograniczenie podstawowe lub ograniczenie pomocnicze ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) dla parametru @ no__t-3, wraz z zestawem dostępni członkowie o nazwie @ no__t-4 w `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="bd998-403">W przeciwnym razie zestaw składa się z wszystkich dostępnych członków ([dostęp członków](basic-concepts.md#member-access)) o nazwie @ no__t-1 w @ no__t-2, w tym dziedziczone elementy członkowskie i dostępni członkowie o nazwie @ no__t-3 w `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="bd998-404">Jeśli `T` jest konstruowanym typem, zestaw elementów członkowskich zostanie uzyskany przez podstawianie argumentów typu, zgodnie z opisem w [składowych konstruowanych typów](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="bd998-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="bd998-405">Elementy członkowskie, które zawierają modyfikator `override`, są wykluczone z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="bd998-406">Następnie Jeśli `K` wynosi zero, wszystkie typy zagnieżdżone, których deklaracje obejmują parametry typu, są usuwane.</span><span class="sxs-lookup"><span data-stu-id="bd998-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="bd998-407">Jeśli `K` nie jest zerem, wszystkie elementy członkowskie z inną liczbą parametrów typu są usuwane.</span><span class="sxs-lookup"><span data-stu-id="bd998-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="bd998-408">Należy pamiętać, że jeśli `K` ma wartość zero, metody z parametrami typu nie są usuwane, ponieważ proces wnioskowania typów ([wnioskowanie](expressions.md#type-inference)o typie) może być w stanie wywnioskować argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="bd998-409">Następnie, jeśli element członkowski jest *wywoływany*, wszystkie elementy członkowskie inne niż*wywoływać* są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="bd998-410">Następnie elementy członkowskie, które są ukryte przez inne elementy członkowskie, są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="bd998-411">Dla każdego elementu członkowskiego `S.M` w zestawie, gdzie `S` jest typem, w którym jest zadeklarowany element członkowski @ no__t-2, są stosowane następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="bd998-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="bd998-412">Jeśli `M` to element członkowski stałej, pola, właściwości, zdarzenia lub wyliczenia, wszystkie elementy członkowskie zadeklarowane w typie podstawowym `S` zostaną usunięte z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="bd998-413">Jeśli `M` jest deklaracją typu, wszystkie typy, które nie są zadeklarowane w typie podstawowym `S` są usuwane z zestawu, a wszystkie deklaracje typów o tej samej liczbie parametrów typu jako `M` zadeklarowane w typie podstawowym `S` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="bd998-414">Jeśli `M` to metoda, wszystkie elementy członkowskie inne niż metody zadeklarowane w typie podstawowym `S` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="bd998-415">Następnie elementy członkowskie interfejsu, które są ukryte przez elementy członkowskie klasy, są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="bd998-416">Ten krok ma wpływ tylko wtedy, gdy `T` jest parametrem typu, a `T` ma zarówno obowiązującą klasę bazową, inną niż `object`, jak i niepusty obowiązujący zestaw interfejsów ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="bd998-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="bd998-417">Dla każdego elementu członkowskiego `S.M` w zestawie, gdzie `S` jest typem, w którym jest zadeklarowany element członkowski `M`, są stosowane następujące reguły, jeśli `S` jest deklaracją klasy inną niż `object`:</span><span class="sxs-lookup"><span data-stu-id="bd998-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="bd998-418">Jeśli `M` to wartość stałej, pola, właściwości, zdarzenia, elementu członkowskiego wyliczenia lub deklaracji typu, wszystkie elementy członkowskie zadeklarowane w deklaracji interfejsu są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="bd998-419">Jeśli `M` to metoda, wszystkie elementy członkowskie inne niż metody zadeklarowane w deklaracji interfejsu są usuwane z zestawu, a wszystkie metody o tej samej sygnaturze co `M` zadeklarowane w deklaracji interfejsu są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="bd998-420">Na koniec po usunięciu ukrytych elementów członkowskich zostanie określony wynik wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="bd998-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="bd998-421">Jeśli zestaw składa się z pojedynczego elementu członkowskiego, który nie jest metodą, wówczas ten element członkowski jest wynikiem wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="bd998-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="bd998-422">W przeciwnym razie, jeśli zestaw zawiera tylko metody, ta grupa metod jest wynikiem wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="bd998-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="bd998-423">W przeciwnym razie odnośnik jest niejednoznaczny i wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-424">W przypadku przeszukiwania elementów członkowskich w typach innych niż parametry typu i interfejsy oraz wyszukiwania elementów członkowskich w interfejsach, które są ściśle dziedziczenia (każdy interfejs w łańcuchu dziedziczenia ma dokładnie zero lub jeden bezpośredni interfejs podstawowy), efekt reguł wyszukiwania to Wystarczy, że pochodni członkowie ukrywają elementy bazowe o tej samej nazwie lub podpisie.</span><span class="sxs-lookup"><span data-stu-id="bd998-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="bd998-425">Takie wyszukiwania o pojedynczej dziedziczeniu nigdy nie są niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="bd998-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="bd998-426">Niejasności, które mogą wystąpić w odnośnikach elementów członkowskich w interfejsach z wieloma dziedziczeniem, są opisane w [dostęp do elementu członkowskiego interfejsu](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="bd998-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="bd998-427">Typy podstawowe</span><span class="sxs-lookup"><span data-stu-id="bd998-427">Base types</span></span>

<span data-ttu-id="bd998-428">W celach wyszukiwania elementów członkowskich typ `T` jest traktowany jako mający następujące typy podstawowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="bd998-429">Jeśli `T` jest `object`, wówczas `T` nie ma typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="bd998-430">Jeśli `T` to *enum_type*, typy podstawowe `T` są typami klas `System.Enum`, `System.ValueType` i `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="bd998-431">Jeśli `T` to *struct_type*, typy podstawowe `T` są typami klas `System.ValueType` i `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="bd998-432">Jeśli `T` to *class_type*, podstawowe typy `T` są klasami podstawowymi `T`, łącznie z typem klasy `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="bd998-433">Jeśli `T` to *INTERFACE_TYPE*, typy podstawowe `T` są podstawowymi interfejsami `T` i typu klasy `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="bd998-434">Jeśli `T` to *array_type*, typy podstawowe `T` są typami klas `System.Array` i `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="bd998-435">Jeśli `T` to *delegate_type*, typy podstawowe `T` są typami klas `System.Delegate` i `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="bd998-436">Elementy członkowskie funkcji</span><span class="sxs-lookup"><span data-stu-id="bd998-436">Function members</span></span>

<span data-ttu-id="bd998-437">Składowe funkcji są elementami członkowskimi, które zawierają instrukcje wykonywalne.</span><span class="sxs-lookup"><span data-stu-id="bd998-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="bd998-438">Składowe funkcji są zawsze członkami typów i nie mogą być elementami członkowskimi przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="bd998-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="bd998-439">C#definiuje następujące kategorie elementów członkowskich funkcji:</span><span class="sxs-lookup"><span data-stu-id="bd998-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="bd998-440">Metody</span><span class="sxs-lookup"><span data-stu-id="bd998-440">Methods</span></span>
*  <span data-ttu-id="bd998-441">properties</span><span class="sxs-lookup"><span data-stu-id="bd998-441">Properties</span></span>
*  <span data-ttu-id="bd998-442">Events</span><span class="sxs-lookup"><span data-stu-id="bd998-442">Events</span></span>
*  <span data-ttu-id="bd998-443">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="bd998-443">Indexers</span></span>
*  <span data-ttu-id="bd998-444">Operatory zdefiniowane przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="bd998-444">User-defined operators</span></span>
*  <span data-ttu-id="bd998-445">Konstruktory wystąpień</span><span class="sxs-lookup"><span data-stu-id="bd998-445">Instance constructors</span></span>
*  <span data-ttu-id="bd998-446">Konstruktory statyczne</span><span class="sxs-lookup"><span data-stu-id="bd998-446">Static constructors</span></span>
*  <span data-ttu-id="bd998-447">Destruktory</span><span class="sxs-lookup"><span data-stu-id="bd998-447">Destructors</span></span>

<span data-ttu-id="bd998-448">Oprócz destruktorów i konstruktorów statycznych (które nie mogą być wywoływane jawnie), instrukcje zawarte w składowych funkcji są wykonywane za pomocą wywołań elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="bd998-449">Rzeczywista składnia do pisania wywołania składowej funkcji zależy od określonej kategorii elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="bd998-450">Lista argumentów ([listy argumentów](expressions.md#argument-lists)) wywołania elementu członkowskiego funkcji udostępnia wartości rzeczywiste lub odwołania do zmiennych dla parametrów elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="bd998-451">Wywołania metod ogólnych mogą wykorzystywać wnioskowanie o typie w celu określenia zestawu argumentów typu, które mają zostać przekazane do metody.</span><span class="sxs-lookup"><span data-stu-id="bd998-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="bd998-452">Ten proces jest opisany w [wnioskach o typie](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="bd998-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="bd998-453">Wywołania metod, indeksatorów, operatorów i wystąpień stosują rozwiązanie przeciążenia, aby określić, który zestaw elementów członkowskich funkcji ma być wywoływany.</span><span class="sxs-lookup"><span data-stu-id="bd998-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="bd998-454">Ten proces jest opisany w temacie [rozpoznawanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="bd998-455">Gdy określony element członkowski funkcji zostanie zidentyfikowany w czasie wiązania, prawdopodobnie przez rozpoznanie przeciążenia, rzeczywisty proces wykonywania wywoływany przez element członkowski funkcji jest opisany w [kontroli w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="bd998-456">Poniższa tabela zawiera podsumowanie przetwarzania, które odbywa się w konstrukcjach obejmujących sześć kategorii elementów członkowskich funkcji, które można jawnie wywołać.</span><span class="sxs-lookup"><span data-stu-id="bd998-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="bd998-457">W tabeli, `e`, `x`, `y` i `value` wskazują wyrażenia sklasyfikowane jako zmienne lub wartości, `T` wskazuje wyrażenie sklasyfikowane jako typ, `F` jest prostą nazwą metody, a `P` jest prostą nazwą właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd998-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="bd998-458">__Construct__</span><span class="sxs-lookup"><span data-stu-id="bd998-458">__Construct__</span></span>     | <span data-ttu-id="bd998-459">__Przykład__</span><span class="sxs-lookup"><span data-stu-id="bd998-459">__Example__</span></span>    | <span data-ttu-id="bd998-460">__Opis__</span><span class="sxs-lookup"><span data-stu-id="bd998-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="bd998-461">Wywołanie metody</span><span class="sxs-lookup"><span data-stu-id="bd998-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="bd998-462">Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszej metody `F` w zawierającej ją klasie lub strukturze.</span><span class="sxs-lookup"><span data-stu-id="bd998-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="bd998-463">Metoda jest wywoływana z listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="bd998-464">Jeśli metoda nie jest `static`, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="bd998-465">Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszej metody `F` w klasie lub strukturze `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="bd998-466">Błąd czasu powiązania występuje, jeśli metoda nie jest `static`.</span><span class="sxs-lookup"><span data-stu-id="bd998-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="bd998-467">Metoda jest wywoływana z listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="bd998-468">Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszej metody F w klasie, strukturze lub interfejsie podanej przez typ `e`.</span><span class="sxs-lookup"><span data-stu-id="bd998-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="bd998-469">Błąd czasu powiązania występuje, gdy metoda jest `static`.</span><span class="sxs-lookup"><span data-stu-id="bd998-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="bd998-470">Metoda jest wywoływana z wyrażeniem wystąpienia `e` i listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="bd998-471">Dostęp do właściwości</span><span class="sxs-lookup"><span data-stu-id="bd998-471">Property access</span></span>   | `P`            | <span data-ttu-id="bd998-472">Metoda dostępu `get` właściwości `P` w zawierającej ją klasie lub strukturze jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="bd998-473">Błąd czasu kompilacji występuje, jeśli `P` jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="bd998-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="bd998-474">Jeśli `P` nie jest `static`, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="bd998-475">Metoda dostępu `set` właściwości `P` w zawartej klasie lub strukturze jest wywoływana z listą argumentów `(value)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="bd998-476">Błąd czasu kompilacji występuje, jeśli `P` jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="bd998-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="bd998-477">Jeśli `P` nie jest `static`, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="bd998-478">Metoda dostępu `get` właściwości `P` w klasie lub strukturze `T` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="bd998-479">Błąd czasu kompilacji występuje, jeśli `P` nie jest `static` lub jeśli `P` jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="bd998-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="bd998-480">Metoda dostępu `set` właściwości `P` w klasie lub strukturze `T` jest wywoływana z listą argumentów `(value)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="bd998-481">Błąd czasu kompilacji występuje, jeśli `P` nie jest `static` lub jeśli `P` jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="bd998-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="bd998-482">Metoda dostępu `get` właściwości `P` w klasie, strukturze lub interfejsie określona przez typ `e` jest wywoływana przy użyciu wyrażenia wystąpienia `e`.</span><span class="sxs-lookup"><span data-stu-id="bd998-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="bd998-483">Błąd czasu powiązania występuje, jeśli `P` jest `static` lub jeśli `P` jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="bd998-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="bd998-484">Metoda dostępu `set` właściwości `P` w klasie, strukturze lub interfejsie określona przez typ `e` jest wywoływana z wyrażeniem wystąpienia `e` i listą argumentów `(value)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="bd998-485">Błąd czasu powiązania występuje, jeśli `P` jest `static` lub jeśli `P` jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="bd998-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="bd998-486">Dostęp do zdarzeń</span><span class="sxs-lookup"><span data-stu-id="bd998-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="bd998-487">Metoda dostępu `add` zdarzenia `E` z klasy zawierającej lub struktury jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="bd998-488">Jeśli `E` nie jest statyczna, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="bd998-489">Metoda dostępu `remove` zdarzenia `E` z klasy zawierającej lub struktury jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="bd998-490">Jeśli `E` nie jest statyczna, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="bd998-491">Metoda dostępu `add` zdarzenia `E` w klasie lub strukturze `T` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="bd998-492">Błąd czasu powiązania występuje, jeśli `E` nie jest statyczna.</span><span class="sxs-lookup"><span data-stu-id="bd998-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="bd998-493">Metoda dostępu `remove` zdarzenia `E` w klasie lub strukturze `T` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="bd998-494">Błąd czasu powiązania występuje, jeśli `E` nie jest statyczna.</span><span class="sxs-lookup"><span data-stu-id="bd998-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="bd998-495">Metoda dostępu `add` zdarzenia `E` w klasie, strukturze lub interfejsie przyznanym przez typ `e` jest wywoływana przy użyciu wyrażenia wystąpienia `e`.</span><span class="sxs-lookup"><span data-stu-id="bd998-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="bd998-496">Jeśli `E` jest statyczna, występuje błąd czasu powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="bd998-497">Metoda dostępu `remove` zdarzenia `E` w klasie, strukturze lub interfejsie przyznanym przez typ `e` jest wywoływana przy użyciu wyrażenia wystąpienia `e`.</span><span class="sxs-lookup"><span data-stu-id="bd998-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="bd998-498">Jeśli `E` jest statyczna, występuje błąd czasu powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="bd998-499">Dostęp indeksatora</span><span class="sxs-lookup"><span data-stu-id="bd998-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="bd998-500">Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego indeksatora w klasie, strukturze lub interfejsie przyznanym przez typ e.</span><span class="sxs-lookup"><span data-stu-id="bd998-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="bd998-501">Metoda dostępu `get` indeksatora jest wywoływana z wyrażeniem wystąpienia `e` i listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="bd998-502">Błąd czasu powiązania występuje, gdy indeksator jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="bd998-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="bd998-503">Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego indeksatora w klasie, strukturze lub interfejsie podanym przez typ `e`.</span><span class="sxs-lookup"><span data-stu-id="bd998-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="bd998-504">Metoda dostępu `set` indeksatora jest wywoływana z wyrażeniem wystąpienia `e` i listą argumentów `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="bd998-505">Błąd czasu powiązania występuje, gdy indeksator jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="bd998-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="bd998-506">Wywołanie operatora</span><span class="sxs-lookup"><span data-stu-id="bd998-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="bd998-507">Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego operatora jednoargumentowego w klasie lub strukturze podanej przez typ `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="bd998-508">Wybrany operator jest wywoływany z listą argumentów `(x)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="bd998-509">Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego operatora binarnego w klasach lub strukturach określonych przez typy `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="bd998-510">Wybrany operator jest wywoływany z listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="bd998-511">Wywołanie konstruktora wystąpienia</span><span class="sxs-lookup"><span data-stu-id="bd998-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="bd998-512">Rozpoznanie przeciążenia jest stosowane do wybierania najlepszego konstruktora wystąpienia w klasie lub strukturze `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="bd998-513">Konstruktor wystąpienia jest wywoływany z listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="bd998-514">Listy argumentów</span><span class="sxs-lookup"><span data-stu-id="bd998-514">Argument lists</span></span>

<span data-ttu-id="bd998-515">Każdy element członkowski funkcji i obiekt delegowany zawiera listę argumentów, która dostarcza wartości rzeczywiste lub odwołania do zmiennych parametrów elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="bd998-516">Składnia określająca listę argumentów wywołania elementu członkowskiego funkcji zależy od kategorii elementu członkowskiego funkcji:</span><span class="sxs-lookup"><span data-stu-id="bd998-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="bd998-517">W przypadku konstruktorów wystąpień, metod, indeksatorów i delegatów argumenty są określone jako *argument_list*, zgodnie z poniższym opisem.</span><span class="sxs-lookup"><span data-stu-id="bd998-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="bd998-518">W przypadku indeksatorów podczas wywoływania metody dostępu `set` lista argumentów dodatkowo zawiera wyrażenie określone jako prawy operand operatora przypisania.</span><span class="sxs-lookup"><span data-stu-id="bd998-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="bd998-519">Dla właściwości, lista argumentów jest pusta podczas wywoływania metody dostępu `get` i składa się z wyrażenia określonego jako prawy operand operatora przypisania podczas wywoływania metody dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="bd998-520">Dla zdarzeń lista argumentów składa się z wyrażenia określonego jako prawy operand operatora `+=` lub `-=`.</span><span class="sxs-lookup"><span data-stu-id="bd998-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="bd998-521">Dla operatorów zdefiniowanych przez użytkownika lista argumentów składa się z pojedynczego operandu jednoargumentowego operatora lub dwóch operandów operatora binarnego.</span><span class="sxs-lookup"><span data-stu-id="bd998-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="bd998-522">Argumenty właściwości ([Właściwości](classes.md#properties)), zdarzenia ([zdarzenia](classes.md#events)) i Operatory zdefiniowane przez użytkownika ([Operatory](classes.md#operators)) są zawsze przekazane jako parametry wartości ([Parametry wartości](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="bd998-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="bd998-523">Argumenty indeksatorów ([indeksatorów](classes.md#indexers)) są zawsze przekazane jako parametry wartości ([Parametry wartości](classes.md#value-parameters)) lub tablice parametrów ([tablice parametrów](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="bd998-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="bd998-524">Parametry referencyjne i wyjściowe nie są obsługiwane dla tych kategorii elementów członkowskich funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="bd998-525">Argumenty konstruktora wystąpienia, metody, indeksatora lub wywołania delegata są określone jako *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="bd998-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="bd998-526">*Argument_list* składa się z jednego lub więcej *argumentów*s, rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="bd998-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="bd998-527">Każdy argument składa się z opcjonalnego *argument_name* , po którym następuje *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="bd998-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="bd998-528">*Argument* z *argument_name* jest określany jako ***argument nazwany***, podczas gdy *Argument* bez *argument_name* jest ***argumentem pozycyjnym***.</span><span class="sxs-lookup"><span data-stu-id="bd998-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="bd998-529">Występuje błąd dla argumentu pozycyjnego, który ma być wyświetlany po nazwanym argumencie w *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="bd998-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="bd998-530">*Argument_value* może przyjmować jedną z następujących form:</span><span class="sxs-lookup"><span data-stu-id="bd998-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="bd998-531">*Wyrażenie*wskazujące, że argument jest przenoszona jako parametr wartości ([Parametry wartości](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="bd998-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="bd998-532">Słowo kluczowe `ref` następuje przez *variable_reference* ([odwołania do zmiennych](variables.md#variable-references)), wskazujące, że argument jest przenoszona jako parametr odwołania ([parametry odwołania](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="bd998-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="bd998-533">Zmienna musi być ostatecznie przypisana ([przypisanie](variables.md#definite-assignment)), zanim będzie mogła zostać przeniesiona jako parametr odwołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="bd998-534">Słowo kluczowe `out` następuje przez *variable_reference* ([odwołania do zmiennych](variables.md#variable-references)), wskazujące, że argument jest przekazywane jako parametr wyjściowy ([Parametry wyjściowe](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="bd998-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="bd998-535">Zmienna jest uznawana za ostatecznie przypisaną ([przypisanie](variables.md#definite-assignment)) po wywołaniu elementu członkowskiego, w którym zmienna jest przenoszona jako parametr wyjściowy.</span><span class="sxs-lookup"><span data-stu-id="bd998-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="bd998-536">Odpowiednie parametry</span><span class="sxs-lookup"><span data-stu-id="bd998-536">Corresponding parameters</span></span>

<span data-ttu-id="bd998-537">Dla każdego argumentu na liście argumentów musi istnieć odpowiedni parametr w elemencie członkowskim funkcji lub w wywołaniu delegowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="bd998-538">Lista parametrów użyta w poniższym przykładzie jest określona w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="bd998-539">W przypadku metod wirtualnych i indeksatorów zdefiniowanych w klasach lista parametrów jest wybierana z najbardziej szczegółowej deklaracji lub przesłonięcia składowej funkcji, rozpoczynając od typu statycznego odbiornika i przeszukiwania przez klasy bazowe.</span><span class="sxs-lookup"><span data-stu-id="bd998-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="bd998-540">W przypadku metod interfejsu i indeksatorów lista parametrów jest wybierana jako najbardziej Szczegółowa definicja elementu członkowskiego, rozpoczynając od typu interfejsu i przeszukiwaniem przez interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="bd998-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="bd998-541">Jeśli nie zostanie znaleziona lista parametrów unikatowych, jest to lista parametrów z niedostępnymi nazwami i żadne parametry opcjonalne nie są konstruowane, dlatego wywołania nie mogą używać parametrów nazwanych ani pomijać opcjonalnych argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="bd998-542">Dla metod częściowych jest używana lista parametrów definiująca Deklaracja metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="bd998-543">Dla wszystkich innych elementów członkowskich funkcji i delegatów istnieje tylko jedna lista parametrów, która jest używana.</span><span class="sxs-lookup"><span data-stu-id="bd998-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="bd998-544">Pozycja argumentu lub parametru jest definiowana jako liczba argumentów lub parametrów poprzedzających je na liście argumentów lub liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="bd998-545">Odpowiednie parametry argumentów elementu członkowskiego funkcji są określane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="bd998-546">Argumenty w *argument_list* konstruktorów wystąpień, metodach, indeksatorach i delegatów:</span><span class="sxs-lookup"><span data-stu-id="bd998-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="bd998-547">Argument pozycyjny, gdzie stały parametr występuje w tym samym położeniu na liście parametrów odpowiada temu parametrowi.</span><span class="sxs-lookup"><span data-stu-id="bd998-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="bd998-548">Argument pozycyjny składowej funkcji z tablicą parametrów wywoływaną w jej normalnej postaci odpowiada tablicy parametrów, która musi występować w tym samym położeniu na liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="bd998-549">Argument pozycyjny elementu członkowskiego funkcji z tablicą parametrów wywołaną w jego rozwiniętej postaci, gdzie żaden stały parametr nie występuje w tym samym położeniu na liście parametrów, odnosi się do elementu w tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="bd998-550">Nazwany argument odpowiada parametrowi o tej samej nazwie na liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="bd998-551">W przypadku indeksatorów podczas wywoływania metody dostępu `set` wyrażenie określone jako prawy operand operatora przypisania odpowiada niejawnemu parametrowi `value` deklaracji dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="bd998-552">Dla właściwości podczas wywoływania metody dostępu `get` nie ma argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="bd998-553">Podczas wywoływania metody dostępu `set` wyrażenie określone jako prawy operand operatora przypisania odpowiada niejawnemu parametrowi `value` deklaracji dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="bd998-554">Dla operatorów jednoargumentowych zdefiniowanych przez użytkownika (w tym konwersji) pojedynczy operand odpowiada jednemu parametrowi deklaracji operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="bd998-555">W przypadku operatorów binarnych zdefiniowanych przez użytkownika lewy argument operacji odpowiada pierwszemu parametrowi, a prawy operand odpowiada drugiemu parametrowi deklaracji operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="bd998-556">Ocena czasu wykonywania dla list argumentów</span><span class="sxs-lookup"><span data-stu-id="bd998-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="bd998-557">W czasie wykonywania wywołania elementu członkowskiego funkcji ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) wyrażenia lub odwołania do zmiennych listy argumentów są oceniane w kolejności od lewej do prawej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="bd998-558">W przypadku parametru wartości wyrażenie argumentu jest oceniane i niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) do odpowiedniego typu parametru.</span><span class="sxs-lookup"><span data-stu-id="bd998-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="bd998-559">Wartość będąca wynikiem jest wartością początkową parametru value w wywołaniu elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="bd998-560">Dla parametru reference lub Output jest oceniane odwołanie do zmiennej, a wynikowa lokalizacja magazynu stanowi lokalizację magazynu reprezentowanego przez parametr w wywołaniu elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="bd998-561">Jeśli odwołanie do zmiennej określone jako parametr Reference lub Output jest elementem tablicy *reference_type*, wykonywane jest sprawdzanie w czasie wykonywania, aby upewnić się, że typ elementu tablicy jest identyczny z typem parametru.</span><span class="sxs-lookup"><span data-stu-id="bd998-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="bd998-562">Jeśli to sprawdzenie nie powiedzie się, zostanie zgłoszony `System.ArrayTypeMismatchException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="bd998-563">Metody, indeksatory i konstruktory wystąpień mogą deklarować jako tablicę parametrów ([tablice parametrów](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="bd998-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="bd998-564">Takie składowe funkcji są wywoływane w ich normalnej formie lub w rozwiniętej formie, w zależności od tego, który ma zastosowanie ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="bd998-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="bd998-565">Gdy element członkowski funkcji z tablicą parametrów jest wywoływany w jego normalnej postaci, argument określony dla tablicy parametrów musi być pojedynczym wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="bd998-566">W takim przypadku tablica parametrów działa dokładnie tak, jak parametr value.</span><span class="sxs-lookup"><span data-stu-id="bd998-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="bd998-567">Gdy element członkowski funkcji z tablicą parametrów jest wywoływany w rozwiniętej postaci, wywołanie musi określać zero lub więcej argumentów pozycyjnych dla tablicy parametrów, gdzie każdy argument jest wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne ](conversions.md#implicit-conversions)) do typu elementu tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="bd998-568">W takim przypadku wywołanie tworzy wystąpienie typu tablicy parametrów o długości odpowiadającej liczbie argumentów, inicjuje elementy instancji Array z podaną wartością argumentów i używa nowo utworzonego wystąpienia tablicy jako wartości rzeczywistej argument.</span><span class="sxs-lookup"><span data-stu-id="bd998-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="bd998-569">Wyrażenia listy argumentów są zawsze oceniane w kolejności, w jakiej zostały wpisane.</span><span class="sxs-lookup"><span data-stu-id="bd998-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="bd998-570">W tym przykładzie przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="bd998-571">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="bd998-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="bd998-572">Reguły wariancji tablic ([Kowariancja tablic](arrays.md#array-covariance)) zezwalają na wartość typu tablicy `A[]`, aby było odwołaniem do wystąpienia typu tablicy `B[]`, pod warunkiem, że konwersja niejawnego odwołania istnieje z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="bd998-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="bd998-573">Ze względu na te reguły, gdy element tablicy *reference_type* jest przenoszona jako parametr Reference lub Output, wymagane jest sprawdzenie w czasie wykonywania, aby upewnić się, że rzeczywisty typ elementu tablicy jest identyczny z parametrem.</span><span class="sxs-lookup"><span data-stu-id="bd998-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="bd998-574">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="bd998-575">drugie wywołanie `F` powoduje wygenerowanie `System.ArrayTypeMismatchException`, ponieważ rzeczywisty typ elementu `b` jest `string`, a nie `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="bd998-576">Gdy element członkowski funkcji z tablicą parametrów jest wywoływany w rozwiniętej postaci, wywołanie jest przetwarzane dokładnie tak, jakby wyrażenie tworzenia tablicy z inicjatorem tablicy ([wyrażenia tworzenia tablicy](expressions.md#array-creation-expressions)) zostało wstawione wokół rozwiniętych parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="bd998-577">Na przykład, dana deklaracja</span><span class="sxs-lookup"><span data-stu-id="bd998-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="bd998-578">następujące wywołania rozwiniętej formy metody</span><span class="sxs-lookup"><span data-stu-id="bd998-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="bd998-579">odpowiada dokładnie na</span><span class="sxs-lookup"><span data-stu-id="bd998-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="bd998-580">W szczególności należy zauważyć, że pusta tablica jest tworzona, gdy dla tablicy parametrów nie podano żadnych argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="bd998-581">Gdy argumenty są pomijane z elementu członkowskiego funkcji z odpowiednimi parametrami opcjonalnymi, domyślne argumenty deklaracji elementu członkowskiego funkcji są niejawnie przekazane.</span><span class="sxs-lookup"><span data-stu-id="bd998-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="bd998-582">Ponieważ są one zawsze stałe, ich ocena nie będzie miała wpływu na kolejność oceny pozostałych argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="bd998-583">Wnioskowanie o typie</span><span class="sxs-lookup"><span data-stu-id="bd998-583">Type inference</span></span>

<span data-ttu-id="bd998-584">Gdy metoda ogólna jest wywoływana bez określenia argumentów typu, proces ***wnioskowania o typie*** próbuje wnioskować o argumenty typu dla wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="bd998-585">Obecność wnioskowania o typie umożliwia bardziej wygodną składnię do wywoływania metody ogólnej i umożliwia programisty uniknięcie określania nadmiarowych informacji o typie.</span><span class="sxs-lookup"><span data-stu-id="bd998-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="bd998-586">Na przykład, dana Deklaracja metody:</span><span class="sxs-lookup"><span data-stu-id="bd998-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="bd998-587">możliwe jest wywołanie metody `Choose` bez jawnego określenia argumentu typu:</span><span class="sxs-lookup"><span data-stu-id="bd998-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="bd998-588">Za pomocą wnioskowania o typie argumenty typu `int` i `string` są określane na podstawie argumentów metody.</span><span class="sxs-lookup"><span data-stu-id="bd998-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="bd998-589">Wnioskowanie o typie odbywa się w ramach przetwarzania powiązań metody wywołań metod ([wywołań metod](expressions.md#method-invocations)) i jest wykonywana przed etapem rozwiązywania przeciążenia wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="bd998-590">Gdy dana grupa metod jest określona w wywołaniu metody, a żadne argumenty typu nie są określone jako część wywołania metody, wnioskowanie typu jest stosowane do każdej metody ogólnej w grupie metod.</span><span class="sxs-lookup"><span data-stu-id="bd998-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="bd998-591">Jeśli wnioskowanie o typie powiedzie się, argumenty typu wywnioskowanego są używane do określenia typów argumentów dla kolejnego rozwiązania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="bd998-592">Jeśli rozwiązanie przeciążenia wybiera metodę rodzajową jako jedną do wywołania, wówczas argumenty typu wywnioskowanego są używane jako argumenty typu dla wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="bd998-593">Jeśli wnioskowanie o typie dla określonej metody nie powiedzie się, ta metoda nie bierze udziału w rozpoznawaniu przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="bd998-594">Błąd wnioskowania o typie, w obu przypadkach nie powoduje błędu powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="bd998-595">Jednak często prowadzi do błędu w czasie trwania powiązania, gdy rozwiązanie przeciążenia nie może znaleźć żadnych odpowiednich metod.</span><span class="sxs-lookup"><span data-stu-id="bd998-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="bd998-596">Jeśli podana liczba argumentów jest inna niż liczba parametrów w metodzie, wnioskowanie natychmiast zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="bd998-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="bd998-597">W przeciwnym razie Załóżmy, że metoda ogólna ma następujący podpis:</span><span class="sxs-lookup"><span data-stu-id="bd998-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="bd998-598">Za pomocą wywołania metody w formularzu `M(E1...Em)` zadanie wnioskowania o typie polega na znalezieniu unikatowych argumentów typu `S1...Sn` dla każdego z parametrów typu `X1...Xn`, aby wywołanie `M<S1...Sn>(E1...Em)` było prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="bd998-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="bd998-599">Podczas procesu wnioskowania każdy parametr typu `Xi` został *rozwiązany* do określonego typu `Si` lub *nienaprawione* ze skojarzonym zestawem *granic*.</span><span class="sxs-lookup"><span data-stu-id="bd998-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="bd998-600">Każda granica jest częścią typu `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="bd998-601">Początkowo Każda zmienna typu `Xi` jest nierozwiązana z pustym zestawem granic.</span><span class="sxs-lookup"><span data-stu-id="bd998-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="bd998-602">Wnioskowanie o typie odbywa się w fazach.</span><span class="sxs-lookup"><span data-stu-id="bd998-602">Type inference takes place in phases.</span></span> <span data-ttu-id="bd998-603">Każda faza podejmie próbę ustalenia argumentów typu dla większej liczby zmiennych typu w oparciu o wyniki poprzedniej fazy.</span><span class="sxs-lookup"><span data-stu-id="bd998-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="bd998-604">Pierwsza faza polega na wstępnym wnioskach o granicach, podczas gdy druga faza naprawia zmienne typu dla określonych typów i wnioskuje o dalsze granice.</span><span class="sxs-lookup"><span data-stu-id="bd998-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="bd998-605">Druga faza może być konieczna wielokrotnie.</span><span class="sxs-lookup"><span data-stu-id="bd998-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="bd998-606">*Uwaga:* Wnioskowanie o typie ma miejsce nie tylko wtedy, gdy wywoływana jest metoda ogólna.</span><span class="sxs-lookup"><span data-stu-id="bd998-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="bd998-607">Wnioskowanie o typie dla konwersji grup metod jest opisane w [wnioskach o typie do konwersji grup metod](expressions.md#type-inference-for-conversion-of-method-groups) i znalezienie najlepszego wspólnego typu zestawu wyrażeń jest opisany w artykule [Znajdowanie najlepszego wspólnego typu zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="bd998-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="bd998-608">Pierwsza faza</span><span class="sxs-lookup"><span data-stu-id="bd998-608">The first phase</span></span>

<span data-ttu-id="bd998-609">Dla każdego z argumentów metody `Ei`:</span><span class="sxs-lookup"><span data-stu-id="bd998-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="bd998-610">Jeśli `Ei` jest funkcją anonimową, *jawne wnioskowanie o typie parametru* ([jawne wnioskowanie o typie parametrów](expressions.md#explicit-parameter-type-inferences)) zostało wykonane z `Ei` do `Ti`</span><span class="sxs-lookup"><span data-stu-id="bd998-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="bd998-611">W przeciwnym razie, jeśli `Ei` ma typ `U` i `xi` jest parametrem value, nastąpiło *przewnioskowanie o niższej granicy* *od* `U` *do* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="bd998-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="bd998-612">W przeciwnym razie, jeśli `Ei` ma typ `U` i `xi` to `ref` lub `out` parametr, to *w* przypadku `U` nastąpiło *dokładne wnioskowanie* *o* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="bd998-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="bd998-613">W przeciwnym razie dla tego argumentu nie są wykonywane żadne wnioskowanie.</span><span class="sxs-lookup"><span data-stu-id="bd998-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="bd998-614">Druga faza</span><span class="sxs-lookup"><span data-stu-id="bd998-614">The second phase</span></span>

<span data-ttu-id="bd998-615">Druga faza przebiega w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="bd998-616">Wszystkie *nienaprawione* zmienne typu `Xi`, które nie są *zależne od* ([zależność](expressions.md#dependence)) dowolnych `Xj` są naprawione ([naprawianie](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="bd998-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="bd998-617">Jeśli nie istnieją takie zmienne typu, wszystkie *nienaprawione* zmienne typu `Xi` są *naprawione* , dla których wszystkie następujące wstrzymano:</span><span class="sxs-lookup"><span data-stu-id="bd998-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="bd998-618">Istnieje co najmniej jedna zmienna typu `Xj`, która zależy od `Xi`</span><span class="sxs-lookup"><span data-stu-id="bd998-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="bd998-619">`Xi` zawiera niepusty zestaw granic</span><span class="sxs-lookup"><span data-stu-id="bd998-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="bd998-620">Jeśli nie istnieją takie zmienne typu, a nadal istnieją *nienaprawione* zmienne typu, wnioskowanie o typie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="bd998-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="bd998-621">W przeciwnym razie, jeśli nie istnieją żadne dalsze zmienne typu *nienaprawionego* , wnioskowanie o typie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="bd998-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="bd998-622">W przeciwnym razie dla wszystkich argumentów `Ei` z odpowiadającym typem parametru `Ti`, gdzie *typy danych wyjściowych* ([typy wyjściowe](expressions.md#output-types)) zawierają *nienaprawione* zmienne typu `Xj`, ale *typy wejściowe* ([typy wejściowe](expressions.md#input-types)) nie,  *Wnioskowanie o typie danych wyjściowych* ([wnioskowanie o typie danych wyjściowych](expressions.md#output-type-inferences)) zostało wykonane *z* 1 *do* 3.</span><span class="sxs-lookup"><span data-stu-id="bd998-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="bd998-623">Następnie druga faza jest powtarzana.</span><span class="sxs-lookup"><span data-stu-id="bd998-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="bd998-624">Typy wejściowe</span><span class="sxs-lookup"><span data-stu-id="bd998-624">Input types</span></span>

<span data-ttu-id="bd998-625">Jeśli `E` jest grupą metod lub niejawnie wpisaną funkcją anonimową, a `T` jest typem delegata lub typem drzewa wyrażenia, wszystkie typy parametrów `T` są *typami wejściowymi* `E` *z typem* `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="bd998-626">Typy wyjściowe</span><span class="sxs-lookup"><span data-stu-id="bd998-626">Output types</span></span>

<span data-ttu-id="bd998-627">Jeśli `E` to grupa metod lub funkcja anonimowa, a `T` jest typem delegata lub typem drzewa wyrażenia, zwracany typ `T` jest *typem danych wyjściowych* `E` *z typem* `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="bd998-628">Od</span><span class="sxs-lookup"><span data-stu-id="bd998-628">Dependence</span></span>

<span data-ttu-id="bd998-629">Zmienna typu *nienaprawionego* `Xi` *zależy bezpośrednio* od nieustalonej zmiennej typu `Xj`, jeśli dla pewnego argumentu `Ek` z typem `Tk` `Xj` występuje w *typie danych wejściowych* `Ek` z typem `Tk` i 0 występuje w  *typ wyjścia* 2 z typem 3.</span><span class="sxs-lookup"><span data-stu-id="bd998-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="bd998-630">`Xj` *zależy* od `Xi`, jeśli `Xj` *zależy bezpośrednio* od `Xi` lub jeśli `Xi` *zależy bezpośrednio* od `Xk` i `Xk` *zależy* od 1.</span><span class="sxs-lookup"><span data-stu-id="bd998-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="bd998-631">W tym przypadku "zależy od" jest przechodnie, ale nie jest zamykany zwrotnie "jest zależny od bezpośredniej".</span><span class="sxs-lookup"><span data-stu-id="bd998-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="bd998-632">Wnioskowanie o typie danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="bd998-632">Output type inferences</span></span>

<span data-ttu-id="bd998-633">*Wnioskowanie o typie danych wyjściowych* jest wykonywane *z* wyrażenia `E` *do* typu `T` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="bd998-634">Jeśli `E` to funkcja anonimowa z wywnioskowanym typem zwracanym `U` ([wywnioskowany typ zwracany](expressions.md#inferred-return-type)) i `T` jest typem delegata lub typem drzewa wyrażenia z typem zwracanym `Tb`, a następnie *wnioskem* o niższym[powiązaniu](expressions.md#lower-bound-inferences) wprowadzono *z* `U` *do* 0.</span><span class="sxs-lookup"><span data-stu-id="bd998-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="bd998-635">W przeciwnym razie, jeśli `E` jest grupą metod, a `T` jest typem delegata lub typem drzewa wyrażenia z typami parametrów `T1...Tk` i typem zwracanym `Tb`, a rozpoznawanie przeciążenia `E` z typami `T1...Tk` daje jedną metodę z typem zwracanym `U` , a następnie *w @no__t-* 9 *do* 1 nadano *dolny Zakres wnioskowania* .</span><span class="sxs-lookup"><span data-stu-id="bd998-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="bd998-636">W przeciwnym razie, jeśli `E` jest wyrażeniem z typem `U`, to w przypadku wartości z zakresu *od* @no__t *do* `T` nastąpi *przewnioskowanie o niższym* poziomie.</span><span class="sxs-lookup"><span data-stu-id="bd998-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="bd998-637">W przeciwnym razie nie są wykonywane żadne wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="bd998-638">Wnioskowanie typu jawnego parametru</span><span class="sxs-lookup"><span data-stu-id="bd998-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="bd998-639">*Jawne wnioskowanie o typie parametrów* jest wykonywane *z* wyrażenia `E` *do* typu `T` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="bd998-640">Jeśli `E` jest jawnie wpisaną funkcją anonimową z typami parametrów `U1...Uk` i `T` to typ delegata lub typ drzewa wyrażenia z typami parametrów `V1...Vk`, a następnie dla każdego @no__t- 4 zostanie wykonane[](expressions.md#exact-inferences) *dokładne wnioskowanie (dokładne wnioskowania) od* `Ui` *do* odpowiadającego 0.</span><span class="sxs-lookup"><span data-stu-id="bd998-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="bd998-641">Dokładne wnioskowanie</span><span class="sxs-lookup"><span data-stu-id="bd998-641">Exact inferences</span></span>

<span data-ttu-id="bd998-642">*Dokładne wnioskowanie* *z* typu `U` *do* typu `V` jest wykonywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="bd998-643">Jeśli `V` to jeden z *nieustalonych* `Xi`, `U` zostanie dodany do zestawu ścisłych granic dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="bd998-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="bd998-644">W przeciwnym razie zestawy `V1...Vk` i `U1...Uk` są określane przez sprawdzenie, czy którykolwiek z następujących przypadków ma zastosowanie:</span><span class="sxs-lookup"><span data-stu-id="bd998-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="bd998-645">`V` jest typem tablicy `V1[...]` i `U` jest typem tablicy `U1[...]` tej samej rangi</span><span class="sxs-lookup"><span data-stu-id="bd998-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="bd998-646">`V` jest typem `V1?` i `U` jest typem `U1?`</span><span class="sxs-lookup"><span data-stu-id="bd998-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="bd998-647">`V` jest konstruowanym typem `C<V1...Vk>`and `U` jest konstruowany typ `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="bd998-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="bd998-648">Jeśli którykolwiek z tych przypadków ma zastosowanie, to *dokładne wnioskowanie* jest wykonywane *z* każdego `Ui` *do* odpowiadającego `Vi`.</span><span class="sxs-lookup"><span data-stu-id="bd998-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="bd998-649">W przeciwnym razie nie wprowadzono żadnych wniosków.</span><span class="sxs-lookup"><span data-stu-id="bd998-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="bd998-650">Wnioskowanie o niższych ograniczeniach</span><span class="sxs-lookup"><span data-stu-id="bd998-650">Lower-bound inferences</span></span>

<span data-ttu-id="bd998-651">*Mniej ograniczone wnioskowanie* *z* typu `U` *do* typu `V` jest wykonywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="bd998-652">Jeśli `V` to jeden z *nieustalonych* `Xi`, `U` zostanie dodany do zestawu dolnych granic dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="bd998-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="bd998-653">W przeciwnym razie, jeśli `V` jest typem `V1?`and `U` jest typem `U1?`, to Dolna granica jest wprowadzana z `U1` do `V1`.</span><span class="sxs-lookup"><span data-stu-id="bd998-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="bd998-654">W przeciwnym razie zestawy `U1...Uk` i `V1...Vk` są określane przez sprawdzenie, czy którykolwiek z następujących przypadków ma zastosowanie:</span><span class="sxs-lookup"><span data-stu-id="bd998-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="bd998-655">`V` jest typem tablicy `V1[...]` i `U` jest typem tablicy `U1[...]` (lub parametrem typu, którego efektywny typ podstawowy to `U1[...]`) o tej samej rangi</span><span class="sxs-lookup"><span data-stu-id="bd998-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="bd998-656">`V` to jeden z `IEnumerable<V1>`, `ICollection<V1>` lub `IList<V1>`, a `U` jest typem tablicy jednowymiarowej `U1[]` (lub parametrem typu, którego efektywnym typem podstawowym jest `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="bd998-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="bd998-657">`V` jest konstruowaną klasą, strukturą, interfejsem lub typem delegata `C<V1...Vk>` i istnieje unikatowy typ `C<U1...Uk>`, taki jak `U` (lub, jeśli `U` jest parametrem typu, jego skuteczna Klasa bazowa lub którykolwiek element członkowski jego efektywnego zestawu interfejsów) jest taka sama jak , dziedziczy po (bezpośrednio lub pośrednio) lub implementuje (bezpośrednio lub pośrednio) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="bd998-658">(Ograniczenie "unikatowość" oznacza, że w interfejsie Case `C<T> {} class U: C<X>, C<Y> {}` nie są wykonywane żadne wnioskowanie podczas wnioskowania od `U` do `C<T>`, ponieważ `U1` może być `X` lub `Y`).</span><span class="sxs-lookup"><span data-stu-id="bd998-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="bd998-659">Jeśli którykolwiek z tych przypadków stosuje, wnioskowanie jest wykonywane *z* każdego `Ui` *do* odpowiadającego `Vi` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="bd998-660">Jeśli `Ui` jest nieznane jako typ referencyjny, zostanie wykonane *dokładne wnioskowanie*</span><span class="sxs-lookup"><span data-stu-id="bd998-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="bd998-661">W przeciwnym razie, jeśli `U` jest typem tablicy, zostanie wykonane *mniej ograniczone wnioskowanie*</span><span class="sxs-lookup"><span data-stu-id="bd998-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="bd998-662">W przeciwnym razie, jeśli `V` jest `C<V1...Vk>`, wnioskowanie zależy od typu i-ty parametru `C`:</span><span class="sxs-lookup"><span data-stu-id="bd998-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="bd998-663">Jeśli jest to współwariant, zostanie wprowadzony *niższy zakres wnioskowania* .</span><span class="sxs-lookup"><span data-stu-id="bd998-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="bd998-664">Jeśli jest kontrawariantne, zostanie wykonane *górne ograniczenie wnioskowania* .</span><span class="sxs-lookup"><span data-stu-id="bd998-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="bd998-665">Jeśli jest niezmienna, następuje *dokładne wnioskowanie* .</span><span class="sxs-lookup"><span data-stu-id="bd998-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="bd998-666">W przeciwnym razie nie są wykonywane żadne wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="bd998-667">Wnioskowanie o górnych granicach</span><span class="sxs-lookup"><span data-stu-id="bd998-667">Upper-bound inferences</span></span>

<span data-ttu-id="bd998-668">*Górne ograniczenie wnioskowania* *z* typu `U` *do* typu `V` jest wykonywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="bd998-669">Jeśli `V` to jeden z *nieustalonych* `Xi`, `U` zostanie dodany do zestawu górnych granic dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="bd998-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="bd998-670">W przeciwnym razie zestawy `V1...Vk` i `U1...Uk` są określane przez sprawdzenie, czy którykolwiek z następujących przypadków ma zastosowanie:</span><span class="sxs-lookup"><span data-stu-id="bd998-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="bd998-671">`U` jest typem tablicy `U1[...]` i `V` jest typem tablicy `V1[...]` tej samej rangi</span><span class="sxs-lookup"><span data-stu-id="bd998-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="bd998-672">`U` to jeden z `IEnumerable<Ue>`, `ICollection<Ue>` lub `IList<Ue>`, a `V` jest typem tablicy jednowymiarowej `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="bd998-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="bd998-673">`U` jest typem `U1?` i `V` jest typem `V1?`</span><span class="sxs-lookup"><span data-stu-id="bd998-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="bd998-674">`U` jest konstruowanym typem klasy, struktury, interfejsu lub delegata `C<U1...Uk>` i `V` jest klasą, strukturą, interfejsem lub typem delegata, który jest identyczny z, dziedziczy po (bezpośrednio lub pośrednio) lub implementuje (bezpośrednio lub pośrednio) unikatowy typ `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="bd998-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="bd998-675">(Ograniczenie "unikatowość" oznacza, że jeśli mamy `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, podczas wnioskowania od `C<U1>` do `V<Q>` nie są wykonywane żadne wnioskowanie.</span><span class="sxs-lookup"><span data-stu-id="bd998-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="bd998-676">Wnioskowanie nie jest wykonywane z `U1` do `X<Q>` lub `Y<Q>`).</span><span class="sxs-lookup"><span data-stu-id="bd998-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="bd998-677">Jeśli którykolwiek z tych przypadków stosuje, wnioskowanie jest wykonywane *z* każdego `Ui` *do* odpowiadającego `Vi` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="bd998-678">Jeśli `Ui` jest nieznane jako typ referencyjny, zostanie wykonane *dokładne wnioskowanie*</span><span class="sxs-lookup"><span data-stu-id="bd998-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="bd998-679">W przeciwnym razie, jeśli `V` jest typem tablicy, zostanie wykonane *górne wnioskowanie*</span><span class="sxs-lookup"><span data-stu-id="bd998-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="bd998-680">W przeciwnym razie, jeśli `U` jest `C<U1...Uk>`, wnioskowanie zależy od typu i-ty parametru `C`:</span><span class="sxs-lookup"><span data-stu-id="bd998-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="bd998-681">Jeśli jest to współwariant, nadano *górną granicę* .</span><span class="sxs-lookup"><span data-stu-id="bd998-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="bd998-682">Jeśli jest kontrawariantne, zostanie wykonane *obniżenie wnioskowania o niższym poziomie* .</span><span class="sxs-lookup"><span data-stu-id="bd998-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="bd998-683">Jeśli jest niezmienna, następuje *dokładne wnioskowanie* .</span><span class="sxs-lookup"><span data-stu-id="bd998-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="bd998-684">W przeciwnym razie nie są wykonywane żadne wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="bd998-685">Mocowanie</span><span class="sxs-lookup"><span data-stu-id="bd998-685">Fixing</span></span>

<span data-ttu-id="bd998-686">Zmienna typu *nienaprawionego* `Xi` z zestawem granic jest *ustalona* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="bd998-687">Zestaw *typów kandydatów* `Uj` jest uruchamiany jako zestaw wszystkich typów w zestawie granic dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="bd998-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="bd998-688">Następnie analizujemy poszczególne powiązania `Xi` z kolei: Dla każdego dokładnego powiązania `U` z `Xi` wszystkich typów `Uj`, które nie są identyczne z `U` są usuwane z zestawu kandydatów.</span><span class="sxs-lookup"><span data-stu-id="bd998-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="bd998-689">Dla każdej dolnej granicy `U` z `Xi` wszystkich typów `Uj`, do których *nie* istnieje niejawna konwersja z `U` są usuwane z zestawu kandydatów.</span><span class="sxs-lookup"><span data-stu-id="bd998-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="bd998-690">Dla każdej górnej granicy `U` z `Xi` wszystkich typów `Uj`, z których *nie* istnieje niejawna konwersja `U` są usuwane z zestawu kandydatów.</span><span class="sxs-lookup"><span data-stu-id="bd998-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="bd998-691">Jeśli wśród pozostałych typów kandydatów `Uj` istnieje unikatowy typ `V`, z którego istnieje niejawna konwersja na wszystkie inne typy kandydatów, a następnie `Xi` jest ustalone do `V`.</span><span class="sxs-lookup"><span data-stu-id="bd998-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="bd998-692">W przeciwnym razie wnioskowanie o typie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="bd998-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="bd998-693">Wywnioskowany typ zwracany</span><span class="sxs-lookup"><span data-stu-id="bd998-693">Inferred return type</span></span>

<span data-ttu-id="bd998-694">Wywnioskowany typ zwracany funkcji anonimowej `F` jest używany podczas wnioskowania o typie i rozpoznawania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="bd998-695">Wywnioskowany typ zwracany można określić tylko dla funkcji anonimowej, w której znane są wszystkie typy parametrów, ponieważ są one jawnie podane przez funkcję anonimowej konwersji lub wywnioskowane podczas wnioskowania o typie w otaczającym ogólnym wywołanie metody.</span><span class="sxs-lookup"><span data-stu-id="bd998-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="bd998-696">***Wywnioskowany typ wyniku*** jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="bd998-697">Jeśli treść `F` jest *wyrażeniem* , które ma typ, wywnioskowany typ wyniku `F` jest typem tego wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="bd998-698">Jeśli treść `F` jest *blokiem* , a zestaw wyrażeń w instrukcji `return` bloku ma najlepszy wspólny typ `T` ([znalezienie najlepszego wspólnego typu zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), a następnie typ wynik wnioskowany `F` to `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="bd998-699">W przeciwnym razie nie można wywnioskować typu wyniku dla `F`.</span><span class="sxs-lookup"><span data-stu-id="bd998-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="bd998-700">***Wywnioskowany typ zwracany*** jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="bd998-701">Jeśli `F` jest Async i treścią `F` jest wyrażenie sklasyfikowane jako Nothing ([klasyfikacje wyrażeń](expressions.md#expression-classifications)) lub blok instrukcji, gdzie żadne instrukcje return nie mają wyrażeń, odroczony zwracany typ to `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="bd998-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="bd998-702">Jeśli `F` jest Async i ma wywnioskowany typ wyniku `T`, odroczony typ zwracany to `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="bd998-703">Jeśli `F` nie jest Async i ma wywnioskowany typ wyniku `T`, odroczony typ zwracany to `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="bd998-704">W przeciwnym razie nie można wywnioskować zwracanego typu dla `F`.</span><span class="sxs-lookup"><span data-stu-id="bd998-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="bd998-705">Przykładowo wnioskowanie o typie obejmujące funkcje anonimowe, należy wziąć pod uwagę metodę rozszerzenia `Select` zadeklarowaną w klasie `System.Linq.Enumerable`:</span><span class="sxs-lookup"><span data-stu-id="bd998-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="bd998-706">Przy założeniu, że przestrzeń nazw `System.Linq` została zaimportowana z klauzulą `using` i że Klasa `Customer` z właściwością `Name` typu `string`, Metoda `Select` może służyć do wybierania nazw list klientów:</span><span class="sxs-lookup"><span data-stu-id="bd998-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="bd998-707">Wywołanie metody rozszerzenia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)) `Select` jest przetwarzane przez ponowne zapisanie wywołania do wywołania metody statycznej:</span><span class="sxs-lookup"><span data-stu-id="bd998-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="bd998-708">Ponieważ argumenty typu nie zostały jawnie określone, wnioskowanie typu jest używane do wywnioskowania argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="bd998-709">Najpierw argument `customers` jest powiązany z parametrem `source`, które wywołują `T` do `Customer`.</span><span class="sxs-lookup"><span data-stu-id="bd998-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="bd998-710">Następnie przy użyciu opisanego powyżej procesu wnioskowania o typie funkcji anonimowej `c` ma typ `Customer`, a wyrażenie `c.Name` jest powiązane z typem zwracanym `selector` parametru, wnioskowanie `S` ma być `string`.</span><span class="sxs-lookup"><span data-stu-id="bd998-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="bd998-711">W rezultacie wywołanie jest równoważne</span><span class="sxs-lookup"><span data-stu-id="bd998-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="bd998-712">a wynik jest typu `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="bd998-713">Poniższy przykład ilustruje sposób, w jaki anonimowe funkcje wnioskowania umożliwiają informacje o typie "Flow" między argumentami w wywołaniu metody ogólnej.</span><span class="sxs-lookup"><span data-stu-id="bd998-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="bd998-714">Dana metoda:</span><span class="sxs-lookup"><span data-stu-id="bd998-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="bd998-715">Wnioskowanie o typie dla wywołania:</span><span class="sxs-lookup"><span data-stu-id="bd998-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="bd998-716">wykonuje następujące czynności: Najpierw argument `"1:15:30"` jest powiązany z parametrem `value`, które wywołują `X` do `string`.</span><span class="sxs-lookup"><span data-stu-id="bd998-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="bd998-717">Następnie parametr pierwszej funkcji anonimowej, `s`, otrzymuje wnioskowany typ `string`, a wyrażenie `TimeSpan.Parse(s)` jest powiązane z typem zwracanym `f1`, wnioskowanie `Y` do `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="bd998-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="bd998-718">Na koniec parametr drugiej funkcji anonimowej, `t`, otrzymuje wnioskowany typ `System.TimeSpan`, a wyrażenie `t.TotalSeconds` jest powiązane z typem zwracanym `f2`, wnioskowanie `Z` do `double`.</span><span class="sxs-lookup"><span data-stu-id="bd998-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="bd998-719">W związku z tym wynik wywołania jest typu `double`.</span><span class="sxs-lookup"><span data-stu-id="bd998-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="bd998-720">Wnioskowanie o typie dla konwersji grup metod</span><span class="sxs-lookup"><span data-stu-id="bd998-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="bd998-721">Podobnie jak w przypadku wywołań metod ogólnych, wnioskowanie o typie należy również zastosować, gdy grupa metod `M` zawierająca metodę generyczną jest konwertowana na dany typ delegata `D` ([konwersje grup metod](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="bd998-722">Dana metoda</span><span class="sxs-lookup"><span data-stu-id="bd998-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="bd998-723">a grupa metod `M` jest przypisywana do typu delegata `D` zadanie wnioskowania o typie polega na znalezieniu argumentów typu `S1...Sn`, aby wyrażenie:</span><span class="sxs-lookup"><span data-stu-id="bd998-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="bd998-724">staną się zgodne ([deklaracje delegatów](delegates.md#delegate-declarations)) z `D`.</span><span class="sxs-lookup"><span data-stu-id="bd998-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="bd998-725">W przeciwieństwie do algorytmu wnioskowania o typie dla wywołań metod ogólnych, w tym przypadku są tylko *typy*argumentów, bez *wyrażeń*argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="bd998-726">W szczególności nie ma żadnych funkcji anonimowych i w związku z tym nie ma potrzeby stosowania wielu faz wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="bd998-727">Zamiast tego, wszystkie `Xi` są uznawane za *nienaprawione*, a z każdego typu argumentu `Uj` @no__t *-5 jest* tworzony *wniosek* o *niższej granicy* , `Tj` z `M`.</span><span class="sxs-lookup"><span data-stu-id="bd998-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="bd998-728">Jeśli dla dowolnego `Xi` nie znaleziono żadnych granic, wnioskowanie o typie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="bd998-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="bd998-729">W przeciwnym razie wszystkie `Xi` są *naprawione* jako odpowiadające `Si`, które są wynikiem wnioskowania o typie.</span><span class="sxs-lookup"><span data-stu-id="bd998-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="bd998-730">Znajdowanie najlepszego wspólnego typu zestawu wyrażeń</span><span class="sxs-lookup"><span data-stu-id="bd998-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="bd998-731">W niektórych przypadkach wspólny typ musi zostać wywnioskowany dla zestawu wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="bd998-732">W szczególności typy elementów niejawnie wpisanych tablic i zwracane typy funkcji anonimowych z treści *bloku* są dostępne w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="bd998-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="bd998-733">Intuicyjnie, z uwzględnieniem zestawu wyrażeń `E1...Em` to wnioskowanie powinno być równoważne wywołaniu metody</span><span class="sxs-lookup"><span data-stu-id="bd998-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="bd998-734">z `Ei` jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="bd998-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="bd998-735">Dokładniej, wnioskowanie rozpoczyna się od *nieustalonej* zmiennej typu `X`.</span><span class="sxs-lookup"><span data-stu-id="bd998-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="bd998-736">*Wnioskowanie o typie danych wyjściowych* są następnie wykonywane *z* każdego `Ei` *do* `X`.</span><span class="sxs-lookup"><span data-stu-id="bd998-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="bd998-737">Na koniec `X` jest *stała* i, jeśli to się powiedzie, typ otrzymany `S` jest wynikiem najlepszego wspólnego typu dla wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="bd998-738">Jeśli żaden taki `S` nie istnieje, wyrażenia nie mają najlepszego wspólnego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="bd998-739">Rozpoznanie przeciążenia</span><span class="sxs-lookup"><span data-stu-id="bd998-739">Overload resolution</span></span>

<span data-ttu-id="bd998-740">Rozwiązanie przeciążania to mechanizm umożliwiający wybranie najlepszego elementu członkowskiego funkcji w celu wywołania listy argumentów i zestawu elementów członkowskich funkcji kandydujących.</span><span class="sxs-lookup"><span data-stu-id="bd998-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="bd998-741">Rozwiązanie przeciążania wybiera element członkowski funkcji do wywołania w następujących odrębnych C#kontekstach w obrębie:</span><span class="sxs-lookup"><span data-stu-id="bd998-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="bd998-742">Wywołanie metody o nazwie w *invocation_expression* ([wywołania metody](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="bd998-743">Wywołanie konstruktora wystąpienia o nazwie w *object_creation_expression* ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="bd998-744">Wywołanie metody dostępu indeksatora za poorednictwem *element_access* ([dostęp do elementu](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="bd998-745">Wywołanie wstępnie zdefiniowanego lub zdefiniowanego przez użytkownika operatora przywoływanego w wyrażeniu ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="bd998-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="bd998-746">Każdy z tych kontekstów definiuje zestaw elementów członkowskich funkcji kandydujących i listę argumentów we własnym unikatowy sposób, jak opisano szczegółowo w sekcjach wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="bd998-747">Na przykład zestaw kandydatów dla wywołania metody nie obejmuje metod oznaczonych `override` ([Wyszukiwanie elementów członkowskich](expressions.md#member-lookup)), a metody w klasie bazowej nie są kandydatami, jeśli stosowana jest jakakolwiek metoda w klasie pochodnej ([wywołania metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="bd998-748">Po zidentyfikowaniu elementów członkowskich funkcji kandydujących i listy argumentów wybór najlepszego elementu członkowskiego funkcji jest taki sam we wszystkich przypadkach:</span><span class="sxs-lookup"><span data-stu-id="bd998-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="bd998-749">Z uwzględnieniem zestawu odpowiednich członków funkcji kandydujących należy zlokalizować element członkowski najlepszych funkcji w tym zestawie.</span><span class="sxs-lookup"><span data-stu-id="bd998-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="bd998-750">Jeśli zestaw zawiera tylko jeden element członkowski funkcji, to element członkowski funkcji jest najlepszą funkcją.</span><span class="sxs-lookup"><span data-stu-id="bd998-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="bd998-751">W przeciwnym razie, element członkowski najlepszych funkcji jest składową funkcji, która jest lepsza niż wszystkie inne elementy członkowskie funkcji w odniesieniu do danej listy argumentów, pod warunkiem, że każdy element członkowski funkcji jest porównywany z innymi składowymi funkcji przy użyciu reguł w [funkcji lepsza element członkowski](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="bd998-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="bd998-752">Jeśli nie ma dokładnie jednego elementu członkowskiego funkcji, który jest lepszy niż wszystkie inne składowe funkcji, wywołanie elementu członkowskiego funkcji jest niejednoznaczne i występuje błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-753">W poniższych sekcjach opisano dokładne znaczenie ***elementów członkowskich funkcji*** i ***lepszy element członkowski funkcji***.</span><span class="sxs-lookup"><span data-stu-id="bd998-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="bd998-754">Odpowiedni element członkowski funkcji</span><span class="sxs-lookup"><span data-stu-id="bd998-754">Applicable function member</span></span>

<span data-ttu-id="bd998-755">Członek funkcji jest określany jako ***element członkowski funkcji*** w odniesieniu do listy argumentów `A`, jeśli spełnione są wszystkie z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="bd998-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="bd998-756">Każdy argument w `A` odpowiada parametrowi w deklaracji elementu członkowskiego funkcji zgodnie z opisem w [odpowiednich parametrach](expressions.md#corresponding-parameters), a każdy parametr, do którego nie odpowiada żaden argument, jest parametrem opcjonalnym.</span><span class="sxs-lookup"><span data-stu-id="bd998-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="bd998-757">Dla każdego argumentu w `A` tryb przekazywania parametrów argumentu (tj. Value, `ref` lub `out`) jest identyczny z trybem przekazywania parametrów odpowiedniego parametru, i</span><span class="sxs-lookup"><span data-stu-id="bd998-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="bd998-758">dla parametru value lub tablicy parametrów niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z argumentu do typu odpowiadającego parametru lub</span><span class="sxs-lookup"><span data-stu-id="bd998-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="bd998-759">w przypadku parametru `ref` lub `out` typ argumentu jest identyczny z typem odpowiadającego parametru.</span><span class="sxs-lookup"><span data-stu-id="bd998-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="bd998-760">Po wszystkich parametr `ref` lub `out` jest aliasem dla argumentu, który został przesłany.</span><span class="sxs-lookup"><span data-stu-id="bd998-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="bd998-761">W przypadku składowej funkcji, która zawiera tablicę parametrów, jeśli element członkowski funkcji jest stosowany przez powyższe reguły, jest on uznawany za stosowany w jego ***normalnej postaci***.</span><span class="sxs-lookup"><span data-stu-id="bd998-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="bd998-762">Jeśli element członkowski funkcji zawierający tablicę parametrów nie ma zastosowania w jego normalnej postaci, element członkowski funkcji może być stosowany w jego ***rozwiniętej postaci***:</span><span class="sxs-lookup"><span data-stu-id="bd998-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="bd998-763">Rozwinięty formularz jest konstruowany przez zastąpienie tablicy parametrów w deklaracji elementu członkowskiego funkcji wartością zero lub więcej parametrów wartości typu elementu tablicy parametrów w taki sposób, że liczba argumentów na liście argumentów `A` pasuje do łącznej liczby parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="bd998-764">Jeśli `A` ma mniejszą liczbę argumentów niż liczba stałych parametrów w deklaracji elementu członkowskiego funkcji, rozwinięta forma elementu członkowskiego funkcji nie może być skonstruowana i nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="bd998-765">W przeciwnym razie rozszerzona forma jest stosowana, jeśli dla każdego argumentu w `A` tryb przekazywania parametru argumentu jest identyczny z trybem przekazywania parametrów odpowiadającym mu parametrem.</span><span class="sxs-lookup"><span data-stu-id="bd998-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="bd998-766">dla parametru wartości ustalonej lub parametru wartości utworzonego przez ekspansję niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typu argumentu do typu odpowiadającego parametru lub</span><span class="sxs-lookup"><span data-stu-id="bd998-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="bd998-767">w przypadku parametru `ref` lub `out` typ argumentu jest identyczny z typem odpowiadającego parametru.</span><span class="sxs-lookup"><span data-stu-id="bd998-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="bd998-768">Lepsza składowa funkcji</span><span class="sxs-lookup"><span data-stu-id="bd998-768">Better function member</span></span>

<span data-ttu-id="bd998-769">Na potrzeby określenia lepszego elementu członkowskiego funkcja, Oddzielna lista argumentów A jest zbudowana zawierające tylko wyrażenia argumentów w kolejności, w jakiej występują na liście pierwotnych argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="bd998-770">Listy parametrów dla każdego elementu członkowskiego funkcji kandydującej są konstruowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="bd998-771">Rozwinięty formularz jest używany, jeśli element członkowski funkcji ma zastosowanie tylko w rozwiniętym formularzu.</span><span class="sxs-lookup"><span data-stu-id="bd998-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="bd998-772">Parametry opcjonalne bez odpowiednich argumentów są usuwane z listy parametrów</span><span class="sxs-lookup"><span data-stu-id="bd998-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="bd998-773">Parametry są zmieniane w kolejności, tak że pojawiają się w tym samym miejscu co odpowiadający argument na liście argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="bd998-774">Podano listę argumentów `A` z zestawem wyrażeń argumentów `{E1, E2, ..., En}` i dwa odpowiednie składowe funkcji `Mp` i `Mq` z typami parametrów `{P1, P2, ..., Pn}` i `{Q1, Q2, ..., Qn}`, `Mp` jest zdefiniowana jako ***lepsza składowa funkcji*** niż `Mq`, jeśli</span><span class="sxs-lookup"><span data-stu-id="bd998-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="bd998-775">dla każdego argumentu niejawna konwersja z `Ex` do `Qx` nie jest lepsza niż jawna konwersja z `Ex` do `Px`, i</span><span class="sxs-lookup"><span data-stu-id="bd998-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="bd998-776">dla co najmniej jednego argumentu konwersja z `Ex` do `Px` jest lepsza niż konwersja z `Ex` do `Qx`.</span><span class="sxs-lookup"><span data-stu-id="bd998-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="bd998-777">Podczas przeprowadzania tej oceny, jeśli `Mp` lub `Mq` ma zastosowanie w jego rozwiniętej postaci, `Px` lub `Qx` odwołuje się do parametru w rozwiniętej postaci listy parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="bd998-778">W przypadku, gdy parametr sequences @ no__t-0 i `{Q1, Q2, ..., Qn}` są równoważne (tj. Każda `Pi` ma konwersję tożsamości na odpowiadającą `Qi`), są stosowane następujące reguły podziału powiązania, aby określić lepszy element członkowski funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="bd998-779">Jeśli `Mp` jest metodą nierodzajową i `Mq` jest metodą rodzajową, wówczas `Mp` jest lepsza niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="bd998-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="bd998-780">W przeciwnym razie, jeśli `Mp` ma zastosowanie w swojej normalnej formie i `Mq` ma tablicę `params` i ma zastosowanie tylko w jego rozwiniętej postaci, `Mp` jest lepsza niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="bd998-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="bd998-781">W przeciwnym razie, jeśli `Mp` ma więcej zadeklarowanych parametrów niż `Mq`, wówczas `Mp` jest lepsza niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="bd998-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="bd998-782">Taka sytuacja może wystąpić, jeśli obie metody mają tablice `params` i są stosowane tylko w rozwiniętych formularzach.</span><span class="sxs-lookup"><span data-stu-id="bd998-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="bd998-783">W przeciwnym razie, jeśli wszystkie parametry `Mp` mają odpowiadający argument, a argumenty domyślne muszą zostać zastąpione dla co najmniej jednego parametru opcjonalnego w `Mq`, `Mp` jest lepiej niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="bd998-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="bd998-784">W przeciwnym razie, jeśli `Mp` ma więcej konkretnych typów parametrów niż `Mq`, wówczas `Mp` jest lepsza niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="bd998-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="bd998-785">Niech `{R1, R2, ..., Rn}` i `{S1, S2, ..., Sn}` reprezentują typy parametrów nieskonkretyzowanych i nierozwiniętych `Mp` i `Mq`.</span><span class="sxs-lookup"><span data-stu-id="bd998-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="bd998-786">typy parametrów `Mp` są bardziej szczegółowe niż `Mq`, jeśli dla każdego parametru `Rx` nie są mniej specyficzne niż `Sx`, a dla co najmniej jednego parametru `Rx` jest bardziej szczegółowy niż `Sx`:</span><span class="sxs-lookup"><span data-stu-id="bd998-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="bd998-787">Parametr typu jest mniej specyficzny niż parametr niebędący typem.</span><span class="sxs-lookup"><span data-stu-id="bd998-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="bd998-788">Rekursywnie skonstruowany typ jest bardziej szczegółowy niż inny skonstruowany typ (z tą samą liczbą argumentów typu), jeśli co najmniej jeden argument typu jest bardziej szczegółowy i żaden argument typu nie jest mniej specyficzny niż odpowiedni argument typu w drugim.</span><span class="sxs-lookup"><span data-stu-id="bd998-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="bd998-789">Typ tablicy jest bardziej szczegółowy niż inny typ tablicy (o tej samej liczbie wymiarów), jeśli typ elementu pierwszego jest bardziej szczegółowy niż typ elementu drugiego.</span><span class="sxs-lookup"><span data-stu-id="bd998-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="bd998-790">W przeciwnym razie, jeśli jeden element członkowski jest niezniesionym operatorem, a drugi jest operatorem podnoszenia, to niezniesione.</span><span class="sxs-lookup"><span data-stu-id="bd998-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="bd998-791">W przeciwnym razie żaden element członkowski funkcji nie jest lepszy.</span><span class="sxs-lookup"><span data-stu-id="bd998-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="bd998-792">Lepsza konwersja z wyrażenia</span><span class="sxs-lookup"><span data-stu-id="bd998-792">Better conversion from expression</span></span>

<span data-ttu-id="bd998-793">Nadano niejawną konwersję `C1`, która konwertuje z wyrażenia `E` na typ `T1` i niejawną konwersję `C2`, która jest konwertowana z wyrażenia `E` do typu `T2`, `C1` jest ***lepszą konwersją*** niż `C2`, jeśli @no__ t-9 nie pasuje dokładnie do 0 i co najmniej jednego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="bd998-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="bd998-794">`E` dokładnie pasuje do `T1` ([dokładnie pasujące wyrażenie](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="bd998-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="bd998-795">`T1` to lepsza wartość docelowa konwersji niż `T2` ([lepsza wartość docelowa konwersji](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="bd998-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="bd998-796">Wyrażenie dokładnie pasujące</span><span class="sxs-lookup"><span data-stu-id="bd998-796">Exactly matching Expression</span></span>

<span data-ttu-id="bd998-797">Uwzględniając wyrażenie `E` i typ `T`, `E` dokładnie pasuje do `T`, jeśli jedno z następujących posiada:</span><span class="sxs-lookup"><span data-stu-id="bd998-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="bd998-798">`E` ma typ `S` i konwersja tożsamości istnieje z `S` do `T`</span><span class="sxs-lookup"><span data-stu-id="bd998-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="bd998-799">`E` to funkcja anonimowa, `T` jest typem delegata `D` lub typem drzewa wyrażenia `Expression<D>` i jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="bd998-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="bd998-800">Wywnioskowany typ zwracany `X` istnieje dla `E` w kontekście listy parametrów `D` ([wnioskowany typ zwracany](expressions.md#inferred-return-type)) i konwersja tożsamości istnieje z `X` do typu zwracanego `D`</span><span class="sxs-lookup"><span data-stu-id="bd998-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="bd998-801">Wartość `E` jest nieasynchroniczna i `D` ma typ zwracany `Y` lub `E` jest Async i `D` ma typ zwracany `Task<Y>` i jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="bd998-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="bd998-802">Treść `E` jest wyrażeniem, które dokładnie pasuje do `Y`</span><span class="sxs-lookup"><span data-stu-id="bd998-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="bd998-803">Treść `E` to blok instrukcji, gdzie każda instrukcja return zwraca wyrażenie, które dokładnie pasuje do `Y`</span><span class="sxs-lookup"><span data-stu-id="bd998-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="bd998-804">Lepszy cel konwersji</span><span class="sxs-lookup"><span data-stu-id="bd998-804">Better conversion target</span></span>

<span data-ttu-id="bd998-805">Podano dwa różne typy `T1` i `T2` `T1` to lepsza wartość docelowa konwersji niż `T2`, Jeśli niejawna konwersja z `T2` do `T1` istnieje, a co najmniej jeden z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="bd998-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="bd998-806">Niejawna konwersja z `T1` na `T2` istnieje</span><span class="sxs-lookup"><span data-stu-id="bd998-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="bd998-807">`T1` jest typem delegata `D1` lub typem drzewa wyrażenia `Expression<D1>`, `T2` jest typem delegata `D2` lub typem drzewa wyrażenia `Expression<D2>`, `D1` ma typ zwracany `S1` i jedną z następujących blokad :</span><span class="sxs-lookup"><span data-stu-id="bd998-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="bd998-808">`D2` to zwracanie void</span><span class="sxs-lookup"><span data-stu-id="bd998-808">`D2` is void returning</span></span>
   * <span data-ttu-id="bd998-809">`D2` ma typ zwracany `S2`, a `S1` to lepsza wartość docelowa konwersji niż `S2`</span><span class="sxs-lookup"><span data-stu-id="bd998-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="bd998-810">`T1` to `Task<S1>`, `T2` jest `Task<S2>`, a `S1` jest lepszym celem konwersji niż `S2`</span><span class="sxs-lookup"><span data-stu-id="bd998-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="bd998-811">`T1` jest `S1` lub `S1?`, gdzie `S1` jest podpisanym typem całkowitym, a `T2` jest `S2` lub `S2?`, gdzie `S2` jest niepodpisanym typem całkowitym.</span><span class="sxs-lookup"><span data-stu-id="bd998-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="bd998-812">Opracowany</span><span class="sxs-lookup"><span data-stu-id="bd998-812">Specifically:</span></span>
   * <span data-ttu-id="bd998-813">`S1` to `sbyte` i `S2` to `byte`, `ushort`, `uint` lub `ulong`</span><span class="sxs-lookup"><span data-stu-id="bd998-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="bd998-814">`S1` to `short` i `S2` to `ushort`, `uint` lub `ulong`</span><span class="sxs-lookup"><span data-stu-id="bd998-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="bd998-815">`S1` jest `int` i `S2` jest `uint` lub `ulong`</span><span class="sxs-lookup"><span data-stu-id="bd998-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="bd998-816">`S1` jest `long` i `S2` jest `ulong`</span><span class="sxs-lookup"><span data-stu-id="bd998-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="bd998-817">Przeciążanie klas ogólnych</span><span class="sxs-lookup"><span data-stu-id="bd998-817">Overloading in generic classes</span></span>

<span data-ttu-id="bd998-818">Chociaż sygnatury jako zadeklarowane muszą być unikatowe, możliwe jest zastępowanie argumentów typu w identycznych sygnaturach.</span><span class="sxs-lookup"><span data-stu-id="bd998-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="bd998-819">W takich przypadkach reguły łamania i przeciążania przeciążenia powyżej spowodują wybranie najbardziej szczegółowego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="bd998-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="bd998-820">W poniższych przykładach przedstawiono nieprawidłowe i nieprawidłowe przeciążenia zgodne z tą regułą:</span><span class="sxs-lookup"><span data-stu-id="bd998-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="bd998-821">Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia</span><span class="sxs-lookup"><span data-stu-id="bd998-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="bd998-822">W przypadku większości operacji powiązanych dynamicznie zestaw możliwych kandydatów do rozwiązania jest nieznany w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="bd998-823">W niektórych przypadkach, jednak zestaw kandydatów jest znany w czasie kompilacji:</span><span class="sxs-lookup"><span data-stu-id="bd998-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="bd998-824">Wywołania metody statycznej z argumentami dynamicznymi</span><span class="sxs-lookup"><span data-stu-id="bd998-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="bd998-825">Wywołania metody wystąpienia, w których odbiorca nie jest wyrażeniem dynamicznym</span><span class="sxs-lookup"><span data-stu-id="bd998-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="bd998-826">Wywołania indeksatora, w których odbiorca nie jest wyrażeniem dynamicznym</span><span class="sxs-lookup"><span data-stu-id="bd998-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="bd998-827">Wywołania konstruktora z argumentami dynamicznymi</span><span class="sxs-lookup"><span data-stu-id="bd998-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="bd998-828">W takich przypadkach jest przeprowadzane ograniczone sprawdzanie czasu kompilacji dla każdego kandydata, aby zobaczyć, czy którykolwiek z nich może być stosowany w czasie wykonywania. Ta kontrola obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="bd998-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-829">Wnioskowanie o typie częściowym: Dowolny argument typu, który nie zależy bezpośrednio lub pośrednio od argumentu typu `dynamic` jest wnioskowany przy użyciu reguł [typu wnioskowania](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="bd998-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="bd998-830">Pozostałe argumenty typu są nieznane.</span><span class="sxs-lookup"><span data-stu-id="bd998-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="bd998-831">Sprawdzenie częściowego zastosowania: Stosowanie jest sprawdzane zgodnie z [odpowiednimi elementami członkowskimi funkcji](expressions.md#applicable-function-member), ale ignorowanie parametrów, których typy są nieznane.</span><span class="sxs-lookup"><span data-stu-id="bd998-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="bd998-832">Jeśli żaden kandydat nie przekaże tego testu, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="bd998-833">Wywołanie elementu członkowskiego funkcji</span><span class="sxs-lookup"><span data-stu-id="bd998-833">Function member invocation</span></span>

<span data-ttu-id="bd998-834">W tej sekcji opisano proces, który odbywa się w czasie wykonywania w celu wywołania określonego elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="bd998-835">Przyjęto założenie, że proces powiązania czasu już ustalił określony element członkowski do wywołania, prawdopodobnie przez zastosowanie rozpoznawania przeciążenia do zestawu elementów członkowskich funkcji kandydujących.</span><span class="sxs-lookup"><span data-stu-id="bd998-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="bd998-836">Do celów opisywania procesu wywołania składowe funkcji są podzielone na dwie kategorie:</span><span class="sxs-lookup"><span data-stu-id="bd998-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="bd998-837">Statyczne składowe funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-837">Static function members.</span></span> <span data-ttu-id="bd998-838">Są to konstruktory wystąpień, metody statyczne, dostęp do właściwości statycznych i Operatory zdefiniowane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bd998-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="bd998-839">Statyczne składowe funkcji są zawsze niewirtualne.</span><span class="sxs-lookup"><span data-stu-id="bd998-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="bd998-840">Elementy członkowskie funkcji wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-840">Instance function members.</span></span> <span data-ttu-id="bd998-841">Są to metody wystąpień, metod dostępu do właściwości wystąpienia i obiektu dostępu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="bd998-842">Elementy członkowskie funkcji wystąpienia są niewirtualne lub wirtualne i są zawsze wywoływane w konkretnym wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="bd998-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="bd998-843">Wystąpienie jest obliczane przez wyrażenie wystąpienia i jest dostępne w składowej funkcji jako `this` ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="bd998-844">Przetwarzanie elementu członkowskiego funkcji w czasie wykonywania składa się z następujących kroków, gdzie `M` jest składową funkcji i, jeśli `M` jest składową wystąpienia, `E` jest wyrażeniem wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="bd998-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="bd998-845">Jeśli `M` jest składową funkcji statycznej:</span><span class="sxs-lookup"><span data-stu-id="bd998-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="bd998-846">Lista argumentów jest szacowana zgodnie z opisem w liście [argumentów](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="bd998-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="bd998-847">`M` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-847">`M` is invoked.</span></span>

*  <span data-ttu-id="bd998-848">Jeśli `M` jest składową funkcji wystąpienia zadeklarowaną w *value_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="bd998-849">`E` jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-849">`E` is evaluated.</span></span> <span data-ttu-id="bd998-850">Jeśli ta Ocena spowoduje wyjątek, kolejne kroki nie są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="bd998-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="bd998-851">Jeśli `E` nie jest sklasyfikowany jako zmienna, tworzona jest tymczasowa zmienna lokalna typu `E` i zostanie przypisana wartość `E` do tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="bd998-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="bd998-852">`E` jest następnie ponownie klasyfikowane jako odwołanie do tej tymczasowej zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="bd998-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="bd998-853">Zmienna tymczasowa jest dostępna jako `this` w `M`, ale nie w żaden inny sposób.</span><span class="sxs-lookup"><span data-stu-id="bd998-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="bd998-854">W tym celu, tylko wtedy, gdy `E` jest wartością true, można, aby obiekt wywołujący przestrzegał zmian, które `M` `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="bd998-855">Lista argumentów jest szacowana zgodnie z opisem w liście [argumentów](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="bd998-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="bd998-856">`M` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-856">`M` is invoked.</span></span> <span data-ttu-id="bd998-857">Zmienna, do której odwołuje się `E`, to zmienna, do której odwołuje się `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="bd998-858">Jeśli `M` jest składową funkcji wystąpienia zadeklarowaną w *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="bd998-859">`E` jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-859">`E` is evaluated.</span></span> <span data-ttu-id="bd998-860">Jeśli ta Ocena spowoduje wyjątek, kolejne kroki nie są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="bd998-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="bd998-861">Lista argumentów jest szacowana zgodnie z opisem w liście [argumentów](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="bd998-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="bd998-862">Jeśli typ `E` to *value_type*, konwersja z opakowania ([konwersje z opakowania](types.md#boxing-conversions)) jest wykonywana w celu przekonwertowania `E` do typu `object`, a `E` jest uważana za typ `object` w poniższych krokach.</span><span class="sxs-lookup"><span data-stu-id="bd998-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="bd998-863">W takim przypadku `M` może być tylko składową `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="bd998-864">Wartość `E` jest sprawdzana jako prawidłowa.</span><span class="sxs-lookup"><span data-stu-id="bd998-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="bd998-865">Jeśli wartość `E` jest `null`, zgłaszany jest `System.NullReferenceException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="bd998-866">Implementacja elementu członkowskiego funkcji do wywołania została określona:</span><span class="sxs-lookup"><span data-stu-id="bd998-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="bd998-867">Jeśli typ powiązania `E` jest interfejsem, element członkowski funkcji do wywołania jest implementacją `M` dostarczoną przez typ czasu wykonywania wystąpienia przywoływanego przez `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="bd998-868">Ten element członkowski funkcji jest określany przez zastosowanie reguł mapowania interfejsu ([Mapowanie interfejsu](interfaces.md#interface-mapping)) do określenia implementacji `M` dostarczonej przez typ czasu wykonywania wystąpienia, do którego odwołuje się `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="bd998-869">W przeciwnym razie, jeśli `M` jest składową funkcji wirtualnej, element członkowski funkcji do wywołania jest implementacją `M` dostarczoną przez typ czasu wykonywania wystąpienia przywoływanego przez `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="bd998-870">Ten element członkowski funkcji jest określany przez zastosowanie zasad określania najbardziej pochodnej implementacji ([metod wirtualnych](classes.md#virtual-methods)) `M` w odniesieniu do typu czasu wykonywania wystąpienia, do którego odwołuje się `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="bd998-871">W przeciwnym razie `M` jest składową funkcji innej niż wirtualna, a element członkowski funkcji do wywołania jest `M`.</span><span class="sxs-lookup"><span data-stu-id="bd998-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="bd998-872">Implementacja elementu członkowskiego funkcji określona w powyższym kroku jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="bd998-873">Obiekt, do którego odwołuje się `E`, to obiekt, do którego odwołuje się `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="bd998-874">Wywołania w wystąpieniach opakowanych</span><span class="sxs-lookup"><span data-stu-id="bd998-874">Invocations on boxed instances</span></span>

<span data-ttu-id="bd998-875">Element członkowski funkcji zaimplementowany w *value_type* może być wywoływany za pomocą wystąpienia opakowanego tego *value_type* w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="bd998-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="bd998-876">Gdy element członkowski funkcji jest `override` metody dziedziczonej z typu `object` i jest wywoływany za pośrednictwem wyrażenia wystąpienia typu `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="bd998-877">Gdy element członkowski funkcji jest implementacją składowej funkcji interfejsu i jest wywoływany za pomocą wyrażenia wystąpienia elementu *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="bd998-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="bd998-878">Gdy element członkowski funkcji jest wywoływany przez delegata.</span><span class="sxs-lookup"><span data-stu-id="bd998-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="bd998-879">W takich sytuacjach zapakowane wystąpienie jest traktowane jako zmienna *value_type*, a ta zmienna to zmienna, do której odwołuje się `this` w wywołaniu elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="bd998-880">W szczególności oznacza to, że gdy element członkowski funkcji jest wywoływany w przypadku wystąpienia zapakowanego, możliwe jest, aby element członkowski funkcji modyfikował wartość zawartą w zapakowanym wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="bd998-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="bd998-881">Wyrażenia podstawowe</span><span class="sxs-lookup"><span data-stu-id="bd998-881">Primary expressions</span></span>

<span data-ttu-id="bd998-882">Wyrażenia podstawowe zawierają najprostszą postać wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="bd998-883">Wyrażenia podstawowe są podzielone między *array_creation_expression*s i *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="bd998-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="bd998-884">W ten sposób w ten sposób traktowanie wyrażenia tworzenia tablicowe, a nie wymienienie go wraz z innymi prostymi formularzami wyrażeń, umożliwia gramatykę w celu uniemożliwienia potencjalnie mylącego kodu, takiego jak</span><span class="sxs-lookup"><span data-stu-id="bd998-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="bd998-885">który w przeciwnym razie mógłby być interpretowany jako</span><span class="sxs-lookup"><span data-stu-id="bd998-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="bd998-886">Literały</span><span class="sxs-lookup"><span data-stu-id="bd998-886">Literals</span></span>

<span data-ttu-id="bd998-887">Element *primary_expression* , który składa się z *literału* ([literałów](lexical-structure.md#literals)), jest klasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="bd998-888">Ciągi interpolowane</span><span class="sxs-lookup"><span data-stu-id="bd998-888">Interpolated strings</span></span>

<span data-ttu-id="bd998-889">*Interpolated_string_expression* składa się ze znaku `$`, po którym następuje zwykły lub Verbatim literał ciągu, w którym znajdują się przerwy, rozdzielane przez `{` i `}`, otaczające wyrażenia i specyfikacje formatowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="bd998-890">Interpolowane wyrażenie ciągu jest wynikiem *interpolated_string_literal* , który został podzielony na poszczególne tokeny, zgodnie z opisem w [interpolowanych literałach ciągu](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="bd998-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="bd998-891">*Constant_expression* w interpolacji musi mieć niejawną konwersję na `int`.</span><span class="sxs-lookup"><span data-stu-id="bd998-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="bd998-892">*Interpolated_string_expression* jest sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="bd998-893">Jeśli jest ona natychmiast konwertowana na `System.IFormattable` lub `System.FormattableString` z niejawną interpolowaną konwersją ciągu ([niejawne konwersje ciągów niejawnie](conversions.md#implicit-interpolated-string-conversions)), interpolowane wyrażenie ciągu ma ten typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="bd998-894">W przeciwnym razie ma typ `string`.</span><span class="sxs-lookup"><span data-stu-id="bd998-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="bd998-895">Jeśli typ ciągu interpolowanego jest `System.IFormattable` lub `System.FormattableString`, znaczenie jest wywołaniem `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="bd998-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="bd998-896">Jeśli typ jest `string`, znaczenie wyrażenia jest wywołaniem do `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="bd998-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="bd998-897">W obu przypadkach lista argumentów wywołania składa się z literału ciągu formatu z symbolami zastępczymi dla każdej interpolacji oraz argumentu dla każdego wyrażenia odpowiadającego posiadaczom miejsc.</span><span class="sxs-lookup"><span data-stu-id="bd998-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="bd998-898">Literał ciągu formatu jest skonstruowany w następujący sposób, gdzie `N` to liczba interpolacji w *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="bd998-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="bd998-899">Jeśli *interpolated_regular_string_whole* lub *interpolated_verbatim_string_whole* następuje po znaku `$`, a następnie literał ciągu formatu jest tym tokenem.</span><span class="sxs-lookup"><span data-stu-id="bd998-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="bd998-900">W przeciwnym razie literał ciągu formatu składa się z:</span><span class="sxs-lookup"><span data-stu-id="bd998-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="bd998-901">Najpierw *interpolated_regular_string_start* lub *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="bd998-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="bd998-902">Następnie dla każdej liczby `I` z `0` do `N-1`:</span><span class="sxs-lookup"><span data-stu-id="bd998-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="bd998-903">Reprezentacja dziesiętna `I`</span><span class="sxs-lookup"><span data-stu-id="bd998-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="bd998-904">A następnie, Jeśli odpowiednia *Interpolacja* ma *constant_expression*, następuje `,` (przecinek), a następnie dziesiętną reprezentację wartości *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="bd998-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="bd998-905">Następnie *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* lub *interpolated_verbatim_string_end* natychmiast po odpowiedniej interpolacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="bd998-906">Kolejne argumenty są po prostu *wyrażeniami* z *interpolacji* (jeśli istnieją) w kolejności.</span><span class="sxs-lookup"><span data-stu-id="bd998-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="bd998-907">Do zrobienia: przykłady.</span><span class="sxs-lookup"><span data-stu-id="bd998-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="bd998-908">Proste nazwy</span><span class="sxs-lookup"><span data-stu-id="bd998-908">Simple names</span></span>

<span data-ttu-id="bd998-909">*Simple_name* składa się z identyfikatora, opcjonalnie, po którym następuje lista argumentów typu:</span><span class="sxs-lookup"><span data-stu-id="bd998-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="bd998-910">*Simple_name* ma postać `I` lub formularza `I<A1,...,Ak>`, gdzie `I` jest pojedynczym identyfikatorem, a `<A1,...,Ak>` jest opcjonalnym *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="bd998-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="bd998-911">Gdy nie określono *type_argument_list* , należy wziąć pod uwagę, że `K` ma wartość zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="bd998-912">*Simple_name* jest oceniane i sklasyfikowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="bd998-913">Jeśli `K` ma wartość zero, a *simple_name* pojawia się w *bloku* i jeśliobszar deklaracji zmiennej[lokalnej (lub](basic-concepts.md#declarations)otaczającego *bloku*) zawiera zmienną lokalną, parametr lub stałą z Nazwa @ no__t-6, a następnie *simple_name* odwołuje się do tej zmiennej lokalnej, parametru lub stałej i jest klasyfikowana jako zmienna lub wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="bd998-914">Jeśli `K` to zero i *simple_name* pojawia się w treści deklaracji metody ogólnej i jeśli ta deklaracja zawiera parametr typu o nazwie @ no__t-2, a następnie *simple_name* odwołuje się do tego parametru typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="bd998-915">W przeciwnym razie dla każdego typu wystąpienia @ no__t-0 ([Typ wystąpienia](classes.md#the-instance-type)), rozpoczynając od typu wystąpienia, bezpośrednio otaczającą deklarację typu i kontynuując typ wystąpienia każdej klasy lub deklaracji struktury (jeśli istnieje):</span><span class="sxs-lookup"><span data-stu-id="bd998-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="bd998-916">Jeśli `K` ma wartość zero, a deklaracja `T` zawiera parametr typu o nazwie @ no__t-2, wówczas *simple_name* odwołuje się do tego parametru typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="bd998-917">W przeciwnym razie, jeśli odnośnik elementu członkowskiego ([odnośnik elementów członkowskich](expressions.md#member-lookup)) elementu `I` w `T` z argumentami `K` @ no__t-4type da dopasowanie:</span><span class="sxs-lookup"><span data-stu-id="bd998-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="bd998-918">Jeśli `T` jest typem wystąpienia bezpośrednio otaczającej klasy lub typu struktury, a odnośnik identyfikuje jedną lub więcej metod, wynik jest grupą metod ze skojarzonym wyrażeniem wystąpienia o wartości `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="bd998-919">Jeśli określono listę argumentów typu, jest ona używana w wywołaniu metody ogólnej ([wywołania metody](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="bd998-920">W przeciwnym razie, jeśli `T` jest typem wystąpienia bezpośrednio otaczającej klasy lub typu struktury, jeśli wyszukiwanie identyfikuje element członkowski wystąpienia i jeśli odwołanie występuje w treści konstruktora wystąpienia, metody wystąpienia lub akcesora wystąpienia, wynik jest taka sama jak dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `this.I`.</span><span class="sxs-lookup"><span data-stu-id="bd998-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="bd998-921">Może się to zdarzyć tylko wtedy, gdy `K` wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="bd998-922">W przeciwnym razie wynik jest taki sam jak dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `T.I` lub `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="bd998-923">W tym przypadku jest to błąd czasu wiązania dla *simple_name* , aby odwołać się do elementu członkowskiego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="bd998-924">W przeciwnym razie dla każdej przestrzeni nazw @ no__t-0, rozpoczynając od przestrzeni nazw, w której występuje *simple_name* , kontynuując z każdą otaczającą przestrzeń nazw (jeśli istnieje) i kończącą się globalną przestrzenią nazw, następujące kroki są oceniane do momentu zlokalizowania jednostki:</span><span class="sxs-lookup"><span data-stu-id="bd998-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="bd998-925">Jeśli `K` to zero i `I` to nazwa przestrzeni nazw w @ no__t-2, a następnie:</span><span class="sxs-lookup"><span data-stu-id="bd998-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="bd998-926">Jeśli lokalizacja, w której występuje *simple_name* , jest ujęta w deklarację przestrzeni nazw dla `N`, a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , która kojarzy nazwę @ no__t-4 z Przestrzeń nazw lub typ, a następnie *simple_name* jest niejednoznaczna i wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="bd998-927">W przeciwnym razie *simple_name* odwołuje się do przestrzeni nazw o nazwie `I` w `N`.</span><span class="sxs-lookup"><span data-stu-id="bd998-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="bd998-928">W przeciwnym razie, jeśli `N` zawiera dostępny typ o nazwie @ no__t-1 i `K` @ no__t-3type parametry, wówczas:</span><span class="sxs-lookup"><span data-stu-id="bd998-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="bd998-929">Jeśli wartość `K` wynosi zero, a lokalizacja, w której występuje *simple_name* , jest ujęta w deklarację przestrzeni nazw dla `N`, a Deklaracja przestrzeni nazw zawiera element *extern_alias_directive* lub *using_alias_directive* , który kojarzy Nadaj nazwę @ no__t-5 z przestrzenią nazw lub typem, a następnie *simple_name* jest niejednoznaczny, a wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="bd998-930">W przeciwnym razie *namespace_or_type_name* odwołuje się do typu złożonego za pomocą podanych argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="bd998-931">W przeciwnym razie, jeśli lokalizacja, w której występuje *simple_name* , jest ujęta w deklarację przestrzeni nazw dla @ no__t-1:</span><span class="sxs-lookup"><span data-stu-id="bd998-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="bd998-932">Jeśli `K` to zero, a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , która kojarzy nazwę @ no__t-3 z zaimportowaną przestrzenią nazw lub typem, *simple_name* odwołuje się do tej przestrzeni nazw lub Wprowadź.</span><span class="sxs-lookup"><span data-stu-id="bd998-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="bd998-933">W przeciwnym razie, jeśli przestrzenie nazw i deklaracje typów zaimportowane przez *using_namespace_directive*s i *using_static_directive*z deklaracji przestrzeni nazw zawierają dokładnie jeden dostępny typ lub statyczny element członkowski, który ma nazwę @ no__ parametry t-2 i `K` @ no__t-4type, a następnie *simple_name* odwołują się do tego typu lub elementu członkowskiego skonstruowanego za pomocą podanych argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="bd998-934">W przeciwnym razie, jeśli przestrzenie nazw i typy zaimportowane przez *using_namespace_directive*s deklaracji przestrzeni nazw zawierają więcej niż jeden statyczny element członkowski typu lub nierozszerzającej metody o nazwie @ no__t-1 i `K` @ no__t-3type następnie *simple_name* jest niejednoznaczny i wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="bd998-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="bd998-935">Należy zauważyć, że cały krok jest ściśle równoległy do odpowiedniego kroku w przetwarzaniu *namespace_or_type_name* ([nazw i typów](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="bd998-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="bd998-936">W przeciwnym razie *simple_name* jest niezdefiniowany i wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="bd998-937">Wyrażenia w nawiasach</span><span class="sxs-lookup"><span data-stu-id="bd998-937">Parenthesized expressions</span></span>

<span data-ttu-id="bd998-938">*Parenthesized_expression* składa się z *wyrażenia* ujętego w nawiasy.</span><span class="sxs-lookup"><span data-stu-id="bd998-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="bd998-939">*Parenthesized_expression* jest oceniane przez obliczenie *wyrażenia* w nawiasach.</span><span class="sxs-lookup"><span data-stu-id="bd998-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="bd998-940">Jeśli *wyrażenie* w nawiasach oznacza przestrzeń nazw lub typ, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="bd998-941">W przeciwnym razie wynik *parenthesized_expression* jest wynikiem obliczenia zawartego *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="bd998-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="bd998-942">Dostęp do elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="bd998-942">Member access</span></span>

<span data-ttu-id="bd998-943">*Member_access* składa się z *primary_expression*, a *predefined_type*lub *qualified_alias_member*, po którym następuje token "`.`", po którym następuje *Identyfikator*, opcjonalnie po którym następuje *type_argument_list* .</span><span class="sxs-lookup"><span data-stu-id="bd998-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="bd998-944">Produkcja *qualified_alias_member* jest definiowana w [kwalifikatorach aliasów przestrzeni nazw](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="bd998-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="bd998-945">*Member_access* ma postać `E.I` lub formularza `E.I<A1, ..., Ak>`, gdzie `E` jest wyrażeniem podstawowym, `I` jest pojedynczym identyfikatorem i `<A1, ..., Ak>` jest opcjonalnym *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="bd998-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="bd998-946">Gdy nie określono *type_argument_list* , należy wziąć pod uwagę, że `K` ma wartość zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="bd998-947">*Member_access* z *primary_expression* typu `dynamic` jest dynamicznie powiązany ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-948">W takim przypadku kompilator klasyfikuje dostęp do elementu członkowskiego jako dostęp do właściwości typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="bd998-949">Poniższe reguły do określenia znaczenia *member_access* są następnie stosowane w czasie wykonywania przy użyciu typu czasu wykonywania, a nie typu czasu kompilacji *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="bd998-950">Jeśli ta Klasyfikacja czasu wykonywania prowadzi do grupy metod, dostęp do elementu członkowskiego musi być *primary_expression* elementu *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="bd998-951">*Member_access* jest oceniane i sklasyfikowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="bd998-952">Jeśli `K` ma wartość zero, a `E` jest przestrzenią nazw, a `E` zawiera zagnieżdżoną przestrzeń nazw o nazwie @ no__t-3, wówczas wynikiem jest ta przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="bd998-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="bd998-953">W przeciwnym razie, jeśli `E` jest przestrzenią nazw i `E` zawiera dostępny typ o nazwie @ no__t-2 i `K` @ no__t-4type parametry, wynik jest typem skonstruowanym z podanym argumentem typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="bd998-954">Jeśli `E` to *predefined_type* lub *primary_expression* sklasyfikowany jako typ, jeśli `E` nie jest parametrem typu, a jeśli odnośnik elementu członkowskiego ([odnośnik elementów członkowskich](expressions.md#member-lookup)) `I` w `E` z `K` @ no__t-8type, generuje dopasowanie, następnie `E.I` jest oceniane i sklasyfikowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="bd998-955">Jeśli `I` identyfikuje typ, wynik jest typem skonstruowanym z podanym argumentem typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="bd998-956">Jeśli `I` identyfikuje jedną lub więcej metod, wynik jest grupą metod bez skojarzonego wyrażenia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="bd998-957">Jeśli określono listę argumentów typu, jest ona używana w wywołaniu metody ogólnej ([wywołania metody](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="bd998-958">Jeśli `I` identyfikuje właściwość `static`, wynik jest dostęp do właściwości bez skojarzonego wyrażenia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="bd998-959">Jeśli `I` identyfikuje pole `static`:</span><span class="sxs-lookup"><span data-stu-id="bd998-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="bd998-960">Jeśli pole jest `readonly` i odwołanie występuje poza statycznym konstruktorem klasy lub struktury, w której jest zadeklarowane pole, wynik jest wartością, a mianowicie wartością pola statycznego @ no__t-1 w @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="bd998-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="bd998-961">W przeciwnym razie wynik jest zmienną, a mianowicie pole statyczne @ no__t-0 w @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="bd998-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="bd998-962">Jeśli `I` identyfikuje zdarzenie `static`:</span><span class="sxs-lookup"><span data-stu-id="bd998-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="bd998-963">Jeśli odwołanie występuje w klasie lub strukturze, w której zdarzenie jest zadeklarowane, a zdarzenie zostało zadeklarowane bez *event_accessor_declarations* ([Events](classes.md#events)), a następnie `E.I` jest przetwarzane dokładnie tak, jakby `I` było polem statycznym.</span><span class="sxs-lookup"><span data-stu-id="bd998-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="bd998-964">W przeciwnym razie wynik jest dostęp do zdarzenia bez skojarzonego wyrażenia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="bd998-965">Jeśli `I` identyfikuje stałą, wynik jest wartością, a mianowicie wartością tej stałej.</span><span class="sxs-lookup"><span data-stu-id="bd998-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="bd998-966">Jeśli `I` identyfikuje element członkowski wyliczenia, wynik jest wartością, a mianowicie wartością tego elementu członkowskiego wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="bd998-967">W przeciwnym razie `E.I` jest nieprawidłowym odwołaniem do elementu członkowskiego i wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="bd998-968">Jeśli `E` to dostęp do właściwości, indeksatora, zmienna lub wartość, typ, który jest @ no__t-1, i wyszukiwanie elementu członkowskiego ([odnośnik elementu członkowskiego](expressions.md#member-lookup)) `I` w `T` z `K` @ no__t-6type zwraca dopasowanie, a następnie `E.I` jest oceniane i sklasyfikowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="bd998-969">Po pierwsze, jeśli `E` jest dostępną właściwością lub indeksatorem, zostanie uzyskana wartość dostępu właściwości lub indeksatora ([wartości wyrażeń](expressions.md#values-of-expressions)) i `E` są ponownie klasyfikowane jako wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="bd998-970">Jeśli `I` identyfikuje jedną lub więcej metod, wynik jest grupą metod ze skojarzonym wyrażeniem wystąpienia o wartości `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="bd998-971">Jeśli określono listę argumentów typu, jest ona używana w wywołaniu metody ogólnej ([wywołania metody](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="bd998-972">Jeśli `I` identyfikuje Właściwość wystąpienia,</span><span class="sxs-lookup"><span data-stu-id="bd998-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="bd998-973">Jeśli `E` to `this`, `I` identyfikuje automatycznie implementowaną Właściwość ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) bez metody ustawiającej, a odwołanie występuje w konstruktorze wystąpienia dla klasy lub typu struktury `T`, a następnie wynik to zmienna, czyli ukryte pole zapasowe dla właściwości autoproperty podaną przez `I` w wystąpieniu `T` podanym przez `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="bd998-974">W przeciwnym razie wynik jest dostęp do właściwości ze skojarzonym wyrażeniem wystąpienia @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="bd998-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="bd998-975">Jeśli `T` to *class_type* i `I` identyfikuje pole wystąpienia tego *class_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="bd998-976">Jeśli wartość `E` jest `null`, zostanie zgłoszony `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="bd998-977">W przeciwnym razie, jeśli pole jest `readonly`, a odwołanie występuje poza konstruktorem wystąpienia klasy, w której jest zadeklarowane pole, wynik jest wartością, a mianowicie wartość pola @ no__t-1 w obiekcie, do którego odwołuje się @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="bd998-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="bd998-978">W przeciwnym razie wynik jest zmienną, a mianowicie pole @ no__t-0 w obiekcie, do którego odwołuje się @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="bd998-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="bd998-979">Jeśli `T` to *struct_type* i `I` identyfikuje pole wystąpienia tego *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="bd998-980">Jeśli `E` jest wartością lub jeśli pole jest `readonly`, a odwołanie występuje poza konstruktorem instancji struktury, w której jest zadeklarowane pole, wynik jest wartością, a mianowicie wartość pola @ no__t-2 w wystąpieniu struktury podanym przez parametr @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="bd998-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="bd998-981">W przeciwnym razie wynik jest zmienną, a mianowicie pole @ no__t-0 w wystąpieniu struktury podanym przez @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="bd998-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="bd998-982">Jeśli `I` identyfikuje zdarzenie wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="bd998-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="bd998-983">Jeśli odwołanie występuje w klasie lub strukturze, w której zdarzenie jest zadeklarowane, a zdarzenie zostało zadeklarowane bez *event_accessor_declarations* ([Events](classes.md#events)), a odwołanie nie występuje po lewej stronie operatora `+=` lub `-=` , a następnie `E.I` jest przetwarzana dokładnie tak, jakby `I` była polem wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="bd998-984">W przeciwnym razie wynikiem jest dostęp do zdarzenia ze skojarzonym wyrażeniem wystąpienia @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="bd998-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="bd998-985">W przeciwnym razie zostanie podjęta próba przetworzenia `E.I` jako wywołania metody rozszerzenia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="bd998-986">Jeśli to się nie powiedzie, `E.I` jest nieprawidłowym odwołaniem do składowej i wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="bd998-987">Identyczne nazwy proste i nazwy typów</span><span class="sxs-lookup"><span data-stu-id="bd998-987">Identical simple names and type names</span></span>

<span data-ttu-id="bd998-988">W celu uzyskania dostępu do elementu członkowskiego w formularzu `E.I`, jeśli `E` jest pojedynczym identyfikatorem, a znaczenie `E` jako *simple_name* ([Simple Names](expressions.md#simple-names)) to stała, pole, właściwość, zmienna lokalna lub parametr z tym samym typem co znaczenie dla `E` jako *TYPE_NAME* ([nazwa przestrzeni nazw i typów](basic-concepts.md#namespace-and-type-names)), a następnie oba możliwe znaczenia `E` są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="bd998-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="bd998-989">Dwa możliwe znaczenia `E.I` nigdy nie są niejednoznaczne, ponieważ `I` musi być elementem członkowskim typu `E` w obu przypadkach.</span><span class="sxs-lookup"><span data-stu-id="bd998-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="bd998-990">Innymi słowy, reguła po prostu zezwala na dostęp do statycznych elementów członkowskich i zagnieżdżonych typów `E`, w których wystąpił błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="bd998-991">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="bd998-992">Niejasności gramatyki</span><span class="sxs-lookup"><span data-stu-id="bd998-992">Grammar ambiguities</span></span>

<span data-ttu-id="bd998-993">Produkcje dla *simple_name* ([proste nazwy](expressions.md#simple-names)) i *member_access* ([dostęp do elementów członkowskich](expressions.md#member-access)) mogą dać podstawę niejasności w gramatyki dla wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="bd998-994">Na przykład, instrukcja:</span><span class="sxs-lookup"><span data-stu-id="bd998-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="bd998-995">może być interpretowany jako wywołanie `F` z dwoma argumentami, `G < A` i `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="bd998-996">Alternatywnie, można go interpretować jako wywołanie do `F` z jednym argumentem, który jest wywołaniem metody ogólnej @ no__t-1 z dwoma argumentami typu i jednym argumentem regularnym.</span><span class="sxs-lookup"><span data-stu-id="bd998-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="bd998-997">Jeśli sekwencja tokenów może być analizowana (w kontekście) jako *simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) lub *pointer_member_access* ([wskaźnik dostępu do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)) kończący się na *type_argument_ Lista* ([argumenty typu](types.md#type-arguments)), token bezpośrednio po zamykającym tokenie `>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="bd998-998">Jeśli jest to jedna z</span><span class="sxs-lookup"><span data-stu-id="bd998-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="bd998-999">następnie *type_argument_list* jest zachowywany jako część *simple_name*, *member_access* lub *pointer_member_access* i wszelkie inne możliwe przeanalizowanie sekwencji tokenów jest odrzucane.</span><span class="sxs-lookup"><span data-stu-id="bd998-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="bd998-1000">W przeciwnym razie *type_argument_list* nie jest uważany za część *simple_name*, *member_access* lub *pointer_member_access*, nawet jeśli nie istnieje żadna inna możliwa analiza sekwencji tokenów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="bd998-1001">Należy pamiętać, że te reguły nie są stosowane podczas analizowania elementu *type_argument_list* w *namespace_or_type_name* ([nazwy przestrzeni nazw i typów](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="bd998-1002">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="bd998-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="bd998-1003">zgodnie z tą regułą, należy interpretować jako wywołanie `F` z jednym argumentem, który jest wywołaniem metody generycznej `G` z dwoma argumentami typu i jednym argumentem regularnym.</span><span class="sxs-lookup"><span data-stu-id="bd998-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="bd998-1004">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="bd998-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="bd998-1005">Każdy z nich będzie interpretowany jako wywołanie `F` z dwoma argumentami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="bd998-1006">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="bd998-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="bd998-1007">będzie interpretowany jako operator mniejszości, operator większości, i jednoargumentowy operator plus, tak jakby instrukcja była zapisywana `x = (F < A) > (+y)`, a nie jako *simple_name* z *type_argument_list* , po którym następuje operator binarny Plus.</span><span class="sxs-lookup"><span data-stu-id="bd998-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="bd998-1008">W instrukcji</span><span class="sxs-lookup"><span data-stu-id="bd998-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="bd998-1009">tokeny `C<T>` są interpretowane jako *namespace_or_type_name* z *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="bd998-1010">Wyrażenia wywołania</span><span class="sxs-lookup"><span data-stu-id="bd998-1010">Invocation expressions</span></span>

<span data-ttu-id="bd998-1011">Element *invocation_expression* jest używany do wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="bd998-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="bd998-1012">*Invocation_expression* jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), jeśli co najmniej jeden z następujących posiada:</span><span class="sxs-lookup"><span data-stu-id="bd998-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="bd998-1013">*Primary_expression* ma typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="bd998-1014">Co najmniej jeden argument opcjonalnej *argument_list* ma typ czasu kompilacji `dynamic`, a *primary_expression* nie ma typu delegata.</span><span class="sxs-lookup"><span data-stu-id="bd998-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="bd998-1015">W takim przypadku kompilator klasyfikuje *invocation_expression* jako wartość typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="bd998-1016">Poniższe reguły do określenia znaczenia *invocation_expression* są następnie stosowane w czasie wykonywania, przy użyciu typu run-time zamiast typu czasu kompilacji dla *primary_expression* i argumentów, które mają typ czasu kompilacji @no_ _T-2.</span><span class="sxs-lookup"><span data-stu-id="bd998-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="bd998-1017">Jeśli *primary_expression* nie ma typu czasu kompilacji `dynamic`, wówczas wywołanie metody przejdzie do ograniczonego sprawdzenia czasu kompilacji zgodnie z opisem w [sprawdzeniu czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="bd998-1018">*Primary_expression* elementu *invocation_expression* musi być grupą metod lub wartością *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="bd998-1019">Jeśli *primary_expression* jest grupą metod, *invocation_expression* jest wywołaniem metody ([wywołania metody](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="bd998-1020">Jeśli *primary_expression* jest wartością *delegate_type*, *invocation_expression* jest wywołaniem delegata ([Delegaty wywołań](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="bd998-1021">Jeśli *primary_expression* nie jest grupą metod ani wartością *delegate_type*, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-1022">Opcjonalne *argument_list* ([listy argumentów](expressions.md#argument-lists)) zawierają wartości lub odwołania do zmiennych dla parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="bd998-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="bd998-1023">Wynik oceny *invocation_expression* jest klasyfikowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="bd998-1024">Jeśli *invocation_expression* wywołuje metodę lub delegata, który zwraca `void`, wynik nie ma wartości Nothing.</span><span class="sxs-lookup"><span data-stu-id="bd998-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="bd998-1025">Wyrażenie, które jest sklasyfikowane jako Nothing, jest dozwolone tylko w kontekście *statement_expression* ([instrukcji wyrażenia](statements.md#expression-statements)) lub jako treść *lambda_expression* ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="bd998-1026">W przeciwnym razie wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="bd998-1027">W przeciwnym razie wynik jest wartością typu zwracanego przez metodę lub delegat.</span><span class="sxs-lookup"><span data-stu-id="bd998-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="bd998-1028">Wywołania metody</span><span class="sxs-lookup"><span data-stu-id="bd998-1028">Method invocations</span></span>

<span data-ttu-id="bd998-1029">W przypadku wywołania metody *primary_expression* elementu *invocation_expression* musi być grupą metod.</span><span class="sxs-lookup"><span data-stu-id="bd998-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="bd998-1030">Grupa metod identyfikuje jedną metodę do wywołania lub zestaw przeciążonych metod, z których można wybrać konkretną metodę do wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="bd998-1031">W tym ostatnim przypadku określenie konkretnej metody do wywołania jest zależne od kontekstu dostarczonego przez typy argumentów w *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="bd998-1032">Przetwarzanie powiązań metody w wyniku wywołania metod `M(A)`, gdzie `M` jest grupą metod (może to dotyczyć *type_argument_list*), a `A` jest opcjonalny *argument_list*, składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-1033">Zestaw metod kandydujących dla wywołania metody jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="bd998-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="bd998-1034">Dla każdej metody `F` skojarzonej z grupą metod `M`:</span><span class="sxs-lookup"><span data-stu-id="bd998-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="bd998-1035">Jeśli `F` jest nieogólny, `F` jest kandydatem, gdy:</span><span class="sxs-lookup"><span data-stu-id="bd998-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="bd998-1036">`M` nie ma listy argumentów typu i</span><span class="sxs-lookup"><span data-stu-id="bd998-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="bd998-1037">`F` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="bd998-1038">Jeśli `F` jest rodzajowy i `M` nie ma listy argumentów typu, `F` jest kandydatem, gdy:</span><span class="sxs-lookup"><span data-stu-id="bd998-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="bd998-1039">Wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) kończy się powodzeniem, wnioskowanie listy argumentów typu dla wywołania i</span><span class="sxs-lookup"><span data-stu-id="bd998-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="bd998-1040">Gdy argumenty typu wywnioskowane zostaną zastąpione odpowiednimi parametrami typu metody, wszystkie skompilowane typy na liście parametrów F spełniają ich ograniczenia ([spełniające warunki ograniczające](types.md#satisfying-constraints)), a lista parametrów `F` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="bd998-1041">Jeśli `F` jest rodzajowy i `M` zawiera listę argumentów typu, `F` jest kandydatem, gdy:</span><span class="sxs-lookup"><span data-stu-id="bd998-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="bd998-1042">`F` ma taką samą liczbę parametrów typu metody jak podano na liście argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="bd998-1043">Gdy argumenty typu są zastępowane odpowiednimi parametrami typu metody, wszystkie skompilowane typy na liście parametrów F spełniają ich ograniczenia ([spełniające warunki ograniczające](types.md#satisfying-constraints)), a lista parametrów `F` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="bd998-1044">Zestaw metod kandydujących jest zredukowany, aby zawierał tylko metody z najbardziej pochodnych typów: Dla każdej metody `C.F` w zestawie, gdzie `C` jest typem, w którym Metoda `F` jest zadeklarowana, wszystkie metody zadeklarowane w typie podstawowym `C` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="bd998-1045">Ponadto jeśli `C` jest typem klasy innym niż `object`, wszystkie metody zadeklarowane w typie interfejsu są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="bd998-1046">(Ta ostatnia reguła ma wpływ tylko wtedy, gdy grupa metod była wynikiem przeszukiwania elementu członkowskiego w parametrze typu mającym obowiązującą klasę bazową inną niż obiekt i niepusty zestaw obowiązujących interfejsów).</span><span class="sxs-lookup"><span data-stu-id="bd998-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="bd998-1047">Jeśli wynikający z tego zestaw metod kandydujących jest pusty, dalsze przetwarzanie w poniższych krokach zostanie porzucone, a zamiast tego zostanie podjęta próba przetworzenia wywołania jako wywołania metody rozszerzenia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="bd998-1048">Jeśli to się nie powiedzie, żadne odpowiednie metody nie istnieją i wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="bd998-1049">Najlepsza Metoda zestawu metod kandydujących jest identyfikowana przy użyciu [reguł rozpoznawania przeciążenia.](expressions.md#overload-resolution)</span><span class="sxs-lookup"><span data-stu-id="bd998-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="bd998-1050">Jeśli nie można zidentyfikować pojedynczej metody, wywołanie metody jest niejednoznaczne i występuje błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="bd998-1051">Podczas rozpoznawania przeciążenia parametry metody ogólnej są brane pod uwagę po podstawianiu argumentów typu (dostarczonych lub wywnioskowanych) dla odpowiednich parametrów typu metody.</span><span class="sxs-lookup"><span data-stu-id="bd998-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="bd998-1052">Zostanie wykonana ostateczna weryfikacja wybranej najlepszej metody:</span><span class="sxs-lookup"><span data-stu-id="bd998-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="bd998-1053">Metoda jest weryfikowana w kontekście grupy metod: Jeśli najlepsza metoda jest metodą statyczną, Grupa metod musi mieć wynik *simple_name* lub *member_access* za pośrednictwem typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="bd998-1054">Jeśli najlepsza metoda jest metodą wystąpienia, Grupa metod musi mieć wynik z *simple_name*, *member_access* za pośrednictwem zmiennej lub wartości lub *base_access*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="bd998-1055">Jeśli żaden z tych wymagań nie ma wartości true, wystąpi błąd w czasie trwania powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="bd998-1056">Jeśli najlepsza metoda jest metodą rodzajową, argumenty typu (dostarczone lub wywnioskowane) są sprawdzane względem ograniczeń ([spełnianie ograniczeń](types.md#satisfying-constraints)) zadeklarowanych dla metody ogólnej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="bd998-1057">Jeśli którykolwiek z argumentów typu nie spełnia odpowiednich ograniczeń w parametrze typu, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-1058">Po wybraniu metody i sprawdzeniu jej pod kątem czasu wiązania przez powyższe kroki, rzeczywiste wywołanie w czasie wykonywania jest przetwarzane zgodnie z regułami wywołania elementu członkowskiego, które opisano w [sprawdzeniu czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="bd998-1059">Intuicyjny efekt opisanych powyżej reguł rozpoznawania jest następujący: Aby zlokalizować konkretną metodę wywoływaną przez wywołanie metody, Zacznij od typu wskazywanego przez wywołanie metody i Kontynuuj łańcuch dziedziczenia do momentu, gdy zostanie znaleziona co najmniej jedna odpowiednia, dostępna, niezastępująca Deklaracja metody.</span><span class="sxs-lookup"><span data-stu-id="bd998-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="bd998-1060">Następnie należy wykonać wnioskowanie o typie i metodę rozpoznawania przeciążenia dla zestawu odpowiednich, dostępnych, zastąpień metod zadeklarowanych w tym typie i wywoływać metody w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="bd998-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="bd998-1061">Jeśli żadna metoda nie została znaleziona, spróbuj zamiast tego przetworzyć wywołanie jako wywołanie metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="bd998-1062">Wywołania metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="bd998-1062">Extension method invocations</span></span>

<span data-ttu-id="bd998-1063">W wywołaniu metody ([wywołania w zapakowanych wystąpieniach](expressions.md#invocations-on-boxed-instances)) jednego z formularzy</span><span class="sxs-lookup"><span data-stu-id="bd998-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="bd998-1064">Jeśli normalne przetwarzanie wywołania nie odnajdzie odpowiednich metod, podejmowana jest próba przetworzenia konstrukcji jako wywołania metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="bd998-1065">Jeśli *wyrażenie* lub którykolwiek z *argumentów* ma typ czasu kompilacji `dynamic`, metody rozszerzające nie będą miały zastosowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="bd998-1066">Celem jest znalezienie najlepszego *type_name* `C`, aby można było wykonać odpowiednie wywołanie metody statycznej:</span><span class="sxs-lookup"><span data-stu-id="bd998-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="bd998-1067">Metoda rozszerzająca `Ci.Mj` ***kwalifikuje się*** , jeśli:</span><span class="sxs-lookup"><span data-stu-id="bd998-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="bd998-1068">`Ci` jest klasą nieogólną, niezagnieżdżoną</span><span class="sxs-lookup"><span data-stu-id="bd998-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="bd998-1069">Nazwa `Mj` jest *identyfikatorem*</span><span class="sxs-lookup"><span data-stu-id="bd998-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="bd998-1070">`Mj` jest dostępne i stosowane w przypadku zastosowania do argumentów jako metody statycznej, jak pokazano powyżej</span><span class="sxs-lookup"><span data-stu-id="bd998-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="bd998-1071">W wyszukiwaniu istnieje niejawna tożsamość, odwołanie lub opakowanie z *wyrażenia* do typu pierwszego parametru `Mj`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="bd998-1072">Wyszukiwanie `C` przebiega w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="bd998-1073">Począwszy od najbliższej deklaracji przestrzeni nazw, kontynuując każdą zachodzącą deklarację przestrzeni nazw i kończącą się jednostką kompilacji, kolejne próby są wykonywane w celu znalezienia kandydującego zestawu metod rozszerzających:</span><span class="sxs-lookup"><span data-stu-id="bd998-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="bd998-1074">Jeśli dana przestrzeń nazw lub jednostka kompilacji bezpośrednio zawiera deklaracje typu nieogólnego `Ci` z uprawnionymi metodami rozszerzenia `Mj`, wówczas zestaw tych metod rozszerzających jest zestaw kandydatów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="bd998-1075">Jeśli typy `Ci` zaimportowane przez *using_static_declarations* i bezpośrednio zadeklarowane w przestrzeniach nazw zaimportowanych przez *using_namespace_directive*s w danej przestrzeni nazw lub jednostce kompilacji, zawierają bezpośrednio odpowiednie metody rozszerzenia `Mj`, następnie zestaw tych metod rozszerzających jest zestawem kandydatów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="bd998-1076">Jeśli w żadnej deklaracji przestrzeni nazw lub jednostce kompilacji nie zostanie znaleziony zestaw kandydatów, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="bd998-1077">W przeciwnym razie Rozpoznanie przeciążenia jest stosowane do zestawu kandydatów zgodnie z opisem w ([rozpoznawanie przeciążenia](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="bd998-1078">Jeśli nie zostanie znaleziona żadna Najlepsza Metoda, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="bd998-1079">`C` jest typem, w ramach którego Najlepsza Metoda jest zadeklarowana jako Metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="bd998-1080">Przy użyciu `C` jako elementu docelowego wywołanie metody jest następnie przetwarzane jako wywołanie metody statycznej (sprawdzanie w[czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="bd998-1081">Powyższe zasady oznaczają, że metody wystąpień mają pierwszeństwo przed metodami rozszerzania, te metody rozszerzające dostępne w deklaracji wewnętrznej przestrzeni nazw mają pierwszeństwo przed metodami rozszerzenia dostępnymi w deklaracjach zewnętrznych przestrzeni nazw i tym rozszerzeniu Metody zadeklarowane bezpośrednio w przestrzeni nazw mają pierwszeństwo przed metodami rozszerzenia importowanymi do tej samej przestrzeni nazw za pomocą dyrektywy using Namespace.</span><span class="sxs-lookup"><span data-stu-id="bd998-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="bd998-1082">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="bd998-1083">W przykładzie metoda `B` ma pierwszeństwo przed pierwszą metodą rozszerzenia, a metoda `C` ma pierwszeństwo przed obiema metodami rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="bd998-1084">Dane wyjściowe tego przykładu to:</span><span class="sxs-lookup"><span data-stu-id="bd998-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="bd998-1085">`D.G` ma pierwszeństwo przed `C.G`, a `E.F` ma pierwszeństwo przed `D.F` i `C.F`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="bd998-1086">Delegowanie wywołań</span><span class="sxs-lookup"><span data-stu-id="bd998-1086">Delegate invocations</span></span>

<span data-ttu-id="bd998-1087">Dla wywołania delegata *primary_expression* *invocation_expression* musi być wartością typu *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="bd998-1088">Co więcej, biorąc pod uwagę, że *delegate_type* być elementem członkowskim funkcji z taką samą listą parametrów jak *delegate_type*, *Delegate_type* musi mieć zastosowanie ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)) w odniesieniu do *argument_ Lista* *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="bd998-1089">Przetwarzanie w czasie wykonywania delegata wywołania formularza `D(A)`, gdzie `D` jest *primary_expressionem* *delegate_type* i `A` jest opcjonalny *argument_list*, składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-1090">`D` jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1090">`D` is evaluated.</span></span> <span data-ttu-id="bd998-1091">Jeśli ta Ocena spowoduje wyjątek, nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1092">Wartość `D` jest sprawdzana jako prawidłowa.</span><span class="sxs-lookup"><span data-stu-id="bd998-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="bd998-1093">Jeśli wartość `D` jest `null`, zgłaszany jest `System.NullReferenceException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1094">W przeciwnym razie `D` jest odwołaniem do wystąpienia delegata.</span><span class="sxs-lookup"><span data-stu-id="bd998-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="bd998-1095">Wywołania elementu członkowskiego funkcji ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) są wykonywane na każdej z wywoływanych jednostek na liście wywołań delegata.</span><span class="sxs-lookup"><span data-stu-id="bd998-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="bd998-1096">W przypadku wywoływanych jednostek składających się z wystąpienia i metody wystąpienia wystąpienie dla wywołania jest wystąpieniem zawartym w jednostce, którą można wywołać.</span><span class="sxs-lookup"><span data-stu-id="bd998-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="bd998-1097">Dostęp do elementu</span><span class="sxs-lookup"><span data-stu-id="bd998-1097">Element access</span></span>

<span data-ttu-id="bd998-1098">*Element_access* składa się z *primary_no_array_creation_expression*, po którym następuje token "`[`", po którym następuje *argument_list*, a następnie token "`]`".</span><span class="sxs-lookup"><span data-stu-id="bd998-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="bd998-1099">*Argument_list* składa się z jednego lub więcej *argumentów*s, rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="bd998-1100">*Argument_list* *element_access* nie może zawierać argumentów `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="bd998-1101">*Element_access* jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), jeśli co najmniej jeden z następujących posiada:</span><span class="sxs-lookup"><span data-stu-id="bd998-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="bd998-1102">*Primary_no_array_creation_expression* ma typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="bd998-1103">Co najmniej jedno wyrażenie *argument_list* ma typ czasu kompilacji `dynamic`, a *primary_no_array_creation_expression* nie ma typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="bd998-1104">W takim przypadku kompilator klasyfikuje *element_access* jako wartość typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="bd998-1105">Poniższe reguły do określenia znaczenia *element_access* są następnie stosowane w czasie wykonywania przy użyciu typu czasu wykonywania, a nie typu czasu kompilacji dla *primary_no_array_creation_expression* i *argument_list* wyrażenia, które mają typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="bd998-1106">Jeśli *primary_no_array_creation_expression* nie ma typu czasu kompilacji `dynamic`, wówczas dostęp do elementu jest niezależny od ograniczonego sprawdzenia czasu kompilacji zgodnie z opisem w [sprawdzaniu poprawności dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="bd998-1107">Jeśli *primary_no_array_creation_expression* *element_access* jest wartością *array_type*, *element_access* jest dostęp do tablicy ([dostęp do tablicy](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="bd998-1108">W przeciwnym razie *primary_no_array_creation_expression* musi być zmienną lub wartością typu klasy, struktury lub interfejsu, który ma co najmniej jeden element członkowski indeksatora, w tym przypadku *element_access* jest dostępem indeksatora ([dostęp do indeksatora](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="bd998-1109">Dostęp do tablicy</span><span class="sxs-lookup"><span data-stu-id="bd998-1109">Array access</span></span>

<span data-ttu-id="bd998-1110">W przypadku dostępu do tablicy *primary_no_array_creation_expression* *element_access* musi być wartością *array_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="bd998-1111">Ponadto *argument_list* dostępu do tablicy nie może zawierać nazwanych argumentów. Liczba wyrażeń w *argument_list* musi być taka sama jak ranga *array_type*, a każde wyrażenie musi być typu `int`, `uint`, `long`, `ulong` lub musi być niejawnie konwertowane na co najmniej jeden z tych typów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="bd998-1112">Wynikiem oceny dostępu do tablicy jest zmienna typu elementu tablicy, a mianowicie element tablicy wybrany przez wartości w wyrażeniach w *argument_list*. 1}.</span><span class="sxs-lookup"><span data-stu-id="bd998-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="bd998-1113">Przetwarzanie dostępu tablicy w czasie wykonywania `P[A]`, gdzie `P` jest *primary_no_array_creation_expression* *array_type* i `A` jest *argument_list*, składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-1114">`P` jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1114">`P` is evaluated.</span></span> <span data-ttu-id="bd998-1115">Jeśli ta Ocena spowoduje wyjątek, nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1116">Wyrażenia indeksu *argument_list* są oceniane w kolejności, od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="bd998-1117">Po dokonaniu oceny każdego wyrażenia indeksu niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) do jednego z następujących typów jest wykonywana: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="bd998-1118">Pierwszy typ na tej liście, dla którego istnieje niejawna konwersja, jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="bd998-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="bd998-1119">Na przykład jeśli wyrażenie index jest typu `short`, to niejawna konwersja na `int` jest wykonywana, ponieważ niejawne konwersje z `short` do `int` i od `short` do `long` są możliwe.</span><span class="sxs-lookup"><span data-stu-id="bd998-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="bd998-1120">Jeśli Obliczanie wyrażenia indeksu lub kolejnej niejawnej konwersji powoduje wyjątek, nie są oceniane żadne dalsze wyrażenia indeksu i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1121">Wartość `P` jest sprawdzana jako prawidłowa.</span><span class="sxs-lookup"><span data-stu-id="bd998-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="bd998-1122">Jeśli wartość `P` jest `null`, zgłaszany jest `System.NullReferenceException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1123">Wartość każdego wyrażenia w *argument_list* jest sprawdzana względem rzeczywistych granic każdego wymiaru wystąpienia tablicy, do którego odwołuje się `P`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="bd998-1124">Jeśli co najmniej jedna wartość jest poza zakresem, zgłaszany jest `System.IndexOutOfRangeException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1125">Obliczono lokalizację elementu tablicy podaną przez wyrażenia indeksu, a ta lokalizacja jest wynikiem dostępu do tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="bd998-1126">Dostęp indeksatora</span><span class="sxs-lookup"><span data-stu-id="bd998-1126">Indexer access</span></span>

<span data-ttu-id="bd998-1127">W przypadku dostępu indeksatora *primary_no_array_creation_expression* elementu *element_access* musi być zmienną lub wartością typu klasy, struktury lub interfejsu, a ten typ musi implementować jeden lub więcej indeksatorów, które są stosowane w odniesieniu do *argument_list* *element_access*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="bd998-1128">Przetwarzanie w czasie wiązania dostępu indeksatora do formularza `P[A]`, gdzie `P` jest *primary_no_array_creation_expressionem* klasy, struktury lub typu interfejsu `T`, a `A` jest *argument_list*, składa się z następujących elementów: czynnooci</span><span class="sxs-lookup"><span data-stu-id="bd998-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-1129">Zestaw indeksatorów udostępnianych przez `T` jest skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="bd998-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="bd998-1130">Zestaw składa się z wszystkich indeksatorów zadeklarowanych w `T` lub typ podstawowy `T`, które nie są `override` deklaracji i są dostępne w bieżącym kontekście ([dostęp do elementów członkowskich](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="bd998-1131">Zestaw jest zredukowany do tych indeksatorów, które mają zastosowanie i nie są ukryte przez inne indeksatory.</span><span class="sxs-lookup"><span data-stu-id="bd998-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="bd998-1132">Poniższe reguły są stosowane do każdego indeksatora `S.I` w zestawie, gdzie `S` jest typem, w którym zadeklarowano indeksator `I`:</span><span class="sxs-lookup"><span data-stu-id="bd998-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="bd998-1133">Jeśli `I` nie ma zastosowania w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)), wówczas `I` zostanie usunięty z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="bd998-1134">Jeśli `I` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)), wówczas wszystkie indeksatory zadeklarowane w typie podstawowym `S` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="bd998-1135">Jeśli `I` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)) i `S` jest typem klasy innym niż `object`, wszystkie indeksatory zadeklarowane w interfejsie są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="bd998-1136">Jeśli zestaw wyników indeksatorów jest pusty, nie ma żadnych odpowiednich indeksatorów i wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="bd998-1137">Najlepszy indeksator zestawu indeksatorów kandydujących jest identyfikowany przy użyciu [reguł rozpoznawania przeciążenia.](expressions.md#overload-resolution)</span><span class="sxs-lookup"><span data-stu-id="bd998-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="bd998-1138">Jeśli nie można zidentyfikować pojedynczego najlepszego indeksatora, dostęp indeksatora jest niejednoznaczny i wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="bd998-1139">Wyrażenia indeksu *argument_list* są oceniane w kolejności, od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="bd998-1140">Wynikiem przetwarzania dostępu indeksatora jest wyrażenie sklasyfikowane jako dostęp indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="bd998-1141">Wyrażenie dostępu indeksatora odwołuje się do indeksatora określonego w powyższym kroku i ma skojarzone wyrażenie wystąpienia o wartości `P` i skojarzonej z nim listy argumentów `A`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="bd998-1142">W zależności od kontekstu, w którym jest używany, dostęp indeksatora powoduje wywołanie *metody dostępu get* lub *set metody dostępu* indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="bd998-1143">Jeśli dostęp indeksatora jest elementem docelowym przypisania, *metoda dostępu set* jest wywoływana, aby przypisać nową wartość ([przypisanie proste](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="bd998-1144">We wszystkich innych przypadkach *metoda dostępu get* jest wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="bd998-1145">Ten dostęp</span><span class="sxs-lookup"><span data-stu-id="bd998-1145">This access</span></span>

<span data-ttu-id="bd998-1146">*This_access* składa się z zastrzeżonego słowa `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="bd998-1147">*This_access* jest dozwolony tylko w *bloku* konstruktora wystąpienia, metody wystąpienia lub akcesora wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="bd998-1148">Ma jedną z następujących znaczenia:</span><span class="sxs-lookup"><span data-stu-id="bd998-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="bd998-1149">Gdy `this` jest używany w *primary_expression* w konstruktorze wystąpienia klasy, jest sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="bd998-1150">Typ wartości to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) klasy, w której występuje użycie, a wartość to odwołanie do konstruowanego obiektu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="bd998-1151">Gdy `this` jest używany w *primary_expression* w metodzie wystąpienia lub akcesorze wystąpienia klasy, jest on sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="bd998-1152">Typ wartości to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) klasy, w której występuje użycie, a wartość to odwołanie do obiektu, dla którego wywołana została metoda lub akcesor.</span><span class="sxs-lookup"><span data-stu-id="bd998-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="bd998-1153">Gdy `this` jest używany w *primary_expression* w konstruktorze wystąpienia struktury, jest on klasyfikowany jako zmienna.</span><span class="sxs-lookup"><span data-stu-id="bd998-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="bd998-1154">Typ zmiennej to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) struktury, w której występuje użycie, a zmienna reprezentuje strukturę konstruowaną.</span><span class="sxs-lookup"><span data-stu-id="bd998-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="bd998-1155">Zmienna `this` konstruktora wystąpienia struktury zachowuje się dokładnie tak samo jak parametr `out` typu struktury — w szczególności oznacza to, że zmienna musi być ostatecznie przypisana w każdej ścieżce wykonywania konstruktora wystąpień.</span><span class="sxs-lookup"><span data-stu-id="bd998-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="bd998-1156">Gdy `this` jest używany w *primary_expression* w metodzie wystąpienia lub metodach dostępu do wystąpienia struktury, jest on klasyfikowany jako zmienna.</span><span class="sxs-lookup"><span data-stu-id="bd998-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="bd998-1157">Typ zmiennej to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) struktury, w której występuje użycie.</span><span class="sxs-lookup"><span data-stu-id="bd998-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="bd998-1158">Jeśli metoda lub akcesor nie jest iteratorem ([iteratorów](classes.md#iterators)), zmienna `this` reprezentuje strukturę, dla której wywołano metodę lub akcesor, i zachowuje się dokładnie tak samo jak parametr `ref` typu struktury.</span><span class="sxs-lookup"><span data-stu-id="bd998-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="bd998-1159">Jeśli metoda lub akcesor jest iteratorem, zmienna `this` reprezentuje kopię struktury, dla której wywołano metodę lub akcesor, i zachowuje się dokładnie tak samo jak parametr wartości typu struktury.</span><span class="sxs-lookup"><span data-stu-id="bd998-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="bd998-1160">Użycie `this` w *primary_expression* w kontekście innym niż wymienione powyżej jest błędem czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="bd998-1161">W szczególności nie można odwołać się do `this` w metodzie statycznej, metodzie dostępu do właściwości statycznych lub w *variable_initializer* deklaracji pola.</span><span class="sxs-lookup"><span data-stu-id="bd998-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="bd998-1162">Dostęp podstawowy</span><span class="sxs-lookup"><span data-stu-id="bd998-1162">Base access</span></span>

<span data-ttu-id="bd998-1163">*Base_access* składa się z słowa zastrzeżonego `base`, po którym następuje token "`.`" i identyfikator lub *argument_list* ujęty w nawiasy kwadratowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="bd998-1164">*Base_access* służy do uzyskiwania dostępu do składowych klasy bazowej, które są ukryte przez podobne nazwy członków w bieżącej klasie lub strukturze.</span><span class="sxs-lookup"><span data-stu-id="bd998-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="bd998-1165">*Base_access* jest dozwolony tylko w *bloku* konstruktora wystąpienia, metody wystąpienia lub akcesora wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="bd998-1166">Gdy `base.I` występuje w klasie lub strukturze, `I` musi zwrócić element członkowski klasy bazowej tej klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="bd998-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="bd998-1167">Podobnie, gdy `base[E]` występuje w klasie, odpowiedni indeksator musi istnieć w klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="bd998-1168">W czasie wiązania *base_access* wyrażenia w postaci `base.I` i `base[E]` są oceniane dokładnie tak, jakby były zapisywane `((B)this).I` i `((B)this)[E]`, gdzie `B` jest klasą bazową klasy lub struktury, w której występuje konstrukcja.</span><span class="sxs-lookup"><span data-stu-id="bd998-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="bd998-1169">W ten sposób `base.I` i `base[E]` odpowiada `this.I` i `this[E]`, z wyjątkiem tego, że `this` jest wyświetlana jako wystąpienie klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="bd998-1170">Gdy *base_access* odwołuje się do elementu członkowskiego funkcji wirtualnej (metody, właściwości lub indeksatora), oznacza to, że element członkowski funkcji, który ma zostać wywołany w czasie wykonywania ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jest zmieniany.</span><span class="sxs-lookup"><span data-stu-id="bd998-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="bd998-1171">Wywoływany element członkowski funkcji jest określany przez znalezienie najbardziej pochodnej implementacji ([metod wirtualnych](classes.md#virtual-methods)) elementu członkowskiego funkcji w odniesieniu do `B` (zamiast w odniesieniu do typu czasu wykonywania `this`, tak jak zwykle w przypadku dostępu innego niż podstawowy) .</span><span class="sxs-lookup"><span data-stu-id="bd998-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="bd998-1172">W tym celu w `override` elementu członkowskiego funkcji `virtual` *base_access* może służyć do wywołania dziedziczonej implementacji elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="bd998-1173">Jeśli element członkowski funkcji, do którego odwołuje się *base_access* , jest abstrakcyjny, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="bd998-1174">Operatory przyrostka i zmniejszenie</span><span class="sxs-lookup"><span data-stu-id="bd998-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="bd998-1175">Operand przyrostkowej operacji zwiększania lub zmniejszania musi być wyrażeniem sklasyfikowanym jako zmienna, dostęp do właściwości lub dostęp indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="bd998-1176">Wynik operacji jest wartością tego samego typu co argument operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="bd998-1177">Jeśli *primary_expression* ma typ czasu kompilacji `dynamic`, a następnie operator jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), *post_increment_expression* lub *post_decrement_expression* ma typ czasu kompilacji `dynamic` a w czasie wykonywania są stosowane następujące reguły z typem czasu wykonywania *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="bd998-1178">Jeśli operand przyrostowej operacji zwiększania lub zmniejszania jest dostępną właściwością lub indeksatorem, właściwość lub indeksator musi mieć metodę dostępu `get` i `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="bd998-1179">W przeciwnym razie wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-1180">Metoda rozpoznawania przeciążenia operatora jednoargumentowego (Metoda[rozpoznawania przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) została zastosowana w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1181">Wstępnie zdefiniowane operatory `++` i `--` są dostępne dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 i dowolnego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="bd998-1182">Wstępnie zdefiniowane operatory `++` zwracają wartość wygenerowaną przez dodanie 1 do operandu, a wstępnie zdefiniowane operatory `--` zwracają wartość wygenerowaną przez odjęcie 1 od operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="bd998-1183">W kontekście `checked`, jeśli wynik tego dodawania lub odejmowania znajduje się poza zakresem typu wyników, a typ wyniku jest typem całkowitym lub typem wyliczeniowym, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="bd998-1184">Przetwarzanie w czasie wykonywania przyrostkowej przyrostowej lub zmniejszania operacji w postaci `x++` lub `x--` składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="bd998-1185">Jeśli `x` jest klasyfikowane jako zmienna:</span><span class="sxs-lookup"><span data-stu-id="bd998-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="bd998-1186">`x` jest oceniane, aby utworzyć zmienną.</span><span class="sxs-lookup"><span data-stu-id="bd998-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="bd998-1187">Wartość `x` jest zapisywana.</span><span class="sxs-lookup"><span data-stu-id="bd998-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="bd998-1188">Wybrany operator jest wywoływany z zapisaną wartością `x` jako argument.</span><span class="sxs-lookup"><span data-stu-id="bd998-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="bd998-1189">Wartość zwrócona przez operator jest przechowywana w lokalizacji podawanej przez oszacowanie `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="bd998-1190">Zapisana wartość `x` jest wynikiem operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="bd998-1191">Jeśli `x` jest sklasyfikowany jako dostęp do właściwości lub indeksatora:</span><span class="sxs-lookup"><span data-stu-id="bd998-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="bd998-1192">Wyrażenie wystąpienia (jeśli `x` nie jest `static`) i lista argumentów (jeśli `x` jest dostępem indeksatora) skojarzonym z `x` są oceniane, a wyniki są używane w kolejnych wywołaniach metody dostępu `get` i `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="bd998-1193">Metoda dostępu `get` elementu `x` jest wywoływana, a zwrócona wartość jest zapisywana.</span><span class="sxs-lookup"><span data-stu-id="bd998-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="bd998-1194">Wybrany operator jest wywoływany z zapisaną wartością `x` jako argument.</span><span class="sxs-lookup"><span data-stu-id="bd998-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="bd998-1195">Metoda dostępu `set` elementu `x` jest wywoływana z wartością zwracaną przez operator jako argument `value`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="bd998-1196">Zapisana wartość `x` jest wynikiem operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="bd998-1197">Operatory `++` i `--` obsługują również notację prefiksu ([Operatory przyrostu i zmniejszania prefiksu](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="bd998-1198">Zwykle wynik `x++` lub `x--` jest wartością `x` przed operacją, a wynikiem `++x` lub `--x` jest wartość `x` po operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="bd998-1199">W obu przypadkach `x` sama wartość jest taka sama po operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="bd998-1200">Implementację `operator ++` lub `operator --` można wywołać przy użyciu notacji przyrostkowej lub prefiksowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="bd998-1201">Nie można mieć odrębnych implementacji operatora dla dwóch notacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="bd998-1202">Operator new</span><span class="sxs-lookup"><span data-stu-id="bd998-1202">The new operator</span></span>

<span data-ttu-id="bd998-1203">Operator `new` służy do tworzenia nowych wystąpień typów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="bd998-1204">Istnieją trzy formy wyrażeń `new`:</span><span class="sxs-lookup"><span data-stu-id="bd998-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="bd998-1205">Wyrażenia tworzenia obiektów są używane do tworzenia nowych wystąpień typów klas i typów wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="bd998-1206">Wyrażenia tworzenia tablic są używane do tworzenia nowych wystąpień typów tablicowych.</span><span class="sxs-lookup"><span data-stu-id="bd998-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="bd998-1207">Wyrażenia tworzenia delegatów są używane do tworzenia nowych wystąpień typów delegatów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="bd998-1208">Operator `new` sugeruje utworzenie wystąpienia typu, ale niekoniecznie ma dynamiczną alokację pamięci.</span><span class="sxs-lookup"><span data-stu-id="bd998-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="bd998-1209">W szczególności wystąpienia typów wartości nie wymagają dodatkowej pamięci poza zmiennymi, w których znajdują się i nie występują żadne dynamiczne alokacje, gdy `new` jest używany do tworzenia wystąpień typów wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="bd998-1210">Wyrażenia tworzenia obiektów</span><span class="sxs-lookup"><span data-stu-id="bd998-1210">Object creation expressions</span></span>

<span data-ttu-id="bd998-1211">Element *object_creation_expression* jest używany do tworzenia nowego wystąpienia elementu *class_type* lub *value_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="bd998-1212">*Typem* elementu *object_creation_expression* musi być *class_type*, *value_type* lub *type_parameter*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="bd998-1213">*Typ* nie może być @no__t *class_type*-1.</span><span class="sxs-lookup"><span data-stu-id="bd998-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="bd998-1214">Opcjonalne *argument_list* ([listy argumentów](expressions.md#argument-lists)) są dozwolone tylko wtedy, gdy *typem* jest *class_type* lub *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="bd998-1215">Wyrażenie tworzenia obiektu może pominąć listę argumentów konstruktora i otaczające nawiasy, pod warunkiem że zawiera Inicjator obiektu lub inicjator kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="bd998-1216">Pominięcie listy argumentów konstruktora i otaczające nawiasy są równoważne do określenia pustej listy argumentów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="bd998-1217">Przetwarzanie wyrażenia tworzenia obiektu, które obejmuje inicjatora obiektów lub inicjator kolekcji, składa się z pierwszego przetwarzania konstruktora wystąpienia, a następnie przetwarzania inicjowania elementu członkowskiego lub elementu określonego przez Inicjator obiektu ([ ](expressions.md#object-initializers)Inicjatory obiektów) lub inicjalizatora kolekcji ([inicjalizacje kolekcji](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="bd998-1218">Jeśli którykolwiek z argumentów w opcjonalnej *argument_list* ma typ czasu kompilowania `dynamic`, *object_creation_expression* jest dynamicznie powiązany ([powiązanie dynamiczne](expressions.md#dynamic-binding)), a w czasie wykonywania są stosowane następujące reguły przy użyciu czasu wykonywania typ argumentów *argument_list* o typie czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="bd998-1219">Jednak tworzenie obiektów jest ograniczone przez ograniczony czas kompilacji, zgodnie z opisem w [sprawdzaniu poprawności dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="bd998-1220">Przetwarzanie powiązania *object_creation_expression* formularza `new T(A)`, gdzie `T` to *class_type* lub *value_type* , a `A` jest opcjonalny *argument_list*, składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="bd998-1221">Jeśli `T` to *value_type* i `A` nie istnieje:</span><span class="sxs-lookup"><span data-stu-id="bd998-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="bd998-1222">*Object_creation_expression* jest domyślnym wywołaniem konstruktora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="bd998-1223">Wynikiem *object_creation_expression* jest wartość typu `T`, a mianowicie wartość domyślna dla `T`, zgodnie z definicją w [typie System. ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="bd998-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="bd998-1224">W przeciwnym razie, jeśli `T` to *type_parameter* i nieobecny `A`:</span><span class="sxs-lookup"><span data-stu-id="bd998-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="bd998-1225">Jeśli nie określono ograniczenia typu wartości lub ograniczenia konstruktora ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) dla `T`, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="bd998-1226">Wynik *object_creation_expression* jest wartością typu czasu wykonywania, z którym jest powiązany parametr typu, a mianowicie wynik wywołania domyślnego konstruktora tego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="bd998-1227">Typ czasu wykonywania może być typem referencyjnym lub typem wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="bd998-1228">W przeciwnym razie, jeśli `T` to *class_type* lub *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="bd998-1229">Jeśli `T` to `abstract` *class_type*, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="bd998-1230">Konstruktor wystąpienia do wywołania jest określany przy użyciu reguł rozdzielczości przeciążenia przeciążania [przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="bd998-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="bd998-1231">Zestaw konstruktorów wystąpień kandydatów składa się z wszystkich dostępnych konstruktorów wystąpień zadeklarowanych w `T`, które mają zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="bd998-1232">Jeśli zestaw konstruktorów wystąpień kandydatów jest pusty lub nie można zidentyfikować pojedynczego konstruktora najlepszego wystąpienia, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="bd998-1233">Wynik *object_creation_expression* jest wartością typu `T`, a mianowicie wartość wygenerowaną przez wywołanie konstruktora wystąpienia określonego w powyższym kroku.</span><span class="sxs-lookup"><span data-stu-id="bd998-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="bd998-1234">W przeciwnym razie *object_creation_expression* jest nieprawidłowy i wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-1235">Nawet jeśli *object_creation_expression* jest powiązany dynamicznie, typem czasu kompilacji jest nadal `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="bd998-1236">Przetwarzanie w czasie wykonywania *object_creation_expression* formularza `new T(A)`, gdzie `T` to *class_type* lub *struct_type* i `A` jest opcjonalny *argument_list*, składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="bd998-1237">Jeśli `T` to *class_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="bd998-1238">Przydzielono nowe wystąpienie klasy `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="bd998-1239">Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia, zgłaszany jest `System.OutOfMemoryException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="bd998-1240">Wszystkie pola nowego wystąpienia są inicjowane do ich wartości domyślnych ([wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="bd998-1241">Konstruktor wystąpienia jest wywoływany zgodnie z regułami wywołania elementu członkowskiego funkcji ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="bd998-1242">Odwołanie do nowo przydzielonych wystąpień jest automatycznie przenoszone do konstruktora wystąpień i można uzyskać do niego dostęp z poziomu tego konstruktora jako `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="bd998-1243">Jeśli `T` to *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="bd998-1244">Wystąpienie typu `T` jest tworzone przez przydzielenie tymczasowej zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="bd998-1245">Ponieważ Konstruktor wystąpienia elementu *struct_type* jest wymagany do jednokrotnego przypisania wartości do każdego pola tworzonego wystąpienia, nie jest wymagane inicjowanie zmiennej tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="bd998-1246">Konstruktor wystąpienia jest wywoływany zgodnie z regułami wywołania elementu członkowskiego funkcji ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="bd998-1247">Odwołanie do nowo przydzielonych wystąpień jest automatycznie przenoszone do konstruktora wystąpień i można uzyskać do niego dostęp z poziomu tego konstruktora jako `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="bd998-1248">Inicjatory obiektów</span><span class="sxs-lookup"><span data-stu-id="bd998-1248">Object initializers</span></span>

<span data-ttu-id="bd998-1249">***Inicjator obiektu*** określa wartości dla zero lub więcej pól, właściwości lub indeksowane elementy obiektu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="bd998-1250">Inicjator obiektu składa się z kolejności inicjatorów elementów członkowskich ujętych w tokenach `{` i `}` i rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="bd998-1251">Każdy *member_initializer* wyznacza element docelowy dla inicjalizacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="bd998-1252">*Identyfikator* musi mieć nazwę dostępnego pola lub właściwości obiektu, który jest inicjowany, natomiast *argument_list* ujęty w nawiasy kwadratowe muszą określać argumenty dostępnego indeksatora dla inicjowanego obiektu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="bd998-1253">Wystąpił błąd inicjatora obiektu, aby uwzględnić więcej niż jednego inicjatora elementu członkowskiego dla tego samego pola lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd998-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="bd998-1254">Każdy *initializer_target* następuje znak równości i wyrażenie, inicjator obiektu lub inicjator kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="bd998-1255">Wyrażenia w inicjatorze obiektów nie są możliwe do odwoływania się do nowo utworzonego obiektu, który jest inicjowany.</span><span class="sxs-lookup"><span data-stu-id="bd998-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="bd998-1256">Inicjator elementu członkowskiego, który określa wyrażenie po znaku równości jest przetwarzane w taki sam sposób jak przypisanie ([przypisanie proste](expressions.md#simple-assignment)) do obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="bd998-1257">Inicjator elementu członkowskiego, który określa Inicjator obiektu po znaku równości, jest ***zagnieżdżonym inicjatorem obiektów***, czyli inicjalizacją osadzonego obiektu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="bd998-1258">Zamiast przypisywać nową wartość do pola lub właściwości, przypisania w inicjatorze zagnieżdżonych obiektów są traktowane jako przypisania do elementów członkowskich pola lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd998-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="bd998-1259">Inicjatory zagnieżdżonych obiektów nie mogą być stosowane do właściwości z typem wartości lub do pól tylko do odczytu z typem wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="bd998-1260">Inicjator elementu członkowskiego, który określa inicjator kolekcji po znaku równości jest inicjalizacją osadzonej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="bd998-1261">Zamiast przypisywać nową kolekcję do pola docelowego, właściwości lub indeksatora elementy podane w inicjatorze są dodawane do kolekcji, do której odwołuje się element docelowy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="bd998-1262">Element docelowy musi być typu kolekcji, który spełnia wymagania określone w [inicjatorach kolekcji](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="bd998-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="bd998-1263">Argumenty inicjatora indeksu zawsze będą oceniane dokładnie jeden raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="bd998-1264">W związku z tym nawet jeśli argumenty zakończą się nigdy nie są używane (np. z powodu pustego inicjatora zagnieżdżonego), zostaną ocenione pod kątem ich efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="bd998-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="bd998-1265">Następująca Klasa reprezentuje punkt z dwoma współrzędnymi:</span><span class="sxs-lookup"><span data-stu-id="bd998-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="bd998-1266">Wystąpienie `Point` można utworzyć i zainicjować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="bd998-1267">ma ten sam skutek co</span><span class="sxs-lookup"><span data-stu-id="bd998-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="bd998-1268">gdzie `__a` to w inny sposób niewidoczna i niedostępna zmienna tymczasowa.</span><span class="sxs-lookup"><span data-stu-id="bd998-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="bd998-1269">Następująca Klasa reprezentuje prostokąt utworzony na podstawie dwóch punktów:</span><span class="sxs-lookup"><span data-stu-id="bd998-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="bd998-1270">Wystąpienie `Rectangle` można utworzyć i zainicjować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="bd998-1271">ma ten sam skutek co</span><span class="sxs-lookup"><span data-stu-id="bd998-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="bd998-1272">gdzie `__r`, `__p1` i `__p2` są zmiennymi tymczasowymi, które w przeciwnym razie są niewidoczne i niedostępne.</span><span class="sxs-lookup"><span data-stu-id="bd998-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="bd998-1273">Jeśli Konstruktor `Rectangle` przypisuje dwa wystąpienia osadzonych `Point`</span><span class="sxs-lookup"><span data-stu-id="bd998-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="bd998-1274">Następująca konstrukcja może służyć do inicjowania osadzonych wystąpień `Point` zamiast przypisywania nowych wystąpień:</span><span class="sxs-lookup"><span data-stu-id="bd998-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="bd998-1275">ma ten sam skutek co</span><span class="sxs-lookup"><span data-stu-id="bd998-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="bd998-1276">Zgodnie z odpowiednią definicją C, Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="bd998-1277">jest odpowiednikiem tej serii przypisań:</span><span class="sxs-lookup"><span data-stu-id="bd998-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="bd998-1278">gdzie `__c` itp., są generowane zmienne, które są niewidoczne i niedostępne dla kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="bd998-1279">Należy zauważyć, że argumenty dla `[0,0]` są oceniane tylko raz, a argumenty dla `[1,2]` są oceniane raz, mimo że nigdy nie są używane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="bd998-1280">Inicjatory kolekcji</span><span class="sxs-lookup"><span data-stu-id="bd998-1280">Collection initializers</span></span>

<span data-ttu-id="bd998-1281">Inicjator kolekcji określa elementy kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="bd998-1282">Inicjator kolekcji składa się z sekwencji elementów inicjujących, zawartych w tokenach `{` i `}` i rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="bd998-1283">Każdy inicjator elementów określa element, który ma zostać dodany do obiektu kolekcji, który jest inicjowany, i składa się z listy wyrażeń ujętych w tokenach `{` i `}` i rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="bd998-1284">Inicjator elementu pojedynczego wyrażenia może być zapisany bez nawiasów klamrowych, ale nie może być wyrażeniem przypisania, aby uniknąć niejednoznaczności z inicjatorami elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="bd998-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="bd998-1285">Produkcja *non_assignment_expression* jest zdefiniowana w [wyrażeniu](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="bd998-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="bd998-1286">Poniżej znajduje się przykład wyrażenia tworzenia obiektu zawierającego inicjator kolekcji:</span><span class="sxs-lookup"><span data-stu-id="bd998-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="bd998-1287">Obiekt kolekcji, do którego zastosowano inicjator kolekcji, musi być typu, który implementuje `System.Collections.IEnumerable` lub wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="bd998-1288">Dla każdego określonego elementu w kolejności inicjator kolekcji wywołuje metodę `Add` w obiekcie docelowym z listą wyrażeń inicjatora elementu jako listą argumentów, stosując normalne wyszukiwanie elementów członkowskich i rozpoznawanie przeciążenia dla każdego wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="bd998-1289">W ten sposób obiekt kolekcji musi mieć odpowiednie wystąpienie lub metodę rozszerzenia o nazwie `Add` dla każdego inicjatora elementu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="bd998-1290">Następująca Klasa reprezentuje kontakt z nazwą i listą numerów telefonów:</span><span class="sxs-lookup"><span data-stu-id="bd998-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="bd998-1291">@No__t-0 można utworzyć i zainicjować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="bd998-1292">ma ten sam skutek co</span><span class="sxs-lookup"><span data-stu-id="bd998-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="bd998-1293">gdzie `__clist`, `__c1` i `__c2` są zmiennymi tymczasowymi, które w przeciwnym razie są niewidoczne i niedostępne.</span><span class="sxs-lookup"><span data-stu-id="bd998-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="bd998-1294">Wyrażenia tworzenia tablicy</span><span class="sxs-lookup"><span data-stu-id="bd998-1294">Array creation expressions</span></span>

<span data-ttu-id="bd998-1295">Element *array_creation_expression* jest używany do tworzenia nowego wystąpienia elementu *array_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="bd998-1296">Wyrażenie tworzenia tablicy w pierwszym formularzu przypisuje wystąpienie tablicy typu, które powoduje usunięcie każdego z poszczególnych wyrażeń z listy wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="bd998-1297">Na przykład wyrażenie tworzenia tablicy `new int[10,20]` tworzy wystąpienie tablicy typu `int[,]` i wyrażenie tworzenia tablicy `new int[10][,]` tworzy tablicę typu `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="bd998-1298">Każde wyrażenie na liście wyrażeń musi być typu `int`, `uint`, `long` lub `ulong` lub niejawnie konwertowane na jeden lub więcej z tych typów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="bd998-1299">Wartość każdego wyrażenia określa długość odpowiedniego wymiaru w nowo przydzielonym wystąpieniu tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="bd998-1300">Ponieważ długość wymiaru tablicy nie może być ujemna, jest to błąd czasu kompilacji, który ma *constant_expression* z ujemną wartością na liście wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="bd998-1301">W przypadku niebezpiecznego kontekstu ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)) nie określono układu tablic.</span><span class="sxs-lookup"><span data-stu-id="bd998-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="bd998-1302">Jeśli wyrażenie tworzenia tablicowe w pierwszym formularzu zawiera inicjatora tablicy, każde wyrażenie na liście wyrażeń musi być stałą, a długość rangi i wymiary określona przez listę wyrażeń musi być zgodna z właściwościami inicjatora tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="bd998-1303">W wyrażeniu tworzenia tablicy drugiego lub trzeciego formularza ranga określonego typu tablicy lub specyfikatora rangi musi być zgodna z inicjatorem tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="bd998-1304">Długości poszczególnych wymiarów są wywnioskowane na podstawie liczby elementów w każdym z odpowiednich poziomów zagnieżdżenia inicjatora tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="bd998-1305">W tym celu wyrażenie</span><span class="sxs-lookup"><span data-stu-id="bd998-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="bd998-1306">dokładnie odpowiada</span><span class="sxs-lookup"><span data-stu-id="bd998-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="bd998-1307">Wyrażenie tworzenia tablicy trzeciego formularza jest określane jako ***wyrażenie tworzenia tablicy niejawnie wpisanej***.</span><span class="sxs-lookup"><span data-stu-id="bd998-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="bd998-1308">Jest on podobny do drugiego formularza, z tą różnicą, że typ elementu tablicy nie jest jawnie określony, ale określony jako najlepszy typowy typ ([znalezienie najlepszego wspólnego typu zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) zestawu wyrażeń w inicjatorze tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="bd998-1309">Dla tablicy wielowymiarowej, tj., gdzie *rank_specifier* zawiera co najmniej jeden przecinek, ten zestaw składa się ze wszystkich *wyrażeń*, które znajdują się w zagnieżdżonych *array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="bd998-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="bd998-1310">Inicjatory tablicy są szczegółowo opisane w [inicjatorach tablicy](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="bd998-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="bd998-1311">Wynik obliczania wyrażenia tworzenia tablicy jest klasyfikowany jako wartość, a mianowicie odwołanie do nowo przydzielonych wystąpień tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="bd998-1312">Przetwarzanie wyrażenia tworzenia tablicy w czasie wykonywania składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-1313">Wyrażenia długości wymiarów *expression_list* są oceniane w kolejności od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="bd998-1314">Po dokonaniu oceny każdego wyrażenia niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) do jednego z następujących typów jest wykonywana: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="bd998-1315">Pierwszy typ na tej liście, dla którego istnieje niejawna konwersja, jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="bd998-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="bd998-1316">Jeśli Obliczanie wyrażenia lub kolejnej niejawnej konwersji powoduje wyjątek, nie są oceniane żadne dalsze wyrażenia i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1317">Obliczone wartości dla długości wymiarów są weryfikowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="bd998-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="bd998-1318">Jeśli co najmniej jedna z wartości jest mniejsza od zera, zgłaszany jest `System.OverflowException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1319">Przydzielono wystąpienie tablicy z podaną długością wymiaru.</span><span class="sxs-lookup"><span data-stu-id="bd998-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="bd998-1320">Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia, zgłaszany jest `System.OutOfMemoryException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="bd998-1321">Wszystkie elementy nowego wystąpienia tablicy są inicjowane do ich wartości domyślnych ([wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="bd998-1322">Jeśli wyrażenie tworzenia tablicy zawiera inicjatora tablicy, każde wyrażenie w inicjatorze tablicy jest oceniane i przypisywane do odpowiadającego mu elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="bd998-1323">Obliczenia i przypisania są wykonywane w kolejności, w której wyrażenia są zapisywane w inicjatorze tablicy — innymi słowy, elementy są inicjowane w kolejności rosnącego indeksu, a najwyższego rzędu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="bd998-1324">Jeśli Obliczanie danego wyrażenia lub kolejnego przypisania do odpowiadającego elementu tablicy powoduje wyjątek, nie są inicjowane żadne dalsze elementy (a pozostałe elementy będą mieć wartości domyślne).</span><span class="sxs-lookup"><span data-stu-id="bd998-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="bd998-1325">Wyrażenie tworzenia tablicy umożliwia utworzenie wystąpienia tablicy z elementami typu tablicy, ale elementy takich tablic muszą być inicjowane ręcznie.</span><span class="sxs-lookup"><span data-stu-id="bd998-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="bd998-1326">Na przykład, instrukcja</span><span class="sxs-lookup"><span data-stu-id="bd998-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="bd998-1327">Tworzy tablicę jednowymiarową z 100 elementami typu `int[]`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="bd998-1328">Wartość początkowa każdego elementu to `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="bd998-1329">Nie jest możliwe dla tego samego wyrażenia tworzenia tablicy, które również tworzy wystąpienia tablic podrzędnych, i instrukcji</span><span class="sxs-lookup"><span data-stu-id="bd998-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="bd998-1330">powoduje błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1330">results in a compile-time error.</span></span> <span data-ttu-id="bd998-1331">Zamiast tego należy wykonać ręczne tworzenie wystąpienia tablic podrzędnych, jak w</span><span class="sxs-lookup"><span data-stu-id="bd998-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="bd998-1332">Gdy tablica tablic ma kształt "prostokątny", jeśli podtablice mają taką samą długość, bardziej wydajne jest użycie tablicy wielowymiarowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="bd998-1333">W powyższym przykładzie Tworzenie wystąpienia tablicy tablic tworzy 101 obiektów — jedną tablicę zewnętrzną i tablicę podrzędną 100.</span><span class="sxs-lookup"><span data-stu-id="bd998-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="bd998-1334">W przeciwieństwie do</span><span class="sxs-lookup"><span data-stu-id="bd998-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="bd998-1335">tworzy tylko jeden obiekt, dwuwymiarową tablicę i wykonuje alokację w pojedynczej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="bd998-1336">Poniżej przedstawiono przykłady niejawnie wpisanych wyrażeń tworzenia tablicy:</span><span class="sxs-lookup"><span data-stu-id="bd998-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="bd998-1337">Ostatnie wyrażenie powoduje błąd czasu kompilacji, ponieważ żadne `int` ani `string` nie są niejawnie konwertowane na inne, więc nie ma żadnego najlepszego wspólnego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="bd998-1338">W tym przypadku jawnie wpisane wyrażenie tworzenia tablicy musi być używane, na przykład określając typ do `object[]`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="bd998-1339">Alternatywnie, jeden z elementów może być rzutowany na wspólny typ podstawowy, który następnie stanie się wywnioskowanym typem elementu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="bd998-1340">Niejawnie wpisane wyrażenia tworzenia tablicy można łączyć z inicjatorami anonimowych obiektów ([wyrażeniami tworzenia obiektów anonimowych](expressions.md#anonymous-object-creation-expressions)) w celu tworzenia anonimowo wpisanych struktur danych.</span><span class="sxs-lookup"><span data-stu-id="bd998-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="bd998-1341">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="bd998-1342">Delegowanie wyrażeń tworzenia</span><span class="sxs-lookup"><span data-stu-id="bd998-1342">Delegate creation expressions</span></span>

<span data-ttu-id="bd998-1343">Element *delegate_creation_expression* jest używany do tworzenia nowego wystąpienia elementu *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="bd998-1344">Argumentem wyrażenia tworzenia delegata musi być grupa metod, funkcja anonimowa lub wartość typu czasu kompilacji `dynamic` lub *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="bd998-1345">Jeśli argument jest grupą metod, identyfikuje metodę i, dla metody wystąpienia, obiekt, dla którego ma zostać utworzony delegat.</span><span class="sxs-lookup"><span data-stu-id="bd998-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="bd998-1346">Jeśli argument jest funkcją anonimową, bezpośrednio definiuje parametry i treść metody obiektu docelowego delegata.</span><span class="sxs-lookup"><span data-stu-id="bd998-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="bd998-1347">Jeśli argument jest wartością, określa wystąpienie delegata, dla którego ma zostać utworzona kopia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="bd998-1348">Jeśli *wyrażenie* ma typ czasu kompilacji `dynamic`, *delegate_creation_expression* jest powiązana dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), a Poniższe reguły są stosowane w czasie wykonywania przy użyciu typu w czasie wykonywania *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="bd998-1349">W przeciwnym razie reguły są stosowane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="bd998-1350">Przetwarzanie powiązania *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* , a `E` jest *wyrażeniem*, składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-1351">Jeśli `E` jest grupą metod, wyrażenie tworzenia delegata jest przetwarzane w taki sam sposób jak w przypadku konwersji grup metod ([konwersje grup](conversions.md#method-group-conversions)metod) z `E` do `D`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="bd998-1352">Jeśli `E` to funkcja anonimowa, wyrażenie tworzenia delegata jest przetwarzane w taki sam sposób jak w przypadku konwersji funkcji anonimowej ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)) z `E` do `D`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="bd998-1353">Jeśli `E` jest wartością, `E` musi być zgodna ([deklaracje delegatów](delegates.md#delegate-declarations)) z `D`, a wynik jest odwołaniem do nowo utworzonego delegata typu `D`, który odwołuje się do tej samej listy wywołań co `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="bd998-1354">Jeśli `E` nie jest zgodny z `D`, wystąpi błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="bd998-1355">Przetwarzanie w czasie wykonywania *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* , a `E` jest *wyrażeniem*, składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="bd998-1356">Jeśli `E` jest grupą metod, wyrażenie tworzenia delegata jest oceniane jako konwersja grup metod ([konwersje grup](conversions.md#method-group-conversions)metod) z `E` do `D`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="bd998-1357">Jeśli `E` to funkcja anonimowa, Tworzenie delegata jest oceniane jako anonimowa funkcja konwersji z `E` na `D` ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="bd998-1358">Jeśli `E` jest wartością elementu *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="bd998-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="bd998-1359">`E` jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1359">`E` is evaluated.</span></span> <span data-ttu-id="bd998-1360">Jeśli ta Ocena spowoduje wyjątek, nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="bd998-1361">Jeśli wartość `E` jest `null`, zgłaszany jest `System.NullReferenceException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="bd998-1362">Zostanie przydzielono nowe wystąpienie typu delegata `D`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="bd998-1363">Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia, zgłaszany jest `System.OutOfMemoryException` i nie są wykonywane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="bd998-1364">Nowe wystąpienie delegata jest inicjowane z tą samą listą wywołania co wystąpienie delegata określone przez `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="bd998-1365">Lista wywołań delegata jest określana podczas tworzenia wystąpienia delegata, a następnie pozostaje stała dla całego okresu istnienia delegata.</span><span class="sxs-lookup"><span data-stu-id="bd998-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="bd998-1366">Innymi słowy, nie można zmienić docelowej możliwej jednostki delegata po jego utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="bd998-1367">Gdy dwa Delegaty są połączone lub jeden z nich jest usuwany z innego ([deklaracje delegatów](delegates.md#delegate-declarations)), nowe wyniki delegata; zawartość żadnego z istniejących delegatów nie została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="bd998-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="bd998-1368">Nie można utworzyć delegata, który odwołuje się do właściwości, indeksatora, zdefiniowanego przez użytkownika operatora, konstruktora wystąpienia, destruktora lub konstruktora statycznego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="bd998-1369">Jak opisano powyżej, gdy delegat jest tworzony na podstawie grupy metod, formalna lista parametrów i zwracany typ delegata określają, które z przeciążonych metod wybrać.</span><span class="sxs-lookup"><span data-stu-id="bd998-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="bd998-1370">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="bd998-1371">pole `A.f` jest inicjowane z delegatem, który odwołuje się do drugiej metody `Square`, ponieważ ta metoda dokładnie pasuje do formalnej listy parametrów i zwracanego typu `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="bd998-1372">W przypadku drugiej metody `Square` nie jest obecna, wystąpił błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="bd998-1373">Wyrażenia tworzenia obiektów anonimowych</span><span class="sxs-lookup"><span data-stu-id="bd998-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="bd998-1374">Element *anonymous_object_creation_expression* jest używany do tworzenia obiektu typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="bd998-1375">Inicjator obiektu anonimowego deklaruje typ anonimowy i zwraca wystąpienie tego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="bd998-1376">Typ anonimowy to typ klasy pustego, który dziedziczy bezpośrednio z `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="bd998-1377">Elementy członkowskie typu anonimowego są sekwencją właściwości tylko do odczytu wywnioskowanej z inicjatora obiektu anonimowego używanego do tworzenia wystąpienia typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="bd998-1378">W przypadku inicjatora obiektów anonimowych formularza</span><span class="sxs-lookup"><span data-stu-id="bd998-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="bd998-1379">deklaruje anonimowy typ formularza</span><span class="sxs-lookup"><span data-stu-id="bd998-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="bd998-1380">gdzie każda `Tx` jest typem odpowiedniego wyrażenia `ex`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="bd998-1381">Wyrażenie użyte w *member_declarator* musi mieć typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="bd998-1382">W ten sposób jest to błąd czasu kompilacji dla wyrażenia w *member_declarator* , aby mieć wartość null lub funkcję anonimową.</span><span class="sxs-lookup"><span data-stu-id="bd998-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="bd998-1383">Jest to również błąd czasu kompilacji dla wyrażenia, aby można było mieć niebezpieczny typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="bd998-1384">Nazwy typu anonimowego i parametru do jego metody `Equals` są generowane automatycznie przez kompilator i nie można się do niego odwoływać w tekście programu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="bd998-1385">W ramach tego samego programu dwa anonimowe Inicjatory obiektów, które określają sekwencję właściwości tych samych nazw i typów czasu kompilacji w tej samej kolejności, spowodują wystąpienie tego samego typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="bd998-1386">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="bd998-1387">przypisanie w ostatnim wierszu jest dozwolone, ponieważ `p1` i `p2` są tego samego typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="bd998-1388">Metody `Equals` i `GetHashcode` w typach anonimowych przesłaniają metody dziedziczone z `object` i są zdefiniowane w kategoriach `Equals` i `GetHashcode` właściwości, tak aby dwa wystąpienia tego samego typu anonimowego były równe, gdy tylko, jeśli wszystkie ich właściwości są większy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="bd998-1389">Element członkowski deklarator może być skrócony do prostej nazwy ([wnioskowania o typie](expressions.md#type-inference)), dostępu do elementu członkowskiego ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), dostęp podstawowy ([dostęp podstawowy](expressions.md#base-access)) lub dostęp do składowej warunkowej o wartości null ([ Wyrażenia warunkowe o wartości null jako inicjatory projekcji](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="bd998-1390">Jest to nazywane ***inicjatorem projekcji*** i jest skrótem dla deklaracji i przypisywania do właściwości o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="bd998-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="bd998-1391">W każdym przypadku element członkowski Deklaratory formularzy</span><span class="sxs-lookup"><span data-stu-id="bd998-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="bd998-1392">są dokładnie równoważne następujących odpowiednio:</span><span class="sxs-lookup"><span data-stu-id="bd998-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="bd998-1393">W tym celu w inicjatorze projekcji *Identyfikator* wybiera zarówno wartość, jak i pole lub właściwość, do której zostanie przypisana wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="bd998-1394">Intuicyjnie, inicjator projekcji nie tylko wartości, ale również nazwę wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="bd998-1395">Operator typeof</span><span class="sxs-lookup"><span data-stu-id="bd998-1395">The typeof operator</span></span>

<span data-ttu-id="bd998-1396">Operator `typeof` służy do uzyskania obiektu `System.Type` dla typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="bd998-1397">Pierwszy formularz *typeof_expression* składa się ze słowa kluczowego `typeof`, po którym następuje *Typ*ujęty w nawiasy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="bd998-1398">Wynikiem wyrażenia tego formularza jest obiekt `System.Type` dla wskazanego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="bd998-1399">Istnieje tylko jeden obiekt `System.Type` dla danego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="bd998-1400">Oznacza to, że dla typu @ no__t-0 `typeof(T) == typeof(T)` ma zawsze wartość true.</span><span class="sxs-lookup"><span data-stu-id="bd998-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="bd998-1401">*Typ* nie może być `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="bd998-1402">Druga forma *typeof_expression* składa się ze słowa kluczowego `typeof`, po którym następuje *unbound_type_name*w nawiasach klamrowych.</span><span class="sxs-lookup"><span data-stu-id="bd998-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="bd998-1403">*Unbound_type_name* jest bardzo podobna do *TYPE_NAME* ([nazw i typów](basic-concepts.md#namespace-and-type-names)nazw), z tą różnicą, że *unbound_type_name* zawiera *generic_dimension_specifier*s, gdzie *TYPE_NAME* zawiera *type_ argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="bd998-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="bd998-1404">Gdy operand elementu *typeof_expression* jest sekwencją tokenów, które spełniają gramatyki obu *unbound_type_name* i *TYPE_NAME*, a mianowicie, gdy nie zawiera *generic_dimension_specifier* ani *type_argument _list*, sekwencja tokenów jest uznawana za *TYPE_NAME*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="bd998-1405">Znaczenie *unbound_type_name* jest określone w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="bd998-1406">Przekonwertuj sekwencję tokenów na *TYPE_NAME* , zastępując każdy *generic_dimension_specifier* z *type_argument_listą* o takiej samej liczbie przecineków i słowem kluczowym `object` jako każdy *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="bd998-1407">Oceń wynikowy *TYPE_NAME*, ignorując wszystkie ograniczenia dotyczące parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="bd998-1408">*Unbound_type_name* jest rozpoznawany jako niezwiązany typ ogólny skojarzony z wynikiem skonstruowanego typu ([powiązane i niepowiązane typy](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="bd998-1409">Wynik *typeof_expression* jest obiektem `System.Type` dla wynikowego niepowiązanego typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="bd998-1410">Trzecia postać *typeof_expression* składa się ze słowa kluczowego `typeof`, po którym następuje słowo kluczowe `void`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="bd998-1411">Wynikiem wyrażenia tego formularza jest obiekt `System.Type`, który reprezentuje nieobecność typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="bd998-1412">Obiekt typu zwracany przez `typeof(void)` różni się od obiektu typu zwracanego dla dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="bd998-1413">Ten obiekt typu specjalnego jest przydatny w bibliotekach klas, które umożliwiają odbicie w metodach w języku, gdzie te metody chcą mieć sposób reprezentowania zwracanego typu metody, w tym metod void, z wystąpieniem `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="bd998-1414">Operatora `typeof` można używać w parametrze typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="bd998-1415">Wynik jest obiektem `System.Type` dla typu czasu wykonywania, który został powiązany z parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="bd998-1416">Operatora `typeof` można także użyć dla typu złożonego lub niepowiązanego typu ogólnego ([powiązane i niepowiązane typy](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="bd998-1417">Obiekt `System.Type` dla niepowiązanego typu ogólnego nie jest taki sam jak obiekt `System.Type` typu wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="bd998-1418">Typ wystąpienia jest zawsze zamknięty typ skonstruowany w czasie wykonywania, tak aby obiekt `System.Type` zależał od argumentów typu run w użyciu, podczas gdy niezwiązany typ ogólny nie ma argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="bd998-1419">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="bd998-1420">tworzy następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-1420">produces the following output:</span></span>
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="bd998-1421">Należy zauważyć, że `int` i `System.Int32` są tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="bd998-1422">Należy również zauważyć, że wynik `typeof(X<>)` nie zależy od argumentu typu, ale wynik `typeof(X<T>)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="bd998-1423">Sprawdzone i niesprawdzone operatory</span><span class="sxs-lookup"><span data-stu-id="bd998-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="bd998-1424">Operatory `checked` i `unchecked` służą do kontrolowania ***kontekstu sprawdzania przepełnienia*** dla operacji arytmetycznych typu całkowitego i konwersji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="bd998-1425">Operator `checked` oblicza wyrażenie zawarte w zaznaczonym kontekście, a operator `unchecked` oblicza wyrażenie zawarte w niesprawdzonym kontekście.</span><span class="sxs-lookup"><span data-stu-id="bd998-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="bd998-1426">*Checked_expression* lub *unchecked_expression* odpowiada dokładnie na *parenthesized_expression* ([wyrażenia w nawiasach](expressions.md#parenthesized-expressions)), z tą różnicą, że zawarte wyrażenie jest oceniane w danym kontekście sprawdzania przepełnienia .</span><span class="sxs-lookup"><span data-stu-id="bd998-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="bd998-1427">Kontekst sprawdzania przepełnienia można również kontrolować za pomocą instrukcji `checked` i `unchecked` ([instrukcje sprawdzone i niesprawdzone](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="bd998-1428">Kontekst sprawdzania przepełnienia ma wpływ na następujące operacje, które zostały określone przez operatory i instrukcje `checked` i `unchecked`:</span><span class="sxs-lookup"><span data-stu-id="bd998-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="bd998-1429">Wstępnie zdefiniowane operatory jednoargumentowe `++` i `--` (operatory[przyrostka i zmniejszenie](expressions.md#postfix-increment-and-decrement-operators) [prefiksu oraz operatory przyrostu i zmniejszania](expressions.md#prefix-increment-and-decrement-operators)), gdy operand jest typem całkowitym.</span><span class="sxs-lookup"><span data-stu-id="bd998-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="bd998-1430">Wstępnie zdefiniowany operator jednoargumentowy `-` ([jednoargumentowy operator minus](expressions.md#unary-minus-operator)), gdy operand jest typu całkowitego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="bd998-1431">Wstępnie zdefiniowane `+`, `-`, `*` i `/` operatory binarne ([Operatory arytmetyczne](expressions.md#arithmetic-operators)), gdy oba operandy są typami całkowitymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="bd998-1432">Jawne konwersje numeryczne ([jawne konwersje liczbowe](conversions.md#explicit-numeric-conversions)) z jednego typu całkowitego do innego typu całkowitego lub z `float` lub `double` do typu całkowitego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="bd998-1433">Gdy jedna z powyższych operacji tworzy wynik, który jest zbyt duży do reprezentowania w typie docelowym, kontekst, w którym wykonywana jest operacja, kontroluje Wynikowe zachowanie:</span><span class="sxs-lookup"><span data-stu-id="bd998-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="bd998-1434">W kontekście `checked`, jeśli operacja jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions)), wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="bd998-1435">W przeciwnym razie, gdy operacja zostanie wykonana w czasie wykonywania, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="bd998-1436">W kontekście `unchecked` wynik jest obcinany przez odrzucenie wszystkich bitów o dużej kolejności, które nie mieszczą się w typie docelowym.</span><span class="sxs-lookup"><span data-stu-id="bd998-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="bd998-1437">Dla wyrażeń niestałych (wyrażeń, które są oceniane w czasie wykonywania), które nie są ujęte w żadne operatory `checked` lub `unchecked`, domyślny kontekst sprawdzania przepełnienia jest `unchecked`, chyba że zewnętrzne czynniki (takie jak przełączniki kompilatora i Konfiguracja środowiska wykonawczego) Wywołaj dla `checked` oceny.</span><span class="sxs-lookup"><span data-stu-id="bd998-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="bd998-1438">W przypadku wyrażeń stałych (wyrażenia, które mogą być w pełni oceniane w czasie kompilacji), domyślny kontekst sprawdzania przepełnienia jest zawsze `checked`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="bd998-1439">Jeśli wyrażenie stałe nie jest jawnie umieszczane w kontekście `unchecked`, nadprzepływy występujące podczas oceny czasu kompilacji wyrażenia zawsze powodują błędy w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="bd998-1440">Nie ma to wpływ na treść funkcji anonimowej, `checked` lub `unchecked` kontekst, w którym występuje funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="bd998-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="bd998-1441">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="bd998-1442">nie zgłoszono błędów czasu kompilacji, ponieważ żadne wyrażenia nie mogą być oceniane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="bd998-1443">W czasie wykonywania Metoda `F` zgłasza `System.OverflowException`, a metoda `G` zwraca wartość-727379968 (Dolna szybkość 32 bitów wyniku spoza zakresu).</span><span class="sxs-lookup"><span data-stu-id="bd998-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="bd998-1444">Zachowanie metody `H` zależy od domyślnego kontekstu sprawdzania przepełnienia dla kompilacji, ale jest to taka sama jak `F` lub taka sama jak `G`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="bd998-1445">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="bd998-1446">przepełnienia występujące podczas oceniania wyrażeń stałych w `F` i `H` powodują zgłoszenie błędów w czasie kompilacji, ponieważ wyrażenia są oceniane w kontekście `checked`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="bd998-1447">Przepełnienie ma również miejsce podczas obliczania wyrażenia stałego w `G`, ale ponieważ Ocena odbywa się w kontekście `unchecked`, przepełnienie nie zostanie zgłoszone.</span><span class="sxs-lookup"><span data-stu-id="bd998-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="bd998-1448">Operatory `checked` i `unchecked` mają wpływ tylko na kontekst sprawdzania przepełnienia dla tych operacji, które znajdują się w nawiasach w tokenach "`(`" i "`)`".</span><span class="sxs-lookup"><span data-stu-id="bd998-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="bd998-1449">Operatory nie mają wpływu na składowe funkcji, które są wywoływane w wyniku obliczania zawartego wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="bd998-1450">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="bd998-1451">Użycie `checked` w `F` nie ma wpływu na ocenę `x * y` w `Multiply`, więc `x * y` jest oceniane w kontekście domyślnego sprawdzania przepełnienia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="bd998-1452">Operator `unchecked` jest wygodny podczas pisania stałych z podpisanych typów całkowitych w notacji szesnastkowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="bd998-1453">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="bd998-1454">Obie stałe szesnastkowe powyżej są typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="bd998-1455">Ponieważ stałe są spoza zakresu `int`, bez operatora `unchecked`, rzutowania do `int` spowodują błędy w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="bd998-1456">Operatory i instrukcje `checked` i `unchecked` umożliwiają programistom kontrolę niektórych aspektów niektórych obliczeń liczbowych.</span><span class="sxs-lookup"><span data-stu-id="bd998-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="bd998-1457">Jednak zachowanie niektórych operatorów numerycznych zależy od typów danych argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="bd998-1458">Na przykład, mnożenie dwóch miejsc dziesiętnych zawsze powoduje wyjątek w przypadku przepełnienia nawet w ramach konstruowania jawnie `unchecked`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="bd998-1459">Podobnie mnożenie dwóch wartości zmiennoprzecinkowych nigdy nie powoduje wykonania wyjątku w przepełnieniu nawet w ramach konstruowania jawnie `checked`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="bd998-1460">Ponadto inne operatory nigdy nie mają wpływ na tryb sprawdzania, bez względu na to, czy jest to ustawienie domyślne, czy jawne.</span><span class="sxs-lookup"><span data-stu-id="bd998-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="bd998-1461">Wyrażenia wartości domyślnych</span><span class="sxs-lookup"><span data-stu-id="bd998-1461">Default value expressions</span></span>

<span data-ttu-id="bd998-1462">Domyślne wyrażenie wartości służy do uzyskiwania wartości domyślnej ([wartości domyślne](variables.md#default-values)) typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="bd998-1463">Zwykle wyrażenie wartości domyślnej jest używane dla parametrów typu, ponieważ może być nieznane, jeśli parametr typu jest typem wartości lub typem referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="bd998-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="bd998-1464">(Nie istnieje konwersja z literału `null` do parametru typu, chyba że parametr typu jest znany jako typ referencyjny).</span><span class="sxs-lookup"><span data-stu-id="bd998-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="bd998-1465">Jeśli *Typ* w *default_value_expression* jest obliczany w czasie wykonywania do typu referencyjnego, wynik jest `null` konwertowany do tego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="bd998-1466">Jeśli *Typ* w *default_value_expression* jest obliczany w czasie wykonywania do typu wartości, wynik jest wartością domyślną *value_type*([konstruktory domyślne](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="bd998-1467">*Default_value_expression* jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions)), jeśli typ jest typem referencyjnym lub parametrem typu, który jest znany jako typ referencyjny ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="bd998-1468">Ponadto *default_value_expression* jest wyrażeniem stałym, jeśli typ jest jednym z następujących typów wartości: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` lub dowolnego typu wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="bd998-1469">Wyrażenia nameof</span><span class="sxs-lookup"><span data-stu-id="bd998-1469">Nameof expressions</span></span>

<span data-ttu-id="bd998-1470">*Nameof_expression* jest używany do uzyskania nazwy jednostki programu jako ciągu stałej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="bd998-1471">Mówiąc gramatycznie, operand *named_entity* jest zawsze wyrażeniem.</span><span class="sxs-lookup"><span data-stu-id="bd998-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="bd998-1472">Ponieważ `nameof` nie jest zastrzeżonym słowem kluczowym, wyrażenie NameOf jest zawsze niejednoznaczne składniowo z wywołaniem prostej nazwy `nameof`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="bd998-1473">Ze względu na zgodność, jeśli wyszukiwanie nazw ([proste nazwy](expressions.md#simple-names)) nazwy `nameof` powiedzie się, wyrażenie jest traktowane jako *invocation_expression* --niezależnie od tego, czy wywołanie jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="bd998-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="bd998-1474">W przeciwnym razie jest to *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="bd998-1475">Znaczenie *named_entity* *nameof_expression* jest jego znaczeniem jako wyrażenia; oznacza to, że jako *simple_name*, *base_access* lub *member_access*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="bd998-1476">Jednak jeśli odnośnik opisany w temacie [proste nazwy](expressions.md#simple-names) i [dostęp do elementów członkowskich](expressions.md#member-access) powoduje błąd, ponieważ element członkowski wystąpienia został znaleziony w kontekście statycznym, *nameof_expression* nie tworzy takiego błędu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="bd998-1477">Jest to błąd czasu kompilacji dla *named_entity* wyznaczania grupy metod w celu uzyskania elementu *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="bd998-1478">Jest to błąd czasu kompilacji dla elementu *named_entity_target* , aby miał typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="bd998-1479">*Nameof_expression* jest wyrażeniem stałym typu `string` i nie ma wpływu na środowisko uruchomieniowe.</span><span class="sxs-lookup"><span data-stu-id="bd998-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="bd998-1480">W odróżnieniu od *named_entity* nie jest oceniana i jest ignorowany w celach analizy przypisywania ([ogólne reguły dla prostych wyrażeń](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="bd998-1481">Jej wartość jest ostatnim identyfikatorem *named_entity* przed opcjonalnym końcowym *type_argument_listem*, przekształconym w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="bd998-1482">Prefiks "`@`", jeśli jest używany, jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="bd998-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="bd998-1483">Każdy *unicode_escape_sequence* jest przekształcany do odpowiedniego znaku Unicode.</span><span class="sxs-lookup"><span data-stu-id="bd998-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="bd998-1484">Wszystkie *formatting_characters* są usuwane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="bd998-1485">Są to te same przekształcenia, które są stosowane w [identyfikatorach](lexical-structure.md#identifiers) podczas testowania równości między identyfikatorami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="bd998-1486">Do zrobienia: przykłady</span><span class="sxs-lookup"><span data-stu-id="bd998-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="bd998-1487">Wyrażenia metody anonimowej</span><span class="sxs-lookup"><span data-stu-id="bd998-1487">Anonymous method expressions</span></span>

<span data-ttu-id="bd998-1488">*Anonymous_method_expression* to jeden z dwóch sposobów definiowania funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="bd998-1489">Są one dokładniej opisane w [wyrażeniach funkcji anonimowych](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="bd998-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="bd998-1490">Operatory jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="bd998-1490">Unary operators</span></span>

<span data-ttu-id="bd998-1491">Operatory `?`, `+`, `-`, `!`, `~`, `++`, `--`, Cast i `await` są nazywane operatorami jednoargumentowymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="bd998-1492">Jeśli argument operacji *unary_expression* ma typ czasu kompilacji `dynamic`, jest on dynamicznie powiązany ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-1493">W takim przypadku typem czasu kompilacji *unary_expression* jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania przy użyciu typu czasu wykonywania argumentu operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="bd998-1494">Operator warunkowy o wartości null</span><span class="sxs-lookup"><span data-stu-id="bd998-1494">Null-conditional operator</span></span>

<span data-ttu-id="bd998-1495">Operator warunkowy null stosuje listę operacji do operandu tylko wtedy, gdy ten operand ma wartość różną od null.</span><span class="sxs-lookup"><span data-stu-id="bd998-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="bd998-1496">W przeciwnym razie wynikiem zastosowania operatora jest `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="bd998-1497">Lista operacji może obejmować dostęp do składowych i operacje dostępu do elementów (które mogą być w stanie null — warunkowo), a także wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="bd998-1498">Na przykład wyrażenie `a.b?[0]?.c()` to *null_conditional_expression* z *primary_expression* `a.b` i *null_conditional_operations* `?[0]` (dostęp do elementu o wartości null), `?.c` (składowa o wartości null dostęp) i `()` (wywołanie).</span><span class="sxs-lookup"><span data-stu-id="bd998-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="bd998-1499">Dla *null_conditional_expression* `E` z *primary_expression* `P`, pozwól, aby `E0` było wyrażeniem, które można usunąć poprzez jednokrotne usunięcie wiodącego `?` z każdej *null_conditional_operations* `E` ma jeden.</span><span class="sxs-lookup"><span data-stu-id="bd998-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="bd998-1500">Koncepcyjnie, `E0` jest wyrażeniem, które zostanie ocenione, jeśli żadna z testów null nie jest reprezentowana przez `?`Ss Znajdź `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="bd998-1501">Ponadto Zezwól `E1` na wyrażenie uzyskane przez usunięcie wiodącego `?` z tylko pierwszej *null_conditional_operations* w `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="bd998-1502">Może to prowadzić do *wyrażenia podstawowego* (jeśli istnieje tylko jeden `?`) lub do innego *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="bd998-1503">Na przykład jeśli `E` jest wyrażeniem `a.b?[0]?.c()`, wówczas `E0` jest wyrażeniem `a.b[0].c()` i `E1` jest wyrażeniem `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="bd998-1504">Jeśli `E0` jest sklasyfikowany jako Nothing, wówczas `E` jest sklasyfikowany jako Nothing.</span><span class="sxs-lookup"><span data-stu-id="bd998-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="bd998-1505">W przeciwnym razie E jest klasyfikowane jako wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="bd998-1506">`E0` i `E1` są używane do określenia znaczenia `E`:</span><span class="sxs-lookup"><span data-stu-id="bd998-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="bd998-1507">Jeśli `E` występuje jako *statement_expression* znaczenie `E` jest takie samo jak instrukcja</span><span class="sxs-lookup"><span data-stu-id="bd998-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="bd998-1508">z tą różnicą, że P jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="bd998-1509">W przeciwnym razie, jeśli `E0` jest sklasyfikowany jako nie wystąpi błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="bd998-1510">W przeciwnym razie niech `T0` będzie typem `E0`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="bd998-1511">Jeśli `T0` jest parametrem typu, który nie jest znany jako typ referencyjny lub typ wartości niedopuszczający wartości null, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="bd998-1512">Jeśli `T0` to typ wartości niedopuszczający wartości null, typ `E` jest `T0?`, a znaczenie `E` jest takie samo jak</span><span class="sxs-lookup"><span data-stu-id="bd998-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="bd998-1513">z tą różnicą, że `P` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="bd998-1514">W przeciwnym razie typ E jest T0, a znaczenie E jest takie samo jak</span><span class="sxs-lookup"><span data-stu-id="bd998-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="bd998-1515">z tą różnicą, że `P` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="bd998-1516">Jeśli `E1` to *null_conditional_expression*, następnie te reguły są stosowane ponownie, zagnieżdżając testy dla `null` do momentu dalszej `?`, a wyrażenie zostało zredukowane w dół do wyrażenia podstawowego `E0`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="bd998-1517">Na przykład, jeśli wyrażenie `a.b?[0]?.c()` występuje jako wyrażenie instrukcji, jak w instrukcji:</span><span class="sxs-lookup"><span data-stu-id="bd998-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="bd998-1518">jego znaczenie jest równoważne z:</span><span class="sxs-lookup"><span data-stu-id="bd998-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="bd998-1519">co ponownie jest równoważne:</span><span class="sxs-lookup"><span data-stu-id="bd998-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="bd998-1520">Z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="bd998-1521">Jeśli występuje w kontekście, w którym jego wartość jest używana, jak w:</span><span class="sxs-lookup"><span data-stu-id="bd998-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="bd998-1522">przy założeniu, że typ końcowego wywołania nie jest typem wartości niedopuszczających wartości null, jego znaczenie jest równoważne:</span><span class="sxs-lookup"><span data-stu-id="bd998-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="bd998-1523">z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="bd998-1524">Wyrażenia warunkowe o wartości null jako inicjatory projekcji</span><span class="sxs-lookup"><span data-stu-id="bd998-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="bd998-1525">Wyrażenie warunkowe o wartości null jest dozwolone tylko jako *member_declarator* w *anonymous_object_creation_expression* ([wyrażenie anonimowego tworzenia obiektów](expressions.md#anonymous-object-creation-expressions)), jeśli zostanie zakończone z dostępem do elementu członkowskiego (opcjonalnie o wartości null).</span><span class="sxs-lookup"><span data-stu-id="bd998-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="bd998-1526">Gramatycznie ten wymóg może być wyrażony jako:</span><span class="sxs-lookup"><span data-stu-id="bd998-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="bd998-1527">Jest to specjalny przypadek gramatyczny *null_conditional_expression* powyżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="bd998-1528">Produkcja dla *member_declarator* w [wyrażeniach tworzenia obiektów anonimowych](expressions.md#anonymous-object-creation-expressions) , a następnie zawiera tylko *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="bd998-1529">Wyrażenia warunkowe o wartości null jako wyrażenia instrukcji</span><span class="sxs-lookup"><span data-stu-id="bd998-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="bd998-1530">Wyrażenie warunkowe o wartości null jest dozwolone tylko jako *statement_expression* ([instrukcje wyrażenia](statements.md#expression-statements)), jeśli zostanie zakończone wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="bd998-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="bd998-1531">Gramatycznie ten wymóg może być wyrażony jako:</span><span class="sxs-lookup"><span data-stu-id="bd998-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="bd998-1532">Jest to specjalny przypadek gramatyczny *null_conditional_expression* powyżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="bd998-1533">Produkcja for *statement_expression* w [instrukcjach Expression](statements.md#expression-statements) obejmuje tylko *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="bd998-1534">Jednoargumentowy operator plus</span><span class="sxs-lookup"><span data-stu-id="bd998-1534">Unary plus operator</span></span>

<span data-ttu-id="bd998-1535">Aby można było wykonać operację `+x`, naliczanie przeciążenia operatora jednoargumentowego ([jednoargumentowe rozpoznanie](expressions.md#unary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1536">Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="bd998-1537">Wstępnie zdefiniowane operatory jednoargumentowe Plus są następujące:</span><span class="sxs-lookup"><span data-stu-id="bd998-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="bd998-1538">Dla każdego z tych operatorów wynik jest po prostu wartością operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="bd998-1539">Jednoargumentowy operator minus</span><span class="sxs-lookup"><span data-stu-id="bd998-1539">Unary minus operator</span></span>

<span data-ttu-id="bd998-1540">Aby można było wykonać operację `-x`, naliczanie przeciążenia operatora jednoargumentowego ([jednoargumentowe rozpoznanie](expressions.md#unary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1541">Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="bd998-1542">Wstępnie zdefiniowane operatory negacji to:</span><span class="sxs-lookup"><span data-stu-id="bd998-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="bd998-1543">Negacja liczby całkowitej:</span><span class="sxs-lookup"><span data-stu-id="bd998-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="bd998-1544">Wynik jest obliczany przez odjęcie wartości `x` od zera.</span><span class="sxs-lookup"><span data-stu-id="bd998-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="bd998-1545">Jeśli wartość `x` jest najmniejszą reprezentacją typu operandu (-2 ^ 31 dla `int` lub-2 ^ 63 dla `long`), matematyczne Negacja `x` nie jest możliwe do zaprezentowania w ramach typu operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="bd998-1546">Jeśli ten problem występuje w kontekście `checked`, zostanie zgłoszony `System.OverflowException`; Jeśli występuje w kontekście `unchecked`, wynik jest wartością operandu, a przepełnienie nie zostanie zgłoszone.</span><span class="sxs-lookup"><span data-stu-id="bd998-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="bd998-1547">Jeśli argument operacji operatora negacji jest typu `uint`, jest konwertowany na typ `long`, a typ wyniku to `long`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="bd998-1548">Wyjątkiem jest reguła, która zezwala na zapisanie wartości `int`-2147483648 (-2 ^ 31) jako dziesiętnego literału liczb całkowitych ([literałów liczb całkowitych](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="bd998-1549">Jeśli argument operacji operatora negacji jest typu `ulong`, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="bd998-1550">Wyjątkiem jest reguła zezwalająca na zapis wartości `long` zakresu od (-2 ^ 63) jako dziesiętnego literału liczb całkowitych ([literałów liczb całkowitych](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="bd998-1551">Negacja zmiennoprzecinkowa:</span><span class="sxs-lookup"><span data-stu-id="bd998-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="bd998-1552">Wynikiem jest wartość `x` z odwróconym znakiem.</span><span class="sxs-lookup"><span data-stu-id="bd998-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="bd998-1553">Jeśli `x` jest NaN, wynik jest również NaN.</span><span class="sxs-lookup"><span data-stu-id="bd998-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="bd998-1554">Negacja dziesiętna:</span><span class="sxs-lookup"><span data-stu-id="bd998-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="bd998-1555">Wynik jest obliczany przez odjęcie wartości `x` od zera.</span><span class="sxs-lookup"><span data-stu-id="bd998-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="bd998-1556">Negacja dziesiętna jest równoznaczna z użyciem jednoargumentowego operatora minus typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="bd998-1557">Operator logiczny negacji</span><span class="sxs-lookup"><span data-stu-id="bd998-1557">Logical negation operator</span></span>

<span data-ttu-id="bd998-1558">Aby można było wykonać operację `!x`, naliczanie przeciążenia operatora jednoargumentowego ([jednoargumentowe rozpoznanie](expressions.md#unary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1559">Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="bd998-1560">Istnieje tylko jeden wstępnie zdefiniowany operator logiczny negacji:</span><span class="sxs-lookup"><span data-stu-id="bd998-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="bd998-1561">Ten operator oblicza logiczne Negacja operandu: Jeśli argument jest `true`, wynik jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="bd998-1562">Jeśli argument jest `false`, wynik jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="bd998-1563">Operator dopełnienia bitowego</span><span class="sxs-lookup"><span data-stu-id="bd998-1563">Bitwise complement operator</span></span>

<span data-ttu-id="bd998-1564">Aby można było wykonać operację `~x`, naliczanie przeciążenia operatora jednoargumentowego ([jednoargumentowe rozpoznanie](expressions.md#unary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1565">Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="bd998-1566">Wstępnie zdefiniowane operatory dopełnienia bitowego są następujące:</span><span class="sxs-lookup"><span data-stu-id="bd998-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="bd998-1567">W przypadku każdego z tych operatorów wynik operacji jest bitowym uzupełnieniem `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="bd998-1568">Każdy typ wyliczeniowy `E` niejawnie zapewnia następujący operator dopełnienia bitowego:</span><span class="sxs-lookup"><span data-stu-id="bd998-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="bd998-1569">Wynik oceny `~x`, gdzie `x` jest wyrażeniem typu wyliczenia `E` z typem podstawowym `U`, jest dokładnie taka sama jak Ocena `(E)(~(U)x)`, z tą różnicą, że konwersja do `E` jest zawsze wykonywana tak jak w kontekście `unchecked` ( [Sprawdzone i niesprawdzone operatory](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="bd998-1570">Operatory prefiksów inkrementacji i dekrementacji</span><span class="sxs-lookup"><span data-stu-id="bd998-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="bd998-1571">Operand operacji zwiększania lub zmniejszania prefiksu musi być wyrażeniem sklasyfikowanym jako zmienna, dostępem do właściwości lub dostępem indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="bd998-1572">Wynik operacji jest wartością tego samego typu co argument operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="bd998-1573">Jeśli operand operacji przyrostu lub zmniejszania prefiksu jest właściwością lub indeksatorem, właściwość lub indeksator musi mieć metodę dostępu `get` i `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="bd998-1574">W przeciwnym razie wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-1575">Metoda rozpoznawania przeciążenia operatora jednoargumentowego (Metoda[rozpoznawania przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) została zastosowana w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1576">Wstępnie zdefiniowane operatory `++` i `--` są dostępne dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 i dowolnego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="bd998-1577">Wstępnie zdefiniowane operatory `++` zwracają wartość wygenerowaną przez dodanie 1 do operandu, a wstępnie zdefiniowane operatory `--` zwracają wartość wygenerowaną przez odjęcie 1 od operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="bd998-1578">W kontekście `checked`, jeśli wynik tego dodawania lub odejmowania znajduje się poza zakresem typu wyników, a typ wyniku jest typem całkowitym lub typem wyliczeniowym, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="bd998-1579">Przetwarzanie w czasie wykonywania operacji przyrostu lub zmniejszania prefiksu w formularzu `++x` lub `--x` składa się z następujących kroków:</span><span class="sxs-lookup"><span data-stu-id="bd998-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="bd998-1580">Jeśli `x` jest klasyfikowane jako zmienna:</span><span class="sxs-lookup"><span data-stu-id="bd998-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="bd998-1581">`x` jest oceniane, aby utworzyć zmienną.</span><span class="sxs-lookup"><span data-stu-id="bd998-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="bd998-1582">Wybrany operator jest wywoływany z wartością `x` jako argumentem.</span><span class="sxs-lookup"><span data-stu-id="bd998-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="bd998-1583">Wartość zwrócona przez operator jest przechowywana w lokalizacji podawanej przez oszacowanie `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="bd998-1584">Wartość zwracana przez operatora jest wynikiem operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="bd998-1585">Jeśli `x` jest sklasyfikowany jako dostęp do właściwości lub indeksatora:</span><span class="sxs-lookup"><span data-stu-id="bd998-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="bd998-1586">Wyrażenie wystąpienia (jeśli `x` nie jest `static`) i lista argumentów (jeśli `x` jest dostępem indeksatora) skojarzonym z `x` są oceniane, a wyniki są używane w kolejnych wywołaniach metody dostępu `get` i `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="bd998-1587">Metoda dostępu `get` elementu `x` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="bd998-1588">Wybrany operator jest wywoływany z wartością zwracaną przez metodę dostępu `get` jako argumentem.</span><span class="sxs-lookup"><span data-stu-id="bd998-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="bd998-1589">Metoda dostępu `set` elementu `x` jest wywoływana z wartością zwracaną przez operator jako argument `value`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="bd998-1590">Wartość zwracana przez operatora jest wynikiem operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="bd998-1591">Operatory `++` i `--` obsługują również notację przyrostkową ([Operatory przyrostu i zmniejszania](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="bd998-1592">Zwykle wynik `x++` lub `x--` jest wartością `x` przed operacją, a wynikiem `++x` lub `--x` jest wartość `x` po operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="bd998-1593">W obu przypadkach `x` sama wartość jest taka sama po operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="bd998-1594">Implementację `operator++` lub `operator--` można wywołać przy użyciu notacji przyrostkowej lub prefiksowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="bd998-1595">Nie można mieć odrębnych implementacji operatora dla dwóch notacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="bd998-1596">Wyrażenia rzutowania</span><span class="sxs-lookup"><span data-stu-id="bd998-1596">Cast expressions</span></span>

<span data-ttu-id="bd998-1597">Element *cast_expression* jest używany do jawnej konwersji wyrażenia na dany typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="bd998-1598">*Cast_expression* formularza `(T)E`, gdzie `T` jest *typem* i `E` jest *unary_expression*, wykonuje jawną konwersję ([jawne konwersje](conversions.md#explicit-conversions)) wartości `E` w celu wpisania `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="bd998-1599">Jeśli żadna jawna konwersja nie istnieje z `E` do `T`, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="bd998-1600">W przeciwnym razie wynik jest wartością wygenerowaną przez konwersję jawną.</span><span class="sxs-lookup"><span data-stu-id="bd998-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="bd998-1601">Wynik jest zawsze klasyfikowany jako wartość, nawet jeśli `E` oznacza zmienną.</span><span class="sxs-lookup"><span data-stu-id="bd998-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="bd998-1602">Gramatyka dla *cast_expression* prowadzi do pewnej składni niejasności.</span><span class="sxs-lookup"><span data-stu-id="bd998-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="bd998-1603">Na przykład wyrażenie `(x)-y` może być interpretowane jako *cast_expression* (rzutowanie `-y` do typu `x`) lub jako *additive_expression* połączone z *parenthesized_expression* (które oblicza wartość `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="bd998-1604">Aby rozwiązać *cast_expression* niejasności, istnieje następująca reguła: Sekwencja jednego lub więcej *tokenów*([białych znaków](lexical-structure.md#white-space)) ujętych w nawiasy jest uważana za początek *cast_expression* tylko wtedy, gdy co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="bd998-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="bd998-1605">Sekwencja tokenów jest poprawną gramatyką dla *typu*, ale nie dla *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="bd998-1606">Sekwencja tokenów jest poprawną gramatyką dla *typu*, a token bezpośrednio po nawiasach zamykających jest tokenem "`~`", tokenem "`!`", tokenem "`(`", *identyfikatorem* ([sekwencjami ucieczki znaków Unicode ](lexical-structure.md#unicode-character-escape-sequences)), *literał* ([literały](lexical-structure.md#literals)) lub *słowo kluczowe* ([Keywords](lexical-structure.md#keywords)), z wyjątkiem 0 i 1.</span><span class="sxs-lookup"><span data-stu-id="bd998-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="bd998-1607">Termin "poprawna Gramatyka" oznacza tylko, że sekwencja tokenów musi być zgodna z konkretną produkcją gramatyczną.</span><span class="sxs-lookup"><span data-stu-id="bd998-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="bd998-1608">W szczególności nie należy traktować faktycznego znaczenia identyfikatorów składników.</span><span class="sxs-lookup"><span data-stu-id="bd998-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="bd998-1609">Na przykład jeśli `x` i `y` są identyfikatorami, `x.y` jest poprawną gramatyką dla typu, nawet jeśli `x.y` nie wskazuje, że typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="bd998-1610">Z poziomu reguły uściślania, która następuje, jeśli `x` i `y` są identyfikatorami, `(x)y`, `(x)(y)` i `(x)(-y)` są *cast_expression*s, ale `(x)-y` nie jest, nawet jeśli `x` identyfikuje typ.</span><span class="sxs-lookup"><span data-stu-id="bd998-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="bd998-1611">Jeśli jednak `x` to słowo kluczowe, które identyfikuje wstępnie zdefiniowany typ (na przykład `int`), a następnie wszystkie cztery formy są *cast_expression*s (ponieważ takie słowo kluczowe nie może być wyrażeniem przez siebie).</span><span class="sxs-lookup"><span data-stu-id="bd998-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="bd998-1612">Wyrażenia await</span><span class="sxs-lookup"><span data-stu-id="bd998-1612">Await expressions</span></span>

<span data-ttu-id="bd998-1613">Operator await jest używany do zawieszania oceny otaczającej funkcji asynchronicznej do momentu zakończenia operacji asynchronicznej reprezentowanej przez operand.</span><span class="sxs-lookup"><span data-stu-id="bd998-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="bd998-1614">Element *await_expression* jest dozwolony tylko w treści funkcji asynchronicznej ([Iteratory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="bd998-1615">W najbliższej otaczającej funkcji asynchronicznej *await_expression* może nie wystąpić w tych miejscach:</span><span class="sxs-lookup"><span data-stu-id="bd998-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="bd998-1616">Wewnątrz zagnieżdżonej funkcji anonimowej (innej niż Async)</span><span class="sxs-lookup"><span data-stu-id="bd998-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="bd998-1617">Wewnątrz bloku elementu *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="bd998-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="bd998-1618">W niebezpiecznym kontekście</span><span class="sxs-lookup"><span data-stu-id="bd998-1618">In an unsafe context</span></span>

<span data-ttu-id="bd998-1619">Należy zauważyć, że element *await_expression* nie może wystąpić w większości miejsc w *query_expression*, ponieważ są składniowo przekształcone w celu użycia nieasynchronicznych wyrażeń lambda.</span><span class="sxs-lookup"><span data-stu-id="bd998-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="bd998-1620">W funkcji asynchronicznej nie można użyć `await` jako identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="bd998-1621">W związku z tym nie istnieje niejednoznaczność składni między wyrażeniami await i różnymi wyrażeniami obejmującymi identyfikatory.</span><span class="sxs-lookup"><span data-stu-id="bd998-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="bd998-1622">Poza funkcjami asynchronicznymi `await` pełni rolę normalnego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="bd998-1623">Operand elementu *await_expression* jest nazywany ***zadaniem***.</span><span class="sxs-lookup"><span data-stu-id="bd998-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="bd998-1624">Reprezentuje operację asynchroniczną, która może być lub nie można jej zakończyć w chwili, gdy *await_expression* jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="bd998-1625">Celem operatora await jest wstrzymanie wykonywania otaczającej funkcji asynchronicznej do momentu zakończenia oczekującego zadania, a następnie uzyskanie wyniku.</span><span class="sxs-lookup"><span data-stu-id="bd998-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="bd998-1626">Wyrażenia oczekujące</span><span class="sxs-lookup"><span data-stu-id="bd998-1626">Awaitable expressions</span></span>

<span data-ttu-id="bd998-1627">Zadanie wyrażenia await jest wymagane, aby można było ***oczekiwać***.</span><span class="sxs-lookup"><span data-stu-id="bd998-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="bd998-1628">Wyrażenie `t` jest oczekiwane, jeśli jedno z następujących zawiera:</span><span class="sxs-lookup"><span data-stu-id="bd998-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="bd998-1629">`t` jest typu czasu kompilacji `dynamic`</span><span class="sxs-lookup"><span data-stu-id="bd998-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="bd998-1630">`t` ma dostępne wystąpienie lub metodę rozszerzającą o nazwie `GetAwaiter` bez parametrów i bez parametrów typu i typem zwracanym `A`, dla którego wszystkie następujące wstrzymano:</span><span class="sxs-lookup"><span data-stu-id="bd998-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="bd998-1631">`A` implementuje interfejs `System.Runtime.CompilerServices.INotifyCompletion` (dalej znany jako `INotifyCompletion` dla zwięzłości)</span><span class="sxs-lookup"><span data-stu-id="bd998-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="bd998-1632">`A` ma dostępną, czytelną Właściwość wystąpienia `IsCompleted` typu `bool`</span><span class="sxs-lookup"><span data-stu-id="bd998-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="bd998-1633">`A` ma dostępną metodę wystąpienia `GetResult` bez parametrów i bez parametrów typu</span><span class="sxs-lookup"><span data-stu-id="bd998-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="bd998-1634">Metoda `GetAwaiter` ma na celu uzyskanie ***oczekiwania*** dla zadania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="bd998-1635">Typ `A` jest określany jako ***Typ oczekiwania*** dla wyrażenia await.</span><span class="sxs-lookup"><span data-stu-id="bd998-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="bd998-1636">Właściwość `IsCompleted` ma na celu określenie, czy zadanie zostało już ukończone.</span><span class="sxs-lookup"><span data-stu-id="bd998-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="bd998-1637">Jeśli tak, nie ma potrzeby wstrzymania oceny.</span><span class="sxs-lookup"><span data-stu-id="bd998-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="bd998-1638">Metoda `INotifyCompletion.OnCompleted` polega na zarejestrowaniu "kontynuacji" do zadania; tj. delegat (typu `System.Action`), który zostanie wywołany po zakończeniu zadania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="bd998-1639">Metoda `GetResult` umożliwia uzyskanie wyniku zadania po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="bd998-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="bd998-1640">Ten wynik może powieść się, prawdopodobnie z wartością wyniku lub może być wyjątek, który jest generowany przez metodę `GetResult`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="bd998-1641">Klasyfikacja wyrażeń await</span><span class="sxs-lookup"><span data-stu-id="bd998-1641">Classification of await expressions</span></span>

<span data-ttu-id="bd998-1642">Wyrażenie `await t` jest sklasyfikowane tak samo jak wyrażenie `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="bd998-1643">Tak więc, jeśli typ zwracany `GetResult` jest `void`, *await_expression* jest klasyfikowany jako Nothing.</span><span class="sxs-lookup"><span data-stu-id="bd998-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="bd998-1644">Jeśli ma typ zwracany inny niż void `T`, *await_expression* jest klasyfikowany jako wartość typu `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="bd998-1645">Ocena czasu wykonywania dla wyrażeń await</span><span class="sxs-lookup"><span data-stu-id="bd998-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="bd998-1646">W czasie wykonywania wyrażenie `await t` jest oceniane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="bd998-1647">Element awaiter `a` jest uzyskiwany przez obliczenie wyrażenia `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="bd998-1648">@No__t-0 `b` jest uzyskiwany poprzez obliczenie wyrażenia `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="bd998-1649">Jeśli `b` to `false`, Ocena zależy od tego, czy `a` implementuje interfejs `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (dalej znany jako `ICriticalNotifyCompletion` dla zwięzłości).</span><span class="sxs-lookup"><span data-stu-id="bd998-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="bd998-1650">To sprawdzanie jest wykonywane w czasie wiązania; tj. w czasie wykonywania, jeśli `a` ma typ czasu kompilacji `dynamic`, a w przeciwnym razie w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="bd998-1651">Niech `r` oznacza delegata wznawiania ([Iteratory](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="bd998-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="bd998-1652">Jeśli `a` nie implementuje `ICriticalNotifyCompletion`, zostanie obliczone wyrażenie `(a as (INotifyCompletion)).OnCompleted(r)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="bd998-1653">Jeśli `a` implementuje `ICriticalNotifyCompletion`, zostanie obliczone wyrażenie `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="bd998-1654">Szacowanie jest zawieszone, a sterowanie jest zwracane do bieżącego obiektu wywołującego funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="bd998-1655">Bezpośrednio po (jeśli `b` było `true`) lub po późniejszym wywołaniu delegata wznawiania (jeśli `b` była `false`), obliczane jest wyrażenie `(a).GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="bd998-1656">Jeśli zwraca wartość, ta wartość jest wynikiem *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="bd998-1657">W przeciwnym razie wynik nie będzie miał żadnego skutku.</span><span class="sxs-lookup"><span data-stu-id="bd998-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="bd998-1658">Implementacja metody interfejsu w programie await `INotifyCompletion.OnCompleted` i `ICriticalNotifyCompletion.UnsafeOnCompleted` powinna spowodować, że delegat `r` jest wywoływany najwyżej raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="bd998-1659">W przeciwnym razie zachowanie otaczającej funkcji asynchronicznej jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="bd998-1660">Operatory arytmetyczne</span><span class="sxs-lookup"><span data-stu-id="bd998-1660">Arithmetic operators</span></span>

<span data-ttu-id="bd998-1661">Operatory `*`, `/`, `%`, `+` i `-` są nazywane operatorami arytmetycznymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="bd998-1662">Jeśli argument operacji operatora arytmetycznego ma typ czasu kompilacji `dynamic`, to wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-1663">W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania przy użyciu typu czasu wykonywania tych argumentów, które mają typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="bd998-1664">Operator mnożenia</span><span class="sxs-lookup"><span data-stu-id="bd998-1664">Multiplication operator</span></span>

<span data-ttu-id="bd998-1665">Aby można było wykonać operację `x * y`, rozpoznawanie przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1666">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-1667">Poniżej przedstawiono wstępnie zdefiniowane operatory mnożenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="bd998-1668">Operator All Oblicza iloczyn `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="bd998-1669">Mnożenie całkowite:</span><span class="sxs-lookup"><span data-stu-id="bd998-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="bd998-1670">W kontekście `checked`, jeśli produkt znajduje się poza zakresem typu wyników, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-1671">W kontekście `unchecked` przepełnienia nie są raportowane i wszelkie znaczące bity o dużej kolejności poza zakresem typu wynik są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="bd998-1672">Mnożenie zmiennoprzecinkowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="bd998-1673">Produkt jest obliczany zgodnie z regułami arytmetycznymi IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="bd998-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="bd998-1674">W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN.</span><span class="sxs-lookup"><span data-stu-id="bd998-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="bd998-1675">W tabeli `x` i `y` są dodatnimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="bd998-1676">`z` to wynik `x * y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="bd998-1677">Jeśli wynik jest za duży dla typu docelowego, `z` to nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="bd998-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="bd998-1678">Jeśli wynik jest za mały dla typu docelowego, `z` wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="bd998-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="bd998-1679">+y</span></span>   | <span data-ttu-id="bd998-1680">-y</span><span class="sxs-lookup"><span data-stu-id="bd998-1680">-y</span></span>   | <span data-ttu-id="bd998-1681">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1681">+0</span></span>  | <span data-ttu-id="bd998-1682">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1682">-0</span></span>  | <span data-ttu-id="bd998-1683">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1683">+inf</span></span> | <span data-ttu-id="bd998-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1684">-inf</span></span> | <span data-ttu-id="bd998-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1685">NaN</span></span> | 
   | <span data-ttu-id="bd998-1686">+x</span><span class="sxs-lookup"><span data-stu-id="bd998-1686">+x</span></span>   | <span data-ttu-id="bd998-1687">+z</span><span class="sxs-lookup"><span data-stu-id="bd998-1687">+z</span></span>   | <span data-ttu-id="bd998-1688">-z</span><span class="sxs-lookup"><span data-stu-id="bd998-1688">-z</span></span>   | <span data-ttu-id="bd998-1689">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1689">+0</span></span>  | <span data-ttu-id="bd998-1690">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1690">-0</span></span>  | <span data-ttu-id="bd998-1691">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1691">+inf</span></span> | <span data-ttu-id="bd998-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1692">-inf</span></span> | <span data-ttu-id="bd998-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1693">NaN</span></span> | 
   | <span data-ttu-id="bd998-1694">-x</span><span class="sxs-lookup"><span data-stu-id="bd998-1694">-x</span></span>   | <span data-ttu-id="bd998-1695">-z</span><span class="sxs-lookup"><span data-stu-id="bd998-1695">-z</span></span>   | <span data-ttu-id="bd998-1696">+z</span><span class="sxs-lookup"><span data-stu-id="bd998-1696">+z</span></span>   | <span data-ttu-id="bd998-1697">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1697">-0</span></span>  | <span data-ttu-id="bd998-1698">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1698">+0</span></span>  | <span data-ttu-id="bd998-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1699">-inf</span></span> | <span data-ttu-id="bd998-1700">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1700">+inf</span></span> | <span data-ttu-id="bd998-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1701">NaN</span></span> | 
   | <span data-ttu-id="bd998-1702">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1702">+0</span></span>   | <span data-ttu-id="bd998-1703">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1703">+0</span></span>   | <span data-ttu-id="bd998-1704">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1704">-0</span></span>   | <span data-ttu-id="bd998-1705">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1705">+0</span></span>  | <span data-ttu-id="bd998-1706">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1706">-0</span></span>  | <span data-ttu-id="bd998-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1707">NaN</span></span>  | <span data-ttu-id="bd998-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1708">NaN</span></span>  | <span data-ttu-id="bd998-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1709">NaN</span></span> | 
   | <span data-ttu-id="bd998-1710">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1710">-0</span></span>   | <span data-ttu-id="bd998-1711">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1711">-0</span></span>   | <span data-ttu-id="bd998-1712">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1712">+0</span></span>   | <span data-ttu-id="bd998-1713">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1713">-0</span></span>  | <span data-ttu-id="bd998-1714">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1714">+0</span></span>  | <span data-ttu-id="bd998-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1715">NaN</span></span>  | <span data-ttu-id="bd998-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1716">NaN</span></span>  | <span data-ttu-id="bd998-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1717">NaN</span></span> | 
   | <span data-ttu-id="bd998-1718">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1718">+inf</span></span> | <span data-ttu-id="bd998-1719">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1719">+inf</span></span> | <span data-ttu-id="bd998-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1720">-inf</span></span> | <span data-ttu-id="bd998-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1721">NaN</span></span> | <span data-ttu-id="bd998-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1722">NaN</span></span> | <span data-ttu-id="bd998-1723">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1723">+inf</span></span> | <span data-ttu-id="bd998-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1724">-inf</span></span> | <span data-ttu-id="bd998-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1725">NaN</span></span> | 
   | <span data-ttu-id="bd998-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1726">-inf</span></span> | <span data-ttu-id="bd998-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1727">-inf</span></span> | <span data-ttu-id="bd998-1728">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1728">+inf</span></span> | <span data-ttu-id="bd998-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1729">NaN</span></span> | <span data-ttu-id="bd998-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1730">NaN</span></span> | <span data-ttu-id="bd998-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1731">-inf</span></span> | <span data-ttu-id="bd998-1732">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1732">+inf</span></span> | <span data-ttu-id="bd998-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1733">NaN</span></span> | 
   | <span data-ttu-id="bd998-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1734">NaN</span></span>  | <span data-ttu-id="bd998-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1735">NaN</span></span>  | <span data-ttu-id="bd998-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1736">NaN</span></span>  | <span data-ttu-id="bd998-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1737">NaN</span></span> | <span data-ttu-id="bd998-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1738">NaN</span></span> | <span data-ttu-id="bd998-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1739">NaN</span></span>  | <span data-ttu-id="bd998-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1740">NaN</span></span>  | <span data-ttu-id="bd998-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1741">NaN</span></span> | 

*  <span data-ttu-id="bd998-1742">Mnożenie dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="bd998-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="bd998-1743">Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-1744">Jeśli wartość wynikowa jest zbyt mała do reprezentowania w formacie `decimal`, wynik wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="bd998-1745">Skala wyniku, przed zaokrągleniem, jest sumą skali dwóch operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="bd998-1746">Mnożenie dziesiętne jest równoważne użyciu operator mnożenia typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="bd998-1747">Operator dzielenia</span><span class="sxs-lookup"><span data-stu-id="bd998-1747">Division operator</span></span>

<span data-ttu-id="bd998-1748">Aby można było wykonać operację `x / y`, rozpoznawanie przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1749">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-1750">Poniżej znajdują się wstępnie zdefiniowane operatory dzielenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="bd998-1751">Operator All Oblicza iloraz wartości `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="bd998-1752">Dzielenie liczb całkowitych:</span><span class="sxs-lookup"><span data-stu-id="bd998-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="bd998-1753">Jeśli wartość argumentu po prawej stronie jest równa zero, zostanie zgłoszony `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="bd998-1754">Podział zaokrągla wynik do zera.</span><span class="sxs-lookup"><span data-stu-id="bd998-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="bd998-1755">W związku z tym wartość bezwzględna wyniku to największa możliwa liczba całkowita, która jest mniejsza lub równa wartości bezwzględnej ilorazu dwóch operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="bd998-1756">Wynik ma wartość zero lub wartość dodatnią, gdy dwa operandy mają ten sam znak i zero lub wartość ujemną, gdy dwa operandy mają przeciwne znaki.</span><span class="sxs-lookup"><span data-stu-id="bd998-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="bd998-1757">Jeśli Lewy argument operacji to najmniejszy możliwy do zaprezentowania wartość `int` lub `long`, a prawy operand to `-1`, występuje przepełnienie.</span><span class="sxs-lookup"><span data-stu-id="bd998-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="bd998-1758">W kontekście `checked` powoduje to wystąpienie `System.ArithmeticException` (lub jego podklasy) do wyrzucania.</span><span class="sxs-lookup"><span data-stu-id="bd998-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="bd998-1759">W kontekście `unchecked` jest to zdefiniowane przez implementację w zależności od tego, czy występuje `System.ArithmeticException` (lub jego podklasa), czy przepełnienie zostanie nieraportowane z wartością obliczoną argumentu operacji po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="bd998-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="bd998-1760">Dzielenie zmiennoprzecinkowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="bd998-1761">Iloraz jest obliczany zgodnie z regułami arytmetycznymi IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="bd998-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="bd998-1762">W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN.</span><span class="sxs-lookup"><span data-stu-id="bd998-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="bd998-1763">W tabeli `x` i `y` są dodatnimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="bd998-1764">`z` to wynik `x / y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="bd998-1765">Jeśli wynik jest za duży dla typu docelowego, `z` to nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="bd998-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="bd998-1766">Jeśli wynik jest za mały dla typu docelowego, `z` wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="bd998-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="bd998-1767">+y</span></span>   | <span data-ttu-id="bd998-1768">-y</span><span class="sxs-lookup"><span data-stu-id="bd998-1768">-y</span></span>   | <span data-ttu-id="bd998-1769">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1769">+0</span></span>   | <span data-ttu-id="bd998-1770">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1770">-0</span></span>   | <span data-ttu-id="bd998-1771">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1771">+inf</span></span> | <span data-ttu-id="bd998-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1772">-inf</span></span> | <span data-ttu-id="bd998-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1773">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1774">+x</span><span class="sxs-lookup"><span data-stu-id="bd998-1774">+x</span></span>   | <span data-ttu-id="bd998-1775">+z</span><span class="sxs-lookup"><span data-stu-id="bd998-1775">+z</span></span>   | <span data-ttu-id="bd998-1776">-z</span><span class="sxs-lookup"><span data-stu-id="bd998-1776">-z</span></span>   | <span data-ttu-id="bd998-1777">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1777">+inf</span></span> | <span data-ttu-id="bd998-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1778">-inf</span></span> | <span data-ttu-id="bd998-1779">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1779">+0</span></span>   | <span data-ttu-id="bd998-1780">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1780">-0</span></span>   | <span data-ttu-id="bd998-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1781">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1782">-x</span><span class="sxs-lookup"><span data-stu-id="bd998-1782">-x</span></span>   | <span data-ttu-id="bd998-1783">-z</span><span class="sxs-lookup"><span data-stu-id="bd998-1783">-z</span></span>   | <span data-ttu-id="bd998-1784">+z</span><span class="sxs-lookup"><span data-stu-id="bd998-1784">+z</span></span>   | <span data-ttu-id="bd998-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1785">-inf</span></span> | <span data-ttu-id="bd998-1786">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1786">+inf</span></span> | <span data-ttu-id="bd998-1787">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1787">-0</span></span>   | <span data-ttu-id="bd998-1788">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1788">+0</span></span>   | <span data-ttu-id="bd998-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1789">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1790">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1790">+0</span></span>   | <span data-ttu-id="bd998-1791">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1791">+0</span></span>   | <span data-ttu-id="bd998-1792">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1792">-0</span></span>   | <span data-ttu-id="bd998-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1793">NaN</span></span>  | <span data-ttu-id="bd998-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1794">NaN</span></span>  | <span data-ttu-id="bd998-1795">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1795">+0</span></span>   | <span data-ttu-id="bd998-1796">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1796">-0</span></span>   | <span data-ttu-id="bd998-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1797">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1798">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1798">-0</span></span>   | <span data-ttu-id="bd998-1799">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1799">-0</span></span>   | <span data-ttu-id="bd998-1800">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1800">+0</span></span>   | <span data-ttu-id="bd998-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1801">NaN</span></span>  | <span data-ttu-id="bd998-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1802">NaN</span></span>  | <span data-ttu-id="bd998-1803">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1803">-0</span></span>   | <span data-ttu-id="bd998-1804">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1804">+0</span></span>   | <span data-ttu-id="bd998-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1805">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1806">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1806">+inf</span></span> | <span data-ttu-id="bd998-1807">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1807">+inf</span></span> | <span data-ttu-id="bd998-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1808">-inf</span></span> | <span data-ttu-id="bd998-1809">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1809">+inf</span></span> | <span data-ttu-id="bd998-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1810">-inf</span></span> | <span data-ttu-id="bd998-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1811">NaN</span></span>  | <span data-ttu-id="bd998-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1812">NaN</span></span>  | <span data-ttu-id="bd998-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1813">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1814">-inf</span></span> | <span data-ttu-id="bd998-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1815">-inf</span></span> | <span data-ttu-id="bd998-1816">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1816">+inf</span></span> | <span data-ttu-id="bd998-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1817">-inf</span></span> | <span data-ttu-id="bd998-1818">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1818">+inf</span></span> | <span data-ttu-id="bd998-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1819">NaN</span></span>  | <span data-ttu-id="bd998-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1820">NaN</span></span>  | <span data-ttu-id="bd998-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1821">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1822">NaN</span></span>  | <span data-ttu-id="bd998-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1823">NaN</span></span>  | <span data-ttu-id="bd998-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1824">NaN</span></span>  | <span data-ttu-id="bd998-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1825">NaN</span></span>  | <span data-ttu-id="bd998-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1826">NaN</span></span>  | <span data-ttu-id="bd998-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1827">NaN</span></span>  | <span data-ttu-id="bd998-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1828">NaN</span></span>  | <span data-ttu-id="bd998-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1829">NaN</span></span>  | 

*  <span data-ttu-id="bd998-1830">Dzielenie dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="bd998-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="bd998-1831">Jeśli wartość argumentu po prawej stronie jest równa zero, zostanie zgłoszony `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="bd998-1832">Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-1833">Jeśli wartość wynikowa jest zbyt mała do reprezentowania w formacie `decimal`, wynik wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="bd998-1834">Skala wyniku jest najmniejszą skalą, która będzie zachować wynik równy najbliższej wartości dziesiętnej możliwej do zaprezentowania do rzeczywistego wyniku matematycznego.</span><span class="sxs-lookup"><span data-stu-id="bd998-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="bd998-1835">Dzielenie dziesiętne jest równoważne użyciu operator dzielenia typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="bd998-1836">Operator reszty</span><span class="sxs-lookup"><span data-stu-id="bd998-1836">Remainder operator</span></span>

<span data-ttu-id="bd998-1837">Aby można było wykonać operację `x % y`, rozpoznawanie przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1838">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-1839">Poniżej przedstawiono wstępnie zdefiniowane operatory pozostałej reszty.</span><span class="sxs-lookup"><span data-stu-id="bd998-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="bd998-1840">Operator All obliczy resztę podziału między `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="bd998-1841">Reszta całkowita:</span><span class="sxs-lookup"><span data-stu-id="bd998-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="bd998-1842">Wynik `x % y` jest wartością wygenerowaną przez `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="bd998-1843">Jeśli `y` wynosi zero, zostanie zgłoszony `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="bd998-1844">Jeśli argument operacji po lewej stronie jest najmniejszą wartością `int` lub `long`, a prawy operand to `-1`, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-1845">W żadnym przypadku `x % y` zgłasza wyjątek, gdzie `x / y` nie zgłasza wyjątku.</span><span class="sxs-lookup"><span data-stu-id="bd998-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="bd998-1846">Pozostała liczba zmiennoprzecinkowa:</span><span class="sxs-lookup"><span data-stu-id="bd998-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="bd998-1847">W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN.</span><span class="sxs-lookup"><span data-stu-id="bd998-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="bd998-1848">W tabeli `x` i `y` są dodatnimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="bd998-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="bd998-1849">`z` to wynik `x % y` i jest obliczany jako `x - n * y`, gdzie `n` to największa możliwa liczba całkowita, która jest mniejsza lub równa `x / y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="bd998-1850">Ta metoda przetwarzania reszty jest analogiczna do tego, który jest używany dla argumentów wartości całkowitych, ale różni się od definicji IEEE 754 (w którym `n` to liczba całkowita najbliżej `x / y`).</span><span class="sxs-lookup"><span data-stu-id="bd998-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="bd998-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="bd998-1851">+y</span></span>   | <span data-ttu-id="bd998-1852">-y</span><span class="sxs-lookup"><span data-stu-id="bd998-1852">-y</span></span>   | <span data-ttu-id="bd998-1853">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1853">+0</span></span>   | <span data-ttu-id="bd998-1854">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1854">-0</span></span>   | <span data-ttu-id="bd998-1855">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1855">+inf</span></span> | <span data-ttu-id="bd998-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1856">-inf</span></span> | <span data-ttu-id="bd998-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1857">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1858">+x</span><span class="sxs-lookup"><span data-stu-id="bd998-1858">+x</span></span>   | <span data-ttu-id="bd998-1859">+z</span><span class="sxs-lookup"><span data-stu-id="bd998-1859">+z</span></span>   | <span data-ttu-id="bd998-1860">+z</span><span class="sxs-lookup"><span data-stu-id="bd998-1860">+z</span></span>   | <span data-ttu-id="bd998-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1861">NaN</span></span>  | <span data-ttu-id="bd998-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1862">NaN</span></span>  | <span data-ttu-id="bd998-1863">x</span><span class="sxs-lookup"><span data-stu-id="bd998-1863">x</span></span>    | <span data-ttu-id="bd998-1864">x</span><span class="sxs-lookup"><span data-stu-id="bd998-1864">x</span></span>    | <span data-ttu-id="bd998-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1865">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1866">-x</span><span class="sxs-lookup"><span data-stu-id="bd998-1866">-x</span></span>   | <span data-ttu-id="bd998-1867">-z</span><span class="sxs-lookup"><span data-stu-id="bd998-1867">-z</span></span>   | <span data-ttu-id="bd998-1868">-z</span><span class="sxs-lookup"><span data-stu-id="bd998-1868">-z</span></span>   | <span data-ttu-id="bd998-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1869">NaN</span></span>  | <span data-ttu-id="bd998-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1870">NaN</span></span>  | <span data-ttu-id="bd998-1871">-x</span><span class="sxs-lookup"><span data-stu-id="bd998-1871">-x</span></span>   | <span data-ttu-id="bd998-1872">-x</span><span class="sxs-lookup"><span data-stu-id="bd998-1872">-x</span></span>   | <span data-ttu-id="bd998-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1873">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1874">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1874">+0</span></span>   | <span data-ttu-id="bd998-1875">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1875">+0</span></span>   | <span data-ttu-id="bd998-1876">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1876">+0</span></span>   | <span data-ttu-id="bd998-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1877">NaN</span></span>  | <span data-ttu-id="bd998-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1878">NaN</span></span>  | <span data-ttu-id="bd998-1879">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1879">+0</span></span>   | <span data-ttu-id="bd998-1880">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1880">+0</span></span>   | <span data-ttu-id="bd998-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1881">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1882">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1882">-0</span></span>   | <span data-ttu-id="bd998-1883">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1883">-0</span></span>   | <span data-ttu-id="bd998-1884">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1884">-0</span></span>   | <span data-ttu-id="bd998-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1885">NaN</span></span>  | <span data-ttu-id="bd998-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1886">NaN</span></span>  | <span data-ttu-id="bd998-1887">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1887">-0</span></span>   | <span data-ttu-id="bd998-1888">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1888">-0</span></span>   | <span data-ttu-id="bd998-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1889">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1890">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1890">+inf</span></span> | <span data-ttu-id="bd998-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1891">NaN</span></span>  | <span data-ttu-id="bd998-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1892">NaN</span></span>  | <span data-ttu-id="bd998-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1893">NaN</span></span>  | <span data-ttu-id="bd998-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1894">NaN</span></span>  | <span data-ttu-id="bd998-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1895">NaN</span></span>  | <span data-ttu-id="bd998-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1896">NaN</span></span>  | <span data-ttu-id="bd998-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1897">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1898">-inf</span></span> | <span data-ttu-id="bd998-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1899">NaN</span></span>  | <span data-ttu-id="bd998-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1900">NaN</span></span>  | <span data-ttu-id="bd998-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1901">NaN</span></span>  | <span data-ttu-id="bd998-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1902">NaN</span></span>  | <span data-ttu-id="bd998-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1903">NaN</span></span>  | <span data-ttu-id="bd998-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1904">NaN</span></span>  | <span data-ttu-id="bd998-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1905">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1906">NaN</span></span>  | <span data-ttu-id="bd998-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1907">NaN</span></span>  | <span data-ttu-id="bd998-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1908">NaN</span></span>  | <span data-ttu-id="bd998-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1909">NaN</span></span>  | <span data-ttu-id="bd998-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1910">NaN</span></span>  | <span data-ttu-id="bd998-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1911">NaN</span></span>  | <span data-ttu-id="bd998-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1912">NaN</span></span>  | <span data-ttu-id="bd998-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1913">NaN</span></span>  | 

*  <span data-ttu-id="bd998-1914">Reszta dziesiętna:</span><span class="sxs-lookup"><span data-stu-id="bd998-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="bd998-1915">Jeśli wartość argumentu po prawej stronie jest równa zero, zostanie zgłoszony `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="bd998-1916">Skala wyniku przed zaokrągleniem jest większa od skali dwóch operandów, a znak wyniku, jeśli wartość jest różna od zera, jest taka sama jak wartość `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="bd998-1917">Reszta dziesiętna jest równoważna z użyciem operatora pozostałej części typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="bd998-1918">Operator dodawania</span><span class="sxs-lookup"><span data-stu-id="bd998-1918">Addition operator</span></span>

<span data-ttu-id="bd998-1919">Aby można było wykonać operację `x + y`, rozpoznawanie przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-1920">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-1921">Wstępnie zdefiniowane operatory dodawania są wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="bd998-1922">W przypadku typów liczbowych i wyliczeniowych wstępnie zdefiniowane operatory dodawania obliczają sumę dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="bd998-1923">Gdy jeden lub oba operandy są typu String, wstępnie zdefiniowane operatory dodawania łączą ciąg reprezentujący operandy.</span><span class="sxs-lookup"><span data-stu-id="bd998-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="bd998-1924">Dodanie liczby całkowitej:</span><span class="sxs-lookup"><span data-stu-id="bd998-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="bd998-1925">W kontekście `checked`, jeśli suma jest poza zakresem typu wyników, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-1926">W kontekście `unchecked` przepełnienia nie są raportowane i wszelkie znaczące bity o dużej kolejności poza zakresem typu wynik są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="bd998-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="bd998-1927">Dodawanie zmiennoprzecinkowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="bd998-1928">Suma jest obliczana zgodnie z regułami arytmetycznymi IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="bd998-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="bd998-1929">W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN.</span><span class="sxs-lookup"><span data-stu-id="bd998-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="bd998-1930">W tabeli `x` i `y` są wartościami niezerowymi, a `z` to wynik `x + y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="bd998-1931">Jeśli `x` i `y` mają taką samą wartość, ale przeciwległe znaki, `z` to dodatnia wartość zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="bd998-1932">Jeśli `x + y` jest zbyt duży do reprezentowania w typie docelowym, `z` jest nieskończoność z tym samym znakiem, co `x + y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="bd998-1933">Y</span><span class="sxs-lookup"><span data-stu-id="bd998-1933">y</span></span>    | <span data-ttu-id="bd998-1934">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1934">+0</span></span>   | <span data-ttu-id="bd998-1935">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1935">-0</span></span>   | <span data-ttu-id="bd998-1936">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1936">+inf</span></span> | <span data-ttu-id="bd998-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1937">-inf</span></span> | <span data-ttu-id="bd998-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1938">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1939">x</span><span class="sxs-lookup"><span data-stu-id="bd998-1939">x</span></span>    | <span data-ttu-id="bd998-1940">z</span><span class="sxs-lookup"><span data-stu-id="bd998-1940">z</span></span>    | <span data-ttu-id="bd998-1941">x</span><span class="sxs-lookup"><span data-stu-id="bd998-1941">x</span></span>    | <span data-ttu-id="bd998-1942">x</span><span class="sxs-lookup"><span data-stu-id="bd998-1942">x</span></span>    | <span data-ttu-id="bd998-1943">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1943">+inf</span></span> | <span data-ttu-id="bd998-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1944">-inf</span></span> | <span data-ttu-id="bd998-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1945">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1946">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1946">+0</span></span>   | <span data-ttu-id="bd998-1947">Y</span><span class="sxs-lookup"><span data-stu-id="bd998-1947">y</span></span>    | <span data-ttu-id="bd998-1948">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1948">+0</span></span>   | <span data-ttu-id="bd998-1949">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1949">+0</span></span>   | <span data-ttu-id="bd998-1950">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1950">+inf</span></span> | <span data-ttu-id="bd998-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1951">-inf</span></span> | <span data-ttu-id="bd998-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1952">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1953">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1953">-0</span></span>   | <span data-ttu-id="bd998-1954">Y</span><span class="sxs-lookup"><span data-stu-id="bd998-1954">y</span></span>    | <span data-ttu-id="bd998-1955">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-1955">+0</span></span>   | <span data-ttu-id="bd998-1956">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-1956">-0</span></span>   | <span data-ttu-id="bd998-1957">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1957">+inf</span></span> | <span data-ttu-id="bd998-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1958">-inf</span></span> | <span data-ttu-id="bd998-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1959">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1960">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1960">+inf</span></span> | <span data-ttu-id="bd998-1961">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1961">+inf</span></span> | <span data-ttu-id="bd998-1962">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1962">+inf</span></span> | <span data-ttu-id="bd998-1963">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1963">+inf</span></span> | <span data-ttu-id="bd998-1964">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1964">+inf</span></span> | <span data-ttu-id="bd998-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1965">NaN</span></span>  | <span data-ttu-id="bd998-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1966">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1967">-inf</span></span> | <span data-ttu-id="bd998-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1968">-inf</span></span> | <span data-ttu-id="bd998-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1969">-inf</span></span> | <span data-ttu-id="bd998-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1970">-inf</span></span> | <span data-ttu-id="bd998-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1971">NaN</span></span>  | <span data-ttu-id="bd998-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-1972">-inf</span></span> | <span data-ttu-id="bd998-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1973">NaN</span></span>  | 
   | <span data-ttu-id="bd998-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1974">NaN</span></span>  | <span data-ttu-id="bd998-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1975">NaN</span></span>  | <span data-ttu-id="bd998-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1976">NaN</span></span>  | <span data-ttu-id="bd998-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1977">NaN</span></span>  | <span data-ttu-id="bd998-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1978">NaN</span></span>  | <span data-ttu-id="bd998-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1979">NaN</span></span>  | <span data-ttu-id="bd998-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-1980">NaN</span></span>  | 

*  <span data-ttu-id="bd998-1981">Dodawanie dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="bd998-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="bd998-1982">Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-1983">Skala wyniku przed zaokrąglaniem jest większym skalą dwóch operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="bd998-1984">Dodawanie dziesiętne jest równoznaczne z użyciem operatora dodawania typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="bd998-1985">Dodawanie wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-1985">Enumeration addition.</span></span> <span data-ttu-id="bd998-1986">Każdy typ wyliczeniowy niejawnie udostępnia następujące wstępnie zdefiniowane operatory, gdzie `E` jest typem wyliczenia, a `U` jest podstawowym typem `E`:</span><span class="sxs-lookup"><span data-stu-id="bd998-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="bd998-1987">W czasie wykonywania operatory są oceniane dokładnie jako `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="bd998-1988">Łączenie ciągów:</span><span class="sxs-lookup"><span data-stu-id="bd998-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="bd998-1989">Te przeciążenia binarnego operatora `+` wykonują łączenie ciągów.</span><span class="sxs-lookup"><span data-stu-id="bd998-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="bd998-1990">Jeśli argument operacji łączenia ciągów jest `null`, zostanie zastąpiony pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="bd998-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="bd998-1991">W przeciwnym razie dowolny argument niebędący ciągiem jest konwertowany na reprezentację ciągu przez wywołanie metody wirtualnej `ToString` dziedziczonej z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="bd998-1992">Jeśli `ToString` zwraca `null`, zostanie zastąpiony pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="bd998-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Wynik operatora łączenia ciągów jest ciągiem zawierającym znaki operandu po lewej stronie, po którym następuje znak operandu z prawej strony. Operator łączenia ciągów nigdy nie zwraca wartości `null`. <span data-ttu-id="bd998-1995">Jeśli nie jest dostępna wystarczająca ilość pamięci do przydzielenia ciągu powstałego, może zostać wygenerowany `System.OutOfMemoryException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="bd998-1996">Delegowanie kombinacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-1996">Delegate combination.</span></span> <span data-ttu-id="bd998-1997">Każdy typ delegata niejawnie udostępnia następujący wstępnie zdefiniowany operator, gdzie `D` jest typem delegata:</span><span class="sxs-lookup"><span data-stu-id="bd998-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="bd998-1998">Operator binarny `+` wykonuje kombinację delegata, gdy oba operandy są typu delegata `D`.</span><span class="sxs-lookup"><span data-stu-id="bd998-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="bd998-1999">(Jeśli operandy mają różne typy delegatów, występuje błąd czasu powiązania). Jeśli pierwszy operand jest `null`, wynik operacji jest wartością drugiego operandu (nawet jeśli jest również `null`).</span><span class="sxs-lookup"><span data-stu-id="bd998-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="bd998-2000">W przeciwnym razie, jeśli drugi operand jest `null`, wówczas wynikiem operacji jest wartość pierwszego operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="bd998-2001">W przeciwnym razie wynik operacji jest nowym wystąpieniem delegata, które wywołuje pierwszy operand, a następnie wywołuje drugi operand.</span><span class="sxs-lookup"><span data-stu-id="bd998-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="bd998-2002">Aby zapoznać się z przykładami kombinacji delegatów, zobacz [operator odejmowania](expressions.md#subtraction-operator) i [delegowanie wywołania](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="bd998-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="bd998-2003">Ponieważ `System.Delegate` nie jest typem delegata, nie zdefiniowano `operator` @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="bd998-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="bd998-2004">Operator odejmowania</span><span class="sxs-lookup"><span data-stu-id="bd998-2004">Subtraction operator</span></span>

<span data-ttu-id="bd998-2005">Aby można było wykonać operację `x - y`, rozpoznawanie przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-2006">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-2007">Poniżej przedstawiono wstępnie zdefiniowane operatory odejmowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="bd998-2008">Operatory wszystkie odejmowania `y` od `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="bd998-2009">Odejmowanie całkowite:</span><span class="sxs-lookup"><span data-stu-id="bd998-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="bd998-2010">W kontekście `checked`, jeśli różnica jest poza zakresem typu wyników, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-2011">W kontekście `unchecked` przepełnienia nie są raportowane i wszelkie znaczące bity o dużej kolejności poza zakresem typu wynik są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="bd998-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="bd998-2012">Odejmowanie zmiennoprzecinkowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="bd998-2013">Różnica jest obliczana zgodnie z regułami arytmetycznymi IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="bd998-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="bd998-2014">W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaNs.</span><span class="sxs-lookup"><span data-stu-id="bd998-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="bd998-2015">W tabeli `x` i `y` są wartościami niezerowymi, a `z` to wynik `x - y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="bd998-2016">Jeśli `x` i `y` są równe, `z` to dodatnia wartość zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="bd998-2017">Jeśli `x - y` jest zbyt duży do reprezentowania w typie docelowym, `z` jest nieskończoność z tym samym znakiem, co `x - y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="bd998-2018">Y</span><span class="sxs-lookup"><span data-stu-id="bd998-2018">y</span></span>    | <span data-ttu-id="bd998-2019">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-2019">+0</span></span>   | <span data-ttu-id="bd998-2020">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-2020">-0</span></span>   | <span data-ttu-id="bd998-2021">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2021">+inf</span></span> | <span data-ttu-id="bd998-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2022">-inf</span></span> | <span data-ttu-id="bd998-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2023">NaN</span></span> | 
   | <span data-ttu-id="bd998-2024">x</span><span class="sxs-lookup"><span data-stu-id="bd998-2024">x</span></span>    | <span data-ttu-id="bd998-2025">z</span><span class="sxs-lookup"><span data-stu-id="bd998-2025">z</span></span>    | <span data-ttu-id="bd998-2026">x</span><span class="sxs-lookup"><span data-stu-id="bd998-2026">x</span></span>    | <span data-ttu-id="bd998-2027">x</span><span class="sxs-lookup"><span data-stu-id="bd998-2027">x</span></span>    | <span data-ttu-id="bd998-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2028">-inf</span></span> | <span data-ttu-id="bd998-2029">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2029">+inf</span></span> | <span data-ttu-id="bd998-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2030">NaN</span></span> | 
   | <span data-ttu-id="bd998-2031">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-2031">+0</span></span>   | <span data-ttu-id="bd998-2032">-y</span><span class="sxs-lookup"><span data-stu-id="bd998-2032">-y</span></span>   | <span data-ttu-id="bd998-2033">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-2033">+0</span></span>   | <span data-ttu-id="bd998-2034">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-2034">+0</span></span>   | <span data-ttu-id="bd998-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2035">-inf</span></span> | <span data-ttu-id="bd998-2036">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2036">+inf</span></span> | <span data-ttu-id="bd998-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2037">NaN</span></span> | 
   | <span data-ttu-id="bd998-2038">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-2038">-0</span></span>   | <span data-ttu-id="bd998-2039">-y</span><span class="sxs-lookup"><span data-stu-id="bd998-2039">-y</span></span>   | <span data-ttu-id="bd998-2040">-0</span><span class="sxs-lookup"><span data-stu-id="bd998-2040">-0</span></span>   | <span data-ttu-id="bd998-2041">+0</span><span class="sxs-lookup"><span data-stu-id="bd998-2041">+0</span></span>   | <span data-ttu-id="bd998-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2042">-inf</span></span> | <span data-ttu-id="bd998-2043">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2043">+inf</span></span> | <span data-ttu-id="bd998-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2044">NaN</span></span> | 
   | <span data-ttu-id="bd998-2045">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2045">+inf</span></span> | <span data-ttu-id="bd998-2046">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2046">+inf</span></span> | <span data-ttu-id="bd998-2047">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2047">+inf</span></span> | <span data-ttu-id="bd998-2048">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2048">+inf</span></span> | <span data-ttu-id="bd998-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2049">NaN</span></span>  | <span data-ttu-id="bd998-2050">\+ plik inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2050">+inf</span></span> | <span data-ttu-id="bd998-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2051">NaN</span></span> | 
   | <span data-ttu-id="bd998-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2052">-inf</span></span> | <span data-ttu-id="bd998-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2053">-inf</span></span> | <span data-ttu-id="bd998-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2054">-inf</span></span> | <span data-ttu-id="bd998-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2055">-inf</span></span> | <span data-ttu-id="bd998-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="bd998-2056">-inf</span></span> | <span data-ttu-id="bd998-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2057">NaN</span></span>  | <span data-ttu-id="bd998-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2058">NaN</span></span> | 
   | <span data-ttu-id="bd998-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2059">NaN</span></span>  | <span data-ttu-id="bd998-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2060">NaN</span></span>  | <span data-ttu-id="bd998-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2061">NaN</span></span>  | <span data-ttu-id="bd998-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2062">NaN</span></span>  | <span data-ttu-id="bd998-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2063">NaN</span></span>  | <span data-ttu-id="bd998-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2064">NaN</span></span>  | <span data-ttu-id="bd998-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="bd998-2065">NaN</span></span> | 

*  <span data-ttu-id="bd998-2066">Odejmowanie dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="bd998-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="bd998-2067">Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="bd998-2068">Skala wyniku przed zaokrąglaniem jest większym skalą dwóch operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="bd998-2069">Odejmowanie dziesiętne jest równoważne użyciu operator odejmowania typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="bd998-2070">Odejmowanie wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-2070">Enumeration subtraction.</span></span> <span data-ttu-id="bd998-2071">Każdy typ wyliczeniowy niejawnie udostępnia następujący wstępnie zdefiniowany operator, gdzie `E` jest typem wyliczenia, a `U` jest typem podstawowym `E`:</span><span class="sxs-lookup"><span data-stu-id="bd998-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="bd998-2072">Ten operator jest oceniany dokładnie jako `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="bd998-2073">Innymi słowy operator oblicza różnicę między wartościami porządkowymi `x` i `y`, a typ wyniku jest podstawowym typem wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="bd998-2074">Ten operator jest oceniany dokładnie jako `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="bd998-2075">Innymi słowy, operator odejmuje wartość od podstawowego typu wyliczenia, która zwraca wartość wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="bd998-2076">Usuwanie delegata.</span><span class="sxs-lookup"><span data-stu-id="bd998-2076">Delegate removal.</span></span> <span data-ttu-id="bd998-2077">Każdy typ delegata niejawnie udostępnia następujący wstępnie zdefiniowany operator, gdzie `D` jest typem delegata:</span><span class="sxs-lookup"><span data-stu-id="bd998-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="bd998-2078">Operator binarny `-` wykonuje usuwanie delegata, gdy oba operandy są typu delegata `D`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="bd998-2079">Jeśli operandy mają różne typy delegatów, wystąpi błąd w czasie trwania powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="bd998-2080">Jeśli pierwszy operand ma `null`, wynik operacji jest `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="bd998-2081">W przeciwnym razie, jeśli drugi operand jest `null`, wówczas wynikiem operacji jest wartość pierwszego operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="bd998-2082">W przeciwnym razie oba operandy reprezentują listy wywołań ([deklaracje delegatów](delegates.md#delegate-declarations)), które mają co najmniej jeden wpis, a wynik jest nową listą wywołania składającą się z listy pierwszego operandu, z której usunięto wpisy drugiego operandu, pod warunkiem że drugi Lista operandów jest prawidłową ciągłą podlistą pierwszego elementu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="bd998-2083">(Aby określić równość podlistów, odpowiednie wpisy są porównywane jako dla operatora równości delegata ([operatorzy równości](expressions.md#delegate-equality-operators)). W przeciwnym razie wynik jest wartością operandu po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="bd998-2084">Żadna z list argumentów operacji nie została zmieniona w procesie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="bd998-2085">Jeśli drugi operand jest zgodny z wieloma podlistami ciągłego wpisów na liście pierwszego operandu, zostanie usunięta podlista z prawej strony.</span><span class="sxs-lookup"><span data-stu-id="bd998-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="bd998-2086">Jeśli usunięcie spowoduje powstanie pustej listy, wynik jest `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="bd998-2087">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-2087">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="bd998-2088">Operatory przesunięcia</span><span class="sxs-lookup"><span data-stu-id="bd998-2088">Shift operators</span></span>

<span data-ttu-id="bd998-2089">Operatory `<<` i `>>` służą do wykonywania operacji przesunięcia bitowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="bd998-2090">Jeśli argument operacji *shift_expression* ma typ czasu kompilacji `dynamic`, to wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-2091">W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania przy użyciu typu czasu wykonywania tych argumentów, które mają typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="bd998-2092">Dla operacji w formie `x << count` lub `x >> count`, rozpoznawanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-2093">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-2094">Podczas deklarowania przeciążonego operatora przesunięcia typ pierwszego operandu musi być zawsze klasą lub strukturą zawierającą deklarację operatora, a typ drugiego operandu musi być zawsze `int`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="bd998-2095">Poniżej przedstawiono wstępnie zdefiniowane operatory przesunięcia.</span><span class="sxs-lookup"><span data-stu-id="bd998-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="bd998-2096">Przesuń w lewo:</span><span class="sxs-lookup"><span data-stu-id="bd998-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="bd998-2097">Operator `<<` przesuwa `x` pozostawione przez liczbę bitów obliczanych w sposób opisany poniżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="bd998-2098">Bity o dużej kolejności poza zakresem wyników `x` są odrzucane, pozostałe bity są przesunięte w lewo, a puste pozycje bitu w kolejności są ustawione na zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="bd998-2099">Przesuń w prawo:</span><span class="sxs-lookup"><span data-stu-id="bd998-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="bd998-2100">Operator `>>` przesuwa `x` bezpośrednio przez liczbę bitów obliczanych w sposób opisany poniżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="bd998-2101">Gdy `x` jest typu `int` lub `long`, bity o niskiej kolejności w `x` są odrzucane, pozostałe bity są przesuwane po prawej stronie, a puste pozycje w porządku o wysokim stopniu są ustawione na zero, jeśli @no__t</span><span class="sxs-lookup"><span data-stu-id="bd998-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="bd998-2102">Gdy `x` jest typu `uint` lub `ulong`, bity o niskiej kolejności w `x` są odrzucane, pozostałe bity są przesuwane po prawej stronie, a puste pozycje w dużej kolejności są ustawiane na zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="bd998-2103">Dla wstępnie zdefiniowanych operatorów liczba bitów do przesunięcia jest obliczana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="bd998-2104">Gdy typ `x` jest `int` lub `uint`, liczba przesunięć jest określona przez pięć bitów `count`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="bd998-2105">Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="bd998-2106">Gdy typ `x` jest `long` lub `ulong`, liczba przesunięć jest określona przez sześć bitów z `count`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="bd998-2107">Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="bd998-2108">Jeśli wynikowa liczba przesunięć jest równa zero, operatory przesunięcia po prostu zwracają wartość `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="bd998-2109">Operacje przesunięcia nigdy nie powodują przepełnienia i tworzą te same wyniki w kontekście `checked` i `unchecked`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="bd998-2110">Gdy argument operacji po lewej stronie operatora `>>` jest ze znakiem typu całkowitego, operator wykonuje arytmetyczne przesunięcie w prawo, co powoduje, że wartość najbardziej znaczącego bitu (bit znaku) operandu jest propagowana do pustych pozycji w dużej kolejności.</span><span class="sxs-lookup"><span data-stu-id="bd998-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="bd998-2111">Gdy lewy argument operacji operatora `>>` jest niepodpisanym typem całkowitym, operator wykonuje logiczne przesunięcie w prawo, co powoduje, że puste pozycje bitu o wysokiej kolejności są zawsze ustawione na zero.</span><span class="sxs-lookup"><span data-stu-id="bd998-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="bd998-2112">Aby wykonać odwrotną operację, która została wywnioskowana z typu operandu, można użyć jawnych rzutowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="bd998-2113">Na przykład jeśli `x` jest zmienną typu `int`, operacja `unchecked((int)((uint)x >> y))` wykonuje przesunięcie logiczne z prawej strony `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="bd998-2114">Operatory relacyjne i testowe typu</span><span class="sxs-lookup"><span data-stu-id="bd998-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="bd998-2115">Operatory `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` i `as` są nazywane operatorami relacyjnymi i typu---testowym.</span><span class="sxs-lookup"><span data-stu-id="bd998-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="bd998-2116">Operator `is` został opisany w [operatorze is](expressions.md#the-is-operator) i operator `as` został opisany w [operatorze as](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="bd998-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="bd998-2117">Operatory `==`, `!=`, `<`, `>`, `<=` i `>=` są ***operatorami porównania***.</span><span class="sxs-lookup"><span data-stu-id="bd998-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="bd998-2118">Jeśli argument operacji operatora porównania ma typ czasu kompilacji `dynamic`, to wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-2119">W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania przy użyciu typu czasu wykonywania tych argumentów, które mają typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="bd998-2120">Dla operacji w formularzu `x` *op* `y`, gdzie *op* jest operatorem porównania, do wybrania implementacji określonego operatora jest stosowane rozpoznawanie przeciążenia ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-2121">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-2122">Wstępnie zdefiniowane operatory porównania są opisane w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="bd998-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="bd998-2123">Wszystkie wstępnie zdefiniowane operatory porównania zwracają wynik typu `bool`, zgodnie z opisem w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="bd998-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="bd998-2124">__Operacja__</span><span class="sxs-lookup"><span data-stu-id="bd998-2124">__Operation__</span></span> | <span data-ttu-id="bd998-2125">__wynik__</span><span class="sxs-lookup"><span data-stu-id="bd998-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="bd998-2126">`true` Jeśli `x` jest równe `y`, `false` w przeciwnym razie</span><span class="sxs-lookup"><span data-stu-id="bd998-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="bd998-2127">`true` Jeśli `x` nie jest równe `y`, `false` w przeciwnym razie</span><span class="sxs-lookup"><span data-stu-id="bd998-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="bd998-2128">`true` Jeśli `x` jest mniejsza niż `y`, `false` w przeciwnym razie</span><span class="sxs-lookup"><span data-stu-id="bd998-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="bd998-2129">`true` Jeśli `x` jest większy niż `y`, `false` w przeciwnym razie</span><span class="sxs-lookup"><span data-stu-id="bd998-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="bd998-2130">`true` Jeśli `x` jest mniejsze niż lub równe `y`, `false` w przeciwnym razie</span><span class="sxs-lookup"><span data-stu-id="bd998-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="bd998-2131">`true` Jeśli `x` jest większe niż lub równe `y`, `false` w przeciwnym razie</span><span class="sxs-lookup"><span data-stu-id="bd998-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="bd998-2132">Operatory porównywania liczb całkowitych</span><span class="sxs-lookup"><span data-stu-id="bd998-2132">Integer comparison operators</span></span>

<span data-ttu-id="bd998-2133">Wstępnie zdefiniowane Operatory porównywania liczb całkowitych to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2133">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="bd998-2134">Każdy z tych operatorów porównuje wartości liczbowe dwóch operandów całkowitych i zwraca wartość `bool`, która wskazuje, czy określona relacja jest `true` czy `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="bd998-2135">Operatory porównania zmiennoprzecinkowego</span><span class="sxs-lookup"><span data-stu-id="bd998-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="bd998-2136">Wstępnie zdefiniowane operatory porównania zmiennoprzecinkowego to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2136">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="bd998-2137">Operatory porównują operandy zgodnie z regułami standardu IEEE 754:</span><span class="sxs-lookup"><span data-stu-id="bd998-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="bd998-2138">Jeśli każdy operand jest NaN, wynik jest `false` dla wszystkich operatorów z wyjątkiem `!=`, dla którego wynik jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="bd998-2139">Dla każdego z dwóch operandów `x != y` zawsze daje ten sam wynik co `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="bd998-2140">Jeśli jednak jeden lub oba operandy są NaN, operatory `<`, `>`, `<=` i `>=` nie generują tych samych wyników co logiczne Negacja operatora przeciwległego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="bd998-2141">Na przykład jeśli jeden z wartości `x` i `y` ma wartość NaN, `x < y` jest `false`, ale `!(x >= y)` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="bd998-2142">Gdy żaden operand nie jest NaN, operatory porównują wartości dwóch argumentów zmiennoprzecinkowych w odniesieniu do kolejności</span><span class="sxs-lookup"><span data-stu-id="bd998-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="bd998-2143">gdzie `min` i `max` to najmniejsza i największa wartość dodatnia wartości, które mogą być reprezentowane w danym formacie zmiennoprzecinkowym.</span><span class="sxs-lookup"><span data-stu-id="bd998-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="bd998-2144">Znaczące efekty tego porządku to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="bd998-2145">Ujemne i dodatnie zera są uważane za równe.</span><span class="sxs-lookup"><span data-stu-id="bd998-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="bd998-2146">Ujemna nieskończoność jest uznawana za mniej niż wszystkie inne wartości, ale jest równa innej nieskończoności ujemnej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="bd998-2147">Nieskończoność dodatnia jest uznawana za większą niż wszystkie inne wartości, ale jest równa innej nieskończoności dodatniej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="bd998-2148">Operatory porównywania dziesiętnego</span><span class="sxs-lookup"><span data-stu-id="bd998-2148">Decimal comparison operators</span></span>

<span data-ttu-id="bd998-2149">Wstępnie zdefiniowane Operatory porównywania dziesiętnego to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="bd998-2150">Każdy z tych operatorów porównuje wartości liczbowe dwóch argumentów operacji dziesiętnych i zwraca wartość `bool`, która wskazuje, czy określona relacja jest `true` czy `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="bd998-2151">Każde porównanie dziesiętne jest równoważne użyciu odpowiadającego operatora relacyjnego lub równości typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="bd998-2152">Operatory równości wartości logicznych</span><span class="sxs-lookup"><span data-stu-id="bd998-2152">Boolean equality operators</span></span>

<span data-ttu-id="bd998-2153">Wstępnie zdefiniowane operatory równości wartości logicznych to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="bd998-2154">Wynik `==` jest `true`, jeśli oba `x` i `y` są `true` lub, jeśli oba `x` i `y` są `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="bd998-2155">W przeciwnym razie wynik jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="bd998-2156">Wynik `!=` jest `false`, jeśli oba `x` i `y` są `true` lub, jeśli oba `x` i `y` są `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="bd998-2157">W przeciwnym razie wynik jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="bd998-2158">Gdy operandy są typu `bool`, operator `!=` daje ten sam wynik jako operator `^`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="bd998-2159">Operatory porównania wyliczenia</span><span class="sxs-lookup"><span data-stu-id="bd998-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="bd998-2160">Każdy typ wyliczeniowy niejawnie udostępnia następujące wstępnie zdefiniowane operatory porównania:</span><span class="sxs-lookup"><span data-stu-id="bd998-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="bd998-2161">Wynik oceny `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczenia `E` z typem podstawowym `U`, a `op` jest jednym z operatorów porównania, jest dokładnie taka sama jak Ocena `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="bd998-2162">Innymi słowy operatory porównywania typów wyliczeniowych po prostu porównują zasadnicze wartości dwóch operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="bd998-2163">Operatory równości typów referencyjnych</span><span class="sxs-lookup"><span data-stu-id="bd998-2163">Reference type equality operators</span></span>

<span data-ttu-id="bd998-2164">Zdefiniowane operatory równości typów referencyjnych są następujące:</span><span class="sxs-lookup"><span data-stu-id="bd998-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="bd998-2165">Operatory zwracają wynik porównania dwóch odwołań dla równości lub nierówności.</span><span class="sxs-lookup"><span data-stu-id="bd998-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="bd998-2166">Ze względu na to, że wstępnie zdefiniowane operatory równości typu referencyjnego akceptują operandy typu `object`, mają zastosowanie do wszystkich typów, które nie deklarują odpowiednich `operator ==` i `operator !=` elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="bd998-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="bd998-2167">Z drugiej strony, wszystkie odpowiednie operatory równości zdefiniowane przez użytkownika skutecznie ukrywają wstępnie zdefiniowane operatory równości typów odwołań.</span><span class="sxs-lookup"><span data-stu-id="bd998-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="bd998-2168">Operatory równości wstępnie zdefiniowanego typu referencyjnego wymagają jednego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="bd998-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="bd998-2169">Oba operandy są wartością typu znanego jako *reference_type* lub literał `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="bd998-2170">Ponadto jawna konwersja odwołań ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) istnieje z typu każdego operandu do typu innego operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="bd998-2171">Jeden operand jest wartością typu `T`, gdzie `T` jest *type_parameter* , a drugi operand jest literałem `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="bd998-2172">Ponadto `T` nie ma ograniczenia typu wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="bd998-2173">Jeśli jeden z tych warunków nie jest spełniony, wystąpi błąd w czasie trwania powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="bd998-2174">Istotne implikacje tych reguł są następujące:</span><span class="sxs-lookup"><span data-stu-id="bd998-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="bd998-2175">Jest to błąd czasu powiązania, który służy do porównywania dwóch odwołań, które są znane jako różne w czasie trwania powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="bd998-2176">Jeśli na przykład typy powiązań argumentów operacji są dwa typy klas `A` i `B`, a nie `A` ani `B` wynikają z innych, wówczas te dwa operandy odwołują się do tego samego obiektu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="bd998-2177">W rezultacie operacja jest traktowana jako błąd czasu powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="bd998-2178">Wstępnie zdefiniowane operatory równości typu referencyjnego nie zezwalają na porównywanie operandów typu wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="bd998-2179">W związku z tym, chyba że typ struktury deklaruje własne operatory równości, nie można porównać wartości tego typu struktury.</span><span class="sxs-lookup"><span data-stu-id="bd998-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="bd998-2180">Operatory równości typu referencyjnego ze wstępnie zdefiniowanymi odwołaniami nigdy nie powodują wykonywania operacji opakowywania dla ich argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="bd998-2181">Nie będzie to miało znaczenia w przypadku wykonywania takich operacji pakowania, ponieważ odwołania do nowo przydzielonych wystąpień opakowanych będą się różnić od wszystkich innych odwołań.</span><span class="sxs-lookup"><span data-stu-id="bd998-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="bd998-2182">Jeśli argument operacji typu parametru typu `T` jest porównywany z `null`, a typ czasu wykonywania `T` jest typem wartości, wynik porównania jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="bd998-2183">Poniższy przykład sprawdza, czy argument nieograniczonego typu parametru typu jest `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="bd998-2184">Konstrukcja `x == null` jest dozwolona nawet wtedy, gdy `T` może reprezentować typ wartości, a wynik jest po prostu zdefiniowany jako `false`, gdy `T` jest typem wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="bd998-2185">W przypadku operacji w postaci `x == y` lub `x != y`, jeśli istnieją odpowiednie `operator ==` lub `operator !=` istnieje, reguły rozpoznawania przeciążenia operatora ([rozpoznawania przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) spowodują wybranie tego operatora zamiast wstępnie zdefiniowanej równości typu odwołania zakład.</span><span class="sxs-lookup"><span data-stu-id="bd998-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="bd998-2186">Jednak zawsze jest możliwe wybranie wstępnie zdefiniowanego operatora równości typów odwołań przez jawne rzutowanie jednego lub obu operandów na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="bd998-2187">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2187">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="bd998-2188">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="bd998-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="bd998-2189">Zmienne `s` i `t` odnoszą się do dwóch różnych wystąpień `string` zawierających te same znaki.</span><span class="sxs-lookup"><span data-stu-id="bd998-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="bd998-2190">Pierwsze wyniki porównania `True` ponieważ wstępnie zdefiniowany operator równości ciągów ([Operatory równości ciągów](expressions.md#string-equality-operators)) jest wybierany, gdy oba operandy są typu `string`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="bd998-2191">Pozostałe porównania wszystkie wyjściowe `False` ze względu na to, że wstępnie zdefiniowany operator równości typu odwołania jest wybierany, gdy jeden lub oba operandy są typu `object`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="bd998-2192">Należy zauważyć, że powyższa technika nie jest istotna dla typów wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="bd998-2193">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2193">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="bd998-2194">dane wyjściowe `False`, ponieważ rzutowania tworzą odwołania do dwóch oddzielnych wystąpień wartości zapakowanych `int`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="bd998-2195">Operatory równości ciągów</span><span class="sxs-lookup"><span data-stu-id="bd998-2195">String equality operators</span></span>

<span data-ttu-id="bd998-2196">Wstępnie zdefiniowane operatory równości ciągów są:</span><span class="sxs-lookup"><span data-stu-id="bd998-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="bd998-2197">Dwie wartości `string` są uważane za równe, gdy jest spełniony jeden z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="bd998-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="bd998-2198">Obie wartości są `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="bd998-2199">Obie wartości są odwołaniami o wartościach innych niż null do wystąpień ciągów, które mają identyczne długości i identyczne znaki w każdej pozycji znaku.</span><span class="sxs-lookup"><span data-stu-id="bd998-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="bd998-2200">Operatory równości ciągów porównują wartości ciągu, a nie odwołania do ciągu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="bd998-2201">Gdy dwa oddzielne wystąpienia ciągu zawierają dokładnie tę samą sekwencję znaków, wartości ciągów są równe, ale odwołania są różne.</span><span class="sxs-lookup"><span data-stu-id="bd998-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="bd998-2202">Zgodnie z opisem w [operatorach równości typu referencyjnego](expressions.md#reference-type-equality-operators)operatory równości typu odwołania mogą służyć do porównywania odwołań ciągów zamiast wartości ciągów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="bd998-2203">Deleguj operatory równości</span><span class="sxs-lookup"><span data-stu-id="bd998-2203">Delegate equality operators</span></span>

<span data-ttu-id="bd998-2204">Każdy typ delegata niejawnie udostępnia następujące wstępnie zdefiniowane operatory porównania:</span><span class="sxs-lookup"><span data-stu-id="bd998-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="bd998-2205">Dwa wystąpienia delegatów są uważane za równe w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="bd998-2206">Jeśli jedno z wystąpień delegatów ma `null`, są równe, jeśli oba są `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="bd998-2207">Jeśli Delegaty mają różne typy czasu wykonywania, nigdy nie są równe.</span><span class="sxs-lookup"><span data-stu-id="bd998-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="bd998-2208">Jeśli oba wystąpienia delegatów mają listę wywołań ([deklaracje delegatów](delegates.md#delegate-declarations)), te wystąpienia są równe, gdy i tylko wtedy, gdy ich listy wywołań są takie same, a każdy wpis na liście wywołań jest równy (jak zdefiniowano poniżej) do odpowiedniego wpis w kolejności na liście wywołań drugiej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="bd998-2209">Następujące reguły określają równość wpisów listy wywołań:</span><span class="sxs-lookup"><span data-stu-id="bd998-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="bd998-2210">Jeśli dwa wpisy listy wywołań odnoszą się do tej samej metody statycznej, wpisy są równe.</span><span class="sxs-lookup"><span data-stu-id="bd998-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="bd998-2211">Jeśli dwa wywołania listy wywołań odnoszą się do tej samej metody niestatycznej w tym samym obiekcie docelowym (zgodnie z definicją przez operatory równości odwołań), wpisy są równe.</span><span class="sxs-lookup"><span data-stu-id="bd998-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="bd998-2212">Wpisy listy wywołań uzyskane z oceny semantycznie identyczne *anonymous_method_expression*s lub *lambda_expression*s z tym samym (prawdopodobnie pustym) zestawem przechwyconych zewnętrznych wystąpień zmiennych są dozwolone (ale nie są wymagane) większy.</span><span class="sxs-lookup"><span data-stu-id="bd998-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="bd998-2213">Operatory równości i null</span><span class="sxs-lookup"><span data-stu-id="bd998-2213">Equality operators and null</span></span>

<span data-ttu-id="bd998-2214">Operatory `==` i `!=` zezwalają jednemu operandowi na wartość typu null, a inne jako literał `null`, nawet jeśli nie istnieje zdefiniowany lub zdefiniowany przez użytkownika operator (w niepodniesionym lub zniesionym formularzu) dla tej operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="bd998-2215">Dla operacji jednego z formularzy</span><span class="sxs-lookup"><span data-stu-id="bd998-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="bd998-2216">gdzie `x` jest wyrażeniem typu dopuszczającego wartość null, jeśli nie można znaleźć odpowiedniego operatora w rozpoznaniu przeciążenia operatora ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)), wynik jest obliczany na podstawie właściwości `HasValue` `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="bd998-2217">W każdym przypadku pierwsze dwa formularze są tłumaczone na `!x.HasValue`, a ostatnie dwa formularze są tłumaczone na `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="bd998-2218">Operator is</span><span class="sxs-lookup"><span data-stu-id="bd998-2218">The is operator</span></span>

<span data-ttu-id="bd998-2219">Operator `is` służy do dynamicznego sprawdzenia, czy typ obiektu w czasie wykonywania jest zgodny z danym typem.</span><span class="sxs-lookup"><span data-stu-id="bd998-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="bd998-2220">Wynik operacji `E is T`, gdzie `E` jest wyrażeniem, a `T` jest typem, jest wartością logiczną wskazującą, czy `E` można pomyślnie przekonwertować na typ `T` przez konwersję odwołania, konwersję z opakowania lub konwersję rozpakowywanie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="bd998-2221">Operacja jest szacowana w następujący sposób, gdy argumenty typu zostały zastąpione dla wszystkich parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="bd998-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="bd998-2222">Jeśli `E` to funkcja anonimowa, wystąpi błąd w czasie kompilacji</span><span class="sxs-lookup"><span data-stu-id="bd998-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="bd998-2223">Jeśli `E` jest grupą metod lub literałem `null`, jeśli typ `E` jest typem referencyjnym lub typem dopuszczającym wartość null, a wartością `E` jest null, wynik ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="bd998-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="bd998-2224">W przeciwnym razie niech `D` reprezentuje dynamiczny typ `E` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="bd998-2225">Jeśli typ `E` jest typem referencyjnym, `D` jest typem czasu wykonywania odwołania do wystąpienia przez `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="bd998-2226">Jeśli typ `E` jest typem dopuszczającym wartość null, `D` jest typem podstawowym tego typu dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="bd998-2227">Jeśli typ `E` nie jest typem wartości null, `D` jest typem `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="bd998-2228">Wynik operacji zależy od `D` i `T` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="bd998-2229">Jeśli `T` jest typem referencyjnym, wynik ma wartość true, jeśli `D` i `T` są tego samego typu, jeśli `D` jest typem referencyjnym i niejawną konwersją referencyjną z `D` do `T` istnieje na `T` istnieje.</span><span class="sxs-lookup"><span data-stu-id="bd998-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="bd998-2230">Jeśli `T` jest typem dopuszczającym wartość null, wynik ma wartość true, jeśli `D` jest typem źródłowym `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="bd998-2231">Jeśli `T` to typ wartości niedopuszczający wartości null, wynik ma wartość true, jeśli `D` i `T` są tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="bd998-2232">W przeciwnym razie wynik ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="bd998-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="bd998-2233">Należy zauważyć, że konwersje zdefiniowane przez użytkownika nie są uwzględniane przez operatora `is`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="bd998-2234">Operator as</span><span class="sxs-lookup"><span data-stu-id="bd998-2234">The as operator</span></span>

<span data-ttu-id="bd998-2235">Operator `as` służy do jawnej konwersji wartości na dany typ referencyjny lub typ dopuszczający wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="bd998-2236">W przeciwieństwie do wyrażenia CAST ([wyrażenia rzutowania](expressions.md#cast-expressions)) operator `as` nigdy nie zgłasza wyjątku.</span><span class="sxs-lookup"><span data-stu-id="bd998-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="bd998-2237">Zamiast tego, jeśli wskazana konwersja nie jest możliwa, obliczona wartość jest `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="bd998-2238">W operacji `E as T`, `E` musi być wyrażeniem, a `T` musi być typem referencyjnym, parametrem typu znanym jako typ referencyjny lub typem dopuszczającym wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="bd998-2239">Ponadto co najmniej jeden z następujących elementów musi mieć wartość true lub w przeciwnym razie wystąpi błąd w czasie kompilacji:</span><span class="sxs-lookup"><span data-stu-id="bd998-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="bd998-2240">Tożsamość ([konwersja tożsamości](conversions.md#identity-conversion)), niejawna wartość null ([niejawne konwersje dopuszczające wartość null](conversions.md#implicit-nullable-conversions)), niejawne odwołanie ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)), opakowanie ([konwersje opakowania](conversions.md#boxing-conversions)), jawny Nullable ([jawny typ Nullable Konwersje](conversions.md#explicit-nullable-conversions)), jawne odwołanie ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) lub rozpakowywanie ([konwersje rozpakowywanie](conversions.md#unboxing-conversions)) istnieje w `E` do `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="bd998-2241">Typ `E` lub `T` jest typem otwartym.</span><span class="sxs-lookup"><span data-stu-id="bd998-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="bd998-2242">`E` jest literałem `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="bd998-2243">Jeśli typ czasu kompilacji `E` nie jest `dynamic`, operacja `E as T` daje ten sam wynik jako</span><span class="sxs-lookup"><span data-stu-id="bd998-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="bd998-2244">z tą różnicą, że `E` jest obliczany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="bd998-2245">Kompilator może być oczekiwany do zoptymalizowania `E as T` w celu przeprowadzenia co najwyżej jednego sprawdzenia typu dynamicznego, w przeciwieństwie do dwóch kontroli typu dynamicznego, które wynikają z powyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="bd998-2246">Jeśli typ czasu kompilacji `E` jest `dynamic`, w przeciwieństwie do operatora rzutowania operator `as` nie jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-2247">W związku z tym rozwinięcie w tym przypadku jest następujące:</span><span class="sxs-lookup"><span data-stu-id="bd998-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="bd998-2248">Należy zauważyć, że niektóre konwersje, takie jak konwersje zdefiniowane przez użytkownika, nie są możliwe za pomocą operatora `as` i powinny być wykonywane przy użyciu wyrażeń rzutowania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="bd998-2249">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-2249">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="bd998-2250">parametr typu `T` z `G` jest znany jako typ referencyjny, ponieważ ma ograniczenie klasy.</span><span class="sxs-lookup"><span data-stu-id="bd998-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="bd998-2251">Nie ma jednak parametru typu `U` z `H`. w związku z tym użycie operatora `as` w `H` jest niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="bd998-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="bd998-2252">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-2252">Logical operators</span></span>

<span data-ttu-id="bd998-2253">Operatory `&`, `^` i `|` są nazywane operatorami logicznymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="bd998-2254">Jeśli argument operacji operatora logicznego ma typ czasu kompilacji `dynamic`, to wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-2255">W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania przy użyciu typu czasu wykonywania tych argumentów, które mają typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="bd998-2256">W przypadku operacji `x op y`, gdzie `op` jest jednym z operatorów logicznych, do wybrania implementacji określonego operatora jest stosowane rozpoznawanie przeciążenia ([rozpoznawanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="bd998-2257">Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="bd998-2258">Wstępnie zdefiniowane operatory logiczne są opisane w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="bd998-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="bd998-2259">Operatory logiczne Integer</span><span class="sxs-lookup"><span data-stu-id="bd998-2259">Integer logical operators</span></span>

<span data-ttu-id="bd998-2260">Wstępnie zdefiniowane operatory logiczne integer są:</span><span class="sxs-lookup"><span data-stu-id="bd998-2260">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="bd998-2261">Operator `&` oblicza bitową koniunkcję logiczną `AND` dwóch operandów, operator `|` oblicza koniunkcję logiczną `OR` dwóch operandów, a operator `^` oblicza bitowe logiczne wyłącznych `OR` z dwóch operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="bd998-2262">Z tych operacji nie jest możliwe przepełnienie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="bd998-2263">Wyliczanie operatorów logicznych</span><span class="sxs-lookup"><span data-stu-id="bd998-2263">Enumeration logical operators</span></span>

<span data-ttu-id="bd998-2264">Każdy typ wyliczeniowy `E` niejawnie udostępnia następujące wstępnie zdefiniowane operatory logiczne:</span><span class="sxs-lookup"><span data-stu-id="bd998-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="bd998-2265">Wynik oceny `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczenia `E` z typem podstawowym `U`, a `op` jest jednym z operatorów logicznych, jest dokładnie taka sama jak Ocena `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="bd998-2266">Innymi słowy operatory logiczne typu wyliczeniowy po prostu wykonują operacje logiczne na podstawowym typie dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="bd998-2267">Operatory logiczne (Boolean)</span><span class="sxs-lookup"><span data-stu-id="bd998-2267">Boolean logical operators</span></span>

<span data-ttu-id="bd998-2268">Wstępnie zdefiniowane operatory logiczne Boolean to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="bd998-2269">Wynik `x & y` jest `true`, jeśli oba `x` i `y` są `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="bd998-2270">W przeciwnym razie wynik jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="bd998-2271">Wynik `x | y` jest `true`, jeśli `x` lub `y` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="bd998-2272">W przeciwnym razie wynik jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="bd998-2273">Wynik `x ^ y` to `true`, jeśli `x` jest `true` i `y` jest `false` lub `x` jest `false` i `y` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="bd998-2274">W przeciwnym razie wynik jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="bd998-2275">Gdy operandy są typu `bool`, operator `^` oblicza ten sam wynik jako operator `!=`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="bd998-2276">Logiczne operatory wartości null</span><span class="sxs-lookup"><span data-stu-id="bd998-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="bd998-2277">Typ Boolean dopuszczający wartość null `bool?` może reprezentować trzy wartości, `true`, `false` i `null`, i jest koncepcyjnie podobny do typu, który jest używany dla wyrażeń logicznych w SQL.</span><span class="sxs-lookup"><span data-stu-id="bd998-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="bd998-2278">Aby upewnić się, że wyniki utworzone przez operatory `&` i `|` dla argumentów operacji `bool?` są spójne z logiką trzech wartości, są dostępne następujące wstępnie zdefiniowane operatory:</span><span class="sxs-lookup"><span data-stu-id="bd998-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="bd998-2279">Poniższa tabela zawiera listę wyników wytwarzanych przez te operatory dla wszystkich kombinacji wartości `true`, `false` i `null`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="bd998-2280">Warunkowe operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-2280">Conditional logical operators</span></span>

<span data-ttu-id="bd998-2281">Operatory `&&` i `||` są nazywane warunkowymi operatorami logicznymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="bd998-2282">Są one również nazywane operatorami logicznymi "krótkie-obwóding".</span><span class="sxs-lookup"><span data-stu-id="bd998-2282">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="bd998-2283">Operatory `&&` i `||` to warunkowe wersje operatorów `&` i `|`:</span><span class="sxs-lookup"><span data-stu-id="bd998-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="bd998-2284">Operacja `x && y` odpowiada operacji `x & y`, z tą różnicą, że `y` jest oceniane tylko wtedy, gdy `x` nie jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="bd998-2285">Operacja `x || y` odpowiada operacji `x | y`, z tą różnicą, że `y` jest oceniane tylko wtedy, gdy `x` nie jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="bd998-2286">Jeśli argument operacji warunkowego operatora logicznego ma typ czasu kompilacji `dynamic`, to wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-2287">W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania przy użyciu typu czasu wykonywania tych argumentów, które mają typ czasu kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="bd998-2288">Operacja `x && y` lub `x || y` jest przetwarzana przez zastosowanie rozdzielczości przeciążenia ([rozpoznawanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)), tak jakby operacja była zapisywana `x & y` lub `x | y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="bd998-2289">Następnie</span><span class="sxs-lookup"><span data-stu-id="bd998-2289">Then,</span></span>

*  <span data-ttu-id="bd998-2290">Jeśli rozwiązanie przeciążenia nie może znaleźć pojedynczego najlepszego operatora lub w przypadku rozpoznawania przeciążenia wybiera jeden ze wstępnie zdefiniowanych wartości całkowitych, występuje błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="bd998-2291">W przeciwnym razie, jeśli wybrany operator jest jednym ze wstępnie zdefiniowanych logicznych operatorów logicznych ([Boolean logiczne operatory](expressions.md#boolean-logical-operators)) lub logicznej wartości null operatorów logicznych (wartości[null logiczne operator logiczny](expressions.md#nullable-boolean-logical-operators)), operacja jest przetwarzana zgodnie z opisem w [ Logiczne warunkowe operatory logiczne](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="bd998-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="bd998-2292">W przeciwnym razie wybrany operator jest operatorem zdefiniowanym przez użytkownika, a operacja jest przetwarzana zgodnie z opisem w [warunkowych operatory logicznej zdefiniowanej przez użytkownika](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="bd998-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="bd998-2293">Nie można bezpośrednio przeciążać warunkowych operatorów logicznych.</span><span class="sxs-lookup"><span data-stu-id="bd998-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="bd998-2294">Jednak ze względu na to, że warunkowe operatory logiczne są oceniane pod względem zwykłych operatorów logicznych, przeciążenia zwykłych operatorów logicznych są, z pewnymi ograniczeniami, również jako przeciążenia warunkowych operatorów logicznych.</span><span class="sxs-lookup"><span data-stu-id="bd998-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="bd998-2295">Opisano to dokładniej w [warunkowych operatory logicznej zdefiniowanej przez użytkownika](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="bd998-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="bd998-2296">Logiczne warunkowe operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="bd998-2297">Gdy operandy `&&` lub `||` są typu `bool` lub gdy operandy są typami, które nie definiują odpowiednich `operator &` lub `operator |`, ale definiują konwersje niejawne na `bool`, operacja jest przetwarzana w następujący sposób. :</span><span class="sxs-lookup"><span data-stu-id="bd998-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="bd998-2298">Operacja `x && y` jest szacowana jako `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="bd998-2299">Innymi słowy, `x` jest najpierw oceniane i konwertowane na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="bd998-2300">Następnie, jeśli `x` jest `true`, `y` jest oceniane i konwertowane na typ `bool` i zostanie to wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="bd998-2301">W przeciwnym razie wynik operacji jest `false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="bd998-2302">Operacja `x || y` jest szacowana jako `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="bd998-2303">Innymi słowy, `x` jest najpierw oceniane i konwertowane na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="bd998-2304">Następnie Jeśli `x` jest `true`, wynik operacji jest `true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="bd998-2305">W przeciwnym razie `y` jest oceniane i konwertowane na typ `bool` i zostanie to wynikiem operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="bd998-2306">Operatory logiczne warunkowe zdefiniowane przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="bd998-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="bd998-2307">Gdy operandy `&&` lub `||` są typu, który deklaruje odpowiednie zdefiniowane przez użytkownika `operator &` lub `operator |`, obie poniższe wartości muszą mieć wartość true, gdzie `T` jest typem, w którym zadeklarowano wybrany operator:</span><span class="sxs-lookup"><span data-stu-id="bd998-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="bd998-2308">Typem zwracanym i typem każdego parametru wybranego operatora musi być `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="bd998-2309">Innymi słowy operator musi obliczać logiczne `AND` lub logiczne `OR` dwóch operandów typu `T` i zwracać wynik typu `T`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="bd998-2310">`T` musi zawierać deklaracje `operator true` i `operator false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="bd998-2311">Jeśli jedno z tych wymagań nie zostanie spełnione, występuje błąd w czasie trwania powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="bd998-2312">W przeciwnym razie operacja `&&` lub `||` jest Szacowana przez połączenie zdefiniowanej przez użytkownika `operator true` lub `operator false` z wybranym operatorem zdefiniowanym przez użytkownika:</span><span class="sxs-lookup"><span data-stu-id="bd998-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="bd998-2313">Operacja `x && y` jest szacowana jako `T.false(x) ? x : T.&(x, y)`, gdzie `T.false(x)` to wywołanie `operator false` zadeklarowane w `T`, a `T.&(x, y)` to wywołanie wybranej `operator &`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="bd998-2314">Innymi słowy, `x` jest najpierw oceniane i `operator false` jest wywoływana w wyniku, aby określić, czy `x` ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="bd998-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="bd998-2315">Następnie, jeśli `x` ma wartość false, wynik operacji jest wartością obliczoną wcześniej dla `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="bd998-2316">W przeciwnym razie zostanie obliczona wartość `y`, a wybrana `operator &` jest wywoływana na wartości obliczonej wcześniej dla `x` i wartości obliczanej dla `y`, aby utworzyć wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="bd998-2317">Operacja `x || y` jest szacowana jako `T.true(x) ? x : T.|(x, y)`, gdzie `T.true(x)` to wywołanie `operator true` zadeklarowane w `T`, a `T.|(x,y)` to wywołanie wybranej `operator|`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="bd998-2318">Innymi słowy, `x` jest najpierw oceniane i `operator true` jest wywoływana w wyniku, aby określić, czy `x` ma wartość "prawda".</span><span class="sxs-lookup"><span data-stu-id="bd998-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="bd998-2319">Następnie, jeśli wartość `x` jest w nieskończoność, wynik operacji jest wartością obliczoną wcześniej dla `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="bd998-2320">W przeciwnym razie zostanie obliczona wartość `y`, a wybrana `operator |` jest wywoływana na wartości obliczonej wcześniej dla `x` i wartości obliczanej dla `y`, aby utworzyć wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="bd998-2321">W jednej z tych operacji wyrażenie podane przez `x` jest oceniane tylko raz, a wyrażenie podane przez `y` nie jest oceniane lub oceniane dokładnie raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="bd998-2322">Aby zapoznać się z przykładem typu, który implementuje `operator true` i `operator false`, zobacz [typ Boolean bazy danych](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="bd998-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="bd998-2323">Operator łączenia wartości null</span><span class="sxs-lookup"><span data-stu-id="bd998-2323">The null coalescing operator</span></span>

<span data-ttu-id="bd998-2324">Operator `??` jest nazywany operatorem łączenia wartości null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="bd998-2325">Wyrażenie łączenia o wartości null w formularzu `a ?? b` wymaga, aby `a` było typu dopuszczającego wartość null lub typ referencyjny.</span><span class="sxs-lookup"><span data-stu-id="bd998-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="bd998-2326">Jeśli `a` ma wartość różną od null, wynik `a ?? b` jest `a`; w przeciwnym razie wynik jest `b`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="bd998-2327">Operacja oblicza `b` tylko wtedy, gdy `a` ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="bd998-2328">Operator łączenia wartości null jest prawym przyciskiem asocjacyjnym, co oznacza, że operacje są pogrupowane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="bd998-2329">Na przykład wyrażenie formularza `a ?? b ?? c` jest oceniane jako `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="bd998-2330">Ogólnie rzecz biorąc wyrażenie formularza `E1 ?? E2 ?? ... ?? En` zwraca pierwsze operandy o wartości innej niż null lub wartość null, jeśli wszystkie operandy mają wartość null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="bd998-2331">Typ wyrażenia `a ?? b` zależy od tego, które konwersje niejawne są dostępne dla operandów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="bd998-2332">W kolejności preferencji typ `a ?? b` to `A0`, `A` lub `B`, gdzie `A` jest typem `a` (pod warunkiem, że `a` ma typ), `B` jest typem `b` (pod warunkiem, że `b` ma typ) , a 0 jest podstawowym typem 1, jeśli 2 jest typem dopuszczającym wartość null lub 3 w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="bd998-2333">W odniesieniu do `a ?? b` jest przetwarzana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="bd998-2334">Jeśli `A` istnieje i nie jest typem dopuszczającym wartość null lub typem referencyjnym, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="bd998-2335">Jeśli `b` jest wyrażeniem dynamicznym, typem wyniku jest `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="bd998-2336">W czasie wykonywania `a` jest najpierw oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="bd998-2337">Jeśli `a` nie ma wartości null, `a` jest konwertowany na dynamiczny i zostanie to wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="bd998-2338">W przeciwnym razie `b` jest oceniane i zostanie to wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="bd998-2339">W przeciwnym razie, jeśli `A` istnieje i jest typem dopuszczającym wartość null i istnieje niejawna konwersja z `b` na `A0`, typ wyniku to `A0`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="bd998-2340">W czasie wykonywania `a` jest najpierw oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="bd998-2341">Jeśli `a` nie ma wartości null, `a` jest nieopakowany do typu `A0` i zostanie to wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="bd998-2342">W przeciwnym razie `b` jest oceniane i konwertowane na typ `A0` i zostanie to wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="bd998-2343">W przeciwnym razie, jeśli `A` istnieje i istnieje niejawna konwersja z `b` na `A`, typ wyniku to `A`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="bd998-2344">W czasie wykonywania `a` jest najpierw oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="bd998-2345">Jeśli `a` nie ma wartości null, `a` zmieni wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="bd998-2346">W przeciwnym razie `b` jest oceniane i konwertowane na typ `A` i zostanie to wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="bd998-2347">W przeciwnym razie, jeśli `b` ma typ `B` i niejawna konwersja istnieje z `a` do `B`, typ wyniku to `B`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="bd998-2348">W czasie wykonywania `a` jest najpierw oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="bd998-2349">Jeśli `a` nie ma wartości null, `a` jest nieopakowany do typu `A0` (jeśli `A` istnieje i dopuszcza wartość null) i konwertowane na typ `B` i zostanie to wynik.</span><span class="sxs-lookup"><span data-stu-id="bd998-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="bd998-2350">W przeciwnym razie `b` jest oceniana i będzie wynikiem.</span><span class="sxs-lookup"><span data-stu-id="bd998-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="bd998-2351">W przeciwnym razie `a` i `b` są niezgodne, a wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="bd998-2352">Operator warunkowy</span><span class="sxs-lookup"><span data-stu-id="bd998-2352">Conditional operator</span></span>

<span data-ttu-id="bd998-2353">Operator `?:` jest nazywany operatorem warunkowym.</span><span class="sxs-lookup"><span data-stu-id="bd998-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="bd998-2354">Jest ona czasem nazywana również operatorem Trzyelementowy.</span><span class="sxs-lookup"><span data-stu-id="bd998-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="bd998-2355">Wyrażenie warunkowe formularza `b ? x : y` najpierw oblicza warunek `b`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="bd998-2356">Następnie, jeśli `b` to `true`, `x` zostanie obliczone i zostanie wynikiem operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="bd998-2357">W przeciwnym razie `y` jest oceniane i będzie wynikiem operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="bd998-2358">Wyrażenie warunkowe nigdy nie oblicza jednocześnie wartości `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="bd998-2359">Operator warunkowy jest z prawej strony asocjacyjny, co oznacza, że operacje są pogrupowane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="bd998-2360">Na przykład wyrażenie formularza `a ? b : c ? d : e` jest oceniane jako `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="bd998-2361">Pierwszy operand operatora `?:` musi być wyrażeniem, które może być niejawnie konwertowane na `bool` lub wyrażenie typu, który implementuje `operator true`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="bd998-2362">Jeśli żadne z tych wymagań nie są spełnione, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="bd998-2363">Drugi i trzeci operand, `x` i `y`, dla operatora `?:` kontrolują typ wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="bd998-2364">Jeśli `x` ma typ `X` i `y` ma typ `Y` then</span><span class="sxs-lookup"><span data-stu-id="bd998-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="bd998-2365">Jeśli niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `X` do `Y`, ale nie z `Y` do `X`, wówczas `Y` jest typem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="bd998-2366">Jeśli niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `Y` do `X`, ale nie z `X` do `Y`, wówczas `X` jest typem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="bd998-2367">W przeciwnym razie nie można określić typu wyrażenia i występuje błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="bd998-2368">Jeśli tylko jeden z `x` i `y` ma typ, a zarówno `x`, jak i `y`, z są niejawnie konwertowane do tego typu, to jest typem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="bd998-2369">W przeciwnym razie nie można określić typu wyrażenia i występuje błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="bd998-2370">Przetwarzanie wyrażenia warunkowego w postaci `b ? x : y` w czasie wykonywania obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="bd998-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-2371">Najpierw jest obliczana wartość `b` i jest określana `bool` `b`:</span><span class="sxs-lookup"><span data-stu-id="bd998-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="bd998-2372">Jeśli istnieje niejawna konwersja z typu `b` na `bool`, to niejawna konwersja jest wykonywana w celu utworzenia wartości `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="bd998-2373">W przeciwnym razie `operator true` zdefiniowany przez typ `b` jest wywoływana w celu utworzenia wartości `bool`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="bd998-2374">Jeśli wartość `bool` utworzona przez krok powyżej to `true`, wówczas `x` jest obliczana i konwertowana na typ wyrażenia warunkowego, a to wynik wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="bd998-2375">W przeciwnym razie `y` jest oceniane i konwertowane na typ wyrażenia warunkowego, a następnie zostanie wynikiem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="bd998-2376">Wyrażenia funkcji anonimowych</span><span class="sxs-lookup"><span data-stu-id="bd998-2376">Anonymous function expressions</span></span>

<span data-ttu-id="bd998-2377">***Funkcja anonimowa*** jest wyrażeniem, które reprezentuje definicję metody "w wierszu".</span><span class="sxs-lookup"><span data-stu-id="bd998-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="bd998-2378">Funkcja anonimowa nie ma wartości ani typu w i sama, ale jest możliwa do przekonwertowania na zgodny delegat lub typ drzewa wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="bd998-2379">Obliczanie konwersji funkcji anonimowej zależy od typu docelowego konwersji: Jeśli jest typem delegata, konwersja szacuje się na wartość delegata odwołującą się do metody, która definiuje funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="bd998-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="bd998-2380">Jeśli jest to typ drzewa wyrażenia, konwersja szacuje się na drzewo wyrażenia, które reprezentuje strukturę metody jako strukturę obiektu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="bd998-2381">Z przyczyn historycznych istnieją dwa rodzaje składni funkcji anonimowych, a mianowicie *lambda_expression*s i *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="bd998-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="bd998-2382">Dla niemal wszystkich celów *lambda_expression*s są bardziej zwięzłe i bardziej wyraźne niż *anonymous_method_expression*s, które pozostaną w języku w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="bd998-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="bd998-2383">Operator `=>` ma takie samo pierwszeństwo jak przypisanie (`=`) i jest skojarzony z prawej strony.</span><span class="sxs-lookup"><span data-stu-id="bd998-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="bd998-2384">Funkcja anonimowa z modyfikatorem `async` jest funkcją asynchroniczną i jest zgodna z regułami opisanymi w [iteratorach](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="bd998-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="bd998-2385">Parametry funkcji anonimowej w postaci *lambda_expression* można jawnie lub niejawnie wpisać.</span><span class="sxs-lookup"><span data-stu-id="bd998-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="bd998-2386">Na liście jawnie wpisanych parametrów typ każdego parametru jest jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="bd998-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="bd998-2387">W przypadku niejawnie wpisanej listy parametrów typy parametrów są wywnioskowane z kontekstu, w którym występuje funkcja anonimowa — w przypadku gdy funkcja anonimowa jest konwertowana na zgodny typ delegata lub typ drzewa wyrażenia, ten typ zapewnia typy parametrów ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="bd998-2388">W funkcji anonimowej z pojedynczym niejawnie określonym parametrem, nawiasy można pominąć z listy parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="bd998-2389">Innymi słowy, funkcja anonimowa formularza</span><span class="sxs-lookup"><span data-stu-id="bd998-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="bd998-2390">może być skrócony do</span><span class="sxs-lookup"><span data-stu-id="bd998-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="bd998-2391">Lista parametrów funkcji anonimowej w formie *anonymous_method_expression* jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="bd998-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="bd998-2392">W przypadku podaną wartość parametry muszą być jawnie wpisane.</span><span class="sxs-lookup"><span data-stu-id="bd998-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="bd998-2393">Jeśli nie, funkcja anonimowa jest konwertowany na delegata z dowolną listą parametrów, która nie zawiera `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="bd998-2394">Treść *bloku* funkcji anonimowej jest osiągalna ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)), chyba że funkcja anonimowa występuje wewnątrz nieosiągalnej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="bd998-2395">Poniżej przedstawiono kilka przykładów funkcji anonimowych:</span><span class="sxs-lookup"><span data-stu-id="bd998-2395">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="bd998-2396">Zachowanie *lambda_expression*s i *anonymous_method_expression*s jest takie samo, z wyjątkiem następujących punktów:</span><span class="sxs-lookup"><span data-stu-id="bd998-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="bd998-2397">*anonymous_method_expression*s zezwala, aby lista parametrów została pominięta całkowicie, co daje Convertibility do delegowania typów dowolnej listy parametrów wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="bd998-2398">*lambda_expression*s Zezwalaj na pomijanie typów parametrów i wywnioskowano, a *anonymous_method_expression*s wymaga jawnego określenia typów parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="bd998-2399">Treść elementu *lambda_expression* może być wyrażeniem lub blokiem instrukcji, a treść elementu *anonymous_method_expression* musi być blokiem instrukcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="bd998-2400">Tylko *lambda_expression*s mają konwersje na zgodne typy drzewa wyrażeń ([Typy drzewa wyrażeń](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="bd998-2401">Sygnatury funkcji anonimowych</span><span class="sxs-lookup"><span data-stu-id="bd998-2401">Anonymous function signatures</span></span>

<span data-ttu-id="bd998-2402">Opcjonalna *anonymous_function_signature* funkcji anonimowej definiuje nazwy i opcjonalnie typy parametrów formalnych dla funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="bd998-2403">Zakres parametrów funkcji anonimowej to *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="bd998-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="bd998-2404">([Zakresy](basic-concepts.md#scopes)) Wraz z listą parametrów (jeśli jest to określone), Metoda anonimowa ([Deklaracja) stanowi](basic-concepts.md#declarations)przestrzeń deklaracji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="bd998-2405">Jest to błąd czasu kompilacji dla nazwy parametru funkcji anonimowej, aby dopasować nazwę zmiennej lokalnej, stałej lokalnej lub parametru, którego zakres zawiera *anonymous_method_expression* lub *lambda_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="bd998-2406">Jeśli funkcja anonimowa ma *explicit_anonymous_function_signature*, zestaw zgodnych typów delegatów i typów drzewa wyrażeń jest ograniczony do tych, które mają te same typy parametrów i Modyfikatory w tej samej kolejności.</span><span class="sxs-lookup"><span data-stu-id="bd998-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="bd998-2407">W przeciwieństwie do konwersji grup metod ([konwersje grup metod](conversions.md#method-group-conversions)), antywariancja typów parametrów funkcji anonimowych nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="bd998-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="bd998-2408">Jeśli funkcja anonimowa nie ma *anonymous_function_signature*, to zestaw zgodnych typów delegatów i typów drzewa wyrażeń jest ograniczony do tych, które nie mają `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="bd998-2409">Należy zauważyć, że element *anonymous_function_signature* nie może zawierać atrybutów ani tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="bd998-2410">Niemniej jednak *anonymous_function_signature* może być zgodny z typem delegata, którego lista parametrów zawiera tablicę parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="bd998-2411">Należy również pamiętać, że konwersja na typ drzewa wyrażenia, nawet jeśli jest zgodna, może nadal kończyć się niepowodzeniem w czasie kompilacji ([Typy drzewa wyrażeń](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="bd998-2412">Anonimowe treści funkcji</span><span class="sxs-lookup"><span data-stu-id="bd998-2412">Anonymous function bodies</span></span>

<span data-ttu-id="bd998-2413">Treść (*wyrażenie* lub *blok*) funkcji anonimowej podlega następującym zasadom:</span><span class="sxs-lookup"><span data-stu-id="bd998-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="bd998-2414">Jeśli funkcja anonimowa zawiera podpis, parametry określone w podpisie są dostępne w treści.</span><span class="sxs-lookup"><span data-stu-id="bd998-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="bd998-2415">Jeśli funkcja anonimowa nie ma podpisu, można ją przekonwertować na typ delegata lub typ wyrażenia z parametrami ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)), ale w treści nie można uzyskać dostępu do parametrów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="bd998-2416">Z wyjątkiem parametrów `ref` lub `out` określonych w sygnaturze (jeśli istnieje) najbliższej otaczającej funkcji anonimowej, jest to błąd czasu kompilacji dla treści, aby uzyskać dostęp do parametru `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="bd998-2417">Gdy typ `this` jest typem struktury, jest to błąd czasu kompilacji dla treści w celu uzyskania dostępu do `this`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="bd998-2418">Jest to prawdziwe, czy dostęp jest jawny (jak w `this.x`) czy niejawny (jak w `x`, gdzie `x` to element członkowski wystąpienia struktury).</span><span class="sxs-lookup"><span data-stu-id="bd998-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="bd998-2419">Ta zasada po prostu uniemożliwia takiemu dostępowi i nie wpływa na to, czy wyniki wyszukiwania elementów członkowskich są elementami członkowskimi struktury.</span><span class="sxs-lookup"><span data-stu-id="bd998-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="bd998-2420">Treść ma dostęp do zmiennych zewnętrznych ([zmiennych zewnętrznych](expressions.md#outer-variables)) funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="bd998-2421">Dostęp do zmiennej zewnętrznej będzie odwoływać się do wystąpienia zmiennej, która jest aktywna w czasie, gdy *lambda_expression* lub *anonymous_method_expression* jest oceniane ([Obliczanie wyrażeń funkcji anonimowych](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="bd998-2422">Jest to błąd czasu kompilacji dla treści, który zawiera instrukcję `goto` instrukcji `break` lub instrukcji `continue`, której element docelowy znajduje się poza treścią lub w treści zawartej funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="bd998-2423">Instrukcja `return` w treści zwraca kontrolę z wywołania najbliższej otaczającej funkcji anonimowej, a nie z otaczającego elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="bd998-2424">Wyrażenie określone w instrukcji `return` musi być niejawnie konwertowane na typ zwracany typu delegata lub typu drzewa wyrażenia, do którego jest konwertowana najbliższe *lambda_expression* lub *anonymous_method_expression* ( [Konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="bd998-2425">Jest on jawnie nieokreślony, niezależnie od tego, czy istnieje sposób, aby wykonać blok anonimowej funkcji innej niż Ocena i wywołanie *lambda_expression* lub *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="bd998-2426">W szczególności kompilator może zdecydować się na zaimplementowanie funkcji anonimowej przez syntezowanie jednej lub więcej nazwanych metod lub typów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="bd998-2427">Nazwy wszelkich takich elementów, które można wyszukiwać, muszą mieć postać zastrzeżona do użycia przez kompilator.</span><span class="sxs-lookup"><span data-stu-id="bd998-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="bd998-2428">Rozpoznawanie przeciążenia i funkcje anonimowe</span><span class="sxs-lookup"><span data-stu-id="bd998-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="bd998-2429">Funkcje anonimowe na liście argumentów uczestniczą w wnioskach o typie i przeciążeniu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="bd998-2430">Aby uzyskać dokładne reguły, zapoznaj się z [wnioskami o typie](expressions.md#type-inference) i [rozpoznaniem przeciążenia](expressions.md#overload-resolution) .</span><span class="sxs-lookup"><span data-stu-id="bd998-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="bd998-2431">Poniższy przykład ilustruje efekt funkcji anonimowych podczas rozpoznawania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="bd998-2432">Klasa `ItemList<T>` ma dwie metody `Sum`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="bd998-2433">Każdy przyjmuje argument `selector`, który wyodrębnia wartość do sumowania z elementu listy.</span><span class="sxs-lookup"><span data-stu-id="bd998-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="bd998-2434">Wyodrębniona wartość może być równa `int` lub `double`, a wynikiem podsumowania jest zarówno `int`, jak i `double`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="bd998-2435">Metody `Sum` mogą na przykład służyć do obliczania sum z listy wierszy szczegółów w kolejności.</span><span class="sxs-lookup"><span data-stu-id="bd998-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="bd998-2436">W pierwszym wywołaniu `orderDetails.Sum` stosowane są obie metody `Sum`, ponieważ funkcja anonimowa `d => d. UnitCount` jest zgodna zarówno z `Func<Detail,int>`, jak i `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="bd998-2437">Jednak rozwiązanie przeciążenia wybiera pierwszą metodę `Sum`, ponieważ konwersja do `Func<Detail,int>` jest lepsza niż konwersja do `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="bd998-2438">W drugim wywołaniu `orderDetails.Sum` jest stosowana tylko druga metoda `Sum`, ponieważ funkcja anonimowa `d => d.UnitPrice * d.UnitCount` generuje wartość typu `double`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="bd998-2439">W rezultacie rozwiązanie przeciążenia wybiera drugą metodę `Sum` dla tego wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="bd998-2440">Funkcje anonimowe i powiązanie dynamiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="bd998-2441">Funkcja anonimowa nie może być odbiorcą, argumentem lub argumentem operacji z dynamiczną operacją.</span><span class="sxs-lookup"><span data-stu-id="bd998-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="bd998-2442">Zmienne zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="bd998-2442">Outer variables</span></span>

<span data-ttu-id="bd998-2443">Każda zmienna lokalna, parametr wartości lub tablica parametrów, której zakres zawiera *lambda_expression* lub *anonymous_method_expression* jest nazywany ***zewnętrzną zmienną*** funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="bd998-2444">W składowej funkcji wystąpienia klasy wartość `this` jest uważana za parametr wartości i jest zewnętrzną zmienną każdej funkcji anonimowej zawartej w składowej funkcji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="bd998-2445">Przechwycone zmienne zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="bd998-2445">Captured outer variables</span></span>

<span data-ttu-id="bd998-2446">Gdy do zmiennej zewnętrznej odwołuje się funkcja anonimowa, zmienna zewnętrzna jest określana jako ***przechwycona*** przez funkcję anonimową.</span><span class="sxs-lookup"><span data-stu-id="bd998-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="bd998-2447">Zwykle okres istnienia zmiennej lokalnej jest ograniczony do wykonywania bloku lub instrukcji, z którą jest skojarzony ([zmienne lokalne](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="bd998-2448">Jednak okres istnienia przechwyconej zmiennej zewnętrznej jest rozszerzany co najmniej do momentu, aż obiekt delegowany lub drzewo wyrażenia utworzone za pomocą funkcji anonimowej staną się uprawnione do wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="bd998-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="bd998-2449">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-2449">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="bd998-2450">Zmienna lokalna `x` jest przechwytywana przez funkcję anonimową, a okres istnienia `x` jest rozszerzony co najmniej do momentu, gdy delegat zwrócony przez `F` będzie uprawniony do wyrzucania elementów bezużytecznych (które nie są wykonywane do momentu zakończenia tego programu).</span><span class="sxs-lookup"><span data-stu-id="bd998-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="bd998-2451">Ponieważ każde wywołanie funkcji anonimowej działa w tym samym wystąpieniu `x`, dane wyjściowe przykład to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="bd998-2452">Gdy zmienna lokalna lub parametr wartości jest przechwytywany przez funkcję anonimową, zmienna lub parametr lokalny nie jest już traktowany jako zmienna stała ([zmienne stałe i ruchome](unsafe-code.md#fixed-and-moveable-variables)), ale zamiast tego jest traktowana jako ruchoma zmienna.</span><span class="sxs-lookup"><span data-stu-id="bd998-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="bd998-2453">W ten sposób każdy kod `unsafe`, który pobiera adres przechwyconej zmiennej zewnętrznej, musi najpierw użyć instrukcji `fixed` do naprawienia zmiennej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="bd998-2454">Należy pamiętać, że w przeciwieństwie do zmiennej nieprzechwyconej, przechwycona zmienna lokalna może być jednocześnie narażona na wiele wątków wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="bd998-2455">Tworzenie wystąpienia zmiennych lokalnych</span><span class="sxs-lookup"><span data-stu-id="bd998-2455">Instantiation of local variables</span></span>

<span data-ttu-id="bd998-2456">Zmienna lokalna jest uznawana za ***wystąpienie*** , gdy wykonanie wejdzie w zakres zmiennej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="bd998-2457">Na przykład po wywołaniu następującej metody zmienna lokalna `x` jest tworzona i inicjowana trzy razy — raz dla każdej iteracji pętli.</span><span class="sxs-lookup"><span data-stu-id="bd998-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="bd998-2458">Jednak przeniesienie deklaracji `x` poza pętlą skutkuje pojedynczym wystąpieniem `x`:</span><span class="sxs-lookup"><span data-stu-id="bd998-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="bd998-2459">Gdy nie przechwycono, nie ma możliwości dokładnego zaobserwowania, jak często jest tworzona zmienna lokalna — ponieważ okresy istnienia wystąpień są rozłączane, istnieje możliwość, że każde wystąpienie będzie używać tej samej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="bd998-2460">Jednak gdy funkcja anonimowa przechwytuje zmienną lokalną, efekty tworzenia wystąpienia stają się oczywiste.</span><span class="sxs-lookup"><span data-stu-id="bd998-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="bd998-2461">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2461">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="bd998-2462">tworzy dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="bd998-2463">Jednakże gdy deklaracja `x` jest przenoszona poza pętlę:</span><span class="sxs-lookup"><span data-stu-id="bd998-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="bd998-2464">Dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="bd998-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="bd998-2465">Jeśli pętla for deklaruje zmienną iteracji, ta zmienna jest uważana za zadeklarowaną poza pętlą.</span><span class="sxs-lookup"><span data-stu-id="bd998-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="bd998-2466">W tym przypadku, jeśli przykład zostanie zmieniony w celu przechwycenia samej zmiennej iteracji:</span><span class="sxs-lookup"><span data-stu-id="bd998-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="bd998-2467">przechwytywane jest tylko jedno wystąpienie zmiennej iteracji, co daje wynik:</span><span class="sxs-lookup"><span data-stu-id="bd998-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="bd998-2468">Istnieje możliwość, że Delegaty funkcji anonimowych do udostępniania niektórych przechwyconych zmiennych jeszcze mają osobne wystąpienia innych.</span><span class="sxs-lookup"><span data-stu-id="bd998-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="bd998-2469">Na przykład jeśli `F` zostanie zmieniony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2469">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="bd998-2470">trzy Delegaty przechwytują to samo wystąpienie `x`, ale oddzielne wystąpienia `y`, a dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="bd998-2471">Oddzielne funkcje anonimowe mogą przechwytywać to samo wystąpienie zmiennej zewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="bd998-2472">W przykładzie:</span><span class="sxs-lookup"><span data-stu-id="bd998-2472">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="bd998-2473">dwie funkcje anonimowe przechwytują to samo wystąpienie zmiennej lokalnej `x`, a tym samym "komunikują się" za pomocą tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="bd998-2474">Dane wyjściowe przykładu to:</span><span class="sxs-lookup"><span data-stu-id="bd998-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="bd998-2475">Obliczanie wyrażeń funkcji anonimowych</span><span class="sxs-lookup"><span data-stu-id="bd998-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="bd998-2476">Funkcja anonimowa `F` musi być zawsze konwertowana na typ delegata `D` lub typ drzewa wyrażenia `E`, bezpośrednio lub przez wykonanie wyrażenia tworzenia delegata `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="bd998-2477">Ta konwersja określa wynik funkcji anonimowej, zgodnie z opisem w [konwersji funkcji anonimowych](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="bd998-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="bd998-2478">Wyrażenia zapytań</span><span class="sxs-lookup"><span data-stu-id="bd998-2478">Query expressions</span></span>

<span data-ttu-id="bd998-2479">***Wyrażenia zapytania*** zapewniają składnię języka zintegrowanego dla zapytań, które są podobne do relacyjnych i hierarchicznych języków zapytań, takich jak SQL i XQuery.</span><span class="sxs-lookup"><span data-stu-id="bd998-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="bd998-2480">Wyrażenie zapytania rozpoczyna się od klauzuli `from` i jest kończyć klauzulą `select` lub `group`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="bd998-2481">Po początkowej klauzuli `from` może następować zero lub więcej `from`, `let`, `where`, `join` lub `orderby` klauzule.</span><span class="sxs-lookup"><span data-stu-id="bd998-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="bd998-2482">Każda klauzula `from` jest generatorem wprowadzającym ***zmienną zakresu*** , która przekracza elementy ***sekwencji***.</span><span class="sxs-lookup"><span data-stu-id="bd998-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="bd998-2483">Każda klauzula `let` wprowadza zmienną zakresu reprezentującą wartość obliczoną przez metodę dla poprzednich zmiennych zakresu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="bd998-2484">Każda klauzula `where` jest filtrem, który wyklucza elementy z wyniku.</span><span class="sxs-lookup"><span data-stu-id="bd998-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="bd998-2485">Każda klauzula `join` porównuje określone klucze sekwencji źródłowej z kluczami innej sekwencji, które dają pasujące pary.</span><span class="sxs-lookup"><span data-stu-id="bd998-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="bd998-2486">Każda klauzula `orderby` zmienia kolejność elementów zgodnie z określonymi kryteriami. Ostateczna klauzula `select` lub `group` określa kształt wyniku w zakresie zmiennych zakresu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="bd998-2487">Na koniec klauzula `into` może służyć do "łączenia" zapytań, traktując wyniki jednego zapytania jako generatora w kolejnych zapytaniach.</span><span class="sxs-lookup"><span data-stu-id="bd998-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="bd998-2488">Niejasności w wyrażeniach zapytania</span><span class="sxs-lookup"><span data-stu-id="bd998-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="bd998-2489">Wyrażenia zapytań zawierają wiele "kontekstowych słów kluczowych", tj. identyfikatorów, które mają specjalne znaczenie w danym kontekście.</span><span class="sxs-lookup"><span data-stu-id="bd998-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="bd998-2490">W odróżnieniu od nich `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, 0, 1 i 2.</span><span class="sxs-lookup"><span data-stu-id="bd998-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="bd998-2491">Aby uniknąć niejasności w wyrażeniach zapytań spowodowanych mieszanym użyciem tych identyfikatorów jako słowa kluczowe lub nazw prostych, te identyfikatory są uznawane za słowa kluczowe w dowolnym miejscu w wyrażeniu zapytania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="bd998-2492">W tym celu wyrażenie zapytania jest dowolnym wyrażeniem rozpoczynającym się od "`from identifier`", po którym następuje dowolny token z wyjątkiem "`;`", "`=`" lub "`,`".</span><span class="sxs-lookup"><span data-stu-id="bd998-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="bd998-2493">Aby można było używać tych słów jako identyfikatorów w wyrażeniu zapytania, mogą one być poprzedzone prefiksem "`@`" ([identyfikatory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="bd998-2494">Tłumaczenie wyrażenia zapytania</span><span class="sxs-lookup"><span data-stu-id="bd998-2494">Query expression translation</span></span>

<span data-ttu-id="bd998-2495">C# Język nie określa semantyki wykonywania wyrażeń zapytań.</span><span class="sxs-lookup"><span data-stu-id="bd998-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="bd998-2496">Zamiast tego wyrażenia zapytania są tłumaczone na wywołania metod, które są zgodne z *wzorcem wyrażenia zapytania* ([wzorzec wyrażenia zapytania](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="bd998-2497">W szczególnych przypadkach wyrażenia zapytania są tłumaczone na wywołania metod o nazwach `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy` i 0. Te metody powinny mieć określone sygnatury i typy wyników, zgodnie z opisem w [wzorcu wyrażenia zapytania](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="bd998-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="bd998-2498">Metody te mogą być wystąpieniem metod obiektu, które są badane lub metod rozszerzających, które są zewnętrzne względem obiektu i implementują rzeczywiste wykonanie zapytania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="bd998-2499">Tłumaczenie z wyrażeń zapytania do wywołania metody jest mapowaniem składni, które występuje przed wykonaniem dowolnego powiązania typu lub Rozpoznanie przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="bd998-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="bd998-2500">Tłumaczenie jest gwarantowane pod kątem poprawności składniowej, ale nie gwarantuje, że jest to bardziej prawidłowy C# kod.</span><span class="sxs-lookup"><span data-stu-id="bd998-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="bd998-2501">Po translacji wyrażeń zapytania wyniki wywoływanych metod są przetwarzane jako zwykłe wywołania metod i może to spowodować błędy odkrywania, na przykład jeśli metody nie istnieją, jeśli argumenty mają nieprawidłowe typy lub jeśli metody są ogólne i Wnioskowanie o typie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="bd998-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="bd998-2502">Wyrażenie zapytania jest przetwarzane przez wielokrotne zastosowanie następujących tłumaczeń, dopóki nie będzie możliwe dalsze zmniejszenie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="bd998-2503">Tłumaczenia są wymienione w kolejności aplikacji: w każdej sekcji przyjęto założenie, że tłumaczenia w powyższych sekcjach zostały wykonane wyczerpująco, a po wyczerpaniu sekcja nie będzie później oglądany w przetwarzaniu tego samego wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="bd998-2504">Przypisanie do zmiennych zakresu nie jest dozwolone w wyrażeniach zapytań.</span><span class="sxs-lookup"><span data-stu-id="bd998-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="bd998-2505">Jednakże C# implementacja może nie zawsze wymuszać tego ograniczenia, ponieważ czasami nie jest to możliwe przy użyciu schematu tłumaczenia składni przedstawionego w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="bd998-2506">Niektóre tłumaczenia wprowadzają zmienne zakresów z przezroczystymi identyfikatorami wskazywanymi przez `*`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="bd998-2507">Specjalne właściwości przezroczystych identyfikatorów są omówione bardziej szczegółowo w [przezroczystych identyfikatorach](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="bd998-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="bd998-2508">Klauzule SELECT i GroupBy z kontynuacjami</span><span class="sxs-lookup"><span data-stu-id="bd998-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="bd998-2509">Wyrażenie zapytania z kontynuacją</span><span class="sxs-lookup"><span data-stu-id="bd998-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="bd998-2510">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="bd998-2511">W przypadku tłumaczeń w poniższych sekcjach przyjęto założenie, że zapytania nie mają kontynuacji `into`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="bd998-2512">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="bd998-2513">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="bd998-2514">końcowe tłumaczenie to</span><span class="sxs-lookup"><span data-stu-id="bd998-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="bd998-2515">Jawne typy zmiennych zakresu</span><span class="sxs-lookup"><span data-stu-id="bd998-2515">Explicit range variable types</span></span>

<span data-ttu-id="bd998-2516">Klauzula `from`, która jawnie określa typ zmiennej zakresu</span><span class="sxs-lookup"><span data-stu-id="bd998-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="bd998-2517">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="bd998-2518">Klauzula `join`, która jawnie określa typ zmiennej zakresu</span><span class="sxs-lookup"><span data-stu-id="bd998-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="bd998-2519">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="bd998-2520">W przypadku tłumaczeń w poniższych sekcjach przyjęto założenie, że zapytania nie mają jawnych typów zmiennych zakresu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="bd998-2521">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="bd998-2522">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="bd998-2523">końcowe tłumaczenie to</span><span class="sxs-lookup"><span data-stu-id="bd998-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="bd998-2524">Jawne typy zmiennych zakresu są przydatne do wykonywania zapytań dotyczących kolekcji implementujących interfejs nieogólny `IEnumerable`, ale nie ogólny interfejs `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="bd998-2525">W powyższym przykładzie jest to przypadek, jeśli `customers` było typu `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="bd998-2526">Wygeneruj wyrażenia zapytania</span><span class="sxs-lookup"><span data-stu-id="bd998-2526">Degenerate query expressions</span></span>

<span data-ttu-id="bd998-2527">Wyrażenie zapytania w formularzu</span><span class="sxs-lookup"><span data-stu-id="bd998-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="bd998-2528">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="bd998-2529">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="bd998-2530">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="bd998-2531">Wygeneruj wyrażenie zapytania, które w sposób nieprosty wybiera elementy źródła.</span><span class="sxs-lookup"><span data-stu-id="bd998-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="bd998-2532">Późniejsza faza tłumaczenia powoduje usunięcie niegenerowanych zapytań, które zostały wprowadzone przez inne etapy tłumaczenia, zastępując je ich źródłem.</span><span class="sxs-lookup"><span data-stu-id="bd998-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="bd998-2533">Ważne jest jednak, aby upewnić się, że wynik wyrażenia zapytania nigdy nie jest obiektem źródłowym, co spowoduje ujawnienie typu i tożsamości źródła do klienta zapytania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="bd998-2534">W związku z tym ten krok chroni wygenerowanie zapytań pisanych bezpośrednio w kodzie źródłowym przez jawne wywołanie `Select` na źródle.</span><span class="sxs-lookup"><span data-stu-id="bd998-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="bd998-2535">Następnie do realizatorów `Select` i innych operatorów zapytań, aby upewnić się, że te metody nigdy nie zwracają samego obiektu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="bd998-2536">Klauzule from, let, WHERE, join i OrderBy</span><span class="sxs-lookup"><span data-stu-id="bd998-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="bd998-2537">Wyrażenie zapytania z drugą klauzulą `from`, po której następuje klauzula `select`</span><span class="sxs-lookup"><span data-stu-id="bd998-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="bd998-2538">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="bd998-2539">Wyrażenie zapytania z drugą klauzulą `from`, po której występuje coś innego niż klauzula `select`:</span><span class="sxs-lookup"><span data-stu-id="bd998-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="bd998-2540">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="bd998-2541">Wyrażenie zapytania z klauzulą `let`</span><span class="sxs-lookup"><span data-stu-id="bd998-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="bd998-2542">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="bd998-2543">Wyrażenie zapytania z klauzulą `where`</span><span class="sxs-lookup"><span data-stu-id="bd998-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="bd998-2544">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="bd998-2545">Wyrażenie zapytania z klauzulą `join` bez `into`, po którym następują klauzulę `select`</span><span class="sxs-lookup"><span data-stu-id="bd998-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="bd998-2546">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="bd998-2547">Wyrażenie zapytania z klauzulą `join` bez `into`, po którym występuje coś innego niż `select` klauzuli</span><span class="sxs-lookup"><span data-stu-id="bd998-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="bd998-2548">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="bd998-2549">Wyrażenie zapytania z klauzulą `join` z `into`, po którym następuje klauzula `select`</span><span class="sxs-lookup"><span data-stu-id="bd998-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="bd998-2550">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="bd998-2551">Wyrażenie zapytania z klauzulą `join` z `into`, po którym następuje element inny niż `select`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="bd998-2552">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="bd998-2553">Wyrażenie zapytania z klauzulą `orderby`</span><span class="sxs-lookup"><span data-stu-id="bd998-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="bd998-2554">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="bd998-2555">Jeśli klauzula porządkowania określi wskaźnik kierunku `descending`, zamiast tego zostanie utworzone wywołanie `OrderByDescending` lub `ThenByDescending`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="bd998-2556">Poniższe tłumaczenia założono, że nie istnieją `let`, `where`, `join` lub `orderby` klauzule i nie więcej niż jedna początkowa klauzula `from` w każdym wyrażeniu zapytania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="bd998-2557">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="bd998-2558">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="bd998-2559">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="bd998-2560">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="bd998-2561">końcowe tłumaczenie to</span><span class="sxs-lookup"><span data-stu-id="bd998-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="bd998-2562">gdzie `x` to identyfikator wygenerowany przez kompilator, który jest niewidoczny i niedostępny.</span><span class="sxs-lookup"><span data-stu-id="bd998-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="bd998-2563">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="bd998-2564">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="bd998-2565">końcowe tłumaczenie to</span><span class="sxs-lookup"><span data-stu-id="bd998-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="bd998-2566">gdzie `x` to identyfikator wygenerowany przez kompilator, który jest niewidoczny i niedostępny.</span><span class="sxs-lookup"><span data-stu-id="bd998-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="bd998-2567">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="bd998-2568">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="bd998-2569">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="bd998-2570">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="bd998-2571">końcowe tłumaczenie to</span><span class="sxs-lookup"><span data-stu-id="bd998-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="bd998-2572">gdzie `x` i `y` są wygenerowanymi przez kompilator identyfikatorami, które w przeciwnym razie są niewidoczne i niedostępne.</span><span class="sxs-lookup"><span data-stu-id="bd998-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="bd998-2573">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="bd998-2574">ma końcowe tłumaczenie</span><span class="sxs-lookup"><span data-stu-id="bd998-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="bd998-2575">Wybierz klauzule</span><span class="sxs-lookup"><span data-stu-id="bd998-2575">Select clauses</span></span>

<span data-ttu-id="bd998-2576">Wyrażenie zapytania w formularzu</span><span class="sxs-lookup"><span data-stu-id="bd998-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="bd998-2577">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="bd998-2578">z wyjątkiem sytuacji, gdy v jest identyfikatorem x, tłumaczenie jest po prostu</span><span class="sxs-lookup"><span data-stu-id="bd998-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="bd998-2579">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="bd998-2580">jest po prostu przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="bd998-2581">Klauzule GroupBy</span><span class="sxs-lookup"><span data-stu-id="bd998-2581">Groupby clauses</span></span>

<span data-ttu-id="bd998-2582">Wyrażenie zapytania w formularzu</span><span class="sxs-lookup"><span data-stu-id="bd998-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="bd998-2583">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="bd998-2584">z wyjątkiem sytuacji, gdy v jest identyfikatorem x, tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="bd998-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="bd998-2585">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="bd998-2586">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="bd998-2587">Identyfikatory przezroczyste</span><span class="sxs-lookup"><span data-stu-id="bd998-2587">Transparent identifiers</span></span>

<span data-ttu-id="bd998-2588">Niektóre tłumaczenia wprowadzają zmienne zakresów z ***przezroczystymi identyfikatorami*** wskazywanymi przez `*`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="bd998-2589">Przezroczyste identyfikatory nie są właściwą funkcją języka; istnieją one tylko jako pośredni krok w procesie tłumaczenia wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="bd998-2590">Gdy tłumaczenie zapytania powoduje iniekcję przezroczystego identyfikatora, dalsze etapy tłumaczenia propagują przezroczysty identyfikator do funkcji anonimowych i inicjatorów obiektów anonimowych.</span><span class="sxs-lookup"><span data-stu-id="bd998-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="bd998-2591">W tych kontekstach przezroczyste identyfikatory mają następujące zachowanie:</span><span class="sxs-lookup"><span data-stu-id="bd998-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="bd998-2592">Gdy przezroczysty identyfikator występuje jako parametr w funkcji anonimowej, elementy członkowskie skojarzonego typu anonimowego są automatycznie w zakresie w treści funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="bd998-2593">Gdy członek z przezroczystym identyfikatorem znajduje się w zakresie, członkowie tego elementu członkowskiego są również w zakresie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="bd998-2594">Gdy przezroczysty identyfikator występuje jako element członkowski deklarator w inicjatorze obiektu anonimowego, wprowadza element członkowski z przezroczystym identyfikatorem.</span><span class="sxs-lookup"><span data-stu-id="bd998-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="bd998-2595">W opisanych powyżej krokach, przezroczyste identyfikatory są zawsze wprowadzane wraz z typami anonimowymi, z zamiarem przechwytywania wielu zmiennych zakresu jako elementów członkowskich jednego obiektu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="bd998-2596">Implementacja programu C# może korzystać z innego mechanizmu niż typy anonimowe, aby grupować jednocześnie wiele zmiennych zakresu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="bd998-2597">Poniższe przykłady translacji założono, że są używane typy anonimowe i pokazują, jak przezroczyste identyfikatory mogą być przetłumaczone.</span><span class="sxs-lookup"><span data-stu-id="bd998-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="bd998-2598">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="bd998-2599">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="bd998-2600">który jest bardziej przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="bd998-2601">które po wymazaniu przezroczystych identyfikatorów są równoważne</span><span class="sxs-lookup"><span data-stu-id="bd998-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="bd998-2602">gdzie `x` to identyfikator wygenerowany przez kompilator, który jest niewidoczny i niedostępny.</span><span class="sxs-lookup"><span data-stu-id="bd998-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="bd998-2603">Przykład</span><span class="sxs-lookup"><span data-stu-id="bd998-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="bd998-2604">jest przetłumaczony na</span><span class="sxs-lookup"><span data-stu-id="bd998-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="bd998-2605">który jest bardziej skrócony do</span><span class="sxs-lookup"><span data-stu-id="bd998-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="bd998-2606">końcowe tłumaczenie to</span><span class="sxs-lookup"><span data-stu-id="bd998-2606">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="bd998-2607">gdzie `x`, `y` i `z` są wygenerowanymi przez kompilator identyfikatory, które w przeciwnym razie są niewidoczne i niedostępne.</span><span class="sxs-lookup"><span data-stu-id="bd998-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="bd998-2608">Wzorzec wyrażenia zapytania</span><span class="sxs-lookup"><span data-stu-id="bd998-2608">The query expression pattern</span></span>

<span data-ttu-id="bd998-2609">***Wzorzec wyrażenia zapytania*** stanowi wzorzec metod, które typy mogą zaimplementować do obsługi wyrażeń zapytań.</span><span class="sxs-lookup"><span data-stu-id="bd998-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="bd998-2610">Ponieważ wyrażenia zapytania są tłumaczone na wywołania metod przy użyciu mapowania składni, typy mają znaczną elastyczność w sposobie implementowania wzorca wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="bd998-2611">Na przykład metody wzorca mogą być implementowane jako metody instancji lub jako metody rozszerzające, ponieważ dwie mają tę samą składnię wywołania, a metody mogą zażądać delegatów lub drzew wyrażeń, ponieważ funkcje anonimowe są konwertowane do obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="bd998-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="bd998-2612">Zalecany kształt typu ogólnego `C<T>` obsługujący wzorzec wyrażenia zapytania jest przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="bd998-2613">Typ ogólny jest używany w celu zilustrowania właściwych relacji między parametrami i typami wyników, ale istnieje możliwość zaimplementowania wzorca dla typów innych niż ogólne.</span><span class="sxs-lookup"><span data-stu-id="bd998-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="bd998-2614">W powyższych metodach użyto ogólnych typów delegatów `Func<T1,R>` i `Func<T1,T2,R>`, ale mogą być równie dobrze używane inne Delegaty lub typy drzewa wyrażeń z tymi samymi relacjami w typach parametrów i wyników.</span><span class="sxs-lookup"><span data-stu-id="bd998-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="bd998-2615">Zwróć uwagę na to, że zalecana relacja między `C<T>` i `O<T>` zapewnia, że metody `ThenBy` i `ThenByDescending` są dostępne tylko w wyniku `OrderBy` lub `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="bd998-2616">Zauważ również, że zalecany kształt wyniku `GroupBy`--sekwencji sekwencji, gdzie każda sekwencja wewnętrzna ma dodatkową właściwość `Key`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="bd998-2617">Przestrzeń nazw `System.Linq` zawiera implementację wzorca operatora zapytania dla dowolnego typu, który implementuje interfejs `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="bd998-2618">Operatory przypisania</span><span class="sxs-lookup"><span data-stu-id="bd998-2618">Assignment operators</span></span>

<span data-ttu-id="bd998-2619">Operatory przypisania przypisują nową wartość do zmiennej, właściwości, zdarzenia lub elementu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="bd998-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="bd998-2620">Lewy operand przypisania musi być wyrażeniem sklasyfikowanym jako zmienna, dostępem do właściwości, dostępem indeksatora lub dostępem do zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="bd998-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="bd998-2621">Operator `=` nosi nazwę ***prostego operatora przypisania***.</span><span class="sxs-lookup"><span data-stu-id="bd998-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="bd998-2622">Przypisuje wartość operandu Right do zmiennej, właściwości lub elementu indeksatora podaną przez lewy argument operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="bd998-2623">Lewy argument operacji prostego operatora przypisania nie może być dostępem do zdarzeń (z wyjątkiem opisanych w [zdarzeniach przypominających pola](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="bd998-2624">Prosty operator przypisania został opisany w [prostym przypisaniu](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="bd998-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="bd998-2625">Operatory przypisania inne niż operator `=` są nazywane ***operatorami przypisania złożonego***.</span><span class="sxs-lookup"><span data-stu-id="bd998-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="bd998-2626">Te operatory wykonują wskazane operacje na dwóch operandach, a następnie przypisują wartość wyniki do zmiennej, właściwości lub elementu indeksatora podanego przez lewy operand.</span><span class="sxs-lookup"><span data-stu-id="bd998-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="bd998-2627">Operatory przypisania złożonego są opisane w [przypisaniu złożonym](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="bd998-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="bd998-2628">Operatory `+=` i `-=` z wyrażeniem dostępu do zdarzenia jako lewy operand są nazywane *operatorami przypisania zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="bd998-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="bd998-2629">Żaden inny operator przypisania nie jest prawidłowy w przypadku dostępu do zdarzenia jako lewego operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="bd998-2630">Operatory przypisania zdarzeń są opisane w [przypisaniu zdarzeń](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="bd998-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="bd998-2631">Operatory przypisania są z prawej strony skojarzenia, co oznacza, że operacje są pogrupowane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="bd998-2632">Na przykład wyrażenie formularza `a = b = c` jest oceniane jako `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="bd998-2633">Przypisanie proste</span><span class="sxs-lookup"><span data-stu-id="bd998-2633">Simple assignment</span></span>

<span data-ttu-id="bd998-2634">Operator `=` nosi nazwę prostego operatora przypisania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="bd998-2635">Jeśli Lewy argument operacji prostego przypisywania ma postać `E.P` lub `E[Ei]`, gdzie `E` ma typ czasu kompilacji `dynamic`, to przypisanie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-2636">W takim przypadku typ czasu kompilowania w wyrażeniu przypisania jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania na podstawie typu czasu wykonywania `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="bd998-2637">W prostym przypisaniu, prawy operand musi być wyrażeniem, które jest niejawnie konwertowane na typ operandu po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="bd998-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="bd998-2638">Operacja przypisuje wartość operandu Right do zmiennej, właściwości lub elementu indeksatora podaną przez lewy argument operacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="bd998-2639">Wynikiem prostego wyrażenia przypisania jest wartość przypisana do lewego operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="bd998-2640">Wynik ma ten sam typ co lewy operand i jest zawsze sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="bd998-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="bd998-2641">Jeśli argument operacji po lewej stronie jest dostępną właściwością lub indeksatorem, właściwość lub indeksator musi mieć metodę dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="bd998-2642">W przeciwnym razie wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-2643">Przetwarzanie prostego przypisania formularza `x = y` w czasie wykonywania obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="bd998-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="bd998-2644">Jeśli `x` jest klasyfikowane jako zmienna:</span><span class="sxs-lookup"><span data-stu-id="bd998-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="bd998-2645">`x` jest oceniane, aby utworzyć zmienną.</span><span class="sxs-lookup"><span data-stu-id="bd998-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="bd998-2646">`y` jest oceniane i, w razie potrzeby, konwertowane na typ `x` za pomocą konwersji niejawnej ([konwersje niejawne](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="bd998-2647">Jeśli zmienna określona przez `x` jest elementem tablicy *reference_type*, wykonywane jest sprawdzanie w czasie wykonywania, aby upewnić się, że wartość obliczona dla `y` jest zgodna z wystąpieniem tablicy, dla którego `x` jest elementem.</span><span class="sxs-lookup"><span data-stu-id="bd998-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="bd998-2648">Sprawdzanie kończy się powodzeniem, jeśli `y` jest `null` lub Jeśli niejawna konwersja odwołań ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) istnieje z rzeczywistego typu wystąpienia, do którego odwołuje się `y`, do rzeczywistego typu elementu wystąpienia tablicy zawierającego `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="bd998-2649">W przeciwnym razie zostanie zgłoszony `System.ArrayTypeMismatchException`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="bd998-2650">Wartość będąca wynikiem oceny i konwersji `y` jest przechowywana w lokalizacji podawanej przez ocenę `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="bd998-2651">Jeśli `x` jest sklasyfikowany jako dostęp do właściwości lub indeksatora:</span><span class="sxs-lookup"><span data-stu-id="bd998-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="bd998-2652">Wyrażenie wystąpienia (jeśli `x` nie jest `static`) i lista argumentów (jeśli `x` jest dostępem indeksatora) skojarzonym z `x` są oceniane, a wyniki są używane w kolejnym wywołaniu metody dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="bd998-2653">`y` jest oceniane i, w razie potrzeby, konwertowane na typ `x` za pomocą konwersji niejawnej ([konwersje niejawne](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="bd998-2654">Metoda dostępu `set` elementu `x` jest wywoływana z wartością obliczaną dla `y` jako jej argumentu `value`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="bd998-2655">Reguły wariancji tablic ([Kowariancja tablic](arrays.md#array-covariance)) zezwalają na wartość typu tablicy `A[]`, aby było odwołaniem do wystąpienia typu tablicy `B[]`, pod warunkiem, że konwersja niejawnego odwołania istnieje z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="bd998-2656">Ze względu na te reguły przypisanie do elementu tablicy *reference_type* wymaga sprawdzenia w czasie wykonywania, aby upewnić się, że przypisywana wartość jest zgodna z wystąpieniem tablicy.</span><span class="sxs-lookup"><span data-stu-id="bd998-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="bd998-2657">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="bd998-2658">Ostatnie przypisanie powoduje zgłoszenie `System.ArrayTypeMismatchException`, ponieważ wystąpienie `ArrayList` nie może być przechowywane w elemencie `string[]`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="bd998-2659">Gdy właściwość lub indeksator zadeklarowany w *struct_type* jest elementem docelowym przypisania, wyrażenie wystąpienia skojarzone z właściwością lub dostępem indeksatora musi być sklasyfikowane jako zmienna.</span><span class="sxs-lookup"><span data-stu-id="bd998-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="bd998-2660">Jeśli wyrażenie wystąpienia jest sklasyfikowane jako wartość, wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="bd998-2661">Ze względu na [dostęp do elementu członkowskiego](expressions.md#member-access)ta sama reguła dotyczy również pól.</span><span class="sxs-lookup"><span data-stu-id="bd998-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="bd998-2662">Uwzględniając deklaracje:</span><span class="sxs-lookup"><span data-stu-id="bd998-2662">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="bd998-2663">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="bd998-2664">przypisania do `p.X`, `p.Y`, `r.A` i `r.B` są dozwolone, ponieważ `p` i `r` są zmiennymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="bd998-2665">Jednak w przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="bd998-2666">wszystkie przypisania są nieprawidłowe, ponieważ `r.A` i `r.B` nie są zmiennymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="bd998-2667">Przypisanie złożone</span><span class="sxs-lookup"><span data-stu-id="bd998-2667">Compound assignment</span></span>

<span data-ttu-id="bd998-2668">Jeśli Lewy argument operacji przypisania złożonego ma postać `E.P` lub `E[Ei]`, gdzie `E` ma typ czasu kompilacji `dynamic`, to przypisanie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="bd998-2669">W takim przypadku typ czasu kompilowania w wyrażeniu przypisania jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania na podstawie typu czasu wykonywania `E`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="bd998-2670">Operacja `x op= y` jest przetwarzana przez zastosowanie rozpoznawania przeciążania operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)), tak jakby operacja była zapisywana `x op y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="bd998-2671">Następnie</span><span class="sxs-lookup"><span data-stu-id="bd998-2671">Then,</span></span>

*  <span data-ttu-id="bd998-2672">Jeśli zwracany typ wybranego operatora jest niejawnie konwertowany na typ `x`, operacja jest szacowana jako `x = x op y`, z wyjątkiem tego, że `x` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="bd998-2673">W przeciwnym razie, jeśli wybrany operator jest wstępnie zdefiniowanym operatorem, Jeśli zwracany typ wybranego operatora jest jawnie konwertowany na typ `x` i jeśli `y` jest niejawnie konwertowany do typu `x` lub operator jest operatorem przesunięcia , a następnie operacja jest szacowana jako `x = (T)(x op y)`, gdzie `T` jest typem `x`, z wyjątkiem tego, że `x` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="bd998-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="bd998-2674">W przeciwnym razie złożone przypisanie jest nieprawidłowe i wystąpi błąd w czasie trwania powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-2675">Termin "oceniony tylko raz" oznacza, że podczas obliczania `x op y` wyniki wszelkich wyrażeń składników `x` są tymczasowo zapisywane i ponownie używane podczas wykonywania przypisania do `x`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="bd998-2676">Na przykład w przypisaniu `A()[B()] += C()`, gdzie `A` jest metodą zwracającą `int[]`, a `B` i `C` są metodami zwracającymi wartość `int`, metody są wywoływane tylko raz, w kolejności `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="bd998-2677">Gdy lewy operand przypisania złożonego jest dostępem do właściwości lub indeksatorem, właściwość lub indeksator musi mieć metodę dostępu `get` i metodę dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="bd998-2678">W przeciwnym razie wystąpi błąd w czasie powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-2679">Druga reguła powyżej zezwala `x op= y` na ocenę jako `x = (T)(x op y)` w niektórych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="bd998-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="bd998-2680">Reguła istnieje tak, aby wstępnie zdefiniowane operatory mogły być używane jako operatory złożone, gdy argument operacji po lewej stronie jest typu `sbyte`, `byte`, `short`, `ushort` lub `char`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="bd998-2681">Nawet jeśli oba argumenty są jednego z tych typów, wstępnie zdefiniowane operatory generują wynik typu `int`, zgodnie z opisem w [binarnej promocji liczbowych](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="bd998-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="bd998-2682">W związku z tym bez rzutowania nie można przypisać wyniku do lewego operandu.</span><span class="sxs-lookup"><span data-stu-id="bd998-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="bd998-2683">Intuicyjny efekt reguły dla wstępnie zdefiniowanych operatorów to po prostu, że wartość `x op= y` jest dozwolona, jeśli dozwolone są obie `x op y` i `x = y`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="bd998-2684">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-2684">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="bd998-2685">intuicyjną przyczyną każdego błędu jest to, że odpowiednie proste przypisanie mogło również być błędem.</span><span class="sxs-lookup"><span data-stu-id="bd998-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="bd998-2686">Oznacza to również, że operacje przypisania złożonego obsługują podniesione operacje.</span><span class="sxs-lookup"><span data-stu-id="bd998-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="bd998-2687">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="bd998-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="bd998-2688">użyto podnoszenia `+(int?,int?)`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="bd998-2689">Przypisanie zdarzenia</span><span class="sxs-lookup"><span data-stu-id="bd998-2689">Event assignment</span></span>

<span data-ttu-id="bd998-2690">Jeśli argument operacji po lewej stronie operatora `+=` lub `-=` jest klasyfikowany jako dostęp do zdarzenia, wyrażenie jest oceniane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="bd998-2691">Wyrażenie wystąpienia (jeśli istnieje) dostępu do zdarzenia jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="bd998-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="bd998-2692">Jest oceniany prawy operand operatora `+=` lub `-=` i, w razie potrzeby, konwertowany na typ lewego operandu przez niejawną konwersję ([konwersje niejawne](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="bd998-2693">Metoda dostępu do zdarzenia jest wywoływana, z listą argumentów składającą się z prawego operandu, po dokonaniu oceny i, w razie potrzeby, konwersji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="bd998-2694">Jeśli operator został `+=`, metoda dostępu `add` jest wywoływana; Jeśli operator został `-=`, metoda dostępu `remove` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bd998-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="bd998-2695">Wyrażenie przypisania zdarzenia nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="bd998-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="bd998-2696">W związku z tym wyrażenie przypisania zdarzenia jest prawidłowe tylko w kontekście *statement_expression* ([instrukcji wyrażenia](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="bd998-2697">Wyrażenie</span><span class="sxs-lookup"><span data-stu-id="bd998-2697">Expression</span></span>

<span data-ttu-id="bd998-2698">*Wyrażenie* jest *non_assignment_expression* lub *przypisanie*.</span><span class="sxs-lookup"><span data-stu-id="bd998-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="bd998-2699">Wyrażenia stałe</span><span class="sxs-lookup"><span data-stu-id="bd998-2699">Constant expressions</span></span>

<span data-ttu-id="bd998-2700">*Constant_expression* jest wyrażeniem, które może być w pełni oceniane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="bd998-2701">Wyrażenie stałe musi być literałem `null` lub wartością z jednym z następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` 0, @no__t , 5 lub dowolny typ wyliczeniowy.</span><span class="sxs-lookup"><span data-stu-id="bd998-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="bd998-2702">Tylko następujące konstrukcje są dozwolone w wyrażeniach stałych:</span><span class="sxs-lookup"><span data-stu-id="bd998-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="bd998-2703">Literały (w tym literał `null`).</span><span class="sxs-lookup"><span data-stu-id="bd998-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="bd998-2704">Odwołania do elementów członkowskich `const` klas i typów struktury.</span><span class="sxs-lookup"><span data-stu-id="bd998-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="bd998-2705">Odwołania do elementów członkowskich typów wyliczeniowych.</span><span class="sxs-lookup"><span data-stu-id="bd998-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="bd998-2706">Odwołania do parametrów `const` lub zmiennych lokalnych</span><span class="sxs-lookup"><span data-stu-id="bd998-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="bd998-2707">Podwyrażenia w nawiasach, które są same wyrażeniami stałymi.</span><span class="sxs-lookup"><span data-stu-id="bd998-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="bd998-2708">Wyrażenia rzutowania, pod warunkiem, że typ docelowy to jeden z typów wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="bd998-2709">wyrażenia `checked` i `unchecked`</span><span class="sxs-lookup"><span data-stu-id="bd998-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="bd998-2710">Wyrażenia wartości domyślnych</span><span class="sxs-lookup"><span data-stu-id="bd998-2710">Default value expressions</span></span>
*  <span data-ttu-id="bd998-2711">Wyrażenia nameof</span><span class="sxs-lookup"><span data-stu-id="bd998-2711">Nameof expressions</span></span>
*  <span data-ttu-id="bd998-2712">Wstępnie zdefiniowane operatory `+`, `-`, `!` i `~` jednoargumentowe.</span><span class="sxs-lookup"><span data-stu-id="bd998-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="bd998-2713">Wstępnie zdefiniowane `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, 0, 1, 2, 3, 4, 5, 6 i 7 operatorów binarnych, pod warunkiem że każdy operand jest Typ wymieniony powyżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="bd998-2714">Operator warunkowy `?:`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="bd998-2715">Następujące konwersje są dozwolone w wyrażeniach stałych:</span><span class="sxs-lookup"><span data-stu-id="bd998-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="bd998-2716">Konwersje tożsamości</span><span class="sxs-lookup"><span data-stu-id="bd998-2716">Identity conversions</span></span>
*  <span data-ttu-id="bd998-2717">Konwersje numeryczne</span><span class="sxs-lookup"><span data-stu-id="bd998-2717">Numeric conversions</span></span>
*  <span data-ttu-id="bd998-2718">Konwersje wyliczenia</span><span class="sxs-lookup"><span data-stu-id="bd998-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="bd998-2719">Konwersje wyrażeń stałych</span><span class="sxs-lookup"><span data-stu-id="bd998-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="bd998-2720">Konwersje niejawne i jawne, pod warunkiem, że źródło konwersji jest wyrażeniem stałym, którego wartość jest równa null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="bd998-2721">Inne konwersje, w tym opakowanie, rozpakowywanie i niejawne konwersje odwołań wartości innych niż null, są niedozwolone w wyrażeniach stałych.</span><span class="sxs-lookup"><span data-stu-id="bd998-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="bd998-2722">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bd998-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="bd998-2723">Inicjowanie i jest błędem, ponieważ wymagana jest konwersja z opakowaniem.</span><span class="sxs-lookup"><span data-stu-id="bd998-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="bd998-2724">Inicjalizacja str jest błędem, ponieważ wymagana jest niejawna konwersja odwołania z wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="bd998-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="bd998-2725">Gdy wyrażenie spełnia wymagania wymienione powyżej, wyrażenie jest oceniane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="bd998-2726">Jest to prawdziwe, nawet jeśli wyrażenie jest podwyrażeniem większego wyrażenia, które zawiera niestałe konstrukcje.</span><span class="sxs-lookup"><span data-stu-id="bd998-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="bd998-2727">Ocena czasu kompilacji wyrażeń stałych używa tych samych zasad, co w przypadku oceny w czasie wykonywania wyrażeń niestałych, z tą różnicą, że w przypadku oceny czasu wykonywania wywołał wyjątek, Ocena czasu kompilacji powoduje wystąpienie błędu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="bd998-2728">Jeśli wyrażenie stałe nie jest jawnie umieszczane w kontekście `unchecked`, nadmiary występujące w operacjach arytmetycznych typu całkowitego i konwersje podczas obliczania czasu kompilacji wyrażenia zawsze powodują błędy w czasie kompilacji ([stała Expressions](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="bd998-2729">Wyrażenia stałe występują w kontekstach wymienionych poniżej.</span><span class="sxs-lookup"><span data-stu-id="bd998-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="bd998-2730">W tych kontekstach występuje błąd czasu kompilacji, jeśli wyrażenie nie może być w pełni oceniane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd998-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="bd998-2731">Stałe deklaracje ([stałe](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="bd998-2732">Wyliczanie deklaracji elementów członkowskich ([składowe wyliczenia](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="bd998-2733">Domyślne argumenty list parametrów formalnych ([Parametry metody](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="bd998-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="bd998-2734">etykiety `case` instrukcji `switch` ([instrukcja SWITCH](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="bd998-2735">instrukcje `goto case` ([instrukcja goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="bd998-2736">Długości wymiarów w wyrażeniu tworzenia tablicy ([wyrażenia tworzenia tablic](expressions.md#array-creation-expressions)), które zawierają inicjator.</span><span class="sxs-lookup"><span data-stu-id="bd998-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="bd998-2737">Atrybuty ([atrybuty](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="bd998-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="bd998-2738">Niejawna konwersja wyrażeń stałych ([niejawne konwersje wyrażeń stałych](conversions.md#implicit-constant-expression-conversions)) umożliwia konwertowanie stałego wyrażenia typu `int` na `sbyte`, `byte`, `short`, `ushort`, `uint` lub `ulong`, pod warunkiem, że wartość wyrażenie stałe znajduje się w zakresie typu docelowego.</span><span class="sxs-lookup"><span data-stu-id="bd998-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="bd998-2739">Wyrażenia logiczne</span><span class="sxs-lookup"><span data-stu-id="bd998-2739">Boolean expressions</span></span>

<span data-ttu-id="bd998-2740">*Boolean_expression* jest wyrażeniem, które zwraca wynik typu `bool`; bezpośrednio lub poprzez zastosowanie `operator true` w niektórych kontekstach, zgodnie z opisem w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="bd998-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="bd998-2741">Kontrolne wyrażenie warunkowe elementu *if_statement* ([instrukcja IF](statements.md#the-if-statement)), *while_statement* ([instrukcja while](statements.md#the-while-statement)), *do_statement* ([instrukcja](statements.md#the-do-statement)do) lub *for_statement* ([dla Instrukcja](statements.md#the-for-statement)) to element *Boolean_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="bd998-2742">Wyrażenie warunkowe kontrolki operatora `?:` ([operator warunkowy](expressions.md#conditional-operator)) ma te same reguły jak *Boolean_expression*, ale z powodu pierwszeństwa operatorów jest klasyfikowane jako *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="bd998-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="bd998-2743">*Boolean_expression* `E` musi być w stanie utworzyć wartość typu `bool` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bd998-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="bd998-2744">Jeśli `E` jest niejawnie konwertowany na `bool` następnie w czasie wykonywania, że stosowana jest niejawna konwersja.</span><span class="sxs-lookup"><span data-stu-id="bd998-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="bd998-2745">W przeciwnym razie rozpoznawanie przeciążania operatora jednoargumentowego ([jednoargumentowego rozpoznawania operatora](expressions.md#unary-operator-overload-resolution)) służy do znajdowania unikatowej najlepszej implementacji operatora `true` w `E`, a ta implementacja jest stosowana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="bd998-2746">Jeśli żaden taki operator nie zostanie znaleziony, wystąpi błąd w czasie trwania powiązania.</span><span class="sxs-lookup"><span data-stu-id="bd998-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="bd998-2747">Typ struktury `DBBool` w [typie Boolean bazy danych](structs.md#database-boolean-type) zawiera przykład typu, który implementuje `operator true` i `operator false`.</span><span class="sxs-lookup"><span data-stu-id="bd998-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
