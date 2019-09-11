---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876800"
---
# <a name="variables"></a><span data-ttu-id="454b4-101">Zmienne</span><span class="sxs-lookup"><span data-stu-id="454b4-101">Variables</span></span>

<span data-ttu-id="454b4-102">Zmienne reprezentują lokalizacje magazynu.</span><span class="sxs-lookup"><span data-stu-id="454b4-102">Variables represent storage locations.</span></span> <span data-ttu-id="454b4-103">Każda zmienna ma typ, który określa, jakie wartości mogą być przechowywane w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="454b4-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="454b4-104">C#jest językiem bezpiecznym dla typu, a C# kompilator gwarantuje, że wartości przechowywane w zmiennych są zawsze odpowiednim typem.</span><span class="sxs-lookup"><span data-stu-id="454b4-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="454b4-105">Wartość zmiennej można zmienić poprzez przypisanie lub użycie `++` operatorów i. `--`</span><span class="sxs-lookup"><span data-stu-id="454b4-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="454b4-106">Zmienna musi być ***ostatecznie przypisana*** ([przypisanie określone](variables.md#definite-assignment)), zanim będzie można uzyskać jej wartość.</span><span class="sxs-lookup"><span data-stu-id="454b4-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="454b4-107">Zgodnie z opisem w poniższych sekcjach zmienne są ***początkowo przypisywane*** lub ***początkowo***nieprzypisane.</span><span class="sxs-lookup"><span data-stu-id="454b4-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="454b4-108">Początkowo przypisana zmienna ma dobrze zdefiniowaną wartość początkową i jest zawsze traktowana jako ostatecznie przypisana.</span><span class="sxs-lookup"><span data-stu-id="454b4-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="454b4-109">Początkowo nieprzypisana zmienna nie ma wartości początkowej.</span><span class="sxs-lookup"><span data-stu-id="454b4-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="454b4-110">W przypadku początkowo nieprzypisanej zmiennej, która ma zostać uznana za ostatecznie przypisaną w określonej lokalizacji, przypisanie do zmiennej musi nastąpić w każdej możliwej ścieżce wykonywania prowadzącej do tej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="454b4-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="454b4-111">Kategorie zmiennych</span><span class="sxs-lookup"><span data-stu-id="454b4-111">Variable categories</span></span>

<span data-ttu-id="454b4-112">C#definiuje siedem kategorii zmiennych: zmiennych statycznych, zmiennych wystąpień, elementów tablicy, parametrów wartości, parametrów odwołania, parametrów wyjściowych i zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="454b4-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="454b4-113">W poniższych sekcjach opisano każdą z tych kategorii.</span><span class="sxs-lookup"><span data-stu-id="454b4-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="454b4-114">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="454b4-114">In the example</span></span>
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
<span data-ttu-id="454b4-115">`x`to zmienna statyczna, `y` czy zmienna wystąpienia, `v[0]` jest elementem tablicy, `a` jest parametrem `c` wartości, `b` jest parametrem referencyjnym, jest parametrem wyjściowym i `i` jest zmienną lokalną .</span><span class="sxs-lookup"><span data-stu-id="454b4-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="454b4-116">Zmienne statyczne</span><span class="sxs-lookup"><span data-stu-id="454b4-116">Static variables</span></span>

<span data-ttu-id="454b4-117">Pole zadeklarowane za pomocą `static` modyfikatora nosi nazwę ***zmiennej statycznej***.</span><span class="sxs-lookup"><span data-stu-id="454b4-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="454b4-118">Zmienna statyczna występuje przed wykonaniem konstruktora statycznego ([konstruktory statyczne](classes.md#static-constructors)) dla jego typu zawierającego i przestaje istnieć, gdy skojarzona domena aplikacji przestanie istnieć.</span><span class="sxs-lookup"><span data-stu-id="454b4-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="454b4-119">Początkowa wartość zmiennej statycznej jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu zmiennej.</span><span class="sxs-lookup"><span data-stu-id="454b4-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="454b4-120">W celach o określonym przeznaczeniu, zmienna statyczna jest traktowana jako początkowo przypisana.</span><span class="sxs-lookup"><span data-stu-id="454b4-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="454b4-121">Zmienne wystąpienia</span><span class="sxs-lookup"><span data-stu-id="454b4-121">Instance variables</span></span>

<span data-ttu-id="454b4-122">Pole zadeklarowane bez `static` modyfikatora jest nazywane ***zmienną wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="454b4-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="454b4-123">Zmienne wystąpień w klasach</span><span class="sxs-lookup"><span data-stu-id="454b4-123">Instance variables in classes</span></span>

<span data-ttu-id="454b4-124">Zmienna wystąpienia klasy występuje, gdy jest tworzone nowe wystąpienie tej klasy i przestaje istnieć, gdy nie ma odwołań do tego wystąpienia i destruktora wystąpienia (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="454b4-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="454b4-125">Początkowa wartość zmiennej wystąpienia klasy jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu zmiennej.</span><span class="sxs-lookup"><span data-stu-id="454b4-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="454b4-126">Na potrzeby określonego sprawdzania przypisania zmienna wystąpienia klasy jest traktowana jako początkowo przypisana.</span><span class="sxs-lookup"><span data-stu-id="454b4-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="454b4-127">Zmienne wystąpienia w strukturach</span><span class="sxs-lookup"><span data-stu-id="454b4-127">Instance variables in structs</span></span>

<span data-ttu-id="454b4-128">Zmienna wystąpienia struktury ma dokładnie taki sam okres istnienia, jak zmienna struktury, do której należy.</span><span class="sxs-lookup"><span data-stu-id="454b4-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="454b4-129">Innymi słowy, gdy zmienna typu struktury jest w stanie istnienia lub przestaje istnieć, więc należy wykonać te zmienne wystąpienia struktury.</span><span class="sxs-lookup"><span data-stu-id="454b4-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="454b4-130">Początkowy stan przypisania zmiennej wystąpienia struktury jest taki sam, jak w przypadku zawierającej ją zmiennej struktury.</span><span class="sxs-lookup"><span data-stu-id="454b4-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="454b4-131">Innymi słowy, gdy zmienna struktury jest uznawana za wstępnie przypisaną, więc ich zmienne wystąpień i gdy zmienna struktury jest uznawana za początkowo nieprzypisane, jego zmienne wystąpienia są podobnie nieprzypisane.</span><span class="sxs-lookup"><span data-stu-id="454b4-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="454b4-132">Elementy tablicy</span><span class="sxs-lookup"><span data-stu-id="454b4-132">Array elements</span></span>

<span data-ttu-id="454b4-133">Elementy tablicy są obecne w momencie utworzenia wystąpienia tablicy i przestaną istnieć, gdy nie ma odwołań do tego wystąpienia tablicy.</span><span class="sxs-lookup"><span data-stu-id="454b4-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="454b4-134">Wartość początkowa każdego z elementów tablicy jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu elementów tablicy.</span><span class="sxs-lookup"><span data-stu-id="454b4-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="454b4-135">Na potrzeby określonego sprawdzania przypisania element array jest traktowany jako wstępnie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="454b4-136">Parametry wartości</span><span class="sxs-lookup"><span data-stu-id="454b4-136">Value parameters</span></span>

<span data-ttu-id="454b4-137">Parametr zadeklarowany bez `ref` modyfikatora `out` or jest ***parametrem wartości***.</span><span class="sxs-lookup"><span data-stu-id="454b4-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="454b4-138">Parametr value występuje w przypadku wywołania elementu członkowskiego funkcji (metody, konstruktora wystąpienia, metody dostępu lub operatora) lub funkcji anonimowej, do której należy parametr, i jest inicjowany przy użyciu wartości argumentu podanego w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="454b4-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="454b4-139">Parametr wartości zwykle przestaje istnieć po zwrocie elementu członkowskiego funkcji lub funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="454b4-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="454b4-140">Jeśli jednak parametr value jest przechwytywany przez funkcję anonimową ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)), jego czas życia rozciąga się co najmniej do momentu, aż obiekt delegowany lub drzewo wyrażenia utworzone na podstawie tej funkcji anonimowej będzie kwalifikować się do wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="454b4-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="454b4-141">Na potrzeby określonego sprawdzania przypisania parametr value jest traktowany jako wstępnie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="454b4-142">Parametry odwołania</span><span class="sxs-lookup"><span data-stu-id="454b4-142">Reference parameters</span></span>

<span data-ttu-id="454b4-143">Parametr zadeklarowany za pomocą `ref` modyfikatora jest ***parametrem referencyjnym***.</span><span class="sxs-lookup"><span data-stu-id="454b4-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="454b4-144">Parametr Reference nie tworzy nowej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="454b4-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="454b4-145">Zamiast tego parametr odwołania reprezentuje tę samą lokalizację magazynu, co zmienna określona jako argument w elemencie członkowskim funkcji lub wywołaniu funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="454b4-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="454b4-146">W ten sposób wartość parametru odwołania jest zawsze taka sama jak zmienna bazowa.</span><span class="sxs-lookup"><span data-stu-id="454b4-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="454b4-147">Następujące określone reguły przypisywania dotyczą parametrów referencyjnych.</span><span class="sxs-lookup"><span data-stu-id="454b4-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="454b4-148">Zwróć uwagę na różne reguły parametrów wyjściowych opisanych w [parametrach wyjściowych](variables.md#output-parameters).</span><span class="sxs-lookup"><span data-stu-id="454b4-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="454b4-149">Zmienna musi być ostatecznie przypisana ([przypisanie](variables.md#definite-assignment)), zanim będzie mogła zostać przeniesiona jako parametr odwołania w składowej funkcji lub delegatze wywołania.</span><span class="sxs-lookup"><span data-stu-id="454b4-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="454b4-150">W składowej funkcji lub funkcji anonimowej jest uznawany za początkowo przypisany parametr Reference.</span><span class="sxs-lookup"><span data-stu-id="454b4-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="454b4-151">W ramach metody wystąpienia lub akcesora wystąpienia typu `this` struktury słowo kluczowe zachowuje się dokładnie jako parametr Reference typu struktury ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="454b4-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="454b4-152">Parametry wyjściowe</span><span class="sxs-lookup"><span data-stu-id="454b4-152">Output parameters</span></span>

<span data-ttu-id="454b4-153">Parametr zadeklarowany za pomocą `out` modyfikatora jest ***parametrem wyjściowym***.</span><span class="sxs-lookup"><span data-stu-id="454b4-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="454b4-154">Parametr wyjściowy nie tworzy nowej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="454b4-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="454b4-155">Zamiast tego parametr wyjściowy reprezentuje tę samą lokalizację magazynu, co zmienna określona jako argument elementu członkowskiego funkcji lub delegata wywołania.</span><span class="sxs-lookup"><span data-stu-id="454b4-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="454b4-156">W ten sposób wartość parametru wyjściowego jest zawsze taka sama jak zmienna bazowa.</span><span class="sxs-lookup"><span data-stu-id="454b4-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="454b4-157">Następujące określone reguły przypisywania mają zastosowanie do parametrów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="454b4-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="454b4-158">Zwróć uwagę na różne reguły parametrów referencyjnych opisanych w [parametrach referencyjnych](variables.md#reference-parameters).</span><span class="sxs-lookup"><span data-stu-id="454b4-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="454b4-159">Zmienna nie musi być ostatecznie przypisana, zanim będzie mogła zostać przeniesiona jako parametr wyjściowy w składowej funkcji lub w wywołaniu obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="454b4-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="454b4-160">Po normalnym zakończeniu elementu członkowskiego funkcji lub delegatze wywołania Każda zmienna, która została przeniesiona jako parametr wyjściowy, jest uznawana za przypisaną w tej ścieżce wykonywania.</span><span class="sxs-lookup"><span data-stu-id="454b4-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="454b4-161">W ramach składowej funkcji lub funkcji anonimowej, parametr wyjściowy jest uznawany za początkowo nieprzypisane.</span><span class="sxs-lookup"><span data-stu-id="454b4-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="454b4-162">Każdy parametr wyjściowy elementu członkowskiego funkcji lub funkcji anonimowej musi być ostatecznie przypisany ([przypisanie określone](variables.md#definite-assignment)) zanim element członkowski funkcji lub funkcja anonimowa normalnie zwróci wynik.</span><span class="sxs-lookup"><span data-stu-id="454b4-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="454b4-163">W konstruktorze wystąpienia typu `this` struktury słowo kluczowe zachowuje się dokładnie jako parametr wyjściowy typu struktury ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="454b4-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="454b4-164">Zmienne lokalne</span><span class="sxs-lookup"><span data-stu-id="454b4-164">Local variables</span></span>

<span data-ttu-id="454b4-165">***Zmienna lokalna*** jest zadeklarowana przez element *local_variable_declaration*, który może wystąpić w *bloku*, *for_statement*, *switch_statement* lub *using_statement*; lub *foreach_statement* lub *specific_catch_clause* dla *try_statement*.</span><span class="sxs-lookup"><span data-stu-id="454b4-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="454b4-166">Okres istnienia zmiennej lokalnej jest częścią wykonywania programu, podczas której zagwarantowano, że magazyn jest zarezerwowany dla tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="454b4-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="454b4-167">Ten okres istnienia rozszerza co najmniej od wpisu do *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_catch_clause* , z którym jest skojarzony, do wykonywanie tego *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_catch_clause* kończą się w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="454b4-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="454b4-168">(Wprowadzenie do zamkniętego *bloku* lub wywołanie metody zawiesza się, ale nie kończy wykonywania bieżącego *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_ catch_clause*.) Jeśli zmienna lokalna jest przechwytywana przez funkcję anonimową ([przechwycone zmienne zewnętrzne](expressions.md#captured-outer-variables)), jego okres istnienia rozszerza co najmniej do momentu delegata lub drzewa wyrażenia utworzonego na podstawie funkcji anonimowej, wraz z innymi obiektami, które odwołują się do przechwycona zmienna kwalifikuje się do wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="454b4-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="454b4-169">Jeśli *blok*nadrzędny, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_catch_clause* jest wprowadzany rekursywnie, tworzone jest nowe wystąpienie zmiennej lokalnej czas i jego *local_variable_initializer*, jeśli istnieją, są oceniane za każdym razem.</span><span class="sxs-lookup"><span data-stu-id="454b4-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="454b4-170">Zmienna lokalna wprowadzona przez *local_variable_declaration* nie jest inicjowana automatycznie i w związku z tym nie ma wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="454b4-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="454b4-171">Na potrzeby określonego sprawdzania przypisania, zmienna lokalna wprowadzona przez *local_variable_declaration* jest uznawana za początkowo nieprzypisane.</span><span class="sxs-lookup"><span data-stu-id="454b4-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="454b4-172">*Local_variable_declaration* może zawierać *local_variable_initializer*, w tym przypadku zmienna jest uważana za ostatecznie przypisaną tylko po wyrażeniu inicjowania ([instrukcje deklaracji](variables.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="454b4-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="454b4-173">W zakresie zmiennej lokalnej wprowadzonej przez *local_variable_declaration*jest to błąd czasu kompilacji, który odwołuje się do tej zmiennej lokalnej w pozycji tekstowej, która poprzedza jej *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="454b4-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="454b4-174">Jeśli deklaracja zmiennej lokalnej jest niejawna ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)), jest również błędem do odwoływania się do zmiennej w ramach jej *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="454b4-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="454b4-175">Zmienna lokalna wprowadzona przez *foreach_statement* lub *specific_catch_clause* jest uznawana za ostatecznie przypisaną w całym zakresie.</span><span class="sxs-lookup"><span data-stu-id="454b4-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="454b4-176">Rzeczywisty okres istnienia zmiennej lokalnej jest zależny od implementacji.</span><span class="sxs-lookup"><span data-stu-id="454b4-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="454b4-177">Na przykład kompilator może statycznie określić, że zmienna lokalna w bloku jest używana tylko dla małej części tego bloku.</span><span class="sxs-lookup"><span data-stu-id="454b4-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="454b4-178">Korzystając z tej analizy, kompilator może wygenerować kod, który powoduje, że magazyn zmiennej ma krótszy okres istnienia niż jego blok zawierający.</span><span class="sxs-lookup"><span data-stu-id="454b4-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="454b4-179">Magazyn, do którego odwołuje się lokalna zmienna referencyjna, jest odzyskiwana niezależnie od okresu istnienia lokalnej zmiennej odwołania ([Automatyczne zarządzanie pamięcią](basic-concepts.md#automatic-memory-management)).</span><span class="sxs-lookup"><span data-stu-id="454b4-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="454b4-180">Wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="454b4-180">Default values</span></span>

<span data-ttu-id="454b4-181">Następujące kategorie zmiennych są automatycznie inicjowane na wartości domyślne:</span><span class="sxs-lookup"><span data-stu-id="454b4-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="454b4-182">Zmienne statyczne.</span><span class="sxs-lookup"><span data-stu-id="454b4-182">Static variables.</span></span>
*  <span data-ttu-id="454b4-183">Zmienne wystąpienia klasy Instances.</span><span class="sxs-lookup"><span data-stu-id="454b4-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="454b4-184">Elementy tablicy.</span><span class="sxs-lookup"><span data-stu-id="454b4-184">Array elements.</span></span>

<span data-ttu-id="454b4-185">Wartość domyślna zmiennej zależy od typu zmiennej i jest określana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="454b4-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="454b4-186">Dla zmiennej *value_type*wartość domyślna jest taka sama jak wartość obliczana przez konstruktora domyślnego *value_type*([konstruktory domyślne](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="454b4-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="454b4-187">Dla zmiennej *reference_type*wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="454b4-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="454b4-188">Inicjowanie do wartości domyślnych jest zwykle wykonywane przez zainicjowanie pamięci przez Menedżera pamięci lub moduł wyrzucania elementów bezużytecznych do wszystkich-bitów-zero przed przydzieleniem do użycia.</span><span class="sxs-lookup"><span data-stu-id="454b4-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="454b4-189">Z tego powodu wygodnie jest użyć wszystkich-bitów-zero do reprezentowania odwołania o wartości null.</span><span class="sxs-lookup"><span data-stu-id="454b4-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="454b4-190">Przypisanie określone</span><span class="sxs-lookup"><span data-stu-id="454b4-190">Definite assignment</span></span>

<span data-ttu-id="454b4-191">W danej lokalizacji kodu wykonywalnego elementu członkowskiego, zmienna jest określana jako ***czasowo przypisana*** , jeśli kompilator może udowodnić, przez określoną analityczny przepływ statyczny ([precyzyjne reguły określania określonego przypisania](variables.md#precise-rules-for-determining-definite-assignment)), że zmienna Program został zainicjowany automatycznie lub miał miejsce docelowe co najmniej jedno przypisanie.</span><span class="sxs-lookup"><span data-stu-id="454b4-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="454b4-192">Nieformalnie określone reguły przypisywania są następujące:</span><span class="sxs-lookup"><span data-stu-id="454b4-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="454b4-193">Początkowo przypisana zmienna ([wstępnie przypisane zmienne](variables.md#initially-assigned-variables)) jest zawsze uznawana za ostatecznie przypisaną.</span><span class="sxs-lookup"><span data-stu-id="454b4-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="454b4-194">Początkowo nieprzypisane zmienne (początkowo nieprzypisane[zmienne](variables.md#initially-unassigned-variables)) są uznawane za ostatecznie przypisane w danej lokalizacji, jeśli wszystkie możliwe ścieżki wykonywania prowadzące do tej lokalizacji zawierają co najmniej jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="454b4-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="454b4-195">Proste przypisanie ([przypisanie proste](expressions.md#simple-assignment)), w którym zmienna jest argumentem lewego operandu.</span><span class="sxs-lookup"><span data-stu-id="454b4-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="454b4-196">Wyrażenie wywołania (wyrażenia[wywołania](expressions.md#invocation-expressions)) lub wyrażenie tworzenia obiektu ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)), które przekazuje zmienną jako parametr wyjściowy.</span><span class="sxs-lookup"><span data-stu-id="454b4-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="454b4-197">Dla zmiennej lokalnej deklaracja zmiennej lokalnej ([lokalnych deklaracji zmiennych](statements.md#local-variable-declarations)), która zawiera inicjator zmiennych.</span><span class="sxs-lookup"><span data-stu-id="454b4-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="454b4-198">Formalna Specyfikacja zgodna z powyższymi regułami nieformalnymi jest opisana w [wstępnie przypisanych zmiennych](variables.md#initially-assigned-variables), [początkowo nieprzypisane zmienne](variables.md#initially-unassigned-variables)i [precyzyjne reguły określania przypisania](variables.md#precise-rules-for-determining-definite-assignment).</span><span class="sxs-lookup"><span data-stu-id="454b4-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="454b4-199">Określone Stany przypisania zmiennych wystąpienia zmiennej *struct_type* są śledzone indywidualnie, a także w sposób zbiorczy.</span><span class="sxs-lookup"><span data-stu-id="454b4-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="454b4-200">Oprócz powyższych reguł stosowane są następujące reguły do zmiennych *struct_type* i ich zmiennych wystąpień:</span><span class="sxs-lookup"><span data-stu-id="454b4-200">In addition to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="454b4-201">Zmienna wystąpienia jest uznawana za ostatecznie przypisaną, jeśli jej zawierająca zmienną *struct_type* jest uznawana za ostatecznie przypisaną.</span><span class="sxs-lookup"><span data-stu-id="454b4-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="454b4-202">Zmienna *struct_type* jest uznawana za ostatecznie przypisaną, jeśli każda z jej zmiennych wystąpienia jest uznawana za ostatecznie przypisaną.</span><span class="sxs-lookup"><span data-stu-id="454b4-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="454b4-203">Określone przypisanie jest wymaganiem w następujących kontekstach:</span><span class="sxs-lookup"><span data-stu-id="454b4-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="454b4-204">Zmienna musi być ostatecznie przypisana w każdej lokalizacji, w której zostanie uzyskana wartość.</span><span class="sxs-lookup"><span data-stu-id="454b4-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="454b4-205">Gwarantuje to, że niezdefiniowane wartości nigdy nie wystąpią.</span><span class="sxs-lookup"><span data-stu-id="454b4-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="454b4-206">Wystąpienie zmiennej w wyrażeniu jest traktowane do uzyskania wartości zmiennej, z wyjątkiem przypadków, gdy</span><span class="sxs-lookup"><span data-stu-id="454b4-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="454b4-207">Zmienna jest lewym operandem prostego przypisywania,</span><span class="sxs-lookup"><span data-stu-id="454b4-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="454b4-208">Zmienna jest przenoszona jako parametr wyjściowy lub</span><span class="sxs-lookup"><span data-stu-id="454b4-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="454b4-209">Zmienna jest zmienną *struct_type* i występuje jako lewy operand dostępu do elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="454b4-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="454b4-210">Zmienna musi być ostatecznie przypisana w każdej lokalizacji, w której jest przenoszona jako parametr referencyjny.</span><span class="sxs-lookup"><span data-stu-id="454b4-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="454b4-211">Daje to pewność, że wywoływany element członkowski funkcji może uwzględniać początkowo przypisany parametr Reference.</span><span class="sxs-lookup"><span data-stu-id="454b4-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="454b4-212">Wszystkie parametry wyjściowe elementu członkowskiego funkcji muszą być ostatecznie przypisane do każdej lokalizacji, w której element członkowski funkcji zwraca ( `return` za pomocą instrukcji lub przez wykonanie do końca treści elementu członkowskiego funkcji).</span><span class="sxs-lookup"><span data-stu-id="454b4-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="454b4-213">Gwarantuje to, że elementy członkowskie funkcji nie zwracają niezdefiniowanych wartości w parametrach wyjściowych, dzięki czemu kompilator powinien wziąć pod uwagę wywołanie elementu członkowskiego, który pobiera zmienną jako parametr wyjściowy równoważne przypisanie do zmiennej.</span><span class="sxs-lookup"><span data-stu-id="454b4-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="454b4-214">Zmienna konstruktora wystąpienia struct_type musi być ostatecznie przypisana w każdej lokalizacji, w której Konstruktor wystąpienia zwraca. `this`</span><span class="sxs-lookup"><span data-stu-id="454b4-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="454b4-215">Wstępnie przypisane zmienne</span><span class="sxs-lookup"><span data-stu-id="454b4-215">Initially assigned variables</span></span>

<span data-ttu-id="454b4-216">Następujące kategorie zmiennych są klasyfikowane jako początkowo przypisane:</span><span class="sxs-lookup"><span data-stu-id="454b4-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="454b4-217">Zmienne statyczne.</span><span class="sxs-lookup"><span data-stu-id="454b4-217">Static variables.</span></span>
*  <span data-ttu-id="454b4-218">Zmienne wystąpienia klasy Instances.</span><span class="sxs-lookup"><span data-stu-id="454b4-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="454b4-219">Zmienne wystąpienia wstępnie przypisanych zmiennych struktury.</span><span class="sxs-lookup"><span data-stu-id="454b4-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="454b4-220">Elementy tablicy.</span><span class="sxs-lookup"><span data-stu-id="454b4-220">Array elements.</span></span>
*  <span data-ttu-id="454b4-221">Parametry wartości.</span><span class="sxs-lookup"><span data-stu-id="454b4-221">Value parameters.</span></span>
*  <span data-ttu-id="454b4-222">Parametry odwołania.</span><span class="sxs-lookup"><span data-stu-id="454b4-222">Reference parameters.</span></span>
*  <span data-ttu-id="454b4-223">Zmienne zadeklarowane w `catch` klauzuli `foreach` lub instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="454b4-224">Początkowo nieprzypisane zmienne</span><span class="sxs-lookup"><span data-stu-id="454b4-224">Initially unassigned variables</span></span>

<span data-ttu-id="454b4-225">Następujące kategorie zmiennych są klasyfikowane jako początkowo nieprzypisane:</span><span class="sxs-lookup"><span data-stu-id="454b4-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="454b4-226">Zmienne wystąpienia początkowo nieprzypisane zmienne struktury.</span><span class="sxs-lookup"><span data-stu-id="454b4-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="454b4-227">Parametry wyjściowe, w tym `this` zmienna konstruktorów wystąpień struktury.</span><span class="sxs-lookup"><span data-stu-id="454b4-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="454b4-228">Zmienne lokalne, z wyjątkiem tych zadeklarowanych `catch` w klauzuli `foreach` lub instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="454b4-229">Precyzyjne reguły określania określonego przypisania</span><span class="sxs-lookup"><span data-stu-id="454b4-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="454b4-230">Aby określić, że każda użyta zmienna jest ostatecznie przypisana, kompilator musi użyć procesu, który jest równoważny z opisanym w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="454b4-231">Kompilator przetwarza treść każdego elementu członkowskiego funkcji, który ma co najmniej jedną wstępnie przypisaną zmienną.</span><span class="sxs-lookup"><span data-stu-id="454b4-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="454b4-232">Dla każdej początkowo nieprzypisanej zmiennej *v*kompilator określa ***określony stan przypisania*** dla *v* w każdym z następujących punktów w składowej funkcji:</span><span class="sxs-lookup"><span data-stu-id="454b4-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="454b4-233">Na początku każdej instrukcji</span><span class="sxs-lookup"><span data-stu-id="454b4-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="454b4-234">W punkcie końcowym ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)) każdej instrukcji</span><span class="sxs-lookup"><span data-stu-id="454b4-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="454b4-235">Na każdym łuku, który przenosi formant do innej instrukcji lub do punktu końcowego instrukcji</span><span class="sxs-lookup"><span data-stu-id="454b4-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="454b4-236">Na początku każdego wyrażenia</span><span class="sxs-lookup"><span data-stu-id="454b4-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="454b4-237">Na końcu każdego wyrażenia</span><span class="sxs-lookup"><span data-stu-id="454b4-237">At the end of each expression</span></span>

<span data-ttu-id="454b4-238">Określony stan przypisania *v* może być:</span><span class="sxs-lookup"><span data-stu-id="454b4-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="454b4-239">W nieskończony sposób.</span><span class="sxs-lookup"><span data-stu-id="454b4-239">Definitely assigned.</span></span> <span data-ttu-id="454b4-240">Oznacza *to, że* dla wszystkich możliwych przepływów sterowania do tego punktu przypisano wartość.</span><span class="sxs-lookup"><span data-stu-id="454b4-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="454b4-241">Nie jest on ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-241">Not definitely assigned.</span></span> <span data-ttu-id="454b4-242">W przypadku stanu zmiennej na końcu wyrażenia typu `bool`, stan zmiennej, która nie jest ostatecznie przypisany, może (ale niekoniecznie) wchodzić do jednego z następujących podstanów:</span><span class="sxs-lookup"><span data-stu-id="454b4-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="454b4-243">W nieskończony sposób przypisany po prawdziwe wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="454b4-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="454b4-244">Ten stan wskazuje, że wartość *v* jest ostatecznie przypisana, jeśli wyrażenie logiczne jest oceniane jako prawdziwe, ale nie musi być przypisywane, jeśli wyrażenie logiczne zostało ocenione jako FAŁSZ.</span><span class="sxs-lookup"><span data-stu-id="454b4-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="454b4-245">Jest on przypisany w nieskończoność po wyrażeniu false.</span><span class="sxs-lookup"><span data-stu-id="454b4-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="454b4-246">Ten stan wskazuje, że w przypadku wyrażenia logicznego ocenianego jako FAŁSZ jest przypisywana wartość " *v* ", ale nie jest to konieczne, jeśli wyrażenie logiczne jest oceniane jako true.</span><span class="sxs-lookup"><span data-stu-id="454b4-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="454b4-247">Poniższe reguły określają, jak stan zmiennej *v* jest określany w każdej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="454b4-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="454b4-248">Ogólne reguły dla instrukcji</span><span class="sxs-lookup"><span data-stu-id="454b4-248">General rules for statements</span></span>

*  <span data-ttu-id="454b4-249">wartość *v* nie jest ostatecznie przypisana na początku treści elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="454b4-250">*v* jest ostatecznie przypisana na początku dowolnej nieosiągalnej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="454b4-251">Określony stan przypisania *v* na początku każdej innej instrukcji jest określany przez sprawdzenie, czy stan przypisania jest określony *, na wszystkich* transferach przepływów sterowania przeznaczonych dla początku tej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="454b4-252">Jeśli (i tylko wtedy, gdy) *v* jest ostatecznie przypisany do wszystkich takich transferów przepływów sterowania, a następnie *v* jest ostatecznie przypisana na początku instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="454b4-253">Zestaw możliwych transferów przepływów sterowania jest określany w taki sam sposób, jak sprawdzanie osiągalności instrukcji ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="454b4-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="454b4-254">Określony stan przypisania *v* w punkcie `checked`końcowym bloku, `do`, `while` `if` `unchecked` `for` ,,`foreach`,,,,, lub `lock` `using` `switch`instrukcja jest określana przez sprawdzenie, czy stan przypisania jest określony *, na wszystkich* transferach przepływów sterowania przeznaczonych dla punktu końcowego tej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="454b4-255">Jeśli wartość *v* jest ostatecznie przypisana do wszystkich takich transferów przepływów sterowania, to *v* jest ostatecznie przypisana w punkcie końcowym instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="454b4-256">Przypadku wartość *v* nie jest ostatecznie przypisana w punkcie końcowym instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="454b4-257">Zestaw możliwych transferów przepływów sterowania jest określany w taki sam sposób, jak sprawdzanie osiągalności instrukcji ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="454b4-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="454b4-258">Blokuj instrukcje, sprawdzone i niesprawdzone instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="454b4-259">Określony stan przypisania *v* na formancie transfer do pierwszej instrukcji z listy instrukcji w bloku (lub do punktu końcowego bloku, jeśli lista instrukcji jest pusta) jest taka sama jak w przypadku instrukcji przypisania " *v* " przed blokiem , `checked`, lub `unchecked` .</span><span class="sxs-lookup"><span data-stu-id="454b4-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="454b4-260">Instrukcje wyrażeń</span><span class="sxs-lookup"><span data-stu-id="454b4-260">Expression statements</span></span>

<span data-ttu-id="454b4-261">Dla instrukcji wyrażenia *stmt* , która składa się z *wyrażenia expression:*</span><span class="sxs-lookup"><span data-stu-id="454b4-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="454b4-262">*v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-263">Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, jest on ostatecznie przypisywany w punkcie końcowym *stmt*; przypadku nie jest on ostatecznie przypisywany w punkcie końcowym *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="454b4-264">Deklaracje deklaracji</span><span class="sxs-lookup"><span data-stu-id="454b4-264">Declaration statements</span></span>

*  <span data-ttu-id="454b4-265">Jeśli *stmt* jest instrukcją deklaracji bez inicjatorów, wówczas *v* ma ten sam, określony stan przypisania w punkcie końcowym *stmt* , jak na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-266">Jeśli *stmt* jest instrukcją deklaracji z inicjatorami, oznacza to, że określony stan przypisania dla *v* jest określany tak, jakby *stmt* były listą instrukcji z jedną instrukcją przypisania dla każdej deklaracji z inicjatorem (w kolejności Deklaracja).</span><span class="sxs-lookup"><span data-stu-id="454b4-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="454b4-267">If — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-267">If statements</span></span>

<span data-ttu-id="454b4-268">Dla instrukcji stmt formularza: `if`</span><span class="sxs-lookup"><span data-stu-id="454b4-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="454b4-269">*v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-270">Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, to jest on ostatecznie przypisany do transferu przepływu sterowania do *then_stmt* i do *else_stmt* lub do punktu końcowego *stmt* , jeśli nie ma klauzuli else.</span><span class="sxs-lookup"><span data-stu-id="454b4-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="454b4-271">Jeśli funkcja *v* ma stan "czas przypisania po prawdziwym wyrażeniu" na końcu wyrażenia *, to*jest on ostatecznie przypisany do transferu przepływu sterowania do *then_stmt*i nie jest on ostatecznie przypisywany w przepływie sterowania do *else_ stmt* lub do punktu końcowego *stmt* , jeśli nie ma klauzuli else.</span><span class="sxs-lookup"><span data-stu-id="454b4-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="454b4-272">Jeśli w wersji *v* znajduje się stan "czas przypisania po wartości false" na końcu *wyrażenia, to*jest on ostatecznie przypisany do transferu przepływu sterowania do *else_stmt*i nie jest on ostatecznie przypisywany w przepływie sterowania do *then_stmt* .</span><span class="sxs-lookup"><span data-stu-id="454b4-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="454b4-273">Jest on ostatecznie przypisywany w punkcie końcowym *stmt* , jeśli i tylko wtedy, gdy jest on ostatecznie przypisywany w punkcie końcowym *then_stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="454b4-274">W przeciwnym razie *v* jest uznawana za nieczasowo przypisane w przepływie sterowania do *then_stmt* lub *else_stmt*lub do punktu końcowego *stmt* , jeśli nie istnieje klauzula else.</span><span class="sxs-lookup"><span data-stu-id="454b4-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="454b4-275">Instrukcji switch</span><span class="sxs-lookup"><span data-stu-id="454b4-275">Switch statements</span></span>

<span data-ttu-id="454b4-276">W instrukcji stmt *z wyrażeniem kontrolnym:* `switch`</span><span class="sxs-lookup"><span data-stu-id="454b4-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="454b4-277">Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-278">Określony stan przypisania *v* w przepływie sterowania przeniesieniu do osiągalnej listy instrukcji bloku przełączenia jest taki sam, jak określony stan przypisania *v* na końcu *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="454b4-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="454b4-279">While — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-279">While statements</span></span>

<span data-ttu-id="454b4-280">W przypadku instrukcji stmt formularza: `while`</span><span class="sxs-lookup"><span data-stu-id="454b4-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="454b4-281">*v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-282">Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, to jest on ostatecznie przypisany do transferu przepływu sterowania do *while_body* i do punktu końcowego *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="454b4-283">Jeśli funkcja *v* ma stan "czas przypisania po prawdziwym wyrażeniu" na końcu wyrażenia *, to*jest on ostatecznie przypisany do przepływu sterowania przepływem do *while_body*, ale nie jest on ostatecznie przypisywany w punkcie końcowym *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="454b4-284">Jeśli w wersji *v* znajduje się stan "czas przypisania po wartości false" na końcu *wyrażenia, to*jest on ostatecznie przypisany do przepływu sterowania, który jest przesyłany do punktu końcowego *stmt*, ale nie jest on ostatecznie przypisany w przepływie sterowania do *while _body*.</span><span class="sxs-lookup"><span data-stu-id="454b4-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="454b4-285">Instrukcje do</span><span class="sxs-lookup"><span data-stu-id="454b4-285">Do statements</span></span>

<span data-ttu-id="454b4-286">W przypadku instrukcji stmt formularza: `do`</span><span class="sxs-lookup"><span data-stu-id="454b4-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="454b4-287">*v* ma ten sam nieograniczony stan przypisania w przepływie sterowania, od początku *stmt* do *do_body* , jak na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-288">*v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak w punkcie końcowym *do_body*.</span><span class="sxs-lookup"><span data-stu-id="454b4-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="454b4-289">Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, to jest on ostatecznie przypisany do przepływu sterowania, który jest przenoszony do punktu końcowego *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="454b4-290">Jeśli w wersji *v* znajduje się stan "czas przypisania po wartości false" na końcu *wyrażenia, to*jest on ostatecznie przypisany do przepływu sterowania, który jest przesyłany do punktu końcowego *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="454b4-291">For — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-291">For statements</span></span>

<span data-ttu-id="454b4-292">Określone sprawdzanie przypisania dla `for` instrukcji formularza:</span><span class="sxs-lookup"><span data-stu-id="454b4-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="454b4-293">wykonuje się tak, jakby instrukcja była zapisywana:</span><span class="sxs-lookup"><span data-stu-id="454b4-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="454b4-294">Jeśli *for_condition* zostanie pominięty z `for` instrukcji, oszacowanie ostatecznego przypisania jest realizowane tak, jakby *for_condition* zostały zastąpione `true` w powyższym rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="454b4-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="454b4-295">Instrukcje Break, Continue i goto</span><span class="sxs-lookup"><span data-stu-id="454b4-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="454b4-296">Określony stan przypisania *v* w przeniesieniu przepływu `break`sterowania spowodowany przez, `continue`, lub `goto` instrukcji jest taki sam jak określony stan przypisania *v* na początku instrukcji.</span><span class="sxs-lookup"><span data-stu-id="454b4-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="454b4-297">Throw — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-297">Throw statements</span></span>

<span data-ttu-id="454b4-298">Dla instrukcji *stmt* formularza</span><span class="sxs-lookup"><span data-stu-id="454b4-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="454b4-299">Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak określony stan przypisania *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="454b4-300">Return — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-300">Return statements</span></span>

<span data-ttu-id="454b4-301">Dla instrukcji *stmt* formularza</span><span class="sxs-lookup"><span data-stu-id="454b4-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="454b4-302">Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak określony stan przypisania *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-303">Jeśli *v* jest parametrem wyjściowym, musi być ostatecznie przypisany:</span><span class="sxs-lookup"><span data-stu-id="454b4-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="454b4-304">po wyjściu</span><span class="sxs-lookup"><span data-stu-id="454b4-304">after *expr*</span></span>
    * <span data-ttu-id="454b4-305">lub na `finally` końcu bloku `try` - lub`try` ,który`return` należy do instrukcji. - `finally` `catch` - `finally`</span><span class="sxs-lookup"><span data-stu-id="454b4-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="454b4-306">W przypadku instrukcji stmt formularza:</span><span class="sxs-lookup"><span data-stu-id="454b4-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="454b4-307">Jeśli *v* jest parametrem wyjściowym, musi być ostatecznie przypisany:</span><span class="sxs-lookup"><span data-stu-id="454b4-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="454b4-308">przed *stmt*</span><span class="sxs-lookup"><span data-stu-id="454b4-308">before *stmt*</span></span>
    * <span data-ttu-id="454b4-309">lub na `finally` końcu bloku `try` - lub`try` ,który`return` należy do instrukcji. - `finally` `catch` - `finally`</span><span class="sxs-lookup"><span data-stu-id="454b4-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="454b4-310">Instrukcje try-catch</span><span class="sxs-lookup"><span data-stu-id="454b4-310">Try-catch statements</span></span>

<span data-ttu-id="454b4-311">W przypadku instrukcji *stmt* formularza:</span><span class="sxs-lookup"><span data-stu-id="454b4-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="454b4-312">Określony stan przypisania *v* na początku *try_block* jest taki sam, jak określony stan przypisania *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-313">Określony stan przypisania *v* na początku *catch_block_i* ( *dla każdej z*nich) jest taki sam jak określony stan przypisania *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-314">Określony stan przypisania *v* w punkcie końcowym *stmt* jest w przypadku, gdy (i tylko wtedy, gdy) *v* jest ostatecznie przypisywany w punkcie końcowym *try_block* i każdy *catch_block_i* (za każdym *i* z 1 do *n* ).</span><span class="sxs-lookup"><span data-stu-id="454b4-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="454b4-315">Try-finally — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-315">Try-finally statements</span></span>

<span data-ttu-id="454b4-316">W przypadku instrukcji stmt formularza: `try`</span><span class="sxs-lookup"><span data-stu-id="454b4-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="454b4-317">Określony stan przypisania *v* na początku *try_block* jest taki sam, jak określony stan przypisania *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-318">Określony stan przypisania *v* na początku *finally_block* jest taki sam, jak określony stan przypisania *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-319">Określony stan przypisania *v* w punkcie końcowym *stmt* jest ostatecznie przypisany, jeśli (i tylko wtedy, gdy) co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="454b4-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="454b4-320">wartość *v* jest ostatecznie przypisana w punkcie końcowym *try_block*</span><span class="sxs-lookup"><span data-stu-id="454b4-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="454b4-321">wartość *v* jest ostatecznie przypisana w punkcie końcowym *finally_block*</span><span class="sxs-lookup"><span data-stu-id="454b4-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="454b4-322">Jeśli przeprowadzono transfer przepływu sterowania `goto` (na przykład instrukcja), który rozpoczyna się w *try_block*i kończą się poza *try_block*, wówczas *v* jest również uznawany za oczekiwany w przypadku, gdy *v* jest ostatecznie przypisano w punkcie końcowym *finally_block*.</span><span class="sxs-lookup"><span data-stu-id="454b4-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="454b4-323">(To nie jest tylko wtedy, gdy w przypadku, gdy *v* jest ostatecznie przypisany z innego powodu tego transferu przepływu sterowania, nadal jest uznawany za ostatecznie przypisany).</span><span class="sxs-lookup"><span data-stu-id="454b4-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="454b4-324">Try-catch-finally — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-324">Try-catch-finally statements</span></span>

<span data-ttu-id="454b4-325">Analiza określona przez przypisanie dla `try` - `catch` instrukcjiwpostaci-: `finally`</span><span class="sxs-lookup"><span data-stu-id="454b4-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="454b4-326">wykonuje się tak, jakby `try` instrukcja była - `finally` instrukcją otaczającą `try` - `catch` instrukcję:</span><span class="sxs-lookup"><span data-stu-id="454b4-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="454b4-327">Poniższy przykład ilustruje sposób, w jaki różne bloki `try` instrukcji ([Instrukcja try](statements.md#the-try-statement)) wpływają na określone przypisanie.</span><span class="sxs-lookup"><span data-stu-id="454b4-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a><span data-ttu-id="454b4-328">Instrukcja foreach</span><span class="sxs-lookup"><span data-stu-id="454b4-328">Foreach statements</span></span>

<span data-ttu-id="454b4-329">W przypadku instrukcji stmt formularza: `foreach`</span><span class="sxs-lookup"><span data-stu-id="454b4-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="454b4-330">Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-331">Określony stan przypisania *v* w przepływie sterowania transfer do *embedded_statement* lub do punktu końcowego *stmt* jest taki sam jak stan *v* na końcu *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="454b4-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="454b4-332">Using — instrukcje</span><span class="sxs-lookup"><span data-stu-id="454b4-332">Using statements</span></span>

<span data-ttu-id="454b4-333">W przypadku instrukcji stmt formularza: `using`</span><span class="sxs-lookup"><span data-stu-id="454b4-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="454b4-334">Określony stan przypisania *v* na początku *resource_acquisition* jest taki sam jak stan *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-335">Określony stan przypisania *v* w przeniesieniu przepływu sterowania do *embedded_statement* jest taki sam jak stan *v* na końcu *resource_acquisition*.</span><span class="sxs-lookup"><span data-stu-id="454b4-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="454b4-336">Instrukcje Lock</span><span class="sxs-lookup"><span data-stu-id="454b4-336">Lock statements</span></span>

<span data-ttu-id="454b4-337">W przypadku instrukcji stmt formularza: `lock`</span><span class="sxs-lookup"><span data-stu-id="454b4-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="454b4-338">Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-339">Określony stan przypisania *v* w przeniesieniu przepływu sterowania do *embedded_statement* jest taki sam jak stan *v* na końcu *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="454b4-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="454b4-340">Instrukcje Yield</span><span class="sxs-lookup"><span data-stu-id="454b4-340">Yield statements</span></span>

<span data-ttu-id="454b4-341">W przypadku instrukcji stmt formularza: `yield return`</span><span class="sxs-lookup"><span data-stu-id="454b4-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="454b4-342">Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="454b4-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="454b4-343">Określony stan przypisania *v* na końcu *stmt* jest taki sam jak stan *v* na końcu *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="454b4-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="454b4-344">`yield break` Instrukcja nie ma wpływu na stan o określonym przypisaniu.</span><span class="sxs-lookup"><span data-stu-id="454b4-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="454b4-345">Ogólne reguły dla prostych wyrażeń</span><span class="sxs-lookup"><span data-stu-id="454b4-345">General rules for simple expressions</span></span>

<span data-ttu-id="454b4-346">Następująca reguła ma zastosowanie do tych rodzajów wyrażeń: literałów ([literałów](expressions.md#literals)), proste nazwy ([proste nazwy](expressions.md#simple-names)), wyrażenia dostępu do składowych ([dostęp do elementów członkowskich](expressions.md#member-access)), nieindeksowane wyrażenia dostępu podstawowego ([dostęp podstawowy](expressions.md#base-access)), `typeof`wyrażenia ([operator typeof](expressions.md#the-typeof-operator)), wyrażenia wartości domyślnych ([wyrażenia wartości domyślnych](expressions.md#default-value-expressions)) i `nameof` wyrażenia ([wyrażenia nameof](expressions.md#nameof-expressions)).</span><span class="sxs-lookup"><span data-stu-id="454b4-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="454b4-347">Określony stan przypisania *v* na końcu takiego wyrażenia jest taki sam jak określony stan przypisania *v* na początku wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="454b4-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="454b4-348">Ogólne reguły dla wyrażeń z osadzonymi wyrażeniami</span><span class="sxs-lookup"><span data-stu-id="454b4-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="454b4-349">Następujące reguły mają zastosowanie do tych rodzajów wyrażeń: wyrażeń ujętych w nawiasy ([wyrażenia w nawiasach](expressions.md#parenthesized-expressions)), wyrażenia dostępu do elementów ([dostęp do elementów](expressions.md#element-access)), podstawowe wyrażenia dostępu z indeksowanie ([dostęp podstawowy](expressions.md#base-access)), przyrosty i wyrażenia zmniejszania ([Operatory przyrostka i zmniejszenie](expressions.md#postfix-increment-and-decrement-operators), [Operatory przyrostu i zmniejszania prefiksu](expressions.md#prefix-increment-and-decrement-operators)), wyrażenia rzutowania ( `+`[wyrażenia rzutowania](expressions.md#cast-expressions)), jednoargumentowe `-`,, `~`, `*`wyrażenia, dane `+`binarne `-`, `*` `/`,, ,,`%`,,,,,, `<<` `>>` `<` `<=` `>` `>=` `==`, `!=`, ,`is`,, ,`^`wyrażenia ([Operatory arytmetyczne](expressions.md#arithmetic-operators), [operatory przesunięcia](expressions.md#shift-operators), relacyjne i testy typu `|` `as` `&` [ Operatory](expressions.md#relational-and-type-testing-operators), [Operatory logiczne](expressions.md#logical-operators)), złożone wyrażenia przypisania `checked` ([przypisanie złożone](expressions.md#compound-assignment)) i `unchecked` wyrażenia ([Operatory sprawdzone i niesprawdzone](expressions.md#the-checked-and-unchecked-operators)), a także tablica i delegat wyrażenia tworzenia ([operator new](expressions.md#the-new-operator)).</span><span class="sxs-lookup"><span data-stu-id="454b4-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="454b4-350">Każdy z tych wyrażeń zawiera co najmniej jedno Podwyrażenie, które jest niewarunkowo oceniane w ustalonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="454b4-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="454b4-351">Na przykład operator binarny `%` oblicza po lewej stronie operatora, po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="454b4-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="454b4-352">Operacja indeksowania szacuje wyrażenie indeksowane, a następnie oblicza każde wyrażenie indeksu w kolejności od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="454b4-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="454b4-353">Dla *wyrażenia Expression,* które ma podwyrażenia *E1, E2,..., EN*, oceniane w tej kolejności:</span><span class="sxs-lookup"><span data-stu-id="454b4-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="454b4-354">Określony stan przypisania *v* na początku *E1* jest taki sam jak określony stan przypisania na początku *wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="454b4-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="454b4-355">Określony stan przypisania *v* na początku *EI* (*i* więcej niż jeden) jest taki sam jak określony stan przypisania na końcu poprzedniego wyrażenia podrzędnego.</span><span class="sxs-lookup"><span data-stu-id="454b4-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="454b4-356">Określony stan przypisania *v* na końcu *wyrażenia* jest taki sam, jak określony stan przypisania na końcu *pl*</span><span class="sxs-lookup"><span data-stu-id="454b4-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="454b4-357">Wyrażenia wywołania i wyrażenia tworzenia obiektów</span><span class="sxs-lookup"><span data-stu-id="454b4-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="454b4-358">*Wyrażenie wyrażenia wywołania* w postaci:</span><span class="sxs-lookup"><span data-stu-id="454b4-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="454b4-359">lub wyrażenie tworzenia obiektu formularza:</span><span class="sxs-lookup"><span data-stu-id="454b4-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="454b4-360">Dla wyrażenia wywołania, określony stan przypisania *v* przed *primary_expression* jest taki sam jak stan *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-361">Dla wyrażenia wywołania, określony stan przypisania *v* przed *arg1* jest taki sam jak stan *v* po *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="454b4-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="454b4-362">W przypadku wyrażenia tworzenia obiektu określony stan przypisania *v* przed *arg1* jest taki sam jak stan *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-363">Dla każdego argumentu *Argi*, określony stan przypisania *v* po *Argi* jest określany przez reguły wyrażeń normalnych, ignorując wszystkie `ref` lub `out` modyfikatory.</span><span class="sxs-lookup"><span data-stu-id="454b4-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="454b4-364">Dla każdego argumentu *Argi* dla każdego *i* więcej niż jednego, określony stan przypisania *v* przed *Argi* jest taki sam jak stan *v* po poprzednim *ARG*.</span><span class="sxs-lookup"><span data-stu-id="454b4-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="454b4-365">Jeśli zmienna *v* jest `out` przenoszona jako argument (tj. argument formularza `out v`) w którymkolwiek z argumentów, stanie *v* po elemencie *Expr* jest ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="454b4-366">Przypadku stan *v* po wyszukiwaniu *jest taki* sam jak stan *v* po *argN*.</span><span class="sxs-lookup"><span data-stu-id="454b4-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="454b4-367">Dla inicjatorów tablicy ([wyrażenia tworzenia tablic](expressions.md#array-creation-expressions)), inicjatorów obiektów ([inicjatorów obiektów](expressions.md#object-initializers)), inicjatorów kolekcji ([Inicjatory kolekcji](expressions.md#collection-initializers)) i inicjatorów obiektów anonimowych ([Tworzenie obiektów anonimowych wyrażenia](expressions.md#anonymous-object-creation-expressions)) określony stan przypisania jest określany przez rozszerzanie, że te konstrukcje są zdefiniowane w warunkach.</span><span class="sxs-lookup"><span data-stu-id="454b4-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="454b4-368">Proste wyrażenia przypisania</span><span class="sxs-lookup"><span data-stu-id="454b4-368">Simple assignment expressions</span></span>

<span data-ttu-id="454b4-369">*Wyrażenie wyrażenia w* postaci `w = expr_rhs`:</span><span class="sxs-lookup"><span data-stu-id="454b4-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="454b4-370">Określony stan przypisania wartości *v* przed *expr_rhs* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-371">Określony stan przypisania *v* po wyjściu jest określany przez:</span><span class="sxs-lookup"><span data-stu-id="454b4-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="454b4-372">Jeśli *w* jest taka sama jak zmienna *v*, *oznacza to,* że określony stan przypisania *v* po wystawce jest ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="454b4-373">W przeciwnym razie, jeśli przypisanie występuje w konstruktorze wystąpienia typu struktury, jeśli *w* jest dostęp do właściwości wskazującej automatycznie zaimplementowaną Właściwość *P* w tworzonym wystąpieniu i *v* jest polem ukrytego zapasowego *P*, a następnie określony stan przypisania *v* po elemencie *Expr* jest ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="454b4-374">W przeciwnym razie określony stan przypisania *v* po wyszukiwaniu *jest taki* sam jak w przypadku określonego stanu przypisania *v* po *expr_rhs*.</span><span class="sxs-lookup"><span data-stu-id="454b4-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="454b4-375">wyrażenia & & (warunkowe i)</span><span class="sxs-lookup"><span data-stu-id="454b4-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="454b4-376">*Wyrażenie wyrażenia w* postaci `expr_first && expr_second`:</span><span class="sxs-lookup"><span data-stu-id="454b4-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="454b4-377">Określony stan przypisania wartości *v* przed *expr_first* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-378">Określony stan przypisania wartości *v* przed *expr_second* jest ostatecznie przypisywany, jeśli stanem *v* po *expr_first* jest w nieskończony sposób przypisany lub "w znacznym przypisaniu po true Expression".</span><span class="sxs-lookup"><span data-stu-id="454b4-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="454b4-379">W przeciwnym razie nie jest to ostatecznie przypisane.</span><span class="sxs-lookup"><span data-stu-id="454b4-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="454b4-380">Określony stan przypisania *v* po wyjściu jest określany przez:</span><span class="sxs-lookup"><span data-stu-id="454b4-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="454b4-381">Jeśli *expr_first* jest wyrażeniem stałym o wartości `false`, oznacza to, że określony stan przypisania *v* po *wyrażeniu* jest taki sam, jak w przypadku określonego stanu przypisania *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="454b4-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="454b4-382">W przeciwnym razie, jeśli stan *v* po *expr_first* jest ostatecznie przypisany, stan *v* po elemencie *Expr* jest ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="454b4-383">W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany, a stanem *v* po *expr_first* jest "w sposób terminowy przypisany po wartości false", wówczas stanem *v* *po wyrażeniu jest* nieskończoność pisywany.</span><span class="sxs-lookup"><span data-stu-id="454b4-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="454b4-384">W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany lub "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po prawdziwe wyrażenie".</span><span class="sxs-lookup"><span data-stu-id="454b4-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="454b4-385">W przeciwnym razie, jeśli stanem *v* po *expr_first* jest "w sposób terminowy przypisany po false Expression", a stanem *v* po *expr_second* jest "w sposób terminowy przypisany po false Expression", wówczas stanem *v* po  *wyrażenie* jest "w sposób terminowy przypisany po false Expression".</span><span class="sxs-lookup"><span data-stu-id="454b4-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="454b4-386">W przeciwnym razie stan *v* *po wyszukiwaniu nie jest* ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="454b4-387">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="454b4-387">In the example</span></span>
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="454b4-388">zmienna `i` jest uznawana za ostatecznie przypisaną w jednej z osadzonych instrukcji `if` instrukcji, ale nie w drugim.</span><span class="sxs-lookup"><span data-stu-id="454b4-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="454b4-389">W instrukcji w metodzie `F`zmienna `i` jest ostatecznie przypisana w pierwszej instrukcji osadzonej, ponieważ wykonywanie wyrażenia `(i = y)` zawsze poprzedza wykonywanie tej osadzonej instrukcji. `if`</span><span class="sxs-lookup"><span data-stu-id="454b4-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="454b4-390">Natomiast zmienna `i` nie jest ostatecznie przypisana w drugiej instrukcji osadzonej, ponieważ `x >= 0` mogło przetestować wartość false, co spowodowało przypisanie zmiennej `i` .</span><span class="sxs-lookup"><span data-stu-id="454b4-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="454b4-391">|| wyrażenia (warunkowe lub)</span><span class="sxs-lookup"><span data-stu-id="454b4-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="454b4-392">*Wyrażenie wyrażenia w* postaci `expr_first || expr_second`:</span><span class="sxs-lookup"><span data-stu-id="454b4-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="454b4-393">Określony stan przypisania wartości *v* przed *expr_first* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-394">Określony stan przypisania wartości *v* przed *expr_second* jest ostatecznie przypisany, jeśli stanem *v* po *expr_first* jest albo "w sposób terminowy przypisany po wartości false".</span><span class="sxs-lookup"><span data-stu-id="454b4-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="454b4-395">W przeciwnym razie nie jest to ostatecznie przypisane.</span><span class="sxs-lookup"><span data-stu-id="454b4-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="454b4-396">Instrukcja przypisania o określonej wartości *v* po *wyrażeniu* jest określana przez:</span><span class="sxs-lookup"><span data-stu-id="454b4-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="454b4-397">Jeśli *expr_first* jest wyrażeniem stałym o wartości `true`, oznacza to, że określony stan przypisania *v* po *wyrażeniu* jest taki sam, jak w przypadku określonego stanu przypisania *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="454b4-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="454b4-398">W przeciwnym razie, jeśli stan *v* po *expr_first* jest ostatecznie przypisany, stan *v* po elemencie *Expr* jest ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="454b4-399">W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany, a stanem *v* po *expr_first* jest "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu jest* nieskończoność pisywany.</span><span class="sxs-lookup"><span data-stu-id="454b4-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="454b4-400">W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany lub "w sposób terminowy przypisany po wartości false", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po false Expression".</span><span class="sxs-lookup"><span data-stu-id="454b4-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="454b4-401">W przeciwnym razie, jeśli stanem *v* po *expr_first* jest "w sposób terminowy przypisany po prawdziwym wyrażeniu", a stanem *v* po *expr_second* jest "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu* jest "w sposób terminowy przypisany po true Expression".</span><span class="sxs-lookup"><span data-stu-id="454b4-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="454b4-402">W przeciwnym razie stan *v* *po wyszukiwaniu nie jest* ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="454b4-403">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="454b4-403">In the example</span></span>
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="454b4-404">zmienna `i` jest uznawana za ostatecznie przypisaną w jednej z osadzonych instrukcji `if` instrukcji, ale nie w drugim.</span><span class="sxs-lookup"><span data-stu-id="454b4-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="454b4-405">W instrukcji w metodzie `G`zmienna `i` jest ostatecznie przypisana w drugiej instrukcji osadzonej, ponieważ wykonanie wyrażenia `(i = y)` zawsze poprzedza wykonywanie tej osadzonej instrukcji. `if`</span><span class="sxs-lookup"><span data-stu-id="454b4-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="454b4-406">Natomiast zmienna `i` nie jest ostatecznie przypisana w pierwszej instrukcji osadzonej, ponieważ `x >= 0` mogło przetestować wartość true, co spowodowało przypisanie zmiennej `i` .</span><span class="sxs-lookup"><span data-stu-id="454b4-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="454b4-407">!</span><span class="sxs-lookup"><span data-stu-id="454b4-407">!</span></span> <span data-ttu-id="454b4-408">wyrażenia logicznej negacji</span><span class="sxs-lookup"><span data-stu-id="454b4-408">(logical negation) expressions</span></span>

<span data-ttu-id="454b4-409">*Wyrażenie wyrażenia w* postaci `! expr_operand`:</span><span class="sxs-lookup"><span data-stu-id="454b4-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="454b4-410">Określony stan przypisania wartości *v* przed *expr_operand* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-411">Określony stan przypisania *v* po wyjściu jest określany przez:</span><span class="sxs-lookup"><span data-stu-id="454b4-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="454b4-412">Jeśli stan *v* po \* expr_operand \* jest przypisywany w określony sposób, stan *v* po elemencie *Expr* jest ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="454b4-413">Jeśli stan *v* \*po \*\* expr_operand \* nie jest ostatecznie przypisany, stan *v* po wyjściu nie jest przypisany ostatecznie.</span><span class="sxs-lookup"><span data-stu-id="454b4-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="454b4-414">Jeśli stanem *v* po \* expr_operand \* jest "w sposób terminowy przypisany po wartości false", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po prawdziwe wyrażenie".</span><span class="sxs-lookup"><span data-stu-id="454b4-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="454b4-415">Jeśli stanem *v* po \* expr_operand \* jest "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po false Expression".</span><span class="sxs-lookup"><span data-stu-id="454b4-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="454b4-416">??</span><span class="sxs-lookup"><span data-stu-id="454b4-416">??</span></span> <span data-ttu-id="454b4-417">(zerowe łączenie) wyrażenia</span><span class="sxs-lookup"><span data-stu-id="454b4-417">(null coalescing) expressions</span></span>

<span data-ttu-id="454b4-418">*Wyrażenie wyrażenia w* postaci `expr_first ?? expr_second`:</span><span class="sxs-lookup"><span data-stu-id="454b4-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="454b4-419">Określony stan przypisania wartości *v* przed *expr_first* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-420">Określony stan przypisania *v* przed *expr_second* jest taki sam, jak w przypadku określonego stanu przypisania *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="454b4-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="454b4-421">Instrukcja przypisania o określonej wartości *v* po *wyrażeniu* jest określana przez:</span><span class="sxs-lookup"><span data-stu-id="454b4-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="454b4-422">Jeśli *expr_first* jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions)) o wartości null, stanie *v* po *wyrażeniu* jest taka sama jak stan *v* po *expr_second*.</span><span class="sxs-lookup"><span data-stu-id="454b4-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="454b4-423">W przeciwnym razie stan *v* po wyszukiwaniu *jest taki* sam jak w przypadku określonego stanu przypisania *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="454b4-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="454b4-424">?: (warunkowe) wyrażenia</span><span class="sxs-lookup"><span data-stu-id="454b4-424">?: (conditional) expressions</span></span>

<span data-ttu-id="454b4-425">*Wyrażenie wyrażenia w* postaci `expr_cond ? expr_true : expr_false`:</span><span class="sxs-lookup"><span data-stu-id="454b4-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="454b4-426">Określony stan przypisania *v* przed *expr_cond* jest taki sam jak stan *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="454b4-427">Określony stan przypisania *v* przed *expr_true* jest ostatecznie przypisany, jeśli tylko jedno z następujących posiada:</span><span class="sxs-lookup"><span data-stu-id="454b4-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="454b4-428">*expr_cond* jest wyrażeniem stałym z wartością`false`</span><span class="sxs-lookup"><span data-stu-id="454b4-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="454b4-429">stan *v* po *expr_cond* jest ostatecznie przypisany lub "w sposób terminowy przypisany po true Expression".</span><span class="sxs-lookup"><span data-stu-id="454b4-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="454b4-430">Określony stan przypisania *v* przed *expr_false* jest ostatecznie przypisany, jeśli tylko jedno z następujących posiada:</span><span class="sxs-lookup"><span data-stu-id="454b4-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="454b4-431">*expr_cond* jest wyrażeniem stałym z wartością`true`</span><span class="sxs-lookup"><span data-stu-id="454b4-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="454b4-432">stan *v* po *expr_cond* jest ostatecznie przypisany lub "w sposób terminowy przypisany po false Expression".</span><span class="sxs-lookup"><span data-stu-id="454b4-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="454b4-433">Określony stan przypisania *v* po wyjściu jest określany przez:</span><span class="sxs-lookup"><span data-stu-id="454b4-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="454b4-434">Jeśli *expr_cond* jest wyrażeniem stałym[(wyrażenia stałe](expressions.md#constant-expressions)) z `true` wartością, wówczas stanem *v* po wyrażeniu *jest taka* sama jak stan *v* po *expr_true*.</span><span class="sxs-lookup"><span data-stu-id="454b4-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="454b4-435">W przeciwnym razie, jeśli *expr_cond* jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions) `false` ) z wartością, stanem *v* po wyrażeniu jest taka sama jak stan *v* po *expr_false*.</span><span class="sxs-lookup"><span data-stu-id="454b4-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="454b4-436">W przeciwnym razie, jeśli stan *v* po *expr_true* jest ostatecznie przypisany i stanem *v* po *expr_false* jest ostatecznie przypisany, stan *v* po elemencie *Expr* jest ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="454b4-437">W przeciwnym razie stan *v* *po wyszukiwaniu nie jest* ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="454b4-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="454b4-438">Funkcje anonimowe</span><span class="sxs-lookup"><span data-stu-id="454b4-438">Anonymous functions</span></span>

<span data-ttu-id="454b4-439">Dla wyrażenia *lambda_expression* lub *anonymous_method_expression* *z treścią*(albo *blokiem* lub *wyrażeniem*):</span><span class="sxs-lookup"><span data-stu-id="454b4-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="454b4-440">Określony stan przypisania zewnętrznej zmiennej *v* przed *treścią* jest taki sam jak stan *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="454b4-441">Oznacza to, że określony stan przypisania zmiennych zewnętrznych jest Dziedziczony z kontekstu funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="454b4-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="454b4-442">Określony stan przypisania zmiennej zewnętrznej *v* po wyjściu jest taki sam jak stan *v* przed *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="454b4-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="454b4-443">Przykład</span><span class="sxs-lookup"><span data-stu-id="454b4-443">The example</span></span>
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
<span data-ttu-id="454b4-444">generuje błąd czasu kompilacji, ponieważ `max` nie jest on ostatecznie przypisany, gdzie jest zadeklarowana funkcja anonimowa.</span><span class="sxs-lookup"><span data-stu-id="454b4-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="454b4-445">Przykład</span><span class="sxs-lookup"><span data-stu-id="454b4-445">The example</span></span>
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
<span data-ttu-id="454b4-446">generuje również błąd czasu kompilacji, ponieważ przypisanie do `n` funkcji anonimowej nie ma wpływu na określony `n` stan przypisania poza funkcję anonimową.</span><span class="sxs-lookup"><span data-stu-id="454b4-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="454b4-447">Odwołania do zmiennych</span><span class="sxs-lookup"><span data-stu-id="454b4-447">Variable references</span></span>

<span data-ttu-id="454b4-448">*Variable_reference* jest *wyrażeniem* , które jest sklasyfikowane jako zmienna.</span><span class="sxs-lookup"><span data-stu-id="454b4-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="454b4-449">*Variable_reference* oznacza lokalizację magazynu, do którego można uzyskać dostęp zarówno w celu pobrania bieżącej wartości, jak i zapisania nowej wartości.</span><span class="sxs-lookup"><span data-stu-id="454b4-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="454b4-450">W języku C C++i *variable_reference* jest znany jako *lvalue*.</span><span class="sxs-lookup"><span data-stu-id="454b4-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="454b4-451">Niepodzielność odwołań do zmiennych</span><span class="sxs-lookup"><span data-stu-id="454b4-451">Atomicity of variable references</span></span>

<span data-ttu-id="454b4-452">Odczyty i zapisy następujących typów danych są niepodzielne `bool`: `char`, `byte` `uint` `ushort`, `sbyte` `short` `int`, ,,,,,itypyreferencyjne.`float`</span><span class="sxs-lookup"><span data-stu-id="454b4-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="454b4-453">Ponadto odczyty i zapisy typów wyliczeniowych z typem podstawowym na poprzedniej liście również są niepodzielne.</span><span class="sxs-lookup"><span data-stu-id="454b4-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="454b4-454">Odczyty i zapisy innych typów, w `long`tym `ulong` `double`,,, `decimal`i, jak również typy zdefiniowane przez użytkownika, nie są gwarantowane jako niepodzielne.</span><span class="sxs-lookup"><span data-stu-id="454b4-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="454b4-455">Poza funkcjami biblioteki zaprojektowanymi do tego celu nie istnieje gwarancja niepodzielnego odczytu i zapisu, na przykład w przypadku zwiększenia lub zmniejszenia.</span><span class="sxs-lookup"><span data-stu-id="454b4-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

