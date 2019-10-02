---
ms.openlocfilehash: 300d5fc2a2fadd98472d73c122226146605b01dd
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703998"
---
# <a name="introduction"></a><span data-ttu-id="51ac0-101">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="51ac0-101">Introduction</span></span>

<span data-ttu-id="51ac0-102">C#(wymawiane "Zobacz Sharp") to prosty, nowoczesny, zorientowany obiektowo i bezpieczny dla typu język programowania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="51ac0-103">C#ma swoje elementy główne w rodzinie C i będzie od razu znane programistom języków C, C++i Java.</span><span class="sxs-lookup"><span data-stu-id="51ac0-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="51ac0-104">C#jest ustandaryzowany przez ECMA International jako standard ***ECMA-334*** i ISO/IEC jako standard ***iso/IEC 23270*** .</span><span class="sxs-lookup"><span data-stu-id="51ac0-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="51ac0-105">C# Kompilator firmy Microsoft dla .NET Framework jest zgodny z implementacją obu tych standardów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="51ac0-106">C#jest językiem zorientowanym na obiekt, ale C# dodatkowo obsługuje programowanie ***zorientowane na składniki*** .</span><span class="sxs-lookup"><span data-stu-id="51ac0-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="51ac0-107">Współczesny projekt oprogramowania jest coraz bardziej oparty na składnikach oprogramowania w postaci samodzielnych i samodzielnych pakietów funkcji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="51ac0-108">Klucz do takich składników polega na tym, że stanowią one model programowania z właściwościami, metodami i zdarzeniami; mają one atrybuty, które dostarczają deklaracyjne informacje o składniku; i zawierają własną dokumentację.</span><span class="sxs-lookup"><span data-stu-id="51ac0-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="51ac0-109">C#Program udostępnia konstrukcje językowe umożliwiające bezpośrednie wsparcie tych koncepcji, C# co sprawia, że jest to bardzo naturalny język, w którym można tworzyć i używać składników oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="51ac0-110">Niektóre C# funkcje ułatwiają tworzenie niezawodnych i trwałych aplikacji: ***Wyrzucanie elementów bezużytecznych*** automatycznie odzyskuje pamięć zajmowaną przez nieużywane obiekty; ***Obsługa wyjątków*** zapewnia strukturalne i rozszerzalne podejście do wykrywania błędów i odzyskiwania. a ***bezpieczny typ*** projektu języka sprawia, że nie można odczytać z niezainicjowanych zmiennych, indeksować tablice poza granicami lub wykonywać rzutowania typu unchecked.</span><span class="sxs-lookup"><span data-stu-id="51ac0-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="51ac0-111">C#ma ***ujednolicony system typów***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="51ac0-112">Wszystkie C# typy, w tym typy pierwotne `int` , `double`takie jak i, dziedziczą `object` z jednego typu głównego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="51ac0-113">W związku z tym wszystkie typy używają zestawu typowych operacji, a wartości dowolnego typu mogą być przechowywane, transportowane i obsługiwane w spójny sposób.</span><span class="sxs-lookup"><span data-stu-id="51ac0-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="51ac0-114">Ponadto C# obsługuje zarówno typy odwołań zdefiniowane przez użytkownika, jak i typy wartości, co umożliwia dynamiczne przydzielanie obiektów oraz przechowywanie w wierszu lekkich struktur.</span><span class="sxs-lookup"><span data-stu-id="51ac0-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="51ac0-115">Aby upewnić C# się, że programy i biblioteki mogą się rozwijać w czasie w zgodnym zakresie, przeznaczenie ma duże znaczenie C#w projekcie ***wersji*** .</span><span class="sxs-lookup"><span data-stu-id="51ac0-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="51ac0-116">W wielu językach programowania jest mało uwagi na ten problem, a w efekcie programy w tych językach są częściej częściej niż to konieczne, gdy są wprowadzane nowsze wersje bibliotek zależnych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="51ac0-117">C#Zagadnienia dotyczące projektu, które miały bezpośredni wpływ na wersje, obejmują oddzielność `virtual` i `override` modyfikatory, reguły rozpoznawania przeciążania metod oraz obsługę jawnych deklaracji elementów członkowskich interfejsu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="51ac0-118">Pozostała część tego rozdziału zawiera opis najważniejszych funkcji C# języka.</span><span class="sxs-lookup"><span data-stu-id="51ac0-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="51ac0-119">Chociaż dalsze rozdziały opisują reguły i wyjątki w szczegółowo zorientowanym i niekiedy w sposób matematyczny, ten rozdział dąży do przejrzystości i zwięzłości na koszt kompletności.</span><span class="sxs-lookup"><span data-stu-id="51ac0-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="51ac0-120">Celem jest zapewnienie czytelnikowi wprowadzenia do języka, który ułatwia pisanie wczesnych programów i odczytywanie dalszych rozdziałów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="51ac0-121">Hello world</span><span class="sxs-lookup"><span data-stu-id="51ac0-121">Hello world</span></span>

<span data-ttu-id="51ac0-122">Program "Hello, World" jest tradycyjnie używany do wprowadzania języka programowania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="51ac0-123">W tym miejscu znajduje C#się w:</span><span class="sxs-lookup"><span data-stu-id="51ac0-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="51ac0-124">C#pliki źródłowe mają zwykle rozszerzenie `.cs`pliku.</span><span class="sxs-lookup"><span data-stu-id="51ac0-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="51ac0-125">Przy założeniu, że program "Hello, World" jest przechowywany w `hello.cs`pliku, program można skompilować za pomocą kompilatora firmy C# Microsoft przy użyciu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="51ac0-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```console
csc hello.cs
```
<span data-ttu-id="51ac0-126">tworzy zestaw wykonywalny o nazwie `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="51ac0-127">Dane wyjściowe generowane przez tę aplikację po jej uruchomieniu to</span><span class="sxs-lookup"><span data-stu-id="51ac0-127">The output produced by this application when it is run is</span></span>
```console
Hello, World
```

<span data-ttu-id="51ac0-128">Program "Hello, World" rozpoczyna się `using` od dyrektywy, która odwołuje się do `System` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="51ac0-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="51ac0-129">Przestrzenie nazw zapewniają hierarchiczną metodę C# organizowania programów i bibliotek.</span><span class="sxs-lookup"><span data-stu-id="51ac0-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="51ac0-130">Przestrzenie nazw zawierają typy i inne przestrzenie nazw — `System` na przykład przestrzeń nazw zawiera wiele typów, takich `Console` jak Klasa, do której odwołuje się program, oraz liczba innych przestrzeni nazw, `IO` takich `Collections`jak i.</span><span class="sxs-lookup"><span data-stu-id="51ac0-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="51ac0-131">`using` Dyrektywa odwołująca się do danej przestrzeni nazw umożliwia niekwalifikowane użycie typów, które są elementami członkowskimi tej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="51ac0-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="51ac0-132">Ze względu `using` na dyrektywę program może użyć `Console.WriteLine` jako skrótu dla elementu `System.Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="51ac0-133">Klasa zadeklarowana przez program "Hello, World" ma jeden element członkowski, Metoda o nazwie `Main`. `Hello`</span><span class="sxs-lookup"><span data-stu-id="51ac0-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="51ac0-134">Metoda jest zadeklarowana `static` z modyfikatorem. `Main`</span><span class="sxs-lookup"><span data-stu-id="51ac0-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="51ac0-135">Chociaż metody wystąpień mogą odwoływać się do określonego wystąpienia obiektu otaczającego za `this`pomocą słowa kluczowego, metody statyczne działają bez odwołania do określonego obiektu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="51ac0-136">Zgodnie z Konwencją metoda statyczna `Main` o nazwie służy jako punkt wejścia programu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="51ac0-137">Dane wyjściowe programu są tworzone przez `WriteLine` metodę `Console` klasy w `System` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="51ac0-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="51ac0-138">Ta klasa jest dostarczana przez .NET Framework biblioteki klas, które domyślnie są przywoływane przez kompilator firmy Microsoft C# .</span><span class="sxs-lookup"><span data-stu-id="51ac0-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="51ac0-139">Należy zauważyć C# , że sama nie ma oddzielnej biblioteki środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="51ac0-140">Zamiast tego .NET Framework jest biblioteką środowiska uruchomieniowego C#programu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="51ac0-141">Struktura programu</span><span class="sxs-lookup"><span data-stu-id="51ac0-141">Program structure</span></span>

<span data-ttu-id="51ac0-142">Kluczowe koncepcje organizacyjne C# w programie to ***programy***, ***przestrzenie nazw***, ***typy***, ***elementy członkowskie***i ***zestawy***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="51ac0-143">C#programy składają się z co najmniej jednego pliku źródłowego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="51ac0-144">Programy deklarują typy, które zawierają składowe i mogą być zorganizowane w przestrzenie nazw.</span><span class="sxs-lookup"><span data-stu-id="51ac0-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="51ac0-145">Klasy i interfejsy są przykładami typów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="51ac0-146">Pola, metody, właściwości i zdarzenia są przykładami elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="51ac0-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="51ac0-147">Gdy C# programy są kompilowane, są fizycznie spakowane do zestawów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="51ac0-148">Zestawy `.exe` zazwyczaj mają rozszerzenie pliku lub `.dll`, w zależności od tego, czy implementują ***aplikacje*** lub ***biblioteki***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="51ac0-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="51ac0-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="51ac0-150">deklaruje klasę o `Stack` nazwie w przestrzeni nazw `Acme.Collections`o nazwie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="51ac0-151">W pełni kwalifikowana nazwa tej klasy `Acme.Collections.Stack`to.</span><span class="sxs-lookup"><span data-stu-id="51ac0-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="51ac0-152">Klasa zawiera kilka elementów członkowskich: pole o nazwie `top`, dwie metody o `Push` nazwie `Pop`i i zagnieżdżoną klasę o `Entry`nazwie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="51ac0-153">Klasa dodatkowo zawiera trzy elementy członkowskie: pole o nazwie `next`, pole o nazwie `data`i Konstruktor. `Entry`</span><span class="sxs-lookup"><span data-stu-id="51ac0-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="51ac0-154">Przy założeniu, że kod źródłowy przykładu jest przechowywany w pliku `acme.cs`, wiersz polecenia</span><span class="sxs-lookup"><span data-stu-id="51ac0-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```console
csc /t:library acme.cs
```
<span data-ttu-id="51ac0-155">kompiluje przykład jako bibliotekę (kod bez `Main` punktu wejścia) i tworzy zestaw o nazwie. `acme.dll`</span><span class="sxs-lookup"><span data-stu-id="51ac0-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="51ac0-156">Zestawy zawierają kod wykonywalny w postaci instrukcji ***języka pośredniego*** (IL) i informacji symbolicznych w formie ***metadanych***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="51ac0-157">Przed wykonaniem kod IL w zestawie jest automatycznie konwertowany na kod specyficzny dla procesora przez kompilator just-in-Time (JIT) środowiska uruchomieniowego języka wspólnego platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="51ac0-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="51ac0-158">Ponieważ zestaw jest samoopisującą się jednostką funkcji, która zawiera kod i metadane, nie ma potrzeby stosowania `#include` dyrektyw i plików nagłówkowych w programie. C#</span><span class="sxs-lookup"><span data-stu-id="51ac0-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="51ac0-159">Typy publiczne i składowe zawarte w określonym zestawie są udostępniane w C# programie po prostu przez odwołanie się do tego zestawu podczas kompilowania programu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="51ac0-160">Na przykład ten program używa `Acme.Collections.Stack` klasy `acme.dll` z zestawu:</span><span class="sxs-lookup"><span data-stu-id="51ac0-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="51ac0-161">Jeśli program jest `test.cs`przechowywany w pliku, po `test.cs` skompilowaniu `acme.dll` `/r` zestawu można odwoływać się za pomocą opcji kompilatora:</span><span class="sxs-lookup"><span data-stu-id="51ac0-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```console
csc /r:acme.dll test.cs
```
<span data-ttu-id="51ac0-162">Spowoduje to utworzenie zestawu wykonywalnego `test.exe`o nazwie, który w przypadku uruchamiania generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="51ac0-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```console
100
10
1
```
<span data-ttu-id="51ac0-163">C#zezwala na przechowywanie tekstu źródłowego programu w kilku plikach źródłowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="51ac0-164">W przypadku kompilowania programu C# z obsługą wielu plików wszystkie pliki źródłowe są przetwarzane razem, a pliki źródłowe mogą swobodnie odwoływać się do siebie nawzajem — jest to tak samo, jakby wszystkie pliki źródłowe zostały połączone w jeden duży plik przed przetworzeniem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="51ac0-165">Deklaracje przesyłania dalej nie są C# nigdy niewymagane w programie, w związku z czym z kilkoma wyjątkami, zamówienie deklaracji jest nieważne.</span><span class="sxs-lookup"><span data-stu-id="51ac0-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="51ac0-166">C#nie ogranicza pliku źródłowego do deklarowania tylko jednego typu publicznego ani nie wymaga, aby nazwa pliku źródłowego była zgodna z typem zadeklarowanym w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="51ac0-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="51ac0-167">Typy i zmienne</span><span class="sxs-lookup"><span data-stu-id="51ac0-167">Types and variables</span></span>

<span data-ttu-id="51ac0-168">Istnieją dwa rodzaje typów w C#: ***typy wartości*** i ***typy odwołań***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="51ac0-169">Zmienne typów wartości bezpośrednio zawierają swoje dane, a zmienne typów referencyjnych przechowują odwołania do danych, które są znane jako obiekty.</span><span class="sxs-lookup"><span data-stu-id="51ac0-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="51ac0-170">W przypadku typów referencyjnych istnieje możliwość, że dwie zmienne odwołują się do tego samego obiektu, w tym przypadku operacje na jednej zmiennej mają wpływ na obiekt, do którego odwołuje się inna zmienna.</span><span class="sxs-lookup"><span data-stu-id="51ac0-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="51ac0-171">W przypadku typów wartości zmiennych każda z nich ma własną kopię danych i nie jest możliwe wykonywanie operacji na nich, aby wpływać na inne (z wyjątkiem `ref` zmiennych i `out` parametrów).</span><span class="sxs-lookup"><span data-stu-id="51ac0-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="51ac0-172">C#typy wartości są dalej podzielone na ***typy proste***, ***typy wyliczeniowe***, ***typy struktur***i ***Typy dopuszczające wartość null***, a C#typy odwołań są dalej podzielone na ***typy klas***, ***typy interfejsów***, ***Tablica typy***i ***typy delegatów***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="51ac0-173">Poniższa tabela zawiera omówienie C#systemu typu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="51ac0-174">__Kategoria__</span><span class="sxs-lookup"><span data-stu-id="51ac0-174">__Category__</span></span>    |                 | <span data-ttu-id="51ac0-175">__Opis__</span><span class="sxs-lookup"><span data-stu-id="51ac0-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="51ac0-176">Typy wartości</span><span class="sxs-lookup"><span data-stu-id="51ac0-176">Value types</span></span>     | <span data-ttu-id="51ac0-177">Typy proste</span><span class="sxs-lookup"><span data-stu-id="51ac0-177">Simple types</span></span>    | <span data-ttu-id="51ac0-178">Całkowita część ze `sbyte`znakiem `int`:, `short`,,`long`</span><span class="sxs-lookup"><span data-stu-id="51ac0-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="51ac0-179">Całka bez `byte`znaku `ushort`: `uint`,,,`ulong`</span><span class="sxs-lookup"><span data-stu-id="51ac0-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="51ac0-180">Znaki Unicode:`char`</span><span class="sxs-lookup"><span data-stu-id="51ac0-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="51ac0-181">Liczba zmiennoprzecinkowa IEEE `float`:,`double`</span><span class="sxs-lookup"><span data-stu-id="51ac0-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="51ac0-182">Liczba dziesiętna o dużej precyzji:`decimal`</span><span class="sxs-lookup"><span data-stu-id="51ac0-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="51ac0-183">Typu`bool`</span><span class="sxs-lookup"><span data-stu-id="51ac0-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="51ac0-184">Typy wyliczeniowe</span><span class="sxs-lookup"><span data-stu-id="51ac0-184">Enum types</span></span>      | <span data-ttu-id="51ac0-185">Typy formularza zdefiniowane przez użytkownika`enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="51ac0-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="51ac0-186">Typy struktur</span><span class="sxs-lookup"><span data-stu-id="51ac0-186">Struct types</span></span>    | <span data-ttu-id="51ac0-187">Typy formularza zdefiniowane przez użytkownika`struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="51ac0-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="51ac0-188">Typy dopuszczające wartości null</span><span class="sxs-lookup"><span data-stu-id="51ac0-188">Nullable types</span></span>  | <span data-ttu-id="51ac0-189">Rozszerzenia wszystkich innych typów wartości z `null` wartością</span><span class="sxs-lookup"><span data-stu-id="51ac0-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="51ac0-190">Typy odwołań</span><span class="sxs-lookup"><span data-stu-id="51ac0-190">Reference types</span></span> | <span data-ttu-id="51ac0-191">Typy klas</span><span class="sxs-lookup"><span data-stu-id="51ac0-191">Class types</span></span>     | <span data-ttu-id="51ac0-192">Ostateczna Klasa bazowa dla wszystkich innych typów:`object`</span><span class="sxs-lookup"><span data-stu-id="51ac0-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="51ac0-193">Ciągi Unicode:`string`</span><span class="sxs-lookup"><span data-stu-id="51ac0-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="51ac0-194">Typy formularza zdefiniowane przez użytkownika`class C {...}`</span><span class="sxs-lookup"><span data-stu-id="51ac0-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="51ac0-195">Typy interfejsów</span><span class="sxs-lookup"><span data-stu-id="51ac0-195">Interface types</span></span> | <span data-ttu-id="51ac0-196">Typy formularza zdefiniowane przez użytkownika`interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="51ac0-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="51ac0-197">Typy tablic</span><span class="sxs-lookup"><span data-stu-id="51ac0-197">Array types</span></span>     | <span data-ttu-id="51ac0-198">Pojedyncze i wielowymiarowe, na przykład `int[]` i`int[,]`</span><span class="sxs-lookup"><span data-stu-id="51ac0-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="51ac0-199">Typy delegatów</span><span class="sxs-lookup"><span data-stu-id="51ac0-199">Delegate types</span></span>  | <span data-ttu-id="51ac0-200">Typy formularzy zdefiniowane przez użytkownika, np.`delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="51ac0-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="51ac0-201">Osiem typów całkowitych zapewnia obsługę 8-bitowych, 16-bitowych, 32-bitowych i 64-bitowych wartości w postaci podpisanej lub niepodpisanej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="51ac0-202">Dwa typy zmiennoprzecinkowe, `float` i `double`, są reprezentowane przy użyciu 32-bitowej pojedynczej precyzji i 64-bitowe formatów IEEE 754 o podwójnej precyzji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="51ac0-203">`decimal` Typ jest 128-bitowym typem danych odpowiednim dla obliczeń finansowych i pieniężnych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="51ac0-204">C#Typ jest używany do reprezentowania wartości logicznych — wartości, które `true` są lub `false`. `bool`</span><span class="sxs-lookup"><span data-stu-id="51ac0-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="51ac0-205">Przetwarzanie znaków i ciągów w C# programie używa kodowania Unicode.</span><span class="sxs-lookup"><span data-stu-id="51ac0-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="51ac0-206">Typ reprezentuje jednostkę kodu UTF-16, `string` a typ reprezentuje sekwencję jednostek kodu UTF-16. `char`</span><span class="sxs-lookup"><span data-stu-id="51ac0-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="51ac0-207">Poniższa tabela zawiera podsumowanie C#typów liczbowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="51ac0-208">__Kategoria__</span><span class="sxs-lookup"><span data-stu-id="51ac0-208">__Category__</span></span>      | <span data-ttu-id="51ac0-209">__Bitów__</span><span class="sxs-lookup"><span data-stu-id="51ac0-209">__Bits__</span></span> | <span data-ttu-id="51ac0-210">__Typ__</span><span class="sxs-lookup"><span data-stu-id="51ac0-210">__Type__</span></span>  | <span data-ttu-id="51ac0-211">__Zakres/precyzja__</span><span class="sxs-lookup"><span data-stu-id="51ac0-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="51ac0-212">Całkowita ze znakiem</span><span class="sxs-lookup"><span data-stu-id="51ac0-212">Signed integral</span></span>   | <span data-ttu-id="51ac0-213">8</span><span class="sxs-lookup"><span data-stu-id="51ac0-213">8</span></span>        | `sbyte`   | <span data-ttu-id="51ac0-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="51ac0-214">-128...127</span></span> |
|                   | <span data-ttu-id="51ac0-215">16</span><span class="sxs-lookup"><span data-stu-id="51ac0-215">16</span></span>       | `short`   | <span data-ttu-id="51ac0-216">-32768... 32, 767</span><span class="sxs-lookup"><span data-stu-id="51ac0-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="51ac0-217">32</span><span class="sxs-lookup"><span data-stu-id="51ac0-217">32</span></span>       | `int`     | <span data-ttu-id="51ac0-218">-2147483648... 2, 147, 483 647</span><span class="sxs-lookup"><span data-stu-id="51ac0-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="51ac0-219">64</span><span class="sxs-lookup"><span data-stu-id="51ac0-219">64</span></span>       | `long`    | <span data-ttu-id="51ac0-220">-zakresu od... 9, 223, 372, 036, 854, 775,</span><span class="sxs-lookup"><span data-stu-id="51ac0-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="51ac0-221">Całkowita bez znaku</span><span class="sxs-lookup"><span data-stu-id="51ac0-221">Unsigned integral</span></span> | <span data-ttu-id="51ac0-222">8</span><span class="sxs-lookup"><span data-stu-id="51ac0-222">8</span></span>        | `byte`    | <span data-ttu-id="51ac0-223">0... 255</span><span class="sxs-lookup"><span data-stu-id="51ac0-223">0...255</span></span> |
|                   | <span data-ttu-id="51ac0-224">16</span><span class="sxs-lookup"><span data-stu-id="51ac0-224">16</span></span>       | `ushort`  | <span data-ttu-id="51ac0-225">0... 65, 535</span><span class="sxs-lookup"><span data-stu-id="51ac0-225">0...65,535</span></span> |
|                   | <span data-ttu-id="51ac0-226">32</span><span class="sxs-lookup"><span data-stu-id="51ac0-226">32</span></span>       | `uint`    | <span data-ttu-id="51ac0-227">0... 4, 294, 967, 295</span><span class="sxs-lookup"><span data-stu-id="51ac0-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="51ac0-228">64</span><span class="sxs-lookup"><span data-stu-id="51ac0-228">64</span></span>       | `ulong`   | <span data-ttu-id="51ac0-229">0... 18, 446, 744, 073, 709, 551, 615</span><span class="sxs-lookup"><span data-stu-id="51ac0-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="51ac0-230">Liczba zmiennoprzecinkowa</span><span class="sxs-lookup"><span data-stu-id="51ac0-230">Floating point</span></span>    | <span data-ttu-id="51ac0-231">32</span><span class="sxs-lookup"><span data-stu-id="51ac0-231">32</span></span>       | `float`   | <span data-ttu-id="51ac0-232">1,5 × 10 ^ − 45 do 3,4 × 10 ^ 38, 7-cyfrowa precyzja</span><span class="sxs-lookup"><span data-stu-id="51ac0-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="51ac0-233">64</span><span class="sxs-lookup"><span data-stu-id="51ac0-233">64</span></span>       | `double`  | <span data-ttu-id="51ac0-234">5,0 × 10 ^ − 324 do 1,7 × 10 ^ 308, 15-cyfrowa precyzja</span><span class="sxs-lookup"><span data-stu-id="51ac0-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="51ac0-235">Decimal</span><span class="sxs-lookup"><span data-stu-id="51ac0-235">Decimal</span></span>           | <span data-ttu-id="51ac0-236">128</span><span class="sxs-lookup"><span data-stu-id="51ac0-236">128</span></span>      | `decimal` | <span data-ttu-id="51ac0-237">1,0 × 10 ^ − 28 do 7,9 × 10 ^ 28, 28-cyfrowa precyzja</span><span class="sxs-lookup"><span data-stu-id="51ac0-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="51ac0-238">C#programy używają ***deklaracji typu*** do tworzenia nowych typów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="51ac0-239">Deklaracja typu określa nazwę i składowe nowego typu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="51ac0-240">C#Pięć kategorii typów są definiowane przez użytkownika: typy klas, typy struktur, typy interfejsów, typy wyliczeniowe i typy delegatów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="51ac0-241">Typ klasy definiuje strukturę danych, która zawiera składowe danych (pola) i składowe funkcji (metody, właściwości i inne).</span><span class="sxs-lookup"><span data-stu-id="51ac0-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="51ac0-242">Typy klas obsługują pojedyncze dziedziczenie i polimorfizm, czyli mechanizmy, w których klasy pochodne mogą poszerzać i specjalizację klas bazowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="51ac0-243">Typ struktury jest podobny do typu klasy w tym, że reprezentuje strukturę z składowymi danych i składowymi funkcji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="51ac0-244">Jednak w przeciwieństwie do klas, struktury są typami wartości i nie wymagają przydziału sterty.</span><span class="sxs-lookup"><span data-stu-id="51ac0-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="51ac0-245">Typy struktur nie obsługują dziedziczenia określonego przez użytkownika, a wszystkie typy struktur niejawnie dziedziczą `object`po typie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="51ac0-246">Typ interfejsu definiuje kontrakt jako nazwany zestaw elementów członkowskich funkcji publicznych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="51ac0-247">Klasa lub struktura implementująca interfejs musi zapewniać implementacje składowych funkcji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="51ac0-248">Interfejs może dziedziczyć z wielu interfejsów podstawowych, a Klasa lub struktura może zaimplementować wiele interfejsów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="51ac0-249">Typ delegata reprezentuje odwołania do metod z określoną listą parametrów i zwracanym typem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="51ac0-250">Delegaty umożliwiają traktowanie metod jako jednostek, które mogą być przypisane do zmiennych i przekazane jako parametry.</span><span class="sxs-lookup"><span data-stu-id="51ac0-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="51ac0-251">Delegaty są podobne do koncepcji wskaźników funkcji, które znajdują się w innych językach, ale w przeciwieństwie do wskaźników funkcji Delegaty są zorientowane obiektowo i są bezpieczne dla typów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="51ac0-252">Typy klas, struktur, interfejsów i delegatów obsługują ogólne, dzięki czemu można je sparametryzowane z innymi typami.</span><span class="sxs-lookup"><span data-stu-id="51ac0-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="51ac0-253">Typ wyliczeniowy jest typem odrębnym o nazwanych stałych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="51ac0-254">Każdy typ wyliczeniowy ma typ podstawowy, który musi być jednym z ośmiu typów całkowitych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="51ac0-255">Zestaw wartości typu wyliczeniowego jest taki sam jak zestaw wartości typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="51ac0-256">C#obsługuje tablice pojedynczych i wielowymiarowych dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="51ac0-257">W przeciwieństwie do typów wymienionych powyżej, typy tablicy nie muszą być zadeklarowane przed użyciem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="51ac0-258">Zamiast tego typy tablic są konstruowane przez następujące nazwy typu z nawiasami kwadratowymi.</span><span class="sxs-lookup"><span data-stu-id="51ac0-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="51ac0-259">Na przykład `int[]` jest `int` `int` `int`tablicą jednowymiarową, `int[,]` która jest tablicą dwuwymiarową, i `int[][]` jest tablicą jednowymiarową tablic jednowymiarowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="51ac0-260">Typy dopuszczające wartość null nie muszą również być zadeklarowane, aby można było ich używać.</span><span class="sxs-lookup"><span data-stu-id="51ac0-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="51ac0-261">Dla każdego typu `T` wartości niedopuszczających wartości null istnieje odpowiedni typ `T?`dopuszczający wartość null, który może `null`zawierać dodatkowe wartości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="51ac0-262">Na przykład `int?` jest typem, który może zawierać dowolną 32 bitową liczbę całkowitą lub wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="51ac0-263">C#System typów jest jednorodny tak, że wartość dowolnego typu może być traktowana jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="51ac0-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="51ac0-264">Każdy typ C# bezpośrednio lub pośrednio pochodzi od `object` typu klasy i `object` jest ostateczną klasą bazową wszystkich typów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="51ac0-265">Wartości typów referencyjnych są traktowane jako obiekty, po prostu wyświetlając wartości jako typ `object`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="51ac0-266">Wartości typów wartości są traktowane jako obiekty ***przez wykonywanie operacji*** ***pakowania i*** rozpakowywania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="51ac0-267">W poniższym przykładzie `int` wartość jest konwertowana na `object` i z powrotem do `int`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="51ac0-268">Gdy wartość typu wartości jest konwertowana na typ `object`, wystąpienie obiektu, nazywane również "polem", jest przydzielone do przechowywania wartości, a wartość jest kopiowana do tego pola.</span><span class="sxs-lookup"><span data-stu-id="51ac0-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="51ac0-269">Z drugiej strony, `object` gdy odwołanie jest rzutowane na typ wartości, jest przeprowadzane sprawdzenie, że przywoływany obiekt jest polem poprawnego typu wartości i, jeśli sprawdzenie zakończy się powodzeniem, wartość w polu jest kopiowana.</span><span class="sxs-lookup"><span data-stu-id="51ac0-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="51ac0-270">C#ujednolicony system typów efektywnie oznacza, że typy wartości mogą stać się obiektami "na żądanie".</span><span class="sxs-lookup"><span data-stu-id="51ac0-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="51ac0-271">Ze względu na niezjednoczenie bibliotek ogólnego przeznaczenia, które używają `object` typu, można używać zarówno z typami referencyjnymi, jak i typami wartości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="51ac0-272">W programie istnieją różne rodzaje ***zmiennych*** , w C#tym pola, elementy tablicy, zmienne lokalne i parametry.</span><span class="sxs-lookup"><span data-stu-id="51ac0-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="51ac0-273">Zmienne reprezentują lokalizacje przechowywania, a Każda zmienna ma typ, który określa, jakie wartości mogą być przechowywane w zmiennej, jak pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="51ac0-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="51ac0-274">__Typ zmiennej__</span><span class="sxs-lookup"><span data-stu-id="51ac0-274">__Type of Variable__</span></span>    | <span data-ttu-id="51ac0-275">__Możliwa zawartość__</span><span class="sxs-lookup"><span data-stu-id="51ac0-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="51ac0-276">Typ wartości niedopuszczający wartości null</span><span class="sxs-lookup"><span data-stu-id="51ac0-276">Non-nullable value type</span></span> | <span data-ttu-id="51ac0-277">Wartość tego dokładnego typu</span><span class="sxs-lookup"><span data-stu-id="51ac0-277">A value of that exact type</span></span> |
| <span data-ttu-id="51ac0-278">Typ wartości null</span><span class="sxs-lookup"><span data-stu-id="51ac0-278">Nullable value type</span></span>     | <span data-ttu-id="51ac0-279">Wartość null lub wartość tego dokładnego typu</span><span class="sxs-lookup"><span data-stu-id="51ac0-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="51ac0-280">Odwołanie o wartości null, odwołanie do obiektu dowolnego typu odwołania lub odwołanie do wartości opakowanej dowolnego typu wartości</span><span class="sxs-lookup"><span data-stu-id="51ac0-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="51ac0-281">Typ klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-281">Class type</span></span>              | <span data-ttu-id="51ac0-282">Odwołanie o wartości null, odwołanie do wystąpienia tego typu klasy lub odwołanie do wystąpienia klasy pochodzącej od tego typu klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="51ac0-283">Typ interfejsu</span><span class="sxs-lookup"><span data-stu-id="51ac0-283">Interface type</span></span>          | <span data-ttu-id="51ac0-284">Odwołanie o wartości null, odwołanie do wystąpienia typu klasy implementującego ten typ interfejsu lub odwołanie do wartości opakowanej typu wartości implementującej ten typ interfejsu</span><span class="sxs-lookup"><span data-stu-id="51ac0-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="51ac0-285">Typ tablicy</span><span class="sxs-lookup"><span data-stu-id="51ac0-285">Array type</span></span>              | <span data-ttu-id="51ac0-286">Odwołanie o wartości null, odwołanie do wystąpienia tego typu tablicy lub odwołanie do wystąpienia zgodnego typu tablicy</span><span class="sxs-lookup"><span data-stu-id="51ac0-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="51ac0-287">Typ delegata</span><span class="sxs-lookup"><span data-stu-id="51ac0-287">Delegate type</span></span>           | <span data-ttu-id="51ac0-288">Odwołanie o wartości null lub odwołanie do wystąpienia tego typu delegata</span><span class="sxs-lookup"><span data-stu-id="51ac0-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="51ac0-289">Wyrażenia</span><span class="sxs-lookup"><span data-stu-id="51ac0-289">Expressions</span></span>

<span data-ttu-id="51ac0-290">***Wyrażenia*** są konstruowane na podstawie ***operandów*** i ***operatorów***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="51ac0-291">Operatory wyrażenia wskazują operacje do zastosowania dla operandów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="51ac0-292">Przykłady operatorów to `+`, `-`, `*`, `/`i `new`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="51ac0-293">Przykładami operandów są literały, pola, zmienne lokalne i wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="51ac0-294">Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególne operatory.</span><span class="sxs-lookup"><span data-stu-id="51ac0-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="51ac0-295">Na przykład, wyrażenie `x + y * z` jest oceniane jako `x + (y * z)`, ponieważ operator `*` ma wyższy priorytet niż operator `+`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="51ac0-296">Większość operatorów może być ***przeciążona***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="51ac0-297">Przeciążanie operatora umożliwia określanie definiowanych przez użytkownika implementacji operatorów dla operacji, w których jeden lub oba operandy mają typ struktury lub klasy zdefiniowanej przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="51ac0-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="51ac0-298">Poniższa tabela podsumowuje C#operatory, wymieniając kategorie operatorów w kolejności od najwyższego do najniższego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="51ac0-299">Operatory w tej samej kategorii mają takie samo pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="51ac0-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="51ac0-300">__Kategoria__</span><span class="sxs-lookup"><span data-stu-id="51ac0-300">__Category__</span></span>                     | <span data-ttu-id="51ac0-301">__Expression__</span><span class="sxs-lookup"><span data-stu-id="51ac0-301">__Expression__</span></span>    | <span data-ttu-id="51ac0-302">__Opis__</span><span class="sxs-lookup"><span data-stu-id="51ac0-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="51ac0-303">Podstawowy</span><span class="sxs-lookup"><span data-stu-id="51ac0-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="51ac0-304">Dostęp do elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="51ac0-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="51ac0-305">Wywołanie metody i delegata</span><span class="sxs-lookup"><span data-stu-id="51ac0-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="51ac0-306">Dostęp do tablicy i indeksatora</span><span class="sxs-lookup"><span data-stu-id="51ac0-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="51ac0-307">Postinkrementacja</span><span class="sxs-lookup"><span data-stu-id="51ac0-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="51ac0-308">Postdekrementacja</span><span class="sxs-lookup"><span data-stu-id="51ac0-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="51ac0-309">Utworzenie obiektu i delegata</span><span class="sxs-lookup"><span data-stu-id="51ac0-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="51ac0-310">Tworzenie obiektu z inicjatorem</span><span class="sxs-lookup"><span data-stu-id="51ac0-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="51ac0-311">Inicjator obiektu anonimowego</span><span class="sxs-lookup"><span data-stu-id="51ac0-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="51ac0-312">Tworzenie tablicy</span><span class="sxs-lookup"><span data-stu-id="51ac0-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="51ac0-313">Uzyskaj `System.Type` obiekt dla`T`</span><span class="sxs-lookup"><span data-stu-id="51ac0-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="51ac0-314">Obliczenie wyrażenia w kontekście sprawdzanym</span><span class="sxs-lookup"><span data-stu-id="51ac0-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="51ac0-315">Obliczenie wyrażenia w kontekście niesprawdzanym</span><span class="sxs-lookup"><span data-stu-id="51ac0-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="51ac0-316">Uzyskaj wartość domyślną typu`T`</span><span class="sxs-lookup"><span data-stu-id="51ac0-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="51ac0-317">Funkcja anonimowa (metoda anonimowa)</span><span class="sxs-lookup"><span data-stu-id="51ac0-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="51ac0-318">Jednostk</span><span class="sxs-lookup"><span data-stu-id="51ac0-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="51ac0-319">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="51ac0-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="51ac0-320">Negacja</span><span class="sxs-lookup"><span data-stu-id="51ac0-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="51ac0-321">Negacja logiczna</span><span class="sxs-lookup"><span data-stu-id="51ac0-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="51ac0-322">Negacja bitowa</span><span class="sxs-lookup"><span data-stu-id="51ac0-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="51ac0-323">Preinkrementacja</span><span class="sxs-lookup"><span data-stu-id="51ac0-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="51ac0-324">Predekrementacja</span><span class="sxs-lookup"><span data-stu-id="51ac0-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="51ac0-325">Jawna konwersja `x` na typ `T`</span><span class="sxs-lookup"><span data-stu-id="51ac0-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="51ac0-326">Asynchronicznie poczekaj na ukończenie `x`</span><span class="sxs-lookup"><span data-stu-id="51ac0-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="51ac0-327">Mnożeniowy</span><span class="sxs-lookup"><span data-stu-id="51ac0-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="51ac0-328">Mnożenie</span><span class="sxs-lookup"><span data-stu-id="51ac0-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="51ac0-329">Dzielenie</span><span class="sxs-lookup"><span data-stu-id="51ac0-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="51ac0-330">Reszta</span><span class="sxs-lookup"><span data-stu-id="51ac0-330">Remainder</span></span> |
| <span data-ttu-id="51ac0-331">Dana</span><span class="sxs-lookup"><span data-stu-id="51ac0-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="51ac0-332">Dodawanie, łączenie ciągów, łączenie delegatów</span><span class="sxs-lookup"><span data-stu-id="51ac0-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="51ac0-333">Odejmowanie, usuwanie delegata</span><span class="sxs-lookup"><span data-stu-id="51ac0-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="51ac0-334">Shift</span><span class="sxs-lookup"><span data-stu-id="51ac0-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="51ac0-335">Przesunięcie w lewo</span><span class="sxs-lookup"><span data-stu-id="51ac0-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="51ac0-336">Przesunięcie w prawo</span><span class="sxs-lookup"><span data-stu-id="51ac0-336">Shift right</span></span> |
| <span data-ttu-id="51ac0-337">Testowanie relacyjne i typu</span><span class="sxs-lookup"><span data-stu-id="51ac0-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="51ac0-338">Mniejsze niż</span><span class="sxs-lookup"><span data-stu-id="51ac0-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="51ac0-339">Większe niż</span><span class="sxs-lookup"><span data-stu-id="51ac0-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="51ac0-340">Mniejsze niż lub równe</span><span class="sxs-lookup"><span data-stu-id="51ac0-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="51ac0-341">Większe niż lub równe</span><span class="sxs-lookup"><span data-stu-id="51ac0-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="51ac0-342">`true` Zwracaj `T`, jeśli `x` jest ,`false` w przeciwnym razie</span><span class="sxs-lookup"><span data-stu-id="51ac0-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="51ac0-343">Zwraca `x` wartość wpisaną `T`jako, `null` lub `x` Jeśli nie jest`T`</span><span class="sxs-lookup"><span data-stu-id="51ac0-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="51ac0-344">Równości</span><span class="sxs-lookup"><span data-stu-id="51ac0-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="51ac0-345">Równa się</span><span class="sxs-lookup"><span data-stu-id="51ac0-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="51ac0-346">Nie równa się</span><span class="sxs-lookup"><span data-stu-id="51ac0-346">Not equal</span></span> |
| <span data-ttu-id="51ac0-347">Logicznego AND</span><span class="sxs-lookup"><span data-stu-id="51ac0-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="51ac0-348">Liczba całkowita bitowa, koniunkcja logiczna i</span><span class="sxs-lookup"><span data-stu-id="51ac0-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="51ac0-349">Logicznego XOR</span><span class="sxs-lookup"><span data-stu-id="51ac0-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="51ac0-350">Bitowe XOR dla wartości całkowitych, logiczne XOR dla wartości binarnych</span><span class="sxs-lookup"><span data-stu-id="51ac0-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="51ac0-351">Logicznego OR</span><span class="sxs-lookup"><span data-stu-id="51ac0-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="51ac0-352">Bitowe OR dla wartości całkowitych, logiczne OR dla wartości binarnych</span><span class="sxs-lookup"><span data-stu-id="51ac0-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="51ac0-353">Warunkowego AND</span><span class="sxs-lookup"><span data-stu-id="51ac0-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="51ac0-354">Oblicza tylko wtedy, `x`gdyjest `y``true`</span><span class="sxs-lookup"><span data-stu-id="51ac0-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="51ac0-355">Warunkowego OR</span><span class="sxs-lookup"><span data-stu-id="51ac0-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="51ac0-356">Oblicza tylko wtedy, `x`gdyjest `y``false`</span><span class="sxs-lookup"><span data-stu-id="51ac0-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="51ac0-357">Łączenia wartości null</span><span class="sxs-lookup"><span data-stu-id="51ac0-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="51ac0-358">Jest wynikiem, Aby`x` w przeciwnym razie `null` `y` `x`</span><span class="sxs-lookup"><span data-stu-id="51ac0-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="51ac0-359">Warunkowe</span><span class="sxs-lookup"><span data-stu-id="51ac0-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="51ac0-360">`x` Daje w `y` przypadku, gdy`z` jest`x` `true``false`</span><span class="sxs-lookup"><span data-stu-id="51ac0-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="51ac0-361">Przypisania lub funkcji anonimowej</span><span class="sxs-lookup"><span data-stu-id="51ac0-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="51ac0-362">Przypisanie</span><span class="sxs-lookup"><span data-stu-id="51ac0-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="51ac0-363">Przypisanie złożone; obsługiwane operatory to `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="51ac0-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="51ac0-364">Funkcja anonimowa (wyrażenie lambda)</span><span class="sxs-lookup"><span data-stu-id="51ac0-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="51ac0-365">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="51ac0-365">Statements</span></span>

<span data-ttu-id="51ac0-366">Akcje programu są wyrażane przy użyciu ***instrukcji***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="51ac0-367">C#obsługuje kilka różnych rodzajów instrukcji, które są zdefiniowane w zakresie osadzonych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="51ac0-368">***Blok*** umożliwia zapisanie wielu instrukcji w kontekstach, w których Pojedyncza instrukcja jest dozwolona.</span><span class="sxs-lookup"><span data-stu-id="51ac0-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="51ac0-369">Blok składa się z listy instrukcji pisanych między ogranicznikami `{` i. `}`</span><span class="sxs-lookup"><span data-stu-id="51ac0-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="51ac0-370">***Instrukcje deklaracji*** są używane do deklarowania zmiennych lokalnych i stałych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="51ac0-371">***Instrukcje wyrażeń*** są używane do obliczania wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="51ac0-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="51ac0-372">Wyrażenia, które mogą być używane jako instrukcje, obejmują wywołania metod, alokacje obiektów przy użyciu `new` operatora, przydziałów używających `=` i operatorów przypisania złożonego, operacji zwiększania i zmniejszania przy użyciu `++`operatory i`--` .</span><span class="sxs-lookup"><span data-stu-id="51ac0-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="51ac0-373">***Instrukcje wyboru*** są używane do wybierania jednej z wielu możliwych instrukcji do wykonania na podstawie wartości niektórych wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="51ac0-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="51ac0-374">W tej grupie są `if` instrukcje i. `switch`</span><span class="sxs-lookup"><span data-stu-id="51ac0-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="51ac0-375">***Instrukcje iteracji*** są używane do wielokrotnego wykonywania osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="51ac0-376">W `while`tej grupie są instrukcje, `do`, `for`, i. `foreach`</span><span class="sxs-lookup"><span data-stu-id="51ac0-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="51ac0-377">***Instrukcje skoku*** są używane do transferowania kontroli.</span><span class="sxs-lookup"><span data-stu-id="51ac0-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="51ac0-378">W tej `break`grupie są instrukcje, `continue`, `goto` `throw` `yield` , ,i.`return`</span><span class="sxs-lookup"><span data-stu-id="51ac0-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="51ac0-379">`try`... Instrukcja jest używana do przechwytywania wyjątków, które występują podczas wykonywania bloku, `try`i... `catch` `finally` Instrukcja służy do określania kodu finalizacji, który jest zawsze wykonywany, niezależnie od tego, czy wystąpił wyjątek, czy nie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="51ac0-380">Instrukcje `checked` and`unchecked` są używane do kontrolowania kontekstu sprawdzania przepełnienia dla operacji arytmetycznych typu całkowitego i konwersji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="51ac0-381">`lock` Instrukcja służy do uzyskiwania blokady wzajemnego wykluczania dla danego obiektu, wykonywania instrukcji i zwalniania blokady.</span><span class="sxs-lookup"><span data-stu-id="51ac0-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="51ac0-382">`using` Instrukcja służy do uzyskiwania zasobu, wykonywania instrukcji, a następnie usuwania tego zasobu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="51ac0-383">Poniżej przedstawiono przykłady poszczególnych rodzajów instrukcji</span><span class="sxs-lookup"><span data-stu-id="51ac0-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="51ac0-384">__Deklaracje zmiennej lokalnej__</span><span class="sxs-lookup"><span data-stu-id="51ac0-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="51ac0-385">__Lokalna deklaracja stała__</span><span class="sxs-lookup"><span data-stu-id="51ac0-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="51ac0-386">__Instrukcja wyrażenia__</span><span class="sxs-lookup"><span data-stu-id="51ac0-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="51ac0-387">__`if`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-387">__`if` statement__</span></span>

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


<span data-ttu-id="51ac0-388">__`switch`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-388">__`switch` statement__</span></span>

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

<span data-ttu-id="51ac0-389">__`while`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="51ac0-390">__`do`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="51ac0-391">__`for`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="51ac0-392">__`foreach`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="51ac0-393">__`break`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="51ac0-394">__`continue`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="51ac0-395">__`goto`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-395">__`goto` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

<span data-ttu-id="51ac0-396">__`return`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="51ac0-397">__`yield`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-397">__`yield` statement__</span></span>

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="51ac0-398">__`throw`and `try` — instrukcje__</span><span class="sxs-lookup"><span data-stu-id="51ac0-398">__`throw` and `try` statements__</span></span>

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

<span data-ttu-id="51ac0-399">__`checked`and `unchecked` — instrukcje__</span><span class="sxs-lookup"><span data-stu-id="51ac0-399">__`checked` and `unchecked` statements__</span></span>

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

<span data-ttu-id="51ac0-400">__`lock`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-400">__`lock` statement__</span></span>

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

<span data-ttu-id="51ac0-401">__`using`Merge__</span><span class="sxs-lookup"><span data-stu-id="51ac0-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="51ac0-402">Klasy i obiekty</span><span class="sxs-lookup"><span data-stu-id="51ac0-402">Classes and objects</span></span>

<span data-ttu-id="51ac0-403">***Klasy*** są najbardziej podstawowym typem w języku C#.</span><span class="sxs-lookup"><span data-stu-id="51ac0-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="51ac0-404">Klasa jest strukturą danych, która łączy stan (pola) i akcje (metody i inne elementy członkowskie funkcji) w jednej jednostce.</span><span class="sxs-lookup"><span data-stu-id="51ac0-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="51ac0-405">Klasa zawiera definicję dla dynamicznie utworzonych ***wystąpień*** klasy, znanych również jako ***obiekty***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="51ac0-406">Klasy obsługują ***dziedziczenie*** i ***polimorfizm***, natomiast mechanizmy, w których ***klasy pochodne*** mogą poszerzać i specjalizację ***klas bazowych***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="51ac0-407">Nowe klasy są tworzone za pomocą deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-407">New classes are created using class declarations.</span></span> <span data-ttu-id="51ac0-408">Deklaracja klasy rozpoczyna się od nagłówka, który określa atrybuty i Modyfikatory klasy, nazwę klasy, klasę bazową (jeśli ma to zastosowanie) i interfejsy zaimplementowane przez klasę.</span><span class="sxs-lookup"><span data-stu-id="51ac0-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="51ac0-409">Po tym nagłówku następuje treść klasy, która składa się z listy deklaracji elementów członkowskich, które są zapisywane między `{` ogranicznikami `}`i.</span><span class="sxs-lookup"><span data-stu-id="51ac0-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="51ac0-410">Poniżej znajduje się deklaracja klasy prostej o nazwie `Point`:</span><span class="sxs-lookup"><span data-stu-id="51ac0-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="51ac0-411">Wystąpienia klas są tworzone przy użyciu `new` operatora, który przydziela pamięć dla nowego wystąpienia, wywołuje konstruktora w celu zainicjowania wystąpienia i zwraca odwołanie do wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="51ac0-412">Poniższe instrukcje tworzą dwa `Point` obiekty i przechowują odwołania do tych obiektów w dwóch zmiennych:</span><span class="sxs-lookup"><span data-stu-id="51ac0-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="51ac0-413">Pamięć zajęta przez obiekt jest automatycznie odzyskiwana, gdy obiekt nie jest już używany.</span><span class="sxs-lookup"><span data-stu-id="51ac0-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="51ac0-414">Nie jest to konieczne ani możliwe, aby jawnie cofnąć alokację obiektów w programie C#.</span><span class="sxs-lookup"><span data-stu-id="51ac0-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="51ac0-415">Members</span><span class="sxs-lookup"><span data-stu-id="51ac0-415">Members</span></span>

<span data-ttu-id="51ac0-416">Elementy członkowskie klasy są ***statycznymi elementami członkowskimi*** lub ***wystąpieniami***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="51ac0-417">Statyczne składowe należą do klas, a elementy członkowskie wystąpienia należą do obiektów (wystąpień klas).</span><span class="sxs-lookup"><span data-stu-id="51ac0-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="51ac0-418">Poniższa tabela zawiera omówienie rodzajów elementów członkowskich, które może zawierać Klasa.</span><span class="sxs-lookup"><span data-stu-id="51ac0-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="51ac0-419">__Członkiem__</span><span class="sxs-lookup"><span data-stu-id="51ac0-419">__Member__</span></span>   | <span data-ttu-id="51ac0-420">__Opis__</span><span class="sxs-lookup"><span data-stu-id="51ac0-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="51ac0-421">Stałe</span><span class="sxs-lookup"><span data-stu-id="51ac0-421">Constants</span></span>    | <span data-ttu-id="51ac0-422">Wartości stałe skojarzone z klasą</span><span class="sxs-lookup"><span data-stu-id="51ac0-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="51ac0-423">Pola</span><span class="sxs-lookup"><span data-stu-id="51ac0-423">Fields</span></span>       | <span data-ttu-id="51ac0-424">Zmienne klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-424">Variables of the class</span></span> |
| <span data-ttu-id="51ac0-425">Metody</span><span class="sxs-lookup"><span data-stu-id="51ac0-425">Methods</span></span>      | <span data-ttu-id="51ac0-426">Obliczenia i akcje, które mogą być wykonywane przez klasę</span><span class="sxs-lookup"><span data-stu-id="51ac0-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="51ac0-427">properties</span><span class="sxs-lookup"><span data-stu-id="51ac0-427">Properties</span></span>   | <span data-ttu-id="51ac0-428">Akcje skojarzone z odczytem i pisaniem nazwanych właściwości klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="51ac0-429">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="51ac0-429">Indexers</span></span>     | <span data-ttu-id="51ac0-430">Akcje skojarzone z wystąpieniami indeksowania klasy, takimi jak tablica</span><span class="sxs-lookup"><span data-stu-id="51ac0-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="51ac0-431">Events</span><span class="sxs-lookup"><span data-stu-id="51ac0-431">Events</span></span>       | <span data-ttu-id="51ac0-432">Powiadomienia, które mogą zostać wygenerowane przez klasę</span><span class="sxs-lookup"><span data-stu-id="51ac0-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="51ac0-433">Operatory</span><span class="sxs-lookup"><span data-stu-id="51ac0-433">Operators</span></span>    | <span data-ttu-id="51ac0-434">Konwersje i operatory wyrażeń obsługiwane przez klasę</span><span class="sxs-lookup"><span data-stu-id="51ac0-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="51ac0-435">Konstruktorów</span><span class="sxs-lookup"><span data-stu-id="51ac0-435">Constructors</span></span> | <span data-ttu-id="51ac0-436">Akcje wymagane do zainicjowania wystąpień klasy lub samej klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="51ac0-437">Destruktory</span><span class="sxs-lookup"><span data-stu-id="51ac0-437">Destructors</span></span>  | <span data-ttu-id="51ac0-438">Akcje do wykonania przed trwałe odrzuceniem wystąpień klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="51ac0-439">Types</span><span class="sxs-lookup"><span data-stu-id="51ac0-439">Types</span></span>        | <span data-ttu-id="51ac0-440">Zagnieżdżone typy zadeklarowane przez klasę</span><span class="sxs-lookup"><span data-stu-id="51ac0-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="51ac0-441">Ułatwienia dostępu</span><span class="sxs-lookup"><span data-stu-id="51ac0-441">Accessibility</span></span>

<span data-ttu-id="51ac0-442">Każdy element członkowski klasy ma skojarzoną dostępność, która kontroluje regiony tekstu programu, które mogą uzyskać dostęp do elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="51ac0-443">Istnieje pięć możliwych form ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="51ac0-444">Są one podsumowane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="51ac0-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="51ac0-445">__Ułatwienia dostępu__</span><span class="sxs-lookup"><span data-stu-id="51ac0-445">__Accessibility__</span></span>    | <span data-ttu-id="51ac0-446">__Znaczenie__</span><span class="sxs-lookup"><span data-stu-id="51ac0-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="51ac0-447">Dostęp nie jest ograniczony</span><span class="sxs-lookup"><span data-stu-id="51ac0-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="51ac0-448">Dostęp ograniczony do tej klasy lub klas pochodnych od tej klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="51ac0-449">Dostęp ograniczony do tego programu</span><span class="sxs-lookup"><span data-stu-id="51ac0-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="51ac0-450">Dostęp ograniczony do tego programu lub klas pochodzących od tej klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="51ac0-451">Dostęp ograniczony do tej klasy</span><span class="sxs-lookup"><span data-stu-id="51ac0-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="51ac0-452">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="51ac0-452">Type parameters</span></span>

<span data-ttu-id="51ac0-453">Definicja klasy może określać zestaw parametrów typu przez poniższą nazwę klasy z nawiasami kątowymi zawierającymi listę nazw parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="51ac0-454">Parametry typu mogą być używane w treści deklaracji klasy do definiowania elementów członkowskich klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="51ac0-455">W poniższym przykładzie parametry `Pair` typu są `TFirst` i `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="51ac0-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="51ac0-456">Typ klasy zadeklarowanej do wykonania parametrów typu jest nazywany typem klasy generycznej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="51ac0-457">Typy struktur, interfejsów i delegatów mogą być również rodzajowe.</span><span class="sxs-lookup"><span data-stu-id="51ac0-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="51ac0-458">Gdy używana jest Klasa generyczna, należy podać argumenty typu dla każdego z parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="51ac0-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="51ac0-459">Typ ogólny z podanymi argumentami typu, `Pair<int,string>` jak powyżej, jest nazywany typem skonstruowanym.</span><span class="sxs-lookup"><span data-stu-id="51ac0-459">A generic type with type arguments provided, like `Pair<int,string>` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="51ac0-460">Klas podstawowych</span><span class="sxs-lookup"><span data-stu-id="51ac0-460">Base classes</span></span>

<span data-ttu-id="51ac0-461">Deklaracja klasy może określać klasę bazową, postępując według nazwy klasy i parametrów typu z dwukropkiem i nazwą klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="51ac0-462">Pominięcie specyfikacji klasy bazowej jest taka sama jak pochodna typu `object`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="51ac0-463">`Point3D` W poniższym przykładzie klasą bazową jest `Point`, `Point` a klasą bazową jest `object`:</span><span class="sxs-lookup"><span data-stu-id="51ac0-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="51ac0-464">Klasa dziedziczy elementy członkowskie swojej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="51ac0-465">Dziedziczenie oznacza, że Klasa niejawnie zawiera wszystkie elementy członkowskie swojej klasy podstawowej, z wyjątkiem dla wystąpienia i konstruktorów statycznych i destruktorów klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="51ac0-466">Klasa pochodna może dodawać nowych członków do tych, które dziedziczy, ale nie może usunąć definicji dziedziczonego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="51ac0-467">W poprzednim przykładzie `Point3D` `x` dziedziczy pola i `y` z `Point`, i każde `Point3D` wystąpienie zawiera trzy pola, `x`, `y`, i `z`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="51ac0-468">Niejawna konwersja istnieje z typu klasy do dowolnego z jego typów klas podstawowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="51ac0-469">W związku z tym zmienna typu klasy może odwoływać się do wystąpienia tej klasy lub wystąpienia dowolnej klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="51ac0-470">Na przykład uwzględniając poprzednie deklaracje klas, zmienna typu `Point` może odwoływać się do `Point` lub `Point3D`:</span><span class="sxs-lookup"><span data-stu-id="51ac0-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="51ac0-471">Pola</span><span class="sxs-lookup"><span data-stu-id="51ac0-471">Fields</span></span>

<span data-ttu-id="51ac0-472">Pole jest zmienną, która jest skojarzona z klasą lub wystąpieniem klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="51ac0-473">Pole zadeklarowane z `static` modyfikatorem definiuje ***pole statyczne***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="51ac0-474">Pole statyczne identyfikuje dokładnie jedną lokalizację magazynu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="51ac0-475">Niezależnie od tego, ile wystąpień klasy zostało utworzonych, istnieje tylko jedna kopia pola statycznego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="51ac0-476">Pole zadeklarowane bez `static` modyfikatora definiuje ***pole wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="51ac0-477">Każde wystąpienie klasy zawiera oddzielną kopię wszystkich pól wystąpienia tej klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="51ac0-478">W `Color` poniższym przykładzie każde wystąpienie klasy ma oddzielną kopię `r` `g` `White`pól,, i `b` `Black`wystąpienia `Red`, ale istnieje tylko jedna kopia,,, `Green` i`Blue` pola statyczne:</span><span class="sxs-lookup"><span data-stu-id="51ac0-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="51ac0-479">Jak pokazano w poprzednim przykładzie ***pola tylko do odczytu*** mogą być zadeklarowane za pomocą `readonly` modyfikatora.</span><span class="sxs-lookup"><span data-stu-id="51ac0-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="51ac0-480">Przypisanie do `readonly` pola może wystąpić tylko jako część deklaracji pola lub konstruktora w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="51ac0-481">Metody</span><span class="sxs-lookup"><span data-stu-id="51ac0-481">Methods</span></span>

<span data-ttu-id="51ac0-482">***Metoda*** to element członkowski implementujący obliczenia lub akcję, które mogą być wykonywane przez obiekt lub klasę.</span><span class="sxs-lookup"><span data-stu-id="51ac0-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="51ac0-483">***Metody statyczne*** są dostępne za pomocą klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="51ac0-484">***Metody wystąpienia*** są dostępne za pomocą wystąpień klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="51ac0-485">Metody mają (prawdopodobnie pustą) listę ***parametrów***reprezentujących wartości lub odwołania do zmiennych, które są przenoszone do metody i ***Typ zwracany***, który określa typ wartości obliczanej i zwracanej przez metodę.</span><span class="sxs-lookup"><span data-stu-id="51ac0-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="51ac0-486">Zwracany typ metody to `void` , jeśli nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="51ac0-487">Podobnie jak typy, metody mogą także mieć zestaw parametrów typu, dla których argumenty typu muszą być określone, gdy wywoływana jest metoda.</span><span class="sxs-lookup"><span data-stu-id="51ac0-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="51ac0-488">W przeciwieństwie do typów, argumenty typu często można wywnioskować na podstawie argumentów wywołania metody i nie muszą być jawnie określone.</span><span class="sxs-lookup"><span data-stu-id="51ac0-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="51ac0-489">***Sygnatura*** metody musi być unikatowa w klasie, w której metoda jest zadeklarowana.</span><span class="sxs-lookup"><span data-stu-id="51ac0-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="51ac0-490">Podpis metody składa się z nazwy metody, liczby parametrów typu oraz liczby, modyfikatorów i typów jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="51ac0-491">Sygnatura metody nie zawiera typu zwracanego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="51ac0-492">Parametry</span><span class="sxs-lookup"><span data-stu-id="51ac0-492">Parameters</span></span>

<span data-ttu-id="51ac0-493">Parametry służą do przekazywania wartości lub odwołań do zmiennych do metod.</span><span class="sxs-lookup"><span data-stu-id="51ac0-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="51ac0-494">Parametry metody pobierają rzeczywiste wartości z ***argumentów*** , które są określone podczas wywoływania metody.</span><span class="sxs-lookup"><span data-stu-id="51ac0-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="51ac0-495">Istnieją cztery rodzaje parametrów: parametry wartości, parametry odwołania, parametry wyjściowe i tablice parametrów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="51ac0-496">***Parametr value*** jest używany do przekazywania parametru wejściowego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="51ac0-497">Parametr value odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z argumentu, który został przesłany dla parametru.</span><span class="sxs-lookup"><span data-stu-id="51ac0-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="51ac0-498">Modyfikacje parametru value nie wpływają na argument, który został przesłany dla parametru.</span><span class="sxs-lookup"><span data-stu-id="51ac0-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="51ac0-499">Parametry wartości mogą być opcjonalne, określając wartość domyślną, aby można było pominąć odpowiednie argumenty.</span><span class="sxs-lookup"><span data-stu-id="51ac0-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="51ac0-500">***Parametr Reference*** służy do przekazywania parametrów wejściowych i wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="51ac0-501">Argument przesłany dla parametru odwołania musi być zmienną i podczas wykonywania metody parametr Reference reprezentuje tę samą lokalizację magazynu co zmienna argumentu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="51ac0-502">Parametr odwołania jest zadeklarowany z `ref` modyfikatorem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="51ac0-503">W poniższym przykładzie pokazano sposób użycia `ref` parametrów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-503">The following example shows the use of `ref` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="51ac0-504">***Parametr wyjściowy*** jest używany do przekazywania parametrów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="51ac0-505">Parametr wyjściowy jest podobny do parametru odwołania, z tą różnicą, że początkowa wartość argumentu dostarczonego przez wywołującego jest nieważna.</span><span class="sxs-lookup"><span data-stu-id="51ac0-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="51ac0-506">Parametr wyjściowy jest zadeklarowany z `out` modyfikatorem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="51ac0-507">W poniższym przykładzie pokazano sposób użycia `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="51ac0-508">***Tablica parametrów*** umożliwia przekazanie zmiennej liczbie argumentów do metody.</span><span class="sxs-lookup"><span data-stu-id="51ac0-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="51ac0-509">Tablica parametrów jest zadeklarowana z `params` modyfikatorem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="51ac0-510">Tylko ostatni parametr metody może być tablicą parametrów, a typ tablicy parametrów musi być typem tablicy jednowymiarowej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="51ac0-511">Metody `Write` i`WriteLine` klasy są dobrymi przykładami użycia tablicy parametrów. `System.Console`</span><span class="sxs-lookup"><span data-stu-id="51ac0-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="51ac0-512">Są one deklarowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="51ac0-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="51ac0-513">W metodzie, która używa tablicy parametrów, tablica parametrów zachowuje się dokładnie tak jak zwykły parametr typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="51ac0-514">Jednak w wywołaniu metody z tablicą parametrów możliwe jest przekazanie jednego argumentu typu tablicy parametrów lub dowolnej liczby argumentów typu elementu tablicy parametrów w parametrze.</span><span class="sxs-lookup"><span data-stu-id="51ac0-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="51ac0-515">W tym drugim przypadku wystąpienie tablicy jest automatycznie tworzone i inicjowane z podanym argumentami.</span><span class="sxs-lookup"><span data-stu-id="51ac0-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="51ac0-516">Ten przykład</span><span class="sxs-lookup"><span data-stu-id="51ac0-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="51ac0-517">jest równoznaczny z zapisem poniżej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="51ac0-518">Treść metody i zmienne lokalne</span><span class="sxs-lookup"><span data-stu-id="51ac0-518">Method body and local variables</span></span>

<span data-ttu-id="51ac0-519">Treść metody Określa instrukcje do wykonania, gdy metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="51ac0-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="51ac0-520">Treść metody może deklarować zmienne, które są specyficzne dla wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="51ac0-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="51ac0-521">Takie zmienne są nazywane ***zmiennymi lokalnymi***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="51ac0-522">Deklaracja zmiennej lokalnej określa nazwę typu, nazwę zmiennej i prawdopodobnie wartość początkową.</span><span class="sxs-lookup"><span data-stu-id="51ac0-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="51ac0-523">Poniższy przykład deklaruje zmienną `i` lokalną z początkową wartością zero i zmienną `j` lokalną bez wartości początkowej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="51ac0-524">C#wymaga, aby zmienna lokalna była ***przypisana ostatecznie*** przed uzyskaniem jej wartości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="51ac0-525">Na przykład jeśli deklaracja poprzedniej `i` nie zawierała wartości początkowej, kompilator zgłosi błąd dla kolejnych zastosowań `i` , ponieważ `i` nie zostanie on ostatecznie przypisany w tych punktach w programie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="51ac0-526">Metoda może użyć `return` instrukcji do zwrócenia kontroli do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="51ac0-527">W wyniku zwrócenia `void` `return` metody instrukcje nie mogą określać wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="51ac0-528">W metodzie zwracającej`void`nie, `return` instrukcje muszą zawierać wyrażenie, które oblicza wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="51ac0-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="51ac0-529">Metody static i instance</span><span class="sxs-lookup"><span data-stu-id="51ac0-529">Static and instance methods</span></span>

<span data-ttu-id="51ac0-530">Metoda zadeklarowana za pomocą `static` modyfikatora jest ***metodą statyczną***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="51ac0-531">Metoda statyczna nie działa w konkretnym wystąpieniu i może bezpośrednio uzyskiwać dostęp do statycznych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="51ac0-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="51ac0-532">Metoda zadeklarowana bez `static` modyfikatora jest ***metodą wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="51ac0-533">Metoda wystąpienia działa w konkretnym wystąpieniu i może uzyskiwać dostęp do elementów członkowskich static i instance.</span><span class="sxs-lookup"><span data-stu-id="51ac0-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="51ac0-534">Wystąpienie, na którym wywołano metodę wystąpienia, można jawnie uzyskać do niego `this`dostęp.</span><span class="sxs-lookup"><span data-stu-id="51ac0-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="51ac0-535">Wystąpił błąd podczas odwoływania się `this` do w metodzie statycznej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="51ac0-536">Następująca `Entity` Klasa zawiera elementy członkowskie statyczne i wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="51ac0-537">Każde `Entity` wystąpienie zawiera numer seryjny (i najprawdopodobniej inne informacje, które nie są wyświetlane w tym miejscu).</span><span class="sxs-lookup"><span data-stu-id="51ac0-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="51ac0-538">`Entity` Konstruktor (który przypomina metodę wystąpienia) Inicjuje nowe wystąpienie przy użyciu następnego dostępnego numeru seryjnego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="51ac0-539">Ponieważ Konstruktor jest członkiem wystąpienia, może uzyskać dostęp zarówno `serialNo` do pola wystąpienia, `nextSerialNo` jak i pola statycznego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="51ac0-540">Metody `GetNextSerialNo` `serialNo` i `SetNextSerialNo`staticmogą uzyskać dostęp do pola statycznego,alemożetobyćbłąd,abyuzyskaćbezpośrednidostępdopolawystąpienia.`nextSerialNo`</span><span class="sxs-lookup"><span data-stu-id="51ac0-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="51ac0-541">Poniższy przykład pokazuje użycie `Entity` klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="51ac0-542">Należy zauważyć, `SetNextSerialNo` że `GetNextSerialNo` metody i są wywoływane `GetSerialNo` dla klasy, podczas gdy metoda wystąpienia jest wywoływana w wystąpieniach klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="51ac0-543">Metody wirtualne, przesłonięcia i abstrakcyjne</span><span class="sxs-lookup"><span data-stu-id="51ac0-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="51ac0-544">Gdy deklaracja metody wystąpienia zawiera `virtual` modyfikator, metoda jest uznawana za ***metodę wirtualną***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="51ac0-545">Gdy modyfikator `virtual` nie jest obecny, metoda jest uznawana za ***metodę niewirtualną***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="51ac0-546">Gdy wywoływana jest metoda wirtualna, ***typem czasu wykonywania*** wystąpienia, dla którego odbywa się wywołanie określa rzeczywistą implementację metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="51ac0-547">W wywołaniu metody niewirtualnej ***Typ czasu kompilacji*** wystąpienia jest czynnikiem decydującym.</span><span class="sxs-lookup"><span data-stu-id="51ac0-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="51ac0-548">Metoda wirtualna może zostać ***przesłonięta*** w klasie pochodnej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="51ac0-549">Gdy deklaracja metody wystąpienia zawiera `override` modyfikator, metoda zastępuje dziedziczną metodę wirtualną z tą samą sygnaturą.</span><span class="sxs-lookup"><span data-stu-id="51ac0-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="51ac0-550">Podczas gdy deklaracja metody wirtualnej wprowadza nową metodę, Deklaracja metody przesłonięcia specjalizacji istniejącej dziedziczonej metody wirtualnej, dostarczając nową implementację tej metody.</span><span class="sxs-lookup"><span data-stu-id="51ac0-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="51ac0-551">Metoda ***abstrakcyjna*** jest metodą wirtualną bez implementacji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="51ac0-552">Metoda abstrakcyjna jest zadeklarowana z `abstract` modyfikatorem i jest dozwolona tylko w klasie, która również jest `abstract`zadeklarowana.</span><span class="sxs-lookup"><span data-stu-id="51ac0-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="51ac0-553">Metoda abstrakcyjna musi zostać przesłonięta w każdej nieabstrakcyjnej klasie pochodnej.</span><span class="sxs-lookup"><span data-stu-id="51ac0-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="51ac0-554">Poniższy przykład deklaruje klasę `Expression`abstrakcyjną, która reprezentuje węzeł drzewa wyrażenia i trzy `Constant`klasy pochodne,, `VariableReference`i `Operation`, które implementują węzły drzewa wyrażeń dla stałych, zmienna odwołania i operacje arytmetyczne.</span><span class="sxs-lookup"><span data-stu-id="51ac0-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="51ac0-555">(Jest to podobne do, ale nie należy mylić z typami drzewa wyrażenia wprowadzonymi w [typach drzew wyrażeń](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="51ac0-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="51ac0-556">Poprzednie cztery klasy mogą służyć do modelowania wyrażeń arytmetycznych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="51ac0-557">Na przykład przy użyciu wystąpień tych klas wyrażenie `x + 3` może być reprezentowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="51ac0-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="51ac0-558">Metoda wystąpienia jest wywoływana w celu obliczenia `double` danego wyrażenia i utworzenia wartości. `Expression` `Evaluate`</span><span class="sxs-lookup"><span data-stu-id="51ac0-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="51ac0-559">Metoda przyjmuje jako argument a `Hashtable` , który zawiera nazwy zmiennych (jako klucze wpisów) i wartości (jako wartości wpisów).</span><span class="sxs-lookup"><span data-stu-id="51ac0-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="51ac0-560">`Evaluate` Metoda jest wirtualną metodą abstrakcyjną, co oznacza, że nieabstrakcyjne klasy pochodne muszą zastąpić je, aby zapewnić rzeczywistą implementację.</span><span class="sxs-lookup"><span data-stu-id="51ac0-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="51ac0-561">`Constant` Implementacjapoprostu`Evaluate` zwraca przechowywaną stałą.</span><span class="sxs-lookup"><span data-stu-id="51ac0-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="51ac0-562">Implementacja `VariableReference`programu wyszukuje nazwę zmiennej w elemencie Hashtable i zwraca wartość wynikową.</span><span class="sxs-lookup"><span data-stu-id="51ac0-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="51ac0-563">Implementacja najpierw szacuje lewy i prawy operand (cyklicznie wywołując ich `Evaluate` metody), a następnie wykonuje daną operację arytmetyczną. `Operation`</span><span class="sxs-lookup"><span data-stu-id="51ac0-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="51ac0-564">Poniższy program używa `Expression` klas do obliczenia wyrażenia `x * (y + 2)` pod kątem różnych wartości `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="51ac0-565">Przeciążanie metody</span><span class="sxs-lookup"><span data-stu-id="51ac0-565">Method overloading</span></span>

<span data-ttu-id="51ac0-566">***Przeciążanie*** metod pozwala wielu metodom w tej samej klasie mieć taką samą nazwę, o ile mają unikatowe podpisy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="51ac0-567">Podczas kompilowania wywołania przeciążonej metody kompilator używa ***rozdzielczości przeciążenia*** do określenia konkretnej metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="51ac0-568">Rozpoznanie przeciążenia umożliwia znalezienie jednej metody, która najlepiej pasuje do argumentów lub zgłasza błąd, jeśli nie można znaleźć pojedynczego najlepszego dopasowania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="51ac0-569">W poniższym przykładzie przedstawiono sposób rozwiązywania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="51ac0-570">Komentarz dla każdego wywołania `Main` metody pokazuje, która metoda jest faktycznie wywoływana.</span><span class="sxs-lookup"><span data-stu-id="51ac0-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="51ac0-571">Jak pokazano w przykładzie, dana metoda może być zawsze wybierana przez jawne rzutowanie argumentów na dokładne typy parametrów i/lub jawne dostarczenie argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="51ac0-572">Inne elementy członkowskie funkcji</span><span class="sxs-lookup"><span data-stu-id="51ac0-572">Other function members</span></span>

<span data-ttu-id="51ac0-573">Elementy członkowskie, które zawierają kod wykonywalny, są określane zbiorczo jako ***elementy członkowskie*** klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="51ac0-574">W poprzedniej sekcji opisano metody, które są podstawowym rodzajem elementów członkowskich funkcji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="51ac0-575">W tej sekcji opisano inne rodzaje składowych funkcji obsługiwane przez C#: konstruktory, właściwości, indeksatory, zdarzenia, operatory i destruktory.</span><span class="sxs-lookup"><span data-stu-id="51ac0-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="51ac0-576">Poniższy kod przedstawia klasę generyczną o nazwie `List<T>`, która implementuje rozwijaną listę obiektów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="51ac0-577">Klasa zawiera kilka przykładów typowych rodzajów elementów członkowskich funkcji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="51ac0-578">Konstruktorów</span><span class="sxs-lookup"><span data-stu-id="51ac0-578">Constructors</span></span>

<span data-ttu-id="51ac0-579">C#obsługuje zarówno konstruktory wystąpienia, jak i statyczne.</span><span class="sxs-lookup"><span data-stu-id="51ac0-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="51ac0-580">***Konstruktor wystąpienia*** jest członkiem, który implementuje akcje wymagane do zainicjowania wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="51ac0-581">***Statyczny Konstruktor*** jest członkiem, który implementuje akcje wymagane do zainicjowania samej klasy podczas pierwszego ładowania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="51ac0-582">Konstruktor jest zadeklarowany jak metoda bez zwracanego typu i o takiej samej nazwie jak zawierająca klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="51ac0-583">Jeśli deklaracja konstruktora zawiera `static` modyfikator, deklaruje Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="51ac0-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="51ac0-584">W przeciwnym razie deklaruje Konstruktor wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="51ac0-585">Konstruktory wystąpień mogą być przeciążone.</span><span class="sxs-lookup"><span data-stu-id="51ac0-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="51ac0-586">Na przykład `List<T>` Klasa deklaruje dwa konstruktory wystąpień, jeden bez parametrów i jeden, który `int` pobiera parametr.</span><span class="sxs-lookup"><span data-stu-id="51ac0-586">For example, the `List<T>` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="51ac0-587">Konstruktory wystąpień są wywoływane przy `new` użyciu operatora.</span><span class="sxs-lookup"><span data-stu-id="51ac0-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="51ac0-588">Poniższe instrukcje przydzielą `List<string>` dwa wystąpienia przy użyciu każdego konstruktora `List` klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-588">The following statements allocate two `List<string>` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="51ac0-589">W przeciwieństwie do innych elementów członkowskich, konstruktory wystąpień nie są dziedziczone, a Klasa nie ma konstruktorów wystąpień innych niż zadeklarowane w klasie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="51ac0-590">Jeśli nie podano konstruktora wystąpienia dla klasy, zostanie automatycznie podana pusta wartość bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="51ac0-591">properties</span><span class="sxs-lookup"><span data-stu-id="51ac0-591">Properties</span></span>

<span data-ttu-id="51ac0-592">***Właściwości*** są naturalnym rozszerzeniem pól.</span><span class="sxs-lookup"><span data-stu-id="51ac0-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="51ac0-593">Oba są nazwanymi członkami ze skojarzonymi typami, a składnia dostępu do pól i właściwości jest taka sama.</span><span class="sxs-lookup"><span data-stu-id="51ac0-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="51ac0-594">Jednak w przeciwieństwie do pól właściwości nie oznacza lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="51ac0-595">Zamiast tego właściwości mają metody ***dostępu*** określające instrukcje, które mają być wykonywane, gdy ich wartości są odczytywane lub zapisywane.</span><span class="sxs-lookup"><span data-stu-id="51ac0-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="51ac0-596">Właściwość jest zadeklarowana jako pole, z tą różnicą, że deklaracja kończy `get` się akcesorem i/ `set` lub akcesorem zapisanym `{` między ogranicznikami i `}` zamiast kończyć się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="51ac0-597">Właściwość, która `get` ma zarówno akcesor, `set` jak i akcesora `get` , jest ***właściwością do odczytu i zapisu***, właściwość, która ma tylko metodę dostępu, jest ***właściwością tylko do odczytu***i właściwość, która `set` ma tylko metodę dostępu, jest ***Właściwość tylko do zapisu***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="51ac0-598">Metoda `get` dostępu odpowiada metodzie bez parametrów z wartością zwracaną typu właściwości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="51ac0-599">Poza elementem docelowym przypisania, gdy właściwość jest przywoływana w wyrażeniu, `get` metoda dostępu do właściwości jest wywoływana w celu obliczenia wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="51ac0-600">Metoda dostępu odpowiada metodzie z pojedynczym parametrem o nazwie `value` i bez zwracanego typu. `set`</span><span class="sxs-lookup"><span data-stu-id="51ac0-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="51ac0-601">Gdy właściwość jest przywoływana jako element docelowy przypisania lub jako operand `++` lub `--`, `set` metoda dostępu jest wywoływana z argumentem, który dostarcza nową wartość.</span><span class="sxs-lookup"><span data-stu-id="51ac0-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="51ac0-602">Klasa deklaruje dwie `Count` właściwości i `Capacity`, które są tylko do odczytu i odczytu i zapisu. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="51ac0-602">The `List<T>` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="51ac0-603">Poniżej przedstawiono przykład użycia tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="51ac0-604">Podobnie jak pola i metody, C# obsługuje zarówno właściwości wystąpienia, jak i właściwości statyczne.</span><span class="sxs-lookup"><span data-stu-id="51ac0-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="51ac0-605">Właściwości statyczne są zadeklarowane za `static` pomocą modyfikatora, a właściwości wystąpienia są deklarowane bez użycia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="51ac0-606">Metody dostępu właściwości mogą być wirtualne.</span><span class="sxs-lookup"><span data-stu-id="51ac0-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="51ac0-607">Gdy Deklaracja właściwości zawiera `virtual`modyfikator, `abstract`lub `override` , ma zastosowanie do akcesorów właściwości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="51ac0-608">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="51ac0-608">Indexers</span></span>

<span data-ttu-id="51ac0-609">***Indeksator*** jest członkiem, który umożliwia indeksowanie obiektów w taki sam sposób jak w przypadku tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="51ac0-610">Indeksator jest zadeklarowany jak właściwość, z tą różnicą, że `this` po nazwie składowej następuje lista parametrów zapisywana między `[` ogranicznikami i `]`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="51ac0-611">Parametry są dostępne w metodach dostępu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="51ac0-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="51ac0-612">Podobnie jak w przypadku właściwości, indeksatory mogą być tylko do odczytu i zapisu, tylko do odczytu i do zapisu, a Akcesory dla indeksatora mogą być wirtualne.</span><span class="sxs-lookup"><span data-stu-id="51ac0-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="51ac0-613">Klasa deklaruje pojedynczy indeksator do odczytu i zapisu, który `int` pobiera parametr. `List`</span><span class="sxs-lookup"><span data-stu-id="51ac0-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="51ac0-614">Indeksator umożliwia indeksowanie `List` wystąpień z `int` wartościami.</span><span class="sxs-lookup"><span data-stu-id="51ac0-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="51ac0-615">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="51ac0-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="51ac0-616">Indeksatory mogą być przeciążone, co oznacza, że Klasa może deklarować wiele indeksatorów, o ile liczba lub typy ich parametrów różnią się.</span><span class="sxs-lookup"><span data-stu-id="51ac0-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="51ac0-617">Events</span><span class="sxs-lookup"><span data-stu-id="51ac0-617">Events</span></span>

<span data-ttu-id="51ac0-618">***Zdarzenie*** jest członkiem, który umożliwia klasy lub obiektowi dostarczanie powiadomień.</span><span class="sxs-lookup"><span data-stu-id="51ac0-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="51ac0-619">Zdarzenie jest zadeklarowane jak pole, z tą różnicą, że `event` deklaracja zawiera słowo kluczowe i typ musi być typem delegata.</span><span class="sxs-lookup"><span data-stu-id="51ac0-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="51ac0-620">W obrębie klasy, która deklaruje element członkowski zdarzenia, zdarzenie zachowuje się podobnie jak pole typu delegata (pod warunkiem, że zdarzenie nie jest abstrakcyjne i nie deklaruje metod dostępu).</span><span class="sxs-lookup"><span data-stu-id="51ac0-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="51ac0-621">W polu jest przechowywane odwołanie do delegata, który reprezentuje programy obsługi zdarzeń, które zostały dodane do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="51ac0-622">Jeśli nie ma żadnych dojść do zdarzeń, pole jest `null`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="51ac0-623">Klasa deklaruje pojedynczy element członkowski zdarzenia o `Changed`nazwie, który wskazuje, że dodano nowy element do listy. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="51ac0-623">The `List<T>` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="51ac0-624">Zdarzenie jest zgłaszane `OnChanged` przez metodę wirtualną, która najpierw sprawdza, czy zdarzenie jest `null` (oznacza, że nie ma żadnych programów obsługi). `Changed`</span><span class="sxs-lookup"><span data-stu-id="51ac0-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="51ac0-625">Pojęcie związane z wywoływaniem zdarzenia jest dokładnie równoważne do wywołania delegata reprezentowanego przez zdarzenie, co oznacza, że nie istnieją żadne specjalne konstrukcje języka do wywoływania zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="51ac0-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="51ac0-626">Klienci reagują na zdarzenia za poorednictwem ***programów obsługi zdarzeń***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="51ac0-627">Procedury obsługi zdarzeń są dołączane `+=` przy użyciu operatora i usuwane `-=` przy użyciu operatora.</span><span class="sxs-lookup"><span data-stu-id="51ac0-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="51ac0-628">Poniższy przykład dołącza procedurę obsługi zdarzeń do `Changed` zdarzenia. `List<string>`</span><span class="sxs-lookup"><span data-stu-id="51ac0-628">The following example attaches an event handler to the `Changed` event of a `List<string>`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="51ac0-629">W przypadku zaawansowanych scenariuszy, w których wymagana jest kontrola bazowego magazynu zdarzenia, deklaracja zdarzenia może jawnie dostarczyć `add` i `remove` akcesorów, które `set` są nieco podobne do metody dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="51ac0-630">Operatory</span><span class="sxs-lookup"><span data-stu-id="51ac0-630">Operators</span></span>

<span data-ttu-id="51ac0-631">***Operator*** jest członkiem, który definiuje znaczenie zastosowania określonego operatora wyrażenia do wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="51ac0-632">Można zdefiniować trzy rodzaje operatorów: operatory jednoargumentowe, operatory binarne i operatory konwersji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="51ac0-633">Wszystkie operatory muszą być zadeklarowane `public` jako `static`i.</span><span class="sxs-lookup"><span data-stu-id="51ac0-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="51ac0-634">Klasa deklaruje dwa `operator==` operatory i `operator!=`i w ten sposób daje nowe znaczenie dla wyrażeń, które stosują te `List` operatory do wystąpień. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="51ac0-634">The `List<T>` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="51ac0-635">W tym celu operatory definiują równość dwóch `List<T>` wystąpień w porównaniu z poszczególnymi obiektami zawartymi `Equals` przy użyciu metod.</span><span class="sxs-lookup"><span data-stu-id="51ac0-635">Specifically, the operators define equality of two `List<T>` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="51ac0-636">Poniższy przykład używa `==` operatora do porównywania dwóch `List<int>` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="51ac0-636">The following example uses the `==` operator to compare two `List<int>` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="51ac0-637">Pierwsze `Console.WriteLine` dane wyjściowe `True` , ponieważ dwie listy zawierają tę samą liczbę obiektów z tymi samymi wartościami w tej samej kolejności.</span><span class="sxs-lookup"><span data-stu-id="51ac0-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="51ac0-638">`Console.WriteLine` `False` `List<int>` Nie zdefiniowano `operator==`, pierwsze miałoby wynik, ponieważ `a` i odwołuje się do `b` różnych wystąpień. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="51ac0-638">Had `List<T>` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="51ac0-639">Destruktory</span><span class="sxs-lookup"><span data-stu-id="51ac0-639">Destructors</span></span>

<span data-ttu-id="51ac0-640">***Destruktor*** jest członkiem, który implementuje akcje wymagane do zadestruktorowania wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="51ac0-641">Destruktory nie mogą mieć parametrów, nie mogą mieć modyfikatorów dostępności i nie mogą być wywoływane jawnie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="51ac0-642">Destruktor dla wystąpienia jest wywoływany automatycznie podczas wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="51ac0-643">Moduł wyrzucania elementów bezużytecznych jest dozwolony w przypadku podejmowania decyzji o zbieraniu obiektów i destruktorów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="51ac0-644">W odróżnieniu od czasu wywołania destruktorów nie jest deterministyczna, a destruktory mogą być wykonywane na dowolnym wątku.</span><span class="sxs-lookup"><span data-stu-id="51ac0-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="51ac0-645">Z tych i innych powodów klasy powinny implementować destruktory tylko wtedy, gdy żadne inne rozwiązania nie są możliwe.</span><span class="sxs-lookup"><span data-stu-id="51ac0-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="51ac0-646">`using` Instrukcja zawiera lepsze podejście do niszczenia obiektów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="51ac0-647">Struktury</span><span class="sxs-lookup"><span data-stu-id="51ac0-647">Structs</span></span>

<span data-ttu-id="51ac0-648">Podobnie jak klasy, ***struktury*** są strukturami danych, które mogą zawierać składowe danych i składowe funkcji, ale w przeciwieństwie do klas, struktury są typami wartości i nie wymagają przydziału sterty.</span><span class="sxs-lookup"><span data-stu-id="51ac0-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="51ac0-649">Zmienna typu struktury bezpośrednio przechowuje dane struktury, podczas gdy zmienna typu klasy przechowuje odwołanie do obiektu przydzielanego dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="51ac0-650">Typy struktur nie obsługują dziedziczenia określonego przez użytkownika, a wszystkie typy struktur niejawnie dziedziczą `object`po typie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="51ac0-651">Struktury są szczególnie przydatne w przypadku małych struktur danych, które mają semantykę wartości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="51ac0-652">Liczby zespolone, punkty w układzie współrzędnych lub pary klucz-wartość w słowniku są wszystkimi dobrymi przykładami struktur.</span><span class="sxs-lookup"><span data-stu-id="51ac0-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="51ac0-653">Użycie struktur zamiast klas dla małych struktur danych może spowodować znaczną różnicę liczby alokacji pamięci wykonywanej przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="51ac0-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="51ac0-654">Na przykład następujący program tworzy i Inicjuje tablicę z 100 punktów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="51ac0-655">Z `Point` zaimplementowaną klasą, 101 oddzielnych obiektów są tworzone wystąpienia — jeden dla tablicy i jeden dla elementów 100.</span><span class="sxs-lookup"><span data-stu-id="51ac0-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="51ac0-656">Alternatywą jest tworzenie `Point` struktury.</span><span class="sxs-lookup"><span data-stu-id="51ac0-656">An alternative is to make `Point` a struct.</span></span>

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="51ac0-657">Teraz zostanie utworzony tylko jeden obiekt — jeden dla tablicy — i `Point` wystąpienia są przechowywane w wierszu tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="51ac0-658">Konstruktory struktury są wywoływane z `new` operatorem, ale nie oznacza to, że pamięć jest przyznana.</span><span class="sxs-lookup"><span data-stu-id="51ac0-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="51ac0-659">Zamiast dynamicznie przydzielać obiektu i zwracać odwołanie do niego, Konstruktor struktury po prostu zwraca wartość struktury (zazwyczaj w tymczasowej lokalizacji na stosie), a następnie ta wartość jest kopiowana w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="51ac0-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="51ac0-660">Klasy, istnieje możliwość, że dwie zmienne odwołują się do tego samego obiektu, a tym samym możliwe dla operacji na jednej zmiennej, aby wpływać na obiekt, do którego odwołuje się inna zmienna.</span><span class="sxs-lookup"><span data-stu-id="51ac0-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="51ac0-661">W przypadku struktur zmienne każda z nich ma własną kopię danych i nie jest możliwe wykonywanie operacji na nich, aby wpływać na inne.</span><span class="sxs-lookup"><span data-stu-id="51ac0-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="51ac0-662">Na przykład dane wyjściowe generowane przez Poniższy fragment kodu zależą od tego, czy `Point` jest klasą czy strukturą.</span><span class="sxs-lookup"><span data-stu-id="51ac0-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="51ac0-663">Jeśli `Point` jest klasą, dane wyjściowe są `20` spowodowane `a` `b` tym samym obiektem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="51ac0-664">Jeśli `Point` jest strukturą, dane wyjściowe wynikają `10` z faktu `a` , `b` że przypisanie do tworzy kopię wartości i ta kopia nie ma wpływ na kolejne przypisanie do `a.x`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="51ac0-665">Poprzedni przykład wyróżnia dwa ograniczenia struktur.</span><span class="sxs-lookup"><span data-stu-id="51ac0-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="51ac0-666">Najpierw kopiowanie całej struktury jest zwykle mniej wydajne niż kopiowanie odwołania do obiektu, dlatego przypisanie i wartość przekazywanie parametrów mogą być bardziej kosztowne z strukturami niż w przypadku typów referencyjnych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="51ac0-667">Po drugie, z `ref` wyjątkiem `out` parametrów i, nie jest możliwe tworzenie odwołań do struktur, które wywołują zasady ich użycia w wielu sytuacjach.</span><span class="sxs-lookup"><span data-stu-id="51ac0-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="51ac0-668">Tablice</span><span class="sxs-lookup"><span data-stu-id="51ac0-668">Arrays</span></span>

<span data-ttu-id="51ac0-669">***Tablica*** to struktura danych zawierająca pewną liczbę zmiennych, do których dostęp jest uzyskiwany za pomocą obliczonych indeksów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="51ac0-670">Zmienne zawarte w tablicy, nazywane również ***elementami*** tablicy, są tego samego typu, a ten typ jest nazywany ***typem elementu*** tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="51ac0-671">Typy tablic są typami odwołań, a deklaracja zmiennej tablicowej po prostu ustawia miejsce na odwołanie do wystąpienia tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="51ac0-672">Rzeczywiste wystąpienia tablicy są tworzone dynamicznie w czasie wykonywania przy użyciu `new` operatora.</span><span class="sxs-lookup"><span data-stu-id="51ac0-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="51ac0-673">Operacja określa długość nowego wystąpienia tablicy, które jest następnie naprawione dla okresu istnienia wystąpienia. `new`</span><span class="sxs-lookup"><span data-stu-id="51ac0-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="51ac0-674">Indeksy elementów z zakresu tablicy od `0` do. `Length - 1`</span><span class="sxs-lookup"><span data-stu-id="51ac0-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="51ac0-675">Operator automatycznie inicjuje elementy tablicy do ich wartości domyślnych, które na przykład są równe zero dla wszystkich typów liczbowych i `null` dla wszystkich typów odwołań. `new`</span><span class="sxs-lookup"><span data-stu-id="51ac0-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="51ac0-676">Poniższy przykład tworzy tablicę `int` elementów, Inicjuje tablicę i drukuje zawartość tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="51ac0-677">Ten przykład tworzy i działa na ***tablicy jednowymiarowej***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="51ac0-678">C#obsługuje również ***tablice***wielowymiarowe.</span><span class="sxs-lookup"><span data-stu-id="51ac0-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="51ac0-679">Liczba wymiarów typu tablicy, znana również jako ***ranga*** typu tablicy, jest taka, a liczba przecinkiów zapisywana między nawiasami kwadratowymi typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="51ac0-680">Poniższy przykład przydziela jednowymiarową, dwuwymiarową i trójwymiarową tablicę.</span><span class="sxs-lookup"><span data-stu-id="51ac0-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="51ac0-681">Tablica zawiera 10 elementów `a2` , tablica zawiera 50 (10 × 5 `a3` ) elementów, a tablica zawiera 100 (10 × 5 x 2) elementów. `a1`</span><span class="sxs-lookup"><span data-stu-id="51ac0-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="51ac0-682">Typ elementu tablicy może być dowolnego typu, łącznie z typem tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="51ac0-683">Tablica z elementami typu tablicy jest czasami nazywana ***tablicą nieregularną*** , ponieważ długości tablic elementów nie wszystkie muszą być takie same.</span><span class="sxs-lookup"><span data-stu-id="51ac0-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="51ac0-684">Poniższy przykład alokuje tablicę `int`tablic:</span><span class="sxs-lookup"><span data-stu-id="51ac0-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="51ac0-685">Pierwszy wiersz tworzy tablicę z trzema elementami, każdy typ `int[]` i każdy z początkową `null`wartością.</span><span class="sxs-lookup"><span data-stu-id="51ac0-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="51ac0-686">Kolejne wiersze inicjują następnie trzy elementy z odwołaniami do poszczególnych wystąpień tablicy o różnej długości.</span><span class="sxs-lookup"><span data-stu-id="51ac0-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="51ac0-687">Operator zezwala na określenie wartości początkowych elementów tablicy przy użyciu ***inicjatora tablicy***, który jest listą wyrażeń pisanych ogranicznikami `{` i. `}` `new`</span><span class="sxs-lookup"><span data-stu-id="51ac0-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="51ac0-688">Poniższy przykład przydziela i inicjuje `int[]` z trzema elementami.</span><span class="sxs-lookup"><span data-stu-id="51ac0-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="51ac0-689">Należy zauważyć, że długość tablicy jest wywnioskowana na podstawie liczby wyrażeń między `{` i. `}`</span><span class="sxs-lookup"><span data-stu-id="51ac0-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="51ac0-690">Deklaracje zmiennej lokalnej i pola mogą być skrócone w taki sposób, aby nie trzeba było przestawiać tego typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="51ac0-691">Oba poprzednie przykłady są równoważne z następującymi:</span><span class="sxs-lookup"><span data-stu-id="51ac0-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="51ac0-692">Interfejsy</span><span class="sxs-lookup"><span data-stu-id="51ac0-692">Interfaces</span></span>

<span data-ttu-id="51ac0-693">***Interfejs*** definiuje kontrakt, który może być zaimplementowany przez klasy i struktury.</span><span class="sxs-lookup"><span data-stu-id="51ac0-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="51ac0-694">Interfejs może zawierać metody, właściwości, zdarzenia i indeksatory.</span><span class="sxs-lookup"><span data-stu-id="51ac0-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="51ac0-695">Interfejs nie dostarcza implementacji elementów członkowskich, które definiuje — tylko określa elementy członkowskie, które muszą być dostarczone przez klasy lub struktury, które implementują interfejs.</span><span class="sxs-lookup"><span data-stu-id="51ac0-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="51ac0-696">Interfejsy mogą wykorzystywać ***wielokrotne dziedziczenie***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="51ac0-697">W poniższym przykładzie interfejs `IComboBox` dziedziczy z obu `ITextBox` i `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="51ac0-698">Klasy i struktury mogą implementować wiele interfejsów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="51ac0-699">W poniższym przykładzie Klasa `EditBox` implementuje zarówno `IControl` , jak i `IDataBound`.</span><span class="sxs-lookup"><span data-stu-id="51ac0-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="51ac0-700">Gdy Klasa lub struktura implementuje określony interfejs, wystąpienia tej klasy lub struktury mogą być niejawnie konwertowane na typ tego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="51ac0-701">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="51ac0-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="51ac0-702">W przypadkach, gdy wystąpienie nie jest statycznie znane do implementacji określonego interfejsu, można użyć rzutowania typu dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="51ac0-703">Na przykład następujące instrukcje używają rzutowania typu dynamicznego do uzyskiwania implementacji obiektu `IControl` i `IDataBound` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="51ac0-704">Ponieważ rzeczywisty typ obiektu to `EditBox`, Rzutowanie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="51ac0-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="51ac0-705">W `EditBox` poprzedniej `public` klasie `Paint` Metoda `IControl` `IDataBound` z interfejsu i Metodazinterfejsusąimplementowanezapomocąelementówczłonkowskich.`Bind`</span><span class="sxs-lookup"><span data-stu-id="51ac0-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="51ac0-706">C#obsługuje również ***jawne implementacje elementu członkowskiego interfejsu***, za pomocą którego Klasa lub struktura może uniknąć `public`tworzenia elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="51ac0-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="51ac0-707">Implementacja jawnego elementu członkowskiego interfejsu jest zapisywana przy użyciu w pełni kwalifikowanej nazwy elementu członkowskiego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="51ac0-708">Na przykład `EditBox` Klasa może `IControl.Paint` zaimplementować metody i `IDataBound.Bind` przy użyciu jawnych implementacji elementu członkowskiego interfejsu w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="51ac0-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="51ac0-709">Dostęp do jawnych elementów członkowskich interfejsu można uzyskać tylko za pośrednictwem typu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="51ac0-710">Na przykład implementacja `IControl.Paint` dostarczonych przez poprzednią `EditBox` klasę może być `EditBox` wywoływana tylko przez pierwsze `IControl` przekonwertowanie odwołania do typu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="51ac0-711">Wyliczenia</span><span class="sxs-lookup"><span data-stu-id="51ac0-711">Enums</span></span>

<span data-ttu-id="51ac0-712">***Typ wyliczeniowy*** to odrębny typ wartości zawierający zestaw nazwanych stałych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="51ac0-713">Poniższy przykład deklaruje i używa typu wyliczeniowego `Color` o nazwie z trzema `Red`stałymi wartościami `Blue`,, `Green`i.</span><span class="sxs-lookup"><span data-stu-id="51ac0-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="51ac0-714">Każdy typ wyliczeniowy ma odpowiedni typ całkowity nazywany ***typem podstawowym*** typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="51ac0-715">Typ wyliczeniowy, który nie deklaruje jawnie typu podstawowego, ma typ `int`podstawowy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="51ac0-716">Format magazynu typu wyliczeniowego i zakres możliwych wartości są określane przez jego typ podstawowy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="51ac0-717">Zbiór wartości, dla których można zastosować typ wyliczeniowy, nie jest ograniczony przez elementy członkowskie wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="51ac0-718">W szczególności każda wartość typu podstawowego wyliczenia może być rzutowana na typ wyliczeniowy i jest unikatową prawidłową wartością tego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="51ac0-719">Poniższy przykład deklaruje typ wyliczeniowy o nazwie `Alignment` z `sbyte`typem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="51ac0-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="51ac0-720">Jak pokazano w poprzednim przykładzie, Deklaracja elementu członkowskiego wyliczenia może zawierać wyrażenie stałe, które określa wartość elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="51ac0-721">Wartość stała dla każdego elementu członkowskiego wyliczenia musi znajdować się w zakresie bazowego typu wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="51ac0-722">Jeśli deklaracja elementu członkowskiego wyliczenia nie określa jawnie wartości, element członkowski otrzymuje wartość zero (jeśli jest to pierwszy element członkowski typu enum) lub wartość w postaci jednokrotnie poprzedzającej składową wyliczenia plus jeden.</span><span class="sxs-lookup"><span data-stu-id="51ac0-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="51ac0-723">Wartości wyliczeniowe mogą być konwertowane na wartości całkowite i odwrotnie przy użyciu rzutowania typu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="51ac0-724">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="51ac0-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="51ac0-725">Wartość domyślna dowolnego typu wyliczeniowego jest wartością całkowitą zero przekonwertowaną na typ wyliczeniowy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="51ac0-726">W przypadkach, gdy zmienne są automatycznie inicjowane do wartości domyślnej, jest to wartość nadana zmiennym typów wyliczeniowych.</span><span class="sxs-lookup"><span data-stu-id="51ac0-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="51ac0-727">Aby wartość domyślna typu wyliczeniowego była łatwo dostępna, literał `0` niejawnie konwertowany na dowolny typ wyliczeniowy.</span><span class="sxs-lookup"><span data-stu-id="51ac0-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="51ac0-728">W ten sposób można używać następujących sposobów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="51ac0-729">Delegaty</span><span class="sxs-lookup"><span data-stu-id="51ac0-729">Delegates</span></span>

<span data-ttu-id="51ac0-730">***Typ delegata*** reprezentuje odwołania do metod z określoną listą parametrów i zwracanym typem.</span><span class="sxs-lookup"><span data-stu-id="51ac0-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="51ac0-731">Delegaty umożliwiają traktowanie metod jako jednostek, które mogą być przypisane do zmiennych i przekazane jako parametry.</span><span class="sxs-lookup"><span data-stu-id="51ac0-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="51ac0-732">Delegaty są podobne do koncepcji wskaźników funkcji, które znajdują się w innych językach, ale w przeciwieństwie do wskaźników funkcji Delegaty są zorientowane obiektowo i są bezpieczne dla typów.</span><span class="sxs-lookup"><span data-stu-id="51ac0-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="51ac0-733">Poniższy przykład deklaruje i używa typu delegata o `Function`nazwie.</span><span class="sxs-lookup"><span data-stu-id="51ac0-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="51ac0-734">Wystąpienie `Function` typu delegata może odwoływać się do dowolnej metody, która `double` przyjmuje argument i zwraca `double` wartość.</span><span class="sxs-lookup"><span data-stu-id="51ac0-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="51ac0-735">Metoda odnosi `Function` się do elementów a `double[]`, zwracając `double[]` wynik. `Apply`</span><span class="sxs-lookup"><span data-stu-id="51ac0-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="51ac0-736">Metoda jest używana do zastosowania `double[]`trzech różnych funkcji do. `Apply` `Main`</span><span class="sxs-lookup"><span data-stu-id="51ac0-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="51ac0-737">Delegat może odwoływać się do metody statycznej (takiej `Square` jak `Math.Sin` lub w poprzednim przykładzie) lub metody `m.Multiply` wystąpienia (na przykład w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="51ac0-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="51ac0-738">Delegat, który odwołuje się do metody wystąpienia, odwołuje się również do określonego obiektu i gdy metoda wystąpienia jest wywoływana za pomocą delegata `this` , ten obiekt zostaje w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="51ac0-739">Delegatów można także tworzyć za pomocą funkcji anonimowych, które są "metodami wbudowanymi", które są tworzone na bieżąco.</span><span class="sxs-lookup"><span data-stu-id="51ac0-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="51ac0-740">Funkcje anonimowe mogą zobaczyć zmienne lokalne otaczających metod.</span><span class="sxs-lookup"><span data-stu-id="51ac0-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="51ac0-741">W ten sposób przykład mnożnika można napisać łatwiej, bez użycia `Multiplier` klasy:</span><span class="sxs-lookup"><span data-stu-id="51ac0-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="51ac0-742">Interesująca i przydatna właściwość delegata polega na tym, że nie wie ani nie posługuje się klasą metody, do której się odwołuje; wszystkie te kwestie polegają na tym, że metoda przywoływana ma te same parametry i zwracany typ jako delegat.</span><span class="sxs-lookup"><span data-stu-id="51ac0-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="51ac0-743">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="51ac0-743">Attributes</span></span>

<span data-ttu-id="51ac0-744">Typy, elementy członkowskie i inne jednostki w C# programie obsługują Modyfikatory kontrolujące pewne aspekty ich zachowania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="51ac0-745">Na przykład dostępność metody jest `public`kontrolowana za pomocą modyfikatorów, `protected`, `internal`i `private` .</span><span class="sxs-lookup"><span data-stu-id="51ac0-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="51ac0-746">C#służy do uogólniania tej funkcji w taki sposób, aby typy informacji deklaratywnych zdefiniowanych przez użytkownika mogły być dołączane do jednostek programu i pobierane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="51ac0-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="51ac0-747">Programy określają te dodatkowe informacje deklaracyjne przez definiowanie i używanie ***atrybutów***.</span><span class="sxs-lookup"><span data-stu-id="51ac0-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="51ac0-748">Poniższy przykład deklaruje `HelpAttribute` atrybut, który może być umieszczony w jednostkach programu, aby udostępnić linki do powiązanej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="51ac0-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="51ac0-749">Wszystkie klasy atrybutów pochodzą z `System.Attribute` klasy bazowej dostarczonej przez .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="51ac0-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="51ac0-750">Atrybuty mogą być stosowane, dając ich nazwę, wraz z dowolnymi argumentami, wewnątrz nawiasów kwadratowych tuż przed skojarzoną deklaracją.</span><span class="sxs-lookup"><span data-stu-id="51ac0-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="51ac0-751">Jeśli nazwa atrybutu zostanie zakończona `Attribute`, ta część nazwy może zostać pominięta, gdy występuje odwołanie do atrybutu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="51ac0-752">Na przykład `HelpAttribute` atrybut może być używany w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="51ac0-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="51ac0-753">Ten `HelpAttribute` przykład dołącza `Widget` do klasyi`HelpAttribute` drugi do metodywklasie.`Display`</span><span class="sxs-lookup"><span data-stu-id="51ac0-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="51ac0-754">Publiczne konstruktory klasy atrybutów kontrolują informacje, które muszą być dostarczone, gdy atrybut jest dołączony do jednostki programu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="51ac0-755">Dodatkowe informacje można uzyskać, odwołując się do właściwości publicznego odczytu i zapisu klasy atrybutów (takich jak odwołanie do `Topic` właściwości wcześniej).</span><span class="sxs-lookup"><span data-stu-id="51ac0-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="51ac0-756">Poniższy przykład pokazuje, jak informacje o atrybutach danej jednostki programu mogą być pobierane w czasie wykonywania przy użyciu odbicia.</span><span class="sxs-lookup"><span data-stu-id="51ac0-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="51ac0-757">Gdy określony atrybut jest żądany przez odbicie, Konstruktor klasy atrybutu jest wywoływany z informacjami podanymi w źródle programu i zwracane jest wystąpienie atrybutu wynikowego.</span><span class="sxs-lookup"><span data-stu-id="51ac0-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="51ac0-758">Jeśli dodatkowe informacje zostały przekazane za pomocą właściwości, te właściwości są ustawiane na podane wartości przed zwróceniem wystąpienia atrybutu.</span><span class="sxs-lookup"><span data-stu-id="51ac0-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
