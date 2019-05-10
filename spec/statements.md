---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488813"
---
# <a name="statements"></a><span data-ttu-id="fb0dd-101">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="fb0dd-101">Statements</span></span>

<span data-ttu-id="fb0dd-102">C# zawiera szereg instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-102">C# provides a variety of statements.</span></span> <span data-ttu-id="fb0dd-103">Większość z tych instrukcji, nie będą niczym nowym dla deweloperów, którzy mają programowane w językach C i C++.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="fb0dd-104">*Embedded_statement* nieterminalnych służy do instrukcji, które są wyświetlane w inne instrukcje.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="fb0dd-105">Korzystanie z *embedded_statement* zamiast *instrukcji* nie obejmuje korzystanie z instrukcje deklaracji i labeled — instrukcje w tych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="fb0dd-106">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb0dd-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="fb0dd-107">powoduje błąd w czasie kompilacji, ponieważ `if` instrukcja wymaga *embedded_statement* zamiast *instrukcji* jego IF gałęzi.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="fb0dd-108">Jeśli ten kod, były dozwolone, następnie zmienna `i` może być zadeklarowany, ale nigdy nie może być używana.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="fb0dd-109">Jednak, umieszczając `i`w deklaracji w bloku, przykład jest nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="fb0dd-110">Punkty końcowe i osiągalności</span><span class="sxs-lookup"><span data-stu-id="fb0dd-110">End points and reachability</span></span>

<span data-ttu-id="fb0dd-111">Ma każdej instrukcji ***punktu końcowego***.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="fb0dd-112">W sposób intuicyjny punkt końcowy w instrukcji jest to lokalizacja, który następuje bezpośrednio po instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="fb0dd-113">Zasady wykonywania instrukcje złożone (instrukcji, które zawierają osadzonych instrukcji) określ akcję, która jest wykonywana, gdy kontrola osiąga punkt końcowy osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="fb0dd-114">Na przykład gdy kontrola osiąga punkt końcowy w instrukcji w bloku, formant jest przekazywany do następnej instrukcji w bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="fb0dd-115">Jeśli oświadczenie prawdopodobnie można osiągnąć przez wykonanie, instrukcja jest nazywany ***osiągalny***.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="fb0dd-116">Z drugiej strony, w przypadku braku możliwości instrukcja zostanie wykonana instrukcja jest nazywany ***nieosiągalny***.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="fb0dd-117">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="fb0dd-118">drugie wywołanie `Console.WriteLine` jest nieosiągalny, ponieważ nie ma możliwości zostanie wykonana instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="fb0dd-119">Ostrzeżenie jest zgłaszane w przypadku kompilator Określa, że instrukcja jest nieosiągalna.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="fb0dd-120">Jest specjalnie nie błąd dla instrukcji jest nieosiągalne.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="fb0dd-121">Aby ustalić, czy określonej instrukcji lub punkt końcowy jest osiągalny, kompilator wykonuje analizę przepływu zgodnie z regułami osiągalności zdefiniowane dla każdej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="fb0dd-122">Analizę przepływu bierze pod uwagę wartości wyrażeń stałych ([wyrażeń stałych](expressions.md#constant-expressions)) które kontrolują zachowanie instrukcji, ale możliwe wartości inne niż stałe wyrażenia nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="fb0dd-123">Innymi słowy do celów analizy przepływu sterowania, niestałe wyrażenie danego typu została uznana za wszystkie możliwe wartości tego typu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="fb0dd-124">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="fb0dd-125">wyrażenie logiczne z `if` instrukcja jest wyrażeniem stałym, ponieważ oba operandy z `==` operator są stałymi.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="fb0dd-126">Jak stałe wyrażenie jest obliczane w czasie kompilacji, tworzenie wartość `false`, `Console.WriteLine` wywołanie jest traktowane jako niedostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="fb0dd-127">Jednak jeśli `i` zostanie zmieniony jako zmienna lokalna</span><span class="sxs-lookup"><span data-stu-id="fb0dd-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="fb0dd-128">`Console.WriteLine` wywołanie jest uważany za dostępny, mimo że w rzeczywistości zostanie nigdy nie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="fb0dd-129">*Bloku* funkcji składowej jest zawsze uważane za dostępne.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="fb0dd-130">Kolejno oceny zasad osiągalności każdej instrukcji w bloku, można określić wysyłające dowolnej podanej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="fb0dd-131">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="fb0dd-132">osiągalności drugiego `Console.WriteLine` jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="fb0dd-133">Pierwszy `Console.WriteLine` Instrukcja wyrażenia jest osiągalny ponieważ blok `F` metody jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="fb0dd-134">Punkt końcowy pierwszego `Console.WriteLine` Instrukcja wyrażenia jest dostępny, ponieważ w tej instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="fb0dd-135">`if` Instrukcji jest dostępny, ponieważ koniec wskazuje pierwszego `Console.WriteLine` Instrukcja wyrażenia jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="fb0dd-136">Drugi `Console.WriteLine` Instrukcja wyrażenia jest osiągalny ponieważ wyrażenie logiczne z `if` instrukcji nie ma wartości stałej `false`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="fb0dd-137">Istnieją dwie sytuacje, w których jest to błąd czasu kompilacji dla punktu końcowego instrukcji być dostępny:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="fb0dd-138">Ponieważ `switch` instrukcji nie zezwala na sekcji przełącznika, aby "przechodzić" do następnej sekcji przełącznika, jest to błąd czasu kompilacji dla punktu końcowego listy instrukcji w sekcji przełącznika, być dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="fb0dd-139">Jeśli ten błąd występuje, jest zazwyczaj wskazuje który `break` brakuje instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="fb0dd-140">Jest to błąd czasu kompilacji dla punktu końcowego w bloku funkcja elementu członkowskiego, który oblicza wartość być dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="fb0dd-141">Jeśli ten błąd występuje, zazwyczaj jest wskazanie, `return` brakuje instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="fb0dd-142">Bloki</span><span class="sxs-lookup"><span data-stu-id="fb0dd-142">Blocks</span></span>

<span data-ttu-id="fb0dd-143">A *bloku* zezwala na wiele instrukcji, które ma zostać zapisany w kontekstach, których jest dozwolone pojedynczej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="fb0dd-144">A *bloku* składa się z opcjonalną *statement_list* ([Wyświetla listę instrukcji](statements.md#statement-lists)), ujęty w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="fb0dd-145">W przypadku pominięcia listy instrukcji bloku jest określany jako pusta.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="fb0dd-146">Blok może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="fb0dd-147">Zakres zmiennej lokalnej zmiennej czy stałej zadeklarowane w bloku jest bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="fb0dd-148">Blok jest wykonywany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-149">Jeśli blok jest pusta, kontrola jest przekazywana do punktu końcowego bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="fb0dd-150">Jeśli blok nie jest pusty, kontrola jest przekazywana do listy instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="fb0dd-151">Gdy i kontrola osiąga punkt końcowy listy instrukcji, kontrola jest przekazywana do punktu końcowego bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="fb0dd-152">Listy instrukcji w bloku jest osiągalna, jeśli blok, sama jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="fb0dd-153">Punkt końcowy w bloku jest osiągalny, jeśli blok jest puste lub jeśli punkt końcowy listy instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="fb0dd-154">A *bloku* zawierający co najmniej jeden `yield` instrukcji ([instrukcji yield](statements.md#the-yield-statement)) nazywa się blokiem iteratora.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="fb0dd-155">Iterator bloki są używane do implementowania funkcji członków jako Iteratory ([Iteratory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="fb0dd-156">Niektóre dodatkowe ograniczenia mają zastosowanie do iteratora bloków:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="fb0dd-157">Jest to błąd czasu kompilacji dla `return` instrukcję, aby są wyświetlane w bloku iteratora (ale `yield return` są dozwolone).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="fb0dd-158">Jest to błąd czasu kompilacji dla blokiem iteratora, tak zawierała niebezpiecznym kontekście ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="fb0dd-159">Blokiem iteratora zawsze definiuje bezpieczne kontekstu, nawet wtedy, gdy jego deklaracji jest zagnieżdżony w niebezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="fb0dd-160">List — instrukcja</span><span class="sxs-lookup"><span data-stu-id="fb0dd-160">Statement lists</span></span>

<span data-ttu-id="fb0dd-161">A ***listy instrukcji*** składa się z jedną lub więcej instrukcji, zapisane w sekwencji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="fb0dd-162">Instrukcja listy występują w *bloku*s ([bloki](statements.md#blocks)) i w *switch_block*s ([instrukcji switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="fb0dd-163">Listy instrukcji jest wykonywana przez transferowanie formantu do pierwszej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="fb0dd-164">Gdy i kontrola osiąga punkt końcowy w instrukcji, kontrola jest przekazywana do następnej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="fb0dd-165">Gdy i kontrola osiąga punkt końcowy ostatnią instrukcję, kontrola jest przekazywana do punktu końcowego listy instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="fb0dd-166">Instrukcja z listy instrukcji jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="fb0dd-167">Instrukcja jest pierwszej instrukcji i samej listy instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="fb0dd-168">Poprzednia instrukcja punkt końcowy jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="fb0dd-169">Instrukcja to instrukcja labeled i etykieta odwołuje się osiągalny `goto` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="fb0dd-170">Punkt końcowy listy instrukcji jest osiągalny, jeśli punkt końcowy ostatnią instrukcję, na liście jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="fb0dd-171">Pusta instrukcja</span><span class="sxs-lookup"><span data-stu-id="fb0dd-171">The empty statement</span></span>

<span data-ttu-id="fb0dd-172">*Empty_statement* nic nie robi.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="fb0dd-173">Pusta instrukcja jest używane, gdy istnieją żadne operacje do wykonania w kontekście, w których wymagane jest użycie instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="fb0dd-174">Wykonanie pustą instrukcję po prostu przekazuje sterowanie do instrukcji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="fb0dd-175">W związku z tym punkt końcowy pusta instrukcja jest osiągalna, jeśli pusta instrukcja jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="fb0dd-176">Pusta instrukcja może służyć podczas zapisywania `while` instrukcja o wartości null treści:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="fb0dd-177">Ponadto pusta instrukcja może służyć do deklarowania etykietę tuż przed zamykającym "`}`" bloku:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="fb0dd-178">Labeled — instrukcje</span><span class="sxs-lookup"><span data-stu-id="fb0dd-178">Labeled statements</span></span>

<span data-ttu-id="fb0dd-179">A *labeled_statement* zezwala na instrukcję, aby być poprzedzona przez etykietę.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="fb0dd-180">Labeled — instrukcje są dozwolone w blokach, ale nie są dozwolone jako osadzonych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="fb0dd-181">Instrukcja labeled deklaruje etykietę o nazwie podanej przez *identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="fb0dd-182">Zakres etykiety jest cały blok, w którym etykiety zadeklarowano, wraz ze wszystkimi zagnieżdżonych bloków.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="fb0dd-183">Jest to błąd czasu kompilacji dla dwóch etykiet o takiej samej nazwie, aby mieć nakładających się zakresów.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="fb0dd-184">Etykiety mogą być przywoływane z `goto` instrukcji ([instrukcji goto](statements.md#the-goto-statement)) w zakresie etykiety.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="fb0dd-185">Oznacza to, że `goto` instrukcji można przekazać sterowanie wewnątrz bloków i poza bloków, ale nigdy nie na bloki.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="fb0dd-186">Etykiety mają własnej przestrzeni deklaracji i nie kolidują z innymi identyfikatorami.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="fb0dd-187">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb0dd-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="fb0dd-188">jest prawidłowy i używa nazwy `x` jako parametr i etykietę.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="fb0dd-189">Wykonanie instrukcji oznaczonej etykietą dokładnie odpowiada wykonywania instrukcji następującej etykiety.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="fb0dd-190">Oprócz osiągalności dostarczone przez Normalny przepływ sterowania, jest osiągalna, jeśli etykieta odwołuje się osiągalny instrukcji oznaczonej etykietą `goto` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="fb0dd-191">(Wyjątek: Jeśli `goto` Instrukcja znajduje się wewnątrz `try` zawierającej `finally` bloku i instrukcja labeled znajduje się poza `try`i punkt końcowy `finally` bloku jest niedostępny, a następnie nie jest dostępny z etykietą instrukcji które `goto` instrukcja.)</span><span class="sxs-lookup"><span data-stu-id="fb0dd-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="fb0dd-192">Instrukcje deklaracji</span><span class="sxs-lookup"><span data-stu-id="fb0dd-192">Declaration statements</span></span>

<span data-ttu-id="fb0dd-193">A *declaration_statement* deklaruje lokalną zmienną lub stałą.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="fb0dd-194">Instrukcje deklaracji są dozwolone w blokach, ale nie są dozwolone jako osadzonych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="fb0dd-195">Deklaracje zmiennych lokalnych</span><span class="sxs-lookup"><span data-stu-id="fb0dd-195">Local variable declarations</span></span>

<span data-ttu-id="fb0dd-196">A *local_variable_declaration* deklaruje jeden lub więcej zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="fb0dd-197">*Local_variable_type* z *local_variable_declaration* bezpośrednio określa rodzaj zmiennych, wprowadzone przez tę deklarację, albo wskazuje z identyfikatorem `var` , można wywnioskować typu, oparte na inicjatora.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="fb0dd-198">Typ następuje lista *local_variable_declarator*s, z których każdy wprowadza nową zmienną.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="fb0dd-199">A *local_variable_declarator* składa się z *identyfikator* , nazwy zmiennej, opcjonalnie następuje "`=`" token i *local_variable_initializer* daje to początkowa wartość zmiennej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="fb0dd-200">W kontekście deklaracji zmiennej lokalnej, var identyfikator działa jako kontekstowe słowo kluczowe ([słowa kluczowe](lexical-structure.md#keywords)). Gdy *local_variable_type* jest określony jako `var` i bez typu o nazwie `var` jest w zakresie deklaracja znajduje się ***niejawnie wpisanych deklaracji zmiennej lokalnej***, którego typem jest wnioskowany z typu wyrażenia inicjatora skojarzone.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="fb0dd-201">Niejawnie wpisane deklaracje zmiennych lokalnych podlegają następującym ograniczeniom:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="fb0dd-202">*Local_variable_declaration* nie może zawierać wiele *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="fb0dd-203">*Local_variable_declarator* musi zawierać *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="fb0dd-204">*Local_variable_initializer* musi być *wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="fb0dd-205">Inicjator *wyrażenie* musi mieć typ kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="fb0dd-206">Inicjator *wyrażenie* nie można odwołać się sama Zmienna zadeklarowana</span><span class="sxs-lookup"><span data-stu-id="fb0dd-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="fb0dd-207">Poniżej przedstawiono przykłady niepoprawne niejawnie wpisane deklaracji zmiennych lokalnych:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="fb0dd-208">Wartość zmiennej lokalnej jest uzyskiwana przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)), i wartość zmiennej lokalnej jest modyfikowane przy użyciu *przypisania* () [Operatory przypisania](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="fb0dd-209">Zmienna lokalna musi zostać zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) w każdej lokalizacji, w których uzyskuje się wartość.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="fb0dd-210">Zakres zmiennej lokalnej zadeklarowanej w *local_variable_declaration* bloku, w którym występuje deklaracja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="fb0dd-211">Jest błędem do odwoływania się do zmiennej lokalnej w stanie tekstową poprzedzającym *local_variable_declarator* zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="fb0dd-212">W zakresie zmiennej lokalnej jest błąd kompilacji, aby zadeklarować lokalnego innej zmiennej czy stałej o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="fb0dd-213">Deklaracji zmiennej lokalnej, która deklaruje wiele zmiennych jest odpowiednikiem w wielu deklaracjach zmiennych pojedynczego tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="fb0dd-214">Ponadto inicjatorze zmiennej w deklaracji zmiennej lokalnej dokładnie odpowiada instrukcji przypisania, który jest wstawiany bezpośrednio po deklaracji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="fb0dd-215">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb0dd-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="fb0dd-216">dokładnie odpowiada</span><span class="sxs-lookup"><span data-stu-id="fb0dd-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="fb0dd-217">W niejawnie wpisane deklaracji zmiennej lokalnej typ zmiennej lokalnej deklarowanej przyjmuje się taka sama jak typ wyrażenia używane do inicjowania zmiennej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="fb0dd-218">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="fb0dd-219">Niejawnie wpisane lokalnej deklaracji zmiennej powyżej dokładnie odpowiadają następujące deklaracje jawnie wpisanych:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="fb0dd-220">Deklaracji stałej lokalnej</span><span class="sxs-lookup"><span data-stu-id="fb0dd-220">Local constant declarations</span></span>

<span data-ttu-id="fb0dd-221">A *local_constant_declaration* deklaruje co najmniej jedną stałą lokalnego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="fb0dd-222">*Typu* z *local_constant_declaration* Określa typ stałe wprowadzonych przez tę deklarację.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="fb0dd-223">Typ następuje lista *constant_declarator*s, z których każdy wprowadza nowe — stała.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="fb0dd-224">A *constant_declarator* składa się z *identyfikator* nazw stałą, a następnie "`=`" token, a następnie *constant_expression* ([ Wyrażenia stałe](expressions.md#constant-expressions)) daje wartość stałej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="fb0dd-225">*Typu* i *constant_expression* deklaracji stałej lokalnej należy wykonać te same zasady, jak te w deklaracji stałej składowej ([stałe](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="fb0dd-226">Wartość stała lokalna są uzyskiwane przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="fb0dd-227">Zakres stała lokalna jest bloku, w którym występuje deklaracja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="fb0dd-228">Jest błędem do odwoływania się do stała lokalna w stanie tekstową poprzedzającym jego *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="fb0dd-229">W zakresie stała lokalna jest błąd kompilacji, aby zadeklarować lokalnego innej zmiennej czy stałej o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="fb0dd-230">Deklaracji stałej lokalnej, która deklaruje kilka stałych jest odpowiednikiem w wielu deklaracjach pojedynczego stałe tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="fb0dd-231">Instrukcje wyrażeń</span><span class="sxs-lookup"><span data-stu-id="fb0dd-231">Expression statements</span></span>

<span data-ttu-id="fb0dd-232">*Expression_statement* daje w wyniku podanego wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="fb0dd-233">Wartość obliczona przez wyrażenie, jeśli istnieje, zostanie odrzucony.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="fb0dd-234">Nie wszystkie wyrażenia są dozwolone jako instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="fb0dd-235">W szczególności wyrażenia takie jak `x + y` i `x == 1` który jedynie obliczenia wartości, (które zostaną odrzucone), nie są dozwolone jako instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="fb0dd-236">Wykonywanie *expression_statement* oblicza wyrażenie zawarte, a następnie przekazuje sterowanie do punktu końcowego *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="fb0dd-237">Punkt końcowy *expression_statement* jest dostępny, jeśli to *expression_statement* jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="fb0dd-238">Instrukcje wyboru</span><span class="sxs-lookup"><span data-stu-id="fb0dd-238">Selection statements</span></span>

<span data-ttu-id="fb0dd-239">Instrukcje wyboru Wybierz jeden z wielu możliwych instrukcji do wykonania na podstawie wartości w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="fb0dd-240">If — instrukcja</span><span class="sxs-lookup"><span data-stu-id="fb0dd-240">The if statement</span></span>

<span data-ttu-id="fb0dd-241">`if` Instrukcja wybiera instrukcję do wykonania w oparciu o wartość wyrażenia logicznego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="fb0dd-242">`else` Część jest skojarzony z leksykalnie najbliższej poprzedzającej `if` , jest dozwolony przez składnię.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="fb0dd-243">W efekcie `if` instrukcji formularza</span><span class="sxs-lookup"><span data-stu-id="fb0dd-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="fb0dd-244">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="fb0dd-244">is equivalent to</span></span>
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

<span data-ttu-id="fb0dd-245">`if` Instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-246">*Boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)) jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="fb0dd-247">Jeśli wyrażenie logiczne daje `true`, kontrola jest przekazywana do pierwszej instrukcji osadzonych.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="fb0dd-248">Gdy i kontrola osiąga punkt końcowy w tej instrukcji, kontrola jest przekazywana do punktu końcowego `if` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="fb0dd-249">Jeśli wyrażenie logiczne daje `false` i, jeśli `else` części, kontrola jest przekazywana do drugiego osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="fb0dd-250">Gdy i kontrola osiąga punkt końcowy w tej instrukcji, kontrola jest przekazywana do punktu końcowego `if` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="fb0dd-251">Jeśli wyrażenie logiczne daje `false` i, jeśli `else` część nie jest obecny, kontrola jest przekazywana do punktu końcowego `if` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="fb0dd-252">Pierwszy osadzona instrukcja `if` instrukcja jest osiągalny Jeśli `if` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `false`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="fb0dd-253">Drugi osadzona instrukcja `if` instrukcji, jeśli jest obecny, jest dostępny jeśli `if` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="fb0dd-254">Punkt końcowy `if` instrukcja jest osiągalna, jeśli co najmniej jeden z jego osadzonych instrukcji punktu końcowego jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="fb0dd-255">Ponadto punkt końcowy `if` instrukcji bez `else` części jest dostępny jeśli `if` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="fb0dd-256">Instrukcja switch</span><span class="sxs-lookup"><span data-stu-id="fb0dd-256">The switch statement</span></span>

<span data-ttu-id="fb0dd-257">Instrukcja switch wybiera do wykonania listę instrukcji, mających element label skojarzonego przełącznika, który odpowiada wartości w wyrażeniu przełącznika.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="fb0dd-258">A *switch_statement* składa się z słowa kluczowego `switch`, następuje wyrażenia ujętego w nawiasy (nazywane w wyrażeniu przełącznika), a następnie *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="fb0dd-259">*Switch_block* składa się z zero lub więcej *switch_section*s, ujęte w nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="fb0dd-260">Każdy *switch_section* składa się z co najmniej jeden *switch_label*następuje s *statement_list* ([Wyświetla listę instrukcji](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="fb0dd-261">***Regulujące typu*** z `switch` instrukcji jest ustanawiane przez wyrażenie switch.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="fb0dd-262">Jeśli typ wyrażenia switch `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, lub *enum_type*, lub jeśli jest typu dopuszczającego wartość null, jednego z tych typów, a następnie jest regulujące typu `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="fb0dd-263">W przeciwnym razie dokładnie jeden zdefiniowany przez użytkownika niejawnej konwersji ([zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions)) musi istnieć z typu wyrażenia switch do jednej z następujących możliwych typów regulujące: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, lub typ dopuszczający wartość null, jednego z tych typów.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="fb0dd-264">W przeciwnym razie nie takie istnieje niejawna konwersja lub jeśli więcej niż jeden taki niejawnej konwersji istnieje, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-265">Wyrażenie stałe każdego `case` etykiety musi określa wartość, która jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) do zarządzania typu `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="fb0dd-266">Występuje błąd kompilacji, jeśli co najmniej dwóch `case` etykiety w tej samej `switch` instrukcji Określ taką samą wartość stałą.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="fb0dd-267">Może być co najwyżej jeden `default` etykiet w instrukcji switch.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="fb0dd-268">A `switch` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-269">Wyrażenie switch jest obliczane i konwertowane na typ zarządzania.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="fb0dd-270">Jeśli określono jednej ze stałych w `case` etykiety w tej samej `switch` instrukcja jest równa wartości w wyrażeniu przełącznika, kontrola jest przekazywana do listy instrukcji po dopasowanej `case` etykiety.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="fb0dd-271">Jeśli żadna ze stałych określone w `case` etykiety w tej samej `switch` instrukcja jest równa wartości w wyrażeniu przełącznika i, jeśli `default` etykiety, kontrola jest przekazywana do instrukcji poniżej listy `default` Etykieta.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="fb0dd-272">Jeśli żadna ze stałych określone w `case` etykiety w tej samej `switch` instrukcja jest równa wartości w wyrażeniu przełącznika i jeśli nie `default` etykiety, kontrola jest przekazywana do punktu końcowego `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="fb0dd-273">Jeśli punkt końcowy listy instrukcji w sekcji przełącznika, jest dostępny, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="fb0dd-274">Jest to nazywane reguły "nie należą do".</span><span class="sxs-lookup"><span data-stu-id="fb0dd-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="fb0dd-275">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb0dd-275">The example</span></span>
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
<span data-ttu-id="fb0dd-276">jest prawidłowy, ponieważ żadna sekcja przełącznika ma osiągalnego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="fb0dd-277">W przeciwieństwie do C i C++ wykonanie sekcji przełącznika nie jest dozwolone "przechodzić" do następnej sekcji przełącznika, a przykład</span><span class="sxs-lookup"><span data-stu-id="fb0dd-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="fb0dd-278">spowoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-278">results in a compile-time error.</span></span> <span data-ttu-id="fb0dd-279">Podczas wykonywania sekcji przełącznika ma być stosowana przez wykonanie innej sekcji przełącznika, jawnie `goto case` lub `goto default` instrukcji należy użyć:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="fb0dd-280">Wiele etykiet są dozwolone w *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="fb0dd-281">Przykład</span><span class="sxs-lookup"><span data-stu-id="fb0dd-281">The example</span></span>
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
<span data-ttu-id="fb0dd-282">jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-282">is valid.</span></span> <span data-ttu-id="fb0dd-283">Przykład naruszają reguły "nie należą do", ponieważ etykiet `case 2:` i `default:` są dostępne w ramach tego samego *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="fb0dd-284">Reguła "nie należą do" uniemożliwia klasę typowych błędów, które występują w językach C i C++ gdy `break` instrukcji przypadkowo są pomijane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="fb0dd-285">Ponadto ze względu na tę regułę sekcji przełącznika `switch` instrukcji można dowolnie zmieniać wprowadza bez wywierania wpływu na zachowanie poufności informacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="fb0dd-286">Na przykład sekcji `switch` powyższych instrukcji można wycofać bez wywierania wpływu na zachowanie poufności informacji:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="fb0dd-287">Listy instrukcji w sekcji przełącznika, zazwyczaj kończy się `break`, `goto case`, lub `goto default` instrukcji, ale wszystkie konstrukcji, która renderuje punkt końcowy listy instrukcji jest nieosiągalny jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="fb0dd-288">Na przykład `while` instrukcji wyrażenia logicznego w wartości clientauthtrustmode `true` wiadomo, że nigdy nie zasięg jej punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="fb0dd-289">Podobnie `throw` lub `return` instrukcji zawsze zostanie przetransferowany kontrolki w innym miejscu i nigdy nie osiągnie jej punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="fb0dd-290">Tak więc poniższy przykład jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="fb0dd-291">Typ zarządzania `switch` instrukcja może być typem `string`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="fb0dd-292">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-292">For example:</span></span>
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

<span data-ttu-id="fb0dd-293">Operatory porównania ciągów, takich jak ([operatory porównania ciągów](expressions.md#string-equality-operators)), `switch` instrukcji jest uwzględniana wielkość liter i wykona sekcji danym przełącznikiem tylko wtedy, gdy ciąg wyrażenia switch dokładnie odpowiada `case` etykiety stała.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="fb0dd-294">Gdy regulujące typu `switch` instrukcja jest `string`, wartość `null` jest dozwolona jako stała etykietę "case".</span><span class="sxs-lookup"><span data-stu-id="fb0dd-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="fb0dd-295">*Statement_list*s *switch_block* może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="fb0dd-296">Zakres zmiennej lokalnej zmiennej czy stałej zadeklarowanych w blok "switch" jest blok "switch".</span><span class="sxs-lookup"><span data-stu-id="fb0dd-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="fb0dd-297">Listy instrukcji w sekcji danym przełącznikiem jest osiągalny Jeśli `switch` instrukcji jest osiągalny i co najmniej jedną z następujących ma wartość true:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="fb0dd-298">Wyrażenie switch jest wartością stałą.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="fb0dd-299">Wyrażenie switch jest wartością stałą, który odpowiada `case` etykiety w sekcji przełącznika.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="fb0dd-300">Wyrażenie switch jest wartością stałą, która nie pasuje do żadnego `case` etykietę, a sekcja przełącznika zawiera `default` etykiety.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="fb0dd-301">Etykiety switch sekcji przełącznika odwołuje się osiągalny `goto case` lub `goto default` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="fb0dd-302">Punkt końcowy `switch` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="fb0dd-303">`switch` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="fb0dd-304">`switch` Instrukcja jest osiągalny, wyrażenie switch jest wartością stałą, a nie `default` etykiety jest obecny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="fb0dd-305">`switch` Instrukcja jest osiągalny, wyrażenie switch jest wartością stałą, która nie pasuje do żadnego `case` etykiety, a nie `default` etykiety jest obecny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="fb0dd-306">Instrukcje iteracji</span><span class="sxs-lookup"><span data-stu-id="fb0dd-306">Iteration statements</span></span>

<span data-ttu-id="fb0dd-307">Instrukcje iteracji wykonać wielokrotnie osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="fb0dd-308">While — instrukcja</span><span class="sxs-lookup"><span data-stu-id="fb0dd-308">The while statement</span></span>

<span data-ttu-id="fb0dd-309">`while` Instrukcji warunkowo wykonuje osadzona instrukcja zero lub więcej razy.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="fb0dd-310">A `while` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-311">*Boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)) jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="fb0dd-312">Jeśli wyrażenie logiczne daje `true`, kontrola jest przekazywana do instrukcji osadzonych.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="fb0dd-313">Gdy, a kontrola osiąga punkt końcowy osadzonych instrukcji (prawdopodobnie w wykonywania `continue` instrukcji), kontrola jest przekazywana do stanu sprzed `while` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="fb0dd-314">Jeśli wyrażenie logiczne daje `false`, kontrola jest przekazywana do punktu końcowego `while` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="fb0dd-315">W ramach osadzona instrukcja z `while` instrukcji, `break` — instrukcja ([instrukcji break](statements.md#the-break-statement)) może służyć do kontrola jest przekazywana do punktu końcowego `while` — instrukcja (tym samym Kończenie iteracji osadzonego Instrukcja) i `continue` — instrukcja ([instrukcji continue](statements.md#the-continue-statement)) może służyć do kontrola jest przekazywana do punktu końcowego osadzona instrukcja (ten sposób wykonywania iteracji innego `while` instrukcji).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="fb0dd-316">Osadzona instrukcja z `while` instrukcja jest osiągalny Jeśli `while` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `false`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="fb0dd-317">Punkt końcowy `while` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="fb0dd-318">`while` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `while` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="fb0dd-319">`while` Instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="fb0dd-320">Instrukcji</span><span class="sxs-lookup"><span data-stu-id="fb0dd-320">The do statement</span></span>

<span data-ttu-id="fb0dd-321">`do` Instrukcji warunkowo wykonuje osadzona instrukcja jeden lub więcej razy.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="fb0dd-322">A `do` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-323">Kontrola jest przekazywana do instrukcji osadzonych.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="fb0dd-324">Gdy, a kontrola osiąga punkt końcowy osadzonych instrukcji (prawdopodobnie w wykonywania `continue` instrukcji), *boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)) jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="fb0dd-325">Jeśli wyrażenie logiczne daje `true`, kontrola jest przekazywana do stanu sprzed `do` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="fb0dd-326">W przeciwnym razie kontrola jest przekazywana do punktu końcowego `do` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="fb0dd-327">W ramach osadzona instrukcja z `do` instrukcji, `break` — instrukcja ([instrukcji break](statements.md#the-break-statement)) może służyć do kontrola jest przekazywana do punktu końcowego `do` — instrukcja (tym samym Kończenie iteracji osadzonego Instrukcja) i `continue` — instrukcja ([instrukcji continue](statements.md#the-continue-statement)) może służyć do kontrola jest przekazywana do punktu końcowego osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="fb0dd-328">Osadzona instrukcja z `do` instrukcja jest osiągalny Jeśli `do` instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="fb0dd-329">Punkt końcowy `do` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="fb0dd-330">`do` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `do` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="fb0dd-331">Osadzona instrukcja punkt końcowy jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="fb0dd-332">For — instrukcja</span><span class="sxs-lookup"><span data-stu-id="fb0dd-332">The for statement</span></span>

<span data-ttu-id="fb0dd-333">`for` Instrukcja oblicza sekwencję wyrażenia inicjowania, a następnie, gdy warunek ma wartość true, wielokrotnie wykonuje osadzona instrukcja i ocenia sekwencji wyrażeń iteracji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="fb0dd-334">*For_initializer*, jeśli jest obecny, składa się z jednej *local_variable_declaration* ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) lub Podaj listę *statement_ wyrażenie*s ([instrukcje wyrażeń](statements.md#expression-statements)) rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="fb0dd-335">Zakres zmienna lokalna zadeklarowana przez *for_initializer* rozpoczyna się od *local_variable_declarator* zmiennej i rozszerza się na końcu osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="fb0dd-336">Zakres obejmuje *for_condition* i *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="fb0dd-337">*For_condition*, jeśli jest obecna, musi być *boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="fb0dd-338">*For_iterator*, jeśli jest obecny, składa się z listy *statement_expression*s ([instrukcje wyrażeń](statements.md#expression-statements)) rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="fb0dd-339">Dla instrukcji jest wykonywany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-340">Jeśli *for_initializer* jest obecne, inicjatory zmiennej lub wyrażenia instrukcji są wykonywane w kolejności, są one zapisywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="fb0dd-341">Wykonanie tego kroku tylko raz.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-341">This step is only performed once.</span></span>
*  <span data-ttu-id="fb0dd-342">Jeśli *for_condition* jest obecny, zostanie on oceniony.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="fb0dd-343">Jeśli *for_condition* nie jest obecny lub jeśli wyznaczające `true`, kontrola jest przekazywana do instrukcji osadzonych.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="fb0dd-344">Gdy, a kontrola osiąga punkt końcowy osadzonych instrukcji (prawdopodobnie w wykonywania `continue` instrukcji), wyrażeń *for_iterator*, jeśli istnieją, są obliczane w kolejności, a następnie innej iteracji wykonywana, począwszy od wersji ewaluacyjnej usługi *for_condition* w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="fb0dd-345">Jeśli *for_condition* jest obecny i ocena daje `false`, kontrola jest przekazywana do punktu końcowego `for` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="fb0dd-346">W ramach osadzona instrukcja z `for` instrukcji, `break` — instrukcja ([instrukcji break](statements.md#the-break-statement)) może służyć do kontrola jest przekazywana do punktu końcowego `for` — instrukcja (tym samym Kończenie iteracji osadzonego Instrukcja) i `continue` — instrukcja ([instrukcji continue](statements.md#the-continue-statement)) może służyć do kontrola jest przekazywana do punktu końcowego osadzona instrukcja (wykonywanie w związku z tym *for_iterator* i wykonywanie iteracji innego `for` instrukcji, począwszy od *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="fb0dd-347">Osadzona instrukcja z `for` instrukcja jest osiągalna, jeśli jest spełniony jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="fb0dd-348">`for` Instrukcja jest dostępny i nie *for_condition* jest obecny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="fb0dd-349">`for` Instrukcja jest osiągalny i *for_condition* jest obecny i nie ma wartości stałej `false`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="fb0dd-350">Punkt końcowy `for` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="fb0dd-351">`for` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `for` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="fb0dd-352">`for` Instrukcja jest osiągalny i *for_condition* jest obecny i nie ma wartości stałej `true`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="fb0dd-353">Instrukcja foreach</span><span class="sxs-lookup"><span data-stu-id="fb0dd-353">The foreach statement</span></span>

<span data-ttu-id="fb0dd-354">`foreach` Instrukcji wylicza elementów kolekcji, wykonywania osadzonych instrukcji dla każdego elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="fb0dd-355">*Typu* i *identyfikator* z `foreach` instrukcja deklaruje ***zmiennej iteracyjnej*** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="fb0dd-356">Jeśli `var` identyfikator jest podawana jako *local_variable_type*, a nie typu o nazwie `var` jest w zakresie, zmienną iteracji jest nazywany ***zmiennej iteracyjnej niejawnie wpisane***, i jego typ przyjmuje się typ elementu `foreach` instrukcji, jak określono poniżej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="fb0dd-357">Zmienna iteracji odnosi się do zmiennej lokalnej tylko do odczytu z zakresem, który rozciąga osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="fb0dd-358">Podczas wykonywania `foreach` instrukcji, Zmienna iteracji reprezentuje element kolekcji, dla którego iteracji obecnie wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="fb0dd-359">Występuje błąd kompilacji, jeśli osadzona instrukcja próbuje zmodyfikować zmienną iteracji (za pośrednictwem przydziału lub `++` i `--` operatory) lub Przekaż Zmienna iteracji jako `ref` lub `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="fb0dd-360">W następującym w celu skrócenia programu, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` i `IEnumerator<T>` można znaleźć odpowiadające typy w przestrzeniach nazw `System.Collections` i `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="fb0dd-361">Foreach — instrukcja przetwarzania kompilacji najpierw określi ***— typ kolekcji***, ***typu modułu wyliczającego*** i ***typ elementu*** wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="fb0dd-362">Oznaczanie przebiega w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="fb0dd-363">Jeśli typ `X` z *wyrażenie* jest typem tablicy, a następnie istnieje niejawna konwersja odwołania z `X` do `IEnumerable` interfejsu (ponieważ `System.Array` implementuje ten interfejs).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="fb0dd-364">***— Typ kolekcji*** jest `IEnumerable` interfejsu ***typu modułu wyliczającego*** jest `IEnumerator` interfejsu i ***typ elementu*** jest typ elementu Tablica typu `X`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="fb0dd-365">Jeśli typ `X` z *wyrażenie* jest `dynamic` , a następnie istnieje niejawna konwersja z *wyrażenie* do `IEnumerable` interfejsu ([niejawne dynamiczny Konwersje](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="fb0dd-366">***— Typ kolekcji*** jest `IEnumerable` interfejsu i ***typu modułu wyliczającego*** jest `IEnumerator` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="fb0dd-367">Jeśli `var` identyfikator jest podawana jako *local_variable_type* , a następnie ***typ elementu*** jest `dynamic`, w przeciwnym razie jest `object`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="fb0dd-368">W przeciwnym razie określenia czy typ `X` ma odpowiednią `GetEnumerator` metody:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="fb0dd-369">Wykonaj wyszukać składowej w typie `X` o identyfikatorze `GetEnumerator` i bez argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="fb0dd-370">Jeśli wyszukanie członka nie generuje dopasowania lub powoduje niejednoznaczność lub tworzy dopasowanie, która nie jest grupą metod, sprawdź, czy interfejs wyliczalny zgodnie z poniższym opisem.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="fb0dd-371">Zaleca się, że ostrzeżenie wydawane Jeśli wyszukiwanie elementu członkowskiego generuje wszystko oprócz grupy metod lub Brak dopasowania.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="fb0dd-372">Wykonanie rozwiązania przeciążenia, przy użyciu wynikowy grupy metod i pustą listą argumentów.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="fb0dd-373">Jeśli funkcja rozpoznawania przeciążeń w wyniku żadnych odpowiednich metod, powoduje niejednoznaczność lub skutkuje pojedynczego najlepszą metodę, ale ta metoda jest statyczny lub niepubliczny, wyszukaj interfejs wyliczalny zgodnie z poniższym opisem.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="fb0dd-374">Zaleca się, że ostrzeżenie wydawane Jeśli funkcja rozpoznawania przeciążeń generuje niczego poza metodą jednoznaczną publiczne wystąpienia lub nie ma zastosowania metody.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="fb0dd-375">Jeśli typ zwracany `E` z `GetEnumerator` metoda nie jest klasą, typu struktury lub interfejsu, błąd jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="fb0dd-376">Wyszukiwanie elementu członkowskiego jest wykonywana na `E` z identyfikatorem `Current` i bez argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="fb0dd-377">W przypadku wyszukiwania elementu członkowskiego generuje niezgodności, wynikiem jest błąd lub wynik jest jakikolwiek inny niż właściwości publiczne wystąpienia, która umożliwia odczytywanie, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="fb0dd-378">Wyszukiwanie elementu członkowskiego jest wykonywana na `E` z identyfikatorem `MoveNext` i bez argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="fb0dd-379">W przypadku wyszukiwania elementu członkowskiego generuje niezgodności, wynikiem jest błąd lub wynik jest końcówką grupy metod, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="fb0dd-380">Rozpoznanie przeciążenia odbywa się w grupie metody z pustą listą argumentów.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="fb0dd-381">Jeśli wyniki rozpoznawania przeciążenia w nie odpowiednich metod, powoduje niejednoznaczność lub wyniki w pojedynczej najlepszej metody, ale ta metoda jest statyczna lub niepubliczna lub nie jest typem zwracanym `bool`, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="fb0dd-382">***— Typ kolekcji*** jest `X`, ***typu modułu wyliczającego*** jest `E`i ***typ elementu*** jest typem `Current` właściwości.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="fb0dd-383">W przeciwnym razie sprawdź, czy wyliczalny interfejs:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="fb0dd-384">Jeśli wśród wszystkich typów `Ti` dla której istnieje niejawna konwersja z `X` do `IEnumerable<Ti>`, ma typ unikatowego `T` tak, aby `T` nie `dynamic` i dla wszystkich innych `Ti` istnieje niejawna konwersja z `IEnumerable<T>` do `IEnumerable<Ti>`, a następnie ***— typ kolekcji*** jest interfejsem `IEnumerable<T>`, ***typu modułu wyliczającego*** interfejs `IEnumerator<T>`i ***typ elementu*** jest `T`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="fb0dd-385">W przeciwnym razie, jeśli istnieje więcej niż jeden taki typ `T`, a następnie jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="fb0dd-386">W przeciwnym razie, jeśli istnieje niejawna konwersja z `X` do `System.Collections.IEnumerable` interfejsu, a następnie ***— typ kolekcji*** ten interfejs jest ***typu modułu wyliczającego*** interfejs `System.Collections.IEnumerator`i ***typ elementu*** jest `object`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="fb0dd-387">W przeciwnym razie jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="fb0dd-388">Powyższe kroki, jeśli to się powiedzie, jednoznacznie wygenerować typu kolekcji `C`, moduł wyliczający typu `E` i typ elementu `T`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="fb0dd-389">Instrukcja foreach formularza</span><span class="sxs-lookup"><span data-stu-id="fb0dd-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="fb0dd-390">jest rozszerzany do:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-390">is then expanded to:</span></span>
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

<span data-ttu-id="fb0dd-391">Zmienna `e` nie jest widoczne lub dostępne do wyrażenia `x` lub osadzona instrukcja lub inny kod źródłowy programu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="fb0dd-392">Zmienna `v` jest tylko do odczytu w osadzonych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="fb0dd-393">Jeśli nie jest jawną konwersję ([jawne konwersje](conversions.md#explicit-conversions)) z `T` (typ elementu) do `V` ( *local_variable_type* w instrukcji foreach), błąd jest generowany. i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="fb0dd-394">Jeśli `x` ma wartość `null`, `System.NullReferenceException` jest generowany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="fb0dd-395">Implementacja jest dozwolony do zaimplementowania danego Instrukcja foreach inaczej, np. ze względu na wydajność, tak długo, jak zachowanie jest zgodne z rozszerzeniem powyżej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="fb0dd-396">Umieszczanie `v` wewnątrz while jest ważne w przypadku jak są przechwytywane przez funkcję anonimowe występujących w pętli *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="fb0dd-397">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="fb0dd-398">Jeśli `v` została zadeklarowana poza while pętli, mogłyby być udostępniane między wszystkie iteracje i jego wartość po dla pętli będzie końcowa wartość `13`, czyli jakich wywołania z `f` będzie drukować.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="fb0dd-399">Zamiast tego ponieważ każda iteracja ma swoje własne zmiennej `v`, jeden przechwycone przez `f` w pierwszej iteracji będą w dalszym zawierającą wartość parametru `7`, czyli, co zostanie wydrukowany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="fb0dd-400">(Uwaga: zadeklarowana wcześniejszych wersjach języka C# `v` pętli while poza.)</span><span class="sxs-lookup"><span data-stu-id="fb0dd-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="fb0dd-401">Treść na koniec bloku jest tworzona zgodnie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="fb0dd-402">Jeśli istnieje niejawna konwersja z `E` do `System.IDisposable` interfejsu, następnie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="fb0dd-403">Jeśli `E` jest typem wartości niedopuszczającym wartości, a następnie na koniec klauzula jest rozwinięty, odpowiednik semantyczne:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="fb0dd-404">W przeciwnym razie na koniec klauzula jest rozwinięty, odpowiednik semantyczne:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="fb0dd-405">z wyjątkiem, że jeśli `E` jest typem wartości lub parametrem typu wystąpienia typu wartości, a następnie rzutowanie typu `e` do `System.IDisposable` nie spowoduje pakowania wystąpić.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="fb0dd-406">W przeciwnym razie, jeśli `E` typie zapieczętowanym jest ostatecznie klauzula jest rozwinięty, pusty blok:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="fb0dd-407">W przeciwnym razie na koniec klauzuli zostanie rozszerzona, aby:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="fb0dd-408">Zmienna lokalna `d` nie jest widoczne lub dostępne dla kodu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="fb0dd-409">W szczególności, nie powoduje konfliktu z innych zmiennych, których zakres obejmuje bloku finally.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="fb0dd-410">Kolejność, w której `foreach` przechodzi przez elementy tablicy, jest następujący: Dla elementów tablice jednowymiarowe jest przesunięta w indeksie kolejności rosnącej, zaczynając od indeksu `0` i kończąc indeksu `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="fb0dd-411">Dla wielowymiarowych tablic elementów jest przesunięta w taki sposób, że indeksy po prawej stronie wymiaru są pierwszy zwiększona, a następnie dalej wymiaru po lewej stronie, i tak dalej, aby po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="fb0dd-412">Poniższy przykład wyświetla każdej wartości w dwuwymiarowej tablicy, w kolejności elementów:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="fb0dd-413">Dane wyjściowe, generowane jest następująca:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="fb0dd-414">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="fb0dd-415">Typ `n` jest `int`, typ elementu `numbers`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="fb0dd-416">Instrukcje skoku</span><span class="sxs-lookup"><span data-stu-id="fb0dd-416">Jump statements</span></span>

<span data-ttu-id="fb0dd-417">Instrukcje skoku bezwarunkowo przekazać sterowanie.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="fb0dd-418">Nosi nazwę lokalizacji, do którego instrukcja skoku przekazuje sterowanie ***docelowej*** instrukcji skoku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="fb0dd-419">Gdy występuje instrukcja skoku, w bloku, a celem tej instrukcji skoku znajduje się poza ten blok, instrukcja skoku jest nazywany ***wyjść*** bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="fb0dd-420">Gdy instrukcja skoku, mogą przesyłać sterowanie na zewnątrz bloku, je nigdy nie przekazać sterowanie do bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="fb0dd-421">Wykonanie instrukcji skoku jest złożona z obecnością aktywne `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="fb0dd-422">W przypadku braku takich `try` instrukcji, instrukcja skoku bezwarunkowo przekazuje sterowanie z instrukcji skoku do jego element docelowy.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="fb0dd-423">Obecności takich aktywne `try` instrukcji wykonywania jest bardziej złożona.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="fb0dd-424">Czy instrukcja skoku istnieje co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="fb0dd-425">Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="fb0dd-426">Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="fb0dd-427">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-427">In the example</span></span>
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
<span data-ttu-id="fb0dd-428">`finally` skojarzone z dwóch bloków `try` przed kontrola jest przekazywana do obiekt docelowy instrukcji skoku, wykonywane są instrukcje.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="fb0dd-429">Dane wyjściowe, generowane jest następująca:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="fb0dd-430">Instrukcja break</span><span class="sxs-lookup"><span data-stu-id="fb0dd-430">The break statement</span></span>

<span data-ttu-id="fb0dd-431">`break` Instrukcja kończy działanie najbliższej otaczającej `switch`, `while`, `do`, `for`, lub `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="fb0dd-432">Celem `break` instrukcji jest punktem końcowym najbliższej otaczającej `switch`, `while`, `do`, `for`, lub `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="fb0dd-433">Jeśli `break` instrukcja nie jest ujęta w `switch`, `while`, `do`, `for`, lub `foreach` występuje błąd kompilacji instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-434">Gdy wiele `switch`, `while`, `do`, `for`, lub `foreach` instrukcje są zagnieżdżone w obrębie siebie nawzajem `break` poufności informacji odnoszą się tylko do deklaracji najbardziej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="fb0dd-435">Aby przekazać sterowanie na wielu poziomach zagnieżdżenia, `goto` — instrukcja ([instrukcji goto](statements.md#the-goto-statement)) musi być używana.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="fb0dd-436">A `break` instrukcja nie może zamknąć `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="fb0dd-437">Gdy `break` występuje instrukcja w obrębie `finally` zablokować, obiektu docelowego `break` instrukcja musi być w tej samej `finally` zablokować; w przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-438">A `break` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-439">Jeśli `break` instrukcja kończy działanie co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="fb0dd-440">Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="fb0dd-441">Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="fb0dd-442">Kontrola jest przekazywana do lokalizacji docelowej `break` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="fb0dd-443">Ponieważ `break` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `break` instrukcji nigdy nie jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="fb0dd-444">Instrukcja kontynuowania</span><span class="sxs-lookup"><span data-stu-id="fb0dd-444">The continue statement</span></span>

<span data-ttu-id="fb0dd-445">`continue` Instrukcji uruchamia nową iterację najbliższej otaczającej `while`, `do`, `for`, lub `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="fb0dd-446">Celem `continue` instrukcja jest punkt końcowy osadzona instrukcja najbliższej otaczającej `while`, `do`, `for`, lub `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="fb0dd-447">Jeśli `continue` instrukcja nie jest ujęta w `while`, `do`, `for`, lub `foreach` występuje błąd kompilacji instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-448">Gdy wiele `while`, `do`, `for`, lub `foreach` instrukcje są zagnieżdżone w obrębie siebie nawzajem `continue` poufności informacji odnoszą się tylko do deklaracji najbardziej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="fb0dd-449">Aby przekazać sterowanie na wielu poziomach zagnieżdżenia, `goto` — instrukcja ([instrukcji goto](statements.md#the-goto-statement)) musi być używana.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="fb0dd-450">A `continue` instrukcja nie może zamknąć `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="fb0dd-451">Gdy `continue` występuje instrukcja w obrębie `finally` zablokować, obiektu docelowego `continue` instrukcja musi być w tej samej `finally` zablokować; w przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-452">A `continue` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-453">Jeśli `continue` instrukcja kończy działanie co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="fb0dd-454">Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="fb0dd-455">Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="fb0dd-456">Kontrola jest przekazywana do lokalizacji docelowej `continue` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="fb0dd-457">Ponieważ `continue` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `continue` instrukcji nigdy nie jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="fb0dd-458">Goto — instrukcja</span><span class="sxs-lookup"><span data-stu-id="fb0dd-458">The goto statement</span></span>

<span data-ttu-id="fb0dd-459">`goto` Instrukcji przekazuje sterowanie do instrukcji, która jest oznaczona przez etykietę.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="fb0dd-460">Celem `goto` *identyfikator* instrukcja to instrukcja labeled od podanej etykiety.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="fb0dd-461">Jeśli etykiety o podanej nazwie nie istnieje w bieżącym funkcja składowa lub `goto` instrukcja nie jest w zakresie etykiety, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="fb0dd-462">Ta reguła zezwala na używanie `goto` instrukcję, aby przekazać sterowanie poza zakres zagnieżdżonych, ale nie do zagnieżdżony zakres.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="fb0dd-463">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-463">In the example</span></span>
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
<span data-ttu-id="fb0dd-464">`goto` instrukcja jest używane do przekazywania kontroli zagnieżdżony zakres.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="fb0dd-465">Celem `goto case` instrukcja jest listy instrukcji w bezpośrednio otaczającej `switch` instrukcji ([instrukcji switch](statements.md#the-switch-statement)), który zawiera `case` etykietę z danej wartości stałej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="fb0dd-466">Jeśli `goto case` instrukcja nie jest ujęta w `switch` instrukcji, jeśli *constant_expression* jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) do zarządzania typu najbliższej otaczającej `switch` instrukcji, lub jeśli najbliższej otaczającej `switch` nie zawiera instrukcji `case` etykiety z danej wartości stałych, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-467">Celem `goto default` instrukcja jest listy instrukcji w bezpośrednio otaczającej `switch` — instrukcja ([instrukcji switch](statements.md#the-switch-statement)), który zawiera `default` etykiety.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="fb0dd-468">Jeśli `goto default` instrukcja nie jest ujęta w `switch` instrukcji, lub jeśli najbliższej otaczającej `switch` nie zawiera instrukcji `default` etykiety, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-469">A `goto` instrukcja nie może zamknąć `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="fb0dd-470">Gdy `goto` występuje instrukcja w obrębie `finally` zablokować, obiekt docelowy `goto` instrukcja musi być w tej samej `finally` bloku, lub w przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-471">A `goto` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-472">Jeśli `goto` instrukcja kończy działanie co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="fb0dd-473">Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="fb0dd-474">Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="fb0dd-475">Kontrola jest przekazywana do lokalizacji docelowej `goto` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="fb0dd-476">Ponieważ `goto` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `goto` instrukcji nigdy nie jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="fb0dd-477">Instrukcja return</span><span class="sxs-lookup"><span data-stu-id="fb0dd-477">The return statement</span></span>

<span data-ttu-id="fb0dd-478">`return` Instrukcja zwraca kontrolę do bieżącego obiektu wywołującego funkcji, w którym `return` występuje instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="fb0dd-479">A `return` instrukcji za pomocą wyrażenia nie można użyć tylko w funkcji elementu członkowskiego, który nie może obliczyć wartość, czyli metody typu wyniku ([treści metody](classes.md#method-body)) `void`, `set` metody dostępu właściwości lub indeksator, `add` i `remove` Akcesory zdarzenie, Konstruktor wystąpienia, statyczny Konstruktor lub destruktor.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="fb0dd-480">A `return` instrukcji za pomocą wyrażenia należy używać tylko w funkcji elementu członkowskiego, który oblicza wartość, czyli metody z typem innym niż void wynik `get` metody dostępu właściwości lub indeksatora lub operatora zdefiniowanego przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="fb0dd-481">Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia do zwracanego typu zawierającego element członkowski funkcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="fb0dd-482">Zwróć instrukcji można używać w treści wyrażenia funkcja anonimowa ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) i uczestniczyć w określeniu, jakie konwersje istnieje dla tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="fb0dd-483">Jest to błąd czasu kompilacji dla `return` instrukcji, które będą wyświetlane na `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="fb0dd-484">A `return` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-485">Jeśli `return` Instrukcja określa wyrażenie, wyrażenie jest obliczane i wynikową wartość jest konwertowana na typ zwracany funkcji zawierającego przez niejawną konwersję.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="fb0dd-486">Wynik konwersji staje się wartości wyniku utworzonej przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="fb0dd-487">Jeśli `return` instrukcji jest ujęta w co najmniej jeden `try` lub `catch` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="fb0dd-488">Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="fb0dd-489">Ten proces jest powtarzany do momentu `finally` blokuje wszystkie otaczającej `try` instrukcje zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="fb0dd-490">Jeśli zawierającego funkcja nie jest funkcji asynchronicznej, zwróceniem sterowania obiektowi wywołującemu zawierającego funkcji wraz z wartość wyniku ewentualne.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="fb0dd-491">Jeśli funkcja zawierającego funkcji asynchronicznej, zwróceniem sterowania do bieżącego obiektu wywołującego, a wartość wyniku, jest rejestrowana w zadaniu zwracanym zgodnie z opisem w ([interfejsy modułu wyliczającego](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="fb0dd-492">Ponieważ `return` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `return` instrukcji nigdy nie jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="fb0dd-493">Instrukcji "throw"</span><span class="sxs-lookup"><span data-stu-id="fb0dd-493">The throw statement</span></span>

<span data-ttu-id="fb0dd-494">`throw` Instrukcji zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="fb0dd-495">A `throw` instrukcji za pomocą wyrażenia zgłasza wartość utworzone przez obliczenie wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="fb0dd-496">Wyrażenie musi oznaczają wartości o typie danej klasy `System.Exception`, typu klasy, która pochodzi od klasy `System.Exception` lub typu parametru typu, który ma `System.Exception` (lub jego podklasę) jako swojej klasy bazowej skuteczne.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="fb0dd-497">Jeśli generuje obliczania wyrażenia `null`, `System.NullReferenceException` jest generowany, zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="fb0dd-498">A `throw` instrukcji za pomocą wyrażenia nie można użyć tylko w `catch` zablokować, w którym to przypadku tej instrukcji ponownie zgłasza wyjątek, który jest aktualnie obsługiwany przez który `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="fb0dd-499">Ponieważ `throw` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `throw` instrukcji nigdy nie jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="fb0dd-500">Gdy wyjątek jest zgłaszany, kontrola jest przekazywana do pierwszego `catch` w otaczającej klauzuli `try` instrukcję, która może obsłużyć wyjątek.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="fb0dd-501">Proces, który ma miejsce, od punktu rzuceniem wyjątku do punktu transferowanie formantu do obsługi odpowiednich wyjątków jest znany jako ***propagacji wyjątku***.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="fb0dd-502">Propagacji wyjątku, który składa się z wielokrotnie oceny następujące kroki, dopóki `catch` klauzula, która odpowiada wyjątek zostanie znaleziony.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="fb0dd-503">W tym opisie ***throw punktu*** początkowo jest lokalizacja, w którym wyjątek jest zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="fb0dd-504">W bieżącym elemencie członkowskim funkcji każdego `try` instrukcję, która zawiera punkt throw jest sprawdzany pod.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="fb0dd-505">Dla każdej instrukcji `S`, począwszy od najbardziej wewnętrzną funkcją `try` instrukcji i kończącą najbardziej zewnętrzna `try` instrukcji, są sprawdzane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="fb0dd-506">Jeśli `try` bloku `S` otacza punktu throw i jeśli S ma co najmniej jeden `catch` klauzule `catch` klauzule są badane w kolejności występowania, aby zlokalizować odpowiedni program obsługi wyjątku, zgodnie z regułami określonymi w Sekcja [instrukcjami "try"](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="fb0dd-507">Jeśli pasujący obiekt typu `catch` znajduje się klauzuli, propagacji wyjątku zostało zakończone przez przeniesienie kontroli do bloku, który `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="fb0dd-508">W przeciwnym razie, jeśli `try` bloku lub `catch` bloku `S` otacza punktu throw i, jeśli `S` ma `finally` bloku, formant jest przekazywany do `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="fb0dd-509">Jeśli `finally` inny wyjątek bloku, zakończeniem przetwarzania bieżącego wyjątku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="fb0dd-510">W przeciwnym razie, gdy kontrola osiąga punkt końcowy `finally` bloku, przetwarzanie bieżącego wyjątku jest kontynuowane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="fb0dd-511">Jeśli nie znajdują się w na bieżącego wywołania funkcji obsługi wyjątków, wywołania funkcji zostanie zakończony i wystąpi jedno z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="fb0dd-512">Jeśli bieżącą funkcję jest inne niż async, powyższe kroki powtarzają się do obiektu wywołującego funkcji z punktem throw odpowiadający instrukcji, z którego została wywołana funkcja składowa.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="fb0dd-513">W przypadku bieżącej funkcji asynchronicznych i zwracającą zadanie, wyjątek jest rejestrowana w zadaniu zwracanym przechodzi w stan uszkodzoną lub anulowane zgodnie z opisem w [interfejsy modułu wyliczającego](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="fb0dd-514">W przypadku bieżącej funkcji asynchronicznych i zwracającą typ void, kontekst synchronizacji bieżącego wątku jest powiadamiany, zgodnie z opisem w [Wyliczalne interfejsy](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="fb0dd-515">Przetwarzanie wyjątków zakończy wszystkie wywołania funkcji elementu członkowskiego, w bieżącym wątku, wskazującą, czy wątek żadna procedura obsługi wyjątku, następnie wątek jest zakończony.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="fb0dd-516">Wpływ stracą jest zdefiniowane w implementacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="fb0dd-517">Instrukcjami "try"</span><span class="sxs-lookup"><span data-stu-id="fb0dd-517">The try statement</span></span>

<span data-ttu-id="fb0dd-518">`try` Instrukcji udostępnia mechanizm przechwytywanie wyjątków, które występują podczas wykonywania bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="fb0dd-519">Ponadto `try` instrukcja oferuje możliwość określenia bloku kodu, który jest zawsze wykonywane, kiedy formant opuszcza `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="fb0dd-520">Istnieją trzy możliwe formy `try` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="fb0dd-521">A `try` bloku, po której następuje co najmniej jeden `catch` bloków.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="fb0dd-522">A `try` bloku następuje `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="fb0dd-523">A `try` bloku, po której następuje co najmniej jeden `catch` następuje bloki `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="fb0dd-524">Podczas `catch` klauzula Określa *exception_specifier*, musi być typu `System.Exception`, typ, który pochodzi od klasy `System.Exception` lub parametr typu, który ma `System.Exception` (lub jego podklasę) jako jego obowiązujące od Klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="fb0dd-525">Gdy `catch` klauzula określa zarówno *exception_specifier* z *identyfikator*, ***zmiennej wyjątku*** o podanej nazwie i typie jest zadeklarowana.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="fb0dd-526">Zmienna wyjątek odnosi się do zmiennej lokalnej z zakresem, który rozciąga `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="fb0dd-527">Podczas wykonywania *exception_filter* i *bloku*, zmienna wyjątek reprezentuje aktualnie obsługiwany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="fb0dd-528">Do celów sprawdzania asercję określonego przypisania zmiennej wyjątek jest uznawany za zdecydowanie przypisana, aby w swoim zakresie całego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="fb0dd-529">Chyba że `catch` klauzula zawiera nazwa zmiennej wyjątku, nie można uzyskać dostępu do obiektu wyjątku w filtrze i `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="fb0dd-530">A `catch` klauzula, która nie określa *exception_specifier* nosi nazwę ogólnego `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="fb0dd-531">Niektóre języki programowania może obsługiwać wyjątki, które nie są stałego, ponieważ obiekt jest pochodną `System.Exception`, mimo że takie wyjątki nigdy nie można wygenerować przez kod języka C#.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="fb0dd-532">Ogólny `catch` klauzuli mogą służyć do przechwytywania wyjątków.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="fb0dd-533">W efekcie ogólnego `catch` klauzuli semantycznie różni się od jednego, który określa typ `System.Exception`, w tym, że pierwsza również może przechwytywać wyjątki od innych języków.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="fb0dd-534">Aby zlokalizować program obsługi wyjątku, `catch` klauzule są badane w kolejności leksykalne.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="fb0dd-535">Jeśli `catch` klauzula Określa typ, ale żaden filtr wyjątku, jest to błąd czasu kompilacji do późniejszej `catch` klauzuli w tym samym `try` instrukcję, aby określić typ, który jest taki sam jak lub jest pochodną, wpisz.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="fb0dd-536">Jeśli `catch` klauzula określa Brak typu i żaden filtr, musi być ostatni `catch` klauzuli w tym `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="fb0dd-537">W ramach `catch` bloku, `throw` — instrukcja ([instrukcji "throw"](statements.md#the-throw-statement)) za pomocą wyrażenia nie może służyć do ponownego zgłoszenia wyjątku, który został zgłoszony przez `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="fb0dd-538">Przypisania do zmiennej wyjątku nie należy zmieniać wyjątek, który jest ponownie zgłoszony.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="fb0dd-539">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-539">In the example</span></span>
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
<span data-ttu-id="fb0dd-540">Metoda `F` wyłapuje wyjątek, zapisuje niektóre informacje diagnostyczne w konsoli, zmienia zmiennej wyjątku i ponownie zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="fb0dd-541">Wyjątek, który jest ponownie zgłoszony jest oryginalnego wyjątku, dzięki czemu dane wyjściowe, generowane są:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="fb0dd-542">Jeśli pierwszy blok catch miał zgłoszony `e` zamiast ponowne generowanie bieżący wyjątek, dane wyjściowe wytwarzane byłoby w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="fb0dd-543">Jest to błąd czasu kompilacji dla `break`, `continue`, lub `goto` instrukcję, aby przekazać sterowanie z `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="fb0dd-544">Gdy `break`, `continue`, lub `goto` występuje instrukcja w `finally` bloku, obiekt docelowy instrukcji musi być w tej samej `finally` bloku, lub w przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="fb0dd-545">Jest to błąd czasu kompilacji dla `return` instrukcji w `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="fb0dd-546">A `try` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-547">Kontrola jest przekazywana do `try` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="fb0dd-548">Gdy, a kontrola osiąga punkt końcowy `try` bloku:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="fb0dd-549">Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="fb0dd-550">Kontrola jest przekazywana do punktu końcowego `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="fb0dd-551">Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `try` bloku:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="fb0dd-552">`catch` Zdań, jeśli są sprawdzane w kolejności występowania, aby zlokalizować odpowiedni program obsługi wyjątku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="fb0dd-553">Jeśli `catch` klauzuli nie określa typu lub określa typ wyjątku lub typ podstawowy typu wyjątku:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="fb0dd-554">Jeśli `catch` klauzuli deklaruje zmienną wyjątek, obiekt wyjątku jest przypisywana do zmiennej wyjątku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="fb0dd-555">Jeśli `catch` klauzuli deklaruje filtra wyjątku, filtr jest oceniany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="fb0dd-556">Jeśli daje w wyniku `false`klauzuli "catch" nie ma dopasowania i wyszukiwanie jest kontynuowane za pośrednictwem dowolnych kolejnych `catch` klauzule obsługi odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="fb0dd-557">W przeciwnym razie `catch` klauzula jest uznawane za zgodne i kontrola jest przekazywana do dopasowywania `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="fb0dd-558">Gdy, a kontrola osiąga punkt końcowy `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="fb0dd-559">Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="fb0dd-560">Kontrola jest przekazywana do punktu końcowego `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="fb0dd-561">Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="fb0dd-562">Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="fb0dd-563">Wyjątek jest propagowany do następnego otaczający `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="fb0dd-564">Jeśli `try` nie ma instrukcji `catch` klauzule lub jeśli nie `catch` klauzuli odpowiada wyjątek:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="fb0dd-565">Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="fb0dd-566">Wyjątek jest propagowany do następnego otaczający `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="fb0dd-567">Instrukcje `finally` bloku są wykonywane zawsze, kiedy formant opuszcza `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="fb0dd-568">Jest to istotne, czy transfer kontroli występuje w wyniku normalnego wykonania, w wyniku wykonania `break`, `continue`, `goto`, lub `return` instrukcji, lub w wyniku propagowanie wyjątku z `try` Instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="fb0dd-569">Jeśli wyjątek zostanie zgłoszony podczas wykonywania `finally` zablokować, a nie przechwycono w tym samym bloku finally, wyjątek jest propagowany do następnego otaczający `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="fb0dd-570">Jeśli inny wyjątek w trakcie propagowanie, ten wyjątek zostanie utracony.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="fb0dd-571">Proces propagowanie wyjątku jest omówiona w opisie `throw` — instrukcja ([instrukcji "throw"](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="fb0dd-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="fb0dd-572">`try` Bloku `try` instrukcja jest osiągalny Jeśli `try` instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="fb0dd-573">A `catch` bloku `try` instrukcja jest osiągalny Jeśli `try` instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="fb0dd-574">`finally` Bloku `try` instrukcja jest osiągalny Jeśli `try` instrukcji jest nieosiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="fb0dd-575">Punkt końcowy `try` instrukcja jest osiągalna, jeśli są spełnione oba z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="fb0dd-576">Punkt końcowy `try` bloku jest dostępny, lub koniec punktu z co najmniej jeden `catch` bloku jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="fb0dd-577">Jeśli `finally` bloku jest obecny, punkt końcowy `finally` bloku jest osiągalny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="fb0dd-578">Checked i unchecked instrukcji</span><span class="sxs-lookup"><span data-stu-id="fb0dd-578">The checked and unchecked statements</span></span>

<span data-ttu-id="fb0dd-579">`checked` i `unchecked` instrukcje służą do sterowania ***przepełnienia, sprawdzając kontekst*** typu całkowitego operacje arytmetyczne i konwersje.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="fb0dd-580">`checked` Instrukcji powoduje, że wszystkie wyrażenia w *bloku* mogło zostać ocenione w kontekście sprawdzanym i `unchecked` instrukcji powoduje, że wszystkie wyrażenia w *bloku* mogło zostać ocenione kontekst niezaznaczone.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="fb0dd-581">`checked` i `unchecked` instrukcje są dokładnie odpowiednikiem `checked` i `unchecked` operatorów ([operatory checked i unchecked](expressions.md#the-checked-and-unchecked-operators)), z tą różnicą, że działają one na blokach zamiast wyrażenia .</span><span class="sxs-lookup"><span data-stu-id="fb0dd-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="fb0dd-582">Instrukcji "lock"</span><span class="sxs-lookup"><span data-stu-id="fb0dd-582">The lock statement</span></span>

<span data-ttu-id="fb0dd-583">`lock` Instrukcji uzyskuje blokadę wykluczania wzajemnego dla danego obiektu, wykonuje instrukcję tak długo, a następnie zwalnia blokadę.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="fb0dd-584">Wyrażenie `lock` instrukcja musi oznaczają wartości typu, znane jako *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="fb0dd-585">Brak konwersji niejawnej konwersji boxing ([konwersje Boxing](conversions.md#boxing-conversions)) nigdy nie jest wykonywane dla wyrażenie `lock` instrukcji, a więc jest to błąd kompilacji, wyrażenia wskazać wartość *value_type*.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="fb0dd-586">A `lock` instrukcji formularza</span><span class="sxs-lookup"><span data-stu-id="fb0dd-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="fb0dd-587">gdzie `x` to wyrażenie *reference_type*, odpowiada dokładnie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="fb0dd-588">z tą różnicą, że `x` jest obliczany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="fb0dd-589">Natomiast przechowywane blokadę wykluczania wzajemnego kodu wykonywanego w tym samym wątku wykonywania można również uzyskać i zwalnia blokadę.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="fb0dd-590">Jednak kodu wykonywanego w innych wątków jest zablokowany rezygnację z włączenia blokady dopóki blokada jest zwalniana.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="fb0dd-591">Blokowanie `System.Type` obiektów w celu synchronizowania dostępu do danych statycznych nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="fb0dd-592">Inny kod może zostać zablokowany na tym samym typie, co może skutkować zakleszczenia.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="fb0dd-593">Lepszym rozwiązaniem jest do synchronizowania dostępu do danych statycznych, blokując prywatnego obiektu statycznego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="fb0dd-594">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="fb0dd-595">Instrukcja using</span><span class="sxs-lookup"><span data-stu-id="fb0dd-595">The using statement</span></span>

<span data-ttu-id="fb0dd-596">`using` Instrukcji uzyskuje co najmniej jeden zasób, wykonuje instrukcję, a następnie usuwa zasób.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="fb0dd-597">A ***zasobów*** jest klasa lub struktura, która implementuje `System.IDisposable`, która obejmuje pojedynczej metody bez parametrów o nazwie `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="fb0dd-598">Kod, który używa zasobu może wywołać `Dispose` do wskazania, że zasób nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="fb0dd-599">Jeśli `Dispose` nie jest wywoływana, a następnie automatyczne usuwanie ostatecznie występuje w wyniku wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="fb0dd-600">Jeśli formie *resource_acquisition* jest *local_variable_declaration* następnie typ *local_variable_declaration* musi być albo `dynamic` lub typu który może być niejawnie konwertowane na `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="fb0dd-601">Jeśli formie *resource_acquisition* jest *wyrażenie* , a następnie to wyrażenie musi umożliwiać niejawną konwersję `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="fb0dd-602">Zmienne lokalne są deklarowane w *resource_acquisition* są przeznaczone tylko do odczytu i może zawierać inicjatora.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="fb0dd-603">Występuje błąd kompilacji, jeśli osadzona instrukcja próbuje zmodyfikować te zmienne lokalne (za pośrednictwem przydziału lub `++` i `--` operatory), należy przyjąć adresu je lub przekazać je jako `ref` lub `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="fb0dd-604">A `using` instrukcji jest tłumaczony na trzy części: pozyskiwanie, użycia i usuwania.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="fb0dd-605">Użycie zasobów jest niejawnie ujęty w `try` instrukcję, która obejmuje `finally` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="fb0dd-606">To `finally` klauzuli Usuwa zasób.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="fb0dd-607">Jeśli `null` nabywa zasobów, następnie Brak wywołania `Dispose` jest wykonywane, i jest zgłaszany żaden wyjątek.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="fb0dd-608">Jeśli zasób jest typu `dynamic` konwersją dynamicznie za pośrednictwem niejawną konwersję dynamicznej ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions)) do `IDisposable` podczas pozyskiwania w celu zapewnienia, że konwersja została się powodzeniem przed rozpoczęciem użytkowania i usuwania.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="fb0dd-609">A `using` instrukcji formularza</span><span class="sxs-lookup"><span data-stu-id="fb0dd-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="fb0dd-610">odnosi się do jednej z trzech możliwych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="fb0dd-611">Gdy `ResourceType` jest typem wartości niedopuszczającym wartości jest rozszerzenie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="fb0dd-612">W przeciwnym razie, gdy `ResourceType` jest typem wartościowym lub typem referencyjnym, inne niż `dynamic`, jest rozszerzenie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="fb0dd-613">W przeciwnym razie, gdy `ResourceType` jest `dynamic`, jest rozszerzenie</span><span class="sxs-lookup"><span data-stu-id="fb0dd-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="fb0dd-614">W obu rozwinięciu `resource` zmienna jest tylko do odczytu w osadzonych instrukcji i `d` zmienna jest niedostępna z powodu i niewidoczności, aż do osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="fb0dd-615">Implementacja jest dozwolony do zaimplementowania danego przy użyciu instrukcji w różny sposób, np. ze względu na wydajność, tak długo, jak zachowanie jest zgodne z rozszerzeniem powyżej.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="fb0dd-616">A `using` instrukcji formularza</span><span class="sxs-lookup"><span data-stu-id="fb0dd-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="fb0dd-617">ma ten sam trzy możliwe rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-617">has the same three possible expansions.</span></span> <span data-ttu-id="fb0dd-618">W tym przypadku `ResourceType` jest niejawnie typ kompilacji `expression`, jeśli taki istnieje.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="fb0dd-619">W przeciwnym razie interfejsu `IDisposable` sam jest używany jako `ResourceType`.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="fb0dd-620">`resource` Zmienna jest niedostępna z powodu i niewidoczności, aż do osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="fb0dd-621">Gdy *resource_acquisition* mają postać *local_variable_declaration*, istnieje możliwość nabywanie wielu zasobów danego typu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="fb0dd-622">A `using` instrukcji formularza</span><span class="sxs-lookup"><span data-stu-id="fb0dd-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="fb0dd-623">jest zagnieżdżona dokładnie równoważna sekwencji `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="fb0dd-624">Poniższy przykład tworzy plik o nazwie `log.txt` i zapisuje dwa wiersze tekstu do pliku.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="fb0dd-625">Przykład następnie spowoduje otwarcie tego samego pliku do odczytu i kopiuje zawartej wierszy tekstu do konsoli.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="fb0dd-626">Ponieważ `TextWriter` i `TextReader` implementacji klasy `IDisposable` interfejsu, można użyć w przykładzie `using` instrukcje, aby upewnić się, że pliku podstawowego jest poprawnie zamknięte po zapisu lub operacje odczytu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="fb0dd-627">Instrukcja yield</span><span class="sxs-lookup"><span data-stu-id="fb0dd-627">The yield statement</span></span>

<span data-ttu-id="fb0dd-628">`yield` Instrukcja jest używane w bloku iteratora ([bloki](statements.md#blocks)) umożliwiające uzyskanie wartości do obiektu modułu wyliczającego ([obiektów modułu wyliczającego](classes.md#enumerator-objects)) lub obiekt wyliczalny ([Wyliczalny obiektów](classes.md#enumerable-objects)) iterator lub w celu sygnalizowania, że końcem iteracji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="fb0dd-629">`yield` nie jest zarezerwowanym słowem ma specjalne znaczenie tylko wtedy, gdy jest używana bezpośrednio przed `return` lub `break` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="fb0dd-630">W innych kontekstach `yield` można użyć jako identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="fb0dd-631">Istnieje kilka ograniczeń o tym, gdzie `yield` instrukcji może znajdować się, jak opisano w następujących.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="fb0dd-632">Jest to błąd czasu kompilacji dla `yield` instrukcji (Dowolna forma) można znajdować się poza *method_body*, *operator_body* lub *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="fb0dd-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="fb0dd-633">Jest to błąd czasu kompilacji dla `yield` instrukcji (Dowolna forma) się wewnątrz funkcji anonimowych.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="fb0dd-634">Jest to błąd czasu kompilacji dla `yield` instrukcji (Dowolna forma) pojawią się w `finally` klauzuli `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="fb0dd-635">Jest to błąd czasu kompilacji dla `yield return` instrukcję, aby występować w dowolnym miejscu `try` instrukcji, która zawiera dowolne `catch` klauzul.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="fb0dd-636">Poniższy przykład pokazuje niektóre prawidłowe i nieprawidłowego użycia `yield` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="fb0dd-637">Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia w `yield return` instrukcję, aby typ yield ([uzyskanie typu](classes.md#yield-type)) iteratora.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="fb0dd-638">A `yield return` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-639">Wyrażenie w instrukcji jest obliczane, niejawnie konwertowany na typ produktywności i przypisane do `Current` właściwości obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="fb0dd-640">Wykonanie bloku iteratora jest zawieszone.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="fb0dd-641">Jeśli `yield return` Instrukcja znajduje się w co najmniej jeden `try` blokuje skojarzonego `finally` bloki nie są wykonywane w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="fb0dd-642">`MoveNext` Metoda obiekt modułu wyliczającego zwraca `true` do obiektu wywołującego, wskazujący, że obiekt modułu wyliczającego pomyślnie zaawansowane do następnego elementu.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="fb0dd-643">Następne wywołanie obiekt modułu wyliczającego `MoveNext` metoda wznawia wykonanie bloku iteratora, z którym ostatnio zostało wstrzymane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="fb0dd-644">A `yield break` instrukcja jest wykonywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fb0dd-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="fb0dd-645">Jeśli `yield break` instrukcji jest ujęta w co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="fb0dd-646">Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="fb0dd-647">Ten proces jest powtarzany do momentu `finally` blokuje wszystkie otaczającej `try` instrukcje zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="fb0dd-648">Formant zostaje zwrócony do obiektu wywołującego, bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="fb0dd-649">Jest to `MoveNext` metody lub `Dispose` metody obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="fb0dd-650">Ponieważ `yield break` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `yield break` instrukcji nigdy nie jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="fb0dd-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
