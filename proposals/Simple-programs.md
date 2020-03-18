---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "79485225"
---
# <a name="simple-programs"></a><span data-ttu-id="24e63-101">Proste programy</span><span class="sxs-lookup"><span data-stu-id="24e63-101">Simple programs</span></span>

* <span data-ttu-id="24e63-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="24e63-102">[x] Proposed</span></span>
* <span data-ttu-id="24e63-103">[x] prototyp: uruchomiono</span><span class="sxs-lookup"><span data-stu-id="24e63-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="24e63-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="24e63-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="24e63-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="24e63-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="24e63-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="24e63-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="24e63-107">Zezwalaj na wykonywanie sekwencji *instrukcji* bezpośrednio przed *namespace_member_declaration*s *compilation_unit* (tj. plik źródłowy).</span><span class="sxs-lookup"><span data-stu-id="24e63-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="24e63-108">Semantyką jest to, że jeśli taka sekwencja *instrukcji* jest obecna, zostanie wyemitowana następująca deklaracja typu, modulo rzeczywistą nazwę typu i nazwę metody.</span><span class="sxs-lookup"><span data-stu-id="24e63-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="24e63-109">Zobacz również https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="24e63-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="24e63-110">Motywacją</span><span class="sxs-lookup"><span data-stu-id="24e63-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="24e63-111">Istnieje pewna ilość zwyczajowa otaczająca nawet najprostszych programów, z powodu potrzeby jawnej metody `Main`.</span><span class="sxs-lookup"><span data-stu-id="24e63-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="24e63-112">Jest to zgodne z sposobem uczenia się i programowania w języku.</span><span class="sxs-lookup"><span data-stu-id="24e63-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="24e63-113">W związku z tym głównym celem tej funkcji jest umożliwienie C# programom, które nie są niepotrzebnymi standardami, w celu uzyskania informacji na temat użytkowników i przejrzystości kodu.</span><span class="sxs-lookup"><span data-stu-id="24e63-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="24e63-114">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="24e63-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="24e63-115">Składnia</span><span class="sxs-lookup"><span data-stu-id="24e63-115">Syntax</span></span>

<span data-ttu-id="24e63-116">Jedyną dodatkową składnią jest umożliwienie sekwencji *instrukcji*s w jednostce kompilacji tuż przed *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="24e63-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="24e63-117">We wszystkich *compilation_unit* *instrukcja*s musi być wszystkie deklaracje funkcji lokalnych.</span><span class="sxs-lookup"><span data-stu-id="24e63-117">In all but one *compilation_unit* the *statement*s must all be local function declarations.</span></span> 

<span data-ttu-id="24e63-118">Przykład:</span><span class="sxs-lookup"><span data-stu-id="24e63-118">Example:</span></span>

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="24e63-119">Semantyki</span><span class="sxs-lookup"><span data-stu-id="24e63-119">Semantics</span></span>

<span data-ttu-id="24e63-120">Jeśli jakiekolwiek instrukcje najwyższego poziomu są obecne w dowolnej jednostce kompilacji programu, znaczenie jest tak samo, jakby były łączone w treści bloku metody `Main` klasy `Program` w globalnej przestrzeni nazw, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="24e63-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

<span data-ttu-id="24e63-121">Należy zauważyć, że nazwy "program" i "Main" są używane tylko w celach ilustracji, rzeczywiste nazwy używane przez kompilator są zależne od implementacji, a nie typu ani nie można odwoływać się do niego przy użyciu nazwy z kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="24e63-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="24e63-122">Metoda jest oznaczona jako punkt wejścia programu.</span><span class="sxs-lookup"><span data-stu-id="24e63-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="24e63-123">Jawnie zadeklarowane metody, które Konwencją mogą być traktowane jako kandydatów punktu wejścia są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="24e63-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="24e63-124">Ostrzeżenie jest zgłaszane, gdy wystąpi.</span><span class="sxs-lookup"><span data-stu-id="24e63-124">A warning is reported when that happens.</span></span> <span data-ttu-id="24e63-125">Wystąpił błąd podczas określania przełącznika kompilatora `-main:<type>`.</span><span class="sxs-lookup"><span data-stu-id="24e63-125">It is an error to specify `-main:<type>` compiler switch.</span></span>

<span data-ttu-id="24e63-126">Jeśli jedna jednostka kompilacji ma instrukcje inne niż deklaracje funkcji lokalnych, instrukcje od tej jednostki kompilacji są wykonywane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="24e63-126">If any one compilation unit has statements other than local function declarations, statements from that compilation unit occur first.</span></span> <span data-ttu-id="24e63-127">Powoduje to, że jest to dozwolone w przypadku funkcji lokalnych w jednym pliku, aby odwoływać się do zmiennych lokalnych w innym.</span><span class="sxs-lookup"><span data-stu-id="24e63-127">This causes it to be legal for local functions in one file to reference local variables in another.</span></span> <span data-ttu-id="24e63-128">Kolejność przydziałów instrukcji (które wszystkie będą funkcjami lokalnymi) z innych jednostek kompilacji jest niezdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="24e63-128">The order of statement contributions (which would all be local functions) from other compilation units is undefined.</span></span>

<span data-ttu-id="24e63-129">Operacje asynchroniczne są dozwolone w instrukcjach najwyższego poziomu do zakresu, w którym są dozwolone w instrukcjach w ramach zwykłej asynchronicznej metody punktu wejścia.</span><span class="sxs-lookup"><span data-stu-id="24e63-129">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="24e63-130">Jednak nie są one wymagane, jeśli `await` wyrażenia i inne operacje asynchroniczne są pomijane, żadne ostrzeżenie nie zostanie wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="24e63-130">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="24e63-131">Zamiast tego sygnatura wygenerowanej metody punktu wejścia jest równoważna z</span><span class="sxs-lookup"><span data-stu-id="24e63-131">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="24e63-132">W powyższym przykładzie zostanie przedstawiony następujący `$Main` deklaracji metody:</span><span class="sxs-lookup"><span data-stu-id="24e63-132">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="24e63-133">W tym samym czasie przykład podobny do tego:</span><span class="sxs-lookup"><span data-stu-id="24e63-133">At the same time an example like this:</span></span>
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="24e63-134">da:</span><span class="sxs-lookup"><span data-stu-id="24e63-134">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="24e63-135">Zakres zmiennych lokalnych najwyższego poziomu i lokalnych funkcji</span><span class="sxs-lookup"><span data-stu-id="24e63-135">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="24e63-136">Mimo że lokalne zmienne i funkcje najwyższego poziomu są "opakowane" do wygenerowanej metody punktu wejścia, nadal powinny znajdować się w zakresie w całym programie.</span><span class="sxs-lookup"><span data-stu-id="24e63-136">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program.</span></span>
<span data-ttu-id="24e63-137">Na potrzeby oceny prostej nazwy, gdy zostanie osiągnięta globalna przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="24e63-137">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="24e63-138">Najpierw podjęto próbę oszacowania nazwy w wygenerowanej metodzie punktu wejścia i tylko wtedy, gdy ta próba nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="24e63-138">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="24e63-139">Trwa wykonywanie "regularnej" oceny w globalnej deklaracji przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="24e63-139">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="24e63-140">Może to prowadzić do przesłania nazw i typów zadeklarowanych w globalnej przestrzeni nazw oraz do przesłaniania zaimportowanych nazw.</span><span class="sxs-lookup"><span data-stu-id="24e63-140">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="24e63-141">Jeśli prosta Ocena nazw występuje poza instrukcjami najwyższego poziomu, a Ocena zwraca zmienną lokalną lub funkcję najwyższego poziomu, która powinna prowadzić do błędu.</span><span class="sxs-lookup"><span data-stu-id="24e63-141">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="24e63-142">W ten sposób chronimy naszą przyszłą możliwość lepszego korzystania z "funkcji najwyższego poziomu" (scenariusz 2 w https://github.com/dotnet/csharplang/issues/3117)i że można zapewnić przydatną diagnostykę użytkownikom, którzy w niedrogi sposób uważają je za obsługiwaną.</span><span class="sxs-lookup"><span data-stu-id="24e63-142">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

