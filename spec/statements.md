---
ms.openlocfilehash: 94346034a667ad4af26796c0c4bbc96d6ed79aba
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876837"
---
# <a name="statements"></a><span data-ttu-id="404d0-101">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="404d0-101">Statements</span></span>

<span data-ttu-id="404d0-102">C#zawiera różne instrukcje.</span><span class="sxs-lookup"><span data-stu-id="404d0-102">C# provides a variety of statements.</span></span> <span data-ttu-id="404d0-103">Większość z tych instrukcji będzie znana deweloperom, którzy zaprogramowany w języku C i C++.</span><span class="sxs-lookup"><span data-stu-id="404d0-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="404d0-104">*Embedded_statement* nie jest używany do instrukcji, które są wyświetlane w innych instrukcjach.</span><span class="sxs-lookup"><span data-stu-id="404d0-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="404d0-105">Użycie *embedded_statement* zamiast *instrukcji* wyklucza użycie instrukcji deklaracji oraz instrukcji oznaczonych etykietami w tych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="404d0-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="404d0-106">Przykład</span><span class="sxs-lookup"><span data-stu-id="404d0-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="404d0-107">wynikiem jest błąd czasu kompilacji, ponieważ `if` instrukcja wymaga elementu *embedded_statement* , a nie *instrukcji* dla jego gałęzi IF.</span><span class="sxs-lookup"><span data-stu-id="404d0-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="404d0-108">Jeśli ten kod był dozwolony, zmienna `i` zostałaby zadeklarowana, ale nigdy nie można jej użyć.</span><span class="sxs-lookup"><span data-stu-id="404d0-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="404d0-109">Należy jednak pamiętać, że przez umieszczenie `i`deklaracji w bloku, przykład jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="404d0-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="404d0-110">Punkty końcowe i osiągalność</span><span class="sxs-lookup"><span data-stu-id="404d0-110">End points and reachability</span></span>

<span data-ttu-id="404d0-111">Każda instrukcja ma ***punkt końcowy***.</span><span class="sxs-lookup"><span data-stu-id="404d0-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="404d0-112">W intuicyjnych warunkach punkt końcowy instrukcji jest lokalizacją, która jest bezpośrednio zgodna z instrukcją.</span><span class="sxs-lookup"><span data-stu-id="404d0-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="404d0-113">Reguły wykonywania dla złożonych instrukcji (Instrukcje zawierające osadzone instrukcje) określają akcję, która jest wykonywana, gdy kontrolka osiągnie punkt końcowy osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="404d0-114">Na przykład, gdy formant osiągnie punkt końcowy instrukcji w bloku, formant jest przenoszony do następnej instrukcji w bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="404d0-115">Jeśli instrukcja może zostać osiągnięta przez wykonanie, instrukcja jest określana jako ***osiągalna***.</span><span class="sxs-lookup"><span data-stu-id="404d0-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="404d0-116">Z drugiej strony, jeśli nie jest możliwe, że instrukcja zostanie wykonana, instrukcja jest określana jako ***nieosiągalna***.</span><span class="sxs-lookup"><span data-stu-id="404d0-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="404d0-117">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="404d0-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="404d0-118">drugie wywołanie `Console.WriteLine` jest nieosiągalne, ponieważ nie ma możliwości wykonania instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="404d0-119">Ostrzeżenie jest zgłaszane, gdy kompilator ustali, że instrukcja jest nieosiągalna.</span><span class="sxs-lookup"><span data-stu-id="404d0-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="404d0-120">Nie jest to błąd, aby instrukcja była nieosiągalna.</span><span class="sxs-lookup"><span data-stu-id="404d0-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="404d0-121">Aby określić, czy dana instrukcja lub punkt końcowy są osiągalne, kompilator wykonuje analizę przepływu zgodnie z regułami osiągalności określonymi dla każdej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="404d0-122">Analiza przepływu uwzględnia wartości wyrażeń stałych ([wyrażeń stałych](expressions.md#constant-expressions)) kontrolujących zachowanie instrukcji, ale nie są brane pod uwagę możliwe wartości wyrażeń niestałych.</span><span class="sxs-lookup"><span data-stu-id="404d0-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="404d0-123">Innymi słowy, w celach analizy przepływu sterowania, niestałe wyrażenie danego typu jest uznawane za ma dowolną możliwą wartość tego typu.</span><span class="sxs-lookup"><span data-stu-id="404d0-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="404d0-124">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="404d0-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="404d0-125">wyrażenie `if` logiczne instrukcji jest wyrażeniem stałym, ponieważ oba operandy `==` operatora są stałymi.</span><span class="sxs-lookup"><span data-stu-id="404d0-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="404d0-126">Ponieważ wyrażenie stałe jest oceniane w czasie kompilacji, przez wygenerowanie wartości `false` `Console.WriteLine` , wywołanie jest uznawane za nieosiągalne.</span><span class="sxs-lookup"><span data-stu-id="404d0-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="404d0-127">Jeśli `i` jednak zostanie zmieniony jako zmienna lokalna</span><span class="sxs-lookup"><span data-stu-id="404d0-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="404d0-128">`Console.WriteLine` wywołanie jest uznawane za osiągalne, mimo że w rzeczywistości nigdy nie zostanie wykonane.</span><span class="sxs-lookup"><span data-stu-id="404d0-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="404d0-129">*Blok* składowej funkcji jest zawsze uznawany za osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="404d0-130">Poprzez dokonanie oceny reguł osiągalności każdej instrukcji w bloku, można określić osiągalność każdej z tych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="404d0-131">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="404d0-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="404d0-132">osiągalność sekundy `Console.WriteLine` jest określana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="404d0-133">Pierwsza `Console.WriteLine` Instrukcja wyrażenia jest osiągalna, ponieważ blok `F` metody jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="404d0-134">Punkt końcowy pierwszej `Console.WriteLine` instrukcji wyrażenia jest osiągalny, ponieważ ta instrukcja jest osiągalna.</span><span class="sxs-lookup"><span data-stu-id="404d0-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="404d0-135">Instrukcja jest osiągalna, ponieważ punkt końcowy pierwszej `Console.WriteLine` instrukcji wyrażenia jest osiągalny. `if`</span><span class="sxs-lookup"><span data-stu-id="404d0-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="404d0-136">Druga `Console.WriteLine` Instrukcja wyrażenia jest osiągalna, ponieważ wyrażenie `if` logiczne instrukcji nie ma stałej wartości `false`.</span><span class="sxs-lookup"><span data-stu-id="404d0-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="404d0-137">Istnieją dwie sytuacje, w których jest to błąd czasu kompilacji dla punktu końcowego instrukcji, aby uzyskać dostęp:</span><span class="sxs-lookup"><span data-stu-id="404d0-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="404d0-138">`switch` Ponieważ instrukcja nie zezwala sekcji Switch na "przechodzenie" do następnej sekcji Switch, jest to błąd czasu kompilacji dla punktu końcowego listy instrukcji przełącznika, aby uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="404d0-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="404d0-139">W przypadku wystąpienia tego błędu jest to zazwyczaj wskazanie `break` braku instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="404d0-140">Jest to błąd czasu kompilacji dla punktu końcowego bloku elementu członkowskiego funkcji, który oblicza wartość jako osiągalną.</span><span class="sxs-lookup"><span data-stu-id="404d0-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="404d0-141">W przypadku wystąpienia tego błędu zwykle jest to wskazanie, że `return` brakuje instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="404d0-142">Bloki</span><span class="sxs-lookup"><span data-stu-id="404d0-142">Blocks</span></span>

<span data-ttu-id="404d0-143">*Blok* umożliwia zapisanie wielu instrukcji w kontekstach, w których Pojedyncza instrukcja jest dozwolona.</span><span class="sxs-lookup"><span data-stu-id="404d0-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="404d0-144">*Blok* składa się z opcjonalnych *statement_list* ([list instrukcji](statements.md#statement-lists)) ujętych w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="404d0-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="404d0-145">Jeśli lista instrukcji zostanie pominięta, blok jest określany jako pusty.</span><span class="sxs-lookup"><span data-stu-id="404d0-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="404d0-146">Blok może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="404d0-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="404d0-147">Zakres zmiennej lokalnej lub stałej zadeklarowanej w bloku jest blokiem.</span><span class="sxs-lookup"><span data-stu-id="404d0-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="404d0-148">Blok jest wykonywany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="404d0-149">Jeśli blok jest pusty, sterowanie jest przekazywane do punktu końcowego bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="404d0-150">Jeśli blok nie jest pusty, sterowanie jest przekazywane do listy instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="404d0-151">Gdy i Jeśli kontrolka osiągnie punkt końcowy listy instrukcji, sterowanie jest przekazywane do punktu końcowego bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="404d0-152">Lista instrukcji bloku jest osiągalna, jeśli blok jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="404d0-153">Punkt końcowy bloku jest osiągalny, jeśli blok jest pusty lub jeśli punkt końcowy listy instrukcji jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="404d0-154">*Blok* zawierający jedną lub więcej `yield` instrukcji ([instrukcja Yield](statements.md#the-yield-statement)) nosi nazwę bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="404d0-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="404d0-155">Bloki iteratorów są używane do implementowania składowych funkcji jako Iteratory ([Iteratory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="404d0-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="404d0-156">Dodatkowe ograniczenia dotyczą bloków iteratora:</span><span class="sxs-lookup"><span data-stu-id="404d0-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="404d0-157">Jest to błąd czasu kompilacji dla `return` instrukcji, która ma być wyświetlana w bloku iteratora (ale `yield return` instrukcje są dozwolone).</span><span class="sxs-lookup"><span data-stu-id="404d0-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="404d0-158">Jest to błąd czasu kompilacji dla bloku iteratora, który zawiera niebezpieczny kontekst ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="404d0-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="404d0-159">Blok iteratora zawsze definiuje bezpieczny kontekst, nawet jeśli jego deklaracja jest zagnieżdżona w niebezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="404d0-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="404d0-160">Listy instrukcji</span><span class="sxs-lookup"><span data-stu-id="404d0-160">Statement lists</span></span>

<span data-ttu-id="404d0-161">***Lista instrukcji*** składa się z jednej lub kilku instrukcji utworzonych w sekwencji.</span><span class="sxs-lookup"><span data-stu-id="404d0-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="404d0-162">Listy instrukcji występują w *blokach bloku*s ([bloki](statements.md#blocks)) i w *switch_block*s ([instrukcja SWITCH](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="404d0-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="404d0-163">Lista instrukcji jest wykonywana przez przeniesienie kontroli do pierwszej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="404d0-164">Gdy i Jeśli kontrolka osiągnie punkt końcowy instrukcji, sterowanie jest przekazywane do następnej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="404d0-165">Gdy i Jeśli kontrolka osiągnie punkt końcowy ostatniej instrukcji, sterowanie jest przekazywane do punktu końcowego listy instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="404d0-166">Instrukcja na liście instrukcji jest dostępna, jeśli co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="404d0-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="404d0-167">Instrukcja jest pierwszą instrukcją i sama lista instrukcji jest osiągalna.</span><span class="sxs-lookup"><span data-stu-id="404d0-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="404d0-168">Punkt końcowy poprzedniej instrukcji jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="404d0-169">Instrukcja jest instrukcją z etykietą, a etykieta jest przywoływana przez instrukcję `goto` osiągalną.</span><span class="sxs-lookup"><span data-stu-id="404d0-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="404d0-170">Punkt końcowy listy instrukcji jest dostępny, jeśli punkt końcowy ostatniej instrukcji na liście jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="404d0-171">Pusta instrukcja</span><span class="sxs-lookup"><span data-stu-id="404d0-171">The empty statement</span></span>

<span data-ttu-id="404d0-172">*Empty_statement* nic nie robi.</span><span class="sxs-lookup"><span data-stu-id="404d0-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="404d0-173">Pusta instrukcja jest używana, gdy nie ma żadnych operacji do wykonania w kontekście, w którym instrukcja jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="404d0-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="404d0-174">Wykonanie pustej instrukcji po prostu przenosi kontrolę do punktu końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="404d0-175">W związku z tym punkt końcowy pustej instrukcji jest osiągalny, jeśli pusta instrukcja jest osiągalna.</span><span class="sxs-lookup"><span data-stu-id="404d0-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="404d0-176">Pustą instrukcję można użyć podczas pisania `while` instrukcji z pustą treścią:</span><span class="sxs-lookup"><span data-stu-id="404d0-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="404d0-177">Ponadto można użyć pustej instrukcji, aby zadeklarować etykietę tuż przed zamykaniem "`}`" bloku:</span><span class="sxs-lookup"><span data-stu-id="404d0-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="404d0-178">Instrukcje oznaczone</span><span class="sxs-lookup"><span data-stu-id="404d0-178">Labeled statements</span></span>

<span data-ttu-id="404d0-179">*Labeled_statement* umożliwia prefiks instrukcji poprzedzonej etykietą.</span><span class="sxs-lookup"><span data-stu-id="404d0-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="404d0-180">Instrukcje z etykietami są dozwolone w blokach, ale nie są dozwolone jako osadzone instrukcje.</span><span class="sxs-lookup"><span data-stu-id="404d0-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="404d0-181">Instrukcja labeled deklaruje etykietę o nazwie podaną przez *Identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="404d0-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="404d0-182">Zakres etykiety to cały blok, w którym jest zadeklarowana etykieta, w tym wszelkie zagnieżdżone bloki.</span><span class="sxs-lookup"><span data-stu-id="404d0-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="404d0-183">Jest to błąd czasu kompilacji dla dwóch etykiet o tej samej nazwie, aby mieć nakładające się zakresy.</span><span class="sxs-lookup"><span data-stu-id="404d0-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="404d0-184">Do etykiet można odwoływać się `goto` z instrukcji ([instrukcji goto](statements.md#the-goto-statement)) w zakresie etykiety.</span><span class="sxs-lookup"><span data-stu-id="404d0-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="404d0-185">Oznacza to, `goto` że instrukcje mogą przekazywać kontrolę w blokach i poza bloki, ale nigdy nie w blokach.</span><span class="sxs-lookup"><span data-stu-id="404d0-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="404d0-186">Etykiety mają własne miejsce deklaracji i nie zakłócają innych identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="404d0-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="404d0-187">Przykład</span><span class="sxs-lookup"><span data-stu-id="404d0-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="404d0-188">jest prawidłowy i używa nazwy `x` zarówno jako parametru, jak i etykiety.</span><span class="sxs-lookup"><span data-stu-id="404d0-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="404d0-189">Wykonanie instrukcji oznaczonej etykietą odpowiada dokładnie na wykonanie instrukcji następującej po etykiecie.</span><span class="sxs-lookup"><span data-stu-id="404d0-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="404d0-190">Oprócz osiągalności zapewnianej przez normalny przepływ sterowania, instrukcja z etykietą jest osiągalna, jeśli do etykiety odwołuje się osiągalna `goto` instrukcja.</span><span class="sxs-lookup"><span data-stu-id="404d0-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="404d0-191">Oprócz `finally` `try` `finally` Jeśli instrukcja znajduje się wewnątrz, `try` która zawiera blok, a instrukcja z etykietą jest poza, a punkt końcowy bloku jest nieosiągalny, a następnie instrukcja z etykietą nie jest dostępna od `goto` tej `goto` instrukcji).</span><span class="sxs-lookup"><span data-stu-id="404d0-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="404d0-192">Deklaracje deklaracji</span><span class="sxs-lookup"><span data-stu-id="404d0-192">Declaration statements</span></span>

<span data-ttu-id="404d0-193">*Declaration_statement* deklaruje lokalną zmienną lub stałą.</span><span class="sxs-lookup"><span data-stu-id="404d0-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="404d0-194">Instrukcje deklaracji są dozwolone w blokach, ale nie są dozwolone jako osadzone instrukcje.</span><span class="sxs-lookup"><span data-stu-id="404d0-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="404d0-195">Deklaracje zmiennej lokalnej</span><span class="sxs-lookup"><span data-stu-id="404d0-195">Local variable declarations</span></span>

<span data-ttu-id="404d0-196">*Local_variable_declaration* deklaruje co najmniej jedną zmienną lokalną.</span><span class="sxs-lookup"><span data-stu-id="404d0-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="404d0-197">*Local_variable_type* *local_variable_declaration* bezpośrednio określa typ zmiennych wprowadzonych przez deklarację lub wskazuje identyfikator `var` , który powinien zostać wywnioskowany na podstawie skład.</span><span class="sxs-lookup"><span data-stu-id="404d0-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="404d0-198">Po tym typie następuje lista *local_variable_declarator*s, z których każdy wprowadza nową zmienną.</span><span class="sxs-lookup"><span data-stu-id="404d0-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="404d0-199">*Local_variable_declarator* składa się z *identyfikatora* , który nazywa zmienną, opcjonalnie po której następuje token`=`"" i *local_variable_initializer* , który daje początkową wartość zmiennej.</span><span class="sxs-lookup"><span data-stu-id="404d0-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="404d0-200">W kontekście deklaracji zmiennej lokalnej, identyfikator var działa jako kontekstowe słowo kluczowe ([słowa kluczowe](lexical-structure.md#keywords)). Gdy *local_variable_type* jest określony jako `var` , `var` a typ nie jest w zakresie, deklaracja jest ***niejawnie wpisaną deklaracją zmiennej lokalnej***, której typ jest wywnioskowany na podstawie typu skojarzonego inicjatora wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="404d0-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="404d0-201">Niejawnie wpisane deklaracje zmiennych lokalnych podlegają następującym ograniczeniom:</span><span class="sxs-lookup"><span data-stu-id="404d0-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="404d0-202">*Local_variable_declaration* nie może zawierać wielu *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="404d0-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="404d0-203">*Local_variable_declarator* musi zawierać element *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="404d0-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="404d0-204">*Local_variable_initializer* musi być *wyrażeniem*.</span><span class="sxs-lookup"><span data-stu-id="404d0-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="404d0-205">*Wyrażenie* inicjatora musi mieć typ czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="404d0-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="404d0-206">*Wyrażenie* inicjatora nie może odwoływać się do zadeklarowanej zmiennej</span><span class="sxs-lookup"><span data-stu-id="404d0-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="404d0-207">Poniżej przedstawiono przykłady nieprawidłowych niejawnie wpisanych deklaracji zmiennych lokalnych:</span><span class="sxs-lookup"><span data-stu-id="404d0-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="404d0-208">Wartość zmiennej lokalnej jest uzyskiwana w wyrażeniu przy użyciu *simple_name* ([nazw prostych](expressions.md#simple-names)), a wartość zmiennej lokalnej jest modyfikowana przy użyciu *przypisania* ([Operatory przypisania](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="404d0-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="404d0-209">Zmienna lokalna musi być ostatecznie przypisana ([przypisanie określone](variables.md#definite-assignment)) w każdej lokalizacji, w której jest uzyskiwana wartość.</span><span class="sxs-lookup"><span data-stu-id="404d0-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="404d0-210">Zakres zmiennej lokalnej zadeklarowanej w *local_variable_declaration* to blok, w którym występuje deklaracja.</span><span class="sxs-lookup"><span data-stu-id="404d0-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="404d0-211">Jest to błąd, aby odwołać się do zmiennej lokalnej w pozycji tekstowej, która poprzedza *local_variable_declarator* zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="404d0-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="404d0-212">W zakresie zmiennej lokalnej jest to błąd czasu kompilacji, aby zadeklarować inną zmienną lokalną lub stałą o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="404d0-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="404d0-213">Deklaracja zmiennej lokalnej, która deklaruje wiele zmiennych, jest równoważna z wieloma deklaracjami pojedynczych zmiennych tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="404d0-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="404d0-214">Ponadto inicjator zmiennej w deklaracji zmiennej lokalnej odpowiada dokładnie instrukcji przypisania, która jest wstawiana bezpośrednio po deklaracji.</span><span class="sxs-lookup"><span data-stu-id="404d0-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="404d0-215">Przykład</span><span class="sxs-lookup"><span data-stu-id="404d0-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="404d0-216">odpowiada dokładnie na</span><span class="sxs-lookup"><span data-stu-id="404d0-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="404d0-217">W deklaracji niejawnie wpisanej zmiennej lokalnej typ zadeklarowanej zmiennej lokalnej jest taki sam jak typ wyrażenia użytego do zainicjowania zmiennej.</span><span class="sxs-lookup"><span data-stu-id="404d0-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="404d0-218">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="404d0-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="404d0-219">Niejawnie wpisane deklaracje zmiennej lokalnej są dokładnie równoważne z następującymi jawnie określonymi deklaracjami:</span><span class="sxs-lookup"><span data-stu-id="404d0-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="404d0-220">Lokalne deklaracje stałych</span><span class="sxs-lookup"><span data-stu-id="404d0-220">Local constant declarations</span></span>

<span data-ttu-id="404d0-221">*Local_constant_declaration* deklaruje co najmniej jedną stałą lokalną.</span><span class="sxs-lookup"><span data-stu-id="404d0-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="404d0-222">*Typ* *local_constant_declaration* określa typ stałych wprowadzonych przez deklarację.</span><span class="sxs-lookup"><span data-stu-id="404d0-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="404d0-223">Po tym typie następuje lista *constant_declarator*s, z których każdy wprowadza nową stałą.</span><span class="sxs-lookup"><span data-stu-id="404d0-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="404d0-224">*Constant_declarator* składa się z *identyfikatora* , który nazywa stałą, po którym następuje token`=`"", po którym następuje *constant_expression* ([wyrażenia stałe](expressions.md#constant-expressions)), które dają wartość stałej.</span><span class="sxs-lookup"><span data-stu-id="404d0-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="404d0-225">*Typ* i *constant_expression* deklaracji stałej lokalnej muszą być zgodne z tymi samymi regułami, co w przypadku deklaracji stałego elementu członkowskiego ([stałe](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="404d0-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="404d0-226">Wartość stałej lokalnej jest uzyskiwana w wyrażeniu przy użyciu elementu *simple_name* ([Simple Names](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="404d0-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="404d0-227">Zakres stałej lokalnej to blok, w którym występuje deklaracja.</span><span class="sxs-lookup"><span data-stu-id="404d0-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="404d0-228">Jest to błąd, aby odwołać się do lokalnej stałej w pozycji tekstowej, która poprzedza jej *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="404d0-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="404d0-229">W zakresie stałej lokalnej jest to błąd czasu kompilacji, aby zadeklarować inną zmienną lokalną lub stałą o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="404d0-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="404d0-230">Lokalna deklaracja stałej, która deklaruje wiele stałych jest równoznaczna z wieloma deklaracjami pojedynczych stałych tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="404d0-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="404d0-231">Instrukcje wyrażeń</span><span class="sxs-lookup"><span data-stu-id="404d0-231">Expression statements</span></span>

<span data-ttu-id="404d0-232">*Expression_statement* oblicza określone wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="404d0-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="404d0-233">Wartość obliczona przez wyrażenie (jeśli istnieje) jest odrzucana.</span><span class="sxs-lookup"><span data-stu-id="404d0-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="404d0-234">Nie wszystkie wyrażenia są dozwolone jako instrukcje.</span><span class="sxs-lookup"><span data-stu-id="404d0-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="404d0-235">W szczególności wyrażenia takie jak `x + y` i `x == 1` , które tylko obliczają wartość (która zostanie odrzucona), nie są dozwolone jako instrukcje.</span><span class="sxs-lookup"><span data-stu-id="404d0-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="404d0-236">Wykonanie elementu *expression_statement* powoduje oszacowanie zawartego wyrażenia, a następnie przekazanie kontroli do punktu końcowego *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="404d0-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="404d0-237">Punkt końcowy *expression_statement* jest osiągalny, jeśli *expression_statement* jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="404d0-238">Instrukcje wyboru</span><span class="sxs-lookup"><span data-stu-id="404d0-238">Selection statements</span></span>

<span data-ttu-id="404d0-239">Instrukcje wyboru wybierz jedną z wielu możliwych instrukcji do wykonania na podstawie wartości niektórych wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="404d0-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="404d0-240">Instrukcja if</span><span class="sxs-lookup"><span data-stu-id="404d0-240">The if statement</span></span>

<span data-ttu-id="404d0-241">`if` Instrukcja wybiera instrukcję do wykonania na podstawie wartości wyrażenia logicznego.</span><span class="sxs-lookup"><span data-stu-id="404d0-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="404d0-242">Część jest skojarzona z leksykalną najbliższą poprzednią `if` , która jest dozwolona przez składnię. `else`</span><span class="sxs-lookup"><span data-stu-id="404d0-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="404d0-243">W tym celu `if` , instrukcja formularza</span><span class="sxs-lookup"><span data-stu-id="404d0-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="404d0-244">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="404d0-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="404d0-245">`if` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-246">*Boolean_expression* ([wyrażenia logiczne](expressions.md#boolean-expressions)) są oceniane.</span><span class="sxs-lookup"><span data-stu-id="404d0-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="404d0-247">Jeśli wyrażenie logiczne daje `true`, sterowanie jest przekazywane do pierwszej osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="404d0-248">Gdy i jeśli kontrola osiągnie punkt końcowy tej instrukcji, sterowanie jest przekazywane do punktu `if` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="404d0-249">Jeśli wyrażenie logiczne zwraca `false` wartość i `else` jeśli część jest obecna, kontrola jest przekazywana do drugiej osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="404d0-250">Gdy i jeśli kontrola osiągnie punkt końcowy tej instrukcji, sterowanie jest przekazywane do punktu `if` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="404d0-251">Jeśli wyrażenie logiczne zwraca `false` wartość i `else` jeśli część nie istnieje, kontrola jest przekazywana do punktu `if` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="404d0-252">Pierwsza osadzona instrukcja `if` instrukcji jest osiągalna, `if` Jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `false`.</span><span class="sxs-lookup"><span data-stu-id="404d0-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="404d0-253">Druga osadzona instrukcja `if` instrukcji, jeśli jest obecna, jest osiągalna, `if` Jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `true`.</span><span class="sxs-lookup"><span data-stu-id="404d0-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="404d0-254">Punkt `if` końcowy instrukcji jest osiągalny, jeśli punkt końcowy co najmniej jednej z jej osadzonych instrukcji jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="404d0-255">Ponadto punkt `if` końcowy instrukcji bez `else` `if` części jest osiągalny, jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `true`.</span><span class="sxs-lookup"><span data-stu-id="404d0-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="404d0-256">Instrukcja Switch</span><span class="sxs-lookup"><span data-stu-id="404d0-256">The switch statement</span></span>

<span data-ttu-id="404d0-257">Instrukcja Switch wybiera do wykonania list instrukcji z skojarzoną etykietą Switch, która odpowiada wartości wyrażenia Switch.</span><span class="sxs-lookup"><span data-stu-id="404d0-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="404d0-258">*Switch_statement* składa się ze słowa `switch`kluczowego, po którym następuje wyrażenie ujęte w nawiasy (zwane wyrażeniem Switch), a następnie *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="404d0-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="404d0-259">*Switch_block* składa się z zera lub więcej *switch_section*s ujętych w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="404d0-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="404d0-260">Każdy *switch_section* składa się z jednego lub więcej *switch_label*s, po których następuje *statement_list* ([Lista instrukcji](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="404d0-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="404d0-261">Typ`switch` ***rządzący*** instrukcji jest ustalany przez wyrażenie Switch.</span><span class="sxs-lookup"><span data-stu-id="404d0-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="404d0-262">`sbyte`Jeśli typ wyrażenia Switch to, ,`uint` `byte` `ushort` ,,`string`,,,, `short` ,,`int`lub `long` `ulong` `bool` `char`  *enum_type*, lub jeśli jest typem dopuszczającym wartość null odpowiadającym jednemu z tych typów, to jest to typ, `switch` który jest typem instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="404d0-263">W przeciwnym razie dokładnie jedna zdefiniowana przez użytkownika konwersja niejawna ([konwersje zdefiniowane przez użytkownika](conversions.md#user-defined-conversions)) musi istnieć z typu wyrażenia przełącznika do jednego z następujących możliwych rodzajów typów: `sbyte`, `byte`, `short`, `ushort` , `int` `uint`, ,,`string`,, lub, typu dopuszczającego wartość null odpowiadającego jednemu z tych typów. `long` `ulong` `char`</span><span class="sxs-lookup"><span data-stu-id="404d0-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="404d0-264">W przeciwnym razie, jeśli taka niejawna konwersja nie istnieje lub jeśli istnieje więcej niż jedna taka niejawna konwersja, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="404d0-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-265">Wyrażenie stałe każdej `case` etykiety musi oznaczać wartość, która jest niejawnie przekonwertowana ([konwersje niejawne](conversions.md#implicit-conversions)) na typ `switch` decydujący instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="404d0-266">Błąd czasu kompilacji występuje, jeśli co `case` najmniej dwie etykiety w tej samej `switch` instrukcji określają tę samą wartość stałą.</span><span class="sxs-lookup"><span data-stu-id="404d0-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="404d0-267">W instrukcji switch może istnieć co `default` najwyżej jedna etykieta.</span><span class="sxs-lookup"><span data-stu-id="404d0-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="404d0-268">`switch` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-269">Wyrażenie Switch jest oceniane i konwertowane na typ zarządzający.</span><span class="sxs-lookup"><span data-stu-id="404d0-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="404d0-270">Jeśli jedna ze stałych określonych w `case` etykiecie w tej samej `switch` instrukcji jest równa wartości wyrażenia Switch, formant zostanie przeniesiony do listy instrukcji po dopasowanym `case` oznaczeniu.</span><span class="sxs-lookup"><span data-stu-id="404d0-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="404d0-271">Jeśli żadna ze stałych określonych `case` w etykietach w tej samej `switch` instrukcji nie jest równa wartości `default` wyrażenia Switch i jeśli etykieta jest obecna, sterowanie `default` jest przekazywane do listy instrukcji po oznakowan.</span><span class="sxs-lookup"><span data-stu-id="404d0-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="404d0-272">Jeśli żadna ze stałych określonych `case` w etykietach w tej samej `switch` instrukcji nie jest równa wartości wyrażenia Switch i jeśli żadna etykieta nie `default` jest obecna, kontrola `switch` jest przekazywana do punktu końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="404d0-273">Jeśli punkt końcowy listy instrukcji przełącznika jest osiągalny, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="404d0-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="404d0-274">Ta wartość jest znana jako reguła "bez powracania".</span><span class="sxs-lookup"><span data-stu-id="404d0-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="404d0-275">Przykład</span><span class="sxs-lookup"><span data-stu-id="404d0-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="404d0-276">jest prawidłowy, ponieważ żadna sekcja Switch nie ma osiągalnego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="404d0-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="404d0-277">W przeciwieństwie do C++języka C i wykonywanie sekcji Switch nie jest dozwolone w sekcji "przechodzenie" do następnego przełącznika i przykład</span><span class="sxs-lookup"><span data-stu-id="404d0-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="404d0-278">powoduje błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="404d0-278">results in a compile-time error.</span></span> <span data-ttu-id="404d0-279">Po wykonaniu sekcji Switch po wykonaniu kolejnej sekcji Switch musi być użyta jawna `goto case` instrukcja or: `goto default`</span><span class="sxs-lookup"><span data-stu-id="404d0-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="404d0-280">W *switch_section*jest dozwolonych wiele etykiet.</span><span class="sxs-lookup"><span data-stu-id="404d0-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="404d0-281">Przykład</span><span class="sxs-lookup"><span data-stu-id="404d0-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="404d0-282">jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="404d0-282">is valid.</span></span> <span data-ttu-id="404d0-283">Przykład nie narusza reguły "No-through", ponieważ etykiety `case 2:` i `default:` są częścią tego samego *switch_sectionu*.</span><span class="sxs-lookup"><span data-stu-id="404d0-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="404d0-284">Reguła "bez powracania" uniemożliwia wspólną klasę błędów występujących w języku C oraz przypadki C++ , `break` w których instrukcje są przypadkowo pomijane.</span><span class="sxs-lookup"><span data-stu-id="404d0-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="404d0-285">Ponadto ze względu na tę regułę sekcje `switch` przełącznika instrukcji można arbitralnie zmienić bez wpływu na zachowanie instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="404d0-286">Na przykład sekcje `switch` powyższej instrukcji można odwrócić bez wpływu na zachowanie instrukcji:</span><span class="sxs-lookup"><span data-stu-id="404d0-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="404d0-287">Lista instrukcji switch zwykle kończy się w `break`instrukcji, `goto case`, lub `goto default` , ale dowolna konstrukcja, która renderuje punkt końcowy listy instrukcji jest nieosiągalna.</span><span class="sxs-lookup"><span data-stu-id="404d0-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="404d0-288">Na przykład, `while` instrukcja kontrolowana przez wyrażenie `true` logiczne jest znana, aby nie dotrzeć do punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="404d0-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="404d0-289">Podobnie, instrukcja `throw` lub `return` zawsze przenosi formant w innym miejscu i nigdy nie dociera do punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="404d0-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="404d0-290">W tym przypadku następujący przykład jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="404d0-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="404d0-291">Typ `switch` decydujący instrukcji może być typem `string`.</span><span class="sxs-lookup"><span data-stu-id="404d0-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="404d0-292">Przykład:</span><span class="sxs-lookup"><span data-stu-id="404d0-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="404d0-293">Podobnie jak w przypadku operatorów równości ciągów ([Operatory równości ciągów](expressions.md#string-equality-operators)) `switch` , instrukcja uwzględnia wielkość liter i wykona daną sekcję Switch tylko wtedy, gdy ciąg wyrażenia przełącznika dokładnie pasuje do `case` stałej etykiet.</span><span class="sxs-lookup"><span data-stu-id="404d0-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="404d0-294">W przypadku, gdy typem `switch` instrukcji jest `string`, wartość `null` jest dozwolona jako stała etykiet wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="404d0-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="404d0-295">*Statement_list*s *switch_block* może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="404d0-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="404d0-296">Zakres zmiennej lokalnej lub stałej zadeklarowanej w bloku przełącznika to blok przełącznika.</span><span class="sxs-lookup"><span data-stu-id="404d0-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="404d0-297">Lista instrukcji danej sekcji Switch jest osiągalna, jeśli `switch` instrukcja jest osiągalna i co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="404d0-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="404d0-298">Wyrażenie Switch jest wartością niestałą.</span><span class="sxs-lookup"><span data-stu-id="404d0-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="404d0-299">Wyrażenie Switch jest stałą wartością zgodną `case` z etykietą w sekcji Switch.</span><span class="sxs-lookup"><span data-stu-id="404d0-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="404d0-300">Wyrażenie Switch jest wartością stałą, która nie jest zgodna z `case` żadną etykietą, a sekcja Switch `default` zawiera etykietę.</span><span class="sxs-lookup"><span data-stu-id="404d0-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="404d0-301">Do etykiety przełącznika w sekcji przełącznika jest przywoływana osiągalna `goto case` instrukcja `goto default` or.</span><span class="sxs-lookup"><span data-stu-id="404d0-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="404d0-302">Punkt `switch` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="404d0-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="404d0-303">Instrukcja zawiera `break` osiągalną`switch` instrukcję, która kończy wykonywanie instrukcji. `switch`</span><span class="sxs-lookup"><span data-stu-id="404d0-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="404d0-304">Instrukcja jest osiągalna, wyrażenie Switch jest wartością niestałą i żadna etykieta nie `default` jest obecna. `switch`</span><span class="sxs-lookup"><span data-stu-id="404d0-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="404d0-305">Instrukcja jest osiągalna, wyrażenie Switch jest wartością stałą, która nie jest zgodna z `case` żadną etykietą i `default` żadna etykieta nie jest obecna. `switch`</span><span class="sxs-lookup"><span data-stu-id="404d0-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="404d0-306">Instrukcje iteracji</span><span class="sxs-lookup"><span data-stu-id="404d0-306">Iteration statements</span></span>

<span data-ttu-id="404d0-307">Instrukcja iteracji wielokrotnie wykonuje osadzoną instrukcję.</span><span class="sxs-lookup"><span data-stu-id="404d0-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="404d0-308">Instrukcja while</span><span class="sxs-lookup"><span data-stu-id="404d0-308">The while statement</span></span>

<span data-ttu-id="404d0-309">`while` Instrukcja warunkowo wykonuje osadzoną instrukcję zero lub więcej razy.</span><span class="sxs-lookup"><span data-stu-id="404d0-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="404d0-310">`while` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-311">*Boolean_expression* ([wyrażenia logiczne](expressions.md#boolean-expressions)) są oceniane.</span><span class="sxs-lookup"><span data-stu-id="404d0-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="404d0-312">Jeśli wyrażenie logiczne daje `true`, sterowanie jest przekazywane do osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="404d0-313">Gdy i Jeśli kontrolka osiągnie punkt końcowy osadzonej instrukcji (prawdopodobnie z wykonania `continue` instrukcji), sterowanie jest przekazywane na początek `while` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="404d0-314">Jeśli wyrażenie logiczne daje `false`, sterowanie jest przekazywane do punktu `while` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="404d0-315">W osadzonej instrukcji `while` instrukcji `break` instrukcja ([instrukcja break](statements.md#the-break-statement)) może być używana do transferowania `while` kontroli do punktu końcowego instrukcji (w związku z tym kończącej iterację osadzonej instrukcji) i `continue` instrukcja ([Instrukcja continue](statements.md#the-continue-statement)) może być używana do transferowania kontroli do punktu końcowego osadzonej instrukcji (w związku z tym wykonywanie innej `while` iteracji instrukcji).</span><span class="sxs-lookup"><span data-stu-id="404d0-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="404d0-316">Osadzona instrukcja `while` instrukcji jest osiągalna, `while` Jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `false`.</span><span class="sxs-lookup"><span data-stu-id="404d0-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="404d0-317">Punkt `while` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="404d0-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="404d0-318">Instrukcja zawiera `break` osiągalną`while` instrukcję, która kończy wykonywanie instrukcji. `while`</span><span class="sxs-lookup"><span data-stu-id="404d0-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="404d0-319">Instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `true`. `while`</span><span class="sxs-lookup"><span data-stu-id="404d0-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="404d0-320">Instrukcja do</span><span class="sxs-lookup"><span data-stu-id="404d0-320">The do statement</span></span>

<span data-ttu-id="404d0-321">`do` Instrukcja warunkowo wykonuje osadzoną instrukcję jeden lub więcej razy.</span><span class="sxs-lookup"><span data-stu-id="404d0-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="404d0-322">`do` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-323">Kontrolka jest przenoszona do osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="404d0-324">Gdy i jeśli formant osiągnie punkt końcowy osadzonej instrukcji (prawdopodobnie z wykonania `continue` instrukcji), jest oceniane *Boolean_expression* ([wyrażenia logiczne](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="404d0-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="404d0-325">Jeśli wyrażenie logiczne daje `true`, sterowanie jest przekazywane na początek `do` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="404d0-326">W przeciwnym razie kontrola jest przekazywana do punktu `do` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="404d0-327">W osadzonej instrukcji `do` instrukcji `break` instrukcja ([instrukcja break](statements.md#the-break-statement)) może być używana do transferowania `do` kontroli do punktu końcowego instrukcji (w związku z tym kończącej iterację osadzonej instrukcji) i `continue` instrukcja ([Instrukcja continue](statements.md#the-continue-statement)) może służyć do transferowania kontroli do punktu końcowego osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="404d0-328">Osadzona instrukcja `do` instrukcji jest osiągalna, `do` Jeśli instrukcja jest osiągalna.</span><span class="sxs-lookup"><span data-stu-id="404d0-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="404d0-329">Punkt `do` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="404d0-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="404d0-330">Instrukcja zawiera `break` osiągalną`do` instrukcję, która kończy wykonywanie instrukcji. `do`</span><span class="sxs-lookup"><span data-stu-id="404d0-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="404d0-331">Punkt końcowy osadzonej instrukcji jest osiągalny, a wyrażenie logiczne nie ma stałej wartości `true`.</span><span class="sxs-lookup"><span data-stu-id="404d0-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="404d0-332">Instrukcja for</span><span class="sxs-lookup"><span data-stu-id="404d0-332">The for statement</span></span>

<span data-ttu-id="404d0-333">`for` Instrukcja oblicza sekwencję wyrażeń inicjalizacji, a następnie, gdy warunek ma wartość true, wielokrotnie wykonuje osadzoną instrukcję i szacuje sekwencję wyrażeń iteracji.</span><span class="sxs-lookup"><span data-stu-id="404d0-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="404d0-334">*For_initializer*, jeśli jest obecny, składa się z *local_variable_declaration* ([lokalnych deklaracji zmiennych](statements.md#local-variable-declarations)) lub listy *statement_expression*s ([instrukcje wyrażeń](statements.md#expression-statements)) oddzielone przecinkami.</span><span class="sxs-lookup"><span data-stu-id="404d0-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="404d0-335">Zakres zmiennej lokalnej zadeklarowanej przez *for_initializer* zaczyna się od *local_variable_declarator* dla zmiennej i rozciąga się na koniec osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="404d0-336">Zakres zawiera *for_condition* i *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="404d0-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="404d0-337">*For_condition*, jeśli jest obecny, musi być *Boolean_expression* ([wyrażenie logiczne](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="404d0-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="404d0-338">*For_iterator*, jeśli jest obecny, składa się z listy *statement_expression*s ([instrukcje wyrażeń](statements.md#expression-statements)) oddzielone przecinkami.</span><span class="sxs-lookup"><span data-stu-id="404d0-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="404d0-339">Instrukcja for jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-340">Jeśli *for_initializer* jest obecny, inicjatory zmiennych lub wyrażenia instrukcji są wykonywane w kolejności, w jakiej zostały wpisane.</span><span class="sxs-lookup"><span data-stu-id="404d0-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="404d0-341">Ten krok jest wykonywany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="404d0-341">This step is only performed once.</span></span>
*  <span data-ttu-id="404d0-342">Jeśli *for_condition* jest obecny, zostanie ona oceniona.</span><span class="sxs-lookup"><span data-stu-id="404d0-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="404d0-343">Jeśli *for_condition* nie istnieje lub w przypadku oceny `true`, kontrola jest przekazywana do osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="404d0-344">Gdy i Jeśli kontrolka osiągnie punkt końcowy osadzonej instrukcji (prawdopodobnie z wykonania `continue` instrukcji), wyrażenia *for_iterator*(jeśli istnieją) są oceniane w sekwencji, a następnie wykonywana jest inna iteracja, rozpoczynając od Obliczanie *for_condition* w powyższym kroku.</span><span class="sxs-lookup"><span data-stu-id="404d0-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="404d0-345">Jeśli *for_condition* jest obecny i oceny `false`, kontrola jest przekazywana do punktu `for` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="404d0-346">W osadzonej instrukcji `for` instrukcji `break` instrukcja ([instrukcja break](statements.md#the-break-statement)) może być używana do transferowania `for` kontroli do punktu końcowego instrukcji (w związku z tym kończącej iterację osadzonej instrukcji) i `continue` instrukcja ([Instrukcja continue](statements.md#the-continue-statement)) może służyć do transferowania kontroli do punktu końcowego osadzonej instrukcji (w związku z tym wykonywania *for_iterator* i `for` wykonywania innej iteracji instrukcji, zaczynając od *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="404d0-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="404d0-347">Osadzona instrukcja `for` instrukcji jest osiągalna, jeśli jedno z następujących warunków jest prawdziwe:</span><span class="sxs-lookup"><span data-stu-id="404d0-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="404d0-348">Instrukcja jest osiągalna i nie ma *for_condition.* `for`</span><span class="sxs-lookup"><span data-stu-id="404d0-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="404d0-349">Instrukcja jest osiągalna, a *for_condition* jest obecny i nie ma stałej wartości `false`. `for`</span><span class="sxs-lookup"><span data-stu-id="404d0-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="404d0-350">Punkt `for` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="404d0-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="404d0-351">Instrukcja zawiera `break` osiągalną`for` instrukcję, która kończy wykonywanie instrukcji. `for`</span><span class="sxs-lookup"><span data-stu-id="404d0-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="404d0-352">Instrukcja jest osiągalna, a *for_condition* jest obecny i nie ma stałej wartości `true`. `for`</span><span class="sxs-lookup"><span data-stu-id="404d0-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="404d0-353">Instrukcja foreach</span><span class="sxs-lookup"><span data-stu-id="404d0-353">The foreach statement</span></span>

<span data-ttu-id="404d0-354">`foreach` Instrukcja wylicza elementy kolekcji, wykonując osadzoną instrukcję dla każdego elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="404d0-355">*Typ* i *Identyfikator* `foreach` instrukcji deklaruje ***zmienną iteracji*** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="404d0-356">Jeśli identyfikator jest określony jako *local_variable_type*, a typ o nazwie `var` nie należy do zakresu, Zmienna iteracji jest określana jako ***Zmienna iteracji niejawnie wpisanej***, a jej typ jest traktowany jako typ elementu `var` `foreach` instrukcja, jak określono poniżej.</span><span class="sxs-lookup"><span data-stu-id="404d0-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="404d0-357">Zmienna iteracji odnosi się do zmiennej lokalnej tylko do odczytu z zakresem, który wykracza poza osadzoną instrukcję.</span><span class="sxs-lookup"><span data-stu-id="404d0-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="404d0-358">Podczas wykonywania `foreach` instrukcji Zmienna iteracji reprezentuje element kolekcji, dla którego obecnie trwa wykonywanie iteracji.</span><span class="sxs-lookup"><span data-stu-id="404d0-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="404d0-359">Błąd czasu kompilacji występuje, `++` Jeśli osadzona instrukcja próbuje zmodyfikować zmienną iteracji (poprzez przypisanie lub operatory i `--` ) albo przekazać zmienną iteracji jako `ref` parametr lub `out` .</span><span class="sxs-lookup"><span data-stu-id="404d0-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="404d0-360">W następujących przypadkach dla zwięzłości, `IEnumerable` `IEnumerable<T>` , `IEnumerator`i `IEnumerator<T>` zapoznaj się z odpowiednimi typami w przestrzeniach nazw `System.Collections` i `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="404d0-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="404d0-361">Przetwarzanie instrukcji foreach w czasie kompilacji najpierw określa ***Typ kolekcji***, ***Typ modułu wyliczającego*** i ***Typ elementu*** wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="404d0-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="404d0-362">To obliczanie jest przeprowadzane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="404d0-363">Jeśli `X` typ *wyrażenia* jest typem tablicy, istnieje niejawna `X` `IEnumerable` konwersja odwołania z do interfejsu (ponieważ `System.Array` implementuje ten interfejs).</span><span class="sxs-lookup"><span data-stu-id="404d0-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="404d0-364">***Typem kolekcji*** jest `IEnumerable` interfejs, ***typem modułu wyliczającego*** jest `IEnumerator` interfejs, a ***Typ*** `X`elementu jest typem elementu typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="404d0-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="404d0-365">`X` Jeśli typem *wyrażenia* jest `dynamic` ,istniejeniejawnakonwersjazwyrażeniadointerfejsu(niejawnekonwersjedynamiczne).`IEnumerable` [](conversions.md#implicit-dynamic-conversions)</span><span class="sxs-lookup"><span data-stu-id="404d0-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="404d0-366">***Typ kolekcji*** jest `IEnumerable` interfejsem, a ***typem modułu wyliczającego*** jest `IEnumerator` interfejs.</span><span class="sxs-lookup"><span data-stu-id="404d0-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="404d0-367">`dynamic` `object` Jeśli identyfikator jest określony jako local_variable_type, typ elementu to, w przeciwnym razie. `var`</span><span class="sxs-lookup"><span data-stu-id="404d0-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="404d0-368">W przeciwnym razie Ustal, czy `X` typ ma odpowiednią `GetEnumerator` metodę:</span><span class="sxs-lookup"><span data-stu-id="404d0-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="404d0-369">Wykonaj wyszukiwanie elementów członkowskich w typie `X` z identyfikatorem `GetEnumerator` i bez argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="404d0-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="404d0-370">Jeśli wyszukiwanie elementu członkowskiego nie produkuje dopasowania lub tworzy niejednoznaczność lub tworzy dopasowanie, które nie jest grupą metod, należy sprawdzić, czy wyliczalny interfejs został opisany poniżej.</span><span class="sxs-lookup"><span data-stu-id="404d0-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="404d0-371">Zaleca się, aby ostrzeżenie było wydawane, gdy wyszukiwanie elementów członkowskich produkuje wszystkie elementy poza grupą metod lub nie odpowiada.</span><span class="sxs-lookup"><span data-stu-id="404d0-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="404d0-372">Wykonaj rozwiązanie przeciążenia przy użyciu grupy metod i pustej listy argumentów.</span><span class="sxs-lookup"><span data-stu-id="404d0-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="404d0-373">Jeśli rozwiązanie przeciążenia skutkuje brakiem odpowiednich metod, wyniki są niejednoznaczne lub mają jedną najlepszą metodę, ale ta metoda jest statyczna lub nie jest publiczna, Wyszukaj wyliczalny interfejs zgodnie z poniższym opisem.</span><span class="sxs-lookup"><span data-stu-id="404d0-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="404d0-374">Zaleca się, aby ostrzeżenie było wydawane, jeśli rozwiązanie przeciążania produkuje wszystko, z wyjątkiem jednoznacznej metody wystąpienia publicznego lub nie ma zastosowania do odpowiednich metod.</span><span class="sxs-lookup"><span data-stu-id="404d0-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="404d0-375">Jeśli zwracany typ `E` `GetEnumerator` metody nie jest klasą, strukturą lub typem interfejsu, jest generowany błąd i nie są podejmowane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="404d0-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="404d0-376">Wyszukiwanie elementu członkowskiego jest `E` wykonywane na z `Current` identyfikatorem i bez argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="404d0-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="404d0-377">Jeśli wyszukiwanie elementu członkowskiego nie powoduje dopasowania, wynikiem jest błąd lub wynikiem jest wszystko z wyjątkiem właściwości wystąpienia publicznego, która umożliwia odczytywanie, zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="404d0-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="404d0-378">Wyszukiwanie elementu członkowskiego jest `E` wykonywane na z `MoveNext` identyfikatorem i bez argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="404d0-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="404d0-379">Jeśli wyszukiwanie elementu członkowskiego nie powoduje dopasowania, wynikiem jest błąd lub wynikiem jest wszystko poza grupą metod, zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="404d0-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="404d0-380">Rozpoznanie przeciążenia jest wykonywane w grupie metod z pustą listą argumentów.</span><span class="sxs-lookup"><span data-stu-id="404d0-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="404d0-381">Jeśli rozwiązanie przeciążenia skutkuje brakiem odpowiednich metod, wyniki są niejednoznaczne lub mają jedną najlepszą metodę, ale ta metoda jest statyczna lub nie jest publiczna lub jej typem zwracanym jest `bool`błąd i nie są podejmowane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="404d0-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="404d0-382">***Typ kolekcji*** to `X`, ***Typ*** `E`modułu wyliczającego, `Current` a ***Typ elementu*** to typ właściwości.</span><span class="sxs-lookup"><span data-stu-id="404d0-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="404d0-383">W przeciwnym razie Sprawdź, czy wyliczalny interfejs:</span><span class="sxs-lookup"><span data-stu-id="404d0-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="404d0-384">Jeśli wśród wszystkich `Ti` typów, dla których istnieje niejawna konwersja z `X` do na `IEnumerable<Ti>`, istnieje unikatowy typ `T` , który `T` nie `dynamic` jest i dla wszystkich innych `Ti` niejawna `IEnumerable<T>` konwersja `IEnumerable<Ti>`z na, a następnie ***Typ kolekcji*** to `IEnumerable<T>`interfejs, ***typem modułu wyliczającego*** jest interfejs `IEnumerator<T>`, a ***typem elementu*** jest `T`.</span><span class="sxs-lookup"><span data-stu-id="404d0-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="404d0-385">W przeciwnym razie, jeśli istnieje więcej niż jeden taki `T`typ, zostanie wygenerowany błąd i nie zostaną wykonane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="404d0-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="404d0-386">W przeciwnym razie, jeśli istnieje niejawna `X` konwersja z `System.Collections.IEnumerable` do interfejsu, ***typem kolekcji*** jest ten interfejs, ***typem modułu wyliczającego*** jest interfejs `System.Collections.IEnumerator`, a ***typem elementu*** jest `object`.</span><span class="sxs-lookup"><span data-stu-id="404d0-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="404d0-387">W przeciwnym razie zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="404d0-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="404d0-388">Powyższe kroki, w przypadku powodzenia, jednoznacznie tworzą typ `C`kolekcji, typ `E` modułu wyliczającego i typ `T`elementu.</span><span class="sxs-lookup"><span data-stu-id="404d0-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="404d0-389">Instrukcja foreach formularza</span><span class="sxs-lookup"><span data-stu-id="404d0-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="404d0-390">następnie jest rozwinięty do:</span><span class="sxs-lookup"><span data-stu-id="404d0-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="404d0-391">Zmienna `e` nie jest widoczna lub dostępna dla wyrażenia `x` lub osadzonej instrukcji ani żadnego innego kodu źródłowego programu.</span><span class="sxs-lookup"><span data-stu-id="404d0-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="404d0-392">Zmienna `v` jest tylko do odczytu w osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="404d0-393">Jeśli nie istnieje jawna konwersja ([jawne konwersje](conversions.md#explicit-conversions)) z `T` (typ elementu) do `V` ( *local_variable_type* w instrukcji foreach), zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="404d0-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="404d0-394">Jeśli `x` ma wartość `null`, `System.NullReferenceException` jest zgłaszany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="404d0-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="404d0-395">Implementacja może zaimplementować daną instrukcję foreach inaczej, np. ze względu na wydajność, o ile zachowanie jest spójne z powyższym rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="404d0-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="404d0-396">Umieszczanie `v` wewnątrz pętli while jest ważne dla sposobu, w jaki są przechwytywane przez jakąkolwiek anonimową funkcję występującą w *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="404d0-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="404d0-397">Przykład:</span><span class="sxs-lookup"><span data-stu-id="404d0-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="404d0-398">Jeśli `v` został zadeklarowany poza pętlą while, będzie współużytkowana przez wszystkie iteracje, a jej wartość po pętli for będzie wartością końcową, `13`czyli to, `f` co to jest wywołanie.</span><span class="sxs-lookup"><span data-stu-id="404d0-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="404d0-399">Zamiast tego, ponieważ każda iteracja ma własną `v`zmienną, przechwycona `f` przez w pierwszej iteracji będzie nadal przechowywać wartość `7`, co oznacza, że zostanie wydrukowany.</span><span class="sxs-lookup"><span data-stu-id="404d0-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="404d0-400">(Uwaga: wcześniejsze wersje C# zadeklarowane `v` poza pętlą while).</span><span class="sxs-lookup"><span data-stu-id="404d0-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="404d0-401">Treść bloku finally jest tworzona zgodnie z poniższymi krokami:</span><span class="sxs-lookup"><span data-stu-id="404d0-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="404d0-402">Jeśli istnieje niejawna konwersja z `E` `System.IDisposable` do interfejsu,</span><span class="sxs-lookup"><span data-stu-id="404d0-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="404d0-403">Jeśli `E` jest typem wartości niedopuszczających wartości null, klauzula finally jest rozwinięta do semantycznego odpowiednika:</span><span class="sxs-lookup"><span data-stu-id="404d0-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="404d0-404">W przeciwnym razie klauzula finally jest rozwinięta do semantycznego odpowiednika:</span><span class="sxs-lookup"><span data-stu-id="404d0-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="404d0-405">z tą różnicą, że jeśli `E` jest typem wartości, lub parametrem typu skonkretyzowanym do typu wartości, `e` rzutowanie na `System.IDisposable` nie spowoduje wystąpienia opakowania.</span><span class="sxs-lookup"><span data-stu-id="404d0-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="404d0-406">W przeciwnym razie `E` , jeśli jest typem zapieczętowanym, klauzula finally jest rozwinięta do pustego bloku:</span><span class="sxs-lookup"><span data-stu-id="404d0-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="404d0-407">W przeciwnym razie klauzula finally jest rozwinięta do:</span><span class="sxs-lookup"><span data-stu-id="404d0-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="404d0-408">Zmienna `d` lokalna nie jest widoczna dla żadnego kodu użytkownika ani nie jest dostępna dla żadnego z nich.</span><span class="sxs-lookup"><span data-stu-id="404d0-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="404d0-409">W szczególności nie powoduje konfliktu z żadną inną zmienną, której zakres zawiera blok finally.</span><span class="sxs-lookup"><span data-stu-id="404d0-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="404d0-410">Kolejność, w której `foreach` przechodzą elementy tablicy, jest następująca: W przypadku elementów tablic jednowymiarowych odbywa się rosnącą kolejnością indeksu, rozpoczynając od `0` indeksu i kończąc na `Length - 1`indeksie.</span><span class="sxs-lookup"><span data-stu-id="404d0-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="404d0-411">W przypadku tablic wielowymiarowych elementy są przenoszone w taki sposób, że indeksy w wymiarze z prawej strony są najpierw zwiększane, następnie następny lewy wymiar itd.</span><span class="sxs-lookup"><span data-stu-id="404d0-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="404d0-412">Poniższy przykład drukuje każdą wartość w dwuwymiarowej tablicy, w kolejności elementów:</span><span class="sxs-lookup"><span data-stu-id="404d0-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="404d0-413">Utworzone dane wyjściowe są następujące:</span><span class="sxs-lookup"><span data-stu-id="404d0-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="404d0-414">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="404d0-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="404d0-415">Typ `n` `int`elementu jest`numbers`wywnioskowany na wartość</span><span class="sxs-lookup"><span data-stu-id="404d0-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="404d0-416">Instrukcje skoku</span><span class="sxs-lookup"><span data-stu-id="404d0-416">Jump statements</span></span>

<span data-ttu-id="404d0-417">Instrukcje skoku bezwarunkowo przetransferować kontrolę.</span><span class="sxs-lookup"><span data-stu-id="404d0-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="404d0-418">Lokalizacja, do której instrukcja skoku przeniesie formant jest nazywana ***elementem docelowym*** instrukcji skoku.</span><span class="sxs-lookup"><span data-stu-id="404d0-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="404d0-419">Gdy instrukcja skoku występuje w bloku, a obiekt docelowy tej instrukcji skoku znajduje się poza tym blokiem, instrukcja skoku jest określana w celu ***opuszczenia*** bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="404d0-420">Chociaż instrukcja skoku może przetransferować kontrolę z bloku, nigdy nie można przesłać kontroli do bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="404d0-421">Wykonywanie instrukcji skoku jest skomplikowane przez obecność `try` instrukcji wykonywanych przez interwencję.</span><span class="sxs-lookup"><span data-stu-id="404d0-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="404d0-422">W przypadku braku takich `try` instrukcji instrukcja skoku bezwarunkowo przesyła kontrolę z instrukcji skoku do jej obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="404d0-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="404d0-423">W obecności takich `try` instrukcji, wykonywanie jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="404d0-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="404d0-424">Jeśli instrukcja skoku `try` kończy jeden lub więcej bloków ze skojarzonymi `finally` blokami, kontrola `finally` jest początkowo przekazywana do bloku wewnętrznej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="404d0-425">Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="404d0-426">Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.</span><span class="sxs-lookup"><span data-stu-id="404d0-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="404d0-427">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="404d0-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="404d0-428">bloki skojarzone z dwiema `try` instrukcjami są wykonywane przed przekazaniem kontroli do elementu docelowego instrukcji skoku. `finally`</span><span class="sxs-lookup"><span data-stu-id="404d0-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="404d0-429">Utworzone dane wyjściowe są następujące:</span><span class="sxs-lookup"><span data-stu-id="404d0-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="404d0-430">Instrukcja break</span><span class="sxs-lookup"><span data-stu-id="404d0-430">The break statement</span></span>

<span data-ttu-id="404d0-431">`while` `switch` `do`Instrukcja kończy najbliższych otaczających, ,`for`,, lub`foreach`instrukcji. `break`</span><span class="sxs-lookup"><span data-stu-id="404d0-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="404d0-432">Obiekt docelowy `break` instrukcji jest punktem końcowym najbliższej `switch`otaczającej, `while`, `do` `for`,, lub `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="404d0-433">`for` `while` `switch` `foreach` Jeśli instrukcja nie jest ujęta w instrukcji,, `do`, lub, wystąpi błąd w czasie kompilacji. `break`</span><span class="sxs-lookup"><span data-stu-id="404d0-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-434">Gdy wiele `switch`instrukcji `while`, `do`, ,`for` lub`foreach` są zagnieżdżone w obrębie siebie, `break` instrukcja ma zastosowanie tylko do najbardziej wewnętrznej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="404d0-435">Aby przenieść kontrolę na wiele poziomów zagnieżdżenia, `goto` należy użyć instrukcji ([instrukcji goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="404d0-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="404d0-436">Instrukcja nie może `finally` opuścić bloku ([Instrukcja try](statements.md#the-try-statement)). `break`</span><span class="sxs-lookup"><span data-stu-id="404d0-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="404d0-437">Gdy instrukcja występuje `finally` w bloku `break` , obiekt docelowy instrukcji musi znajdować się w tym samym `finally` bloku; w przeciwnym razie wystąpi błąd w czasie kompilacji. `break`</span><span class="sxs-lookup"><span data-stu-id="404d0-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-438">`break` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-439">`finally` `try` `finally` Jeśli instrukcja kończy jeden lub więcej `try` bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `break`</span><span class="sxs-lookup"><span data-stu-id="404d0-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="404d0-440">Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="404d0-441">Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.</span><span class="sxs-lookup"><span data-stu-id="404d0-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="404d0-442">Kontrolka jest przenoszona do elementu docelowego `break` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="404d0-443">Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `break` końcowy instrukcji nie jest nigdy osiągalny. `break`</span><span class="sxs-lookup"><span data-stu-id="404d0-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="404d0-444">Instrukcja continue</span><span class="sxs-lookup"><span data-stu-id="404d0-444">The continue statement</span></span>

<span data-ttu-id="404d0-445">`do` `while` `foreach` `for`Instrukcja uruchamia nową iterację najbliższej otaczającej,,, lub instrukcji. `continue`</span><span class="sxs-lookup"><span data-stu-id="404d0-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="404d0-446">Obiekt `continue` docelowy instrukcji jest punktem końcowym osadzonej instrukcji najbliższej `do` `while`otaczającej,, `for`lub `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="404d0-447">`foreach` `for` `do` `while`Jeśli instrukcja nie jest ujęta w instrukcji,,, lub, wystąpi błąd w czasie kompilacji. `continue`</span><span class="sxs-lookup"><span data-stu-id="404d0-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-448">Gdy wiele `while`instrukcji `do`, `for`,, `foreach` lub są zagnieżdżone w obrębie siebie, `continue` instrukcja ma zastosowanie tylko do najbardziej wewnętrznej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="404d0-449">Aby przenieść kontrolę na wiele poziomów zagnieżdżenia, `goto` należy użyć instrukcji ([instrukcji goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="404d0-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="404d0-450">Instrukcja nie może `finally` opuścić bloku ([Instrukcja try](statements.md#the-try-statement)). `continue`</span><span class="sxs-lookup"><span data-stu-id="404d0-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="404d0-451">Gdy instrukcja występuje `finally` w bloku `continue` , obiekt docelowy instrukcji musi znajdować się w tym samym `finally` bloku; w przeciwnym razie wystąpi błąd w czasie kompilacji. `continue`</span><span class="sxs-lookup"><span data-stu-id="404d0-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-452">`continue` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-453">`finally` `try` `finally` Jeśli instrukcja kończy jeden lub więcej `try` bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `continue`</span><span class="sxs-lookup"><span data-stu-id="404d0-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="404d0-454">Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="404d0-455">Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.</span><span class="sxs-lookup"><span data-stu-id="404d0-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="404d0-456">Kontrolka jest przenoszona do elementu docelowego `continue` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="404d0-457">Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `continue` końcowy instrukcji nie jest nigdy osiągalny. `continue`</span><span class="sxs-lookup"><span data-stu-id="404d0-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="404d0-458">Instrukcja goto</span><span class="sxs-lookup"><span data-stu-id="404d0-458">The goto statement</span></span>

<span data-ttu-id="404d0-459">`goto` Instrukcja przekazuje formant do instrukcji, która jest oznaczona etykietą.</span><span class="sxs-lookup"><span data-stu-id="404d0-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="404d0-460">Obiekt docelowy `goto` instrukcji *identyfikatora* jest instrukcją z etykietą z daną etykietą.</span><span class="sxs-lookup"><span data-stu-id="404d0-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="404d0-461">Jeśli etykieta o danej nazwie nie istnieje w elemencie członkowskim bieżącej funkcji lub jeśli `goto` instrukcja nie znajduje się w zakresie etykiety, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="404d0-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="404d0-462">Ta reguła umożliwia użycie `goto` instrukcji w celu przeniesienia kontroli poza zagnieżdżony zakres, ale nie do zakresu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="404d0-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="404d0-463">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="404d0-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="404d0-464">`goto` instrukcja jest używana do transferowania kontroli poza zagnieżdżony zakres.</span><span class="sxs-lookup"><span data-stu-id="404d0-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="404d0-465">Obiektem docelowym `goto case` instrukcji jest lista instrukcji w bezpośrednio `switch` otaczającej instrukcji ( `case` [instrukcji switch](statements.md#the-switch-statement)), która zawiera etykietę o danej wartości stałej.</span><span class="sxs-lookup"><span data-stu-id="404d0-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="404d0-466">`switch` `switch` [](conversions.md#implicit-conversions)Jeśli instrukcja nie jest ujęta w instrukcję, jeśli constant_expression nie jest zamiennie niejawnie (konwersje niejawne) do typu, najbliższej instrukcji otaczającej lub jeśli `goto case` Najbliższa otaczająca `switch` instrukcja nie `case` zawiera etykiety zawierającej daną wartość stałą, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="404d0-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-467">Obiektem docelowym `goto default` instrukcji jest lista instrukcji w bezpośrednio `switch` otaczającej instrukcji ( `default` [instrukcji switch](statements.md#the-switch-statement)), która zawiera etykietę.</span><span class="sxs-lookup"><span data-stu-id="404d0-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="404d0-468">Jeśli instrukcja nie jest ujęta `switch` w instrukcję lub `switch` Jeśli Najbliższa otaczająca instrukcja nie zawiera `default` etykiety, wystąpi błąd w czasie kompilacji. `goto default`</span><span class="sxs-lookup"><span data-stu-id="404d0-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-469">Instrukcja nie może `finally` opuścić bloku ([Instrukcja try](statements.md#the-try-statement)). `goto`</span><span class="sxs-lookup"><span data-stu-id="404d0-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="404d0-470">Gdy instrukcja występuje `finally` w bloku `goto` , obiekt docelowy instrukcji musi znajdować się w tym samym `finally` bloku lub w przeciwnym razie wystąpi błąd w czasie kompilacji. `goto`</span><span class="sxs-lookup"><span data-stu-id="404d0-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-471">`goto` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-472">`finally` `try` `finally` Jeśli instrukcja kończy jeden lub więcej `try` bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `goto`</span><span class="sxs-lookup"><span data-stu-id="404d0-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="404d0-473">Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="404d0-474">Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.</span><span class="sxs-lookup"><span data-stu-id="404d0-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="404d0-475">Kontrolka jest przenoszona do elementu docelowego `goto` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="404d0-476">Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `goto` końcowy instrukcji nie jest nigdy osiągalny. `goto`</span><span class="sxs-lookup"><span data-stu-id="404d0-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="404d0-477">Instrukcja return</span><span class="sxs-lookup"><span data-stu-id="404d0-477">The return statement</span></span>

<span data-ttu-id="404d0-478">Instrukcja zwraca kontrolę do bieżącego obiektu wywołującego funkcji, w `return` której występuje instrukcja. `return`</span><span class="sxs-lookup"><span data-stu-id="404d0-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="404d0-479">`add` `void` `set` [](classes.md#method-body)Instrukcja bez wyrażenia może być używana tylko w składowej funkcji, która nie oblicza wartości, czyli metody z typem wyniku (treść metody), akcesorem właściwości lub indeksatora, `return` metody `remove` dostępu do zdarzenia, Konstruktor wystąpienia, Konstruktor statyczny lub destruktor.</span><span class="sxs-lookup"><span data-stu-id="404d0-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="404d0-480">Instrukcji z wyrażeniem można użyć tylko w składowej funkcji, która oblicza wartość, czyli metodę z typem wyniku innym niż void `get` , akcesorem właściwości lub indeksatorem lub operatorem zdefiniowanym przez użytkownika. `return`</span><span class="sxs-lookup"><span data-stu-id="404d0-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="404d0-481">Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia do zwracanego typu elementu członkowskiego funkcji zawierającego.</span><span class="sxs-lookup"><span data-stu-id="404d0-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="404d0-482">Instrukcji return można także używać w treści wyrażeń funkcji anonimowych ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) i uczestniczyć w ustaleniu, które konwersje istnieją dla tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="404d0-483">Jest to błąd czasu kompilacji dla `return` instrukcji, która ma być wyświetlana `finally` w bloku ([Instrukcja try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="404d0-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="404d0-484">`return` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-485">`return` Jeśli instrukcja określa wyrażenie, wyrażenie jest oceniane i wynikowa wartość jest konwertowana na typ zwracany funkcji zawierającej przez niejawną konwersję.</span><span class="sxs-lookup"><span data-stu-id="404d0-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="404d0-486">Wynik konwersji jest wartością wynikową wygenerowaną przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="404d0-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="404d0-487">`finally` `try` `try` `finally` `catch` Jeśli instrukcja jest ujęta w jeden lub więcej lub bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `return`</span><span class="sxs-lookup"><span data-stu-id="404d0-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="404d0-488">Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="404d0-489">Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji otaczających.</span><span class="sxs-lookup"><span data-stu-id="404d0-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="404d0-490">Jeśli funkcja zawierająca nie jest funkcją asynchroniczną, formant jest zwracany do obiektu wywołującego funkcji zawierającej wraz z wartością wyniku (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="404d0-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="404d0-491">Jeśli funkcja zawierająca jest funkcją asynchroniczną, formant jest zwracany do bieżącego obiektu wywołującego, a wartość wynikowa (jeśli istnieje) jest rejestrowana w zadaniu zwrotnym, zgodnie z opisem w temacie ([moduły wyliczające](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="404d0-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="404d0-492">Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `return` końcowy instrukcji nie jest nigdy osiągalny. `return`</span><span class="sxs-lookup"><span data-stu-id="404d0-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="404d0-493">Instrukcja throw</span><span class="sxs-lookup"><span data-stu-id="404d0-493">The throw statement</span></span>

<span data-ttu-id="404d0-494">`throw` Instrukcja zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="404d0-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="404d0-495">`throw` Instrukcja z wyrażeniem zwraca wartość wygenerowaną przez obliczenie wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="404d0-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="404d0-496">Wyrażenie musi wskazywać wartość typu `System.Exception`klasy w typie klasy, który pochodzi od `System.Exception` lub typu parametru typu, który ma `System.Exception` (lub podklasę) jako obowiązującą klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="404d0-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="404d0-497">Jeśli zostanie wygenerowane obliczenie wyrażenia `null`, w `System.NullReferenceException` zamian zostanie zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="404d0-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="404d0-498">Instrukcja bez wyrażenia może być używana tylko `catch` w bloku, w takim przypadku instrukcja ponownie zgłasza wyjątek, który jest obecnie obsługiwany przez ten `catch` blok. `throw`</span><span class="sxs-lookup"><span data-stu-id="404d0-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="404d0-499">Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `throw` końcowy instrukcji nie jest nigdy osiągalny. `throw`</span><span class="sxs-lookup"><span data-stu-id="404d0-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="404d0-500">Gdy wyjątek jest zgłaszany, kontrola jest przekazywana do pierwszej `catch` klauzuli w `try` otaczającej instrukcji, która może obsłużyć wyjątek.</span><span class="sxs-lookup"><span data-stu-id="404d0-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="404d0-501">Proces, który ma miejsce od momentu, gdy wyjątek zgłaszany do punktu transferu kontroli do odpowiedniej procedury obsługi wyjątków jest znany jako ***Propagacja wyjątku***.</span><span class="sxs-lookup"><span data-stu-id="404d0-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="404d0-502">Propagacja wyjątku polega na wielokrotnym ocenie następujących kroków do momentu `catch` znalezienia klauzuli zgodnej z wyjątkiem.</span><span class="sxs-lookup"><span data-stu-id="404d0-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="404d0-503">W tym opisie ***punkt throw*** jest początkowo lokalizacją, w której został zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="404d0-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="404d0-504">W bieżącym elemencie członkowskim funkcji są badane `try` wszystkie instrukcje, które obejmują punkt throw.</span><span class="sxs-lookup"><span data-stu-id="404d0-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="404d0-505">Dla każdej instrukcji `S`, zaczynając od najbardziej wewnętrznej `try` instrukcji i kończąc na zewnętrznej `try` instrukcji, oceniane są następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="404d0-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="404d0-506">`try` Jeśli `catch` `catch` blok otaczający punkt throw i jeśli S ma jedną klauzulę, klauzule są badane w kolejności występowania, aby znaleźć odpowiednią procedurę obsługi dla wyjątku, zgodnie z regułami określonymi w `S` Sekcja [instrukcji try](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="404d0-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="404d0-507">Jeśli znajduje się `catch` klauzula dopasowywania, Propagacja wyjątku jest wykonywana przez przeniesienie kontroli do bloku tej `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="404d0-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="404d0-508">W przeciwnym razie, `try` Jeśli blok `catch` lub blok `S` otaczający punkt rzutowania i jeśli `S` ma `finally` blok, sterowanie jest przekazywane do `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="404d0-509">`finally` Jeśli blok zgłasza inny wyjątek, przetwarzanie bieżącego wyjątku zostanie zakończone.</span><span class="sxs-lookup"><span data-stu-id="404d0-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="404d0-510">W przeciwnym razie, gdy formant osiągnie punkt `finally` końcowy bloku, przetwarzanie bieżącego wyjątku jest kontynuowane.</span><span class="sxs-lookup"><span data-stu-id="404d0-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="404d0-511">Jeśli program obsługi wyjątków nie znajduje się w bieżącym wywołaniu funkcji, wywołanie funkcji jest przerywane i jedna z następujących sytuacji:</span><span class="sxs-lookup"><span data-stu-id="404d0-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="404d0-512">Jeśli bieżąca funkcja nie jest asynchroniczna, powyższe kroki są powtórzone dla obiektu wywołującego funkcji z punktem rzutowania odpowiadającym instrukcji, z której wywołano element członkowski funkcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="404d0-513">Jeśli bieżąca funkcja jest asynchroniczna i zwracająca zadania, wyjątek jest rejestrowany w zadaniu zwrotnym, które jest umieszczane w stanie awarii lub anulowania zgodnie z opisem w temacie [interfejsy modułu wyliczającego](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="404d0-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="404d0-514">Jeśli bieżąca funkcja ma wartość Async i zwraca wartość void, kontekst synchronizacji bieżącego wątku zostanie powiadomiony zgodnie z opisem w [wyliczalnych interfejsach](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="404d0-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="404d0-515">Jeśli przetwarzanie wyjątku kończy wszystkie wywołania elementu członkowskiego funkcji w bieżącym wątku, wskazując, że wątek nie ma obsługi dla wyjątku, wątek jest zakończony.</span><span class="sxs-lookup"><span data-stu-id="404d0-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="404d0-516">Wpływ tego zakończenia jest zdefiniowany przez implementację.</span><span class="sxs-lookup"><span data-stu-id="404d0-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="404d0-517">Instrukcja try</span><span class="sxs-lookup"><span data-stu-id="404d0-517">The try statement</span></span>

<span data-ttu-id="404d0-518">`try` Instrukcja zawiera mechanizm przechwytywania wyjątków, które występują podczas wykonywania bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="404d0-519">Ponadto instrukcja umożliwia określenie bloku kodu, który jest zawsze wykonywany, gdy kontrolka `try` opuszcza instrukcję. `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="404d0-520">Istnieją trzy możliwe formy `try` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="404d0-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="404d0-521">Blok, po którym następuje co najmniej `catch` jeden blok. `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="404d0-522">Blok, po którym następuje `finally` blok. `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="404d0-523">Blok, po którym następuje jeden lub `catch` więcej bloków, po `finally` którym następuje blok. `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="404d0-524">`System.Exception` `System.Exception` `System.Exception`Gdy klauzula określa exception_specifier, typ musi być typem, który pochodzi z lub typu parametru typu, który ma (lub podklasę tej klasy) jako obowiązującą klasę bazową. `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="404d0-525">Gdy klauzula określa zarówno *exception_specifier* z *identyfikatorem*, ***zmienna wyjątku*** o podanej nazwie i typie jest zadeklarowana. `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="404d0-526">Zmienna wyjątku odpowiada zmiennej lokalnej z zakresem, który wykracza `catch` poza klauzulę.</span><span class="sxs-lookup"><span data-stu-id="404d0-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="404d0-527">Podczas wykonywania *exception_filter* i *bloku*zmienna wyjątku reprezentuje aktualnie obsługiwany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="404d0-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="404d0-528">W celach o określonym zapisywaniu zmienna wyjątku jest uznawana za ostatecznie przypisaną w całym zakresie.</span><span class="sxs-lookup"><span data-stu-id="404d0-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="404d0-529">O ile `catch` klauzula nie zawiera nazwy zmiennej wyjątku, nie można uzyskać dostępu do obiektu Exception w filtrze i bloku. `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="404d0-530">Klauzula, która nie określa elementu *exception_specifier* , jest nazywana klauzulą generalną `catch`. `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="404d0-531">Niektóre języki programowania mogą obsługiwać wyjątki, które nie są reprezentowane jako obiekt pochodny `System.Exception`, chociaż takie wyjątki nigdy nie mogą być generowane C# przez kod.</span><span class="sxs-lookup"><span data-stu-id="404d0-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="404d0-532">Klauzula General `catch` może służyć do przechwytywania takich wyjątków.</span><span class="sxs-lookup"><span data-stu-id="404d0-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="404d0-533">W rezultacie Klauzula ogólna `catch` jest semantycznie różna od jednej, która określa typ `System.Exception`, w tym, że dawny może również przechwytywać wyjątki z innych języków.</span><span class="sxs-lookup"><span data-stu-id="404d0-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="404d0-534">Aby zlokalizować procedurę obsługi dla wyjątku, `catch` klauzule są badane w kolejności leksykalnej.</span><span class="sxs-lookup"><span data-stu-id="404d0-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="404d0-535">Jeśli klauzula określa typ, ale nie filtr wyjątku, jest to błąd czasu kompilacji dla późniejszej `catch` klauzuli w tej samej `try` instrukcji, aby określić typ, który jest taki sam jak lub pochodzi od typu. `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="404d0-536">Jeśli klauzula nie określa typu ani filtru, musi być ostatnią `catch` klauzulą tej `try` instrukcji. `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="404d0-537">`catch` W bloku instrukcja`throw` ([Instrukcja throw](statements.md#the-throw-statement)) bez wyrażenia może służyć do ponownego zgłoszenia wyjątku, który został przechwycony przez blok. `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="404d0-538">Przypisania do zmiennej wyjątku nie powodują zmiany zgłoszonego wyjątku.</span><span class="sxs-lookup"><span data-stu-id="404d0-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="404d0-539">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="404d0-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="404d0-540">Metoda `F` przechwytuje wyjątek, zapisuje niektóre informacje diagnostyczne do konsoli, modyfikuje zmienną wyjątku i ponowne zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="404d0-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="404d0-541">Wyjątek, który jest ponownie zgłaszany, to oryginalny wyjątek, dlatego utworzony wynik to:</span><span class="sxs-lookup"><span data-stu-id="404d0-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="404d0-542">Jeśli pierwszy blok catch został zgłoszony `e` zamiast ponownego zgłoszenia bieżącego wyjątku, utworzone dane wyjściowe byłyby następujące:</span><span class="sxs-lookup"><span data-stu-id="404d0-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="404d0-543">Jest to błąd `break`czasu kompilacji dla instrukcji, `continue`lub `goto` , aby przenieść kontrolę z `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="404d0-544">`continue` `goto` Gdy instrukcja `finally` ,, lub występuje w bloku,obiektdocelowyinstrukcjimusiznajdowaćsięwtymsamymblokulubwprzeciwnymraziewystąpibłądwczasiekompilacji.`finally` `break`</span><span class="sxs-lookup"><span data-stu-id="404d0-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="404d0-545">Jest to błąd czasu kompilacji dla `return` instrukcji, która ma być wykonywana `finally` w bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="404d0-546">`try` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-547">Kontrolka jest przenoszona `try` do bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="404d0-548">Gdy i jeśli kontrola osiągnie punkt `try` końcowy bloku:</span><span class="sxs-lookup"><span data-stu-id="404d0-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="404d0-549">Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="404d0-550">Kontrolka jest przenoszona do punktu `try` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="404d0-551">Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `try` bloku:</span><span class="sxs-lookup"><span data-stu-id="404d0-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="404d0-552">`catch` Klauzule, jeśli istnieją, są badane w kolejności występowania, aby znaleźć odpowiednią procedurę obsługi dla wyjątku.</span><span class="sxs-lookup"><span data-stu-id="404d0-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="404d0-553">`catch` Jeśli klauzula nie określa typu lub określa typ wyjątku lub typ podstawowy typu wyjątku:</span><span class="sxs-lookup"><span data-stu-id="404d0-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="404d0-554">`catch` Jeśli klauzula deklaruje zmienną wyjątku, obiekt wyjątku jest przypisywany do zmiennej wyjątku.</span><span class="sxs-lookup"><span data-stu-id="404d0-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="404d0-555">`catch` Jeśli klauzula deklaruje filtr wyjątku, filtr jest obliczany.</span><span class="sxs-lookup"><span data-stu-id="404d0-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="404d0-556">Jeśli wartość jest równa `false`, klauzula catch nie jest zgodna i wyszukiwanie kontynuuje się przez wszelkie kolejne `catch` klauzule dla odpowiedniej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="404d0-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="404d0-557">W przeciwnym `catch` razie klauzulajestuważanazadopasowanie,akontrolajestprzekazywanado`catch` zgodnego bloku.</span><span class="sxs-lookup"><span data-stu-id="404d0-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="404d0-558">Gdy i jeśli kontrola osiągnie punkt `catch` końcowy bloku:</span><span class="sxs-lookup"><span data-stu-id="404d0-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="404d0-559">Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="404d0-560">Kontrolka jest przenoszona do punktu `try` końcowego instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="404d0-561">Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="404d0-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="404d0-562">Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="404d0-563">Wyjątek jest propagowany do następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="404d0-564">Jeśli instrukcja nie `catch` ma klauzul lub jeśli żadna klauzula `catch` nie pasuje do wyjątku: `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="404d0-565">Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="404d0-566">Wyjątek jest propagowany do następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="404d0-567">Instrukcje `finally` bloku są zawsze wykonywane, gdy kontrolka `try` opuszcza instrukcję.</span><span class="sxs-lookup"><span data-stu-id="404d0-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="404d0-568">Jest to prawdą, czy transfer kontroli odbywa się w wyniku normalnego wykonywania, w `break`wyniku wykonywania `continue`instrukcji,, `goto`, lub `return` `try` w wyniku propagowania wyjątku z Merge.</span><span class="sxs-lookup"><span data-stu-id="404d0-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="404d0-569">Jeśli wyjątek jest zgłaszany podczas wykonywania `finally` bloku i nie jest przechwytywany w obrębie tego samego bloku finally, wyjątek jest propagowany do następnej `try` otaczającej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="404d0-570">Jeśli inny wyjątek był w trakcie propagowania, ten wyjątek zostanie utracony.</span><span class="sxs-lookup"><span data-stu-id="404d0-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="404d0-571">Proces propagowania wyjątku jest dokładniej opisany w opisie `throw` instrukcji ([Instrukcja throw](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="404d0-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="404d0-572">Blok instrukcji jest osiągalny, `try` Jeśli instrukcja jest osiągalna. `try` `try`</span><span class="sxs-lookup"><span data-stu-id="404d0-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="404d0-573">Blok instrukcji jest osiągalny, `try` Jeśli instrukcja jest osiągalna. `try` `catch`</span><span class="sxs-lookup"><span data-stu-id="404d0-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="404d0-574">Blok instrukcji jest osiągalny, `try` Jeśli instrukcja jest osiągalna. `try` `finally`</span><span class="sxs-lookup"><span data-stu-id="404d0-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="404d0-575">Punkt `try` końcowy instrukcji jest osiągalny, jeśli są spełnione oba poniższe warunki:</span><span class="sxs-lookup"><span data-stu-id="404d0-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="404d0-576">Punkt `try` końcowy bloku jest osiągalny lub punkt końcowy co najmniej jednego `catch` bloku jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="404d0-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="404d0-577">Jeśli znajduje się `finally` blok, punkt końcowy bloku jest osiągalny. `finally`</span><span class="sxs-lookup"><span data-stu-id="404d0-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="404d0-578">Sprawdzone i niesprawdzone instrukcje</span><span class="sxs-lookup"><span data-stu-id="404d0-578">The checked and unchecked statements</span></span>

<span data-ttu-id="404d0-579">Instrukcje `checked` and`unchecked` są używane do kontrolowania ***kontekstu sprawdzania przepełnienia*** dla operacji arytmetycznych typu całkowitego i konwersji.</span><span class="sxs-lookup"><span data-stu-id="404d0-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="404d0-580">Instrukcja powoduje, że wszystkie wyrażenia w *bloku* są oceniane w kontekście sprawdzonym, a instrukcja powoduje `unchecked` , że wszystkie wyrażenia w bloku są oceniane w niesprawdzonym kontekście. `checked`</span><span class="sxs-lookup"><span data-stu-id="404d0-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="404d0-581">`unchecked` Instrukcje `checked` and`unchecked` [są precyzyjnie](expressions.md#the-checked-and-unchecked-operators)równoważne operatorom and (operatory sprawdzone i niezaznaczone), z tą różnicą, że działają na blokach zamiast wyrażeń. `checked`</span><span class="sxs-lookup"><span data-stu-id="404d0-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="404d0-582">Instrukcja lock</span><span class="sxs-lookup"><span data-stu-id="404d0-582">The lock statement</span></span>

<span data-ttu-id="404d0-583">`lock` Instrukcja uzyskuje blokadę wykluczania wzajemnego dla danego obiektu, wykonuje instrukcję, a następnie zwalnia blokadę.</span><span class="sxs-lookup"><span data-stu-id="404d0-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="404d0-584">Wyrażenie `lock` instrukcji musi wskazywać wartość typu znanego jako *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="404d0-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="404d0-585">Żadna niejawna konwersja opakowania ([konwersje](conversions.md#boxing-conversions)napakowywania) nie jest kiedykolwiek wykonywana dla `lock` wyrażenia instrukcji, w rezultacie jest to błąd czasu kompilacji dla wyrażenia, aby zauważyć wartość *value_type*.</span><span class="sxs-lookup"><span data-stu-id="404d0-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="404d0-586">`lock` Instrukcja formularza</span><span class="sxs-lookup"><span data-stu-id="404d0-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="404d0-587">gdzie `x` jest wyrażeniem *reference_type*, jest dokładnie równoważne</span><span class="sxs-lookup"><span data-stu-id="404d0-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="404d0-588">z tą różnicą, że `x` jest obliczany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="404d0-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="404d0-589">Gdy blokada wzajemnego wykluczania jest utrzymywana, kod wykonywany w tym samym wątku wykonywania może również uzyskać i zwolnić blokadę.</span><span class="sxs-lookup"><span data-stu-id="404d0-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="404d0-590">Jednak kod wykonywany w innych wątkach ma zablokowany dostęp do blokady do momentu zwolnienia blokady.</span><span class="sxs-lookup"><span data-stu-id="404d0-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="404d0-591">Blokowanie `System.Type` obiektów w celu synchronizacji dostępu do danych statycznych nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="404d0-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="404d0-592">Inny kod może być zablokowany dla tego samego typu, co może spowodować zakleszczenie.</span><span class="sxs-lookup"><span data-stu-id="404d0-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="404d0-593">Lepszym rozwiązaniem jest synchronizowanie dostępu do danych statycznych przez zablokowanie prywatnego obiektu statycznego.</span><span class="sxs-lookup"><span data-stu-id="404d0-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="404d0-594">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="404d0-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="404d0-595">Instrukcja using</span><span class="sxs-lookup"><span data-stu-id="404d0-595">The using statement</span></span>

<span data-ttu-id="404d0-596">`using` Instrukcja uzyskuje jeden lub więcej zasobów, wykonuje instrukcję, a następnie usuwa zasób.</span><span class="sxs-lookup"><span data-stu-id="404d0-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="404d0-597">***Zasób*** jest klasą lub strukturą, która `System.IDisposable`implementuje, która zawiera pojedynczą metodę bez parametrów `Dispose`o nazwie.</span><span class="sxs-lookup"><span data-stu-id="404d0-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="404d0-598">Kod używający zasobu może wywoływać `Dispose` , aby wskazać, że zasób nie jest już wymagany.</span><span class="sxs-lookup"><span data-stu-id="404d0-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="404d0-599">Jeśli `Dispose` nie jest wywoływana, automatyczne usuwanie następuje ostatecznie w wyniku wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="404d0-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="404d0-600">Jeśli forma *resource_acquisition* jest *local_variable_declaration* , typem *local_variable_declaration* musi być albo `dynamic` lub typu, który może być niejawnie konwertowany na. `System.IDisposable`</span><span class="sxs-lookup"><span data-stu-id="404d0-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="404d0-601">Jeśli forma *resource_acquisition* jest *wyrażeniem* , to wyrażenie musi być niejawnie konwertowane na `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="404d0-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="404d0-602">Zmienne lokalne zadeklarowane w *resource_acquisition* są tylko do odczytu i muszą zawierać inicjator.</span><span class="sxs-lookup"><span data-stu-id="404d0-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="404d0-603">Błąd czasu kompilacji występuje, `++` Jeśli osadzona instrukcja próbuje zmodyfikować te zmienne lokalne (poprzez przypisanie lub operatory i `--` ), pobrać ich adresy lub przekazać je jako `ref` lub `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="404d0-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="404d0-604">`using` Instrukcja jest tłumaczona na trzy części: pozyskiwanie, użycie i usuwanie.</span><span class="sxs-lookup"><span data-stu-id="404d0-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="404d0-605">Użycie zasobu jest niejawnie ujęte w `try` instrukcji, która `finally` zawiera klauzulę.</span><span class="sxs-lookup"><span data-stu-id="404d0-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="404d0-606">Ta `finally` klauzula usuwa zasób.</span><span class="sxs-lookup"><span data-stu-id="404d0-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="404d0-607">Jeśli zasób zostanie uzyskany, żadne `Dispose` wywołanie nie zostanie wykonane i żaden wyjątek nie jest zgłaszany. `null`</span><span class="sxs-lookup"><span data-stu-id="404d0-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="404d0-608">Jeśli zasób jest typu `dynamic` , jest dynamicznie konwertowany przy użyciu niejawnej konwersji dynamicznej ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions)) do `IDisposable` podczas pozyskiwania w celu zapewnienia, że konwersja zakończy się pomyślnie przed użyciem i myśl.</span><span class="sxs-lookup"><span data-stu-id="404d0-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="404d0-609">`using` Instrukcja formularza</span><span class="sxs-lookup"><span data-stu-id="404d0-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="404d0-610">odpowiada jednemu z trzech możliwych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="404d0-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="404d0-611">Gdy `ResourceType` jest typem wartości niedopuszczających wartości null, rozszerzenie jest</span><span class="sxs-lookup"><span data-stu-id="404d0-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="404d0-612">W przeciwnym razie `ResourceType` , gdy jest typem wartości null lub typem odwołania innym niż `dynamic`, rozszerzenie jest</span><span class="sxs-lookup"><span data-stu-id="404d0-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="404d0-613">`ResourceType` W`dynamic`przeciwnym razie rozszerzenie jest</span><span class="sxs-lookup"><span data-stu-id="404d0-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="404d0-614">W obu rozszerzeniach `resource` zmienna jest tylko do odczytu w osadzonej instrukcji, `d` a zmienna jest niedostępna w, i niewidoczna dla, osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="404d0-615">Implementacja może implementować daną instrukcję using inaczej, np. ze względu na wydajność, o ile zachowanie jest spójne z powyższym rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="404d0-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="404d0-616">`using` Instrukcja formularza</span><span class="sxs-lookup"><span data-stu-id="404d0-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="404d0-617">ma te same trzy możliwe rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="404d0-617">has the same three possible expansions.</span></span> <span data-ttu-id="404d0-618">W tym przypadku `ResourceType` jest niejawnie typem `expression`czasu kompilacji, jeśli ma.</span><span class="sxs-lookup"><span data-stu-id="404d0-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="404d0-619">W przeciwnym razie `IDisposable` sam interfejs jest używany `ResourceType`jako.</span><span class="sxs-lookup"><span data-stu-id="404d0-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="404d0-620">`resource` Zmienna jest niedostępna w, i niewidoczna dla osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="404d0-621">Gdy *resource_acquisition* przyjmuje postać *local_variable_declaration*, możliwe jest uzyskanie wielu zasobów danego typu.</span><span class="sxs-lookup"><span data-stu-id="404d0-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="404d0-622">`using` Instrukcja formularza</span><span class="sxs-lookup"><span data-stu-id="404d0-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="404d0-623">jest precyzyjnym odpowiednikiem sekwencji zagnieżdżonych `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="404d0-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="404d0-624">Poniższy przykład tworzy plik o nazwie `log.txt` i zapisuje dwa wiersze tekstu do pliku.</span><span class="sxs-lookup"><span data-stu-id="404d0-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="404d0-625">W tym przykładzie zostanie otwarty ten sam plik do odczytu i kopiowania zawartych wierszy tekstu do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="404d0-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="404d0-626">Ponieważ klasy `TextReader` `IDisposable` i implementują interfejs, przykład może używać `using` instrukcji, aby upewnić się, że plik źródłowy jest prawidłowo zamknięty po operacji zapisu lub odczytu. `TextWriter`</span><span class="sxs-lookup"><span data-stu-id="404d0-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="404d0-627">Instrukcja Yield</span><span class="sxs-lookup"><span data-stu-id="404d0-627">The yield statement</span></span>

<span data-ttu-id="404d0-628">Instrukcja jest używana w bloku iteratora ([bloki](statements.md#blocks)) do przesyłania wartości do obiektu modułu wyliczającego ([obiektów modułu wyliczającego](classes.md#enumerator-objects)) lub wyliczalnego obiektu ([obiektów wyliczalnych](classes.md#enumerable-objects)) iteratora lub do sygnalizowania końcem iteracji. `yield`</span><span class="sxs-lookup"><span data-stu-id="404d0-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="404d0-629">`yield`nie jest słowem zastrzeżonym; ma specjalne znaczenie tylko wtedy, gdy jest używana bezpośrednio `return` przed `break` słowem kluczowym or.</span><span class="sxs-lookup"><span data-stu-id="404d0-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="404d0-630">W innych kontekstach `yield` można użyć jako identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="404d0-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="404d0-631">Istnieje kilka ograniczeń w miejscu, w `yield` którym instrukcja może zostać wyświetlona, zgodnie z opisem w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="404d0-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="404d0-632">Jest to `yield` błąd czasu kompilacji dla instrukcji (jednej z formularzy), która ma być wyświetlana poza *method_body*, *operator_body* lub *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="404d0-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="404d0-633">Jest to błąd czasu kompilacji dla `yield` instrukcji (każdej z formularzy), która ma być wyświetlana wewnątrz funkcji anonimowej.</span><span class="sxs-lookup"><span data-stu-id="404d0-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="404d0-634">Jest to błąd czasu kompilacji dla `yield` instrukcji (każdej z formularzy) do wyświetlenia `finally` w klauzuli `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="404d0-635">Jest to błąd czasu kompilacji dla `yield return` instrukcji pojawia się gdziekolwiek `try` w instrukcji, która zawiera `catch` klauzule.</span><span class="sxs-lookup"><span data-stu-id="404d0-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="404d0-636">W poniższym przykładzie przedstawiono nieprawidłowe i nieprawidłowe zastosowania `yield` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="404d0-637">Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia w `yield return` instrukcji do typu yield ([Typ Yield](classes.md#yield-type)) iteratora.</span><span class="sxs-lookup"><span data-stu-id="404d0-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="404d0-638">`yield return` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-639">Wyrażenie określone w instrukcji jest oceniane, niejawnie konwertowane na typ Yield i przypisywane do `Current` właściwości obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="404d0-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="404d0-640">Wykonywanie bloku iteratora zostało wstrzymane.</span><span class="sxs-lookup"><span data-stu-id="404d0-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="404d0-641">Jeśli instrukcja znajduje się w jednym lub większej `try` liczbie bloków, `finally` skojarzone bloki nie są wykonywane w tym momencie. `yield return`</span><span class="sxs-lookup"><span data-stu-id="404d0-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="404d0-642">Metoda obiektu Enumerator powraca `true` do jego obiektu wywołującego, wskazując, że obiekt Enumerator został pomyślnie zaawansowany do następnego elementu. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="404d0-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="404d0-643">Następne wywołanie `MoveNext` metody obiektu modułu wyliczającego wznawia wykonywanie bloku iteratora od momentu ostatniego wstrzymania.</span><span class="sxs-lookup"><span data-stu-id="404d0-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="404d0-644">`yield break` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="404d0-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="404d0-645">`try` `finally` `finally` `try` Jeśli instrukcja jest ujęta w jeden lub więcej bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `yield break`</span><span class="sxs-lookup"><span data-stu-id="404d0-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="404d0-646">Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="404d0-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="404d0-647">Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji otaczających.</span><span class="sxs-lookup"><span data-stu-id="404d0-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="404d0-648">Formant jest zwracany do obiektu wywołującego bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="404d0-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="404d0-649">Jest `MoveNext` to metoda lub `Dispose` metoda obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="404d0-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="404d0-650">Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `yield break` końcowy instrukcji nie jest nigdy osiągalny. `yield break`</span><span class="sxs-lookup"><span data-stu-id="404d0-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
