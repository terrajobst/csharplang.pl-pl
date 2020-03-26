---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281947"
---
# <a name="simple-programs"></a><span data-ttu-id="a5da0-101">Proste programy</span><span class="sxs-lookup"><span data-stu-id="a5da0-101">Simple programs</span></span>

* <span data-ttu-id="a5da0-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="a5da0-102">[x] Proposed</span></span>
* <span data-ttu-id="a5da0-103">[x] prototyp: uruchomiono</span><span class="sxs-lookup"><span data-stu-id="a5da0-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="a5da0-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="a5da0-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="a5da0-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="a5da0-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="a5da0-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a5da0-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a5da0-107">Zezwalaj na wykonywanie sekwencji *instrukcji* bezpośrednio przed *namespace_member_declaration*s *compilation_unit* (tj. plik źródłowy).</span><span class="sxs-lookup"><span data-stu-id="a5da0-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="a5da0-108">Semantyką jest to, że jeśli taka sekwencja *instrukcji* jest obecna, zostanie wyemitowana następująca deklaracja typu, modulo rzeczywistą nazwę typu i nazwę metody.</span><span class="sxs-lookup"><span data-stu-id="a5da0-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="a5da0-109">Zobacz również https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="a5da0-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="a5da0-110">Motywacją</span><span class="sxs-lookup"><span data-stu-id="a5da0-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a5da0-111">Istnieje pewna ilość zwyczajowa otaczająca nawet najprostszych programów, z powodu potrzeby jawnej metody `Main`.</span><span class="sxs-lookup"><span data-stu-id="a5da0-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="a5da0-112">Jest to zgodne z sposobem uczenia się i programowania w języku.</span><span class="sxs-lookup"><span data-stu-id="a5da0-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="a5da0-113">W związku z tym głównym celem tej funkcji jest umożliwienie C# programom, które nie są niepotrzebnymi standardami, w celu uzyskania informacji na temat użytkowników i przejrzystości kodu.</span><span class="sxs-lookup"><span data-stu-id="a5da0-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a5da0-114">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="a5da0-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="a5da0-115">Składnia</span><span class="sxs-lookup"><span data-stu-id="a5da0-115">Syntax</span></span>

<span data-ttu-id="a5da0-116">Jedyną dodatkową składnią jest umożliwienie sekwencji *instrukcji*s w jednostce kompilacji tuż przed *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="a5da0-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="a5da0-117">Tylko jeden *compilation_unit* może mieć *instrukcję*s.</span><span class="sxs-lookup"><span data-stu-id="a5da0-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="a5da0-118">Przykład:</span><span class="sxs-lookup"><span data-stu-id="a5da0-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="a5da0-119">Semantyki</span><span class="sxs-lookup"><span data-stu-id="a5da0-119">Semantics</span></span>

<span data-ttu-id="a5da0-120">Jeśli jakiekolwiek instrukcje najwyższego poziomu są obecne w dowolnej jednostce kompilacji programu, znaczenie jest tak samo, jakby były łączone w treści bloku metody `Main` klasy `Program` w globalnej przestrzeni nazw, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a5da0-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="a5da0-121">Należy zauważyć, że nazwy "program" i "Main" są używane tylko w celach ilustracji, rzeczywiste nazwy używane przez kompilator są zależne od implementacji, a nie typu ani nie można odwoływać się do niego przy użyciu nazwy z kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="a5da0-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="a5da0-122">Metoda jest oznaczona jako punkt wejścia programu.</span><span class="sxs-lookup"><span data-stu-id="a5da0-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="a5da0-123">Jawnie zadeklarowane metody, które Konwencją mogą być traktowane jako kandydatów punktu wejścia są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="a5da0-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="a5da0-124">Ostrzeżenie jest zgłaszane, gdy wystąpi.</span><span class="sxs-lookup"><span data-stu-id="a5da0-124">A warning is reported when that happens.</span></span> <span data-ttu-id="a5da0-125">Wystąpił błąd podczas określania przełącznika kompilatora `-main:<type>`, gdy istnieją instrukcje najwyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="a5da0-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="a5da0-126">Operacje asynchroniczne są dozwolone w instrukcjach najwyższego poziomu do zakresu, w którym są dozwolone w instrukcjach w ramach zwykłej asynchronicznej metody punktu wejścia.</span><span class="sxs-lookup"><span data-stu-id="a5da0-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="a5da0-127">Jednak nie są one wymagane, jeśli `await` wyrażenia i inne operacje asynchroniczne są pomijane, żadne ostrzeżenie nie zostanie wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="a5da0-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="a5da0-128">Zamiast tego sygnatura wygenerowanej metody punktu wejścia jest równoważna z</span><span class="sxs-lookup"><span data-stu-id="a5da0-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="a5da0-129">W powyższym przykładzie zostanie przedstawiony następujący `$Main` deklaracji metody:</span><span class="sxs-lookup"><span data-stu-id="a5da0-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="a5da0-130">W tym samym czasie przykład podobny do tego:</span><span class="sxs-lookup"><span data-stu-id="a5da0-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="a5da0-131">da:</span><span class="sxs-lookup"><span data-stu-id="a5da0-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="a5da0-132">Zakres zmiennych lokalnych najwyższego poziomu i lokalnych funkcji</span><span class="sxs-lookup"><span data-stu-id="a5da0-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="a5da0-133">Mimo że lokalne zmienne i funkcje najwyższego poziomu są "opakowane" do wygenerowanej metody punktu wejścia, nadal powinny one znajdować się w zakresie w całym programie w każdej jednostce kompilacji.</span><span class="sxs-lookup"><span data-stu-id="a5da0-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="a5da0-134">Na potrzeby oceny prostej nazwy, gdy zostanie osiągnięta globalna przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="a5da0-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="a5da0-135">Najpierw podjęto próbę oszacowania nazwy w wygenerowanej metodzie punktu wejścia i tylko wtedy, gdy ta próba nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="a5da0-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="a5da0-136">Trwa wykonywanie "regularnej" oceny w globalnej deklaracji przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="a5da0-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="a5da0-137">Może to prowadzić do przesłania nazw i typów zadeklarowanych w globalnej przestrzeni nazw oraz do przesłaniania zaimportowanych nazw.</span><span class="sxs-lookup"><span data-stu-id="a5da0-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="a5da0-138">Jeśli prosta Ocena nazw występuje poza instrukcjami najwyższego poziomu, a Ocena zwraca zmienną lokalną lub funkcję najwyższego poziomu, która powinna prowadzić do błędu.</span><span class="sxs-lookup"><span data-stu-id="a5da0-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="a5da0-139">W ten sposób chronimy naszą przyszłą możliwość lepszego korzystania z "funkcji najwyższego poziomu" (scenariusz 2 w https://github.com/dotnet/csharplang/issues/3117)i że można zapewnić przydatną diagnostykę użytkownikom, którzy w niedrogi sposób uważają je za obsługiwaną.</span><span class="sxs-lookup"><span data-stu-id="a5da0-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

