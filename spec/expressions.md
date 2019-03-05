---
ms.openlocfilehash: 67019511d49a786a5d6edf6fea442f745fc40f3f
ms.sourcegitcommit: 0a80f26b8e455c4f09843a10e11e29c24d2d922e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57347277"
---
# <a name="expressions"></a><span data-ttu-id="1b6fe-101">Wyrażenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-101">Expressions</span></span>

<span data-ttu-id="1b6fe-102">Wyrażenie jest sekwencją operatorów i argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="1b6fe-103">W tym rozdziale określa składnię, kolejność oceny operatorów i argumentów i znaczenie wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="1b6fe-104">Wyrażenie klasyfikacji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-104">Expression classifications</span></span>

<span data-ttu-id="1b6fe-105">Wyrażenie jest klasyfikowana jako jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="1b6fe-106">Wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-106">A value.</span></span> <span data-ttu-id="1b6fe-107">Każda wartość ma skojarzony typ.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="1b6fe-108">Zmienna.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-108">A variable.</span></span> <span data-ttu-id="1b6fe-109">Co zmienna ma skojarzony typ, a mianowicie zadeklarowanym typem zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="1b6fe-110">Przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-110">A namespace.</span></span> <span data-ttu-id="1b6fe-111">Wyrażenia z tą klasyfikacją może się pojawić tylko po lewej stronie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="1b6fe-112">W dowolnym kontekście wyrażenia sklasyfikowane jako przestrzeń nazw powoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="1b6fe-113">Typ.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-113">A type.</span></span> <span data-ttu-id="1b6fe-114">Wyrażenia z tą klasyfikacją może się pojawić tylko po lewej stronie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), lub jako argument dla `as` — operator ([jako operator ](expressions.md#the-as-operator)), `is` — operator ([jest operatorem](expressions.md#the-is-operator)), lub `typeof` — operator ([typeof operator](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="1b6fe-115">W dowolnym kontekście sklasyfikowane jako typ wyrażenia powoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="1b6fe-116">Grupy metod, czyli zestaw przeciążonych metod wynikające z wyszukanie członka ([wyszukanie członka](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="1b6fe-117">Grupy metod może mieć wyrażenia skojarzonego wystąpienia i skojarzony typ listy argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="1b6fe-118">Po wywołaniu metody wystąpienia wyniku obliczenia wyrażenia wystąpienia staje się wystąpienia, reprezentowane przez `this` ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="1b6fe-119">Grupy metod jest dozwolony w *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)), *delegate_creation_expression* ([delegowanie tworzenia wyrażenia](expressions.md#delegate-creation-expressions)) i po lewej stronie operatora i mogą być niejawnie konwertowane na typ delegata zgodne ([metody konwersji grup](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="1b6fe-120">W dowolnym kontekście wyrażenia sklasyfikowane jako grupy metod powoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="1b6fe-121">Literał null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-121">A null literal.</span></span> <span data-ttu-id="1b6fe-122">Wyrażenia z tą klasyfikacją można niejawnie przekonwertować typu odwołania lub typ dopuszczający wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="1b6fe-123">Funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-123">An anonymous function.</span></span> <span data-ttu-id="1b6fe-124">Wyrażenia z tą klasyfikacją mogą być niejawnie konwertowane na typ zgodny delegata lub typ drzewa wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="1b6fe-125">Dostęp do właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-125">A property access.</span></span> <span data-ttu-id="1b6fe-126">Dostęp do każdej właściwości jest skojarzony typ, to znaczy typu właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="1b6fe-127">Ponadto dostęp do właściwości może mieć wyrażenia skojarzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="1b6fe-128">Gdy metody dostępu ( `get` lub `set` bloku) wystąpienia dostęp do właściwości jest wywoływana, wynikiem obliczenia wyrażenia wystąpienia staje się wystąpienia, reprezentowane przez `this` ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="1b6fe-129">Dostęp do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-129">An event access.</span></span> <span data-ttu-id="1b6fe-130">Dostęp do każdego zdarzenia jest skojarzony typ, a mianowicie typ zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="1b6fe-131">Ponadto dostęp do zdarzenia może mieć wyrażenia skojarzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="1b6fe-132">Dostęp do zdarzenia może być wyświetlana jako argument operacji po lewej stronie ekranu `+=` i `-=` operatorów ([przypisanie zdarzenia](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="1b6fe-133">W dowolnym kontekście wyrażenia sklasyfikowane jako dostęp do zdarzenia powoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="1b6fe-134">Dostęp indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-134">An indexer access.</span></span> <span data-ttu-id="1b6fe-135">Dostęp indeksatora, co ma skojarzony typ, a mianowicie typ elementu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="1b6fe-136">Ponadto dostęp indeksatora ma wyrażenia skojarzonego wystąpienia oraz lista argumentów skojarzonych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="1b6fe-137">Gdy metody dostępu ( `get` lub `set` bloku) indeksatora dostępu jest wywoływana, wynikiem obliczenia wyrażenia wystąpienia staje się wystąpienia, reprezentowane przez `this` ([dostęp](expressions.md#this-access)) i wynik Ocena listy argumentów staje się lista parametrów wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="1b6fe-138">Brak elementów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-138">Nothing.</span></span> <span data-ttu-id="1b6fe-139">Operacja wykonywana, gdy wyrażenie jest wywołanie metody z typem zwracanym `void`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="1b6fe-140">Sklasyfikowane jako nic nie jest prawidłowy tylko w kontekście wyrażenia *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="1b6fe-141">Ostateczny wynik wyrażenia nigdy nie jest przestrzeń nazw, typu, grupy metod lub dostęp do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="1b6fe-142">Przeciwnie jak wspomniano powyżej, te wyrażenia są kategorie pośrednich konstrukcji, które są dozwolone tylko w określonych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="1b6fe-143">Dostęp do właściwości lub indeksatora dostępu jest zawsze sklasyfikowany jako wartość, wykonując wywołanie *pobierająca* lub *ustawiająca metoda dostępu*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="1b6fe-144">Określonej metody dostępu jest ustalany w kontekście dostępu właściwości lub indeksatora: Jeśli dostęp jest elementem docelowym przypisania, *ustawiająca metoda dostępu* wywoływana w celu przypisania nowej wartości ([przypisanie proste](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="1b6fe-145">W przeciwnym razie *pobierająca* wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="1b6fe-146">Wartości wyrażeń</span><span class="sxs-lookup"><span data-stu-id="1b6fe-146">Values of expressions</span></span>

<span data-ttu-id="1b6fe-147">Większość konstrukcji, które obejmują wyrażenie ostatecznie wymaga wyrażenia do oznaczania ***wartość***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="1b6fe-148">W takich przypadkach Jeśli rzeczywiste wyrażenie wskazuje przestrzeni nazw, typu, grupy metod lub nothing, kompilacji wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="1b6fe-149">Jednak jeśli wyrażenie wskazuje dostęp do właściwości, indeksatora dostępu lub zmienną, wartość właściwości, indeksatora lub zmienna zostanie zastąpiony niejawnie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="1b6fe-150">Wartość zmiennej jest po prostu wartości, które są obecnie przechowywane w lokalizacji przechowywania, określone przez zmienną.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="1b6fe-151">Zmienna muszą być rozważone zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) przed jej wartość można uzyskać lub w przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-152">Wartość właściwości wyrażenia dostęp jest uzyskiwany przez *pobierająca* właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="1b6fe-153">Jeśli nie ma właściwości *pobierająca*, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="1b6fe-154">W przeciwnym razie wywołanie funkcji elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jest wykonywane, i wynik wywołania staje się wartość wyrażenia dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="1b6fe-155">Wartość wyrażenia dostęp indeksatora jest uzyskiwany przez *pobierająca* indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="1b6fe-156">Jeśli nie ma indeksatora *pobierająca*, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="1b6fe-157">W przeciwnym razie wywołanie funkcji elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) odbywa się za pomocą argumentu listy skojarzone z wyrażeniem dostęp indeksatora i wynik wywołania staje się wartością wyrażenia dostęp indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="1b6fe-158">Powiązanie statycznych i dynamicznych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="1b6fe-159">Proces określania znaczenie operacji na podstawie typu lub wartości składowych wyrażeń (argumenty, argumenty operacji, odbiorniki) jest często nazywany ***powiązania***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="1b6fe-160">Na przykład znaczenie wywołania metody jest określana na podstawie typu odbiornik i argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="1b6fe-161">Znaczenie operator jest określana na podstawie typu argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="1b6fe-162">W języku C# znaczenie operacji jest zazwyczaj określana w czasie kompilacji, na podstawie typu jego składowych wyrażeń w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="1b6fe-163">Podobnie jeśli wyrażenie zawiera błąd, błąd jest wykrywane i zgłaszana przez kompilator.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="1b6fe-164">To podejście jest nazywane ***wiązanie statyczne***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="1b6fe-165">Jednak jeśli wyrażenie jest wyrażeniem dynamicznej (czyli ma typ `dynamic`) oznacza to, że wszystkie powiązania, które uczestniczy w powinna być oparta na jego typu run-time (czyli rzeczywisty typ obiektu, który wskazuje, w czasie wykonywania) zamiast typu ma pod czas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="1b6fe-166">Powiązanie takie działanie, w związku z tym jest odroczone do czasu, w którym ma zostać wykonana podczas uruchamiania programu operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="1b6fe-167">Jest to określane jako ***wiązanie dynamiczne***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="1b6fe-168">Podczas operacji dynamicznie jest związany, niewielkiego lub żadnego sprawdzanie jest wykonywane przez kompilator.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="1b6fe-169">Zamiast tego w przypadku niepowodzenia powiązania środowiska wykonawczego błędy są zgłaszane jako wyjątki w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="1b6fe-170">Następujące operacje w języku C# jest zależna od powiązania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="1b6fe-171">Dostęp do elementu członkowskiego: `e.M`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="1b6fe-172">Wywołanie metody: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="1b6fe-173">Wywołanie delegata:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="1b6fe-174">Dostęp do elementu: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="1b6fe-175">Tworzenie obiektów: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="1b6fe-176">Przeciążone operatory jednoargumentowe: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="1b6fe-177">Przeciążone operatory dwuargumentowe: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="1b6fe-178">Operatory przypisania: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="1b6fe-179">Konwersje jawne i niejawne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="1b6fe-180">W przypadku nie wyrażeń dynamicznych C# domyślnie wiązanie statyczne, co oznacza, że typy kompilacji w składowych wyrażeń są używane w procesie wyboru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="1b6fe-181">Jednak gdy jedno z wyrażeń składowych w operacjach wymienionych powyżej jest wyrażenia dynamicznego, operacja jest zamiast tego dynamicznie powiązany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="1b6fe-182">Powiązania w czasie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-182">Binding-time</span></span>

<span data-ttu-id="1b6fe-183">Wiązanie statyczne odbywa się w czasie kompilacji, natomiast wiązanie dynamiczne odbywa się w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="1b6fe-184">W poniższych sekcjach termin ***powiązania w czasie*** odwołuje się do kompilacji lub czasu wykonywania, w zależności od tego, kiedy wiązanie ma miejsce.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="1b6fe-185">Poniższy przykład ilustruje pojęcia statyczne i dynamiczne powiązania i czasu powiązania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="1b6fe-186">Statycznie powiązane są dwa pierwsze wywołania metody: przeciążenia `Console.WriteLine` jest pobierany na podstawie ich argumentu typu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="1b6fe-187">Dlatego podczas wiązania jest kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="1b6fe-188">Trzecie wywołanie jest dynamicznie powiązane: przeciążenia `Console.WriteLine` jest pobierany na podstawie typu środowiska wykonawczego jej argumentu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="1b6fe-189">Dzieje się tak, ponieważ argument jest wyrażenia dynamicznego — jej typ kompilacji jest `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="1b6fe-190">W związku z tym czas powiązania trzecie wywołanie jest środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="1b6fe-191">Wiązanie dynamiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-191">Dynamic binding</span></span>

<span data-ttu-id="1b6fe-192">Wiązanie dynamiczne ma na celu Zezwalaj na interakcję z programów C# ***obiektów dynamicznych***, czyli system typów obiektów, które nie podlegają normalnych reguł języka C#.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="1b6fe-193">Obiekty dynamiczne mogą być obiektów z innych języków programowania przy użyciu różnych typów systemów lub mogą być obiekty, które są programowo Instalatora, aby implementować własne semantykę wiązania dla różnych operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="1b6fe-194">Mechanizm, za pomocą którego obiekt dynamiczny implementuje własne semantyka jest definiowany przez implementację.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="1b6fe-195">Danego interfejsu--ponownie definiowany przez implementację — jest implementowany przez obiekty dynamiczne w celu sygnalizowania, że do języka C# czasu wykonywania, do których mają specjalne semantyki.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="1b6fe-196">W związku z tym w każdym przypadku, gdy operacje na obiekt dynamiczny dynamicznie są powiązane, semantyka własne powiązania, a nie te języka C#, jak określono w tym dokumencie przejąć.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="1b6fe-197">W trakcie celem wiązanie dynamiczne umożliwia współdziałanie z obiektami dynamicznymi, C# umożliwia wiązanie dynamiczne na wszystkich obiektach, czy są one dynamiczne, czy nie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="1b6fe-198">Dzięki temu gładsze integracji z obiektami dynamicznymi, wyniki operacji na nich same nie mogą być obiektami dynamicznymi, ale są nadal typu nieznany do programisty należy w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="1b6fe-199">Również wiązanie dynamiczne mogą pomóc wyeliminować podatne kod oparty na odbiciu, nawet wtedy, gdy żadne obiekty, które są zaangażowane są obiektami dynamicznymi.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="1b6fe-200">Poniżej opisano każdego konstrukcji w języku dokładnie gdy wiązanie dynamiczne jest stosowany, co skompilować kontrola czasu — Jeśli dowolny — są stosowane i jakie kompilacji wynik i wyrażenia klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="1b6fe-201">Typy składowych wyrażeń</span><span class="sxs-lookup"><span data-stu-id="1b6fe-201">Types of constituent expressions</span></span>

<span data-ttu-id="1b6fe-202">Podczas operacji statycznie jest związany, typ wyrażenia składowych (np. odbiornik, argument, indeksu lub operand) zawsze uważany jest za typów w czasie kompilacji to wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="1b6fe-203">Podczas operacji dynamicznie jest związany, typ wyrażenia składowych jest określany na różne sposoby w zależności od typu składowych wyrażeń w czasie kompilacji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="1b6fe-204">Wyrażenie składowych typów w czasie kompilacji `dynamic` ma typ wartości rzeczywistej. wyrażenie daje w wyniku w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="1b6fe-205">Składowe wyrażenia, którego typ kompilacji jest parametrem typu została uznana za typu, który jest powiązany parametr typu w czasie wykonywania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="1b6fe-206">W przeciwnym razie składowych wyrażeń została uznana za typów w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="1b6fe-207">Operatory</span><span class="sxs-lookup"><span data-stu-id="1b6fe-207">Operators</span></span>

<span data-ttu-id="1b6fe-208">Wyrażenia są konstruowane na podstawie ***operandy*** i ***operatory***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="1b6fe-209">Operatory wyrażenia wskazują operacje do zastosowania dla operandów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="1b6fe-210">Przykłady operatorów to `+`, `-`, `*`, `/`i `new`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="1b6fe-211">Przykładami operandów są literały, pola, zmienne lokalne i wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="1b6fe-212">Istnieją trzy rodzaje operatory:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="1b6fe-213">Operatory jednoargumentowe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-213">Unary operators.</span></span> <span data-ttu-id="1b6fe-214">Operatory jednoargumentowe wykorzystać jeden z operandów i używać notacji albo prefiksu (takie jak `--x`) lub notacji przyrostkowej (takie jak `x++`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="1b6fe-215">Operatory binarne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-215">Binary operators.</span></span> <span data-ttu-id="1b6fe-216">Operatory dwuargumentowe mają dwa operandy i używają notacji infiks (takie jak `x + y`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="1b6fe-217">operator trójargumentowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-217">Ternary operator.</span></span> <span data-ttu-id="1b6fe-218">Tylko jeden operator trójargumentowy, `?:`, istnieje; ma trzy operandy i używa wrostkowe notacji (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="1b6fe-219">Kolejność obliczania operatorów w wyrażeniu jest określana przez ***pierwszeństwo*** i ***kojarzenie*** operatorów ([pierwszeństwo i kojarzenie operatorów](expressions.md#operator-precedence-and-associativity)) .</span><span class="sxs-lookup"><span data-stu-id="1b6fe-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="1b6fe-220">Operandy w wyrażeniu są przetwarzane od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="1b6fe-221">Na przykład w `F(i) + G(i++) * H(i)`, Metoda `F` jest wywoływana przy użyciu starej wartości `i`, then — metoda `G` jest wywoływana z stara wartość `i`, a na koniec metody `H` jest wywoływana z nową wartością `i`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="1b6fe-222">To jest oddzielne i niepowiązanych pierwszeństwo operatorów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="1b6fe-223">Niektórych operatorów może być ***przeciążone***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="1b6fe-224">Przeciążanie operatora zezwala implementacje operatora zdefiniowanego przez użytkownika może być określony dla operacji, w przypadku, gdy jeden lub oba operandy są typu klasy lub struktury zdefiniowany przez użytkownika ([przeciążania operatora](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="1b6fe-225">Pierwszeństwo i kojarzenie operatorów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-225">Operator precedence and associativity</span></span>

<span data-ttu-id="1b6fe-226">Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególne operatory.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="1b6fe-227">Na przykład, wyrażenie `x + y * z` jest oceniane jako `x + (y * z)` ponieważ `*` operator ma wyższy priorytet niż plik binarny `+` operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="1b6fe-228">Pierwszeństwo operatora jest ustanawiane zgodnie z definicją jego produkcji skojarzone gramatyki.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="1b6fe-229">Na przykład *additive_expression* składa się z sekwencji *multiplicative_expression*rozdzielone s `+` lub `-` operatorów, co daje `+` i `-` operatory niższy priorytet niż `*`, `/`, i `%` operatorów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="1b6fe-230">Poniższa tabela zawiera podsumowanie wszystkich operatorów w kolejność pierwszeństwa od najwyższego do najniższego:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="1b6fe-231">__Sekcja__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-231">__Section__</span></span>                                                                                   | <span data-ttu-id="1b6fe-232">__Kategoria__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-232">__Category__</span></span>                | <span data-ttu-id="1b6fe-233">__Operatory__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="1b6fe-234">Wyrażenia podstawowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="1b6fe-235">Podstawowy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-235">Primary</span></span>                     | <span data-ttu-id="1b6fe-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="1b6fe-237">Operatory jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="1b6fe-238">Jednoargumentowy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-238">Unary</span></span>                       | <span data-ttu-id="1b6fe-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="1b6fe-240">Operatory arytmetyczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="1b6fe-241">Mnożeniowy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-241">Multiplicative</span></span>              | <span data-ttu-id="1b6fe-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="1b6fe-243">Operatory arytmetyczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="1b6fe-244">Dodatku</span><span class="sxs-lookup"><span data-stu-id="1b6fe-244">Additive</span></span>                    | <span data-ttu-id="1b6fe-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="1b6fe-246">Operatory przesunięcia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="1b6fe-247">Shift</span><span class="sxs-lookup"><span data-stu-id="1b6fe-247">Shift</span></span>                       | <span data-ttu-id="1b6fe-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="1b6fe-249">Operatory relacyjne i badania typu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="1b6fe-250">Relacyjne i badania typu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-250">Relational and type testing</span></span> | <span data-ttu-id="1b6fe-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="1b6fe-252">Operatory relacyjne i badania typu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="1b6fe-253">Równości</span><span class="sxs-lookup"><span data-stu-id="1b6fe-253">Equality</span></span>                    | <span data-ttu-id="1b6fe-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="1b6fe-255">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="1b6fe-256">Logicznego AND</span><span class="sxs-lookup"><span data-stu-id="1b6fe-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="1b6fe-257">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="1b6fe-258">Logicznego XOR</span><span class="sxs-lookup"><span data-stu-id="1b6fe-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="1b6fe-259">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="1b6fe-260">Logicznego OR</span><span class="sxs-lookup"><span data-stu-id="1b6fe-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="1b6fe-261">Operatory logiczne warunkowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="1b6fe-262">Warunkowego AND</span><span class="sxs-lookup"><span data-stu-id="1b6fe-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="1b6fe-263">Operatory logiczne warunkowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="1b6fe-264">Warunkowego OR</span><span class="sxs-lookup"><span data-stu-id="1b6fe-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="1b6fe-265">Wartość null operatora łączącego</span><span class="sxs-lookup"><span data-stu-id="1b6fe-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="1b6fe-266">Łączenia wartości null</span><span class="sxs-lookup"><span data-stu-id="1b6fe-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="1b6fe-267">Operator warunkowy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="1b6fe-268">Warunkowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="1b6fe-269">[Operatory przypisania](expressions.md#assignment-operators), [wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="1b6fe-270">Wyrażenie lambda i przypisanie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-270">Assignment and lambda expression</span></span> | <span data-ttu-id="1b6fe-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="1b6fe-272">W przypadku argumentu operacji między dwa operatory o tym samym priorytecie, łączność operatorów kontrolki kolejność, w której są wykonywane operacje:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="1b6fe-273">Z wyjątkiem operatorów przypisania i null operatora łączącego wszystkie operatory dwuargumentowe to ***lewostronne***, co oznacza, że operacje są wykonywane od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="1b6fe-274">Na przykład `x + y + z` jest oceniane jako `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="1b6fe-275">Operatory przypisania, operatora łączącego o wartości null i operator warunkowy (`?:`) są ***zespolony z prawej***, co oznacza, że operacje są wykonywane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="1b6fe-276">Na przykład `x = y = z` jest oceniane jako `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="1b6fe-277">Pierwszeństwo i asocjacyjność mogą być kontrolowane za pomocą nawiasów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="1b6fe-278">Na przykład `x + y * z` najpierw mnoży `y` przez `z` , a następnie dodaje wynik do `x`, ale `(x + y) * z` najpierw dodaje `x` i `y` i następnie wynik mnoży przez `z`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="1b6fe-279">Przeładowanie operatora</span><span class="sxs-lookup"><span data-stu-id="1b6fe-279">Operator overloading</span></span>

<span data-ttu-id="1b6fe-280">Wszystkie operatory jednoargumentowe i binarny ma wstępnie zdefiniowane implementacji, które są automatycznie dostępne na dowolne wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="1b6fe-281">Oprócz wstępnie zdefiniowanych implementacje implementacje zdefiniowanych przez użytkownika mogą zostać wprowadzone przez dołączenie `operator` deklaracje klas i struktur ([operatory](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="1b6fe-282">Implementacjami operatorów zdefiniowanych przez użytkownika zawsze pierwszeństwo wstępnie zdefiniowanego operatora implementacji: Tylko, gdy nie ma zastosowania operatora zdefiniowanego przez użytkownika przez implementacje będzie implementacje wstępnie zdefiniowanego operatora uznaje się, zgodnie z opisem w [Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [przeciążenia operatora binarnego rozpoznawanie](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="1b6fe-283">***Operatory jednoargumentowe z możliwością przeciążenia*** są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="1b6fe-284">Mimo że `true` i `false` nie są jawnie używane w wyrażeniach (i w związku z tym nie są uwzględnione w tabeli pierwszeństwo w [pierwszeństwo i kojarzenie operatorów](expressions.md#operator-precedence-and-associativity)), są traktowane jako operatorów, ponieważ są one wywoływane w wielu kontekstach wyrażenia: wyrażenia logiczne ([wyrażeń logicznych](expressions.md#boolean-expressions)) i wyrażenia dotyczące zasad dostępu ([operator warunkowy](expressions.md#conditional-operator)) i warunkowego logiczne operatory ([warunkowego operatorów logicznych](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="1b6fe-285">***z możliwością przeciążenia operatorów binarnych*** są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="1b6fe-286">Mogą być przeciążone operatory wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="1b6fe-287">W szczególności, nie jest możliwe przeciążenia dostęp do elementu członkowskiego, wywołanie metody lub `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, i `is` operatorów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="1b6fe-288">Gdy jest przeciążony operator binarny, odpowiedniego operatora przypisania, jest również niejawnie przeciążona.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="1b6fe-289">Na przykład przeciążenia operatora `*` jest również przeciążenia operatora `*=`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="1b6fe-290">Jest to dokładniejszym opisem zawartym w [przydział złożony](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="1b6fe-291">Należy pamiętać, że operator przypisania, sama (`=`) nie mogą być przeciążone.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="1b6fe-292">Przypisanie zawsze wykonuje prostą kopię operacja wartości do zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="1b6fe-293">Rzutowanie operacje, takie jak `(T)x`, są przeciążone, zapewniając konwersje zdefiniowane przez użytkownika ([zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="1b6fe-294">Element dostępu, takich jak `a[x]`, nie jest uważany za z możliwością przeciążenia operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="1b6fe-295">Zamiast tego użytkownika indeksowanie jest obsługiwane przy użyciu indeksatorów ([indeksatory](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="1b6fe-296">W wyrażeniach operatory są wywoływane przy użyciu notacji operatora, a w deklaracji, operatory są wywoływane przy użyciu notacji funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="1b6fe-297">W poniższej tabeli przedstawiono relację między operatora i funkcjonalności notacji jednoargumentowy i operatory binarne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="1b6fe-298">W pierwszej pozycji *op* oznacza wszelkie operatora prefiksowego jednoargumentowego z możliwością przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="1b6fe-299">W drugiej pozycji *op* oznacza przyrostkowe jednoargumentowe `++` i `--` operatorów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="1b6fe-300">W trzecim wpis *op* oznacza wszelkie Oczekiwano operatora binarnego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="1b6fe-301">__Notacja operatora__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-301">__Operator notation__</span></span> | <span data-ttu-id="1b6fe-302">__Notacja funkcji__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="1b6fe-303">Deklaracje operatora zdefiniowanego przez użytkownika zawsze należy wymagać co najmniej jeden z parametrów typu klasy lub struktury, który zawiera deklarację operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="1b6fe-304">W związku z tym nie jest możliwe dla operatora zdefiniowanego przez użytkownika mieć taką samą sygnaturę jak wstępnie zdefiniowanego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="1b6fe-305">Deklaracje operatora zdefiniowanego przez użytkownika nie można zmodyfikować składni, pierwszeństwo i łączność operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="1b6fe-306">Na przykład `/` operator jest zawsze operatora binarnego, zawsze ma poziom priorytetu określona w [pierwszeństwo i kojarzenie operatorów](expressions.md#operator-precedence-and-associativity)i jest zawsze lewostronne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="1b6fe-307">Chociaż jest to możliwe dla operatora zdefiniowanego przez użytkownika wykonać żadnych obliczeń, który go pleases implementacji, które tworzą wyniki niż te, które oczekują intuicyjnie jest niewskazane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="1b6fe-308">Na przykład implementacja `operator ==` należy porównać dwa operandy pod kątem równości i zwraca odpowiednią `bool` wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="1b6fe-309">Opisy poszczególnych operatorach w [wyrażenia podstawowe](expressions.md#primary-expressions) za pośrednictwem [warunkowego operatorów logicznych](expressions.md#conditional-logical-operators) Określ wstępnie zdefiniowanych implementacjami operatorów i wszelkie dodatkowe reguły, które są stosowane Każdy operator.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="1b6fe-310">Wprowadź opis wyrażenia ***Rozpoznanie przeciążenia operatora jednoargumentowego***, ***Rozpoznanie przeciążenia operatora binarnego***, i ***promocji na liczbowe***, definicje są znaleziono, w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="1b6fe-311">Rozpoznanie przeciążenia operatora jednoargumentowego</span><span class="sxs-lookup"><span data-stu-id="1b6fe-311">Unary operator overload resolution</span></span>

<span data-ttu-id="1b6fe-312">Operacja formularza `op x` lub `x op`, gdzie `op` jest operatora jednoargumentowego z możliwością przeciążenia i `x` to wyrażenie typu `X`, jest przetwarzana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="1b6fe-313">Zestaw operatory zdefiniowane przez użytkownika Release candidate dostarczone przez `X` operacji `operator op(x)` jest określany za pomocą zasad [Release Candidate zdefiniowane przez użytkownika operatory](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="1b6fe-314">Jeśli zestaw Release candidate operatory zdefiniowane przez użytkownika nie jest pusta, to staje się zestaw operatorów Release candidate dla tej operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="1b6fe-315">W przeciwnym razie wstępnie zdefiniowanych jednoargumentowe `operator op` implementacji, łącznie z podniesionym formularzy, stają się zestaw operatorów Release candidate dla tej operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="1b6fe-316">Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([wyrażenia podstawowe](expressions.md#primary-expressions) i [operatory jednoargumentowe](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="1b6fe-317">Zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution) są stosowane do zestawu operatorów Release candidate, aby wybrać najlepsze operatora w odniesieniu do listy argumentów `(x)`, a ten operator staje się wynikiem przeciążenia proces rozpoznawania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="1b6fe-318">W przypadku niepowodzenia funkcja rozpoznawania przeciążeń do wybrania jednego operatora najlepsze występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="1b6fe-319">Rozpoznanie przeciążenia operatora binarnego</span><span class="sxs-lookup"><span data-stu-id="1b6fe-319">Binary operator overload resolution</span></span>

<span data-ttu-id="1b6fe-320">Operacji formularza `x op y`, gdzie `op` jest Oczekiwano operatora binarnego `x` to wyrażenie typu `X`, i `y` to wyrażenie typu `Y`, jest przetwarzana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="1b6fe-321">Zestaw operatory zdefiniowane przez użytkownika Release candidate dostarczone przez `X` i `Y` operacji `operator op(x,y)` jest określana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="1b6fe-322">Zestaw składa się z Unii operatory Release candidate, dostarczone przez `X` i operatory Release candidate, dostarczone przez `Y`, każdy określone, używając reguł [Release Candidate zdefiniowane przez użytkownika operatory](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="1b6fe-323">Jeśli `X` i `Y` tego samego typu lub, jeśli `X` i `Y` pochodzą od typu podstawowego typowych operatorów udostępnionego Release candidate występować tylko w połączony zestaw raz, a następnie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="1b6fe-324">Jeśli zestaw Release candidate operatory zdefiniowane przez użytkownika nie jest pusta, to staje się zestaw operatorów Release candidate dla tej operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="1b6fe-325">W przeciwnym razie wstępnie zdefiniowanych danych binarnych `operator op` implementacji, łącznie z podniesionym formularzy, stają się zestaw operatorów Release candidate dla tej operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="1b6fe-326">Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([operatorów arytmetycznych](expressions.md#arithmetic-operators) za pośrednictwem [warunkowego operatorów logicznych](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="1b6fe-327">Dla wstępnie zdefiniowanego typu wyliczeniowego i delegata operatorów tylko operatory uważane za są identyczne ze zdefiniowanymi przez typ wyliczenia lub delegata, który jest typem powiązania w czasie, jeden z argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="1b6fe-328">Zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution) są stosowane do zestawu operatorów Release candidate, aby wybrać najlepsze operatora w odniesieniu do listy argumentów `(x,y)`, a ten operator staje się wynikiem przeciążenia proces rozpoznawania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="1b6fe-329">W przypadku niepowodzenia funkcja rozpoznawania przeciążeń do wybrania jednego operatora najlepsze występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="1b6fe-330">Operatory zdefiniowane przez użytkownika Release Candidate</span><span class="sxs-lookup"><span data-stu-id="1b6fe-330">Candidate user-defined operators</span></span>

<span data-ttu-id="1b6fe-331">Podany typ `T` i operacji `operator op(A)`, gdzie `op` jest Oczekiwano operatora i `A` jest zdefiniowane przez użytkownika operatory, które są dostarczane przez listę argumentów, zbiór Release candidate `T` dla `operator op(A)` jest określana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="1b6fe-332">Określanie typu `T0`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-332">Determine the type `T0`.</span></span> <span data-ttu-id="1b6fe-333">Jeśli `T` jest typem zerowalnym `T0` jest jej typ podstawowy, w przeciwnym razie `T0` jest równa `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="1b6fe-334">Dla wszystkich `operator op` deklaracji w `T0` i zniesione wszystkie rodzaje tych operatorów, jeśli co najmniej jeden operator ma zastosowanie ([dotyczy funkcja składowa](expressions.md#applicable-function-member)) w odniesieniu do listy argumentów `A`, następnie zestawu Operatory Release Candidate składa się z tych operatorów stosowanych w `T0`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="1b6fe-335">W przeciwnym razie, jeśli `T0` jest `object`, zestaw operatorów Release candidate jest pusty.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="1b6fe-336">W przeciwnym razie zestaw operatorów Release candidate udostępniane przez `T0` to zestaw operatorów Release candidate, dostarczone przez bezpośrednie klasy bazowej `T0`, lub od klasy bazowej `T0` Jeśli `T0` jest parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="1b6fe-337">Promocji na liczbowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-337">Numeric promotions</span></span>

<span data-ttu-id="1b6fe-338">Promocji na liczbowe składa się z automatyczne wykonywanie niektórych niejawne konwersje operandów jednoargumentowe wstępnie zdefiniowanych i operatory dwuargumentowe liczbowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="1b6fe-339">Promocji na liczbowe nie jest mechanizm różne, ale raczej efekt zastosowania rozwiązania przeciążenia operatorów wstępnie zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="1b6fe-340">Promocji na liczbowe specjalnie nie wpływa na obliczania operatorów zdefiniowanych przez użytkownika, mimo że operatory zdefiniowane przez użytkownika mogą zostać wdrożone wykazują podobne skutki.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="1b6fe-341">Jako przykład promocji na liczbowe należy wziąć pod uwagę wstępnie zdefiniowanych implementacjach dane binarne `*` operator:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="1b6fe-342">Kiedy przeciążenie reguł rozwiązywania ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)) są stosowane do tego zestawu operatorów, efekt polega na wybraniu pierwszej operatorów, dla których istnieje niejawne konwersje z argumentami o typach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="1b6fe-343">Na przykład dla operacji `b * s`, gdzie `b` jest `byte` i `s` jest `short`, przeciążenia, wybiera rozpoznawania `operator *(int,int)` jako operator najlepsze.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="1b6fe-344">W związku z tym, powoduje, że `b` i `s` są konwertowane na `int`, a typ wyniku jest `int`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="1b6fe-345">Podobnie, dla operacji `i * d`, gdzie `i` jest `int` i `d` jest `double`, przeciążenia, wybiera rozpoznawania `operator *(double,double)` jako operator najlepsze.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="1b6fe-346">Jednoargumentowy liczbowych promocje</span><span class="sxs-lookup"><span data-stu-id="1b6fe-346">Unary numeric promotions</span></span>

<span data-ttu-id="1b6fe-347">Jednoargumentowy liczbowych podwyższania poziomu pojawia się dla argumentów operacji jest wstępnie zdefiniowane `+`, `-`, i `~` operatorów jednoargumentowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="1b6fe-348">Jednoargumentowy liczbowych podwyższania poziomu po prostu składa się z konwersji argumentów operacji typu `sbyte`, `byte`, `short`, `ushort`, lub `char` na typ `int`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="1b6fe-349">Ponadto w przypadku jednoargumentowe `-` operatora jednoargumentowego liczbowych podwyższania poziomu konwertuje argumentów operacji typu `uint` na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="1b6fe-350">Binarny promocji liczbowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-350">Binary numeric promotions</span></span>

<span data-ttu-id="1b6fe-351">Binarny liczbowych podwyższania poziomu pojawia się dla argumentów operacji jest wstępnie zdefiniowane `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, i `<=` operatory binarne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="1b6fe-352">Binarny liczbowych podwyższania poziomu niejawnie konwertuje oba operandy do typu wspólnego, które w razie nierelacyjnych operatorów, staje się również typ wyniku operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="1b6fe-353">Binarny liczbowych podwyższania poziomu polega na stosowaniu następujące reguły, w kolejności, w jakiej znajdują się w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="1b6fe-354">Jeśli jeden z operandów jest typu `decimal`, to drugi operand jest konwertowany na typ `decimal`, lub występuje błąd powiązania, to drugi operand jest typu `float` lub `double`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="1b6fe-355">W przeciwnym razie, jeśli jeden z operandów jest typu `double`, to drugi operand jest konwertowany na typ `double`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="1b6fe-356">W przeciwnym razie, jeśli jeden z operandów jest typu `float`, to drugi operand jest konwertowany na typ `float`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="1b6fe-357">W przeciwnym razie, jeśli jeden z operandów jest typu `ulong`, to drugi operand jest konwertowany na typ `ulong`, lub występuje błąd powiązania, to drugi operand jest typu `sbyte`, `short`, `int`, lub `long`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="1b6fe-358">W przeciwnym razie, jeśli jeden z operandów jest typu `long`, to drugi operand jest konwertowany na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="1b6fe-359">W przeciwnym razie, jeśli jeden z operandów jest typu `uint` i drugi operand jest typu `sbyte`, `short`, lub `int`, oba operandy są konwertowane na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="1b6fe-360">W przeciwnym razie, jeśli jeden z operandów jest typu `uint`, to drugi operand jest konwertowany na typ `uint`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="1b6fe-361">W przeciwnym razie oba operandy są konwertowane na typ `int`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="1b6fe-362">Należy pamiętać, że pierwszej reguły nie zezwala na wszystkie operacje mieszać `decimal` to typ `double` i `float` typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="1b6fe-363">Reguła wynika z faktu, że istnieje nie niejawne konwersje między `decimal` typu i `double` i `float` typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="1b6fe-364">Należy również zauważyć, że nie jest możliwe, że argument typu `ulong` kiedy drugi operand jest typ całkowity ze znakiem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="1b6fe-365">Przyczyną jest to, że istnieje nie typu całkowitego, która może reprezentować pełny zakres `ulong` oraz podpisanych typów całkowitych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="1b6fe-366">W obu powyższych przypadkach wyrażenia rzutowania można jawnie przekonwertować jeden argument do typu, który jest zgodny z drugiego operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="1b6fe-367">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="1b6fe-368">występuje błąd powiązania, ponieważ `decimal` nie może zostać pomnożona przez `double`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="1b6fe-369">Błąd został rozwiązany przez jawnej konwersji drugiego operandu `decimal`, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="1b6fe-370">Operatory podniesionym</span><span class="sxs-lookup"><span data-stu-id="1b6fe-370">Lifted operators</span></span>

<span data-ttu-id="1b6fe-371">***Podniesiony operatory*** zezwala na wstępnie zdefiniowanych i zdefiniowanych przez użytkownika operatorów, które działają na typy wartości nieprzyjmujące ma być używany z formularzami dopuszcza wartości null z tych typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="1b6fe-372">Operatory podniesionym są konstruowane na podstawie wstępnie zdefiniowanych i zdefiniowanych przez użytkownika operatorów, które spełniają określone wymagania zgodnie z opisem w następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="1b6fe-373">Operatorów jednoargumentowych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="1b6fe-374">podniesionym formie operatora istnieje, jeśli typy argumentów i wynik są typami wartości niedopuszczającym wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="1b6fe-375">Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator typom argumentów i wyniku.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="1b6fe-376">Operator podniesionym tworzy wartość null, jeśli argument ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="1b6fe-377">W przeciwnym razie podniesionym operator dekoduje operand, stosuje operator podstawowych i otacza wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="1b6fe-378">Aby uzyskać operatory binarne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="1b6fe-379">podniesionym formie operatora istnieje, jeśli typy argumentów i wynik są wszystkie typy wartości nieprzyjmujące wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="1b6fe-380">Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator dla każdego typu argumentu operacji i wyników.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="1b6fe-381">Operator podniesionym tworzy wartość null, jeśli jeden lub oba operandy mają wartość null (wyjątek jest `&` i `|` Operatorzy `bool?` typ, zgodnie z opisem w [logiczna operatorów logicznych](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="1b6fe-382">W przeciwnym razie podniesionym operator dekoduje operandów, stosuje operator podstawowych i otacza wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="1b6fe-383">Aby uzyskać Operatory równości</span><span class="sxs-lookup"><span data-stu-id="1b6fe-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="1b6fe-384">istnieje podniesionym formie operatora, jeśli typy argumentów operacji są typy wartości nieprzyjmujące i czy typ wyniku `bool`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="1b6fe-385">Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator dla każdego typu operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="1b6fe-386">Operator podniesionym uwzględnia równości dwóch wartości null i wartości null nierówne na jakąkolwiek wartość inną niż null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="1b6fe-387">Jeśli oba operandy są inne niż null, operator podniesionym dekoduje operandów i stosuje bazowego operatora, który generuje `bool` wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="1b6fe-388">Aby uzyskać operatory relacyjne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="1b6fe-389">istnieje podniesionym formie operatora, jeśli typy argumentów operacji są typy wartości nieprzyjmujące i czy typ wyniku `bool`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="1b6fe-390">Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator dla każdego typu operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="1b6fe-391">Operator podniesionym generuje wartość `false` Jeśli jeden lub oba operandy są wartości null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="1b6fe-392">W przeciwnym razie podniesionym operator dekoduje argumenty operacji i stosuje bazowego operatora, który generuje `bool` wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="1b6fe-393">Wyszukiwanie elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="1b6fe-393">Member lookup</span></span>

<span data-ttu-id="1b6fe-394">Wyszukiwanie elementu członkowskiego jest proces, według której jest określana znaczenie nazwę w kontekście określonego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="1b6fe-395">Wyszukanie członka może wystąpić w ramach oceny *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="1b6fe-396">Jeśli *simple_name* lub *member_access* występuje jako *primary_expression* z *invocation_expression* ([ Wywołań metod](expressions.md#method-invocations)), jest nazywany można wywołać elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="1b6fe-397">W przypadku metody lub zdarzenia, czy jest stała, pole lub właściwość typu delegata ([delegatów](delegates.md)) lub typu `dynamic` ([typu dynamicznego](types.md#the-dynamic-type)), a następnie element członkowski jest nazywany *można wywołać*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="1b6fe-398">Wyszukanie członka bierze pod uwagę nie tylko nazwę elementu członkowskiego, ale także liczbę parametrów typu, który element członkowski ma i tego, czy element jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="1b6fe-399">Na potrzeby wyszukanie członka metody rodzajowe i zagnieżdżonych typów rodzajowych ma liczbę parametrów typu w swoich deklaracjach odpowiednich i inni członkowie mają parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="1b6fe-400">Wyszukiwanie elementu członkowskiego nazwy `N` z `K`  parametrów typu w typie `T` są przetwarzane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="1b6fe-401">Po pierwsze, zbiór dostępne elementy członkowskie o nazwie `N` jest określana:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="1b6fe-402">Jeśli `T` jest parametrem typu, a następnie zestaw jest złożenie zestawy dostępne elementy członkowskie o nazwie `N` w każdym z typami określonymi jako ograniczenia podstawowego lub ograniczenia dodatkowej ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) dla  `T`, wraz z zestawu dostępnych składowych o nazwie `N` w `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="1b6fe-403">W przeciwnym razie zestaw zawiera wszystkie dostępne ([dostęp do elementu członkowskiego](basic-concepts.md#member-access)) elementy członkowskie o nazwie `N` w `T`, w tym dziedziczone elementy członkowskie i dostępne elementy członkowskie, o nazwie `N` w `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="1b6fe-404">Jeśli `T` jest zbudowany typ, zestaw elementów członkowskich są uzyskiwane poprzez zastąpienie argumentów typu zgodnie z opisem w [członkowie typy utworzone](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="1b6fe-405">Elementy członkowskie, które obejmują `override` modyfikator są wykluczone z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="1b6fe-406">Następnie, jeśli `K` wynosi zero, wszystkie zagnieżdżone typy, których deklaracje zawierają parametry typu są usuwane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="1b6fe-407">Jeśli `K` jest nie jest zerowa, wszystkie elementy członkowskie z różną liczbę typów parametrów są usuwane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="1b6fe-408">Należy pamiętać, że w przypadku `K` zero, metody ma typ parametry nie są usuwane, ponieważ procesu wnioskowania typu ([wnioskowanie o typie](expressions.md#type-inference)) można wywnioskować argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="1b6fe-409">Następnie, jeśli element jest *wywoływane*, wszystkich innych niż-*można wywołać* elementy członkowskie są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="1b6fe-410">Następnie elementy członkowskie, które są ukryte przez innych członków są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="1b6fe-411">Dla każdego członka `S.M` w zestawie, gdzie `S` jest typem, w którym element członkowski `M` zadeklarowano, stosowane są następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="1b6fe-412">Jeśli `M` jest — stała, pola, właściwości, zdarzenia lub elementu członkowskiego wyliczenia, a następnie wszystkich elementów członkowskich zadeklarowanych w podstawowym typem `S` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="1b6fe-413">Jeśli `M` jest deklaracja typu, a następnie wszystkie inne niż typy zadeklarowane w typie podstawowym z `S` są usuwane z zestawu, a następnie wpisz wszystkie deklaracje z taką samą liczbę parametrów typu jako `M` zadeklarowane w typie podstawowym z `S` są usuwane z tego zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="1b6fe-414">Jeśli `M` jest metodą, a następnie wszyscy członkowie-metoda zadeklarowana w typie podstawowym z `S` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="1b6fe-415">Następnie składowych interfejsu, ukryte przez elementy członkowskie klasy są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="1b6fe-416">Ten krok tylko obowiązuje, jeśli `T` jest parametrem typu i `T` innych niż ma zarówno skuteczne klasę bazową `object` i pusty interfejs skuteczne, konfiguruje ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="1b6fe-417">Dla każdego członka `S.M` w zestawie, gdzie `S` jest typem, w którym element członkowski `M` zadeklarowano, stosowane są następujące reguły, jeśli `S` jest inny niż deklarację klasy `object`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="1b6fe-418">Jeśli `M` jest — stała, pola, właściwości, zdarzenia, składowej wyliczenia lub deklaracji typu, a następnie wszystkich elementów członkowskich zadeklarowanych w deklaracji interfejsu są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="1b6fe-419">Jeśli `M` jest metodą, a następnie wszystkich-metoda elementów członkowskich zadeklarowanych w deklaracji interfejsu są usuwane z zestawu i wszystkich metod z taką samą sygnaturę jak `M` zadeklarowane w interfejsie deklaracji są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="1b6fe-420">Ponadto usunięto ukrytych członków, jest określana wynik wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="1b6fe-421">Jeżeli zestaw zawiera jeden element członkowski, który nie jest metodą, ten element jest wynikiem wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="1b6fe-422">W przeciwnym razie jeśli zestaw zawiera tylko metody, tej grupy metod jest wynikiem wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="1b6fe-423">W przeciwnym razie wyszukiwanie jest niejednoznaczne i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-424">Element członkowski wyszukiwań w typów innych niż parametrów typu i interfejsy i wyszukiwania elementu członkowskiego w interfejsach, które są ściśle pojedyncze dziedziczenie (każdego interfejsu w łańcuch dziedziczenia ma dokładnie zero lub jeden bezpośredni interfejs podstawowy) powoduje reguły wyszukiwania po prostu który uzyskiwany składowe bazowe Ukryj członków przy użyciu tej samej nazwie lub podpisie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="1b6fe-425">Takie pojedyncze dziedziczenie wyszukiwań nigdy nie są niejednoznaczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="1b6fe-426">Niejednoznaczności, które prawdopodobnie mogą wynikać z elementu członkowskiego wyszukiwań w interfejsach dziedziczenia wielokrotnego są opisane w [interfejs dostępu do elementu członkowskiego](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="1b6fe-427">Typy podstawowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-427">Base types</span></span>

<span data-ttu-id="1b6fe-428">Na potrzeby wyszukiwania elementu członkowskiego, typem `T` została uznana za następujące typy podstawowe:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="1b6fe-429">Jeśli `T` jest `object`, następnie `T` nie ma podstawowego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="1b6fe-430">Jeśli `T` jest *enum_type*, typy podstawowe z `T` są typami klas `System.Enum`, `System.ValueType`, i `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="1b6fe-431">Jeśli `T` jest *struct_type*, typy podstawowe z `T` są typami klas `System.ValueType` i `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="1b6fe-432">Jeśli `T` jest *class_type*, typy podstawowe z `T` są klasy bazowe `T`, łącznie z typem klasy `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="1b6fe-433">Jeśli `T` jest *interface_type*, typy podstawowe z `T` są interfejsy podstawowe z `T` i typ klasy `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="1b6fe-434">Jeśli `T` jest *array_type*, typy podstawowe z `T` są typami klas `System.Array` i `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="1b6fe-435">Jeśli `T` jest *delegate_type*, typy podstawowe z `T` są typami klas `System.Delegate` i `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="1b6fe-436">Elementy członkowskie — funkcja</span><span class="sxs-lookup"><span data-stu-id="1b6fe-436">Function members</span></span>

<span data-ttu-id="1b6fe-437">Składowe funkcji są elementy członkowskie, które zawierać instrukcji wykonywalnych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="1b6fe-438">Składowe funkcji są zawsze członkowie typów i nie może być elementów członkowskich przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="1b6fe-439">Język C# definiuje następujące kategorie funkcji elementów członkowskich:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="1b6fe-440">Metody</span><span class="sxs-lookup"><span data-stu-id="1b6fe-440">Methods</span></span>
*  <span data-ttu-id="1b6fe-441">Właściwości</span><span class="sxs-lookup"><span data-stu-id="1b6fe-441">Properties</span></span>
*  <span data-ttu-id="1b6fe-442">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-442">Events</span></span>
*  <span data-ttu-id="1b6fe-443">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="1b6fe-443">Indexers</span></span>
*  <span data-ttu-id="1b6fe-444">Operatory zdefiniowane przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="1b6fe-444">User-defined operators</span></span>
*  <span data-ttu-id="1b6fe-445">Konstruktory wystąpień</span><span class="sxs-lookup"><span data-stu-id="1b6fe-445">Instance constructors</span></span>
*  <span data-ttu-id="1b6fe-446">Konstruktory statyczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-446">Static constructors</span></span>
*  <span data-ttu-id="1b6fe-447">Destruktory</span><span class="sxs-lookup"><span data-stu-id="1b6fe-447">Destructors</span></span>

<span data-ttu-id="1b6fe-448">Z wyjątkiem destruktory i konstruktory statyczne (których nie można jawnie wywołać) instrukcje zawarte w składowe funkcji są wykonywane za pośrednictwem wywołania elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="1b6fe-449">Składnia pisania wywołanie funkcji elementu członkowskiego jest zależna od kategorię składowej określonej funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="1b6fe-450">Na liście argumentów ([listy argumentów](expressions.md#argument-lists)) funkcja elementu członkowskiego wywołania zawiera rzeczywiste wartości i odwołań do zmiennych dla parametrów funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="1b6fe-451">Wywołań metod ogólnych może wykorzystywać wnioskowanie o typie do określania zestawu argumentów typu do przekazania do metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="1b6fe-452">Ten proces jest opisany w [wnioskowanie o typie](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="1b6fe-453">Wywołania metody, indeksatory, operatory i konstruktory wystąpień zatrudniać Rozpoznanie przeciążenia, aby określić, które zestaw Release candidate elementów członkowskich funkcja do wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="1b6fe-454">Ten proces jest opisany w [Rozpoznanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="1b6fe-455">Po członka określonej funkcji został określony w momencie powiązania, prawdopodobnie za pośrednictwem rozwiązania przeciążenia rzeczywisty proces środowiska wykonawczego wywołanie funkcji elementu członkowskiego jest opisany w [sprawdzanie dynamiczne przeciążonymkompilacji](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="1b6fe-456">Poniższa tabela zawiera podsumowanie przetwarzania, które odbywa się w konstrukcji obejmujące sześć kategorii funkcji elementów członkowskich, które mogą być wywoływane jawnie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="1b6fe-457">W tabeli `e`, `x`, `y`, i `value` wskazują sklasyfikowane jako zmiennych lub wartości wyrażenia `T` wskazuje wyrażenie sklasyfikowane jako typ `F` jest prostą nazwą metody i `P` jest prostą nazwą właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="1b6fe-458">__Construct__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-458">__Construct__</span></span>     | <span data-ttu-id="1b6fe-459">__Przykład__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-459">__Example__</span></span>    | <span data-ttu-id="1b6fe-460">__Opis__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="1b6fe-461">Wywołanie metody</span><span class="sxs-lookup"><span data-stu-id="1b6fe-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="1b6fe-462">Rozpoznanie przeciążenia jest stosowany do wybrania najlepszej metody `F` w zawierającego klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="1b6fe-463">Metoda jest wywoływana z listy argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="1b6fe-464">Jeśli metoda nie jest `static`, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="1b6fe-465">Rozpoznanie przeciążenia jest stosowany do wybrania najlepszej metody `F` w klasie lub strukturze `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="1b6fe-466">Występuje błąd powiązania, jeśli metoda nie jest `static`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="1b6fe-467">Metoda jest wywoływana z listy argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="1b6fe-468">Rozpoznanie przeciążenia jest stosowany do Wybierz najlepszą metodę F klasy, struktury lub interfejsu, określone przez typ `e`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="1b6fe-469">Występuje błąd powiązania, jeśli metoda jest `static`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="1b6fe-470">Metoda jest wywoływana za pomocą wyrażenia wystąpienia `e` i listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="1b6fe-471">Dostęp do właściwości</span><span class="sxs-lookup"><span data-stu-id="1b6fe-471">Property access</span></span>   | `P`            | <span data-ttu-id="1b6fe-472">`get` Metody dostępu właściwości `P` w zawierającego klasy lub struktury jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="1b6fe-473">Występuje błąd kompilacji, jeśli `P` jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="1b6fe-474">Jeśli `P` nie `static`, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="1b6fe-475">`set` Metody dostępu właściwości `P` w zawierającego klasy lub struktury jest wywoływana z listy argumentów `(value)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="1b6fe-476">Występuje błąd kompilacji, jeśli `P` jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="1b6fe-477">Jeśli `P` nie `static`, wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="1b6fe-478">`get` Metody dostępu właściwości `P` w klasie lub strukturze `T` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="1b6fe-479">Występuje błąd kompilacji, jeśli `P` nie `static` lub jeśli `P` jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="1b6fe-480">`set` Metody dostępu właściwości `P` w klasie lub strukturze `T` jest wywoływana z listy argumentów `(value)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="1b6fe-481">Występuje błąd kompilacji, jeśli `P` nie `static` lub jeśli `P` jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="1b6fe-482">`get` Metody dostępu właściwości `P` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="1b6fe-483">Występuje błąd powiązania, jeśli `P` jest `static` lub jeśli `P` jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="1b6fe-484">`set` Metody dostępu właściwości `P` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e` i listą argumentów `(value)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="1b6fe-485">Występuje błąd powiązania, jeśli `P` jest `static` lub jeśli `P` jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="1b6fe-486">Dostęp do zdarzenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="1b6fe-487">`add` Metody dostępu zdarzenia `E` w zawierającego klasy lub struktury jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="1b6fe-488">Jeśli `E` jest nie statyczne wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="1b6fe-489">`remove` Metody dostępu zdarzenia `E` w zawierającego klasy lub struktury jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="1b6fe-490">Jeśli `E` jest nie statyczne wyrażenie wystąpienia jest `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="1b6fe-491">`add` Metody dostępu zdarzenia `E` w klasie lub strukturze `T` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="1b6fe-492">Występuje błąd powiązania, jeśli `E` nie jest statyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="1b6fe-493">`remove` Metody dostępu zdarzenia `E` w klasie lub strukturze `T` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="1b6fe-494">Występuje błąd powiązania, jeśli `E` nie jest statyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="1b6fe-495">`add` Metody dostępu zdarzenia `E` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="1b6fe-496">Występuje błąd powiązania, jeśli `E` jest statyczna.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="1b6fe-497">`remove` Metody dostępu zdarzenia `E` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="1b6fe-498">Występuje błąd powiązania, jeśli `E` jest statyczna.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="1b6fe-499">Dostęp indeksatora</span><span class="sxs-lookup"><span data-stu-id="1b6fe-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="1b6fe-500">Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze indeksatora w klasy, struktury lub interfejsu, określone przez typ e.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="1b6fe-501">`get` Akcesor indeksatora jest wywoływana z wyrażenia wystąpienia `e` i listą argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="1b6fe-502">Błąd powiązania występuje, jeśli indeksatora jest tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="1b6fe-503">Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze indeksatora w klasy, struktury lub interfejsu, określone przez typ `e`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="1b6fe-504">`set` Akcesor indeksatora jest wywoływana z wyrażenia wystąpienia `e` i listą argumentów `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="1b6fe-505">Błąd powiązania występuje, jeśli indeksatora jest tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="1b6fe-506">Operator wywołania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="1b6fe-507">Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze operatora jednoargumentowego klasy lub struktury, określone przez typ `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="1b6fe-508">Wybrane operator jest wywoływany z listy argumentów `(x)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="1b6fe-509">Rozpoznanie przeciążenia jest stosowany do wybrania najlepszej operatora binarnego klasach lub strukturach określone przez typy `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="1b6fe-510">Wybrane operator jest wywoływany z listy argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="1b6fe-511">Wywołanie konstruktora wystąpienia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="1b6fe-512">Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze konstruktora wystąpienia klasy lub struktury `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="1b6fe-513">Konstruktor wystąpienia jest wywoływana z listy argumentów `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="1b6fe-514">Listy argumentów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-514">Argument lists</span></span>

<span data-ttu-id="1b6fe-515">Każdy element członkowski i delegata wywołania funkcji obejmuje udostępniającej rzeczywiste wartości i odwołań do zmiennych parametrów funkcja składowa listy argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="1b6fe-516">Składnia służąca do określania listy argumentów wywołania funkcji elementu członkowskiego zależy od kategorii element członkowski funkcji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="1b6fe-517">Na przykład konstruktorów, metod, indeksatorów i delegatów, argumenty są określane jako *argument_list*, zgodnie z poniższym opisem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="1b6fe-518">Dla indeksatorów, podczas wywoływania `set` akcesora do listy argumentów Ponadto obejmuje wyrażenie określone jako prawy operand operatora przypisania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="1b6fe-519">W przypadku właściwości lista argumentów jest pusta, podczas wywoływania `get` metody dostępu i składa się z wyrażenie określone jako prawy operand operatora przypisania podczas wywoływania `set` metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="1b6fe-520">W przypadku zdarzeń z listą argumentów składa się z wyrażenie określone jako prawy operand `+=` lub `-=` operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="1b6fe-521">Operatory zdefiniowane przez użytkownika na liście argumentów składa się z jeden argument operacji operatora jednoargumentowego lub dwa argumenty operacji operatora binarnego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="1b6fe-522">Argumenty właściwości ([właściwości](classes.md#properties)), zdarzenia ([zdarzenia](classes.md#events)) i operatorów zdefiniowanych przez użytkownika ([operatory](classes.md#operators)) zawsze są przekazywane jako wartości parametrów ([ Wartości parametrów](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="1b6fe-523">Argumenty indeksatorów ([indeksatory](classes.md#indexers)) zawsze są przekazywane jako wartości parametrów ([wartości parametrów](classes.md#value-parameters)) lub parameter — tablice ([Parameter — tablice](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="1b6fe-524">Parametry odwołań i dane wyjściowe nie są obsługiwane dla tych kategorii funkcji elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="1b6fe-525">Argumenty wywołania konstruktora, metoda, indeksator lub delegata wystąpienia są określane jako *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="1b6fe-526">*Argument_list* składa się z co najmniej jeden *argument*s, oddzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="1b6fe-527">Każdy argument składa się opcjonalny *argument_name* następuje *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="1b6fe-528">*Argument* z *argument_name* nazywa się ***nazwany argument***, podczas gdy *argument* bez  *argument_name* jest ***argument pozycyjny***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="1b6fe-529">Jest to błąd dla argumentu pozycyjnego pojawi się po argumentu nazwanego w *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="1b6fe-530">*Argument_value* może mieć jedną z następujących form:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="1b6fe-531">*Wyrażenie*, co oznacza, że argument jest przekazywany jako parametr wartości ([wartości parametrów](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="1b6fe-532">Słowo kluczowe `ref` następuje *variable_reference* ([odwołań do zmiennych](variables.md#variable-references)), który wskazuje, że argument jest przekazywany jako parametr przekazany przez odwołanie ([odwołania do parametrów ](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="1b6fe-533">Zmienna musi zostać zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) zanim może być przekazywany jako parametr przekazany przez odwołanie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="1b6fe-534">Słowo kluczowe `out` następuje *variable_reference* ([odwołań do zmiennych](variables.md#variable-references)), co oznacza, że argument jest przekazywany jako parametr wyjściowy ([parametrówwyjściowych](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="1b6fe-535">Zmienna jest uznawany za zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) po funkcji wywołanie elementu członkowskiego, w którym zmienna jest przekazywana jako parametr wyjściowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="1b6fe-536">Odpowiednich parametrów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-536">Corresponding parameters</span></span>

<span data-ttu-id="1b6fe-537">Dla każdego argumentu na liście argumentów musi istnieć odpowiedni parametr w funkcji składowej lub delegata wywoływanego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="1b6fe-538">Lista parametrów używanych w następujących jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="1b6fe-539">Dla metod wirtualnych i zdefiniowane indeksatory w klasach lista parametrów jest pobierana z bardziej konkretny od pozostałych deklaracji lub zastąpienie elementu członkowskiego funkcji, rozpoczynając od typu statycznego odbiornik i przeszukiwania jej klas podstawowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="1b6fe-540">Dla metody interfejsu i indeksatorów, lista parametrów jest pobierane tworzą bardziej konkretny od pozostałych definicji elementu członkowskiego, rozpoczynając od typu interfejsu i przeszukiwania interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="1b6fe-541">Jeśli zostanie znaleziony żadnej listy parametrów unikatowy, listę parametrów z nazwami niedostępne i nie opcjonalne parametry jest konstruowany tak, aby wywołania nie można użyć nazwanych parametrów lub pominięcie argumentów opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="1b6fe-542">Dla metod częściowych jest używany parametr listę definiujące deklaracji metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="1b6fe-543">W przypadku pozostałych funkcji elementów członkowskich i delegatów istnieje tylko jeden parametr listę, która jest używana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="1b6fe-544">Pozycja argumentu lub parametr jest zdefiniowany jako liczba argumentów lub parametry poprzedzającym go na liście argumentów lub listy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="1b6fe-545">Odpowiednich parametrów dla funkcji składowej argumentów są określane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="1b6fe-546">Argumenty w *argument_list* konstruktory wystąpień, metod, indeksatorów i delegatów:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="1b6fe-547">Argument pozycyjny realizowana stały parametr w tej samej pozycji w liście parametrów odnosi się do tego parametru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="1b6fe-548">Argument pozycyjny funkcja elementu członkowskiego z tablicą parametrów, wywoływana w postaci normalne odnosi się do tablicy parametrów, która musi wystąpić na tej samej pozycji w liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="1b6fe-549">Argument pozycyjny funkcja elementu członkowskiego z tablicą parametrów, wywoływana w postaci rozwiniętej ma stały parametru realizowana na tej samej pozycji w liście parametrów, odnosi się do elementu w tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="1b6fe-550">Argument nazwany odnosi się do parametru o takiej samej nazwie w liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="1b6fe-551">Dla indeksatorów, podczas wywoływania `set` dostępu, wyrażenie określone jako prawy operand operatora przypisania odpowiada niejawny `value` parametru `set` deklaracji metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="1b6fe-552">W przypadku właściwości podczas wywoływania `get` ma metody dostępu są bez argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="1b6fe-553">Podczas wywoływania `set` dostępu, wyrażenie określone jako prawy operand operatora przypisania odpowiada niejawny `value` parametru `set` deklaracji metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="1b6fe-554">Operatory jednoargumentowe zdefiniowanych przez użytkownika (w tym konwersji) jeden argument operacji jest odpowiada jeden parametr Deklaracja operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="1b6fe-555">Dla zdefiniowanych przez użytkownika operatory dwuargumentowe lewy operand odnosi się do pierwszego parametru i prawy operand odnosi się do drugiego parametru Deklaracja operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="1b6fe-556">Ocena środowiska wykonawczego z listami argumentów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="1b6fe-557">Podczas przetwarzania środowiska wykonawczego wywołanie funkcji elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), wyrażeń lub zmiennych odwołania do listy argumentów są obliczane w kolejności od lewej do prawej, jako następujące:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="1b6fe-558">Wartość parametru, jest obliczane wyrażenie argumentu i niejawną konwersję ([niejawne konwersje](conversions.md#implicit-conversions)) do odpowiedniego parametru typu jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="1b6fe-559">Wartość wynikowa staje się wartością początkową parametru wartości w wywołanie funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="1b6fe-560">Dla parametru odwołania lub danych wyjściowych odwołanie do zmiennej jest szacowana, a lokalizacja wynikowa magazynu staje się lokalizacji przechowywania, reprezentowany przez parametr w wywołaniu elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="1b6fe-561">Jeśli odwołanie do zmiennej podawana jako parametr odwołanie lub danych wyjściowych jest element tablicy *reference_type*, sprawdzanie w czasie wykonania jest przeprowadzana w celu zapewnienia, że typ elementu tablicy jest taka sama jak typ parametru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="1b6fe-562">Jeżeli to sprawdzenie nie powiedzie się, `System.ArrayTypeMismatchException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="1b6fe-563">Metody, indeksatory i konstruktory wystąpień może deklarować ich parametr najdalej z prawej strony tablicy parametrów ([Parameter — tablice](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="1b6fe-564">Takich funkcji elementów członkowskich są wywoływane w ich normalnym formularza lub w postaci rozwiniętej zależności, na których jest stosowane ([dotyczy funkcja składowa](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="1b6fe-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="1b6fe-565">Wywołana funkcja składowa z tablicą parametrów w postaci normalne argument dla tablicy parametrów musi być wyrażeniem pojedynczym, który jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) na typ tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="1b6fe-566">W tym przypadku tablicy parametrów funkcjonuje dokładnie parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="1b6fe-567">Wywołana funkcja składowa z tablicą parametrów w postaci rozwiniętej wywołania należy określić zero lub więcej argumentów pozycyjnych tablicy parametrów, w którym każdy argument jest wyrażeniem, które jest niejawnie konwertowany ([niejawne Konwersje](conversions.md#implicit-conversions)) na typ elementu tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="1b6fe-568">W takim przypadku wywołanie tworzy wystąpienie typu parametru tablicy o długości odpowiadającej liczbę argumentów, inicjuje elementy wystąpienia tablicy wartościami podany argument i używa wystąpienia nowo utworzona tablica jako rzeczywisty argument.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="1b6fe-569">Wyrażenia listy argumentów są zawsze obliczane w kolejności, w której są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="1b6fe-570">W związku z tym w tym przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-570">Thus, the example</span></span>
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
<span data-ttu-id="1b6fe-571">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="1b6fe-572">Reguły odchyleń wspólnej tablicy ([Kowariancja tablicy](arrays.md#array-covariance)) zezwala na wartość typu tablicowego `A[]` jako odwołanie do wystąpienia typu tablicowego `B[]`, o ile istnieje niejawna konwersja odwołania `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="1b6fe-573">Ze względu na tych zasad, gdy element tablicy *reference_type* jest przekazywany jako parametr odwołanie lub danych wyjściowych, sprawdzanie w czasie wykonania jest wymagany w celu zagwarantowania, że typ rzeczywisty element tablicy jest taka sama jak w przypadku parametru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="1b6fe-574">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-574">In the example</span></span>
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
<span data-ttu-id="1b6fe-575">drugie wywołanie `F` powoduje, że `System.ArrayTypeMismatchException` zostanie wygenerowany, ponieważ element rzeczywisty typ `b` jest `string` i nie `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="1b6fe-576">Wywołana funkcja składowa z tablicą parametrów w postaci rozwiniętej wywołanie jest przetwarzany, dokładnie tak, jakby wyrażenie tworzenia tablicy za pomocą inicjatora tablicy ([wyrażenie tworzenia tablicy](expressions.md#array-creation-expressions)) został wstawiony wokół Parametry rozwinięty.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="1b6fe-577">Na przykład biorąc pod uwagę deklaracji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="1b6fe-578">następujące wywołania rozszerzonej postaci metody</span><span class="sxs-lookup"><span data-stu-id="1b6fe-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="1b6fe-579">dokładnie odpowiadać</span><span class="sxs-lookup"><span data-stu-id="1b6fe-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="1b6fe-580">W szczególności należy pamiętać, że pusta tablica jest tworzony w przypadku zero argumentów dla tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="1b6fe-581">Jeśli argumenty zostały pominięte w funkcja składowa za pomocą odpowiednich parametrów opcjonalnych, argumenty domyślne deklaracji elementu członkowskiego funkcji niejawnie są przekazywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="1b6fe-582">Ponieważ to jest zawsze stałe, ich oceny nie ma wpływu na kolejność oceny pozostałe argumenty.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="1b6fe-583">Wnioskowanie o typie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-583">Type inference</span></span>

<span data-ttu-id="1b6fe-584">Po wywołaniu metody ogólnej bez określania argumentów typu ***wnioskowanie o typie*** proces próbuje wywnioskować argumentów typu na wywołanie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="1b6fe-585">Obecność wnioskowanie o typie umożliwia bardziej wygodne składni ma być używany do wywoływania metody rodzajowej i pozwala programisty uniknąć określania informacji o typie nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="1b6fe-586">Na przykład biorąc pod uwagę deklaracji metody:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="1b6fe-587">istnieje możliwość wywołania `Choose` metody bez jawne określenie argumentów typu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="1b6fe-588">Za pomocą wnioskowanie o typie, argumenty typu `int` i `string` są ustalane na podstawie argumenty do metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="1b6fe-589">Wnioskowanie o typie występuje w ramach przetwarzania powiązania w czasie wywołania metody ([wywołań metody opisywanego](expressions.md#method-invocations)) i ma miejsce przed wykonaniem kroku rozdzielczość przeciążenia wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="1b6fe-590">Gdy w wywołaniu metody zostanie określona grupa konkretnej metody bez argumentów typu są określone jako część wywołania metody, wnioskowanie o typie są stosowane do każdej metody rodzajowej, w grupie metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="1b6fe-591">Jeśli wnioskowanie o typie zakończy się powodzeniem, argumentami typu wywnioskowanego są używane do określ typy tych argumentów dla kolejnych przeciążeniu rozdzielczości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="1b6fe-592">Jeśli funkcja rozpoznawania przeciążeń wybiera metody rodzajowej, do wywołania, argumentami typu wywnioskowanego są używane jako argumenty typu rzeczywistego wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="1b6fe-593">W przypadku niepowodzenia wnioskowanie o typie dla konkretnej metody tej metody nie uczestniczy w przeciążeniu rozdzielczości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="1b6fe-594">Błąd wnioskowanie o typie, w i samego siebie, nie powoduje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="1b6fe-595">Jednak często prowadzi do Błąd powiązania w czasie gdy funkcja rozpoznawania przeciążeń następnie nie odnajdzie żadnych odpowiednich metod.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="1b6fe-596">Jeśli podana liczba argumentów jest inna niż liczba parametrów w metodzie, następnie wnioskowania natychmiast kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="1b6fe-597">W przeciwnym razie Załóżmy, że metody ogólnej ma następujący podpis:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="1b6fe-598">Za pomocą wywołania metody formularza `M(E1...Em)` zadanie wnioskowanie o typie jest znalezienie argumentów typu unikatowy `S1...Sn` dla każdego z parametrów typu `X1...Xn` tak, aby wywołanie `M<S1...Sn>(E1...Em)` staje się nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="1b6fe-599">W trakcie procesu wnioskowania każdego parametru typu `Xi` jest *stałej* do określonego typu `Si` lub *Niepoprawione* skojarzony zestaw *granice*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="1b6fe-600">Każdy z granicami jest określonego typu `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="1b6fe-601">Początkowo każda zmienna typu `Xi` jest Niepoprawione z pustą parą granic.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="1b6fe-602">Wnioskowanie o typie odbywa się etapami.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-602">Type inference takes place in phases.</span></span> <span data-ttu-id="1b6fe-603">Każda faza podejmie próbę wywnioskować argumentów typu dla więcej zmiennych typu na podstawie otrzymanych wyników poprzedniej fazy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="1b6fe-604">Pierwsza faza sprawia, że niektóre początkowej wniosków granice drugiej fazy zmiennych typu do określonych typów poprawek i dalsze wnioskuje granice.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="1b6fe-605">Drugi etap musi być powtarzane wielokrotnie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="1b6fe-606">*Uwaga:* Typ wnioskowania odbywa się nie tylko w przypadku, gdy wywoływana jest metoda ogólnego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="1b6fe-607">Wnioskowanie o typie dla konwersji grupy metod jest opisana w [wnioskowanie konwersji grupy metod typu](expressions.md#type-inference-for-conversion-of-method-groups) i znajdowanie najlepszy typ wspólnego zestawu wyrażeń jest opisany w [znajdowanie najlepszy typ wspólnego zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="1b6fe-608">Pierwsza faza</span><span class="sxs-lookup"><span data-stu-id="1b6fe-608">The first phase</span></span>

<span data-ttu-id="1b6fe-609">Dla każdego z podanych argumentów metody `Ei`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="1b6fe-610">Jeśli `Ei` jest funkcją anonimową *wnioskowanie o typie parametru jawne* ([parametr jawny typ wniosków](expressions.md#explicit-parameter-type-inferences)) zostało wprowadzone przy użyciu `Ei` do `Ti`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="1b6fe-611">W przeciwnym razie, jeśli `Ei` ma typ `U` i `xi` parametr wartość, a następnie *wnioskowania dolna granica* wykonano *z* `U` *do* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="1b6fe-612">W przeciwnym razie, jeśli `Ei` ma typ `U` i `xi` jest `ref` lub `out` parametru, a następnie *dokładnie wnioskowania* wykonano *z* `U` *do* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="1b6fe-613">W przeciwnym razie wykonywane nie wnioskowania dla tego argumentu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="1b6fe-614">Drugi etap</span><span class="sxs-lookup"><span data-stu-id="1b6fe-614">The second phase</span></span>

<span data-ttu-id="1b6fe-615">Drugi etap przebiega w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="1b6fe-616">Wszystkie *Niepoprawione* wpisz zmienne `Xi` nie obsługują *zależą od* ([zależność](expressions.md#dependence)) wszelkie `Xj` są stałe ([ustalanie](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="1b6fe-617">Jeśli istnieje żadnych zmiennych typu, wszystkie *Niepoprawione* wpisz zmienne `Xi` są *stałej* dla którego pomieścić wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="1b6fe-618">Istnieje co najmniej jedną zmienną typu `Xj` zależy `Xi`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="1b6fe-619">`Xi` ma inne niż pusty zestaw granic</span><span class="sxs-lookup"><span data-stu-id="1b6fe-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="1b6fe-620">Jeśli istnieje żadnych zmiennych typu i nadal istnieją *Niepoprawione* wpisz zmiennych, wpisz wnioskowania kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="1b6fe-621">W przeciwnym razie, jeśli jest to żadnych dalszych *Niepoprawione* zmiennych typu istnieje, wnioskowanie o typie zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="1b6fe-622">Inny sposób, w przypadku wszystkich argumentów `Ei` przy użyciu odpowiedniego parametru typu `Ti` gdzie *danych wyjściowych typy* ([danych wyjściowych typy](expressions.md#output-types)) zawierają *Niepoprawione* typ zmiennych `Xj` ale *typów wejściowych* ([typów wejściowych](expressions.md#input-types)) w przeciwnym razie *danych wyjściowych wnioskowanie o typie* ([wniosków o typie danych wyjściowych ](expressions.md#output-type-inferences)) składa się *z* `Ei` *do* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="1b6fe-623">Następnie drugiej fazy jest powtarzany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="1b6fe-624">Typy wejściowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-624">Input types</span></span>

<span data-ttu-id="1b6fe-625">Jeśli `E` jest grupa metod lub niejawnie wpisanych funkcja anonimowa i `T` jest delegatem typu lub typ drzewa wyrażeń, a następnie wszystkie typy parametrów z `T` są *typów wejściowych* z `E` *z typem* `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="1b6fe-626">Typy danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-626">Output types</span></span>

<span data-ttu-id="1b6fe-627">Jeśli `E` jest grupa metod lub funkcją anonimową i `T` jest delegatem typu lub typ drzewa wyrażeń, a następnie zwracany typ `T` jest *wyjściowych typu* `E` *z typem*  `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="1b6fe-628">Zależność</span><span class="sxs-lookup"><span data-stu-id="1b6fe-628">Dependence</span></span>

<span data-ttu-id="1b6fe-629">*Niepoprawione* zmienna typu `Xi` *zależy bezpośrednio od* zmienną typu Niepoprawione `Xj` if dla niektórych argumentów `Ek` z typem `Tk` `Xj` występuje w *typu danych wejściowych* z `Ek` z typem `Tk` i `Xi` odbywa się w *typ danych wyjściowych* z `Ek` z typem `Tk`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="1b6fe-630">`Xj` *zależy od* `Xi` Jeśli `Xj` *zależy bezpośrednio od* `Xi` lub jeśli `Xi` *zależy bezpośrednio od* `Xk` i `Xk` *zależy od* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="1b6fe-631">Tak więc "jest zależny od" jest przechodnia, ale nie zwrotnej zamknięcia "zależy bezpośrednio od".</span><span class="sxs-lookup"><span data-stu-id="1b6fe-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="1b6fe-632">Wniosków o typie danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-632">Output type inferences</span></span>

<span data-ttu-id="1b6fe-633">*Danych wyjściowych wnioskowanie o typie* wykonano *z* wyrażenie `E` *do* typu `T` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="1b6fe-634">Jeśli `E` jest funkcją anonimową z wnioskowanym typem zwracanym `U` ([Inferred zwracany typ](expressions.md#inferred-return-type)) i `T` jest typu delegata lub typ drzewa wyrażeń z typem zwracanym `Tb`, następnie *wnioskowania dolna granica* ([wniosków dolna granica](expressions.md#lower-bound-inferences)) składa się *z* `U` *do* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="1b6fe-635">W przeciwnym razie, jeśli `E` jest grupa metod i `T` jest typu delegata lub typ drzewa wyrażeń z typami parametrów `T1...Tk` i zwracany typ `Tb`i rozpoznawanie przeciążenia `E` z typami `T1...Tk` daje pojedyncza metoda z typem zwracanym `U`, a następnie *wnioskowania dolna granica* składa się *z* `U` *do* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="1b6fe-636">W przeciwnym razie, jeśli `E` to wyrażenie z typem `U`, a następnie *wnioskowania dolna granica* składa się *z* `U` *do* `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="1b6fe-637">W przeciwnym razie są wprowadzane nie wniosków.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="1b6fe-638">Parametr jawny typ wniosków</span><span class="sxs-lookup"><span data-stu-id="1b6fe-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="1b6fe-639">*Wnioskowanie o typie parametru jawne* wykonano *z* wyrażenie `E` *do* typu `T` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="1b6fe-640">Jeśli `E` jest funkcją anonimową jawnie wpisanych z typami parametrów `U1...Uk` i `T` jest typu delegata lub typ drzewa wyrażeń z typami parametrów `V1...Vk` następnie dla każdego `Ui` *dokładnie wnioskowanie* ([dokładnie wniosków](expressions.md#exact-inferences)) składa się *z* `Ui` *do* odpowiednich `Vi`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="1b6fe-641">Dokładne wniosków</span><span class="sxs-lookup"><span data-stu-id="1b6fe-641">Exact inferences</span></span>

<span data-ttu-id="1b6fe-642">*Dokładnie wnioskowania* *z* typu `U` *do* typu `V` składa się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="1b6fe-643">Jeśli `V` jest jednym z *Niepoprawione* `Xi` następnie `U` zostanie dodany do zestawu dokładne granice dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="1b6fe-644">W przeciwnym wypadku ustawia `V1...Vk` i `U1...Uk` są określane przez sprawdzanie w przypadku spełnienia jednego z następujących przypadków:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="1b6fe-645">`V` jest typem tablicowym `V1[...]` i `U` jest typem tablicowym `U1[...]` o tej samej randze</span><span class="sxs-lookup"><span data-stu-id="1b6fe-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="1b6fe-646">`V` jest to typ `V1?` i `U` jest typem `U1?`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="1b6fe-647">`V` jest zbudowany typ `C<V1...Vk>`i `U` jest zbudowany typ `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="1b6fe-648">Przypadku spełnienia jednego z tych przypadków następnie *dokładnie wnioskowania* wykonano *z* każdego `Ui` *do* odpowiednich `Vi`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="1b6fe-649">W przeciwnym razie są wprowadzane nie wniosków.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="1b6fe-650">Dolna granica wniosków</span><span class="sxs-lookup"><span data-stu-id="1b6fe-650">Lower-bound inferences</span></span>

<span data-ttu-id="1b6fe-651">A *wnioskowania dolna granica* *z* typu `U` *do* typu `V` składa się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="1b6fe-652">Jeśli `V` jest jednym z *Niepoprawione* `Xi` następnie `U` zostanie dodany do zestawu dolne granice dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="1b6fe-653">W przeciwnym razie, jeśli `V` jest typem `V1?`i `U` jest typem `U1?` , a następnie wnioskowania dolna granica składa się z `U1` do `V1`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="1b6fe-654">W przeciwnym wypadku ustawia `U1...Uk` i `V1...Vk` są określane przez sprawdzanie w przypadku spełnienia jednego z następujących przypadków:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="1b6fe-655">`V` jest typem tablicowym `V1[...]` i `U` jest typem tablicowym `U1[...]` (lub parametrem typu, którego typ podstawowy skuteczne jest `U1[...]`) o tej samej randze</span><span class="sxs-lookup"><span data-stu-id="1b6fe-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="1b6fe-656">`V` Przykładem jest `IEnumerable<V1>`, `ICollection<V1>` lub `IList<V1>` i `U` jest typem tablicy jednowymiarowej `U1[]`(lub parametrem typu, którego typ podstawowy skuteczne jest `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="1b6fe-657">`V` skonstruowany typ klasy, struktury, interfejsu lub delegata `C<V1...Vk>` a typ unikatowego `C<U1...Uk>` tak, aby `U` (lub, jeśli `U` jest parametrem typu, klasą bazową skuteczne lub dowolny członek swój zestaw skuteczne interfejsu) jest taka sama jak, dziedziczy (bezpośrednio lub pośrednio) lub implementuje (bezpośrednio lub pośrednio) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="1b6fe-658">(Ograniczenie "unikatowości" oznacza, że w przypadku interfejsu `C<T> {} class U: C<X>, C<Y> {}`, a następnie wykonywane nie wnioskowania, gdy wnioskowanie z `U` do `C<T>` ponieważ `U1` może być `X` lub `Y`.)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="1b6fe-659">Przypadku spełnienia jednego z tych przypadków, a następnie następuje wnioskowanie *z* każdego `Ui` *do* odpowiednich `Vi` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="1b6fe-660">Jeśli `Ui` nie jest znany jako typ odwołania, a następnie *dokładnie wnioskowania* wykonano</span><span class="sxs-lookup"><span data-stu-id="1b6fe-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="1b6fe-661">W przeciwnym razie, jeśli `U` jest typem tablicy, a następnie *wnioskowania dolna granica* wykonano</span><span class="sxs-lookup"><span data-stu-id="1b6fe-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="1b6fe-662">W przeciwnym razie, jeśli `V` jest `C<V1...Vk>` , a następnie wnioskowania zależy od parametru typu i tym `C`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="1b6fe-663">Jeśli jest kowariantny, a następnie *wnioskowania dolna granica* wykonano.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="1b6fe-664">Jeśli jest kontrawariantny, a następnie *wnioskowania górną granicę* wykonano.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="1b6fe-665">Jeśli jest to niezmienna, a następnie *dokładnie wnioskowania* wykonano.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="1b6fe-666">W przeciwnym razie są wprowadzane nie wniosków.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="1b6fe-667">Górna granica wniosków</span><span class="sxs-lookup"><span data-stu-id="1b6fe-667">Upper-bound inferences</span></span>

<span data-ttu-id="1b6fe-668">*Wnioskowania górną granicę* *z* typu `U` *do* typu `V` składa się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="1b6fe-669">Jeśli `V` jest jednym z *Niepoprawione* `Xi` następnie `U` zostanie dodany do zestawu górną granicę dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="1b6fe-670">W przeciwnym wypadku ustawia `V1...Vk` i `U1...Uk` są określane przez sprawdzanie w przypadku spełnienia jednego z następujących przypadków:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="1b6fe-671">`U` jest typem tablicowym `U1[...]` i `V` jest typem tablicowym `V1[...]` o tej samej randze</span><span class="sxs-lookup"><span data-stu-id="1b6fe-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="1b6fe-672">`U` Przykładem jest `IEnumerable<Ue>`, `ICollection<Ue>` lub `IList<Ue>` i `V` jest typem tablicy jednowymiarowej `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="1b6fe-673">`U` jest to typ `U1?` i `V` jest typem `V1?`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="1b6fe-674">`U` jest zbudowany klasy, struktury, typ interfejsu lub delegata `C<U1...Uk>` i `V` to klasy, struktury, typ interfejsu lub delegata, która jest taka sama jak dziedziczy (bezpośrednio lub pośrednio) lub implementuje (bezpośrednio lub pośrednio) typ unikatowego `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="1b6fe-675">(Ograniczenie "unikatowości" oznacza, że jeśli będziemy mieć `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, a następnie wykonywane nie wnioskowania, gdy wnioskowanie z `C<U1>` do `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="1b6fe-676">Wniosków nie składają się z `U1` do jednej `X<Q>` lub `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="1b6fe-677">Przypadku spełnienia jednego z tych przypadków, a następnie następuje wnioskowanie *z* każdego `Ui` *do* odpowiednich `Vi` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="1b6fe-678">Jeśli `Ui` nie jest znany jako typ odwołania, a następnie *dokładnie wnioskowania* wykonano</span><span class="sxs-lookup"><span data-stu-id="1b6fe-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="1b6fe-679">W przeciwnym razie, jeśli `V` jest typem tablicy, a następnie *wnioskowania górną granicę* wykonano</span><span class="sxs-lookup"><span data-stu-id="1b6fe-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="1b6fe-680">W przeciwnym razie, jeśli `U` jest `C<U1...Uk>` , a następnie wnioskowania zależy od parametru typu i tym `C`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="1b6fe-681">Jeśli jest kowariantny, a następnie *wnioskowania górną granicę* wykonano.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="1b6fe-682">Jeśli jest kontrawariantny, a następnie *wnioskowania dolna granica* wykonano.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="1b6fe-683">Jeśli jest to niezmienna, a następnie *dokładnie wnioskowania* wykonano.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="1b6fe-684">W przeciwnym razie są wprowadzane nie wniosków.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="1b6fe-685">Naprawianie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-685">Fixing</span></span>

<span data-ttu-id="1b6fe-686">*Niepoprawione* zmienna typu `Xi` z zestawem granice jest *stałej* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="1b6fe-687">Zbiór *typy Release candidate* `Uj` rozpoczyna się jako zbiór wszystkich typów w zestawie granice dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="1b6fe-688">Każda granica sprawdzamy, następnie `Xi` z kolei: Dla każdej granicy dokładnie `U` z `Xi` wszystkich typów `Uj` które nie są identyczne z `U` są usuwane z zestawu Release candidate.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="1b6fe-689">Dla każdego dolna granica `U` z `Xi` wszystkich typów `Uj` do miejsca, które jest *nie* niejawna konwersja z `U` są usuwane z zestawu Release candidate.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="1b6fe-690">Dla każdego górną granicę `U` z `Xi` wszystkich typów `Uj` z tego miejsca, które jest *nie* niejawną konwersję do `U` są usuwane z zestawu Release candidate.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="1b6fe-691">Jeśli wśród pozostałych typów Release candidate `Uj` istnieje unikatowy typ `V` z której istnieje niejawna konwersja na wszystkich innych Release candidate typy, następnie `Xi` zostanie usunięty z `V`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="1b6fe-692">W przeciwnym razie wnioskowanie o typie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="1b6fe-693">Wnioskowany typ zwracany</span><span class="sxs-lookup"><span data-stu-id="1b6fe-693">Inferred return type</span></span>

<span data-ttu-id="1b6fe-694">Wywnioskowane zwracany typ funkcji anonimowych `F` jest używany podczas rozpoznawania typu wnioskowania i przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="1b6fe-695">Wnioskowany typ zwracany można określić tylko dla funkcja anonimowa, na którym wszystkich parametrów, które są znane typy, albo ponieważ one są określone jawnie, dostępne za pośrednictwem konwersję funkcja anonimowa lub wywnioskowane podczas wnioskowania o typie na otaczającego ogólny Wywołanie metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="1b6fe-696">***Wywnioskować typ wyniku*** jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="1b6fe-697">Jeśli treść `F` jest *wyrażenie* ma typ, a następnie typ wyniku wywnioskowane `F` jest typ tego wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="1b6fe-698">Jeśli treść `F` jest *bloku* i zestawu wyrażeń w bloku `return` instrukcji jest najlepszy typ typowe `T` ([znajdowanie najlepsze typ wspólny zestaw wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), następnie wnioskowany typ wyniku `F` jest `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="1b6fe-699">W przeciwnym razie nie można wywnioskować typu wyniku dla `F`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="1b6fe-700">***Wywnioskować zwracanego typu*** jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="1b6fe-701">Jeśli `F` jest asynchroniczne i treści `F` albo wyrażenie, sklasyfikowanych jako nic nie jest ([klasyfikacje wyrażenie](expressions.md#expression-classifications)), lub blok instrukcji, których nie instrukcjach return mają wyrażeń, wnioskowany typ zwracany jest `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="1b6fe-702">Jeśli `F` async i ma typ wyniku wywnioskowane `T`, wnioskowany typ zwracany jest `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="1b6fe-703">Jeśli `F` jest inne niż async i ma typ wyniku wywnioskowane `T`, wnioskowany typ zwracany jest `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="1b6fe-704">W przeciwnym razie nie można wywnioskować zwracanego typu dla `F`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="1b6fe-705">Jako przykład wnioskowanie o typie, obejmujące funkcje anonimowe należy wziąć pod uwagę `Select` — metoda rozszerzenia jest zadeklarowana w `System.Linq.Enumerable` klasy:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="1b6fe-706">Zakładając, że `System.Linq` przestrzeń nazw została zaimportowana z `using` klauzuli i biorąc klasy `Customer` z `Name` właściwości typu `string`, `Select` metoda może służyć do wybierz nazwy listę klientów:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="1b6fe-707">Wywołanie metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)) z `Select` jest przetwarzany, zapisując je ponownie wywołania do wywołania metody statycznej:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="1b6fe-708">Ponieważ argumenty typu nie zostały jawnie określone, wnioskowanie o typie umożliwia wywnioskować argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="1b6fe-709">Po pierwsze, `customers` argument jest powiązany z `source` parametru wnioskowanie `T` jako `Customer`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="1b6fe-710">Następnie przy użyciu funkcji anonimowych procesu wnioskowania opisanych powyżej, wpisz `c` jest danego typu `Customer`i wyrażenie `c.Name` jest powiązana z typem zwracanym `selector` parametru wnioskowanie `S` jako `string`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="1b6fe-711">W związku z tym wywołanie jest równoważne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="1b6fe-712">a wynik jest typu `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="1b6fe-713">W poniższym przykładzie pokazano sposób anonimowy typ funkcji wnioskowanie umożliwia informacje o typie "przepływ" między argumentów w wywołaniu metody rodzajowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="1b6fe-714">Podane metody:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="1b6fe-715">Wnioskowanie o typie wywołania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="1b6fe-716">przechodzi w następujący sposób: Po pierwsze, argument `"1:15:30"` jest powiązany z `value` parametru wnioskowanie `X` jako `string`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="1b6fe-717">Następnie, parametr pierwszy funkcja anonimowa `s`, otrzymuje wnioskowany typ `string`i wyrażenie `TimeSpan.Parse(s)` jest powiązana z typem zwracanym `f1`, wnioskowanie `Y` jako `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="1b6fe-718">Na koniec parametru Druga funkcja anonimowa `t`, otrzymuje wnioskowany typ `System.TimeSpan`i wyrażenie `t.TotalSeconds` jest powiązana z typem zwracanym `f2`, wnioskowanie `Z` jako `double`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="1b6fe-719">W efekcie jest wynik wywołania typu `double`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="1b6fe-720">Wnioskowanie o typie dla konwersji grupy metod</span><span class="sxs-lookup"><span data-stu-id="1b6fe-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="1b6fe-721">Podobnie jak wywołania metody rodzajowe, wnioskowanie o typie należy również będą stosowane podczas grupy metod `M` zawierający metodę ogólną jest konwertowany na typ danego obiektu delegowanego `D` ([metody konwersji grup](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="1b6fe-722">Biorąc pod uwagę — metoda</span><span class="sxs-lookup"><span data-stu-id="1b6fe-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="1b6fe-723">i grupy metod `M` przypisywanych do typu delegata `D` zadanie wnioskowanie o typie jest znalezienie argumentów typu `S1...Sn` tak, aby wyrażenie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="1b6fe-724">staje się niezgodne ([delegować deklaracje](delegates.md#delegate-declarations)) przy użyciu `D`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="1b6fe-725">W przeciwieństwie do algorytmu wnioskowania typu dla wywołań metody rodzajowej, w tym przypadku istnieją tylko argument *typy*, żaden argument *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="1b6fe-726">W szczególności, nie istnieją żadne funkcje anonimowe i dlatego nie ma potrzeby wielu faz wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="1b6fe-727">Zamiast tego wszystkie `Xi` są traktowane jako *Niepoprawione*i *wnioskowania dolna granica* wykonano *z* typ każdego argumentu `Uj` z `D` *do* odpowiedni typ parametru `Tj` z `M`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="1b6fe-728">Jeśli dla żadnego z `Xi` nie znaleziono żadnych granic, wnioskowanie o typie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="1b6fe-729">W przeciwnym razie wszystkie `Xi` są *stałej* do odpowiadającego `Si`, które są wynikiem wnioskowanie o typie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="1b6fe-730">Znajdowanie najlepszy typ wspólnego zestawu wyrażeń</span><span class="sxs-lookup"><span data-stu-id="1b6fe-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="1b6fe-731">W niektórych przypadkach wspólny typ musi wywnioskować zestawu wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="1b6fe-732">W szczególności typy elementu niejawnie wpisane tablice i funkcje anonimowe przy użyciu typów zwracanych *bloku* treści znajdują się w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="1b6fe-733">Intuicyjne, biorąc pod uwagę zestaw wyrażeń `E1...Em` to wnioskowania powinien być odpowiednikiem wywołania metody</span><span class="sxs-lookup"><span data-stu-id="1b6fe-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="1b6fe-734">za pomocą `Ei` jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="1b6fe-735">Bardziej precyzyjne wnioskowania rozpoczyna się od *Niepoprawione* zmienna typu `X`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="1b6fe-736">*Dane wyjściowe wniosków typu* są przeprowadzane *z* każdego `Ei` *do* `X`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="1b6fe-737">Na koniec `X` jest *stałej* i, jeśli to się powiedzie, wynikowy typ `S` jest wynikowy najlepszy typ wspólnych wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="1b6fe-738">Jeśli nie ma takiego `S` istnieje, wyrażenia mają nie najlepiej wspólnego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="1b6fe-739">Rozpoznanie przeciążenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-739">Overload resolution</span></span>

<span data-ttu-id="1b6fe-740">Rozpoznanie przeciążenia jest mechanizmem powiązania w czasie do wybierania najlepszych element członkowski funkcji do wywołania danej listy argumentów i zestaw elementów członkowskich funkcji Release candidate.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="1b6fe-741">Funkcja rozpoznawania przeciążeń wybiera element członkowski funkcji do wywołania w następujących kontekstów distinct w języku C#:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="1b6fe-742">Wywołanie metody o nazwie w *invocation_expression* ([wywołań metody opisywanego](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="1b6fe-743">Wywołanie konstruktora wystąpienia o nazwie w *object_creation_expression* ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="1b6fe-744">Wywołanie metody dostępu indeksatora za pośrednictwem *element_access* ([dostępu do elementu](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="1b6fe-745">Wywołanie operatora wstępnie zdefiniowanych lub zdefiniowanych przez użytkownika, do którego odwołuje się wyrażenie ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="1b6fe-746">Każda z tych kontekstach definiuje zestaw elementów członkowskich z funkcją Release candidate i listy argumentów w własny unikatowy sposób opisanych szczegółowo w sekcji wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="1b6fe-747">Na przykład zestaw kandydatami do wywołania metody nie ma metody oznaczone `override` ([wyszukanie członka](expressions.md#member-lookup)), i metody w klasie bazowej nie są obiektami, jeśli dotyczy dowolną metodę w klasie pochodnej ([ Wywołań metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="1b6fe-748">Po zidentyfikowaniu składowe funkcji Release candidate i listą argumentów, wybór najlepszych funkcja składowa jest taka sama we wszystkich przypadkach:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="1b6fe-749">Biorąc pod uwagę zestaw elementów członkowskich funkcji odpowiednie Release candidate, najlepszych funkcji elementu członkowskiego, znajduje się zestaw.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="1b6fe-750">Jeśli zestaw zawiera tylko jeden element członkowski funkcji, ten element członkowski funkcji jest najlepsze element członkowski funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="1b6fe-751">W przeciwnym razie najlepszą funkcja składowa jest składowej jedną funkcję, która jest lepsze niż inne składowe funkcji w odniesieniu do listy podany argument, pod warunkiem, że każda funkcja składowa jest porównywana do wszystkich innych funkcji elementów członkowskich przy użyciu reguł w [ Element członkowski funkcji lepsze](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="1b6fe-752">Jeśli nie istnieje dokładnie jednego członka funkcji, który jest lepsze niż wszystkie inne elementy członkowskie funkcji, następnie wywołanie funkcji elementu członkowskiego jest niejednoznaczny i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-753">Następujące sekcje definiują dokładny znaczenia warunków ***dotyczy funkcja składowa*** i ***lepsza funkcja składowa***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="1b6fe-754">Dotyczy funkcji składowej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-754">Applicable function member</span></span>

<span data-ttu-id="1b6fe-755">Funkcja składowa jest nazywany ***dotyczy funkcja składowa*** w odniesieniu do listy argumentów `A` gdy są spełnione wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="1b6fe-756">Każdy argument `A` odnosi się do parametru w deklaracji funkcji elementu członkowskiego, zgodnie z opisem w [parametry odpowiadające](expressions.md#corresponding-parameters), i każdego parametru, do którego odnosi się żaden argument nie jest parametrem opcjonalnym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="1b6fe-757">Dla każdego argumentu w `A`, przekazywanie tryb argumentu parametru (czyli wartość `ref`, lub `out`) jest taka sama jak trybu przekazywania parametru odpowiadającego mu parametru i</span><span class="sxs-lookup"><span data-stu-id="1b6fe-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="1b6fe-758">dla parametru wartości lub tablicą parametrów niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z argumentem typu odpowiadającego mu parametru lub</span><span class="sxs-lookup"><span data-stu-id="1b6fe-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="1b6fe-759">Aby uzyskać `ref` lub `out` parametr, typ argumentu jest taka sama jak typu odpowiadającego mu parametru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="1b6fe-760">Gdy wszystkie `ref` lub `out` parametru jest aliasem dla przekazanego argumentu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="1b6fe-761">Funkcja elementu członkowskiego, obejmującą tablicy parametrów Jeśli funkcja składowa ma zastosowanie powyższych reguł, jest ono obowiązywać w jego ***normalny formularz***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="1b6fe-762">Jeśli funkcja składowa, która obejmuje tablicy parametrów nie ma zastosowania w postaci normalne, funkcja składowa zamiast tego można stosować w jego ***rozwinięte formularza***:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="1b6fe-763">Rozwinięty formularza jest tworzony przez zastąpienie tablicy parametrów w deklaracji funkcji elementu członkowskiego o wartości zero lub większą liczbę parametrów wartość parametru typu elementu tablicy, takie tego liczba argumentów w liście argumentów `A` pasuje do sumy Liczba parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="1b6fe-764">Jeśli `A` ma mniej argumentów niż liczba parametrów stałych w deklaracji funkcji elementu członkowskiego, rozwinięty formularza funkcja składowa nie można utworzyć i dlatego nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="1b6fe-765">W przeciwnym razie jest odpowiednie, jeśli dla każdego argumentu w rozszerzonej postaci `A` trybu przekazywania parametru argumentu jest taka sama jak trybu przekazywania parametru odpowiadającego mu parametru i</span><span class="sxs-lookup"><span data-stu-id="1b6fe-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="1b6fe-766">dla parametru wartości stałej lub utworzone przez rozszerzenie niejawna konwersja parametru wartości ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typem argumentu typu odpowiadającego mu parametru lub</span><span class="sxs-lookup"><span data-stu-id="1b6fe-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="1b6fe-767">Aby uzyskać `ref` lub `out` parametr, typ argumentu jest taka sama jak typu odpowiadającego mu parametru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="1b6fe-768">Lepsza funkcja składowa</span><span class="sxs-lookup"><span data-stu-id="1b6fe-768">Better function member</span></span>

<span data-ttu-id="1b6fe-769">Na potrzeby określania, Lepsza funkcja składowa listy argumentów stripped-down A jest tworzona, zawierający tylko argument wyrażenia samodzielnie w kolejności, w jakiej znajdują się na oryginalnej liście argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="1b6fe-770">Listy parametrów dla każdej z funkcji Release candidate, której członkami są skonstruowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="1b6fe-771">Rozwinięty formularza jest używana, jeśli element członkowski funkcji było stosowane tylko w formularzu rozwinięty.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="1b6fe-772">Parametry opcjonalne bez odpowiedniego argumentów są usuwane z listy parametrów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="1b6fe-773">Parametry zostaną ponownie uporządkowane, tak że występuje ono w tym samym położeniu jako odpowiadający argument na liście argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="1b6fe-774">Podanej listy argumentów `A` z zestawem wyrażenia argumentu `{E1, E2, ..., En}` i dwie składowe dotyczy funkcji `Mp` i `Mq` z typami parametrów `{P1, P2, ..., Pn}` i `{Q1, Q2, ..., Qn}`, `Mp` jest definiowany jako ***lepsza funkcja składowa*** niż `Mq` Jeśli</span><span class="sxs-lookup"><span data-stu-id="1b6fe-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="1b6fe-775">dla każdego argumentu niejawna konwersja z `Ex` do `Qx` nie jest lepsze niż niejawna konwersja z `Ex` do `Px`, i</span><span class="sxs-lookup"><span data-stu-id="1b6fe-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="1b6fe-776">co najmniej jednego argumentu, konwersja z `Ex` do `Px` jest lepsze niż konwersja z `Ex` do `Qx`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="1b6fe-777">Podczas przeprowadzania oceny, w tym przypadku `Mp` lub `Mq` jest następnie stosowane w postaci rozwiniętej `Px` lub `Qx` odwołuje się do parametru w postaci rozwiniętej listy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="1b6fe-778">W przypadku, gdy parametr typu sekwencje `{P1, P2, ..., Pn}` i `{Q1, Q2, ..., Qn}` są równoważne (czyli każdy `Pi` ma konwersję tożsamości do odpowiednich `Qi`), stosowane są następujące reguły podziału tie w kolejności, w celu ustalenia, tym lepiej element członkowski funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="1b6fe-779">Jeśli `Mp` jest metodą nieogólnego i `Mq` jest metody rodzajowej, `Mp` jest lepsze niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="1b6fe-780">W przeciwnym razie, jeśli `Mp` ma zastosowanie w postaci normalne i `Mq` ma `params` macierz, a następnie występuje tylko w postaci rozwiniętej następnie `Mp` jest lepsze niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="1b6fe-781">W przeciwnym razie, jeśli `Mp` zadeklarował więcej parametrów niż `Mq`, następnie `Mp` jest lepsze niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="1b6fe-782">Taka sytuacja może wystąpić, jeśli obie metody mają `params` tablic oraz czy ma to zastosowanie tylko w rozwiniętym formularzy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="1b6fe-783">W przeciwnym razie jeśli wszystkie parametry `Mp` mają odpowiedni argument argumenty domyślne powinny być zastępowane dla co najmniej jeden parametr opcjonalny w `Mq` następnie `Mp` jest lepsze niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="1b6fe-784">W przeciwnym razie, jeśli `Mp` zawiera bardziej szczegółowe typy parametrów niż `Mq`, następnie `Mp` jest lepsze niż `Mq`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="1b6fe-785">Pozwól `{R1, R2, ..., Rn}` i `{S1, S2, ..., Sn}` reprezentują typy parametrów bez wystąpień i nierozwiniętymi `Mp` i `Mq`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="1b6fe-786">`Mp`firmy typy parametrów są bardziej szczegółowe niż `Mq`firmy if, dla każdego parametru `Rx` nie jest mniej określonych niż `Sx`, a w przypadku co najmniej jeden parametr `Rx` jest bardziej szczegółowe niż `Sx`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="1b6fe-787">Parametr typu jest mniej określonych niż parametru bez typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="1b6fe-788">Cyklicznie, skonstruowanego typu jest bardziej szczegółowe niż innym typem skonstruowany (z tą samą liczbą argumentów typu), jeśli co najmniej jeden argument typu jest bardziej szczegółowe, a nie argument typu jest mniej określonych niż argument Typ w innym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="1b6fe-789">Typ tablicy jest bardziej szczegółowe niż innego typu tablicy (z taką samą liczbę wymiarów), jeśli typ elementu w pierwszym jest bardziej szczegółowe niż typ elementu w drugim.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="1b6fe-790">W przeciwnym razie jeśli jeden element członkowski jest operator niepodniesione, a drugi to podniesionym operatora, lepiej jest jeden niepodniesione.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="1b6fe-791">W przeciwnym wypadku żadna funkcja składowa jest lepiej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="1b6fe-792">Lepsze Konwersja wyrażenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-792">Better conversion from expression</span></span>

<span data-ttu-id="1b6fe-793">Biorąc pod uwagę niejawną konwersję `C1` konwertuje wyrażenie `E` do typu `T1`i niejawną konwersję `C2` konwertuje wyrażenie `E` do typu `T2`, `C1` jest ***lepsze konwersji*** niż `C2` Jeśli `E` nie jest dokładnie dopasowana `T2` i zawiera co najmniej jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="1b6fe-794">`E` dokładnie odpowiada `T1` ([dokładnie wyrażeniu dopasowania](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="1b6fe-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="1b6fe-795">`T1` jest ona lokalizacją docelową konwersji lepsze niż `T2` ([lepsze docelowy konwersji](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="1b6fe-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="1b6fe-796">Dokładnie pasujące wyrażenie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-796">Exactly matching Expression</span></span>

<span data-ttu-id="1b6fe-797">Podane wyrażenie `E` i typ `T`, `E` dokładnie pasuje `T` jeśli posiada jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="1b6fe-798">`E` zawiera typ `S`, i istnieje konwersja tożsamości z `S` do `T`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="1b6fe-799">`E` jest funkcją anonimową `T` jest typem delegowanym `D` lub typ drzewa wyrażeń `Expression<D>` i zawiera jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="1b6fe-800">Wnioskowany typ zwracany `X` istnieje `E` w kontekście listy parametrów `D` ([Inferred zwracany typ](expressions.md#inferred-return-type)), i istnieje konwersja tożsamości z `X` do zwracanego typu `D`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="1b6fe-801">Albo `E` jest inne niż async i `D` ma typ zwracany `Y` lub `E` jest asynchroniczne i `D` ma typ zwracany `Task<Y>`, i zawiera jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="1b6fe-802">Treść `E` jest wyrażeniem, który dokładnie pasuje `Y`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="1b6fe-803">Treść `E` jest blok instrukcji, w którym każdej instrukcji return zwraca wyrażenie który dokładnie pasuje `Y`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="1b6fe-804">Lepsze docelowy konwersji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-804">Better conversion target</span></span>

<span data-ttu-id="1b6fe-805">Biorąc pod uwagę dwa różne typy `T1` i `T2`, `T1` jest ona lokalizacją docelową konwersji lepsze niż `T2` Jeśli niejawna konwersja z `T2` do `T1` istnieje, i zawiera co najmniej jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="1b6fe-806">Niejawna konwersja z `T1` do `T2` istnieje</span><span class="sxs-lookup"><span data-stu-id="1b6fe-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="1b6fe-807">`T1` jest to typ delegowany `D1` lub typ drzewa wyrażeń `Expression<D1>`, `T2` jest typem delegowanym `D2` lub typ drzewa wyrażeń `Expression<D2>`, `D1` ma typ zwracany `S1` i jedna z przechowuje następujące:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="1b6fe-808">`D2` Zwraca wartość void</span><span class="sxs-lookup"><span data-stu-id="1b6fe-808">`D2` is void returning</span></span>
   * <span data-ttu-id="1b6fe-809">`D2` ma typ zwracany `S2`, i `S1` jest ona lokalizacją docelową konwersji lepsze niż `S2`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="1b6fe-810">`T1` jest `Task<S1>`, `T2` jest `Task<S2>`, i `S1` jest ona lokalizacją docelową konwersji lepsze niż `S2`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="1b6fe-811">`T1` jest `S1` lub `S1?` gdzie `S1` jest typ całkowity ze znakiem i `T2` jest `S2` lub `S2?` gdzie `S2` jest nieoznaczoną liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="1b6fe-812">W szczególności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-812">Specifically:</span></span>
   * <span data-ttu-id="1b6fe-813">`S1` jest `sbyte` i `S2` jest `byte`, `ushort`, `uint`, lub `ulong`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="1b6fe-814">`S1` jest `short` i `S2` jest `ushort`, `uint`, lub `ulong`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="1b6fe-815">`S1` jest `int` i `S2` jest `uint`, lub `ulong`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="1b6fe-816">`S1` jest `long` i `S2` jest `ulong`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="1b6fe-817">Przeciążenie klas ogólnych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-817">Overloading in generic classes</span></span>

<span data-ttu-id="1b6fe-818">Chociaż podpisów, ponieważ nie zadeklarowano muszą być unikatowe, jest to możliwe, że identycznych podpisach powoduje zastąpienie argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="1b6fe-819">W takich przypadkach reguły podziału tie przeciążonym powyżej wybierze najbardziej określonym elemencie członkowskim.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="1b6fe-820">W poniższych przykładach pokazano przeciążenia, które są prawidłowe i nieprawidłowa przy uwzględnieniu tej reguły:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="1b6fe-821">Sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="1b6fe-822">Najbardziej dynamicznie powiązanych operacji ewentualnymi kandydatami do rozpoznawania zestawu jest nieznany w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="1b6fe-823">W niektórych przypadkach jednak zestaw Release candidate jest znany w czasie kompilacji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="1b6fe-824">Wywołania metody statycznej z argumentów dynamicznych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="1b6fe-825">Wystąpienie wywołania metody, której odbiornik nie jest wyrażeniem dynamiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="1b6fe-826">Wywołania indeksatora, której odbiornik nie jest wyrażeniem dynamiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="1b6fe-827">Konstruktor wywołuje z argumentów dynamicznych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="1b6fe-828">W takich przypadkach ograniczone sprawdzanie w czasie kompilacji jest wykonywane dla każdego Release candidate zobaczyć, jeśli któryś z nich prawdopodobnie można zastosować w czasie wykonywania. To sprawdzenie składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-829">Wnioskowanie o typie częściowym: Dowolny typ argumentu, który nie zależy od bezpośrednio lub pośrednio argumentu typu `dynamic` wynika, używając reguł [wnioskowanie o typie](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="1b6fe-830">Pozostałe argumenty typu są nieznane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="1b6fe-831">Sprawdzanie możliwości zastosowania częściowej: Zastosowanie zaznaczono zgodnie z opisem w [dotyczy funkcja składowa](expressions.md#applicable-function-member), ale ignoruje parametry, których typ jest nieznany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="1b6fe-832">Jeśli żaden kandydat zakończy się pomyślnie tego testu, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="1b6fe-833">Wywołania elementu — funkcja</span><span class="sxs-lookup"><span data-stu-id="1b6fe-833">Function member invocation</span></span>

<span data-ttu-id="1b6fe-834">W tej sekcji opisano proces, który ma miejsce w czasie wykonywania można zainicjować członka określonej funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="1b6fe-835">Zakłada się, proces wiązania czas stwierdził już określony element członkowski do wywołania, prawdopodobnie stosując przeciążenia, aby zestaw elementów członkowskich funkcji kandydata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="1b6fe-836">Do celów opisu proces wywołania funkcji elementów członkowskich są podzielone na dwie kategorie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="1b6fe-837">Funkcja statyczne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-837">Static function members.</span></span> <span data-ttu-id="1b6fe-838">Są to konstruktory wystąpień, metod statycznych, metod dostępu do właściwości statycznych i operatory zdefiniowane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="1b6fe-839">Funkcja statyczne elementy członkowskie są zawsze niewirtualną.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="1b6fe-840">Elementy członkowskie wystąpień funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-840">Instance function members.</span></span> <span data-ttu-id="1b6fe-841">Są metody wystąpienia, wystąpienia metod dostępu do właściwości i metod dostępu do indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="1b6fe-842">Elementy członkowskie wystąpień funkcji to-virtual lub wirtualnych i zawsze są wywoływane na konkretnym wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="1b6fe-843">Wystąpienie jest obliczana przez wyrażenie wystąpienia i staje się dostępny w ramach funkcji elementu członkowskiego jako `this` ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="1b6fe-844">Przetwarzanie czasu wykonywania wywołania funkcji elementu członkowskiego składa się z następujących czynności, gdzie `M` jest funkcja składowa i, jeśli `M` jest składową wystąpienia `E` jest wyrażeniem wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="1b6fe-845">Jeśli `M` jest składową statyczną funkcję:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="1b6fe-846">Lista argumentów jest obliczane zgodnie z opisem w [listy argumentów](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="1b6fe-847">`M` jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-847">`M` is invoked.</span></span>

*  <span data-ttu-id="1b6fe-848">Jeśli `M` jest to funkcja składowa zadeklarowana w *value_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="1b6fe-849">`E` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-849">`E` is evaluated.</span></span> <span data-ttu-id="1b6fe-850">Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="1b6fe-851">Jeśli `E` nie jest sklasyfikowany jako zmienną, a następnie tymczasowej zmiennej lokalnej `E`firmy tworzony jest typ i wartość `E` jest przypisany do tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="1b6fe-852">`E` następnie jest sklasyfikowany jako odwołanie do tego tymczasowej zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="1b6fe-853">Zmienna tymczasowa jest dostępny jako `this` w ramach `M`, ale nie w jakikolwiek inny sposób.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="1b6fe-854">W związku z tym, tylko wtedy, gdy `E` jest wartość true, zmienna jest możliwe do obiektu wywołującego obserwować zmiany, `M` sprawia, że aby `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="1b6fe-855">Lista argumentów jest obliczane zgodnie z opisem w [listy argumentów](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="1b6fe-856">`M` jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-856">`M` is invoked.</span></span> <span data-ttu-id="1b6fe-857">Zmienna odwołuje się `E` staje się zmienna odwołuje się `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="1b6fe-858">Jeśli `M` jest to funkcja składowa zadeklarowana w *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="1b6fe-859">`E` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-859">`E` is evaluated.</span></span> <span data-ttu-id="1b6fe-860">Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="1b6fe-861">Lista argumentów jest obliczane zgodnie z opisem w [listy argumentów](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="1b6fe-862">Jeśli typ `E` jest *value_type*, konwersja boxing ([konwersje Boxing](types.md#boxing-conversions)) jest wykonywane w celu przekonwertowania `E` na typ `object`, i `E` jest uznawany za Typ `object` w poniższych krokach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="1b6fe-863">W tym przypadku `M` może być tylko składową z `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="1b6fe-864">Wartość `E` jest sprawdzany w celu był prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="1b6fe-865">Jeśli wartość `E` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="1b6fe-866">Implementacja elementu członkowskiego funkcji do wywołania, jest określana:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="1b6fe-867">Jeśli typ powiązania — czas `E` jest interfejsem, element członkowski funkcji do wywołania to implementacja `M` dostarczone przez typu run-time wystąpienia odwołuje się `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="1b6fe-868">Ta funkcja składowa jest określana przez stosowanie reguł mapowania interfejsu ([mapowania interfejsu](interfaces.md#interface-mapping)) do określenia implementacji `M` dostarczone przez typu run-time wystąpienia odwołuje się `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="1b6fe-869">W przeciwnym razie, jeśli `M` jest elementem członkowskim funkcji wirtualnej element członkowski funkcji do wywołania to implementacja `M` dostarczone przez typu run-time wystąpienia odwołuje się `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="1b6fe-870">Ten element członkowski funkcji zależy od stosowania reguł do określania wykonania najbardziej pochodnej ([metody wirtualnej](classes.md#virtual-methods)) z `M` względem typu run-time wystąpienia odwołuje się `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="1b6fe-871">W przeciwnym razie `M` jest elementem członkowskim niewirtualną i jest element członkowski funkcji do wywołania `M` sam.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="1b6fe-872">Implementacja elementu członkowskiego funkcji, które są określone w poprzednim kroku jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="1b6fe-873">Zawiera odwołanie do obiektu `E` staje się zawiera odwołanie do obiektu `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="1b6fe-874">Wywołania na wystąpieniach ramce</span><span class="sxs-lookup"><span data-stu-id="1b6fe-874">Invocations on boxed instances</span></span>

<span data-ttu-id="1b6fe-875">Funkcja składowa zaimplementowane w *value_type* może być wywoływany za pośrednictwem spakowany wystąpienie tego *value_type* w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="1b6fe-876">Gdy funkcja składowa jest `override` metody dziedziczone z typu `object` i jest wywoływana za pomocą wyrażenia typu `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="1b6fe-877">Gdy funkcja składowa jest implementacją funkcja składowa interfejsu i jest wywoływana za pomocą wyrażenia wystąpienia *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="1b6fe-878">Gdy funkcja składowa zostanie wywołana przez delegata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="1b6fe-879">W takich przypadkach wystąpienie programu prostokątnych jest uważany za zawiera zmienną *value_type*, a ta zmienna staje się zmienna odwołuje się `this` w ramach wywołanie funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="1b6fe-880">W szczególności oznacza to, że gdy funkcja składowa zostanie wywołana w wystąpieniu spakowany, możliwe jest funkcja elementu członkowskiego do modyfikowania wartości zawarte w wystąpieniu programu prostokątnych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="1b6fe-881">Wyrażenia podstawowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-881">Primary expressions</span></span>

<span data-ttu-id="1b6fe-882">Wyrażenia podstawowe obejmują najprostszej formy wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="1b6fe-883">Wyrażenia podstawowe są dzielone między *array_creation_expression*s i *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="1b6fe-884">Traktowanie wyrażenie tworzenia tablicy w ten sposób, zamiast dołączając ją wraz z innych form proste wyrażenie umożliwia gramatykę na potrzeby nie zezwalaj na kod może być mylące, takich jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="1b6fe-885">które w przeciwnym razie może być interpretowany jako</span><span class="sxs-lookup"><span data-stu-id="1b6fe-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="1b6fe-886">Literały</span><span class="sxs-lookup"><span data-stu-id="1b6fe-886">Literals</span></span>

<span data-ttu-id="1b6fe-887">A *primary_expression* składający się z *literału* ([literały](lexical-structure.md#literals)) jest klasyfikowana jako wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="1b6fe-888">Ciągi interpolowane</span><span class="sxs-lookup"><span data-stu-id="1b6fe-888">Interpolated strings</span></span>

<span data-ttu-id="1b6fe-889">*Interpolated_string_expression* składa się z `$` logowanie następuje regularnie lub verbatim literału ciągu, na którym dziury rozdzielone `{` i `}`, należy ująć wyrażenia i formatowanie wymagania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="1b6fe-890">Wyrażenie ciągu interpolowanego jest wynikiem *interpolated_string_literal* , ma zostały podzielone na oddzielne tokeny, zgodnie z opisem w [interpolowane literałów ciągów](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="1b6fe-891">*Constant_expression* interpolacji musi mieć niejawną konwersję do `int`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="1b6fe-892">*Interpolated_string_expression* zostanie sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="1b6fe-893">Jeśli od razu jest konwertowany na `System.IFormattable` lub `System.FormattableString` za pomocą konwersji niejawnych ciąg interpolowany ([niejawne interpolowane ciąg konwersje](conversions.md#implicit-interpolated-string-conversions)), wyrażenie ciągu interpolowanego ma tego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="1b6fe-894">W przeciwnym razie ma typ `string`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="1b6fe-895">Jeśli typ ciągu interpolowanego `System.IFormattable` lub `System.FormattableString`, znaczenie jest wywołaniem `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="1b6fe-896">Jeśli typ jest `string`, znaczenie wyrażenia jest wywołaniem `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="1b6fe-897">W obu przypadkach liście argumentów wywołania składa się z literału przy użyciu symboli zastępczych dla każdego interpolacji i argument każde wyrażenie odpowiadające posiadaczy miejsce ciągu formatu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="1b6fe-898">Literał ciągu formatu jest tworzony następująco, gdzie `N` jest liczba interpolations w *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="1b6fe-899">Jeśli *interpolated_regular_string_whole* lub *interpolated_verbatim_string_whole* następuje `$` Zaloguj się, a następnie literał ciągu formatu jest tego tokenu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="1b6fe-900">W przeciwnym razie literału ciągu formatu składa się z:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="1b6fe-901">Pierwszy *interpolated_regular_string_start* lub *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="1b6fe-902">Następnie dla każdego numeru `I` z `0` do `N-1`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="1b6fe-903">W formie dziesiętnej `I`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="1b6fe-904">Następnie, jeśli odpowiedni *interpolacji* ma *constant_expression*, `,` (przecinek), a następnie dziesiętna reprezentacja wartości *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="1b6fe-905">A następnie *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* lub *interpolated_ verbatim_string_end* natychmiast po odpowiednich interpolacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="1b6fe-906">Pozostałe argumenty są po prostu *wyrażeń* z *interpolations* (jeśli istnieje), w kolejności.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="1b6fe-907">TODO: przykłady.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="1b6fe-908">Proste nazwy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-908">Simple names</span></span>

<span data-ttu-id="1b6fe-909">A *simple_name* składa się z identyfikatorem, opcjonalnie następuje lista argumentów typu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="1b6fe-910">A *simple_name* jest formularza `I` lub w postaci `I<A1,...,Ak>`, gdzie `I` jest pojedynczy identyfikator i `<A1,...,Ak>` to opcjonalna *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="1b6fe-911">Gdy nie *type_argument_list* jest określony, należy wziąć pod uwagę `K` zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="1b6fe-912">*Simple_name* jest obliczane i sklasyfikowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="1b6fe-913">Jeśli `K` jest równa zero i *simple_name* pojawia się w obrębie *bloku* i, jeśli *bloku*firmy (lub otaczającego *bloku*firmy) lokalne Deklaracja zmiennej miejsca ([deklaracje](basic-concepts.md#declarations)) zawiera zmienną lokalną, parametr lub stałą o nazwie `I`, a następnie *simple_name* odwołuje się do tej zmiennej lokalnej parametr lub stała i jest klasyfikowana jako wartości lub zmienne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="1b6fe-914">Jeśli `K` jest równa zero i *simple_name* pojawia się w treści deklaracji metody rodzajowe i jeśli deklaracja zawiera parametr typu o nazwie `I`, a następnie *simple_name*odwołuje się do tego parametru typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="1b6fe-915">W przeciwnym razie dla każdego typu wystąpienia `T` ([typu wystąpienia](classes.md#the-instance-type)), począwszy od typu wystąpienia natychmiastowo otaczającą deklaracji typu i kontynuowanie przy użyciu typu wystąpienia każdej otaczającej klasie lub strukturze Deklaracja (jeśli istnieje):</span><span class="sxs-lookup"><span data-stu-id="1b6fe-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="1b6fe-916">Jeśli `K` to zero, a deklaracja `T` obejmuje parametru typu o nazwie `I`, a następnie *simple_name* odwołuje się do tego parametru typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="1b6fe-917">W przeciwnym razie, jeśli wyszukiwanie elementu członkowskiego ([wyszukanie członka](expressions.md#member-lookup)) z `I` w `T` z `K`  argumentami typu tworzy dopasowania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="1b6fe-918">Jeśli `T` jest typem wystąpienia natychmiastowo otaczającą typu klasy lub struktury i wyszukiwanie identyfikuje jeden lub więcej metod, wynik jest grupą metoda z wyrażeniem skojarzonego wystąpienia `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="1b6fe-919">Jeśli lista argumentów typu został określony, jest używany podczas wywoływania metody ogólnej ([wywołań metody opisywanego](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="1b6fe-920">W przeciwnym razie, jeśli `T` jest typu wystąpienia natychmiastowo otaczającą typu klasy lub struktury, wyszukiwanie identyfikuje elementu członkowskiego wystąpienia, a odwołanie występuje w treści konstruktora wystąpienia, metoda wystąpienia lub metodę dostępu wystąpienia wynik jest taki sam dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `this.I`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="1b6fe-921">To tylko możliwe, gdy `K` wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="1b6fe-922">W przeciwnym razie wynikiem jest taka sama jak dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `T.I` lub `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="1b6fe-923">W tym przypadku jest to błąd czasu powiązania dla *simple_name* do odwoływania się do elementu członkowskiego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="1b6fe-924">W przeciwnym razie dla każdej przestrzeni nazw `N`, począwszy od przestrzeni nazw, w którym *simple_name* problem wystąpi, kontynuując każdego otaczającej przestrzeni nazw (jeśli istnieje), a skończywszy globalnej przestrzeni nazw, poniższe kroki to etapy sprawdzane, dopóki nie znajduje się jednostka:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="1b6fe-925">Jeśli `K` jest równa zero i `I` jest nazwą przestrzeni nazw w `N`, następnie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="1b6fe-926">Jeśli lokalizacja gdzie *simple_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N` i zawiera deklarację przestrzeni nazw *extern_alias_directive* lub  *using_alias_directive* który kojarzy nazwę `I` z przestrzeni nazw lub typ, a następnie *simple_name* jest niejednoznaczny i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="1b6fe-927">W przeciwnym razie *simple_name* odwołuje się do przestrzeni nazw o nazwie `I` w `N`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="1b6fe-928">W przeciwnym razie, jeśli `N` zawiera dostępny typ o nazwie `I` i `K`  parametry typu, następnie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="1b6fe-929">Jeśli `K` to zero i lokalizacja gdzie *simple_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N` i zawiera deklarację przestrzeni nazw *extern_alias_directive*lub *using_alias_directive* który kojarzy nazwę `I` z przestrzeni nazw lub typ, a następnie *simple_name* jest niejednoznaczny i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="1b6fe-930">W przeciwnym razie *namespace_or_type_name* odwołuje się do typu skonstruowany przy użyciu argumentów danego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="1b6fe-931">W przeciwnym razie, jeśli lokalizacja gdzie *simple_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="1b6fe-932">Jeśli `K` jest równa zero i zawiera deklarację przestrzeni nazw *extern_alias_directive* lub *using_alias_directive* który kojarzy nazwę `I` z zaimportowaną przestrzenią nazw lub Typ, a następnie *simple_name* odwołuje się do tej przestrzeni nazw lub typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="1b6fe-933">W przeciwnym razie, jeśli deklaracji przestrzeni nazw i typ zaimportowany przez *using_namespace_directive*s i *using_static_directive*s deklarację przestrzeni nazw zawierać dokładnie jeden typ dostępny lub -extension członka statycznego o nazwie `I` i `K`  parametry typu, a następnie *simple_name* odwołuje się do tego typu lub elementu członkowskiego, skonstruowany przy użyciu argumentów danego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="1b6fe-934">W przeciwnym razie, jeśli obszary nazw i typy zaimportowany przez *using_namespace_directive*s deklarację przestrzeni nazw zawiera więcej niż jeden dostępny typ lub metoda bez rozszerzenia członka statycznego o nazwie `I` i `K`  parametry typu, a następnie *simple_name* jest niejednoznaczny i występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="1b6fe-935">Należy pamiętać, że ten krok jest dokładnie zbliżony do odpowiedniego krok przetwarzania w *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="1b6fe-936">W przeciwnym razie *simple_name* jest niezdefiniowana, i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="1b6fe-937">Wyrażenia ujętego w nawiasy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-937">Parenthesized expressions</span></span>

<span data-ttu-id="1b6fe-938">A *parenthesized_expression* składa się z *wyrażenie* w nawiasach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="1b6fe-939">A *parenthesized_expression* jest oceniany przez dokonanie oceny oprogramowania *wyrażenie* w nawiasach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="1b6fe-940">Jeśli *wyrażenie* między nawiasami wskazuje przestrzeń nazw lub typ, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="1b6fe-941">W przeciwnym razie wynik *parenthesized_expression* wynika z oceny zamkniętego *wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="1b6fe-942">Dostęp do elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="1b6fe-942">Member access</span></span>

<span data-ttu-id="1b6fe-943">A *member_access* składa się z *primary_expression*, *predefined_type*, lub *qualified_alias_member*, a następnie "`.`"token, a następnie *identyfikator*, opcjonalnie następuje *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="1b6fe-944">*Qualified_alias_member* produkcji jest zdefiniowany w [kwalifikatory alias Namespace](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="1b6fe-945">A *member_access* jest formularza `E.I` lub w postaci `E.I<A1, ..., Ak>`, gdzie `E` jest wyrażeniem podstawowym, `I` jest pojedynczy identyfikator i `<A1, ..., Ak>` to opcjonalna  *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="1b6fe-946">Gdy nie *type_argument_list* jest określony, należy wziąć pod uwagę `K` zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="1b6fe-947">A *member_access* z *primary_expression* typu `dynamic` dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-948">W tym przypadku kompilator klasyfikuje dostęp do elementu członkowskiego jako dostęp do właściwości typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="1b6fe-949">Reguły poniżej, aby określić znaczenia *member_access* zostaną następnie zastosowane w czasie wykonywania, zamiast typ kompilacji przy użyciu typu run-time *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="1b6fe-950">Jeśli ta klasyfikacja środowiska wykonawczego prowadzi do grupy metod, a następnie dostęp do elementu członkowskiego musi być *primary_expression* z *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="1b6fe-951">*Member_access* jest obliczane i sklasyfikowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="1b6fe-952">Jeśli `K` jest równa zero i `E` jest przestrzenią nazw i `E` zawiera zagnieżdżone przestrzenie nazw o nazwie `I`, wynik jest tego obszaru nazw.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="1b6fe-953">W przeciwnym razie, jeśli `E` jest przestrzenią nazw i `E` zawiera dostępny typ o nazwie `I` i `K`  parametry typu, wynik jest zbudowany z argumentami typu danego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="1b6fe-954">Jeśli `E` jest *predefined_type* lub *primary_expression* sklasyfikowane jako typ, jeśli `E` nie jest parametrem typu i jeśli wyszukiwanie elementu członkowskiego ([wyszukanie członka](expressions.md#member-lookup)) z `I` w `E` z `K`  parametrów typu generuje dopasowania, następnie `E.I` jest obliczane i sklasyfikowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="1b6fe-955">Jeśli `I` identyfikuje typ, wynik jest zbudowany z argumentami typu danego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="1b6fe-956">Jeśli `I` identyfikuje co najmniej jedną metodę, wynik jest grupy metod z Brak wyrażenia skojarzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="1b6fe-957">Jeśli lista argumentów typu został określony, jest używany podczas wywoływania metody ogólnej ([wywołań metody opisywanego](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="1b6fe-958">Jeśli `I` identyfikuje `static` właściwości, a następnie wynik jest dostęp do właściwości, za pomocą wyrażenia nie skojarzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="1b6fe-959">Jeśli `I` identyfikuje `static` pola:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="1b6fe-960">Jeśli to pole jest `readonly` odwołania jest wykonywane poza statycznego konstruktora klasy lub struktury, w którym zadeklarowany jest pole, a następnie wynik jest wartością, czyli wartość pola statycznego `I` w `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="1b6fe-961">W przeciwnym razie wynik jest zmienną, a mianowicie pole statyczne `I` w `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="1b6fe-962">Jeśli `I` identyfikuje `static` zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="1b6fe-963">Jeśli występuje odwołanie, w obrębie klasy lub struktury, w którym zdarzenie jest zadeklarowana, a zdarzenie była zadeklarowana bez *event_accessor_declarations* ([zdarzenia](classes.md#events)), następnie `E.I` dokładnie przetwarzania tak, jakby `I` zostały pole statyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="1b6fe-964">W przeciwnym razie wynikiem jest dostęp do zdarzenia przy użyciu Brak wyrażenia skojarzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="1b6fe-965">Jeśli `I` identyfikuje stałą, a następnie wynik jest wartością, czyli wartość tej stałej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="1b6fe-966">Jeśli `I` identyfikuje element członkowski wyliczenia, a następnie wynik jest wartością, czyli wartość tego elementu członkowskiego wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="1b6fe-967">W przeciwnym razie `E.I` jest odwołaniem nieprawidłowej składowej i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-968">Jeśli `E` jest dostęp do właściwości, indeksatora dostępu, zmienna lub wartości, którego typ jest `T`i wyszukać składowej ([wyszukanie członka](expressions.md#member-lookup)) z `I` w `T` z `K`  argumentami typu tworzy następnie dopasowania `E.I` jest obliczane i sklasyfikowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="1b6fe-969">Po pierwsze, jeśli `E` jest właściwość lub indeksator dostępu, a następnie wartość właściwości lub indeksatora dostęp jest uzyskiwany ([wartości wyrażeń](expressions.md#values-of-expressions)) i `E` jest sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="1b6fe-970">Jeśli `I` identyfikuje co najmniej jednej metody, a następnie wynik jest grupą metoda z wyrażeniem skojarzonego wystąpienia `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="1b6fe-971">Jeśli lista argumentów typu został określony, jest używany podczas wywoływania metody ogólnej ([wywołań metody opisywanego](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="1b6fe-972">Jeśli `I` identyfikuje właściwości wystąpienia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="1b6fe-973">Jeśli `E` jest `this`, `I` identyfikuje automatycznie implementowanej właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) bez metody ustawiającej i odwołanie ma miejsce w konstruktorze wystąpienia dla Typ klasy lub struktury `T`, a następnie wynik jest zmienną, a mianowicie pole zapasowe ukryte dotyczący właściwości automatycznej podane przez `I` w wystąpieniu programu `T` przez `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="1b6fe-974">W przeciwnym razie wynikiem jest dostęp do właściwości, za pomocą wyrażenia skojarzonego wystąpienia `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="1b6fe-975">Jeśli `T` jest *class_type* i `I` identyfikuje pole wystąpienia, *class_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="1b6fe-976">Jeśli wartość `E` jest `null`, a następnie `System.NullReferenceException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="1b6fe-977">W przeciwnym razie, jeśli pole jest `readonly` odwołania jest wykonywane poza konstruktora wystąpienia klasy, w którym zadeklarowany jest pole, a następnie wynik jest wartością, czyli wartość pola `I` w obiekcie wywoływanym przez `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="1b6fe-978">W przeciwnym razie wynik jest zmienną, czyli pola `I` w obiekcie wywoływanym przez `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="1b6fe-979">Jeśli `T` jest *struct_type* i `I` identyfikuje pole wystąpienia, *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="1b6fe-980">Jeśli `E` jest wartością, czy pole jest `readonly` odwołania jest wykonywane poza konstruktora wystąpienia struktury, w którym zadeklarowany jest pole, a następnie wynik jest wartością, czyli wartość pola `I` w wystąpieniu struktury przez  `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="1b6fe-981">W przeciwnym razie wynik jest zmienną, czyli pola `I` w wystąpieniu struktury przez `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="1b6fe-982">Jeśli `I` identyfikuje wystąpienie zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="1b6fe-983">Jeśli występuje odwołanie, w obrębie klasy lub struktury, w którym zdarzenie jest zadeklarowana, a zdarzenie była zadeklarowana bez *event_accessor_declarations* ([zdarzenia](classes.md#events)), a odwołanie nie jest wykonywane jako po lewej stronie `+=` lub `-=` operatora, a następnie `E.I` jest przetwarzany dokładnie tak, jakby `I` został pola wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="1b6fe-984">W przeciwnym razie wynikiem jest dostęp do zdarzenia przy użyciu wyrażenia skojarzonego wystąpienia `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="1b6fe-985">W przeciwnym razie zostanie podjęta próba przetworzenia `E.I` jako wywołanie metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="1b6fe-986">W przypadku niepowodzenia `E.I` jest odwołaniem nieprawidłowej składowej i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="1b6fe-987">Identyczne nazwy proste i nazwy typów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-987">Identical simple names and type names</span></span>

<span data-ttu-id="1b6fe-988">Dostępu do elementu członkowskiego w postaci `E.I`, jeśli `E` jest pojedynczy identyfikator i, jeśli znaczenie `E` jako *simple_name* ([proste nazwy](expressions.md#simple-names)) jest stała, pole, właściwość lokalnej zmiennej lub parametru za pomocą tego samego typu co znaczenie `E` jako *type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)), a następnie obie znaczeń z `E` są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="1b6fe-989">Znaczenie dwa możliwe `E.I` nigdy nie są niejednoznaczne, ponieważ `I` niekoniecznie musi należeć do typu `E` w obu przypadkach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="1b6fe-990">Innymi słowy, reguła po prostu zezwala na dostęp do statycznych elementów członkowskich i zagnieżdżone typy `E` gdzie błąd kompilacji w przeciwnym razie będą miały miejsce.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="1b6fe-991">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="1b6fe-992">Gramatyka niejednoznaczności</span><span class="sxs-lookup"><span data-stu-id="1b6fe-992">Grammar ambiguities</span></span>

<span data-ttu-id="1b6fe-993">Produkcji dla *simple_name* ([proste nazwy](expressions.md#simple-names)) i *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) może powodować powstanie niejednoznaczności w Gramatyka wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="1b6fe-994">Na przykład instrukcja:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="1b6fe-995">można zinterpretować jako wywołanie `F` za pomocą dwóch argumentów `G < A` i `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="1b6fe-996">Ewentualnie może zostać zinterpretowane jako wywołanie `F` jeden argument, który jest wywołanie metody ogólnej `G` dwa argumenty typu i jeden argument regularne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="1b6fe-997">Jeśli sekwencja tokeny może być analizowana (w kontekście) jako *simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), lub *pointer_member_access* ([dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)) kończą się ciągiem *type_argument_list* ([argumentów typu](types.md#type-arguments)), token następujący bezpośrednio po zamykającym `>` token jest sprawdzany pod.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="1b6fe-998">Jeśli jest to jeden z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="1b6fe-999">a następnie *type_argument_list* są przechowywane jako część *simple_name*, *member_access* lub *pointer_member_access* oraz inne możliwe analizy sekwencja tokenów jest odrzucany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="1b6fe-1000">W przeciwnym razie *type_argument_list* nie jest uważany za część *simple_name*, *member_access* lub *pointer_member_access*nawet wtedy, gdy nie ma żadnych możliwych analizy sekwencja tokeny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="1b6fe-1001">Należy zauważyć, że te reguły są stosowane podczas analizowania *type_argument_list* w *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="1b6fe-1002">Wykonywanie instrukcji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="1b6fe-1003">zgodnie z regułą, są interpretowane jako zostaną wywołanie `F` jeden argument, który jest wywołanie metody ogólnej `G` dwa argumenty typu i jeden argument regularne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="1b6fe-1004">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="1b6fe-1005">będzie każdego interpretowana jako wywołanie `F` za pomocą dwóch argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="1b6fe-1006">Wykonywanie instrukcji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="1b6fe-1007">będzie interpretowany jako operator less than, większa niż operatora i operator, plus jednoargumentowy tak, jakby zaprojektował instrukcji `x = (F < A) > (+y)`, a nie jako *simple_name* z *type_argument_list* następuje operator plus pliku binarnego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="1b6fe-1008">W instrukcji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="1b6fe-1009">tokeny `C<T>` są interpretowane jako *namespace_or_type_name* z *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="1b6fe-1010">Wyrażenia wywołania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1010">Invocation expressions</span></span>

<span data-ttu-id="1b6fe-1011">*Invocation_expression* służy do wywoływania metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="1b6fe-1012">*Invocation_expression* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) jeśli zawiera co najmniej jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="1b6fe-1013">*Primary_expression* ma typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="1b6fe-1014">Co najmniej jeden argument opcjonalne *argument_list* ma typ kompilacji `dynamic` i *primary_expression* nie ma typu delegata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="1b6fe-1015">W tym przypadku kompilator klasyfikuje *invocation_expression* jako wartość typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="1b6fe-1016">Reguły poniżej, aby określić znaczenia *invocation_expression* zostaną następnie zastosowane w czasie wykonywania, przy użyciu typu run-time zamiast typ kompilacji udostępnianych przez *primary_expression* i argumenty, które mają typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="1b6fe-1017">Jeśli *primary_expression* nie ma typu kompilacji `dynamic`, a następnie wywołania metody ulega sprawdzenie w czasie kompilacji ograniczona zgodnie z opisem w [sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="1b6fe-1018">*Primary_expression* z *invocation_expression* musi być grupy metod lub wartość *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="1b6fe-1019">Jeśli *primary_expression* jest grupą metoda *invocation_expression* jest wywołanie metody ([wywołań metody opisywanego](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="1b6fe-1020">Jeśli *primary_expression* jest wartością *delegate_type*, *invocation_expression* to wywołanie delegata ([delegowanie wywołań](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="1b6fe-1021">Jeśli *primary_expression* nie jest grupa metod ani wartości *delegate_type*, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-1022">Opcjonalny *argument_list* ([listy argumentów](expressions.md#argument-lists)) zawiera wartości i odwołań do zmiennych parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="1b6fe-1023">Wynik obliczania wartości *invocation_expression* jest klasyfikowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="1b6fe-1024">Jeśli *invocation_expression* wywołuje metody lub delegata, która zwraca `void`, wynik jest nothing.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="1b6fe-1025">Wyrażenie zostało zaklasyfikowane jako nic nie jest dozwolona tylko w kontekście *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)) lub jako treść metody *lambda_expression*([Wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="1b6fe-1026">W przeciwnym razie wystąpi błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-1027">W przeciwnym razie wynik jest wartością typu zwracane przez metody lub delegata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="1b6fe-1028">Wywołania metod</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1028">Method invocations</span></span>

<span data-ttu-id="1b6fe-1029">Na wywołanie metody *primary_expression* z *invocation_expression* musi być grupą metod.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="1b6fe-1030">Grupy metod identyfikuje jednej metody do wywołania lub zestaw przeciążonych metod do wyboru określonej metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="1b6fe-1031">W tym ostatnim przypadku określonej metody do wywołania jest na podstawie podanego przez typy argumentów kontekstu *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="1b6fe-1032">Powiązania w czasie przetwarzania wywołania metody w postaci `M(A)`, gdzie `M` jest grupa metod (prawdopodobnie w tym *type_argument_list*), i `A` to opcjonalna *argument_ Lista*, składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-1033">Jest tworzony zestaw metod do wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="1b6fe-1034">Dla każdej metody `F` skojarzone z grupą metod `M`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="1b6fe-1035">Jeśli `F` jest nieogólnego, `F` jest kandydatem po:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="1b6fe-1036">`M` nie zawiera typu argumentu listy, a</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="1b6fe-1037">`F` ma zastosowanie w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="1b6fe-1038">Jeśli `F` ogólnego i `M` ma nie listy argumentów typu `F` jest kandydatem po:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="1b6fe-1039">Wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) zakończy się powodzeniem, wnioskowanie listy argumentów typu połączenia, a</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="1b6fe-1040">Gdy argumentami typu wywnioskowanego są zastępowane dla parametrów typu odpowiedniej metody, wszystkie typy utworzone na liście parametrów F spełnia ich ograniczenia ([spełniających ograniczenia](types.md#satisfying-constraints)) oraz listę parametrów `F` jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="1b6fe-1041">Jeśli `F` ogólnego i `M` zawiera listę argumentów typu `F` jest kandydatem po:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="1b6fe-1042">`F` ma taką samą liczbę parametrów typu w metodzie, jak podano w liście argumentów typu i</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="1b6fe-1043">Gdy argumenty typu są zastępowane dla parametrów typu odpowiedniej metody, wszystkie typy utworzone na liście parametrów F spełnia ich ograniczenia ([spełniających ograniczenia](types.md#satisfying-constraints)) oraz listę parametrów `F` ma zastosowanie w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="1b6fe-1044">Zestaw metod Release candidate jest ograniczona do zawierają tylko metody z najbardziej pochodnej typów: Dla każdej metody `C.F` w zestawie, gdzie `C` jest typem, w którym metoda `F` zadeklarowano wszystkich metod zadeklarowanych w podstawowym typem `C` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="1b6fe-1045">Ponadto jeśli `C` jest inny niż typ klasy `object`, wszystkie metody, które zostało zadeklarowane w typie interfejsu są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="1b6fe-1046">(Ta zasada ostatnie tylko ma wpływ podczas grupy metod, wynikiem wyszukiwania elementu członkowskiego dla parametru typu o skutecznej klasy bazowej, innego niż obiekt i pusty interfejs skuteczne, konfiguruje).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="1b6fe-1047">Jeśli wynikowy zestaw metod jest pusta, dalsze przetwarzanie wzdłuż poniższe kroki są porzucone, a zamiast tego zostanie podjęta próba można przetworzyć wywołania jako wywołania metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="1b6fe-1048">W przypadku niepowodzenia, a nie dotyczy istnieją metody, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-1049">Najlepszą metodą zestaw metod Release candidate jest identyfikowany za pomocą zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="1b6fe-1050">Jeśli nie można zidentyfikować pojedynczy najlepszą metodę, wywołanie metody jest niejednoznaczny i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="1b6fe-1051">Podczas rozpoznawania przeciążenia parametry metody ogólnej są traktowane jako po argumentów typu (dostarczane lub wywnioskowane) podstawieniu dla odpowiednich parametrów typu metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="1b6fe-1052">Odbywa się ostatecznej weryfikacji wybranej najlepszą metodę:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="1b6fe-1053">Metoda sprawdzania poprawności w kontekście grupy metod: Jeśli najlepszą metodę statyczną metodę, grupy metod musi mieć związek z *simple_name* lub *member_access* za pośrednictwem typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="1b6fe-1054">Jeśli najlepsza metoda jest metodą wystąpienia, grupy metod musi mieć związek z *simple_name*, *member_access* za pośrednictwem zmiennej lub wartość lub *base_access*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="1b6fe-1055">Jeśli żadna z tych wymagań nie jest spełniony, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="1b6fe-1056">Najlepszą metodę w przypadku metody rodzajowej, argumenty typu (dostarczane lub wywnioskowane) są porównywane z ograniczeniami ([spełniających ograniczenia](types.md#satisfying-constraints)) zadeklarowany dla metody rodzajowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="1b6fe-1057">Jeśli którykolwiek z argumentów typu nie spełniają odpowiednie elementem dla parametru typu, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-1058">Gdy metoda zostanie wybrane i sprawdzone w momencie powiązania przez powyższe kroki, rzeczywiste wywołanie środowiska wykonawczego są przetwarzane zgodnie z regułami wywołania elementu funkcji opisanych w [sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="1b6fe-1059">Intuicyjne efekt zasad rozpoznawania opisanych powyżej jest następująca: Aby zlokalizować konkretnej metody wywoływane przez wywołanie metody, rozpoczynać się typ wskazany przez wywołanie metody i kontynuować górę łańcucha dziedziczenia, dopóki nie zostanie znaleziony co najmniej jedną metodę ma to zastosowanie, dostępny, -Zastępowanie deklaracji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="1b6fe-1060">Następnie wykonaj wnioskowanie o typie przeciążenia w zestawie, który ma to zastosowanie, dostępny, -zastępowanie metod zadeklarowanych w tego typu i wywołaj metodę wybrany w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="1b6fe-1061">Jeśli nie znaleziono metody, spróbuj zamiast tego można przetworzyć wywołania jako wywołania metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="1b6fe-1062">Wywołań metod rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1062">Extension method invocations</span></span>

<span data-ttu-id="1b6fe-1063">W wywołaniu metody ([wywołań w wystąpieniach spakowany](expressions.md#invocations-on-boxed-instances)) z jednego z formularzy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="1b6fe-1064">Jeśli normalnego przetwarzania wywołania wykryje żadnych odpowiednich metod, podejmowana jest próba przetworzenia konstrukcji jako wywołanie metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="1b6fe-1065">Jeśli *expr* ani żadnego z *args* ma typ kompilacji `dynamic`, nie będą miały zastosowania metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="1b6fe-1066">Celem jest znalezienie najlepszych *type_name* `C`, dzięki czemu odpowiedniego wywołania metody statyczne mogą być wykonywane:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="1b6fe-1067">Metody rozszerzenia `Ci.Mj` jest ***kwalifikujących się*** jeśli:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="1b6fe-1068">`Ci` jest klasą nieogólnego, -nested</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="1b6fe-1069">Nazwa `Mj` jest *identyfikator*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="1b6fe-1070">`Mj` jest dostępna i ma zastosowanie, po zastosowaniu do argumentów jako metoda statyczna, jak pokazano powyżej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="1b6fe-1071">Istnieje niejawna tożsamości, odwołanie lub konwersja boxing z *expr* na typ pierwszego parametru `Mj`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="1b6fe-1072">Wyszukaj `C` przechodzi w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="1b6fe-1073">Począwszy od najbliższej otaczającej deklaracji przestrzeni nazw, kontynuując każdy otaczający deklarację przestrzeni nazw, a kończąc zawierającego jednostki kompilacji, kolejne próby znalezienia kandydata zestaw metod rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="1b6fe-1074">Jeśli danej jednostce przestrzeni nazw lub kompilacji bezpośrednio zawiera deklaracje typu nieogólnego `Ci` przy użyciu metody rozszerzenia kwalifikujących się `Mj`, zestaw tych metod rozszerzenia jest zestaw Release candidate.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="1b6fe-1075">Jeśli typy `Ci` importowane przez *using_static_declarations* i bezpośrednio zadeklarowane w przestrzeniach nazw importowanych przez *using_namespace_directive*s w danej przestrzeni nazw lub kompilacji jednostce bezpośrednio zawiera metody rozszerzenia kwalifikujących się `Mj`, zestaw tych metod rozszerzenia jest zestaw Release candidate.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="1b6fe-1076">Jeśli nie ustawiono Release candidate znajduje się w każdej przestrzeni nazw otaczającej jednostki kompilacji lub deklaracji, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-1077">W przeciwnym razie funkcja rozpoznawania przeciążeń jest stosowany do Release candidate, ustaw zgodnie z opisem w ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="1b6fe-1078">Jeśli nie pojedynczego najlepszą metodę zostanie znaleziony, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-1079">`C` jest to typ, w ramach którego najlepszą metodę jest zadeklarowany jako metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="1b6fe-1080">Za pomocą `C` jako obiekt docelowy wywołania metody które są następnie przetwarzane jako wywołanie metody statycznej ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="1b6fe-1081">Poprzedni reguły oznaczają, że wystąpienie metody mają pierwszeństwo przed metody rozszerzenia, że metody rozszerzenia są dostępne w deklaracji przestrzeni nazw wewnętrzny mają pierwszeństwo przed metody rozszerzenia są dostępne w deklaracje zewnętrzne przestrzeni nazw i rozszerzenia metody zadeklarowane jako bezpośrednio w przestrzeni nazw mają pierwszeństwo przed metody rozszerzenia zaimportowane do tej samej przestrzeni nazw z za pomocą dyrektywy przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="1b6fe-1082">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1082">For example:</span></span>
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

<span data-ttu-id="1b6fe-1083">W tym przykładzie `B`firmy metody mają pierwszeństwo przed z pierwszą metodą rozszerzenia i `C`firmy metody mają pierwszeństwo przed obu metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="1b6fe-1084">Dane wyjściowe, w tym przykładzie są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="1b6fe-1085">`D.G` pierwszeństwo `C.G`, i `E.F` ma pierwszeństwo przed zarówno `D.F` i `C.F`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="1b6fe-1086">Delegowanie wywołań</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1086">Delegate invocations</span></span>

<span data-ttu-id="1b6fe-1087">Na wywołanie delegata *primary_expression* z *invocation_expression* musi być wartością z *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="1b6fe-1088">Co więcej, biorąc pod uwagę *delegate_type* jako funkcja składowa o tej samej listy parametrów jako *delegate_type*, *delegate_type* muszą być stosowane () [Dotyczy funkcja składowa](expressions.md#applicable-function-member)) w odniesieniu do *argument_list* z *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="1b6fe-1089">Przetwarzanie środowiska wykonawczego wywołanie delegata w postaci `D(A)`, gdzie `D` jest *primary_expression* z *delegate_type* i `A` jest opcjonalny *argument_list*, składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-1090">`D` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1090">`D` is evaluated.</span></span> <span data-ttu-id="1b6fe-1091">Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1092">Wartość `D` jest sprawdzany w celu był prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="1b6fe-1093">Jeśli wartość `D` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1094">W przeciwnym razie `D` jest odwołanie do wystąpienia delegata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="1b6fe-1095">Element członkowski wywołania funkcji ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) są wykonywane na każdej z nich wywoływane na liście wywołanie delegata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="1b6fe-1096">Wywoływane jednostek składające się z wystąpieniem i metoda wystąpienia wystąpienie wywołania zasady jest wystąpienie zawartego w obiekcie możliwy do wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="1b6fe-1097">Dostęp do elementu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1097">Element access</span></span>

<span data-ttu-id="1b6fe-1098">*Element_access* składa się z *primary_no_array_creation_expression*, a następnie "`[`" token, a następnie *argument_list*, a następnie " `]`"tokenu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="1b6fe-1099">*Argument_list* składa się z co najmniej jeden *argument*s, oddzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="1b6fe-1100">*Argument_list* z *element_access* nie mogą zawierać `ref` lub `out` argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="1b6fe-1101">*Element_access* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) jeśli zawiera co najmniej jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="1b6fe-1102">*Primary_no_array_creation_expression* ma typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="1b6fe-1103">Co najmniej jednego wyrażenia *argument_list* ma typ kompilacji `dynamic` i *primary_no_array_creation_expression* nie ma typu tablicowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="1b6fe-1104">W tym przypadku kompilator klasyfikuje *element_access* jako wartość typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="1b6fe-1105">Reguły poniżej, aby określić znaczenia *element_access* zostaną następnie zastosowane w czasie wykonywania, przy użyciu typu run-time zamiast typ kompilacji udostępnianych przez *primary_no_array_creation_expression*i *argument_list* wyrażeń, które mają typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="1b6fe-1106">Jeśli *primary_no_array_creation_expression* nie ma typu kompilacji `dynamic`, a następnie dostęp do elementu ulega sprawdzenie w czasie kompilacji ograniczona zgodnie z opisem w [kompilacji sprawdzanie dynamiczny Rozpoznanie przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="1b6fe-1107">Jeśli *primary_no_array_creation_expression* z *element_access* jest wartością *array_type*, *element_access* jest dostęp do tablicy ([tablicy dostępu](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="1b6fe-1108">W przeciwnym razie *primary_no_array_creation_expression* musi być zmienną lub wartość klasy, struktury lub typ interfejsu, który ma co najmniej jednego członka indeksatora, w którym to przypadku *element_access* jest dostęp indeksatora ([dostęp indeksatora](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="1b6fe-1109">Dostęp do tablicy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1109">Array access</span></span>

<span data-ttu-id="1b6fe-1110">Aby uzyskać dostęp do tablicy *primary_no_array_creation_expression* z *element_access* musi być wartością z *array_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="1b6fe-1111">Ponadto *argument_list* tablicy dostępu nie może zawierać argumenty nazwane. Liczba wyrażeń w *argument_list* musi być taka sama jak ranga *array_type*, a każde wyrażenie musi być typu `int`, `uint`, `long`, `ulong`, lub musi umożliwiać niejawną konwersję do co najmniej jednej z tych typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="1b6fe-1112">Wynik oceny dostęp do tablicy jest zmienną typu elementu tablicy, a mianowicie elementu tablicy, wybrana przez wartości wyrażeń w *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="1b6fe-1113">Przetwarzanie środowiska wykonawczego dostęp do tablicy w formie `P[A]`, gdzie `P` jest *primary_no_array_creation_expression* z *array_type* i `A` jest *argument_list*, składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-1114">`P` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1114">`P` is evaluated.</span></span> <span data-ttu-id="1b6fe-1115">Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1116">Wyrażenia indeksu *argument_list* są obliczane w kolejności od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="1b6fe-1117">Po dokonaniu oceny każde wyrażenie indeksu niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) jest wykonywane na jeden z następujących typów: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="1b6fe-1118">Pierwszy typ na tej liście, dla której istnieje niejawna konwersja jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="1b6fe-1119">Na przykład jeśli wyrażenie indeksu ma typ `short` następnie niejawną konwersję do `int` jest wykonywana, ponieważ niejawne konwersje z elementu `short` do `int` i z `short` do `long` są możliwe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="1b6fe-1120">Jeśli Szacowanie wyrażenia indeksu lub kolejnych niejawnej konwersji powoduje wyjątek, nie dalsze wyrażenia indeksu są obliczane i żadne dodatkowe kroki są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1121">Wartość `P` jest sprawdzany w celu był prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="1b6fe-1122">Jeśli wartość `P` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1123">Wartość każde wyrażenie w *argument_list* jest sprawdzana względem rzeczywiste granice każdego wymiaru instancji tabeli odwołuje się `P`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="1b6fe-1124">Jeśli co najmniej jednej wartości są poza zakresem, `System.IndexOutOfRangeException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1125">Lokalizacja elementu tablicy, określone przez wyrażeń indeksu jest obliczana, a ta lokalizacja staje się wynik dostęp do tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="1b6fe-1126">Dostęp indeksatora</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1126">Indexer access</span></span>

<span data-ttu-id="1b6fe-1127">Aby uzyskać dostęp indeksatora *primary_no_array_creation_expression* z *element_access* musi być zmienną lub wartość klasy, struktury lub typ interfejsu i ten typ musi implementować jeden lub więcej Indeksatory, które mają zastosowanie w odniesieniu do *argument_list* z *element_access*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="1b6fe-1128">Powiązania w czasie przetwarzania dostęp indeksatora formularza `P[A]`, gdzie `P` jest *primary_no_array_creation_expression* klasy, struktury lub interfejsu typu `T`, i `A` jest *argument_list*, składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-1129">Set indeksatorów, dostarczone przez `T` jest konstruowany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="1b6fe-1130">Zestaw składa się z wszystkich indeksatorów zadeklarowanych w `T` lub podstawowym typem `T` , które nie są `override` deklaracje i są dostępne w bieżącym kontekście ([dostęp do elementu członkowskiego](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="1b6fe-1131">Zestaw jest ograniczone do tych indeksatorów, które są stosowane i nie jest ukryta za innymi indeksatorami.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="1b6fe-1132">Następujące reguły są stosowane do każdego indeksatora `S.I` w zestawie, gdzie `S` jest typem, w której indeksator `I` jest zadeklarowany jako:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="1b6fe-1133">Jeśli `I` nie jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)), następnie `I` zostanie usunięty z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="1b6fe-1134">Jeśli `I` jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)), następnie wszystkie indeksatory zadeklarowane w typie podstawowym z `S` są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="1b6fe-1135">Jeśli `I` jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)) i `S` jest inny niż typ klasy `object`, wszystkie indeksatory zadeklarowanych w interfejsie są usuwane z zestawu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="1b6fe-1136">Jeśli wynikowy zestaw indeksatory Release candidate jest pusta, następnie istnieje nie dotyczy indeksatory i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-1137">Najlepsze indeksatora set indeksatorów Release candidate jest identyfikowany za pomocą zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="1b6fe-1138">Jeśli nie można zidentyfikować pojedynczy indeksatora najlepsze, dostęp do indeksatora jest niejednoznaczny i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-1139">Wyrażenia indeksu *argument_list* są obliczane w kolejności od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="1b6fe-1140">Wynik przetworzenia dostęp indeksatora to wyrażenie, który został sklasyfikowany jako dostęp indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="1b6fe-1141">Wyrażenie dostępu do indeksatora odwołuje się do indeksatora, określone w poprzednim kroku, a ma wyrażenia skojarzonego wystąpienia `P` i lista argumentów skojarzonych z `A`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="1b6fe-1142">W zależności od kontekstu, w którym jest używany, dostęp indeksatora powoduje wywołanie albo *pobierająca* lub *ustawiająca metoda dostępu* indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="1b6fe-1143">Jeśli dostęp indeksatora jest elementem docelowym przypisania, *ustawiająca metoda dostępu* wywoływana w celu przypisania nowej wartości ([przypisanie proste](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="1b6fe-1144">We wszystkich innych przypadkach *pobierająca* wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="1b6fe-1145">Ten dostęp</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1145">This access</span></span>

<span data-ttu-id="1b6fe-1146">A *this_access* składa się z słowo zastrzeżone `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="1b6fe-1147">A *this_access* jest dozwolona tylko w *bloku* konstruktora wystąpienia, metoda wystąpienia lub metodę dostępu, której wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="1b6fe-1148">Daje on następujące znaczenie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="1b6fe-1149">Gdy `this` jest używany w *primary_expression* w konstruktorze wystąpienia klasy, jest klasyfikowana jako wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="1b6fe-1150">Typ wartości jest typu wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) klasy, w ramach którego występuje użycia, a wartość jest odwołanie do obiektu podczas konstruowania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="1b6fe-1151">Gdy `this` jest używany w *primary_expression* w ramach metodę wystąpienia lub metoda dostępu do wystąpienia klasy jest klasyfikowana jako wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="1b6fe-1152">Typ wartości jest typu wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) klasy, w którym występuje użycia, a wartość jest odwołanie do obiektu, dla której wywołano metodę lub metodę dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="1b6fe-1153">Gdy `this` jest używany w *primary_expression* w konstruktorze wystąpienia struktury, jest klasyfikowana jako zmienną.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="1b6fe-1154">Typ zmiennej jest typem wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) struktury, w którym występuje użycia, a zmiennej reprezentuje struktury budowany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="1b6fe-1155">`this` Zmiennej konstruktora wystąpienia struktury zachowuje się tak samo jak `out` parametr typu struktury — w szczególności oznacza to, zmienna musi być zdecydowanie przypisana w każdej ścieżce wykonywania wystąpienia Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="1b6fe-1156">Gdy `this` jest używany w *primary_expression* w ramach metodę wystąpienia lub metoda dostępu do wystąpienia struktury jest klasyfikowana jako zmienną.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="1b6fe-1157">Typ zmiennej jest typem wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) struktury, w którym występuje użycia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="1b6fe-1158">Jeśli metoda lub metoda dostępu nie jest iteratorem ([Iteratory](classes.md#iterators)), `this` zmienna reprezentuje struktury, dla której metodę lub metodę dostępu została wywołana i zachowuje się tak samo jak `ref` parametr typu struktury.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="1b6fe-1159">Jeśli ta metoda lub metody dostępu jest iteratorem, `this` zmienna reprezentuje kopiowania struktury, dla której metodę lub metodę dostępu została wywołana i zachowuje się tak samo jak wartość parametru typu struktury.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="1b6fe-1160">Korzystanie z `this` w *primary_expression* w kontekście innych niż wymienione powyżej jest błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="1b6fe-1161">W szczególności, nie jest możliwe do odwoływania się do `this` w statycznej metody dostępu właściwość statyczna lub *variable_initializer* deklaracji pola.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="1b6fe-1162">Dostęp bazowy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1162">Base access</span></span>

<span data-ttu-id="1b6fe-1163">A *base_access* składa się z słowo zastrzeżone `base` następuje "`.`" token i identyfikator lub moduł *argument_list* w nawiasach kwadratowych:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="1b6fe-1164">A *base_access* umożliwia dostęp do składowych klasy bazowej, ukryte przez podobnie nazwanych elementów członkowskich w bieżącej klasie lub strukturze.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="1b6fe-1165">A *base_access* jest dozwolona tylko w *bloku* konstruktora wystąpienia, metoda wystąpienia lub metodę dostępu, której wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="1b6fe-1166">Gdy `base.I` odbywa się w klasie lub strukturze, `I` musi oznaczają składową klasy bazowej tej klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="1b6fe-1167">Podobnie, gdy `base[E]` występuje w klasie, indeksator musi istnieć w klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="1b6fe-1168">W czasie powiązania *base_access* wyrażeń formularza `base.I` i `base[E]` są oceniane, dokładnie tak, jakby były one napisane `((B)this).I` i `((B)this)[E]`, gdzie `B` jest klasą bazową klasy lub struktury, w której występuje konstrukcja.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="1b6fe-1169">W efekcie `base.I` i `base[E]` odpowiadają `this.I` i `this[E]`, z wyjątkiem `this` są wyświetlane jako wystąpienia klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="1b6fe-1170">Gdy *base_access* odwołuje się funkcja wirtualna elementu członkowskiego (metoda, właściwość lub indeksator), oznaczanie funkcji elementu członkowskiego do wywołania w czasie wykonywania ([sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) zostanie zmieniony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="1b6fe-1171">Funkcja składowa, wywoływana jest określana przez wyszukiwanie najbardziej pochodnej ([metody wirtualnej](classes.md#virtual-methods)) elementu członkowskiego funkcji w odniesieniu do `B` (zamiast w odniesieniu do typów w czasie wykonywania z `this`, jak będzie zwykle w programie access-base).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="1b6fe-1172">W związku z tym, w ramach `override` z `virtual` funkcja składowa *base_access* może służyć do wywołania dziedziczona implementacja funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="1b6fe-1173">Jeśli odwołuje się funkcja składowa *base_access* jest abstrakcyjny, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="1b6fe-1174">Inkrementacja przyrostkowa i operatory dekrementacji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="1b6fe-1175">Argument przyrostka inkrementacji i dekrementacji operacji musi być wyrażeniem sklasyfikowane jako zmienną, dostęp do właściwości lub indeksatora dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="1b6fe-1176">Wynikiem operacji jest wartością tego samego typu jako argumentu operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="1b6fe-1177">Jeśli *primary_expression* ma typ kompilacji `dynamic` , a następnie operator jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)), *post_increment_expression*lub *post_decrement_expression* ma typ kompilacji `dynamic` i następujące zasady są stosowane w czasie wykonywania za pomocą typu run-time z *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="1b6fe-1178">Jeśli argument przyrostkowe Zwiększ operacja dekrementacji jest właściwością lub dostęp indeksatora, właściwość lub indeksator musi mieć zarówno `get` i `set` metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="1b6fe-1179">Jeśli nie jest to możliwe, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-1180">Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1181">Wstępnie zdefiniowane `++` i `--` operatory istnieją dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`i dowolnego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="1b6fe-1182">Wstępnie zdefiniowane `++` operatory zwracają wartość, o których produkowane przez dodanie 1 argument i wstępnie zdefiniowane `--` operatory zwracają wartość, o których produkowane przez odjęcie ilości 1 z operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="1b6fe-1183">W `checked` kontekstu, jeśli wynik to dodawania lub odejmowania jest spoza zakresu typu wyniku, a typ wyniku jest typu całkowitoliczbowego lub typu wyliczeniowego `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="1b6fe-1184">Przetwarzanie środowiska wykonawczego przyrostka inkrementacji lub dekrementacji operacji formularza `x++` lub `x--` składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="1b6fe-1185">Jeśli `x` zostanie sklasyfikowany jako zmienną:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="1b6fe-1186">`x` jest oceniany w celu utworzenia zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="1b6fe-1187">Wartość `x` jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="1b6fe-1188">Operatora jest wywoływana z wartością zapisaną `x` jako argumentem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="1b6fe-1189">Wartość zwracana przez operator znajduje się w lokalizacji określonej przez ocenę `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="1b6fe-1190">Zapisane wartości `x` staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="1b6fe-1191">Jeśli `x` zostanie sklasyfikowany jako właściwość lub indeksator dostępu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="1b6fe-1192">Wyrażenia wystąpienia (Jeśli `x` nie jest `static`) i listą argumentów (Jeśli `x` jest dostęp indeksatora) skojarzony z `x` są oceniane, a wyniki są używane w kolejnych żadaniach `get` i `set` wywołania metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="1b6fe-1193">`get` Akcesor `x` wywoływaną i jest zapisywany w zwracanej wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="1b6fe-1194">Operatora jest wywoływana z wartością zapisaną `x` jako argumentem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="1b6fe-1195">`set` Akcesor `x` jest wywoływana z wartością zwróconą przez operatora jako jego `value` argumentu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="1b6fe-1196">Zapisane wartości `x` staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="1b6fe-1197">`++` i `--` Operatorzy pomocy technicznej notacji prefiksów ([przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="1b6fe-1198">Zazwyczaj wynikiem `x++` lub `x--` jest wartością `x` przed wykonaniem operacji, podczas gdy wynik `++x` lub `--x` jest wartością `x` po wykonaniu operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="1b6fe-1199">W obu przypadkach `x` ma taką samą wartość po wykonaniu operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="1b6fe-1200">`operator ++` Lub `operator --` implementacja może być wywoływany przy użyciu notacji przyrostka lub przedrostka.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="1b6fe-1201">Nie jest możliwe operator oddzielne implementacje dla dwóch notacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="1b6fe-1202">New — operator</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1202">The new operator</span></span>

<span data-ttu-id="1b6fe-1203">`new` Operator jest używany do tworzenia nowych wystąpień typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="1b6fe-1204">Istnieją trzy rodzaje `new` wyrażeń:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="1b6fe-1205">Wyrażenia tworzenia obiektów są używane do tworzenia nowych wystąpień typów klas i typów wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="1b6fe-1206">Wyrażenie tworzenia tablicy są używane do tworzenia nowych wystąpień typów tablicowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="1b6fe-1207">Wyrażenia tworzenia delegata są używane do tworzenia nowych wystąpień obiektu delegowanego typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="1b6fe-1208">`new` Operator wymaga utworzenia wystąpienia typu, ale nie musi oznaczać dynamicznej alokacji pamięci.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="1b6fe-1209">W szczególności wystąpień typów wartości wymagane nie dodatkowe pamięci powyżej zmiennych, w których są one przechowywane i występować nie dynamicznej alokacji podczas `new` służy do tworzenia wystąpień typów wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="1b6fe-1210">Wyrażenia tworzenia obiektów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1210">Object creation expressions</span></span>

<span data-ttu-id="1b6fe-1211">*Object_creation_expression* służy do tworzenia nowego wystąpienia *class_type* lub *value_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="1b6fe-1212">*Typu* z *object_creation_expression* musi być *class_type*, *value_type* lub *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="1b6fe-1213">*Typu* nie może być `abstract` *class_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="1b6fe-1214">Opcjonalny *argument_list* ([listy argumentów](expressions.md#argument-lists)) jest dozwolona tylko wtedy, gdy *typu* jest *class_type* lub *struct_ Typ*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="1b6fe-1215">Wyrażenie tworzenia obiektu można pominąć listy argumentów konstruktora, a otaczającym nawiasów pod warunkiem, że obejmuje Inicjator obiektu lub inicjatora kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="1b6fe-1216">Pominięcie listy argumentów konstruktora i otaczający nawiasów jest równoznaczne z użyciem pustą listą argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="1b6fe-1217">Przetwarzanie wyrażenie tworzenia obiektu, który obejmuje Inicjator obiektu lub inicjatora kolekcji składa się z przetwarzaniem pierwsze konstruktora wystąpienia i następnie przetwarzania inicjalizacje element członkowski lub element określony przez inicjatora obiektów ([ Inicjatory obiektów](expressions.md#object-initializers)) lub inicjatora kolekcji ([inicjatory kolekcji](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="1b6fe-1218">Jeśli którykolwiek z argumentów opcjonalny *argument_list* ma typ kompilacji `dynamic` , a następnie *object_creation_expression* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) i następujące zasady są stosowane w czasie wykonywania za pomocą typu run-time tych argumentów *argument_list* mają typów w czasie kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="1b6fe-1219">Jednak tworzenie obiektu ulega sprawdzenie w czasie kompilacji ograniczona zgodnie z opisem w [kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="1b6fe-1220">Powiązania w czasie przetwarzania *object_creation_expression* formularza `new T(A)`, gdzie `T` jest *class_type* lub *value_type* i `A` to opcjonalna *argument_list*, składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="1b6fe-1221">Jeśli `T` jest *value_type* i `A` nie występuje:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="1b6fe-1222">*Object_creation_expression* jest wywołanie konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="1b6fe-1223">Wynik *object_creation_expression* jest wartość typu `T`, czyli wartość domyślna dla `T` zgodnie z definicją w [typu System.ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="1b6fe-1224">W przeciwnym razie, jeśli `T` jest *type_parameter* i `A` nie występuje:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="1b6fe-1225">Jeśli nie ograniczenie typu wartości lub ograniczenie konstruktora ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) została określona dla `T`, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="1b6fe-1226">Wynik *object_creation_expression* jest wartością typu run-time, powiązanej parametr typu, a mianowicie wynik wywoływania domyślnego konstruktora typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="1b6fe-1227">Typu run-time może być typem referencyjnym lub typem wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="1b6fe-1228">W przeciwnym razie, jeśli `T` jest *class_type* lub *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="1b6fe-1229">Jeśli `T` jest `abstract` *class_type*, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="1b6fe-1230">Konstruktor wystąpienia do wywołania, jest określany za pomocą zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="1b6fe-1231">Zestaw konstruktory wystąpień Release candidate składa się z wszystkie konstruktory dostępne wystąpienia zadeklarowanych w `T` które są stosowane w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="1b6fe-1232">Jeśli zestaw Release candidate konstruktorów wystąpienia jest pusta lub nie można zidentyfikować pojedynczy najlepsze konstruktora wystąpienia, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="1b6fe-1233">Wynik *object_creation_expression* jest wartość typu `T`, czyli wartość utworzone przez wywołanie konstruktora wystąpienia, określone w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="1b6fe-1234">W przeciwnym razie *object_creation_expression* jest nieprawidłowy, i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-1235">Nawet wtedy, gdy *object_creation_expression* jest dynamicznie powiązane, typ kompilacji jest nadal `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="1b6fe-1236">Przetwarzanie środowiska wykonawczego *object_creation_expression* formularza `new T(A)`, gdzie `T` jest *class_type* lub *struct_type* i `A` to opcjonalna *argument_list*, składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="1b6fe-1237">Jeśli `T` jest *class_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="1b6fe-1238">Nowe wystąpienie klasy `T` jest przydzielony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="1b6fe-1239">Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia `System.OutOfMemoryException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="1b6fe-1240">Wszystkie pola nowego wystąpienia są inicjowane do wartości domyślnych ([wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="1b6fe-1241">Konstruktora wystąpienia jest wywoływana, zgodnie z regułami funkcji elementu członkowskiego wywołania ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="1b6fe-1242">Odwołanie do nowo przydzielonego wystąpienia jest automatycznie przekazywana do konstruktora wystąpienia, a wystąpienie jest możliwy z w ramach tego Konstruktor jako `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="1b6fe-1243">Jeśli `T` jest *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="1b6fe-1244">Wystąpienie typu `T` jest tworzony przez przydzielanie tymczasowej zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="1b6fe-1245">Ponieważ konstruktora wystąpienia *struct_type* jest wymagany do zdecydowanie przypisania wartości do każdego pola wystąpienia tworzony, niezbędne jest nie inicjowania zmiennej tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="1b6fe-1246">Konstruktora wystąpienia jest wywoływana, zgodnie z regułami funkcji elementu członkowskiego wywołania ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="1b6fe-1247">Odwołanie do nowo przydzielonego wystąpienia jest automatycznie przekazywana do konstruktora wystąpienia, a wystąpienie jest możliwy z w ramach tego Konstruktor jako `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="1b6fe-1248">Inicjatory obiektów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1248">Object initializers</span></span>

<span data-ttu-id="1b6fe-1249">***Inicjatora obiektu*** określa wartości zero lub więcej pól, właściwości lub indeksowanych elementy obiektu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="1b6fe-1250">Inicjatora obiektu, który składa się z sekwencji inicjatorów składowych, ujęta w `{` i `}` tokeny i je przecinkami.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="1b6fe-1251">Każdy *member_initializer* wyznacza miejsce docelowe dla inicjowania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="1b6fe-1252">*Identyfikator* należy nazywać dostępne pola lub właściwości obiektu inicjowany, natomiast *argument_list* ujęte w nawiasy kwadratowe, należy określić argumenty dla indeksatora dostępny na inicjowany obiekt.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="1b6fe-1253">Jest to błąd dla inicjatora obiektów dołączyć więcej niż jeden inicjator składowej dla tego samego pola lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="1b6fe-1254">Każdy *initializer_target* następuje znak równości i wyrażenia, inicjatora obiektów lub inicjatora kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="1b6fe-1255">Nie jest możliwe dla wyrażeń w obrębie inicjatora obiektów do odwoływania się do nowo utworzony obiekt, który inicjuje go.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="1b6fe-1256">Inicjator składowej, określająca wyrażenie po znaku równości jest przetwarzana w taki sam sposób jak przypisania ([przypisanie proste](expressions.md#simple-assignment)) do obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="1b6fe-1257">Inicjator składowej, który określa inicjatora obiektu po znaku równości ***inicjatora obiektów zagnieżdżonych***, czyli inicjalizacji osadzonego obiektu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="1b6fe-1258">Zamiast przypisywania nową wartość do pola lub właściwości, przydziały w inicjatorze obiektu zagnieżdżone są traktowane jako przypisania do elementów członkowskich pola lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="1b6fe-1259">Inicjatory obiektów zagnieżdżonych nie można zastosować do właściwości z typem wartości lub pola tylko do odczytu z typem wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="1b6fe-1260">Inicjator składowej, która określa inicjatora kolekcji po znaku równości jest inicjalizacji embedded kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="1b6fe-1261">Zamiast przypisywania nową kolekcję, do pola docelowego, właściwość lub indeksator, elementy zawarte w inicjatorze są dodawane do kolekcji odwołuje się element docelowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="1b6fe-1262">Element docelowy musi być typu kolekcji, który spełnia wymagania określone w [inicjatory kolekcji](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="1b6fe-1263">Argumenty do inicjatora indeksu będzie obliczane zawsze dokładnie jeden raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="1b6fe-1264">W związku z tym nawet wtedy, gdy argumenty znajdą się wprowadzenie nigdy używane (np. z powodu pustego inicjalizatora zagnieżdżonych), ich zostanie obliczone dla ich skutki uboczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="1b6fe-1265">Następujące klasy reprezentuje punkt o współrzędnych dwa:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="1b6fe-1266">Wystąpienie `Point` mogą być tworzone i inicjowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="1b6fe-1267">która ma taki sam skutek jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="1b6fe-1268">gdzie `__a` jest niewidoczna w przeciwnym razie i są niedostępne na zmiennej tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="1b6fe-1269">Następujące klasy reprezentuje prostokąt, który został utworzony na podstawie dwa punkty:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="1b6fe-1270">Wystąpienie `Rectangle` mogą być tworzone i inicjowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="1b6fe-1271">która ma taki sam skutek jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1271">which has the same effect as</span></span>
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
<span data-ttu-id="1b6fe-1272">gdzie `__r`, `__p1` i `__p2` to tymczasowe zmienne, które są niewidoczne i są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="1b6fe-1273">Jeśli `Rectangle`firmy Konstruktor przydziela dwa osadzone `Point` wystąpień</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="1b6fe-1274">konstrukcja następujące może służyć do zainicjowania osadzonego `Point` wystąpień zamiast przypisywać nowych wystąpień:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="1b6fe-1275">która ma taki sam skutek jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="1b6fe-1276">Podane odpowiednie definicje języka c, w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="1b6fe-1277">jest równoważne z tej serii przypisania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="1b6fe-1278">gdzie `__c`itp wygenerowanego zmiennych, które są niewidoczne i niedostępne do kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="1b6fe-1279">Należy pamiętać, że argumenty `[0,0]` ocenianą tylko raz i argumenty `[1,2]` są wykonywane tylko raz, nawet jeśli nigdy nie są one używane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="1b6fe-1280">Inicjatory kolekcji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1280">Collection initializers</span></span>

<span data-ttu-id="1b6fe-1281">Inicjator kolekcji określa elementów kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="1b6fe-1282">Inicjator kolekcji składa się z sekwencji inicjatory elementów, ujęta w `{` i `}` tokeny i je przecinkami.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="1b6fe-1283">Inicjator każdego elementu określa element do dodania do inicjowany obiekt kolekcji i składa się z listą wyrażeń ujęta w `{` i `}` tokeny i je przecinkami.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="1b6fe-1284">Element pojedynczego wyrażenia inicjatora mogą być zapisywane bez nawiasów klamrowych, ale następnie nie może być wyrażenia przypisania, aby uniknąć niejednoznaczności z inicjatorami elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="1b6fe-1285">*Non_assignment_expression* produkcji jest zdefiniowany w [wyrażenie](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="1b6fe-1286">Oto przykład wyrażenie tworzenia obiektu, które obejmuje inicjatora kolekcji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="1b6fe-1287">Obiekt kolekcji, do którego zastosowano inicjatora kolekcji musi być typu, który implementuje `System.Collections.IEnumerable` lub występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="1b6fe-1288">Dla każdego określonego elementu w kolejności, wywołuje inicjatora kolekcji `Add` metody w elemencie docelowym obiekt z listy wyrażenia inicjatora elementu jako listę argumentów, stosując wyszukanie członka normalne i przeciążenia dla każdego wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="1b6fe-1289">W związku z tym, obiekt kolekcji musi mieć odpowiednie wystąpienie lub rozszerzenie metody o nazwie `Add` dla każdego inicjatora elementu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="1b6fe-1290">Następujące klasy reprezentuje kontaktu z nazwą i Lista numerów telefonów:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="1b6fe-1291">A `List<Contact>` mogą być tworzone i inicjowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="1b6fe-1292">która ma taki sam skutek jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1292">which has the same effect as</span></span>
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
<span data-ttu-id="1b6fe-1293">gdzie `__clist`, `__c1` i `__c2` to tymczasowe zmienne, które są niewidoczne i są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="1b6fe-1294">Wyrażenie tworzenia tablicy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1294">Array creation expressions</span></span>

<span data-ttu-id="1b6fe-1295">*Array_creation_expression* służy do tworzenia nowego wystąpienia *array_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="1b6fe-1296">Wyrażenie tworzenia tablicy pierwszego formularza przydziela wystąpienie tablicy typu, który powoduje usunięcie wszystkich poszczególnych wyrażenia z listy wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="1b6fe-1297">Na przykład, wyrażenie tworzenia tablicy `new int[10,20]` tworzy wystąpienie tablicy typu `int[,]`, a wyrażenie tworzenia tablicy `new int[10][,]` tworzy tablicę typu `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="1b6fe-1298">Każde wyrażenie w lista wyrażeń musi być typu `int`, `uint`, `long`, lub `ulong`, lub niejawnie konwertowane na co najmniej jeden z tych typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="1b6fe-1299">Wartość każde wyrażenie określa długość odpowiedniego wymiaru w wystąpieniu nowo przydzielonego tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="1b6fe-1300">Ponieważ długość tablicy drugiego wymiaru musi być nieujemna, jest błąd kompilacji, aby *constant_expression* z ujemną wartością na liście wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="1b6fe-1301">Z wyjątkiem składowych w niebezpiecznym kontekście ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)), układ macierzy jest nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="1b6fe-1302">Jeśli wyrażenie tworzenia tablicy pierwszy formularz zawiera inicjatora tablicy, każde wyrażenie w lista wyrażeń musi być stałą i rangi i wymiar długości określonej przez lista wyrażeń musi być zgodne z nazwami inicjatora tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="1b6fe-1303">Wyrażenie tworzenia tablicy drugi lub trzeci formularz ranga specyfikatora typu, lub rangę określonej tablicy musi odpowiadać w przypadku inicjatora tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="1b6fe-1304">Długości wymiarów poszczególnych są dedukowane z liczbą elementów w każdej z odpowiadającymi im poziomami zagnieżdżenia inicjatora tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="1b6fe-1305">W związku z tym wyrażenie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="1b6fe-1306">dokładnie odpowiada</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="1b6fe-1307">Wyrażenie tworzenia tablicy trzeci formularza jest określany jako ***niejawnie wpisane wyrażenie tworzenia tablicy***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="1b6fe-1308">Jest on podobny do drugiej formy, z tą różnicą, że typ elementu tablicy nie zostanie jawnie podana, ale uznane za najlepiej wspólnego typu ([znajdowanie najlepszy typ wspólnego zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) zestawu wyrażeń w tablicy Inicjator.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="1b6fe-1309">Dla tablicy wielowymiarowej czyli takie, gdzie *rank_specifier* zawiera co najmniej jeden przecinek, ten zestaw zawiera wszystkie *wyrażenie*s znaleziony w zagnieżdżonych *array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="1b6fe-1310">Inicjatory tablic są dokładniejszym opisem zawartym w [Array inicjatory](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="1b6fe-1311">Wynik obliczania wartości wyrażenie tworzenia tablicy jest klasyfikowany jako wartości, to znaczy odwołanie do wystąpienia nowo przydzielonego tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="1b6fe-1312">Przetwarzanie środowiska wykonawczego wyrażenie tworzenia tablicy składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-1313">Wymiar wyrażenia długości *expression_list* są obliczane w kolejności od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="1b6fe-1314">Po dokonaniu oceny każde wyrażenie niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) jest wykonywane na jeden z następujących typów: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="1b6fe-1315">Pierwszy typ na tej liście, dla której istnieje niejawna konwersja jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="1b6fe-1316">Jeśli Szacowanie wyrażenia lub kolejnych niejawnej konwersji powoduje wyjątek, nie dalsze wyrażenia są obliczane i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1317">Wartości obliczane dla długości wymiarów są weryfikowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="1b6fe-1318">Jeśli jeden lub więcej wartości jest mniejsza od zera, `System.OverflowException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1319">Wystąpienie tablicy o długości danego wymiaru jest przydzielany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="1b6fe-1320">Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia `System.OutOfMemoryException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="1b6fe-1321">Wszystkie elementy w nowym wystąpieniu tablicy są inicjowane do wartości domyślnych ([wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="1b6fe-1322">Jeśli wyrażenie tworzenia tablicy zawiera inicjatora tablicy, każde wyrażenie w inicjatorze tablicy jest obliczane i przypisane do odpowiadającego mu elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="1b6fe-1323">Wersje ewaluacyjne i przydziały są wykonywane w kolejności wyrażenia są zapisywane w inicjatorze tablicy — innymi słowy, elementy są inicjowane rosnąco indeks, z tym wymiarem po prawej stronie najpierw zwiększyć.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="1b6fe-1324">Jeśli Szacowanie danego wyrażenia lub następne przypisanie do odpowiedniego elementu tablicy, powoduje wyjątek, następnie nie dodatkowe elementy są inicjowane (i wszystkie pozostałe elementy dlatego mają wartości domyślne).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="1b6fe-1325">Wyrażenie tworzenia tablicy pozwala na tworzenie wystąpienia tablicę z elementami typu tablicowego, ale elementy takie tablicy musi zostać zainicjowana ręcznie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="1b6fe-1326">Na przykład instrukcja</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="1b6fe-1327">tworzy tablicę jednowymiarową elementami 100 typu `int[]`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="1b6fe-1328">Początkowa wartość każdy element jest `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="1b6fe-1329">Nie jest możliwe dla tego samego wyrażenie tworzenia tablicy również utworzyć podrzędne tablice i — instrukcja</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="1b6fe-1330">spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1330">results in a compile-time error.</span></span> <span data-ttu-id="1b6fe-1331">Podczas tworzenia wystąpienia podrzędne tablic zamiast tego muszą być wykonywane ręcznie, jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="1b6fe-1332">Gdy tablicy tablic ma kształt "prostokątna", to znaczy, gdy podrzędne tablice są wszystkie tę samą długość, jest bardziej wydajne, aby używać tablicy wielowymiarowej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="1b6fe-1333">W powyższym przykładzie, podczas tworzenia wystąpienia tablicy tablic tworzy obiekty 101 — jedna tablica zewnętrzne i 100 podrzędne tablice.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="1b6fe-1334">Z kolei</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="1b6fe-1335">tworzy tylko jeden obiekt dwuwymiarową tablicę i wykonuje alokacji w pojedynczej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="1b6fe-1336">Poniżej przedstawiono przykłady niejawnie typizowana tablica tworzenia wyrażeń:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="1b6fe-1337">Ostatnie wyrażenie powoduje błąd w czasie kompilacji, ponieważ ani `int` ani `string` jest niejawnie konwertowany do drugiego, a więc nie najlepiej często wpisz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="1b6fe-1338">Wyrażenie tworzenia tablicy jawnie wpisanych musi być używane w tym przypadku, na przykład określenie typu, który ma być `object[]`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="1b6fe-1339">Alternatywnie jeden z elementów mogą być rzutowane na wspólnej typ podstawowy, który następnie stałyby typu wywnioskowanego elementu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="1b6fe-1340">Niejawnie typizowana tablica tworzenia wyrażenia może być łączone z inicjatorach obiektu anonimowego ([wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions)) do utworzenia anonimowo wpisane struktur danych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="1b6fe-1341">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="1b6fe-1342">Wyrażenia tworzenia delegata</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1342">Delegate creation expressions</span></span>

<span data-ttu-id="1b6fe-1343">A *delegate_creation_expression* służy do tworzenia nowego wystąpienia *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="1b6fe-1344">Argument wyrażenia tworzenia delegata musi być grupy metody, funkcją anonimową lub wartość dowolnego typu czasu kompilacji `dynamic` lub *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="1b6fe-1345">Jeśli argument jest grupa metod, określa on metodę i dla metody wystąpienia, obiektu, dla której chcesz utworzyć delegata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="1b6fe-1346">Jeśli argument jest funkcją anonimową bezpośrednio definiuje parametry i treści metody docelowej delegata.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="1b6fe-1347">Jeśli argument jest wartością identyfikuje wystąpienie delegata, o której chcesz utworzyć kopię.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="1b6fe-1348">Jeśli *wyrażenie* ma typ kompilacji `dynamic`, *delegate_creation_expression* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) i poniższymi regułami są stosowane w czasie wykonywania za pomocą typu run-time z *wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="1b6fe-1349">W przeciwnym razie zasady są stosowane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="1b6fe-1350">Powiązania w czasie przetwarzania *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* i `E` jest *wyrażenia* , składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-1351">Jeśli `E` jest grupą metoda wyrażenie tworzenia delegata jest przetwarzana w taki sam sposób jak konwersja grup — metoda ([metody konwersji grup](conversions.md#method-group-conversions)) z `E` do `D`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="1b6fe-1352">Jeśli `E` jest funkcją anonimową wyrażenie tworzenia delegata jest przetwarzana w taki sam sposób jak konwersję funkcja anonimowa ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)) z `E` do `D`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="1b6fe-1353">Jeśli `E` jest wartością, `E` musi być zgodny ([delegować deklaracje](delegates.md#delegate-declarations)) za pomocą `D`, a wynik jest odwołaniem do nowo utworzony delegat typu `D` odwołujący się do tego samego wywołania listy jako `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="1b6fe-1354">Jeśli `E` nie jest zgodny z `D`, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="1b6fe-1355">Przetwarzanie środowiska wykonawczego *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* i `E` jest *wyrażenia* , składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="1b6fe-1356">Jeśli `E` jest grupą metoda wyrażenie tworzenia delegata jest oceniane jako metoda konwersji grupy ([metody konwersji grup](conversions.md#method-group-conversions)) z `E` do `D`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="1b6fe-1357">Jeśli `E` jest funkcją anonimową tworzenia delegata jest oceniane jako funkcja anonimowa konwersja `E` do `D` ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="1b6fe-1358">Jeśli `E` jest wartością *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="1b6fe-1359">`E` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1359">`E` is evaluated.</span></span> <span data-ttu-id="1b6fe-1360">Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="1b6fe-1361">Jeśli wartość `E` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="1b6fe-1362">Nowe wystąpienie typu delegata `D` jest przydzielony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="1b6fe-1363">Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia `System.OutOfMemoryException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="1b6fe-1364">Nowe wystąpienie delegata jest inicjowany za pomocą tej samej listy wywołań jako wystąpienie delegata, określone przez `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="1b6fe-1365">Listy wywołań obiektu delegowanego jest ustalany w chwili delegata zostanie uruchomiony, a następnie pozostaje stała przez cały okres istnienia obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="1b6fe-1366">Innymi słowy nie jest możliwa zmiana jednostek wywoływalnej docelowego delegata po jego utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="1b6fe-1367">Gdy dwa delegaty są połączone lub jeden zostanie usunięta z innego ([delegować deklaracje](delegates.md#delegate-declarations)), nowe delegowanie wyniki; nie istniejące pełnomocnik ma jego zawartość, zmienione.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="1b6fe-1368">Nie jest możliwe do utworzenia delegata, która odwołuje się do właściwości, indeksatora, operatora zdefiniowanego przez użytkownika, Konstruktor wystąpienia, destruktor lub Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="1b6fe-1369">Zgodnie z powyższym opisem kiedy delegat jest tworzony z grupy metod formalnej listy parametrów i zwracany typ delegata określenie przeciążonych metod, aby wybrać.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="1b6fe-1370">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1370">In the example</span></span>
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
<span data-ttu-id="1b6fe-1371">`A.f` pola jest inicjowany z delegatem, który odwołuje się do drugiego `Square` metody, ponieważ ta metoda dokładnie odpowiada formalnej listy parametrów i zwracanym typem `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="1b6fe-1372">Gdyby drugi `Square` metody, które nie są obecne, będzie mieć wystąpił błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="1b6fe-1373">Anonimowy obiekt tworzenia wyrażeń</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="1b6fe-1374">*Anonymous_object_creation_expression* służy do tworzenia obiektu typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="1b6fe-1375">Inicjator obiektu anonimowego deklaruje typu anonimowego i zwraca wystąpienie tego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="1b6fe-1376">Typ anonimowy jest typem klasy pustego, który dziedziczy bezpośrednio z `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="1b6fe-1377">Elementy członkowskie typu anonimowego są sekwencję właściwości tylko do odczytu, wnioskowany z Inicjator obiektu anonimowego użyty do utworzenia wystąpienia typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="1b6fe-1378">W szczególności Inicjator obiektu anonimowego formularza</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="1b6fe-1379">deklaruje typu anonimowego formularza</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="1b6fe-1380">gdzie każdy `Tx` jest typu odpowiedniego wyrażenia `ex`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="1b6fe-1381">Wyrażenie użyte w *member_declarator* musi mieć typ.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="1b6fe-1382">Dlatego jest to błąd kompilacji w wyrażeniu w *member_declarator* jako wartość null lub jest funkcją anonimową.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="1b6fe-1383">Jest również błąd kompilacji, aby wyrażenie ma niezabezpieczonego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="1b6fe-1384">Nazwy typu anonimowego i parametru, aby jego `Equals` metody są automatycznie generowane przez kompilator i nie może się odwoływać tekstu programu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="1b6fe-1385">W tym samym programie dwóch inicjatory obiekt anonimowy, które określają sekwencję właściwości tych samych nazw i typów w czasie kompilacji w tej samej kolejności dadzą wystąpień tego samego typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="1b6fe-1386">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="1b6fe-1387">przypisanie w ostatnim wierszu jest dozwolona, ponieważ `p1` i `p2` są tego samego typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="1b6fe-1388">`Equals` i `GetHashcode` metody dla typów anonimowych zastąpienia metody dziedziczone z `object`i są definiowane w kategoriach `Equals` i `GetHashcode` właściwości, tak aby dwóch wystąpień tego samego typu anonimowego są takie same Jeśli i tylko wtedy, gdy ich właściwości są takie same.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="1b6fe-1389">Deklarator składowej można stosować skrót do prostą nazwą ([wnioskowanie o typie](expressions.md#type-inference)), dostęp do elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), dostępu bazowego ([podstawowa dostępu](expressions.md#base-access)) lub dostęp do elementu członkowskiego warunkowe null ([wyrażenia warunkowe Null jako rzutowanie inicjatorach](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="1b6fe-1390">Jest to nazywane ***inicjatora projekcji*** i jest skrótem deklarację i przypisanie do właściwości o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="1b6fe-1391">W szczególności deklaratory członków formularzy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="1b6fe-1392">odpowiadają dokładnie następujące polecenie, odpowiednio:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="1b6fe-1393">W związku z tym, w przypadku inicjatora projekcji *identyfikator* wybiera zarówno wartość, jak i pola lub właściwości, do którego jest przypisana wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="1b6fe-1394">Intuicyjnie inicjatora projekcji projektów nie tylko wartości, ale również nazwę wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="1b6fe-1395">Typeof — operator</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1395">The typeof operator</span></span>

<span data-ttu-id="1b6fe-1396">`typeof` Operator jest używany do uzyskiwania `System.Type` obiektu dla typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="1b6fe-1397">Pierwszy formularz *typeof_expression* składa się z `typeof` — słowo kluczowe, a następnie ujęty w nawiasy *typu*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="1b6fe-1398">Jest wynikiem wyrażenia tego formularza `System.Type` obiektu dla wskazanego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="1b6fe-1399">Jest tylko jedna `System.Type` obiektu dla danego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="1b6fe-1400">Oznacza to, że dla typu `T`, `typeof(T) == typeof(T)` ma zawsze wartość true.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="1b6fe-1401">*Typu* nie może być `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="1b6fe-1402">Drugiej formy *typeof_expression* składa się z `typeof` — słowo kluczowe, a następnie ujęty w nawiasy *unbound_type_name*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="1b6fe-1403">*Unbound_type_name* jest bardzo podobny do *type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) z tą różnicą, że *unbound_type_name* zawiera *generic_dimension_specifier*s gdzie *type_name* zawiera *type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="1b6fe-1404">Gdy argument operacji *typeof_expression* to sekwencja tokenów spełnia gramatyki zarówno *unbound_type_name* i *type_name*, to znaczy jeśli zawiera on ani *generic_dimension_specifier* ani *type_argument_list*, sekwencja tokenów jest uważany za *type_name*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="1b6fe-1405">Znaczenie *unbound_type_name* jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="1b6fe-1406">Konwertuj sekwencja tokenów *type_name* , zastępując każdego *generic_dimension_specifier* z *type_argument_list* mających taką samą liczbę przecinkami i słowo kluczowe `object` ponieważ do każdego *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="1b6fe-1407">Oceń wynikowy *type_name*, podczas ignoruje wszystkie ograniczenia parametru typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="1b6fe-1408">*Unbound_type_name* jest rozpoznawana jako ogólnego typu niepowiązanego skojarzone z wynikowego skonstruowanego typu ([powiązane i wskazanych typów](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="1b6fe-1409">Wynik *typeof_expression* jest `System.Type` obiekt wynikowy niepowiązany typ ogólny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="1b6fe-1410">Trzecia formą *typeof_expression* składa się z `typeof` — słowo kluczowe, a następnie ujęty w nawiasy `void` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="1b6fe-1411">Jest wynikiem wyrażenia tego formularza `System.Type` obiekt, który reprezentuje braku typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="1b6fe-1412">Typ obiektu zwracanego przez `typeof(void)` różni się od obiektu typu zwracane dla dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="1b6fe-1413">Ten obiekt specjalny typ przydaje się w bibliotekach klas, umożliwiające odbicia do metody w języku, gdzie chcesz tych metod, aby sposobem reprezentowania zwracany typ dowolnej metody, w tym metody void, z wystąpieniem `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="1b6fe-1414">`typeof` Operator może być używany dla parametru typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="1b6fe-1415">Wynik jest `System.Type` obiektu dla typu run-time, który był powiązany z parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="1b6fe-1416">`typeof` Operator może również służyć skonstruowanego typu lub niepowiązanych typu ogólnego ([powiązane i wskazanych typów](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="1b6fe-1417">`System.Type` Obiektu dla typu niepowiązanego dla ogólnego nie jest taka sama jak `System.Type` obiektu typu wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="1b6fe-1418">Typ wystąpienia jest zawsze zamknięte skonstruowanego typu w czasie wykonywania więc jego `System.Type` obiekt jest zależny od argumentów typu run-time w użyciu, podczas niepowiązanych typu ogólnego nie ma argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="1b6fe-1419">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1419">The example</span></span>
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
<span data-ttu-id="1b6fe-1420">generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1420">produces the following output:</span></span>
```
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

<span data-ttu-id="1b6fe-1421">Należy pamiętać, że `int` i `System.Int32` tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="1b6fe-1422">Należy również zauważyć, że wynik `typeof(X<>)` nie zależy od argumentu typu, ale wynik `typeof(X<T>)` jest.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="1b6fe-1423">Operatory checked i unchecked</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="1b6fe-1424">`checked` i `unchecked` operatory są używane do sterowania ***przepełnienia, sprawdzając kontekst*** typu całkowitego operacje arytmetyczne i konwersje.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="1b6fe-1425">`checked` Operator oblicza wyrażenie zawarte w kontekście sprawdzanym i `unchecked` operator oblicza wyrażenie zawarte w kontekście niesprawdzanym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="1b6fe-1426">A *checked_expression* lub *unchecked_expression* odpowiadają dokładnie *parenthesized_expression* ([wyrażeniach z nawiasami](expressions.md#parenthesized-expressions)), z tą różnicą, że zawarte wyrażenie jest obliczane w danym przepełnienia, sprawdzając kontekst.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="1b6fe-1427">Przepełnienie, sprawdzając kontekst można także kontrolować za pomocą `checked` i `unchecked` instrukcji ([instrukcji checked i unchecked](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="1b6fe-1428">Następujące operacje dotyczy przepełnienia, sprawdzając kontekst ustanowione przez `checked` i `unchecked` operatory i instrukcje:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="1b6fe-1429">Wstępnie zdefiniowane `++` i `--` operatorów jednoargumentowych ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators) i [przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)), gdy argument jest całkowitoliczbowego Typ.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="1b6fe-1430">Wstępnie zdefiniowane `-` operatora jednoargumentowego ([jednoargumentowy minus operator](expressions.md#unary-minus-operator)), gdy argument jest typu całkowitego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="1b6fe-1431">Wstępnie zdefiniowane `+`, `-`, `*`, i `/` operatory binarne ([operatorów arytmetycznych](expressions.md#arithmetic-operators)), gdy oba operandy są typu całkowitoliczbowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="1b6fe-1432">Jawnych konwersji liczbowych ([jawnych konwersji liczbowych](conversions.md#explicit-numeric-conversions)) z jednego typu całkowitego, do innego typu całkowitego lub z `float` lub `double` na typ całkowitoliczbowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="1b6fe-1433">Po jednej z powyższych operacji rezultatów, jest zbyt duży do reprezentowania w typie docelowym, kontekst, w którym ta operacja jest wykonywane formantów wynikowe zachowania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="1b6fe-1434">W `checked` kontekstu, jeśli operacja jest wyrażeniem stałym ([wyrażeń stałych](expressions.md#constant-expressions)), wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="1b6fe-1435">W przeciwnym razie, gdy operacja jest wykonywana w czasie wykonywania `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="1b6fe-1436">W `unchecked` kontekstu, wynik zostanie obcięta przez odrzucenie wszelkich najbardziej znaczących bitów, które nie mieszczą się w typie docelowym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="1b6fe-1437">Niestałe wyrażeń (wyrażeń, które są oceniane w czasie wykonywania), nie są ujęte w jakikolwiek `checked` lub `unchecked` operatorów lub instrukcji przepełnienie domyślne sprawdzanie kontekstu jest `unchecked` , chyba że czynniki zewnętrzne, (takie jak kompilator przełączniki i konfiguracji środowiska wykonywania) wymagają `checked` oceny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="1b6fe-1438">Wyrażenia stałe (wyrażenia, które mogą być w pełni obliczane w czasie kompilacji), przepełnienie domyślne sprawdzanie kontekstu jest zawsze `checked`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="1b6fe-1439">Chyba, że wyrażenie stałe jest jawnie umieszczona na `unchecked` kontekstu, przepełnienia, które występują podczas kompilacji obliczenia wyrażenia zawsze powodują błędy czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="1b6fe-1440">Treść funkcją anonimową nie mają wpływu `checked` lub `unchecked` kontekstów, w których występuje funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="1b6fe-1441">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1441">In the example</span></span>
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
<span data-ttu-id="1b6fe-1442">są zgłaszane żadne błędy kompilacji, ponieważ żadna z wyrażeń nie mogą być obliczane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="1b6fe-1443">W czasie wykonywania `F` metoda zgłasza wyjątek `System.OverflowException`i `G` metoda zwraca-727379968 (niższe 32 bity wyniku spoza zakresu).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="1b6fe-1444">Zachowanie `H` metoda zależy od przepełnienie domyślne, sprawdzając kontekst dla kompilacji, ale jest taki sam jak `F` lub taka sama jak `G`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="1b6fe-1445">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1445">In the example</span></span>
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
<span data-ttu-id="1b6fe-1446">przepełnienia, występujące podczas obliczania wyrażenia stałe w `F` i `H` powodują błędy czasu kompilacji, należy podać, ponieważ wyrażenia są obliczane w `checked` kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="1b6fe-1447">Również występuje przepełnienie podczas obliczania wyrażenia stałe w `G`, ale ponieważ obliczanie odbywa się w `unchecked` kontekstu, przeciążenia nie został zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="1b6fe-1448">`checked` i `unchecked` operatory wpływ tylko na przepełnienie, sprawdzając kontekst dla tych operacji, które są w formie tekstu zawarte w "`(`"i"`)`" tokenów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="1b6fe-1449">Operatory nie mają wpływu na elementach członkowskich funkcji, które są wywoływane w wyniku obliczenia wyrażenia zawarte.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="1b6fe-1450">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1450">In the example</span></span>
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
<span data-ttu-id="1b6fe-1451">Korzystanie z `checked` w `F` nie ma wpływu na ocenę `x * y` w `Multiply`, więc `x * y` jest oceniany w przepełnienie domyślne sprawdzanie kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="1b6fe-1452">`unchecked` Operator jest wygodne, podczas zapisywania stałe podpisanych typów całkowitych w formacie szesnastkowym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="1b6fe-1453">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="1b6fe-1454">Stałe szesnastkowe powyżej są typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="1b6fe-1455">Ponieważ stałe są poza `int` zakresu bez `unchecked` operatorów, rzutowania na `int` dałby w efekcie błędy czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="1b6fe-1456">`checked` i `unchecked` operatory i instrukcje umożliwiają deweloperom kontrolować niektóre aspekty niektórych obliczeń numerycznych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="1b6fe-1457">Zachowanie niektóre operatory numeryczne zależy jednak od tego, jaki ich operandami typów danych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="1b6fe-1458">Na przykład, mnożenie dwóch miejsc po przecinku zawsze powoduje wyjątek przy przepełnieniu nawet w sposób jawny `unchecked` konstruowania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="1b6fe-1459">Podobnie, dwa pomnożenie liczby zmiennoprzecinkowe nigdy nie wyników wyjątek przy przepełnieniu nawet w sposób jawny `checked` konstruowania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="1b6fe-1460">Ponadto inne operatory nigdy nie ma wpływ tryb sprawdzania, czy domyślne lub jawny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="1b6fe-1461">Wyrażenia wartości domyślnych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1461">Default value expressions</span></span>

<span data-ttu-id="1b6fe-1462">Wyrażenie wartości domyślnej jest używana do uzyskiwania wartości domyślne ([wartości domyślne](variables.md#default-values)) typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="1b6fe-1463">Zazwyczaj wyrażenie wartości domyślnej jest używana dla parametrów typu, ponieważ może być znane nie, jeśli parametr typu jest typem wartości lub typem referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="1b6fe-1464">(Nie istnieje konwersja z `null` literału z parametrem typu, chyba że parametr typu jest znany jako typ odwołania.)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="1b6fe-1465">Jeśli *typu* w *default_value_expression* obliczane w czasie wykonywania do typu odwołania, wynik jest `null` konwertowane do tego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="1b6fe-1466">Jeśli *typu* w *default_value_expression* ocenia w czasie wykonywania na typ wartości, wynik jest *value_type*jego wartość domyślna ([domyślne Konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="1b6fe-1467">A *default_value_expression* jest wyrażeniem stałym ([wyrażeń stałych](expressions.md#constant-expressions)) Jeśli typ jest typem referencyjnym lub parametr typu, który jest znany jako typu odwołania ([parametr typu ograniczenia](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="1b6fe-1468">Ponadto *default_value_expression* jest wyrażeniem stałym, jeśli typ jest jednym z następujących typów wartości: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, lub dowolny typ wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="1b6fe-1469">Wyrażenie Nameof</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1469">Nameof expressions</span></span>

<span data-ttu-id="1b6fe-1470">A *nameof_expression* służy do uzyskiwania nazwy jednostki programu jako ciąg stałej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="1b6fe-1471">Gramatycznie rzecz biorąc, *named_entity* operand jest zawsze wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="1b6fe-1472">Ponieważ `nameof` nie jest zastrzeżonym słowem kluczowym wyrażenie nameof ma zawsze wartość składniowo niejednoznaczne za pomocą wywołania prostą nazwę `nameof`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="1b6fe-1473">Ze względu na zgodność, jeśli wyszukiwanie nazw ([proste nazwy](expressions.md#simple-names)) o nazwie `nameof` zakończy się powodzeniem, wyrażenie jest traktowane *invocation_expression* — niezależnie od tego, czy jest wywołanie prawne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="1b6fe-1474">W przeciwnym razie jest *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="1b6fe-1475">Znaczenie *named_entity* z *nameof_expression* jest znaczenie go jako wyrażenie; czyli albo jako *simple_name*, *base_access*  lub *member_access*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="1b6fe-1476">Jednak gdy wyszukiwanie opisany w [proste nazwy](expressions.md#simple-names) i [dostęp do elementu członkowskiego](expressions.md#member-access) powoduje błąd, ponieważ znaleziono składową wystąpienia w ramach kontekstu statycznego *nameof_expression*powoduje nie takich błąd.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="1b6fe-1477">Jest to błąd czasu kompilacji dla *named_entity* wyznaczanie grupy metod, aby *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="1b6fe-1478">Jest to błąd czasu kompilacji dla *named_entity_target* typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="1b6fe-1479">A *nameof_expression* jest wyrażeniem stałym typu `string`, nie ma zastosowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="1b6fe-1480">W szczególności jego *named_entity* nie jest oceniany i jest ignorowane dla celów analizy asercję określonego przypisania ([ogólne reguły dotyczące wyrażeń prostych](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="1b6fe-1481">Jej wartość jest ostatni identyfikator *named_entity* przed końcowym znakiem opcjonalne *type_argument_list*, przekształcone w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="1b6fe-1482">Prefiks "`@`", jeśli używany, zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="1b6fe-1483">Każdy *unicode_escape_sequence* jest przekształcana na jego odpowiedniego znaku Unicode.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="1b6fe-1484">Wszelkie *formatting_characters* są usuwane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="1b6fe-1485">Są one stosowane przekształcenia [identyfikatory](lexical-structure.md#identifiers) podczas testowania równości pomiędzy identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="1b6fe-1486">TODO: przykłady</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="1b6fe-1487">Wyrażenia metody anonimowej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1487">Anonymous method expressions</span></span>

<span data-ttu-id="1b6fe-1488">*Anonymous_method_expression* jest jeden z dwóch sposobów definiowania funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="1b6fe-1489">Te ustawienia zostały opisane dalej w [wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="1b6fe-1490">Operatory jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1490">Unary operators</span></span>

<span data-ttu-id="1b6fe-1491">`?`, `+`, `-`, `!`, `~`, `++`, `--`, Multiemisji, i `await` operatory są nazywane operatorami jednoargumentowymi.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="1b6fe-1492">Jeśli argument operacji *unary_expression* ma typ kompilacji `dynamic`, dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-1493">W tym przypadku typ czasu kompilacji *unary_expression* jest `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="1b6fe-1494">Operatorów warunkowych działających z wartością null</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1494">Null-conditional operator</span></span>

<span data-ttu-id="1b6fe-1495">Operatorów warunkowych działających z wartością null dotyczy listy operacji argumentem operacji tylko wtedy, gdy ten argument jest różna od null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="1b6fe-1496">W przeciwnym razie wynik zastosowania operatora jest `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="1b6fe-1497">Lista operacji mogą obejmować dostęp do elementu członkowskiego i operacje dostępu do elementu, (które mogą być warunkowe null), a także wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="1b6fe-1498">Na przykład, wyrażenie `a.b?[0]?.c()` jest *null_conditional_expression* z *primary_expression* `a.b` i *null_conditional_operations* `?[0]` (dostęp do elementu warunkowe null), `?.c` (dostęp do składowej warunkowe null) i `()` (wywołanie).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="1b6fe-1499">Dla *null_conditional_expression* `E` z *primary_expression* `P`systemowi `E0` być wyrażeniem uzyskane w formie tekstu, usuwając wiodących `?`z każdego *null_conditional_operations* z `E` mieć jeden.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="1b6fe-1500">Model `E0` jest wyrażeniem, które zostanie obliczone, jeśli żadne sprawdzanie wartości null, reprezentowane przez `?`uważają, że s `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="1b6fe-1501">Ponadto poinformuj `E1` być wyrażeniem uzyskane w formie tekstu, usuwając wiodące `?` z tylko pierwszy *null_conditional_operations* w `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="1b6fe-1502">Może to prowadzić do *wyrażenia podstawowe* (jeśli istnieje tylko jeden `?`) lub do innego *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="1b6fe-1503">Na przykład jeśli `E` jest wyrażeniem `a.b?[0]?.c()`, następnie `E0` jest wyrażeniem `a.b[0].c()` i `E1` jest wyrażeniem `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="1b6fe-1504">Jeśli `E0` zostanie sklasyfikowany jako nothing, następnie `E` zostanie sklasyfikowany jako nothing.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="1b6fe-1505">W przeciwnym razie E jest klasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="1b6fe-1506">`E0` i `E1` służą do określania znaczenie `E`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="1b6fe-1507">Jeśli `E` występuje jako *statement_expression* znaczenie `E` jest taka sama jak instrukcji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="1b6fe-1508">z tą różnicą, że P jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="1b6fe-1509">W przeciwnym razie, jeśli `E0` zostanie sklasyfikowany jako nic nie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="1b6fe-1510">W przeciwnym razie umożliwiają `T0` typem `E0`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="1b6fe-1511">Jeśli `T0` jest parametrem typu, który nie jest znany jako typem referencyjnym lub typem wartościowym niebędącym występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="1b6fe-1512">Jeśli `T0` jest typem wartości niedopuszczającym wartości, a następnie typ `E` jest `T0?`oraz na znaczenie właściwości `E` jest taka sama jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="1b6fe-1513">z tą różnicą, że `P` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="1b6fe-1514">W przeciwnym razie typ E jest T0 i znaczenie E jest taka sama jak</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="1b6fe-1515">z tą różnicą, że `P` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="1b6fe-1516">Jeśli `E1` sama *null_conditional_expression*, następnie te reguły są stosowane ponownie zagnieżdżania testów dla `null` aż nie wystąpią żadnych dalszych `?`firmy, i w dół do końca została zmniejszona wyrażenie wyrażenia podstawowe `E0`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="1b6fe-1517">Na przykład jeśli wyrażenie `a.b?[0]?.c()` występuje jako instrukcja wyrażenie, tak jak w instrukcji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="1b6fe-1518">jego znaczenie jest równoważne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="1b6fe-1519">co ponownie odpowiada:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="1b6fe-1520">Z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="1b6fe-1521">Jeśli pojawi się w kontekście, gdy jej wartość jest używana, w:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="1b6fe-1522">i przy założeniu, że typ końcowe wywołanie nie jest typem wartości niedopuszczającym wartości, jego znaczenie jest równoważne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="1b6fe-1523">Z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="1b6fe-1524">Wyrażenia warunkowe null jako inicjatory projekcji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="1b6fe-1525">Wyrażenie warunkowe null jest dozwolony tylko jako *member_declarator* w *anonymous_object_creation_expression* ([wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions)) Jeśli kończy się ona dostęp do elementu członkowskiego (opcjonalnie warunkowe null).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="1b6fe-1526">Gramatycznie ten wymóg może być wyrażona jako:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="1b6fe-1527">Jest to szczególny przypadek gramatyka dla *null_conditional_expression* powyżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="1b6fe-1528">Produkcji dla *member_declarator* w [wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions) następnie zawiera tylko *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="1b6fe-1529">Wyrażenia warunkowe null jako wyrażenia instrukcji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="1b6fe-1530">Wyrażenie warunkowe null jest dozwolony tylko jako *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)), jeśli kończy się je za pomocą wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="1b6fe-1531">Gramatycznie ten wymóg może być wyrażona jako:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="1b6fe-1532">Jest to szczególny przypadek gramatyka dla *null_conditional_expression* powyżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="1b6fe-1533">Produkcji dla *statement_expression* w [instrukcje wyrażeń](statements.md#expression-statements) następnie zawiera tylko *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="1b6fe-1534">Jednoargumentowy operator plus</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1534">Unary plus operator</span></span>

<span data-ttu-id="1b6fe-1535">Dla operacji formularza `+x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1536">Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="1b6fe-1537">Wstępnie zdefiniowane jednoargumentowe plus operatory są następujące:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="1b6fe-1538">Dla każdej z tych operatorów wynik jest po prostu wartość operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="1b6fe-1539">Jednoargumentowy operator minus</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1539">Unary minus operator</span></span>

<span data-ttu-id="1b6fe-1540">Dla operacji formularza `-x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1541">Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="1b6fe-1542">Operatory negacji wstępnie zdefiniowane są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="1b6fe-1543">Liczba całkowita negacji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="1b6fe-1544">Wynik jest obliczana przez odjęcie `x` od zera.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="1b6fe-1545">Jeśli wartość elementu `x` jest najmniejszą wartość stałego typu operand (-2 ^ 31 `int` lub -2 ^ 63 dla `long`), następnie matematyczne negację `x` nie jest reprezentowanych w ramach typu operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1545">If the value of of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="1b6fe-1546">Jeśli ten problem wystąpi w ramach `checked` kontekstu, `System.OverflowException` zgłaszany; jeśli jego wystąpieniu w ciągu `unchecked` kontekstu, wynikiem jest wartość operandu i przeciążenia nie został zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="1b6fe-1547">Jeśli argument operator negacji jest typu `uint`, jest konwertowany na typ `long`, a typ wyniku jest `long`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="1b6fe-1548">Wyjątek stanowi regułę, która pozwala na `int` wartość -2147483648 (-2 ^ 31) do zapisania jako dziesiętna liczba całkowita literału ([literały całkowite](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="1b6fe-1549">Jeśli argument operator negacji jest typu `ulong`, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="1b6fe-1550">Wyjątek stanowi regułę, która pozwala na `long` wartość wartość -9223372036854775808 (-2 ^ 63) do zapisania jako dziesiętna liczba całkowita literału ([literały całkowite](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="1b6fe-1551">Zmiennoprzecinkowe negacji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="1b6fe-1552">Wynik jest wartością `x` przy użyciu konta, jego odwrócony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="1b6fe-1553">Jeśli `x` jest wartością typu NaN, wynik jest również wartością typu NaN.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="1b6fe-1554">Negacja dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="1b6fe-1555">Wynik jest obliczana przez odjęcie `x` od zera.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="1b6fe-1556">Negacja dziesiętny jest równoważne użyciu Jednoargumentowy operator typu odejmowania `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="1b6fe-1557">Operator logiczny negacji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1557">Logical negation operator</span></span>

<span data-ttu-id="1b6fe-1558">Dla operacji formularza `!x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1559">Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="1b6fe-1560">Istnieje tylko jeden operator negacji logicznej wstępnie zdefiniowane:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="1b6fe-1561">Ten operator oblicza Negacja logiczna operandu: Jeśli argument jest `true`, wynik jest `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="1b6fe-1562">Jeśli argument jest `false`, wynik jest `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="1b6fe-1563">Operator dopełnienia bitowego</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1563">Bitwise complement operator</span></span>

<span data-ttu-id="1b6fe-1564">Dla operacji formularza `~x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1565">Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="1b6fe-1566">Operatory wstępnie zdefiniowanych dopełnienia bitowego to:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="1b6fe-1567">Dla każdej z tych operatorów, wynik operacji jest uzupełnienie bitowe `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="1b6fe-1568">Każdy typ wyliczenia `E` niejawnie udostępnia następujące operator dopełnienia bitowego:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="1b6fe-1569">Wynik obliczania wartości `~x`, gdzie `x` to wyrażenie typu wyliczeniowego `E` z typu podstawowego `U`, jest dokładnie taka sama jak ocena `(E)(~(U)x)`, z tą różnicą, że konwersja `E` jest zawsze jest wykonywane jako if w `unchecked` kontekstu ([operatory checked i unchecked](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="1b6fe-1570">Przedrostkowe i operatory dekrementacji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="1b6fe-1571">Argument prefiksu inkrementacji lub dekrementacji operacji musi być wyrażeniem sklasyfikowane jako zmienną, dostęp do właściwości lub indeksatora dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="1b6fe-1572">Wynikiem operacji jest wartością tego samego typu jako argumentu operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="1b6fe-1573">Jeśli argument prefiksu Zwiększ operacja dekrementacji jest właściwością lub dostęp indeksatora, właściwość lub indeksator musi mieć zarówno `get` i `set` metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="1b6fe-1574">Jeśli nie jest to możliwe, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-1575">Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1576">Wstępnie zdefiniowane `++` i `--` operatory istnieją dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`i dowolnego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="1b6fe-1577">Wstępnie zdefiniowane `++` operatory zwracają wartość, o których produkowane przez dodanie 1 argument i wstępnie zdefiniowane `--` operatory zwracają wartość, o których produkowane przez odjęcie ilości 1 z operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="1b6fe-1578">W `checked` kontekstu, jeśli wynik to dodawania lub odejmowania jest spoza zakresu typu wyniku, a typ wyniku jest typu całkowitoliczbowego lub typu wyliczeniowego `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="1b6fe-1579">Przetwarzanie środowiska wykonawczego przyrostu prefiks lub zmniejszanie operacji formularza `++x` lub `--x` składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="1b6fe-1580">Jeśli `x` zostanie sklasyfikowany jako zmienną:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="1b6fe-1581">`x` jest oceniany w celu utworzenia zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="1b6fe-1582">Wybrane operator jest wywoływany z wartością `x` jako argumentem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="1b6fe-1583">Wartość zwracana przez operator znajduje się w lokalizacji określonej przez ocenę `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="1b6fe-1584">Wartość zwracana przez operator staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="1b6fe-1585">Jeśli `x` zostanie sklasyfikowany jako właściwość lub indeksator dostępu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="1b6fe-1586">Wyrażenia wystąpienia (Jeśli `x` nie jest `static`) i listą argumentów (Jeśli `x` jest dostęp indeksatora) skojarzony z `x` są oceniane, a wyniki są używane w kolejnych żadaniach `get` i `set` wywołania metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="1b6fe-1587">`get` Akcesor `x` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="1b6fe-1588">Operatora jest wywoływana z wartością zwróconą przez `get` dostępu jako argumentem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="1b6fe-1589">`set` Akcesor `x` jest wywoływana z wartością zwróconą przez operatora jako jego `value` argumentu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="1b6fe-1590">Wartość zwracana przez operator staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="1b6fe-1591">`++` i `--` Operatorzy pomocy technicznej przyrostkowe notacji ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="1b6fe-1592">Zazwyczaj wynikiem `x++` lub `x--` jest wartością `x` przed wykonaniem operacji, podczas gdy wynik `++x` lub `--x` jest wartością `x` po wykonaniu operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="1b6fe-1593">W obu przypadkach `x` ma taką samą wartość po wykonaniu operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="1b6fe-1594">`operator++` Lub `operator--` implementacja może być wywoływany przy użyciu notacji przyrostka lub przedrostka.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="1b6fe-1595">Nie jest możliwe operator oddzielne implementacje dla dwóch notacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="1b6fe-1596">Wyrażenia CAST</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1596">Cast expressions</span></span>

<span data-ttu-id="1b6fe-1597">A *cast_expression* jest używana do jawnie konwersji wyrażenia do danego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="1b6fe-1598">A *cast_expression* formularza `(T)E`, gdzie `T` jest *typu* i `E` jest *unary_expression*, wykonuje jawnego Konwersja ([jawne konwersje](conversions.md#explicit-conversions)) wartości `E` na typ `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="1b6fe-1599">Jeśli istnieje nie jawna konwersja z `E` do `T`, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="1b6fe-1600">W przeciwnym razie wynikiem jest wartość, o których produkowane przez jawną konwersję.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="1b6fe-1601">Wynik jest zawsze klasyfikowane jako wartości, nawet jeśli `E` wskazuje zmienną.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="1b6fe-1602">Gramatyka dla *cast_expression* prowadzi do niektórych niejasności składni.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="1b6fe-1603">Na przykład, wyrażenie `(x)-y` można albo być interpretowane jako *cast_expression* (rzutowanie typu `-y` na typ `x`) lub jako *additive_expression* w połączeniu z *parenthesized_expression* (które oblicza wartość `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="1b6fe-1604">Aby rozwiązać *cast_expression* niejasności, następująca reguła istnieje: Sekwencji jednego lub więcej *tokenu*s ([biały znak](lexical-structure.md#white-space)) ujęta w nawiasy jest traktowana jako początek *cast_expression* tylko wtedy, gdy spełnione są co najmniej jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="1b6fe-1605">Sekwencja tokenów jest poprawny gramatyka dla *typu*, ale nie dla *wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="1b6fe-1606">Sekwencja tokenów jest poprawny gramatyka dla *typu*, i tokenu bezpośrednio po nawiasie zamykającym jest tokenem "`~`", token "`!`", token "`(`",  *Identyfikator* ([sekwencje ucieczki znaków Unicode](lexical-structure.md#unicode-character-escape-sequences)), *literału* ([literały](lexical-structure.md#literals)), lub dowolnego *— słowo kluczowe*([Słowa kluczowe](lexical-structure.md#keywords)) z wyjątkiem `as` i `is`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="1b6fe-1607">Termin "gramatyka poprawne" powyżej oznacza tylko, że sekwencja tokeny muszą być zgodne do konkretnego gramatyczne środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="1b6fe-1608">W szczególności nie uważa rzeczywiste znaczenie żadnych składowych identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="1b6fe-1609">Na przykład jeśli `x` i `y` to identyfikatory, następnie `x.y` jest poprawny gramatyka dla typu, nawet wtedy, gdy `x.y` rzeczywiście nie określa typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="1b6fe-1610">Z reguły uściślania następuje, jeśli `x` i `y` to identyfikatory, `(x)y`, `(x)(y)`, i `(x)(-y)` są *cast_expression*s, ale `(x)-y` nie jest dostępna, nawet wtedy, gdy `x` identyfikuje typ.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="1b6fe-1611">Jednak jeśli `x` jest słowem kluczowym, który identyfikuje wstępnie zdefiniowany typ (takich jak `int`), wówczas są wszystkie cztery formularze *cast_expression*s (ponieważ słów kluczowych nie można prawdopodobnie wyrażenia przez siebie).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="1b6fe-1612">Wyrażeń await</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1612">Await expressions</span></span>

<span data-ttu-id="1b6fe-1613">Operator "await" jest używany do zawieszenia oceny otaczającej funkcji asynchronicznej przed ukończeniem operacji asynchronicznej, reprezentowane przez operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="1b6fe-1614">*Await_expression* jest dozwolona tylko w treści funkcji asynchronicznej ([Iteratory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="1b6fe-1615">W najbliższej otaczającej funkcji asynchronicznej *await_expression* nie może wystąpić w następujących miejscach:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="1b6fe-1616">Wewnątrz funkcji anonimowych zagnieżdżonych (innych niż asynchroniczny)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="1b6fe-1617">Wewnątrz bloku *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="1b6fe-1618">W niebezpiecznym kontekście</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1618">In an unsafe context</span></span>

<span data-ttu-id="1b6fe-1619">Należy pamiętać, że *await_expression* nie może wystąpić w większości miejsc w ramach *query_expression*, ponieważ te składniowo są przekształcane do użycia innego niż asynchronicznych wyrażeń lambda.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="1b6fe-1620">Wewnątrz funkcji asynchronicznej `await` nie można użyć jako identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="1b6fe-1621">Nie ma związku z tym żadnych niejednoznaczności składniowe między wyrażeń await i wyrażenia różnych, dotyczące identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="1b6fe-1622">Poza funkcje asynchroniczne `await` działa jak normalne identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="1b6fe-1623">Argument operacji *await_expression* nosi nazwę ***zadań***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="1b6fe-1624">Reprezentuje operację asynchroniczną, która może być lub może nie być ukończone w czasie *await_expression* jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="1b6fe-1625">Operator "await" ma na celu wstrzymał wykonywanie otaczającej funkcji asynchronicznej, aż oczekiwane zadanie zostało ukończone, a następnie uzyskać jego wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="1b6fe-1626">Oczekujący wyrażeń</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1626">Awaitable expressions</span></span>

<span data-ttu-id="1b6fe-1627">Zadanie wykonywania wyrażenia oczekiwania musi być ***oczekujący***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="1b6fe-1628">Wyrażenie `t` jest oczekujący, jeśli zawiera jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="1b6fe-1629">`t` typu czasu kompilacji `dynamic`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="1b6fe-1630">`t` zawiera dostępnej metody rozszerzenia lub wystąpienia, o nazwie `GetAwaiter` żadnych parametrów i bez parametrów typu i zwracany typ `A` dla którego pomieścić wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="1b6fe-1631">`A` implementuje interfejs `System.Runtime.CompilerServices.INotifyCompletion` (nazywanym dalej `INotifyCompletion` dla zwięzłości)</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="1b6fe-1632">`A` ma właściwość wystąpienia dostępny, można odczytać `IsCompleted` typu `bool`</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="1b6fe-1633">`A` ma dostępną metodą wystąpienia `GetResult` bez parametrów i bez parametrów typu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="1b6fe-1634">Celem `GetAwaiter` metody jest uzyskanie ***awaiter*** dla zadania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="1b6fe-1635">Typ `A` nosi nazwę ***typu awaiter*** wyrażenia await.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="1b6fe-1636">Celem `IsCompleted` właściwość ma na celu określenie, jeśli zadanie zostało już ukończone.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="1b6fe-1637">Jeśli tak, nie ma potrzeby wstrzymania oceny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="1b6fe-1638">Celem `INotifyCompletion.OnCompleted` metodą jest utworzenie "kontynuacja" do zadania; czyli delegata (typu `System.Action`), zostanie wywołana po ukończeniu zadania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="1b6fe-1639">Celem `GetResult` metodą jest można uzyskać wynik zadania po jego zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="1b6fe-1640">Ten wynik może być wykonana pomyślnie, prawdopodobnie z wartością wynik lub może być wyjątek, który jest generowany przez `GetResult` metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="1b6fe-1641">Klasyfikacja wyrażeń await</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1641">Classification of await expressions</span></span>

<span data-ttu-id="1b6fe-1642">Wyrażenie `await t` klasyfikowany jest taki sam sposób jak wyrażenie `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="1b6fe-1643">W związku z tym Jeśli zwracany typ metody `GetResult` jest `void`, *await_expression* zostanie sklasyfikowany jako nothing.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="1b6fe-1644">Jeśli ma on zwracany typ inny niż void `T`, *await_expression* zostanie sklasyfikowany jako wartość typu `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="1b6fe-1645">Środowisko uruchomieniowe oceny wyrażeń await</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="1b6fe-1646">W czasie wykonywania, wyrażenie `await t` jest obliczane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="1b6fe-1647">Awaiter `a` uzyskuje się przez obliczenie wyrażenia `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="1b6fe-1648">A `bool` `b` uzyskuje się przez obliczenie wyrażenia `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="1b6fe-1649">Jeśli `b` jest `false` , a następnie oceny jest zależna od tego, czy `a` implementuje interfejs `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (nazywanym dalej `ICriticalNotifyCompletion` dla zwięzłości).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="1b6fe-1650">To sprawdzenie odbywa się w czasie; wiązania czyli w czasie wykonywania przypadku `a` ma typów w czasie kompilacji `dynamic`i w czasie kompilacji, w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="1b6fe-1651">Pozwól `r` oznaczają delegata wznowienie ([Iteratory](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="1b6fe-1652">Jeśli `a` nie implementuje `ICriticalNotifyCompletion`, następnie wyrażenie `(a as (INotifyCompletion)).OnCompleted(r)` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="1b6fe-1653">Jeśli `a` zaimplementować `ICriticalNotifyCompletion`, następnie wyrażenie `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="1b6fe-1654">Ocena jest następnie wstrzymana, a zwróceniem sterowania do bieżącego obiektu wywołującego funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="1b6fe-1655">Albo natychmiast po (Jeśli `b` został `true`), lub nowszych wywołanie delegata wznawiania (Jeśli `b` został `false`), wyrażenie `(a).GetResult()` jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="1b6fe-1656">Jeśli wartość jest zwracana, ta wartość jest wynikiem *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="1b6fe-1657">W przeciwnym razie wynikiem jest nothing.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="1b6fe-1658">Awaiter implementację metody interfejsu `INotifyCompletion.OnCompleted` i `ICriticalNotifyCompletion.UnsafeOnCompleted` powinno spowodować delegata `r` wywoływanej co najwyżej jeden raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="1b6fe-1659">W przeciwnym razie zachowanie otaczającej funkcji asynchronicznej jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="1b6fe-1660">Operatory arytmetyczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1660">Arithmetic operators</span></span>

<span data-ttu-id="1b6fe-1661">`*`, `/`, `%`, `+`, I `-` operatory są nazywane operatorów arytmetycznych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="1b6fe-1662">Jeśli argument operacji jest operator arytmetyczny ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-1663">W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="1b6fe-1664">Operator mnożenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1664">Multiplication operator</span></span>

<span data-ttu-id="1b6fe-1665">Dla operacji formularza `x * y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1666">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-1667">Operatory mnożenia wstępnie zdefiniowane są wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="1b6fe-1668">Wszystkie operatory Oblicz iloczyn `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="1b6fe-1669">Liczba całkowita mnożenia:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="1b6fe-1670">W `checked` kontekstu, jeśli produkt jest spoza zakresu typu wyniku, `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-1671">W `unchecked` kontekstu, przepełnienia nie są zgłaszane i wszystkie znaczące bity wyższego rzędu spoza zakresu typu wyniku są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="1b6fe-1672">Zmiennoprzecinkowe mnożenia:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="1b6fe-1673">Produkt jest obliczany zgodnie z regułami IEEE 754 arytmetyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="1b6fe-1674">W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="1b6fe-1675">W tabeli `x` i `y` wartości skończoną dodatnią.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="1b6fe-1676">`z` jest wynikiem `x * y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="1b6fe-1677">Jeśli wynik jest za duża dla typu miejsca docelowego `z` jest nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="1b6fe-1678">Jeśli wynik jest za mały dla typu miejsca docelowego `z` wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="1b6fe-1679">+y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1679">+y</span></span>   | <span data-ttu-id="1b6fe-1680">-y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1680">-y</span></span>   | <span data-ttu-id="1b6fe-1681">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1681">+0</span></span>  | <span data-ttu-id="1b6fe-1682">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1682">-0</span></span>  | <span data-ttu-id="1b6fe-1683">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1683">+inf</span></span> | <span data-ttu-id="1b6fe-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1684">-inf</span></span> | <span data-ttu-id="1b6fe-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1685">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-1686">+x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1686">+x</span></span>   | <span data-ttu-id="1b6fe-1687">+z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1687">+z</span></span>   | <span data-ttu-id="1b6fe-1688">-z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1688">-z</span></span>   | <span data-ttu-id="1b6fe-1689">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1689">+0</span></span>  | <span data-ttu-id="1b6fe-1690">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1690">-0</span></span>  | <span data-ttu-id="1b6fe-1691">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1691">+inf</span></span> | <span data-ttu-id="1b6fe-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1692">-inf</span></span> | <span data-ttu-id="1b6fe-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1693">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-1694">-x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1694">-x</span></span>   | <span data-ttu-id="1b6fe-1695">-z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1695">-z</span></span>   | <span data-ttu-id="1b6fe-1696">+z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1696">+z</span></span>   | <span data-ttu-id="1b6fe-1697">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1697">-0</span></span>  | <span data-ttu-id="1b6fe-1698">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1698">+0</span></span>  | <span data-ttu-id="1b6fe-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1699">-inf</span></span> | <span data-ttu-id="1b6fe-1700">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1700">+inf</span></span> | <span data-ttu-id="1b6fe-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1701">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-1702">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1702">+0</span></span>   | <span data-ttu-id="1b6fe-1703">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1703">+0</span></span>   | <span data-ttu-id="1b6fe-1704">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1704">-0</span></span>   | <span data-ttu-id="1b6fe-1705">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1705">+0</span></span>  | <span data-ttu-id="1b6fe-1706">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1706">-0</span></span>  | <span data-ttu-id="1b6fe-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1707">NaN</span></span>  | <span data-ttu-id="1b6fe-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1708">NaN</span></span>  | <span data-ttu-id="1b6fe-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1709">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-1710">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1710">-0</span></span>   | <span data-ttu-id="1b6fe-1711">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1711">-0</span></span>   | <span data-ttu-id="1b6fe-1712">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1712">+0</span></span>   | <span data-ttu-id="1b6fe-1713">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1713">-0</span></span>  | <span data-ttu-id="1b6fe-1714">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1714">+0</span></span>  | <span data-ttu-id="1b6fe-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1715">NaN</span></span>  | <span data-ttu-id="1b6fe-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1716">NaN</span></span>  | <span data-ttu-id="1b6fe-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1717">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-1718">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1718">+inf</span></span> | <span data-ttu-id="1b6fe-1719">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1719">+inf</span></span> | <span data-ttu-id="1b6fe-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1720">-inf</span></span> | <span data-ttu-id="1b6fe-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1721">NaN</span></span> | <span data-ttu-id="1b6fe-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1722">NaN</span></span> | <span data-ttu-id="1b6fe-1723">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1723">+inf</span></span> | <span data-ttu-id="1b6fe-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1724">-inf</span></span> | <span data-ttu-id="1b6fe-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1725">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1726">-inf</span></span> | <span data-ttu-id="1b6fe-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1727">-inf</span></span> | <span data-ttu-id="1b6fe-1728">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1728">+inf</span></span> | <span data-ttu-id="1b6fe-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1729">NaN</span></span> | <span data-ttu-id="1b6fe-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1730">NaN</span></span> | <span data-ttu-id="1b6fe-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1731">-inf</span></span> | <span data-ttu-id="1b6fe-1732">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1732">+inf</span></span> | <span data-ttu-id="1b6fe-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1733">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1734">NaN</span></span>  | <span data-ttu-id="1b6fe-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1735">NaN</span></span>  | <span data-ttu-id="1b6fe-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1736">NaN</span></span>  | <span data-ttu-id="1b6fe-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1737">NaN</span></span> | <span data-ttu-id="1b6fe-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1738">NaN</span></span> | <span data-ttu-id="1b6fe-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1739">NaN</span></span>  | <span data-ttu-id="1b6fe-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1740">NaN</span></span>  | <span data-ttu-id="1b6fe-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1741">NaN</span></span> | 

*  <span data-ttu-id="1b6fe-1742">Mnożenie dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="1b6fe-1743">Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-1744">Jeśli wartość wyniku jest zbyt mała do reprezentowania w `decimal` formatu, wynik wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="1b6fe-1745">Skala wyniku przed dowolnego zaokrąglanie jest suma skale dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="1b6fe-1746">Mnożenie dziesiętny jest równoważne użyciu operator mnożenia typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="1b6fe-1747">Operator dzielenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1747">Division operator</span></span>

<span data-ttu-id="1b6fe-1748">Dla operacji formularza `x / y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1749">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-1750">Poniżej wymieniono operatory dzielenia wstępnie zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="1b6fe-1751">Wszystkie operatory obliczeniowe iloraz `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="1b6fe-1752">Dzielenie liczby całkowitej:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="1b6fe-1753">Jeśli wartość prawy operand jest równa zero, `System.DivideByZeroException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="1b6fe-1754">Podział zostanie zaokrąglony wynik w kierunku zera.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="1b6fe-1755">Dlatego wartość bezwzględna wynik jest największa możliwa liczba całkowita, która jest większa niż wartość bezwzględna ilorazu dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="1b6fe-1756">Wynik jest raz lub zero dodatnie dwa operandy mają ten sam znak i zero lub ujemna, gdy dwa operandy przeciwne znaków.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="1b6fe-1757">Jeśli lewy operand jest najmniejszą stałego `int` lub `long` wartość i prawy operand jest `-1`, występuje przepełnienie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="1b6fe-1758">W `checked` kontekstu, powoduje to, że `System.ArithmeticException` (lub jego podklasę) zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="1b6fe-1759">W `unchecked` kontekstu, jest zdefiniowane w implementacji czy `System.ArithmeticException` (lub jego podklasę) jest zgłaszany lub przeciążenia niezgłoszonych, Lewy argument operacji jest wartością wynikową.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="1b6fe-1760">Dzielenie zmiennoprzecinkowe:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="1b6fe-1761">Iloraz jest obliczane zgodnie z regułami IEEE 754 arytmetyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="1b6fe-1762">W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="1b6fe-1763">W tabeli `x` i `y` wartości skończoną dodatnią.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="1b6fe-1764">`z` jest wynikiem `x / y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="1b6fe-1765">Jeśli wynik jest za duża dla typu miejsca docelowego `z` jest nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="1b6fe-1766">Jeśli wynik jest za mały dla typu miejsca docelowego `z` wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="1b6fe-1767">+y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1767">+y</span></span>   | <span data-ttu-id="1b6fe-1768">-y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1768">-y</span></span>   | <span data-ttu-id="1b6fe-1769">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1769">+0</span></span>   | <span data-ttu-id="1b6fe-1770">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1770">-0</span></span>   | <span data-ttu-id="1b6fe-1771">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1771">+inf</span></span> | <span data-ttu-id="1b6fe-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1772">-inf</span></span> | <span data-ttu-id="1b6fe-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1773">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1774">+x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1774">+x</span></span>   | <span data-ttu-id="1b6fe-1775">+z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1775">+z</span></span>   | <span data-ttu-id="1b6fe-1776">-z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1776">-z</span></span>   | <span data-ttu-id="1b6fe-1777">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1777">+inf</span></span> | <span data-ttu-id="1b6fe-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1778">-inf</span></span> | <span data-ttu-id="1b6fe-1779">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1779">+0</span></span>   | <span data-ttu-id="1b6fe-1780">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1780">-0</span></span>   | <span data-ttu-id="1b6fe-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1781">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1782">-x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1782">-x</span></span>   | <span data-ttu-id="1b6fe-1783">-z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1783">-z</span></span>   | <span data-ttu-id="1b6fe-1784">+z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1784">+z</span></span>   | <span data-ttu-id="1b6fe-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1785">-inf</span></span> | <span data-ttu-id="1b6fe-1786">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1786">+inf</span></span> | <span data-ttu-id="1b6fe-1787">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1787">-0</span></span>   | <span data-ttu-id="1b6fe-1788">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1788">+0</span></span>   | <span data-ttu-id="1b6fe-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1789">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1790">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1790">+0</span></span>   | <span data-ttu-id="1b6fe-1791">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1791">+0</span></span>   | <span data-ttu-id="1b6fe-1792">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1792">-0</span></span>   | <span data-ttu-id="1b6fe-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1793">NaN</span></span>  | <span data-ttu-id="1b6fe-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1794">NaN</span></span>  | <span data-ttu-id="1b6fe-1795">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1795">+0</span></span>   | <span data-ttu-id="1b6fe-1796">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1796">-0</span></span>   | <span data-ttu-id="1b6fe-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1797">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1798">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1798">-0</span></span>   | <span data-ttu-id="1b6fe-1799">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1799">-0</span></span>   | <span data-ttu-id="1b6fe-1800">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1800">+0</span></span>   | <span data-ttu-id="1b6fe-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1801">NaN</span></span>  | <span data-ttu-id="1b6fe-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1802">NaN</span></span>  | <span data-ttu-id="1b6fe-1803">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1803">-0</span></span>   | <span data-ttu-id="1b6fe-1804">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1804">+0</span></span>   | <span data-ttu-id="1b6fe-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1805">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1806">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1806">+inf</span></span> | <span data-ttu-id="1b6fe-1807">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1807">+inf</span></span> | <span data-ttu-id="1b6fe-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1808">-inf</span></span> | <span data-ttu-id="1b6fe-1809">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1809">+inf</span></span> | <span data-ttu-id="1b6fe-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1810">-inf</span></span> | <span data-ttu-id="1b6fe-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1811">NaN</span></span>  | <span data-ttu-id="1b6fe-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1812">NaN</span></span>  | <span data-ttu-id="1b6fe-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1813">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1814">-inf</span></span> | <span data-ttu-id="1b6fe-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1815">-inf</span></span> | <span data-ttu-id="1b6fe-1816">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1816">+inf</span></span> | <span data-ttu-id="1b6fe-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1817">-inf</span></span> | <span data-ttu-id="1b6fe-1818">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1818">+inf</span></span> | <span data-ttu-id="1b6fe-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1819">NaN</span></span>  | <span data-ttu-id="1b6fe-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1820">NaN</span></span>  | <span data-ttu-id="1b6fe-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1821">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1822">NaN</span></span>  | <span data-ttu-id="1b6fe-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1823">NaN</span></span>  | <span data-ttu-id="1b6fe-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1824">NaN</span></span>  | <span data-ttu-id="1b6fe-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1825">NaN</span></span>  | <span data-ttu-id="1b6fe-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1826">NaN</span></span>  | <span data-ttu-id="1b6fe-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1827">NaN</span></span>  | <span data-ttu-id="1b6fe-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1828">NaN</span></span>  | <span data-ttu-id="1b6fe-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1829">NaN</span></span>  | 

*  <span data-ttu-id="1b6fe-1830">Dzielenie dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="1b6fe-1831">Jeśli wartość prawy operand jest równa zero, `System.DivideByZeroException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="1b6fe-1832">Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-1833">Jeśli wartość wyniku jest zbyt mała do reprezentowania w `decimal` formatu, wynik wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="1b6fe-1834">Skalowanie w wyniku jest najmniejsza skalowania, który zachowa wynik równa najbliższej dziesiętnej wartość true wyniku matematyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="1b6fe-1835">Dzielenie dziesiętne jest równoważne użyciu operator dzielenia typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="1b6fe-1836">Operator reszty</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1836">Remainder operator</span></span>

<span data-ttu-id="1b6fe-1837">Dla operacji formularza `x % y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1838">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-1839">Poniżej wymieniono operatory resztę wstępnie zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="1b6fe-1840">Wszystkie operatory obliczeniowe w pozostałej części podział `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="1b6fe-1841">Pozostała liczba całkowita:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="1b6fe-1842">Wynik `x % y` wartość jest generowany przez `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="1b6fe-1843">Jeśli `y` wynosi zero, `System.DivideByZeroException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="1b6fe-1844">Jeśli lewy operand jest najmniejszym `int` lub `long` wartość i prawy operand jest `-1`, `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-1845">W żadnym przypadku nie jest `x % y` zgłosić wyjątek, gdzie `x / y` nie zgłasza wyjątku.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="1b6fe-1846">Zmiennoprzecinkową resztę:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="1b6fe-1847">W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="1b6fe-1848">W tabeli `x` i `y` wartości skończoną dodatnią.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="1b6fe-1849">`z` jest wynikiem `x % y` i jest obliczana jako `x - n * y`, gdzie `n` jest największa możliwa liczba całkowita, która jest mniejsza niż lub równa `x / y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="1b6fe-1850">Ta metoda obliczeniowych resztę jest odpowiednikiem używanym dla liczby całkowitej argumentów, ale różni się od definicji IEEE 754 (w którym `n` jest liczbą całkowitą najbliższą `x / y`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="1b6fe-1851">+y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1851">+y</span></span>   | <span data-ttu-id="1b6fe-1852">-y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1852">-y</span></span>   | <span data-ttu-id="1b6fe-1853">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1853">+0</span></span>   | <span data-ttu-id="1b6fe-1854">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1854">-0</span></span>   | <span data-ttu-id="1b6fe-1855">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1855">+inf</span></span> | <span data-ttu-id="1b6fe-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1856">-inf</span></span> | <span data-ttu-id="1b6fe-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1857">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1858">+x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1858">+x</span></span>   | <span data-ttu-id="1b6fe-1859">+z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1859">+z</span></span>   | <span data-ttu-id="1b6fe-1860">+z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1860">+z</span></span>   | <span data-ttu-id="1b6fe-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1861">NaN</span></span>  | <span data-ttu-id="1b6fe-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1862">NaN</span></span>  | <span data-ttu-id="1b6fe-1863">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1863">x</span></span>    | <span data-ttu-id="1b6fe-1864">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1864">x</span></span>    | <span data-ttu-id="1b6fe-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1865">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1866">-x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1866">-x</span></span>   | <span data-ttu-id="1b6fe-1867">-z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1867">-z</span></span>   | <span data-ttu-id="1b6fe-1868">-z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1868">-z</span></span>   | <span data-ttu-id="1b6fe-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1869">NaN</span></span>  | <span data-ttu-id="1b6fe-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1870">NaN</span></span>  | <span data-ttu-id="1b6fe-1871">-x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1871">-x</span></span>   | <span data-ttu-id="1b6fe-1872">-x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1872">-x</span></span>   | <span data-ttu-id="1b6fe-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1873">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1874">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1874">+0</span></span>   | <span data-ttu-id="1b6fe-1875">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1875">+0</span></span>   | <span data-ttu-id="1b6fe-1876">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1876">+0</span></span>   | <span data-ttu-id="1b6fe-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1877">NaN</span></span>  | <span data-ttu-id="1b6fe-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1878">NaN</span></span>  | <span data-ttu-id="1b6fe-1879">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1879">+0</span></span>   | <span data-ttu-id="1b6fe-1880">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1880">+0</span></span>   | <span data-ttu-id="1b6fe-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1881">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1882">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1882">-0</span></span>   | <span data-ttu-id="1b6fe-1883">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1883">-0</span></span>   | <span data-ttu-id="1b6fe-1884">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1884">-0</span></span>   | <span data-ttu-id="1b6fe-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1885">NaN</span></span>  | <span data-ttu-id="1b6fe-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1886">NaN</span></span>  | <span data-ttu-id="1b6fe-1887">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1887">-0</span></span>   | <span data-ttu-id="1b6fe-1888">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1888">-0</span></span>   | <span data-ttu-id="1b6fe-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1889">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1890">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1890">+inf</span></span> | <span data-ttu-id="1b6fe-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1891">NaN</span></span>  | <span data-ttu-id="1b6fe-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1892">NaN</span></span>  | <span data-ttu-id="1b6fe-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1893">NaN</span></span>  | <span data-ttu-id="1b6fe-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1894">NaN</span></span>  | <span data-ttu-id="1b6fe-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1895">NaN</span></span>  | <span data-ttu-id="1b6fe-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1896">NaN</span></span>  | <span data-ttu-id="1b6fe-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1897">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1898">-inf</span></span> | <span data-ttu-id="1b6fe-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1899">NaN</span></span>  | <span data-ttu-id="1b6fe-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1900">NaN</span></span>  | <span data-ttu-id="1b6fe-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1901">NaN</span></span>  | <span data-ttu-id="1b6fe-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1902">NaN</span></span>  | <span data-ttu-id="1b6fe-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1903">NaN</span></span>  | <span data-ttu-id="1b6fe-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1904">NaN</span></span>  | <span data-ttu-id="1b6fe-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1905">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1906">NaN</span></span>  | <span data-ttu-id="1b6fe-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1907">NaN</span></span>  | <span data-ttu-id="1b6fe-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1908">NaN</span></span>  | <span data-ttu-id="1b6fe-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1909">NaN</span></span>  | <span data-ttu-id="1b6fe-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1910">NaN</span></span>  | <span data-ttu-id="1b6fe-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1911">NaN</span></span>  | <span data-ttu-id="1b6fe-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1912">NaN</span></span>  | <span data-ttu-id="1b6fe-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1913">NaN</span></span>  | 

*  <span data-ttu-id="1b6fe-1914">Reszta dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="1b6fe-1915">Jeśli wartość prawy operand jest równa zero, `System.DivideByZeroException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="1b6fe-1916">Skala wyniku przed dowolnego zaokrąglanie jest większej skali dwóch argumentów operacji, a znak wyniku, jeśli różna od zera, jest taki sam, jak w przypadku `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="1b6fe-1917">Dziesiętna reszta jest równoważne użyciu operator reszty typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="1b6fe-1918">Operator dodawania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1918">Addition operator</span></span>

<span data-ttu-id="1b6fe-1919">Dla operacji formularza `x + y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-1920">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-1921">Operatory dodawania wstępnie zdefiniowane są wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="1b6fe-1922">W przypadku typów liczbowych i wyliczenie operatory dodawania wstępnie zdefiniowanych Obliczanie sumy dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="1b6fe-1923">Gdy jeden lub obydwa operandy są typu ciąg, operatory dodawania wstępnie zdefiniowanych łączenie ciąg reprezentujący argumenty operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="1b6fe-1924">Dodanie liczby całkowitej:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="1b6fe-1925">W `checked` kontekstu, jeśli suma jest spoza zakresu typu wyniku, `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-1926">W `unchecked` kontekstu, przepełnienia nie są zgłaszane i wszystkie znaczące bity wyższego rzędu spoza zakresu typu wyniku są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="1b6fe-1927">Dodanie zmiennoprzecinkowa:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="1b6fe-1928">Suma jest obliczana zgodnie z regułami IEEE 754 arytmetyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="1b6fe-1929">W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="1b6fe-1930">W tabeli `x` i `y` są ograniczone wartości niezerowych i `z` jest wynikiem `x + y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="1b6fe-1931">Jeśli `x` i `y` mieć tej samej wielkości, ale odwrotnych znaków, `z` jest dodatnia zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="1b6fe-1932">Jeśli `x + y` jest zbyt duży, aby przedstawić w typie docelowym `z` jest nieskończony o ten sam znak co `x + y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="1b6fe-1933">t</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1933">y</span></span>    | <span data-ttu-id="1b6fe-1934">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1934">+0</span></span>   | <span data-ttu-id="1b6fe-1935">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1935">-0</span></span>   | <span data-ttu-id="1b6fe-1936">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1936">+inf</span></span> | <span data-ttu-id="1b6fe-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1937">-inf</span></span> | <span data-ttu-id="1b6fe-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1938">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1939">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1939">x</span></span>    | <span data-ttu-id="1b6fe-1940">z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1940">z</span></span>    | <span data-ttu-id="1b6fe-1941">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1941">x</span></span>    | <span data-ttu-id="1b6fe-1942">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1942">x</span></span>    | <span data-ttu-id="1b6fe-1943">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1943">+inf</span></span> | <span data-ttu-id="1b6fe-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1944">-inf</span></span> | <span data-ttu-id="1b6fe-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1945">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1946">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1946">+0</span></span>   | <span data-ttu-id="1b6fe-1947">t</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1947">y</span></span>    | <span data-ttu-id="1b6fe-1948">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1948">+0</span></span>   | <span data-ttu-id="1b6fe-1949">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1949">+0</span></span>   | <span data-ttu-id="1b6fe-1950">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1950">+inf</span></span> | <span data-ttu-id="1b6fe-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1951">-inf</span></span> | <span data-ttu-id="1b6fe-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1952">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1953">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1953">-0</span></span>   | <span data-ttu-id="1b6fe-1954">t</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1954">y</span></span>    | <span data-ttu-id="1b6fe-1955">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1955">+0</span></span>   | <span data-ttu-id="1b6fe-1956">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1956">-0</span></span>   | <span data-ttu-id="1b6fe-1957">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1957">+inf</span></span> | <span data-ttu-id="1b6fe-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1958">-inf</span></span> | <span data-ttu-id="1b6fe-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1959">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1960">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1960">+inf</span></span> | <span data-ttu-id="1b6fe-1961">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1961">+inf</span></span> | <span data-ttu-id="1b6fe-1962">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1962">+inf</span></span> | <span data-ttu-id="1b6fe-1963">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1963">+inf</span></span> | <span data-ttu-id="1b6fe-1964">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1964">+inf</span></span> | <span data-ttu-id="1b6fe-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1965">NaN</span></span>  | <span data-ttu-id="1b6fe-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1966">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1967">-inf</span></span> | <span data-ttu-id="1b6fe-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1968">-inf</span></span> | <span data-ttu-id="1b6fe-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1969">-inf</span></span> | <span data-ttu-id="1b6fe-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1970">-inf</span></span> | <span data-ttu-id="1b6fe-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1971">NaN</span></span>  | <span data-ttu-id="1b6fe-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1972">-inf</span></span> | <span data-ttu-id="1b6fe-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1973">NaN</span></span>  | 
   | <span data-ttu-id="1b6fe-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1974">NaN</span></span>  | <span data-ttu-id="1b6fe-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1975">NaN</span></span>  | <span data-ttu-id="1b6fe-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1976">NaN</span></span>  | <span data-ttu-id="1b6fe-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1977">NaN</span></span>  | <span data-ttu-id="1b6fe-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1978">NaN</span></span>  | <span data-ttu-id="1b6fe-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1979">NaN</span></span>  | <span data-ttu-id="1b6fe-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1980">NaN</span></span>  | 

*  <span data-ttu-id="1b6fe-1981">Dodanie dziesiętne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="1b6fe-1982">Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-1983">Skala wyniku przed dowolnego zaokrąglanie jest większej skali dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="1b6fe-1984">Dodanie dziesiętny jest równoważne użyciu operator dodawania typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="1b6fe-1985">Dodanie wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1985">Enumeration addition.</span></span> <span data-ttu-id="1b6fe-1986">Każdy typ wyliczenia niejawnie udostępnia następujące wstępnie zdefiniowane operatorów, gdzie `E` jest typem wyliczeniowym i `U` jest podstawowym typem `E`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="1b6fe-1987">W czasie wykonywania tych operatorów są oceniane dokładnie jako `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="1b6fe-1988">Łączenie ciągów:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="1b6fe-1989">Taka przeciążona funkcja sprawdza pliku binarnego `+` operator wykonywać ciągów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="1b6fe-1990">Jeśli argument ciągów jest `null`, zostanie zastąpiony ciągiem pustym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="1b6fe-1991">W przeciwnym razie którykolwiek z argumentów niż ciąg jest konwertowany na jego reprezentację ciągu za pomocą wywołania wirtualnego `ToString` metody dziedziczone z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="1b6fe-1992">Jeśli `ToString` zwraca `null`, zostanie zastąpiony ciągiem pustym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   Wynik operatora łączenia ciągów jest ciągiem, który składa się ze znaków lewy operand następują znaki prawy operand. Nigdy nie zwraca operatora łączenia ciągów `null` wartość. <span data-ttu-id="1b6fe-1995">A `System.OutOfMemoryException` mogą być generowane, jeśli nie ma wystarczającej ilości pamięci do przydzielenia do ciągu wynikowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="1b6fe-1996">Kombinacji delegatów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1996">Delegate combination.</span></span> <span data-ttu-id="1b6fe-1997">Każdy typ delegata niejawnie udostępnia następujące wstępnie zdefiniowanego operatora, gdzie `D` jest typ delegata:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="1b6fe-1998">Plik binarny `+` operator wykonuje delegatów, gdy oba operandy są typu delegata, niektóre `D`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="1b6fe-1999">(Jeśli operandów delegata różnych typów, występuje błąd wiązania.) Jeśli pierwszy argument jest `null`, wynik operacji jest wartość drugiego operandu (nawet jeśli jest to `null`).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="1b6fe-2000">W przeciwnym razie, jeśli drugi argument operacji jest `null`, wynik operacji jest wartość pierwszego operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="1b6fe-2001">W przeciwnym razie wynik operacji jest nowe delegowanie, wystąpienie, gdy wywoływany, wywołuje pierwszy operand, a następnie wywołuje drugiego operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="1b6fe-2002">Aby uzyskać przykłady delegatów, zobacz [operator odejmowania](expressions.md#subtraction-operator) i [delegować wywołania](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="1b6fe-2003">Ponieważ `System.Delegate` nie jest typem delegowanym `operator`  `+` nie zdefiniowano dla niego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="1b6fe-2004">Operator odejmowania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2004">Subtraction operator</span></span>

<span data-ttu-id="1b6fe-2005">Dla operacji formularza `x - y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-2006">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-2007">Poniżej wymieniono operatory odejmowania wstępnie zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="1b6fe-2008">Operatory wszystkie odjąć `y` z `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="1b6fe-2009">Liczba całkowita odejmowania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="1b6fe-2010">W `checked` kontekstu, jeśli różnica jest spoza zakresu typu wyniku, `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-2011">W `unchecked` kontekstu, przepełnienia nie są zgłaszane i wszystkie znaczące bity wyższego rzędu spoza zakresu typu wyniku są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="1b6fe-2012">Zmiennoprzecinkowe odejmowania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="1b6fe-2013">Różnica jest obliczane zgodnie z regułami IEEE 754 arytmetyczne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="1b6fe-2014">W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaNs.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="1b6fe-2015">W tabeli `x` i `y` są ograniczone wartości niezerowych i `z` jest wynikiem `x - y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="1b6fe-2016">Jeśli `x` i `y` są takie same `z` jest dodatnia zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="1b6fe-2017">Jeśli `x - y` jest zbyt duży, aby przedstawić w typie docelowym `z` jest nieskończony o ten sam znak co `x - y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="1b6fe-2018">t</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2018">y</span></span>    | <span data-ttu-id="1b6fe-2019">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2019">+0</span></span>   | <span data-ttu-id="1b6fe-2020">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2020">-0</span></span>   | <span data-ttu-id="1b6fe-2021">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2021">+inf</span></span> | <span data-ttu-id="1b6fe-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2022">-inf</span></span> | <span data-ttu-id="1b6fe-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2023">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-2024">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2024">x</span></span>    | <span data-ttu-id="1b6fe-2025">z</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2025">z</span></span>    | <span data-ttu-id="1b6fe-2026">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2026">x</span></span>    | <span data-ttu-id="1b6fe-2027">x</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2027">x</span></span>    | <span data-ttu-id="1b6fe-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2028">-inf</span></span> | <span data-ttu-id="1b6fe-2029">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2029">+inf</span></span> | <span data-ttu-id="1b6fe-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2030">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-2031">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2031">+0</span></span>   | <span data-ttu-id="1b6fe-2032">-y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2032">-y</span></span>   | <span data-ttu-id="1b6fe-2033">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2033">+0</span></span>   | <span data-ttu-id="1b6fe-2034">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2034">+0</span></span>   | <span data-ttu-id="1b6fe-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2035">-inf</span></span> | <span data-ttu-id="1b6fe-2036">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2036">+inf</span></span> | <span data-ttu-id="1b6fe-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2037">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-2038">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2038">-0</span></span>   | <span data-ttu-id="1b6fe-2039">-y</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2039">-y</span></span>   | <span data-ttu-id="1b6fe-2040">-0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2040">-0</span></span>   | <span data-ttu-id="1b6fe-2041">+0</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2041">+0</span></span>   | <span data-ttu-id="1b6fe-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2042">-inf</span></span> | <span data-ttu-id="1b6fe-2043">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2043">+inf</span></span> | <span data-ttu-id="1b6fe-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2044">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-2045">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2045">+inf</span></span> | <span data-ttu-id="1b6fe-2046">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2046">+inf</span></span> | <span data-ttu-id="1b6fe-2047">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2047">+inf</span></span> | <span data-ttu-id="1b6fe-2048">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2048">+inf</span></span> | <span data-ttu-id="1b6fe-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2049">NaN</span></span>  | <span data-ttu-id="1b6fe-2050">+ inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2050">+inf</span></span> | <span data-ttu-id="1b6fe-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2051">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2052">-inf</span></span> | <span data-ttu-id="1b6fe-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2053">-inf</span></span> | <span data-ttu-id="1b6fe-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2054">-inf</span></span> | <span data-ttu-id="1b6fe-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2055">-inf</span></span> | <span data-ttu-id="1b6fe-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2056">-inf</span></span> | <span data-ttu-id="1b6fe-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2057">NaN</span></span>  | <span data-ttu-id="1b6fe-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2058">NaN</span></span> | 
   | <span data-ttu-id="1b6fe-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2059">NaN</span></span>  | <span data-ttu-id="1b6fe-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2060">NaN</span></span>  | <span data-ttu-id="1b6fe-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2061">NaN</span></span>  | <span data-ttu-id="1b6fe-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2062">NaN</span></span>  | <span data-ttu-id="1b6fe-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2063">NaN</span></span>  | <span data-ttu-id="1b6fe-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2064">NaN</span></span>  | <span data-ttu-id="1b6fe-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2065">NaN</span></span> | 

*  <span data-ttu-id="1b6fe-2066">Dziesiętna odejmowania:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="1b6fe-2067">Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="1b6fe-2068">Skala wyniku przed dowolnego zaokrąglanie jest większej skali dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="1b6fe-2069">Odejmowanie dziesiętny jest równoważne użyciu operator odejmowania typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="1b6fe-2070">Wyliczenie odejmowania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2070">Enumeration subtraction.</span></span> <span data-ttu-id="1b6fe-2071">Każdy typ wyliczenia niejawnie udostępnia następujące wstępnie zdefiniowanego operatora, gdzie `E` jest typem wyliczeniowym i `U` jest podstawowym typem `E`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="1b6fe-2072">Ten operator jest oceniane dokładnie jako `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="1b6fe-2073">Innymi słowy, operator oblicza różnicę między wartości porządkowe `x` i `y`, a typ wyniku jest podstawowym typem wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="1b6fe-2074">Ten operator jest oceniane dokładnie jako `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="1b6fe-2075">Innymi słowy operator odejmuje wartość z podstawowym typem wyliczenia, reaguje wartością wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="1b6fe-2076">Delegate removal.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2076">Delegate removal.</span></span> <span data-ttu-id="1b6fe-2077">Każdy typ delegata niejawnie udostępnia następujące wstępnie zdefiniowanego operatora, gdzie `D` jest typ delegata:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="1b6fe-2078">Plik binarny `-` operator wykonuje usuwanie delegata, gdy oba operandy są typu delegata, niektóre `D`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="1b6fe-2079">Jeśli argumenty operacji typu delegowanego innego, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="1b6fe-2080">Jeśli pierwszy argument jest `null`, wynik operacji jest `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="1b6fe-2081">W przeciwnym razie, jeśli drugi argument operacji jest `null`, wynik operacji jest wartość pierwszego operandu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="1b6fe-2082">W przeciwnym razie oba operandy reprezentują wywołania list ([delegować deklaracje](delegates.md#delegate-declarations)) co najmniej jeden wpis, a wynik jest nową listę wywołania składająca się z pierwszego operandu listy z drugiego operandu wpisy usunięte z podano listę drugi argument operacji jest odpowiednie podlisty ciągłych w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="1b6fe-2083">(Aby określić równości podlisty, odpowiednie pozycje są porównywane jak operator równości delegata ([delegować Operatory równości](expressions.md#delegate-equality-operators)).) W przeciwnym razie wynikiem jest wartość operandu po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="1b6fe-2084">Żaden z operandów list jest zmieniany w procesie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="1b6fe-2085">Jeśli drugi argument operacji lista odpowiada wielu podlisty ciągłych wpisów na liście pierwszy operand, podlisty pasującego najdalej z prawej strony pozycji ciągłe zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="1b6fe-2086">Jeśli wyniki usuwania w pustej listy, wynik jest `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="1b6fe-2087">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2087">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="1b6fe-2088">Operatory przesunięcia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2088">Shift operators</span></span>

<span data-ttu-id="1b6fe-2089">`<<` i `>>` operatory są używane do wykonywania operacji przesunięcia bitowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="1b6fe-2090">Jeśli argument operacji *shift_expression* ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-2091">W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="1b6fe-2092">Dla operacji formularza `x << count` lub `x >> count`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-2093">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-2094">Podczas deklarowania przeciążonego operatora przesunięcia, typ pierwszy operand musi być zawsze, klasy lub struktury, zawierające Deklaracja operatora, a typ drugiego argumentu operacji musi być zawsze `int`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="1b6fe-2095">Operatory przesunięcia wstępnie zdefiniowane są wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="1b6fe-2096">Przesunięcie w lewo:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="1b6fe-2097">`<<` Operatora przesunięcia `x` lewo o liczbę bitów obliczane zgodnie z poniższym opisem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="1b6fe-2098">Najbardziej znaczących bitów spoza zakresu typu wyniku `x` są odrzucane, pozostałych bitów są przesuwane w lewo i pozycje bitów pusty niskiego rzędu są ustawione na zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="1b6fe-2099">Przesunięcie w prawo:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="1b6fe-2100">`>>` Operatora przesunięcia `x` prawo o liczbę bitów obliczane zgodnie z poniższym opisem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="1b6fe-2101">Podczas `x` typu `int` lub `long`, bity niskiego rzędu `x` są odrzucane, pozostałe usługi bits zostaną przesunięte w prawo i pozycje bitów pusty wyższego rzędu są ustawione na zero, jeśli `x` jest ujemna i ustaw, jeśli jeden `x` jest ujemna.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="1b6fe-2102">Podczas `x` typu `uint` lub `ulong`, bity niskiego rzędu `x` są odrzucane, pozostałych bitów zostaną przesunięte w prawo i pozycje bitów pusty wyższego rzędu są ustawione na zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="1b6fe-2103">Dla operatorów wstępnie zdefiniowanej liczby bitów, aby przenieść jest obliczany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="1b6fe-2104">Gdy typ `x` jest `int` lub `uint`, licznik przesunięć jest nadawana przez ostatnie pięć bity `count`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="1b6fe-2105">Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="1b6fe-2106">Gdy typ `x` jest `long` lub `ulong`, licznik przesunięć jest nadawana przez ostatnie sześć bity `count`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="1b6fe-2107">Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="1b6fe-2108">Jeśli wynikowe licznik przesunięć wynosi zero, operatory przesunięcia po prostu zwraca wartość `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="1b6fe-2109">Operacje przesunięcia nigdy nie powodują przepełnienia i działają tak samo w `checked` i `unchecked` kontekstów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="1b6fe-2110">Jeśli lewy operand `>>` operator ma typ całkowity ze znakiem, operator dokonuje arytmetycznego przesunięcia bezpośrednio którym wartość najbardziej znaczący bit (bit znaku) operand jest propagowana do pozycji bitów pusty wyższego rzędu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="1b6fe-2111">Jeśli lewy operand `>>` operator jest nieoznaczoną liczbę całkowitą, operator wykonuje Przesunięcie logiczne bezpośrednio polegającego pozycje bitów pusty wyższego rzędu są zawsze ustawione na zero.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="1b6fe-2112">Do wykonania operacji przeciwny tego wnioskowany z typu argumentu operacji, może służyć ma jawnych rzutowań.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="1b6fe-2113">Na przykład jeśli `x` jest zmienną typu `int`, operacja `unchecked((int)((uint)x >> y))` wykonuje Przesunięcie logiczne po prawej stronie `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="1b6fe-2114">Operatory relacyjne i badania typu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="1b6fe-2115">`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` i `as` operatory są nazywane operatory relacyjne i badania typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="1b6fe-2116">`is` Operator jest opisana w [jest operatorem](expressions.md#the-is-operator) i `as` operator jest opisana w [jako operator](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="1b6fe-2117">`==`, `!=`, `<`, `>`, `<=` i `>=` operatory są ***operatory porównania***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="1b6fe-2118">Jeśli argument operacji operatora porównania ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-2119">W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="1b6fe-2120">Dla operacji formularza `x` *op* `y`, gdzie *op* jest operator porównania, funkcja rozpoznawania przeciążeń ([operatora binarnego przeciążenia](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-2121">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-2122">Operatory porównania wstępnie zdefiniowane są opisane w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="1b6fe-2123">Wszystkie operatory porównania wstępnie zdefiniowanych zwracają wynik o typie `bool`, zgodnie z opisem w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="1b6fe-2124">__Operacja__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2124">__Operation__</span></span> | <span data-ttu-id="1b6fe-2125">__wynik__</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="1b6fe-2126">`true` Jeśli `x` jest równa `y`, `false` inaczej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="1b6fe-2127">`true` Jeśli `x` nie jest równa `y`, `false` inaczej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="1b6fe-2128">`true` Jeśli `x` jest mniejsza niż `y`, `false` inaczej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="1b6fe-2129">`true` Jeśli `x` jest większa niż `y`, `false` inaczej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="1b6fe-2130">`true` Jeśli `x` jest mniejsza niż lub równa `y`, `false` inaczej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="1b6fe-2131">`true` Jeśli `x` jest większa niż lub równa `y`, `false` inaczej</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="1b6fe-2132">Operatory porównania liczba całkowita</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2132">Integer comparison operators</span></span>

<span data-ttu-id="1b6fe-2133">Operatory porównania wstępnie zdefiniowanej liczby całkowitej są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2133">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="1b6fe-2134">Każda z tych operatorów porównuje wartości liczbowych całkowitą dwa argumenty i zwraca `bool` wartość, która wskazuje, czy określonej relacji jest `true` lub `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="1b6fe-2135">Operatory porównania zmiennoprzecinkowych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="1b6fe-2136">Operatory porównania zmiennoprzecinkowych wstępnie zdefiniowane są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2136">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="1b6fe-2137">Operatory porównują operandy zgodnie z regułami IEEE 754 standardowe:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="1b6fe-2138">Jeśli jeden z operandów jest wartością typu NaN, wynik jest `false` dla wszystkich operatorów, z wyjątkiem `!=`, dla których jest wynik `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="1b6fe-2139">Dla dowolnego dwa operandy `x != y` zawsze daje ten sam wynik jako `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="1b6fe-2140">Jednakże, jeśli jeden lub obydwa operandy są NaN, `<`, `>`, `<=`, i `>=` operatorów nie daje te same wyniki logiczną negację operatora przeciwnego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="1b6fe-2141">Na przykład, jeśli jedna z `x` i `y` jest NaN, następnie `x < y` jest `false`, ale `!(x >= y)` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="1b6fe-2142">Jeśli żaden z operandów nie jest wartością typu NaN, operatory porównują wartości dwa operandy zmiennoprzecinkowych w odniesieniu do porządkowania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="1b6fe-2143">gdzie `min` i `max` to najmniejsza i największa dodatnią skończoną wartości, które mogą być reprezentowane w danym formacie zmiennoprzecinkowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="1b6fe-2144">Godne uwagi efekty ta kolejność są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="1b6fe-2145">Zer ujemny i dodatni, są uznawane za równe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="1b6fe-2146">Minus nieskończoności jest uważany za mniej niż inne wartości, ale równa innego ujemna nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="1b6fe-2147">Nieskończoności dodatniej jest uważany za większy niż inne wartości, ale równa innego nieskończoności dodatniej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="1b6fe-2148">Operatory porównania dziesiętna</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2148">Decimal comparison operators</span></span>

<span data-ttu-id="1b6fe-2149">Operatory porównania dziesiętna wstępnie zdefiniowane są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="1b6fe-2150">Każda z tych operatorów porównuje wartości liczbowych dwa operandy dziesiętnych i zwraca `bool` wartość, która wskazuje, czy określonej relacji jest `true` lub `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="1b6fe-2151">Każde porównanie dziesiętny jest równoważne użyciu odpowiednie relacyjnych lub operator równości typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="1b6fe-2152">Operatory logiczne równości</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2152">Boolean equality operators</span></span>

<span data-ttu-id="1b6fe-2153">Operatory równości logiczne wstępnie zdefiniowane są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="1b6fe-2154">Wynik `==` jest `true` Jeśli oba `x` i `y` są `true` lub jeśli oba `x` i `y` są `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="1b6fe-2155">W przeciwnym razie wynikiem jest `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="1b6fe-2156">Wynik `!=` jest `false` Jeśli oba `x` i `y` są `true` lub jeśli oba `x` i `y` są `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="1b6fe-2157">W przeciwnym razie wynikiem jest `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="1b6fe-2158">Kiedy operandy są typu `bool`, `!=` operatora daje ten sam wynik jako `^` operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="1b6fe-2159">Operatory porównania wyliczenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="1b6fe-2160">Każdy typ wyliczenia niejawnie zawiera następujące operatory porównania wstępnie zdefiniowane:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="1b6fe-2161">Wynik obliczania wartości `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczeniowego `E` z typu podstawowego `U`, i `op` jest jednym z operatorów porównania, jest dokładnie taka sama jak Ocena `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="1b6fe-2162">Innymi słowy operatory porównania typu wyliczenia po prostu Porównaj podstawowej wartości całkowitej dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="1b6fe-2163">Operatory porównania typu odwołania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2163">Reference type equality operators</span></span>

<span data-ttu-id="1b6fe-2164">Operatory porównania odwołanie wstępnie zdefiniowany typ to:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="1b6fe-2165">Operatory zwracają wynik porównanie dwa odwołania pod względem równości lub bez równości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="1b6fe-2166">Ponieważ operatory równości odwołanie wstępnie zdefiniowany typ akceptuje argumentów operacji typu `object`, odnoszą się do wszystkich typów, które nie są deklarowane dotyczy `operator ==` i `operator !=` elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="1b6fe-2167">Z drugiej strony wszystkie operatory równości zdefiniowanych przez użytkownika dotyczy skutecznie ukryć Operatory równości odwołanie wstępnie zdefiniowanego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="1b6fe-2168">Operatory równości odwołanie wstępnie zdefiniowany typ wymagają jednego z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="1b6fe-2169">Oba operandy są wartości typu, znane jako *reference_type* lub literału `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="1b6fe-2170">Ponadto konwersję jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) istnieje z typu oba operandy typu to drugi operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="1b6fe-2171">Jeden z operandów jest wartość typu `T` gdzie `T` jest *type_parameter* , a drugi operand jest literał `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="1b6fe-2172">Ponadto `T` nie ma ograniczenia typu wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="1b6fe-2173">Jeśli jeden z tych warunków jest spełniony, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="1b6fe-2174">Dostępne są następujące istotne konsekwencje tych reguł:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="1b6fe-2175">Jest to błąd powiązania w czasie, aby użyć operatorów równości typu odwołania wstępnie zdefiniowanych, aby porównać dwa odwołania, które są znane jako różne podczas wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="1b6fe-2176">Na przykład, jeśli typy powiązań czasu operandy są dwa typy klas `A` i `B`, a jeśli żadna `A` ani `B` dziedziczy po drugiej, a następnie byłoby niemożliwe w przypadku dwóch argumentów operacji odwoływać się do tego samego obiektu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="1b6fe-2177">W związku z tym operacja jest uznawany za błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="1b6fe-2178">Operatory porównania odwołanie wstępnie zdefiniowany typ nie zezwalają na wartości operandy typu, który można porównać.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="1b6fe-2179">Dlatego chyba że typ struktury deklaruje swój własny operatory porównania, nie jest możliwe do porównania wartości tego typu struktury.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="1b6fe-2180">Operatory porównania odwołanie wstępnie zdefiniowany typ nigdy nie powodują operacje na polach wystąpić w przypadku ich argumentów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="1b6fe-2181">Jest całkowicie nieprzydatna wykonania takich operacji pakowania, ponieważ odwołania do nowo przydzielonego wystąpienia spakowany niekoniecznie będzie różnić się od inne odnośniki do.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="1b6fe-2182">Jeśli argument operacji typu parametru typu `T` jest porównywany z `null`oraz typ środowiska wykonawczego `T` jest typem wartości wynikiem porównania jest `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="1b6fe-2183">Poniższy przykład sprawdza, czy argument nieograniczony parametr typu jest `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="1b6fe-2184">`x == null` Konstrukcja jest dozwolone mimo `T` może reprezentować typ wartości, a wynik jest definiowana po prostu być `false` podczas `T` jest typem wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="1b6fe-2185">Dla operacji formularza `x == y` lub `x != y`, jeśli wszystkie odpowiednie `operator ==` lub `operator !=` istnieje Rozpoznanie przeciążenia operatora ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) wybierze reguł, które Operator zamiast operatora równości odwołanie wstępnie zdefiniowanego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="1b6fe-2186">Jednak zawsze jest możliwe wybrać operator równości typu odwołania wstępnie zdefiniowane przez jawne rzutowanie jedno lub oba operandy na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="1b6fe-2187">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2187">The example</span></span>
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
<span data-ttu-id="1b6fe-2188">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2188">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="1b6fe-2189">`s` i `t` zmienne odnoszą się do dwóch odrębnych `string` wystąpienia zawierające te same znaki.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="1b6fe-2190">Wyświetla pierwszy porównania `True` ponieważ ciąg wstępnie zdefiniowanego operatora równości ([operatory porównania ciągów](expressions.md#string-equality-operators)) zostaje wybrany, gdy oba operandy są typu `string`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="1b6fe-2191">Pozostałych porównań, wszystkie dane wyjściowe `False` ponieważ operatora równości typu odwołania wstępnie jest zaznaczone, gdy co najmniej jeden z operandów jest typu `object`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="1b6fe-2192">Należy pamiętać, że powyższe technika nie istotnych dla typów wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="1b6fe-2193">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2193">The example</span></span>
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
<span data-ttu-id="1b6fe-2194">dane wyjściowe `False` ponieważ rzutowania utworzyć odwołania do dwóch osobnych wystąpień opakowany `int` wartości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="1b6fe-2195">Operatory porównania ciągów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2195">String equality operators</span></span>

<span data-ttu-id="1b6fe-2196">Operatory porównania wstępnie zdefiniowanych parametrów są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="1b6fe-2197">Dwa `string` wartości są uznawane za równe, gdy jest spełniony jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="1b6fe-2198">Obie wartości są `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="1b6fe-2199">Obie wartości są inne niż null odwołania do wystąpienia ciągu, które mają identyczne długości i znaków identyczny w każdej pozycji znaku.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="1b6fe-2200">Operatory porównania ciągu porównanie wartości ciągu zamiast odwołania do ciągu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="1b6fe-2201">Jeśli dwa wystąpienia ciągu oddzielne zawierają dokładnie tej samej sekwencji znaków, wartości ciągi są równe, ale różnią się odwołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="1b6fe-2202">Zgodnie z opisem w [Operatory równości typu odwołania](expressions.md#reference-type-equality-operators), operatory równości typu odwołania może służyć do porównywania odwołań ciągu zamiast wartości ciągu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="1b6fe-2203">Operatory porównania delegata</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2203">Delegate equality operators</span></span>

<span data-ttu-id="1b6fe-2204">Każdy typ delegata niejawnie zawiera następujące operatory porównania wstępnie zdefiniowane:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="1b6fe-2205">Delegat dwóch wystąpień uważa za równe w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="1b6fe-2206">Jeśli jedno z wystąpień delegata jest `null`, są równe tylko wtedy, gdy oba są `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="1b6fe-2207">Jeśli delegatów mają różne typu run-time, nigdy nie są takie same.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="1b6fe-2208">Jeśli oba wystąpienia delegata ma listę wywołań ([delegować deklaracje](delegates.md#delegate-declarations)), te wystąpienia są takie same, tylko wtedy, gdy ich wywołania listy mają taką samą długość, a każdy wpis na liście wywołania jednej firmy jest taki sam, (zdefiniowany poniżej) Aby odpowiadający mu wpis w kolejności na liście wywołania drugiej strony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="1b6fe-2209">Równość wpisów na liście wywołania decydują następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="1b6fe-2210">Jeśli dwa wywołania wpisów na liście zarówno odnoszą się do tej samej statycznej metody, a następnie pozycje są równe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="1b6fe-2211">Jeśli dwa wywołania wpisów na liście oba odnoszą się do tej samej metody niestatycznych na tego samego obiektu docelowego (zgodnie z definicją Operatory równości odwołań), wpisy są równe.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="1b6fe-2212">Wywołanie listy wpisów utworzone z wersji ewaluacyjnej semantycznie identycznych *anonymous_method_expression*s lub *lambda_expression*s przy użyciu tego samego zestawu (prawdopodobnie pusta) przechwyconej zmiennej zewnętrzne wystąpienia są dozwolone (ale nie muszą) być taki sam.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="1b6fe-2213">Operatory równości i o wartości null</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2213">Equality operators and null</span></span>

<span data-ttu-id="1b6fe-2214">`==` i `!=` operatory zezwala na wartość typu dopuszczającego wartość null, a drugie to jeden z operandów `null` literału, nawet jeśli żaden operator wstępnie zdefiniowanych lub zdefiniowanych przez użytkownika (w unlifted lub zniesione formularza) istnieje dla tej operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="1b6fe-2215">Dla operacji o jednym z formularzy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="1b6fe-2216">gdzie `x` to wyrażenie typu dopuszczającego wartość null, jeśli Rozpoznanie przeciążenia operatora ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) nie może znaleźć zastosowanie operatora, wynik zamiast tego jest obliczany na podstawie `HasValue` Właściwość `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="1b6fe-2217">W szczególności pierwsze dwa formularze są tłumaczone na `!x.HasValue`, a ostatnie dwa formularze są tłumaczone na `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="1b6fe-2218">Is — operator</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2218">The is operator</span></span>

<span data-ttu-id="1b6fe-2219">`is` Operator jest używany do dynamicznie Sprawdź, czy typu run-time obiektu jest zgodne z danym typem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="1b6fe-2220">Wynik operacji `E is T`, gdzie `E` to wyrażenie i `T` to typ, jest wartością logiczną wartość wskazującą czy `E` pomyślnie można przekonwertować na typ `T` przy konwersji odwołania, konwersji boxing Konwersja lub konwersji rozpakowującej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="1b6fe-2221">Operacja jest obliczane w następujący sposób, po mają zostać podstawione argumentów typu dla wszystkich parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="1b6fe-2222">Jeśli `E` jest funkcją anonimową, wystąpi błąd kompilacji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="1b6fe-2223">Jeśli `E` jest grupa metod lub `null` literału, jeśli typ `E` jest typem referencyjnym lub typ dopuszczający wartość null i wartości `E` jest wartość null, wynikiem jest wartość false.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="1b6fe-2224">W przeciwnym razie umożliwiają `D` reprezentuje typ dynamiczny `E` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="1b6fe-2225">Jeśli typ `E` jest typem referencyjnym `D` jest typu run-time odwołania wystąpienie przez `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="1b6fe-2226">Jeśli typ `E` jest typem zerowalnym `D` jest podstawowym typem tego typu dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="1b6fe-2227">Jeśli typ `E` jest typem wartości niedopuszczającym wartości `D` jest typem `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="1b6fe-2228">Wynikiem operacji jest zależna od `D` i `T` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="1b6fe-2229">Jeśli `T` jest typem referencyjnym, wynikiem jest wartość true, jeśli `D` i `T` są tego samego typu, jeśli `D` jest typem odwołania i niejawna konwersja odwołania z `D` do `T` istnieje, lub jeśli `D` jest to typ wartości i konwersja boxing `D` do `T` istnieje.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="1b6fe-2230">Jeśli `T` jest typem zerowalnym wynikiem jest wartość true, jeśli `D` jest podstawowym typem `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="1b6fe-2231">Jeśli `T` jest typem wartości niedopuszczającym wartości, wynikiem jest wartość true, jeśli `D` i `T` tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="1b6fe-2232">W przeciwnym razie wynikiem jest wartość false.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="1b6fe-2233">Konwersje zdefiniowane przez użytkownika nie są traktowane przez `is` operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="1b6fe-2234">Operator as</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2234">The as operator</span></span>

<span data-ttu-id="1b6fe-2235">`as` Operator jest używany do jawnie przekonwertować wartość typu danego odwołania lub typ dopuszczający wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="1b6fe-2236">W przeciwieństwie do wyrażenia rzutowania ([rzutowane wyrażenia,](expressions.md#cast-expressions)), `as` operator nigdy nie zgłasza wyjątku.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="1b6fe-2237">Zamiast tego, jeśli wskazany konwersja nie jest możliwe, wartość wynikowa zależy `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="1b6fe-2238">W przypadku operacji formularza `E as T`, `E` musi być wyrażeniem i `T` musi być typem referencyjnym, parametr typu, znane jako typu odwołania lub typ dopuszczający wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="1b6fe-2239">Ponadto co najmniej jedną z następujących musi mieć wartość true lub w przeciwnym razie wystąpi błąd w czasie kompilacji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="1b6fe-2240">Tożsamość ([konwersji tożsamości](conversions.md#identity-conversion)), niejawne dopuszcza wartości null ([niejawne konwersje dopuszczającego wartość null](conversions.md#implicit-nullable-conversions)), niejawne odwołanie ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)), pakowania ([ Konwersje boxing](conversions.md#boxing-conversions)), jest to jawne dopuszczającego wartość null ([jawne konwersje dopuszczającego wartość null](conversions.md#explicit-nullable-conversions)), jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)), i konwersja unboxing ([Konwersje rozpakowywania](conversions.md#unboxing-conversions)) istnieje konwersja z `E` do `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="1b6fe-2241">Typ `E` lub `T` jest typem otwartym.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="1b6fe-2242">`E` jest `null` literału.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="1b6fe-2243">Jeśli typ czasu kompilacji `E` nie `dynamic`, operacja `E as T` daje ten sam wynik jako</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="1b6fe-2244">z tą różnicą, że `E` jest obliczany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="1b6fe-2245">Kompilator można oczekiwać w celu zoptymalizowania `E as T` sprawdzania co najwyżej jednego typu dynamicznego zamiast dwóch kontroli typu dynamicznego też dorozumianych przez rozszerzenie powyżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="1b6fe-2246">Jeśli typ czasu kompilacji `E` jest `dynamic`, w odróżnieniu od operatora rzutowania `as` operator nie jest powiązany dynamicznie ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-2247">W związku z tym rozszerzeniem w tym przypadku to:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="1b6fe-2248">Należy zauważyć, że niektóre konwersji, takie jak konwersje zdefiniowane przez użytkownika nie można zrobić za pomocą `as` operatora i zamiast tego powinny być wykonywane przy użyciu wyrażenia cast.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="1b6fe-2249">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2249">In the example</span></span>
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
<span data-ttu-id="1b6fe-2250">Parametr typu `T` z `G` jest znany jako typ odwołania, ponieważ ma ona ograniczenia klasy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="1b6fe-2251">Parametr typu `U` z `H` nie jest jednak; dlatego używanie `as` operatora w `H` jest niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="1b6fe-2252">Operatory logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2252">Logical operators</span></span>

<span data-ttu-id="1b6fe-2253">`&`, `^`, I `|` operatory są nazywane operatorów logicznych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="1b6fe-2254">Jeśli operand logicznego operatora ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-2255">W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="1b6fe-2256">Dla operacji formularza `x op y`, gdzie `op` jest jednym z operatorów logicznych Rozpoznanie przeciążenia ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="1b6fe-2257">Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="1b6fe-2258">Operatory logiczne wstępnie zdefiniowane są opisane w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="1b6fe-2259">Operatory logiczne liczba całkowita</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2259">Integer logical operators</span></span>

<span data-ttu-id="1b6fe-2260">Operatory logiczne wstępnie zdefiniowanej liczby całkowitej są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2260">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="1b6fe-2261">`&` Operator oblicza operatora testu koniunkcji logicznej `AND` z dwóch argumentów operacji `|` operator oblicza operatora testu koniunkcji logicznej `OR` z dwóch argumentów operacji i `^` operator oblicza wyłączny sumy bitowej dla logicznej `OR` z dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="1b6fe-2262">Nie przepełnienia są możliwe do tych operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="1b6fe-2263">Operatory logiczne wyliczenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2263">Enumeration logical operators</span></span>

<span data-ttu-id="1b6fe-2264">Każdy typ wyliczenia `E` niejawnie udostępnia następujące wstępnie zdefiniowane operatory logiczne:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="1b6fe-2265">Wynik obliczania wartości `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczeniowego `E` z typu podstawowego `U`, i `op` jest jednym z operatorów logicznych, jest dokładnie taka sama jak Ocena `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="1b6fe-2266">Innymi słowy operatorów logicznych typu wyliczenia po prostu wykonać operacji logicznej na podstawowym typem dwóch argumentów operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="1b6fe-2267">Operatory logiczne logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2267">Boolean logical operators</span></span>

<span data-ttu-id="1b6fe-2268">Wstępnie zdefiniowane logiczna operatory logiczne to:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="1b6fe-2269">Wynik `x & y` jest `true` Jeśli oba `x` i `y` są `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="1b6fe-2270">W przeciwnym razie wynikiem jest `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="1b6fe-2271">Wynik `x | y` jest `true` Jeśli `x` lub `y` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="1b6fe-2272">W przeciwnym razie wynikiem jest `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="1b6fe-2273">Wynik `x ^ y` jest `true` Jeśli `x` jest `true` i `y` jest `false`, lub `x` jest `false` i `y` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="1b6fe-2274">W przeciwnym razie wynikiem jest `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="1b6fe-2275">Kiedy operandy są typu `bool`, `^` operator oblicza ten sam wynik jako `!=` operatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="1b6fe-2276">Operatory dopuszczające wartość null, boolean i logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="1b6fe-2277">Dopuszcza wartości null typu boolean `bool?` może reprezentować trzech wartości `true`, `false`, i `null`i jest podobna do przechowywanymi w trzech użytej dla wyrażeń logicznych w języku SQL.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="1b6fe-2278">Aby upewnij się, że wyniki generowane przez `&` i `|` operatory `bool?` operandy są zgodne z logiką przechowywanymi w trzech SQL, następujące operatory wstępnie zdefiniowanych znajdują się:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="1b6fe-2279">W poniższej tabeli przedstawiono efektów tych operatorów dla wszystkich kombinacjach wartości `true`, `false`, i `null`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="1b6fe-2280">Operatory logiczne warunkowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2280">Conditional logical operators</span></span>

<span data-ttu-id="1b6fe-2281">`&&` i `||` operatory są nazywane warunkowego operatorów logicznych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="1b6fe-2282">Są również nazywane "short-circuiting" operatorów logicznych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2282">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="1b6fe-2283">`&&` i `||` operatory są wersjami warunkowego `&` i `|` operatory:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="1b6fe-2284">Operacja `x && y` odnosi się do operacji `x & y`, chyba że `y` jest oceniane tylko wtedy, gdy `x` nie `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="1b6fe-2285">Operacja `x || y` odnosi się do operacji `x | y`, chyba że `y` jest oceniane tylko wtedy, gdy `x` nie `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="1b6fe-2286">Jeśli operand logicznego operatora warunkowego ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-2287">W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="1b6fe-2288">Operacji formularza `x && y` lub `x || y` jest przetwarzany przez zastosowanie Rozpoznanie przeciążenia ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) tak, jakby został napisany operacji `x & y` lub `x | y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="1b6fe-2289">Następnie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2289">Then,</span></span>

*  <span data-ttu-id="1b6fe-2290">Rozpoznanie przeciążenia kończy się niepowodzeniem, można znaleźć jednego operatora najlepsze lub jeśli funkcja rozpoznawania przeciążeń wybiera jeden z operatorów logicznych wstępnie zdefiniowanej liczby całkowitej, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-2291">W przeciwnym razie, jeśli wybrany operator jest jednym z wstępnie zdefiniowanych logiczna operatorów logicznych ([logiczna operatorów logicznych](expressions.md#boolean-logical-operators)) lub operatory dopuszczające wartość null, boolean i logiczne ([operatory dopuszczające wartość null, boolean i logiczne](expressions.md#nullable-boolean-logical-operators)), Operacja zostanie przetworzona, zgodnie z opisem w [operatory logiczne, warunkowe i logiczne](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="1b6fe-2292">W przeciwnym razie wybrane operator jest zdefiniowany przez użytkownika operator, a operacja zostanie przetworzona, zgodnie z opisem w [zdefiniowanych przez użytkownika operatorów logicznych warunkowych](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="1b6fe-2293">Nie jest możliwe bezpośrednio przeciążanie operatorów logicznych warunkowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="1b6fe-2294">Jednak ponieważ operatorów logicznych warunkowego są oceniane pod względem regularne operatorów logicznych, przeciążenia regularne operatory logiczne z pewnymi ograniczeniami również pełnią funkcję przeciążenia operatorów logicznych warunkowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="1b6fe-2295">Jest to dokładniejszym opisem zawartym w [zdefiniowanych przez użytkownika operatorów logicznych warunkowych](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="1b6fe-2296">Operatory logiczne, warunkowe i logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="1b6fe-2297">Podczas argumenty operacji `&&` lub `||` typu `bool`, lub gdy operandy typów, które nie zostaną zdefiniowane odpowiednim `operator &` lub `operator |`, ale definiuje konwersji niejawnych do `bool`, operacja jest przetwarzane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="1b6fe-2298">Operacja `x && y` jest oceniane jako `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="1b6fe-2299">Innymi słowy `x` najpierw jest obliczane i konwertowane na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="1b6fe-2300">Następnie, jeśli `x` jest `true`, `y` jest obliczane i konwertowane na typ `bool`, a ten staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="1b6fe-2301">W przeciwnym razie wynik operacji jest `false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="1b6fe-2302">Operacja `x || y` jest oceniane jako `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="1b6fe-2303">Innymi słowy `x` najpierw jest obliczane i konwertowane na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="1b6fe-2304">Następnie, jeśli `x` jest `true`, wynik operacji jest `true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="1b6fe-2305">W przeciwnym razie `y` jest obliczane i konwertowane na typ `bool`, a ten staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="1b6fe-2306">Zdefiniowane przez użytkownika operatorów logicznych warunkowych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="1b6fe-2307">Gdy argumenty operacji `&&` lub `||` są typów, które deklarują odpowiednim zdefiniowanych przez użytkownika `operator &` lub `operator |`, oba następujące muszą być spełnione, gdzie `T` jest typem, w którym zadeklarowany jest operatora:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="1b6fe-2308">Zwracany typ i typ każdego parametru operatora musi być `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="1b6fe-2309">Oznacza to, należy obliczyć logiczny operator `AND` lub logicznej `OR` z dwóch argumentów operacji typu `T`i musi zwrócić wynik o typie `T`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="1b6fe-2310">`T` musi zawierać deklaracje `operator true` i `operator false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="1b6fe-2311">Błąd powiązania występuje, jeśli żadnego z tych wymagań nie został spełniony.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="1b6fe-2312">W przeciwnym razie `&&` lub `||` operacji jest oceniany, łącząc zdefiniowany przez użytkownika `operator true` lub `operator false` przy użyciu wybranego operatora zdefiniowanego przez użytkownika:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="1b6fe-2313">Operacja `x && y` jest oceniane jako `T.false(x) ? x : T.&(x, y)`, gdzie `T.false(x)` jest wywołanie `operator false` zadeklarowanych w `T`, i `T.&(x, y)` jest wywołaniem wybranego `operator &`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="1b6fe-2314">Innymi słowy `x` najpierw jest obliczane i `operator false` jest wywoływane na wynik, aby określić, czy `x` ma zdecydowanie wartość false.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="1b6fe-2315">Następnie, jeśli `x` ma zdecydowanie wartość FAŁSZ, wynik operacji jest wartością, który został wcześniej obliczane `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="1b6fe-2316">W przeciwnym razie `y` zostało ocenione, a wybranym `operator &` jest wywoływane na wartość wcześniej obliczane `x` i wartość jest obliczana dla `y` do uzyskania wyniku operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="1b6fe-2317">Operacja `x || y` jest oceniane jako `T.true(x) ? x : T.|(x, y)`, gdzie `T.true(x)` jest wywołanie `operator true` zadeklarowanych w `T`, i `T.|(x,y)` jest wywołaniem wybranego `operator|`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="1b6fe-2318">Innymi słowy `x` najpierw jest obliczane i `operator true` jest wywoływane na wynik, aby określić, czy `x` zdecydowanie obowiązuje.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="1b6fe-2319">Następnie, jeśli `x` ma zdecydowanie wartość true, wynik operacji jest wartością, który został wcześniej obliczane `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="1b6fe-2320">W przeciwnym razie `y` zostało ocenione, a wybranym `operator |` jest wywoływane na wartość wcześniej obliczane `x` i wartość jest obliczana dla `y` do uzyskania wyniku operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="1b6fe-2321">W jednej z tych operacji, wyrażenie podane `x` jest tylko wykonywane tylko raz i wyrażenie podane `y` nie obliczeniem i oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="1b6fe-2322">Aby uzyskać przykład typ, który implementuje `operator true` i `operator false`, zobacz [bazy danych typu boolean](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="1b6fe-2323">Wartość null operatora łączącego</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2323">The null coalescing operator</span></span>

<span data-ttu-id="1b6fe-2324">`??` Operator jest nazywany operatora łączącego o wartości null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="1b6fe-2325">Wyrażenie łączącego o wartości null w postaci `a ?? b` wymaga `a` będzie typ dopuszczający wartość null typu lub odwołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="1b6fe-2326">Jeśli `a` jest różna od null, wynik `a ?? b` jest `a`; w przeciwnym razie wynikiem jest `b`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="1b6fe-2327">Daje w wyniku operacji `b` tylko wtedy, gdy `a` ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="1b6fe-2328">Wartość null operatora łączącego jest zespolony z prawej, co oznacza, że operacje są pogrupowane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="1b6fe-2329">Na przykład, wyrażenie w formie `a ?? b ?? c` jest oceniane jako `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="1b6fe-2330">Ogólnie rzecz biorąc warunki, wyrażenie w formie `E1 ?? E2 ?? ... ?? En` zwraca pierwszy operandów, która jest inna niż null lub wartość null, jeśli wszystkie argumenty mają wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="1b6fe-2331">Typ wyrażenia `a ?? b` zależy od których niejawne konwersje są dostępne na operandach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="1b6fe-2332">Według priorytetu, typu `a ?? b` jest `A0`, `A`, lub `B`, gdzie `A` jest typem `a` (pod warunkiem, że `a` ma typ), `B` jest typem `b` () pod warunkiem, że `b` ma typ), a `A0` jest podstawowym typem `A` Jeśli `A` jest typem zerowalnym lub `A` inaczej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="1b6fe-2333">W szczególności `a ?? b` są przetwarzane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="1b6fe-2334">Jeśli `A` istnieje i nie jest typu dopuszczającego wartość null lub typ referencyjny, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-2335">Jeśli `b` jest wyrażeniem dynamiczny typ wyniku jest `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="1b6fe-2336">W czasie wykonywania `a` najpierw jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="1b6fe-2337">Jeśli `a` nie ma wartości null, `a` jest konwertowana na dynamiczne, a ten staje się wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="1b6fe-2338">W przeciwnym razie `b` jest obliczane i staje się on wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="1b6fe-2339">W przeciwnym razie, jeśli `A` istnieje i jest typu dopuszczającego wartość null, i istnieje niejawna konwersja z `b` do `A0`, typ wyniku jest `A0`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="1b6fe-2340">W czasie wykonywania `a` najpierw jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="1b6fe-2341">Jeśli `a` nie ma wartości null, `a` jest nieopakowanych, aby wpisać `A0`, i staje się on wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="1b6fe-2342">W przeciwnym razie `b` jest obliczane i konwertowane na typ `A0`, i staje się on wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="1b6fe-2343">W przeciwnym razie, jeśli `A` istnieje i istnieje niejawna konwersja z `b` do `A`, typ wyniku jest `A`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="1b6fe-2344">W czasie wykonywania `a` najpierw jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="1b6fe-2345">Jeśli `a` nie ma wartości null, `a` staje się wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="1b6fe-2346">W przeciwnym razie `b` jest obliczane i konwertowane na typ `A`, i staje się on wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="1b6fe-2347">W przeciwnym razie, jeśli `b` ma typ `B` i istnieje niejawna konwersja z `a` do `B`, typ wyniku jest `B`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="1b6fe-2348">W czasie wykonywania `a` najpierw jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="1b6fe-2349">Jeśli `a` nie ma wartości null, `a` jest nieopakowanych, aby wpisać `A0` (Jeśli `A` istnieje i ma wartość null) i zostanie przekonwertowana na typ `B`, i staje się on wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="1b6fe-2350">W przeciwnym razie `b` jest obliczane i staje się wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="1b6fe-2351">W przeciwnym razie `a` i `b` występuje są niezgodne, a błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="1b6fe-2352">Operator warunkowy</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2352">Conditional operator</span></span>

<span data-ttu-id="1b6fe-2353">`?:` Operator jest nazywany operatora warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="1b6fe-2354">Jest to czasami również operator trójargumentowy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="1b6fe-2355">Wyrażenie warunkowe w postaci `b ? x : y` najpierw ocenia stan `b`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="1b6fe-2356">Następnie, jeśli `b` jest `true`, `x` jest obliczane i staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="1b6fe-2357">W przeciwnym razie `y` jest obliczane i staje się wynik operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="1b6fe-2358">Wyrażenie warunkowe nigdy nie ocenia zarówno `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="1b6fe-2359">Operator warunkowy jest zespolony z prawej, co oznacza, że operacje są pogrupowane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="1b6fe-2360">Na przykład, wyrażenie w formie `a ? b : c ? d : e` jest oceniane jako `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="1b6fe-2361">Pierwszy argument operacji `?:` operator musi być wyrażeniem, które mogą być niejawnie konwertowane na `bool`, lub wyrażenie typu, który implementuje `operator true`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="1b6fe-2362">Jeśli żadna z tych wymagań nie jest spełniony, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="1b6fe-2363">Drugi i trzeci operand, `x` i `y`, z `?:` operator kontrolowanie typu wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="1b6fe-2364">Jeśli `x` ma typ `X` i `y` ma typ `Y` następnie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="1b6fe-2365">Jeśli niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `X` do `Y`, ale nie z `Y` do `X`, następnie `Y` jest typem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="1b6fe-2366">Jeśli niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `Y` do `X`, ale nie z `X` do `Y`, następnie `X` jest typem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="1b6fe-2367">W przeciwnym razie można określić żadnego typu wyrażenia i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="1b6fe-2368">Jeśli tylko jeden z `x` i `y` ma typ i zarówno `x` i `y`, z są niejawnie konwertowane na tego typu, który jest typu wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="1b6fe-2369">W przeciwnym razie można określić żadnego typu wyrażenia i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="1b6fe-2370">Przetwarzanie czasu wykonywania wyrażenia warunkowego formularza `b ? x : y` składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-2371">Po pierwsze, `b` jest obliczane i `bool` wartość `b` jest określana:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="1b6fe-2372">Jeśli niejawna konwersja z typu `b` do `bool` istnieje, to niejawna konwersja jest przeprowadzana w celu wygenerowania `bool` wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="1b6fe-2373">W przeciwnym razie `operator true` zdefiniowane przez typ `b` jest wywoływana w celu wygenerowania `bool` wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="1b6fe-2374">Jeśli `bool` wartość utworzony w kroku powyżej `true`, następnie `x` jest obliczane i konwertowane na typ wyrażenia warunkowego, a ten staje się wynikiem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="1b6fe-2375">W przeciwnym razie `y` jest obliczane i konwertowane na typ wyrażenia warunkowego, a ten staje się wynikiem wyrażenia warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="1b6fe-2376">Funkcja anonimowa wyrażeń</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2376">Anonymous function expressions</span></span>

<span data-ttu-id="1b6fe-2377">***Funkcja anonimowa*** jest wyrażeniem, które reprezentuje definicję metody "w tekście".</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="1b6fe-2378">Funkcja anonimowa nie ma wartości lub typem w i samego siebie, ale jest konwertowany na zgodny delegata lub typ drzewa wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="1b6fe-2379">Ocena konwersję funkcja anonimowa zależy od typ docelowy konwersji: Jeśli jest to typ delegowany, konwersja wynikiem jest wartość delegata odwołuje się do metody, która definiuje funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="1b6fe-2380">Jeśli typ drzewa wyrażeń, konwersja daje w wyniku drzewo wyrażenia, który reprezentuje strukturę metodę jako struktury obiektu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="1b6fe-2381">Ze względów historycznych istnieją dwa składni odmian funkcje anonimowe, mianowicie *lambda_expression*s i *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="1b6fe-2382">Dla prawie wszystkich celów *lambda_expression*s są bardziej zwięzłe i ekspresyjny niż *anonymous_method_expression*s, które pozostają w języku dla zapewnienia zgodności.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="1b6fe-2383">`=>` Operator ma ten sam priorytet, jako przydział (`=`) i jest zespolony z prawej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="1b6fe-2384">Funkcja anonimowa z `async` modyfikator jest funkcji asynchronicznej i zgodna z zasadami opisanymi w [Iteratory](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="1b6fe-2385">Parametry funkcją anonimową w formie *lambda_expression* można wpisać jawnie lub niejawnie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="1b6fe-2386">Na liście parametrów jawnie wpisanych jest jawnie określony typ każdego parametru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="1b6fe-2387">Na liście parametrów niejawnie wpisane typy parametrów są dedukowane z kontekstu, w którym następuje funkcja anonimowa — w szczególności, gdy funkcja anonimowa jest konwertowany na typ zgodny delegata lub typ drzewa wyrażeń, który zapewnia typ typy parametrów ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="1b6fe-2388">W funkcją anonimową z parametrem pojedynczy, niejawnie wpisane w liście parametrów można pominąć nawiasów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="1b6fe-2389">Innymi słowy funkcja anonimowa formularza</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="1b6fe-2390">można stosować skrót do</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="1b6fe-2391">Lista parametrów funkcji anonimowych w formie *anonymous_method_expression* jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="1b6fe-2392">Biorąc pod uwagę, parametry muszą jawnie wpisane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="1b6fe-2393">Jeśli nie, funkcja anonimowa jest konwertowany na delegata z każdego parametru listy, niezawierający `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="1b6fe-2394">A *bloku* treści funkcją anonimową jest osiągalny ([punktów końcowych i osiągalności](statements.md#end-points-and-reachability)), chyba że funkcja anonimowa odbywa się wewnątrz instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="1b6fe-2395">Przykłady funkcji anonimowych, wykonaj poniższe:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2395">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="1b6fe-2396">Zachowanie *lambda_expression*s i *anonymous_method_expression*s jest taki sam, z wyjątkiem następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="1b6fe-2397">*anonymous_method_expression*s zezwala na liście parametrów, aby można pominąć w całości, reaguje przetwarzania na typy wszystkie listy parametrów wielowartościowych delegatów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="1b6fe-2398">*lambda_expression*s zezwala na używanie typów parametru pominięcia i wywnioskować, natomiast *anonymous_method_expression*s wymagają typy parametrów, należy jawnie podać.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="1b6fe-2399">Treść *lambda_expression* może być wyrażenie lub blok instrukcji, natomiast treści *anonymous_method_expression* musi być blok instrukcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="1b6fe-2400">Tylko *lambda_expression*ma konwersji na typy drzewa wyrażenie zgodne ([typy drzewa wyrażenie](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="1b6fe-2401">Funkcja anonimowa podpisów</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2401">Anonymous function signatures</span></span>

<span data-ttu-id="1b6fe-2402">Opcjonalny *anonymous_function_signature* z funkcją anonimową definiuje nazwy i opcjonalnie typy parametrów formalnych dla funkcji anonimowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="1b6fe-2403">Zakres parametry funkcja anonimowa jest *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="1b6fe-2404">([Zakresy](basic-concepts.md#scopes)) wraz z listy parametrów (jeśli) anonimowe —-treści metody stanowi miejsca deklaracji ([deklaracje](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="1b6fe-2405">Ten sposób jest to błąd kompilacji dla nazwy parametru funkcja anonimowa dopasowany do nazwy zmiennej lokalnej, stała lokalna lub parametr, którego zakres obejmuje *anonymous_method_expression* lub *lambda_ wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="1b6fe-2406">Jeśli funkcja anonimowa *explicit_anonymous_function_signature*, zbiór delegata zgodne typy i drzewa wyrażenie jest ograniczone do tych, które mają te same typy parametrów i Modyfikatory w tej samej kolejności.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="1b6fe-2407">W przeciwieństwie do metody konwersji grup ([metody konwersji grup](conversions.md#method-group-conversions)), ma przeciwwskazań wariancję typy parametrów funkcji anonimowych nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="1b6fe-2408">Jeśli nie jest funkcją anonimową *anonymous_function_signature*, zbiór delegata zgodne typy i drzewa wyrażenie jest ograniczone do tych, które nie mają `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="1b6fe-2409">Należy pamiętać, że *anonymous_function_signature* nie może zawierać atrybutów lub tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="1b6fe-2410">Niemniej jednak *anonymous_function_signature* może być zgodny z typem delegowanym, którego lista parametrów zawiera tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="1b6fe-2411">Należy zauważyć, tej konwersji na typ drzewa wyrażeń, nawet wtedy, gdy zgodne, może nadal się nie powieść w czasie kompilacji ([typy drzewa wyrażenie](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="1b6fe-2412">Gdy ciało funkcji anonimowych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2412">Anonymous function bodies</span></span>

<span data-ttu-id="1b6fe-2413">Treść (*wyrażenie* lub *bloku*) jest funkcją anonimową się następujące zasady:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="1b6fe-2414">Jeśli funkcja anonimowa zawiera podpis, określone w sygnaturze parametry są dostępne w treści.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="1b6fe-2415">Jeśli funkcja anonimowa nie ma sygnatury może on zostać przekonwertowany do typu delegata lub typ wyrażenia o parametry ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)), ale parametry nie są dostępne w treści.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="1b6fe-2416">Z wyjątkiem `ref` lub `out` parametry określone w sygnaturze (jeśli istnieją) w najbliższej otaczającej funkcja anonimowa, jest to błąd czasu kompilacji, treści, aby uzyskać dostęp do `ref` lub `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="1b6fe-2417">Gdy typ `this` jest typem struktury jest to błąd czasu kompilacji, treści, aby uzyskać dostęp do `this`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="1b6fe-2418">Jest to istotne, czy dostęp jest jawne (podobnie jak w `this.x`) lub niejawne (podobnie jak w `x` gdzie `x` jest to składowa struktury).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="1b6fe-2419">Ta reguła po prostu zabrania takiego dostępu i nie ma wpływu na czy wyszukanie członka skutkuje członka struktury.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="1b6fe-2420">Treść ma dostęp do zmiennych zewnętrznych ([zmienne zewnętrzne](expressions.md#outer-variables)) funkcji anonimowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="1b6fe-2421">Dostęp do zewnętrznej zmiennej będzie odwoływać się do wystąpienia w zmiennej, która jest aktywna w czasie *lambda_expression* lub *anonymous_method_expression* jest oceniany ([oceny Funkcja anonimowa wyrażeń](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="1b6fe-2422">Jest to błąd kompilacji w treści zawierać `goto` instrukcji `break` instrukcji lub `continue` instrukcji, którego cel jest poza treścią lub w treści zawartej w anonimowej funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="1b6fe-2423">A `return` instrukcji w treści nie zwróci kontroli z wywołania najbliższej otaczającej funkcja anonimowa, nie z otaczającego element członkowski funkcji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="1b6fe-2424">Wyrażenie określone w `return` instrukcja musi być niejawnie konwertowane na typ zwracany typ delegata lub typ drzewa wyrażeń, do którego najbliższej otaczającej *lambda_expression* lub *anonymous_ method_expression* jest konwertowany ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="1b6fe-2425">Jest on jawnie nieokreślony, czy istnieje sposób wykonania bloku funkcją anonimową innych niż ocena i wywołanie *lambda_expression* lub *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="1b6fe-2426">W szczególności kompilator może zadecydować o stosowaniu funkcją anonimową przez Syntetyzujące jeden lub więcej nazwanych, metody lub typów.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="1b6fe-2427">Nazwy takie syntetyzowany elementy muszą być zarezerwowana do użytku kompilatora formularza.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="1b6fe-2428">Rozpoznanie przeciążenia i funkcje anonimowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="1b6fe-2429">Funkcje anonimowe na liście argumentów uczestniczyć w wnioskowanie o typie i rozpoznanie przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="1b6fe-2430">Zapoznaj się [wnioskowanie o typie](expressions.md#type-inference) i [Rozpoznanie przeciążenia](expressions.md#overload-resolution) dokładnie reguł.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="1b6fe-2431">Poniższy przykład ilustruje efekt funkcjami anonimowymi w przeciążeniu rozdzielczości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="1b6fe-2432">`ItemList<T>` Klasa ma dwa `Sum` metody.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="1b6fe-2433">Każdy przyjmuje `selector` argumentu, który wyodrębnia wartość do zsumowania za pośrednictwem z elementu listy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="1b6fe-2434">Wyodrębniona wartość może być `int` lub `double` i wynikowa suma jest podobnie `int` lub `double`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="1b6fe-2435">`Sum` Metody na przykład można użyć do obliczenia sum z listy wierszy szczegółów zamówienia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="1b6fe-2436">W pierwszym wywołaniu `orderDetails.Sum`, zarówno `Sum` dotyczą metod ponieważ funkcja anonimowa `d => d. UnitCount` jest zgodny z obu `Func<Detail,int>` i `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="1b6fe-2437">Jednak funkcja rozpoznawania przeciążeń wybiera pierwszy `Sum` metody ponieważ konwersja na `Func<Detail,int>` jest lepsze niż konwersja `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="1b6fe-2438">W drugim wywołaniu `orderDetails.Sum`, tylko drugi `Sum` metoda stosowana jest ponieważ funkcja anonimowa `d => d.UnitPrice * d.UnitCount` tworzy wartość typu `double`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="1b6fe-2439">W efekcie przeciążenia rozpoznawania wybiera drugiej `Sum` metodę dla tego wywołania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="1b6fe-2440">Funkcje anonimowe i wiązanie dynamiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="1b6fe-2441">Funkcja anonimowa nie może być odbiorcy, argument lub argument dynamicznie powiązane działania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="1b6fe-2442">Zmienne zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2442">Outer variables</span></span>

<span data-ttu-id="1b6fe-2443">Dowolnej zmiennej lokalnej, wartość parametru lub tablicy parametrów, których zakres obejmuje *lambda_expression* lub *anonymous_method_expression* nosi nazwę ***zewnętrzna zmienna*** funkcji anonimowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="1b6fe-2444">W to funkcja składowa klasy `this` wartość uznaje się za parametrem wartości i nie jest zewnętrzna zmienna funkcji anonimowych zawartych w funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="1b6fe-2445">Przechwyconych zmiennych zewnętrznych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2445">Captured outer variables</span></span>

<span data-ttu-id="1b6fe-2446">Zewnętrzna zmienna odwołuje się funkcją anonimową, zewnętrzna zmienna jest określane jako zostały ***przechwycone*** , funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="1b6fe-2447">Zazwyczaj, okresu istnienia zmiennej lokalnej jest ograniczona do wykonywania bloku lub instrukcji, z którym jest skojarzona ([zmienne lokalne](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="1b6fe-2448">Jednak okres istnienia przechwyconej zmiennej zewnętrzne jest rozszerzany co najmniej do delegata lub drzewa wyrażeń utworzone na podstawie funkcja anonimowa kwalifikuje się do wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="1b6fe-2449">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2449">In the example</span></span>
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
<span data-ttu-id="1b6fe-2450">Zmienna lokalna `x` jest przechwytywany przez funkcja anonimowa, a okres istnienia `x` jest co najmniej rozszerzone do delegata zwróciło `F` kwalifikuje się do wyrzucania elementów bezużytecznych, (która nie jest realizowane aż do końca bardzo program).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="1b6fe-2451">Ponieważ każdego wywołania funkcji anonimowych operuje na to samo wystąpienie elementu `x`, dane wyjściowe z przykładu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="1b6fe-2452">Podczas lokalnej zmiennej lub parametru wartości są przechwytywane przez funkcję anonimową, zmienną lokalną ani parametrem przestaje być uważany za jako zmienna ustalona ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)), ale zamiast tego jest uważany za ruchoma Zmienna.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="1b6fe-2453">Ten sposób dowolny `unsafe` najpierw użyć kodu, który przyjmuje adres przechwyconej zmiennej zewnętrzne `fixed` instrukcję, aby naprawić zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="1b6fe-2454">Należy pamiętać, że w przeciwieństwie do Niewychwycone zmiennej przechwyconej zmiennej lokalnej mogą jednocześnie łączyć się z wielu wątków wykonania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="1b6fe-2455">Podczas tworzenia wystąpienia zmiennych lokalnych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2455">Instantiation of local variables</span></span>

<span data-ttu-id="1b6fe-2456">Zmienna lokalna jest uważany za ***tworzone*** podczas wykonywania wprowadza zakres zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="1b6fe-2457">Na przykład, gdy wywoływana jest metoda następujące, zmienna lokalna `x` jest tworzone i inicjowane trzy razy — raz dla każdej iteracji pętli.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="1b6fe-2458">Jednak przenoszenie deklaracji `x` poza pętlę skutkuje pojedynczego wystąpienia `x`:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="1b6fe-2459">Nie są przechwytywane, nie ma żadnego sposobu, aby przyjrzeć się dokładnie, jak często tworzone jest zmienną lokalną, ponieważ okres istnienia wystąpień są rozłączne, jest możliwe dla każdego wystąpienia po prostu używać tej samej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="1b6fe-2460">Jednak po funkcją anonimową przechwytuje zmienną lokalną, skutki wystąpienia stają się oczywiste.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="1b6fe-2461">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2461">The example</span></span>
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
<span data-ttu-id="1b6fe-2462">generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2462">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="1b6fe-2463">Jednakże, jeśli deklaracja `x` zostanie przeniesiony poza pętlę:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="1b6fe-2464">Dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2464">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="1b6fe-2465">Czy zdefiniowana Pętla for deklaruje zmienną iteracji, że sama zmienna jest uznawane za można zadeklarować poza pętlą.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="1b6fe-2466">W związku z tym jeśli przykładzie jest zmieniany na przechwytywanie sama Zmienna iteracji:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="1b6fe-2467">tylko jedno wystąpienie zmiennej iteracji są przechwytywane, która generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="1b6fe-2468">Istnieje możliwość dla obiektów delegowanych funkcja anonimowa udostępnić niektóre przechwyconych zmiennych, ale mają oddzielne wystąpienia innych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="1b6fe-2469">Na przykład jeśli `F` zostanie zmieniona na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2469">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="1b6fe-2470">trzy delegatów przechwytywania to samo wystąpienie elementu `x` ale oddzielnych wystąpień `y`, a dane wyjściowe są:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="1b6fe-2471">Osobne funkcje anonimowe można przechwycić to samo wystąpienie elementu zewnętrzna zmienna.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="1b6fe-2472">W przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2472">In the example:</span></span>
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
<span data-ttu-id="1b6fe-2473">te dwie funkcje anonimowe przechwytywania to samo wystąpienie elementu Zmienna lokalna `x`, a zatem "Komunikacja" za pośrednictwem tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="1b6fe-2474">Dane wyjściowe z przykładu jest:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2474">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="1b6fe-2475">Obliczanie wyrażeń funkcja anonimowa</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="1b6fe-2476">Funkcja anonimowa `F` zawsze musi zostać skonwertowany do typu delegata `D` lub typ drzewa wyrażeń `E`, bezpośrednio lub za pośrednictwem wykonywania wyrażenia tworzenia delegata `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="1b6fe-2477">Ta konwersja określa wynik funkcja anonimowa, zgodnie z opisem w [konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="1b6fe-2478">Wyrażenia zapytań</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2478">Query expressions</span></span>

<span data-ttu-id="1b6fe-2479">***Wyrażenia zapytań*** zapewniają składnię języka zintegrowanych zapytań, który jest podobny do relacyjne i hierarchiczne zapytania języków, takich jak SQL i języka XQuery.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="1b6fe-2480">Wyrażenie zapytania zaczyna się od `from` klauzula i kończy się znakiem albo `select` lub `group` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="1b6fe-2481">Początkowy `from` klauzuli może następować zero lub więcej `from`, `let`, `where`, `join` lub `orderby` klauzul.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="1b6fe-2482">Każdy `from` klauzula jest generator Przedstawiamy ***zmiennej zakresu*** której zakresy nad elementami ***sekwencji***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="1b6fe-2483">Każdy `let` klauzuli wprowadza zmienną zakresu reprezentujących wartości obliczane przy użyciu poprzednich zmiennych zakresu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="1b6fe-2484">Każdy `where` klauzula jest filtr, który wyklucza elementów z wynikiem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="1b6fe-2485">Każdy `join` klauzuli porównuje klucze określonej sekwencji źródłowej z kluczami sekwencji innego, reaguje pasujących par.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="1b6fe-2486">Każdy `orderby` klauzuli zmienia kolejność elementów zgodnie z określonymi kryteriami. Końcowe `select` lub `group` klauzula określa kształt wyniku w postaci zmiennych zakresu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="1b6fe-2487">Na koniec `into` klauzuli może służyć do "splice" zapytań przez traktowanie wyniki jednego zapytania jako generator w kolejnych zapytań.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="1b6fe-2488">Niejednoznaczności w wyrażeniach zapytań</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="1b6fe-2489">Wyrażenia zapytań zawiera szereg "kontekstowymi słowami kluczowymi", czyli identyfikatorów, które mają specjalne znaczenie w danym kontekście.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="1b6fe-2490">W szczególności są to `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` i `by`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="1b6fe-2491">Aby uniknąć niejednoznaczności w wyrażeniach zapytań spowodowany mieszane użycie tych identyfikatorów jak słowa kluczowe lub nazwy proste, te identyfikatory są uważane za słów kluczowych, gdy występuje w dowolnym miejscu w wyrażeniu zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="1b6fe-2492">W tym celu w wyrażeniu kwerendy jest dowolne wyrażenie, które zaczyna się od "`from identifier`"następuje dowolny token, z wyjątkiem"`;`","`=`"or"`,`".</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="1b6fe-2493">Aby można było używać tych słów jako identyfikatorów w wyrażeniu zapytania, może być prefiksem "`@`" ([identyfikatory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="1b6fe-2494">Translacja wyrażeń zapytania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2494">Query expression translation</span></span>

<span data-ttu-id="1b6fe-2495">W języku C# nie określa semantykę wykonywania wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="1b6fe-2496">Zamiast wyrażenia kwerendy są tłumaczone na wywołania metody, które będą zgodne *wzorzec wyrażenia kwerendy* ([wzorzec wyrażenia kwerendy](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="1b6fe-2497">W szczególności wyrażenia kwerendy są tłumaczone na wywołania metody o nazwie `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, i `Cast`. Te metody powinny mieć określonej sygnatury i typy wyników, zgodnie z opisem w [wzorzec wyrażenia kwerendy](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="1b6fe-2498">Te metody mogą być metod wystąpień obiektu, którego dotyczy kwerenda lub metody rozszerzenia, które są zewnętrzne w stosunku do obiektu i implementują rzeczywiste wykonanie zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="1b6fe-2499">Tłumaczenie z wyrażenia zapytań wywołań metod jest składni mapowanie, które występuje, zanim wszystkie powiązania typu lub Rozpoznanie przeciążenia została wykonana.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="1b6fe-2500">Tłumaczenie jest gwarantowane jest syntaktycznie prawidłowy, ale nie jest gwarantowana w celu wygenerowania semantycznie poprawny kod C#.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="1b6fe-2501">Następujące Translacja wyrażeń zapytania, wynikowy wywołań metody opisywanego są przetwarzane jako wywołania metod regularnych, a to z kolei może ujawnić z błędów, na przykład jeśli te metody nie istnieje, jeśli argumenty mają nieprawidłowe typy lub w przypadku metod ogólnych i wnioskowanie o typie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="1b6fe-2502">Wyrażenie zapytania są przetwarzane przez zastosowanie następujących tłumaczenia wielokrotnie, dopóki nie będą możliwe nie dalsze obniżki.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="1b6fe-2503">Tłumaczenia są wymienione w kolejności stosowania: każdej sekcji przyjęto założenie, że tłumaczenia w poprzednich sekcjach, które mogły zostać wykonane wyczerpująco, a po wyczerpaniu sekcji będzie nie później należy powrócić podczas przetwarzania wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="1b6fe-2504">Przypisanie do zmiennych zakresu są niedozwolone w wyrażeniach zapytań.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="1b6fe-2505">Jednak implementacja języka C# może nie zawsze wymuszać to ograniczenie, ponieważ jest to czasami nie jest możliwe przy użyciu schematu składni tłumaczenia przedstawionych w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="1b6fe-2506">Niektóre tłumaczenia wstrzyknąć zmiennych zakresu o identyfikatorach przezroczyste wskazywane przez `*`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="1b6fe-2507">Specjalne właściwości przezroczyste identyfikatory zostały omówione w dalszych [przezroczyste identyfikatory](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="1b6fe-2508">Wybierz i grupowania klauzul z kontynuacjami</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="1b6fe-2509">Wyrażenie zapytania z kontynuacji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="1b6fe-2510">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="1b6fe-2511">Tłumaczenia w poniższych sekcjach założono kwerendy nie mają `into` kontynuacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="1b6fe-2512">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="1b6fe-2513">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="1b6fe-2514">ostateczne tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="1b6fe-2515">Typy zmiennych zakresu jawne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2515">Explicit range variable types</span></span>

<span data-ttu-id="1b6fe-2516">A `from` klauzula, która jawnie określa typ zmiennej zakresu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="1b6fe-2517">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="1b6fe-2518">A `join` klauzula, która jawnie określa typ zmiennej zakresu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="1b6fe-2519">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2519">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="1b6fe-2520">Tłumaczenia w poniższych sekcjach przyjęto założenie, że zapytania mają typy zmiennych nie jawnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="1b6fe-2521">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="1b6fe-2522">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="1b6fe-2523">ostateczne tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="1b6fe-2524">Typy zmiennych zakresu jawne są przydatne w przypadku zapytań kolekcje, które implementują niepodstawowy `IEnumerable` interfejs, ale nie ogólnego `IEnumerable<T>` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="1b6fe-2525">W powyższym przykładzie będzie to sytuacji, gdy `customers` były typu `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="1b6fe-2526">Wyrażenia zapytań wymiaru degeneracji</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2526">Degenerate query expressions</span></span>

<span data-ttu-id="1b6fe-2527">W wyrażeniu kwerendy w postaci</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="1b6fe-2528">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="1b6fe-2529">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="1b6fe-2530">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="1b6fe-2531">Wyrażenie zapytania wymiaru degeneracji to taki, który przypadku wybiera elementy źródła.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="1b6fe-2532">Nowsze fazy tłumaczenia usuwa wymiaru degeneracji zapytania wprowadzone przez innych kroków translacji, zastępując je ze swoim źródłem.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="1b6fe-2533">Ważne jest jednak do upewnij się, że wynik kwerendy wyrażenie nigdy nie jest obiekt źródłowy, jak, może ujawnić typu i tożsamość źródła do klienta zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="1b6fe-2534">W związku z tym ten krok chroni wymiaru degeneracji zapytania zapisywane bezpośrednio w kodzie źródłowym, wywołując jawnie `Select` na "source".</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="1b6fe-2535">Jest ona maksymalnie implementacje z `Select` i innych operatorów zapytań, aby upewnić się, że te metody nigdy nie należy zwracać samego obiektu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="1b6fe-2536">Od pozwolić w przypadku gdy klauzule sprzężenia i orderby</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="1b6fe-2537">Wyrażenie zapytania przy użyciu drugiej `from` klauzuli `select` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="1b6fe-2538">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="1b6fe-2539">Wyrażenie zapytania przy użyciu drugiej `from` klauzuli następuje coś innego niż `select` klauzuli:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="1b6fe-2540">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="1b6fe-2541">Wyrażenie zapytania przy użyciu `let` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="1b6fe-2542">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="1b6fe-2543">Wyrażenie zapytania przy użyciu `where` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="1b6fe-2544">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="1b6fe-2545">Wyrażenie zapytania przy użyciu `join` klauzuli bez `into` następuje `select` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="1b6fe-2546">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="1b6fe-2547">Wyrażenie zapytania przy użyciu `join` klauzuli bez `into` następuje coś innego niż `select` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="1b6fe-2548">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="1b6fe-2549">Wyrażenie zapytania przy użyciu `join` klauzula `into` następuje `select` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="1b6fe-2550">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="1b6fe-2551">Wyrażenie zapytania przy użyciu `join` klauzula `into` następuje coś innego niż `select` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="1b6fe-2552">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="1b6fe-2553">Wyrażenie zapytania przy użyciu `orderby` — klauzula</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="1b6fe-2554">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="1b6fe-2555">Jeśli kolejność klauzula Określa `descending` wskaźnik kierunku, wywołanie `OrderByDescending` lub `ThenByDescending` jest generowany w zamian.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="1b6fe-2556">Następujące tłumaczenia przyjęto założenie, że istnieją nie `let`, `where`, `join` lub `orderby` klauzul i nie więcej niż jednego początkowego `from` klauzuli w każdym wyrażeniu zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="1b6fe-2557">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="1b6fe-2558">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="1b6fe-2559">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="1b6fe-2560">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="1b6fe-2561">ostateczne tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="1b6fe-2562">gdzie `x` jest identyfikatorem wygenerowanego przez kompilator, który jest niewidoczna i są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="1b6fe-2563">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="1b6fe-2564">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="1b6fe-2565">ostateczne tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="1b6fe-2566">gdzie `x` jest identyfikatorem wygenerowanego przez kompilator, który jest niewidoczna i są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="1b6fe-2567">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="1b6fe-2568">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="1b6fe-2569">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="1b6fe-2570">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="1b6fe-2571">ostateczne tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="1b6fe-2572">gdzie `x` i `y` to identyfikatory wygenerowanego przez kompilator, które w przeciwnym razie są niewidoczne i są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="1b6fe-2573">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="1b6fe-2574">ma ostateczne tłumaczenie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="1b6fe-2575">Wybierz klauzule</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2575">Select clauses</span></span>

<span data-ttu-id="1b6fe-2576">W wyrażeniu kwerendy w postaci</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="1b6fe-2577">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="1b6fe-2578">z wyjątkiem sytuacji, gdy v jest identyfikator x, tłumaczenie jest po prostu</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="1b6fe-2579">Na przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="1b6fe-2580">po prostu przetłumaczyć</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="1b6fe-2581">Groupby klauzule</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2581">Groupby clauses</span></span>

<span data-ttu-id="1b6fe-2582">W wyrażeniu kwerendy w postaci</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="1b6fe-2583">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="1b6fe-2584">z wyjątkiem sytuacji, gdy v jest identyfikator x, tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="1b6fe-2585">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="1b6fe-2586">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="1b6fe-2587">Identyfikatory przezroczyste</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2587">Transparent identifiers</span></span>

<span data-ttu-id="1b6fe-2588">Niektóre tłumaczenia wstrzyknąć zmiennych zakresu przy użyciu ***przezroczyste identyfikatory*** wskazywane przez `*`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="1b6fe-2589">Identyfikatory przezroczysty nie są funkcją języka właściwego; istnieją one tylko jako pośredniego kroku w procesie tłumaczenia wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="1b6fe-2590">Podczas translacji zapytania wprowadza użyciem przezroczystego identyfikatora, dalszych kroków translacji propagować użyciem przezroczystego identyfikatora do inicjatory obiekt anonimowy i funkcjami anonimowymi.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="1b6fe-2591">W tych kontekstach przezroczyste identyfikatory mają następujące zachowanie:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="1b6fe-2592">W przypadku przezroczyste identyfikator jako parametr w funkcją anonimową, elementy członkowskie typu anonimowego skojarzone są wykonywane automatycznie w zakresie treści funkcji anonimowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="1b6fe-2593">Element członkowski z użyciem przezroczystego identyfikatora znajduje się w zakresie, elementy członkowskie tego członka należą również zakres.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="1b6fe-2594">W przypadku użyciem przezroczystego identyfikatora wystąpienia jako deklarator składowej w inicjatorze obiektu anonimowego wprowadza element członkowski z użyciem przezroczystego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="1b6fe-2595">W tłumaczenia opisane powyżej przejrzyste identyfikatory są zawsze wprowadzone wraz z anonimowych typów, z celem przechwytywania wielu zmiennych zakresu jako elementy członkowskie pojedynczego obiektu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="1b6fe-2596">Implementacja języka C# jest dozwolone mechanizm innego niż typy anonimowe umożliwiają grupowanie wielu zmiennych zakresu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="1b6fe-2597">W poniższych przykładach tłumaczenia założono anonimowych typów są używane i pokazują, jak przezroczyste identyfikatory mogą być tłumaczone natychmiast.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="1b6fe-2598">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="1b6fe-2599">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="1b6fe-2600">który jest dalsze przetłumaczyć</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="1b6fe-2601">które, gdy usuwane są identyfikatory przezroczyste, jest odpowiednikiem</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="1b6fe-2602">gdzie `x` jest identyfikatorem wygenerowanego przez kompilator, który jest niewidoczna i są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="1b6fe-2603">Przykład</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="1b6fe-2604">jest tłumaczony na</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="1b6fe-2605">który jest dalsze skrócony do</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="1b6fe-2606">ostateczne tłumaczenie jest</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2606">the final translation of which is</span></span>
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
<span data-ttu-id="1b6fe-2607">gdzie `x`, `y`, i `z` to identyfikatory wygenerowanego przez kompilator, które w przeciwnym razie są niewidoczne i są niedostępne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="1b6fe-2608">Wzorzec wyrażenia zapytania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2608">The query expression pattern</span></span>

<span data-ttu-id="1b6fe-2609">***Wzorzec wyrażenia kwerendy*** ustanawia wzorzec metod, których typy można zaimplementować w celu obsługi wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="1b6fe-2610">Ponieważ wyrażenia kwerendy są przekształcane wywołań metod za pomocą składni mapowania, typy mają znaczną elastyczność w zakresie sposobu wdrażania do wzorca wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="1b6fe-2611">Na przykład metody wzorca można można zaimplementować jako wystąpienie metody lub metody rozszerzenia ponieważ dwa mieć tej samej składni wywołania i metody mogą żądać delegatów lub drzew wyrażeń, ponieważ funkcje anonimowe są konwertowane na wartość oba.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="1b6fe-2612">Zalecane kształt typu rodzajowego `C<T>` , obsługuje wzorzec wyrażenia kwerendy znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="1b6fe-2613">Typ ogólny jest używana w celu zilustrowania odpowiednie relacje między typami parametrów i wynik, ale istnieje możliwość zaimplementowania wzorca dla typów innych niż ogólne także.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="1b6fe-2614">Powyższych metod korzystają z typów Delegat ogólny `Func<T1,R>` i `Func<T1,T2,R>`, ale może równie dobrze korzystali innych typów delegat lub wyrażenie drzewa za pomocą tej samej relacji w typach parametrów i wynik.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="1b6fe-2615">Zwróć uwagę, zalecane relacji między `C<T>` i `O<T>` , dzięki któremu `ThenBy` i `ThenByDescending` metody są dostępne tylko w wyniku `OrderBy` lub `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="1b6fe-2616">Należy również zauważyć zalecane kształt wyniku `GroupBy` — sekwencji, gdzie każda sekwencja wewnętrzny ma dodatkowy `Key` właściwości.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="1b6fe-2617">`System.Linq` Przestrzeń nazw zawiera implementację wzorca operatora zapytania dla dowolnego typu, który implementuje `System.Collections.Generic.IEnumerable<T>` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="1b6fe-2618">Operatory przypisania</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2618">Assignment operators</span></span>

<span data-ttu-id="1b6fe-2619">Operatory przypisania przypisać nową wartość do zmiennej, właściwości, zdarzenia lub elementu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="1b6fe-2620">Lewy operand przypisania musi być wyrażeniem sklasyfikowane jako zmienną, dostęp do właściwości, indeksatora lub zdarzenia dostęp do.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="1b6fe-2621">`=` Operator jest nazywany ***operator przypisania prostego***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="1b6fe-2622">Przypisuje wartość prawy operand do zmiennej, właściwość lub indeksator elementu przez lewy operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="1b6fe-2623">Lewy operand prosty operator przypisania nie może być dostęp do zdarzenia (z wyjątkiem sytuacji opisanej w [zdarzenia podobne do pól](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="1b6fe-2624">Operator przypisania prostego jest opisana w [przypisanie proste](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="1b6fe-2625">Operatory przypisania w innych niż `=` operator są nazywane ***złożone operatory przypisania***.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="1b6fe-2626">Te operatory wykonać operacji wskazanych w dwóch argumentów operacji, a następnie przypisz wynikowej wartości do zmiennej, właściwość lub indeksator elementu przez lewy operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="1b6fe-2627">Złożone operatory przypisania są opisane w [przydział złożony](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="1b6fe-2628">`+=` i `-=` operatory o wyrażenie dostępu do zdarzeń jako lewy operand są nazywane *operatory przypisania zdarzeń*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="1b6fe-2629">Żaden operator przypisywania jest prawidłowy dostęp do zdarzenia jako lewy operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="1b6fe-2630">Operatory przypisania zdarzeń są opisane w [przypisanie zdarzenia](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="1b6fe-2631">Operatory przypisania są łączność do prawej strony, co oznacza, że operacje są pogrupowane od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="1b6fe-2632">Na przykład, wyrażenie w formie `a = b = c` jest oceniane jako `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="1b6fe-2633">Przypisanie proste</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2633">Simple assignment</span></span>

<span data-ttu-id="1b6fe-2634">`=` Operator jest nazywany prosty operator przypisania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="1b6fe-2635">Jeśli lewy operand przypisanie proste ma postać `E.P` lub `E[Ei]` gdzie `E` ma typ kompilacji `dynamic`, a następnie przypisanie dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-2636">W tym przypadku jest typu wyrażenia przypisania w czasie kompilacji `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania, na podstawie typu środowiska wykonawczego `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="1b6fe-2637">W proste zadanie prawy operand musi być wyrażeniem, który jest niejawnie konwertowany na typ operandu po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="1b6fe-2638">Operacja przypisuje wartość prawy operand do zmiennej, właściwość lub indeksator elementu przez lewy operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="1b6fe-2639">Wynik wyrażenia przypisanie proste jest wartość przypisana do lewy operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="1b6fe-2640">Wynik ma ten sam typ jako lewy operand i zawsze jest klasyfikowana jako wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="1b6fe-2641">Jeśli lewy operand jest dostępu właściwość lub indeksator, właściwość lub indeksator musi mieć `set` metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="1b6fe-2642">Jeśli nie jest to możliwe, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-2643">Przetwarzanie środowiska wykonawczego przypisanie proste formularza `x = y` składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="1b6fe-2644">Jeśli `x` zostanie sklasyfikowany jako zmienną:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="1b6fe-2645">`x` jest oceniany w celu utworzenia zmiennej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="1b6fe-2646">`y` jest sprawdzane, a w razie potrzeby konwertowane na typ `x` za pośrednictwem niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="1b6fe-2647">Jeśli zmienna, podane przez `x` jest element tablicy *reference_type*, sprawdzanie w czasie wykonania, odbywa się na upewnij się, że wartość obliczona dla `y` jest zgodny z wystąpieniem tablicy `x` jest element.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="1b6fe-2648">Sprawdzanie zakończy się pomyślnie, jeśli `y` jest `null`, lub jeśli niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje rzeczywisty typ wystąpienia odwołuje się `y` do typu rzeczywistego elementu wystąpienie tablicy zawierającego `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="1b6fe-2649">W przeciwnym razie `System.ArrayTypeMismatchException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="1b6fe-2650">Wartość wynikające z oceny i konwersja `y` są przechowywane w lokalizacji określonej przez ocenę `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="1b6fe-2651">Jeśli `x` zostanie sklasyfikowany jako właściwość lub indeksator dostępu:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="1b6fe-2652">Wyrażenia wystąpienia (Jeśli `x` nie jest `static`) i listą argumentów (Jeśli `x` jest dostęp indeksatora) skojarzony z `x` są oceniane, a wyniki są używane w kolejnych żadaniach `set` wywołania metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="1b6fe-2653">`y` jest sprawdzane, a w razie potrzeby konwertowane na typ `x` za pośrednictwem niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="1b6fe-2654">`set` Akcesor `x` jest wywoływana z wartością obliczane `y` jako jego `value` argumentu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="1b6fe-2655">Reguły odchyleń wspólnej tablicy ([Kowariancja tablicy](arrays.md#array-covariance)) zezwala na wartość typu tablicowego `A[]` jako odwołanie do wystąpienia typu tablicowego `B[]`, o ile istnieje niejawna konwersja odwołania `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="1b6fe-2656">Ze względu na tych zasad, przypisanie do elementu tablicy *reference_type* wymaga sprawdzenia środowiska wykonawczego, aby upewnić się, że wartość jest przypisana jest zgodna z wystąpienia tablicy.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="1b6fe-2657">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="1b6fe-2658">powoduje, że ostatnie przypisanie `System.ArrayTypeMismatchException` zostanie wygenerowany, ponieważ wystąpienie `ArrayList` nie mogą być przechowywane w elemencie `string[]`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="1b6fe-2659">Gdy właściwość lub indeksator zadeklarowanych w *struct_type* jest elementem docelowym przypisania, wyrażenia wystąpienia skojarzony z właściwością lub dostęp indeksatora musi być klasyfikowane jako zmienną.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="1b6fe-2660">Wyrażenia wystąpienia jest klasyfikowana jako wartość, czas powiązania błędu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="1b6fe-2661">Z powodu [dostęp do elementu członkowskiego](expressions.md#member-access), ta sama zasada dotyczy również pola.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="1b6fe-2662">Podane deklaracje:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2662">Given the declarations:</span></span>
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
<span data-ttu-id="1b6fe-2663">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="1b6fe-2664">przypisania do `p.X`, `p.Y`, `r.A`, i `r.B` są dozwolone, ponieważ `p` i `r` zmiennych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="1b6fe-2665">Jednak w przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="1b6fe-2666">przydziały są wszystkie nieprawidłowe, ponieważ `r.A` i `r.B` nie są zmiennymi.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="1b6fe-2667">Przydział złożony</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2667">Compound assignment</span></span>

<span data-ttu-id="1b6fe-2668">Jeśli lewy operand przypisania złożonego mają postać `E.P` lub `E[Ei]` gdzie `E` ma typ kompilacji `dynamic`, a następnie przypisanie dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="1b6fe-2669">W tym przypadku jest typu wyrażenia przypisania w czasie kompilacji `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania, na podstawie typu środowiska wykonawczego `E`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="1b6fe-2670">Operacja formularza `x op= y` jest przetwarzany przez zastosowanie operatora binarnego funkcja rozpoznawania przeciążeń ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) tak, jakby operacji został napisany `x op y`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="1b6fe-2671">Następnie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2671">Then,</span></span>

*  <span data-ttu-id="1b6fe-2672">Jeśli typ zwracany operatora niejawnej konwersji na typ `x`, operacja jest oceniane jako `x = x op y`, chyba że `x` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="1b6fe-2673">W przeciwnym razie, jeśli wybrany operator jest wstępnie zdefiniowanego operatora, jeśli typ zwracany operatora jest jawnie konwertowany na typ `x`i jeśli `y` jest niejawnie konwertowany na typ `x` lub operatora operator, przesunięcia, a następnie operacji jest obliczana jako `x = (T)(x op y)`, gdzie `T` jest typem `x`, chyba że `x` jest oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="1b6fe-2674">W przeciwnym razie przypisania złożonego jest nieprawidłowy i występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-2675">Termin "oceniane tylko raz," oznacza, że podczas obliczania `x op y`, wyniki żadnych składowych wyrażeń `x` zapisywane tymczasowo i następnie ponownie użyte podczas przeprowadzania przypisanie do `x`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="1b6fe-2676">Na przykład w przypisaniu `A()[B()] += C()`, gdzie `A` jest metodą, zwracając `int[]`, i `B` i `C` metod zwracanie `int`, te metody są wywoływane tylko raz, w kolejności `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="1b6fe-2677">Jeśli lewy operand przypisania złożonego jest dostęp do właściwości lub indeksatora dostępu, właściwość lub indeksator musi mieć zarówno `get` metody dostępu i `set` metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="1b6fe-2678">Jeśli nie jest to możliwe, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-2679">Druga reguła powyżej zezwala `x op= y` mogło zostać ocenione jako `x = (T)(x op y)` w określonych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="1b6fe-2680">Istnieje reguła w taki sposób, że wstępnie zdefiniowanych operatory mogą być używane jako operatory złożone, kiedy lewy operand jest typu `sbyte`, `byte`, `short`, `ushort`, lub `char`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="1b6fe-2681">Nawet gdy oba argumenty mają jeden z tych typów, wstępnie zdefiniowanych operatorów daje wynik o typie `int`, zgodnie z opisem w [binarne promocji liczbowych](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="1b6fe-2682">W związku z tym bez rzutowania nie byłoby możliwe Przypisz wynik do lewy operand.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="1b6fe-2683">Intuicyjne wpływu reguły dla wstępnie zdefiniowanego operatorów jest po prostu `x op= y` jest dozwolone, jeśli obie z `x op y` i `x = y` są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="1b6fe-2684">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2684">In the example</span></span>
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
<span data-ttu-id="1b6fe-2685">intuicyjne Przyczyna, dla każdego błędu jest, czy odpowiednie przypisanie proste również byłoby błąd.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="1b6fe-2686">Oznacza to, że przydział złożony, który obsługuje operacje zniesione operacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="1b6fe-2687">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="1b6fe-2688">operator podniesionym `+(int?,int?)` jest używany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="1b6fe-2689">Przypisanie zdarzenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2689">Event assignment</span></span>

<span data-ttu-id="1b6fe-2690">Jeśli lewy operand `+=` lub `-=` operator jest klasyfikowana jako dostęp do zdarzenia, a następnie wyrażenie jest obliczane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="1b6fe-2691">Wyrażenia wystąpienia, dostępu do zdarzeń jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="1b6fe-2692">Prawy operand `+=` lub `-=` operator jest obliczane i, w razie potrzeby konwertowane na typ lewy operand za pośrednictwem niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="1b6fe-2693">Metoda dostępu do zdarzeń zdarzenia jest wywoływana, z listy argumentów, składający się z prawy operand po dokonaniu oceny oraz, w razie potrzeby konwersji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="1b6fe-2694">Jeśli operator `+=`, `add` metody dostępu jest zalecane, jeśli był operator `-=`, `remove` zostanie wywołana metoda dostępu.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="1b6fe-2695">Wyrażenie przypisania zdarzeń nie przekazuje wartość.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="1b6fe-2696">W związku z tym, wyrażenie przypisania zdarzenia jest prawidłowy tylko w kontekście *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="1b6fe-2697">Wyrażenie</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2697">Expression</span></span>

<span data-ttu-id="1b6fe-2698">*Wyrażenie* jest *non_assignment_expression* lub *przypisania*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="1b6fe-2699">Wyrażenia stałe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2699">Constant expressions</span></span>

<span data-ttu-id="1b6fe-2700">A *constant_expression* jest wyrażeniem, które mogą zostać w pełni oszacowane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="1b6fe-2701">Musi być wyrażeniem stałym `null` literałem lub wartość składającą się z jednym z następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, lub dowolny typ wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="1b6fe-2702">W wyrażeniach stałych dopuszczalne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="1b6fe-2703">Literały (w tym `null` literału).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="1b6fe-2704">Odwołuje się do `const` składowych typu klasy i struktury.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="1b6fe-2705">Odwołania do elementów członkowskich typów wyliczeń.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="1b6fe-2706">Odwołuje się do `const` parametry lub zmienne lokalne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="1b6fe-2707">Ujęty w nawiasy wyrażeń podrzędnych, które są także wyrażeń stałych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="1b6fe-2708">Wyrażenia CAST, podać typ docelowy jest jednym z typów wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="1b6fe-2709">`checked` i `unchecked` wyrażeń</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="1b6fe-2710">Wyrażenia wartości domyślnych</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2710">Default value expressions</span></span>
*  <span data-ttu-id="1b6fe-2711">Wyrażenie Nameof</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2711">Nameof expressions</span></span>
*  <span data-ttu-id="1b6fe-2712">Wstępnie zdefiniowane `+`, `-`, `!`, i `~` operatorów jednoargumentowych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="1b6fe-2713">Wstępnie zdefiniowane `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, i `>=` operatory dwuargumentowe, pod warunkiem, każdy argument jest typu wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="1b6fe-2714">`?:` Operatora warunkowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="1b6fe-2715">Poniższe konwersje są dozwolone w wyrażeniach stałych:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="1b6fe-2716">Konwersje tożsamości</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2716">Identity conversions</span></span>
*  <span data-ttu-id="1b6fe-2717">Konwersje liczbowe</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2717">Numeric conversions</span></span>
*  <span data-ttu-id="1b6fe-2718">Konwersje wyliczenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="1b6fe-2719">Konwersje stałego wyrażenia</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="1b6fe-2720">Konwersje jawne i niejawne odwołanie, pod warunkiem że źródło konwersje jest wyrażeniem stałym, którego wynikiem jest wartość null.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="1b6fe-2721">Innych konwersji, w tym pakowanie, Rozpakowywanie i niejawne konwersje odwołań z innych niż null wartości nie są dozwolone w wyrażeń stałych.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="1b6fe-2722">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="1b6fe-2723">Inicjowanie i występuje błąd, ponieważ wymagana jest konwersja boxing.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="1b6fe-2724">Inicjowanie str występuje błąd, ponieważ niejawna konwersja odwołania z wartości innej niż null jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="1b6fe-2725">Zawsze, gdy wyrażenie spełnia wymagania wymienione powyżej, wyrażenie jest obliczane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="1b6fe-2726">Ta zasada obowiązuje, nawet wtedy, gdy wyrażenie jest większego wyrażenia zawierającego konstrukcje niestałe wyrażenie podrzędne.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="1b6fe-2727">Ocena kompilacji wyrażeń stałych używa te same reguły jako czasu wykonywania oceny wyrażeń stałą, z wyjątkiem tego, gdzie czasu wykonywania oceny będzie mieć zgłoszony wyjątek, ocena kompilacji powoduje błąd kompilacji występuje.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="1b6fe-2728">Chyba, że wyrażenie stałe jest jawnie umieszczona na `unchecked` kontekstu, przepełnienia, które występują w typu całkowitego operacjach arytmetycznych i konwersjach podczas obliczania kompilacji wyrażenia zawsze powodują błędy czasu kompilacji ([Wyrażeń stałych](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="1b6fe-2729">Wyrażenia stałe występują w kontekstach wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="1b6fe-2730">W tych kontekstach błąd kompilacji występuje, jeśli wyrażenie nie może zostać w pełni oszacowane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="1b6fe-2731">Deklaracje stałych ([stałe](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="1b6fe-2732">Deklaracji elementu członkowskiego wyliczenia ([typu wyliczeniowego](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="1b6fe-2733">Domyślne argumenty list parametrów formalnych ([parametry metody](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="1b6fe-2734">`case` etykiety `switch` — instrukcja ([instrukcji switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="1b6fe-2735">`goto case` instrukcje ([instrukcji goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="1b6fe-2736">Wymiar długości w wyrażenie tworzenia tablicy ([wyrażenie tworzenia tablicy](expressions.md#array-creation-expressions)) zawierającej inicjatora.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="1b6fe-2737">Atrybuty ([atrybuty](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="1b6fe-2738">Wyrażenie stałe w niejawnych konwersji ([konwersje niejawne wyrażenie stałe](conversions.md#implicit-constant-expression-conversions)) pozwala na stałe wyrażenie typu `int` są konwertowane na `sbyte`, `byte`, `short`, `ushort`, `uint`, lub `ulong`, o ile wartości stałe wyrażenia znajduje się w zakresie typu miejsca docelowego.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="1b6fe-2739">Wyrażenia logiczne</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2739">Boolean expressions</span></span>

<span data-ttu-id="1b6fe-2740">A *boolean_expression* jest wyrażeniem, które daje wynik o typie `bool`; albo bezpośrednio lub za pośrednictwem aplikacji `operator true` w pewnych kontekstach, jak określono poniżej.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="1b6fe-2741">Kontrolowanie wyrażenie warunkowe *if_statement* ([if — instrukcja](statements.md#the-if-statement)), *while_statement* ([while, instrukcja](statements.md#the-while-statement)), *do_statement* ([instrukcji](statements.md#the-do-statement)), lub *for_statement* ([dla instrukcji](statements.md#the-for-statement)) jest *boolean_ wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="1b6fe-2742">Kontrolowanie wyrażenie warunkowe `?:` — operator ([operator warunkowy](expressions.md#conditional-operator)) te same reguły jako *boolean_expression*, ale ze względów operatora jest klasyfikowany pierwszeństwo jako *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="1b6fe-2743">A *boolean_expression* `E` jest wymagana, aby można było utworzyć wartości typu `bool`, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="1b6fe-2744">Jeśli `E` jest niejawnie konwertowany na `bool` , a następnie w czasie wykonywania jest stosowany ten niejawnej konwersji.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="1b6fe-2745">W przeciwnym razie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest używana do znajdowania unikatowy stosowania najlepszych operator `true` na `E`, i że wykonanie jest stosowane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="1b6fe-2746">Jeśli zostanie znaleziony żaden operator takich, występuje błąd wiązania.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="1b6fe-2747">`DBBool` Typ struktury w [bazy danych typu boolean](structs.md#database-boolean-type) stanowi przykład typ, który implementuje `operator true` i `operator false`.</span><span class="sxs-lookup"><span data-stu-id="1b6fe-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
