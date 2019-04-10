---
ms.openlocfilehash: db10046af5d635b430951679a448e23680b18b87
ms.sourcegitcommit: a19fac74c01a6c3da67d38b2f79527145d4edcbc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426815"
---
# <a name="introduction"></a><span data-ttu-id="ada03-101">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="ada03-101">Introduction</span></span>

<span data-ttu-id="ada03-102">C# (Wymowa: "Zobacz Sharp") to proste, nowoczesnych, zorientowane obiektowo i bezpieczny typowo język programowania.</span><span class="sxs-lookup"><span data-stu-id="ada03-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="ada03-103">C# ma korzenie jego rodziny C w językach i będzie znane w programowaniu w języku C, C++ i Java.</span><span class="sxs-lookup"><span data-stu-id="ada03-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="ada03-104">C# jest standardowym by ECMA International jako ***ECMA 334*** standardowe i ISO/IEC jako ***23270 ISO/IEC*** standardowych.</span><span class="sxs-lookup"><span data-stu-id="ada03-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="ada03-105">Kompilator języka C# firmy Microsoft dla programu .NET Framework jest odpowiadające wykonania obu tych standardach.</span><span class="sxs-lookup"><span data-stu-id="ada03-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="ada03-106">C# to język obiektowy, ale C# dalsze obejmuje obsługę ***zorientowanego na*** programowania.</span><span class="sxs-lookup"><span data-stu-id="ada03-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="ada03-107">Projektowania oprogramowania współczesnych coraz bardziej opiera się na składniki oprogramowania w postaci pakietów niezależna i samoopisujące funkcji.</span><span class="sxs-lookup"><span data-stu-id="ada03-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="ada03-108">Kluczem do takich składników jest, czy stanowią one model programowania za pomocą właściwości, metod i zdarzeń; mają atrybuty, które zapewniają deklaracyjne informacje o składniku; i obejmują one we własnej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="ada03-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="ada03-109">C# zawiera konstrukcji języka, aby bezpośrednio obsługują te pojęcia wprowadzania C# bardzo języka naturalnego, w której do tworzenia i używania składników oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="ada03-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="ada03-110">Kilka C# funkcje pomocy do tworzenia niezawodnych i trwałych aplikacji: ***Wyrzucanie elementów bezużytecznych*** automatycznie odzyskuje pamięć zajęta przez nieużywanych obiektów; ***wyjątków*** ze strukturą i rozszerzalny podejście do wykrywania błędów i odzyskiwania; i ***bezpieczny*** projekt języka umożliwia odczytywanie niezainicjowane zmienne Indeksowanie tablic poza ich zakresem, lub wykonać unchecked typu rzutowania.</span><span class="sxs-lookup"><span data-stu-id="ada03-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="ada03-111">C# ma ***unified system typów***.</span><span class="sxs-lookup"><span data-stu-id="ada03-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="ada03-112">Wszystkie C# typów, w tym typów podstawowych takich jak `int` i `double`, dziedziczą z jednym elementem głównym `object` typu.</span><span class="sxs-lookup"><span data-stu-id="ada03-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="ada03-113">W efekcie wszystkie typy udostępnić zestaw typowych operacji, a wartości dowolnego typu mogą być przechowywane, transportowane i wykonywane są operacje w sposób ciągły.</span><span class="sxs-lookup"><span data-stu-id="ada03-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="ada03-114">Ponadto, C# obsługuje typy odwołań zdefiniowanych przez użytkownika i typów wartości, umożliwiając dynamiczna alokacja obiektów, a także magazynu w tekście uproszczone struktur.</span><span class="sxs-lookup"><span data-stu-id="ada03-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="ada03-115">Aby upewnić się, że programy C# i biblioteki mogą z czasem ewoluować w sposób zgodny, znacznie większym naciskiem został umieszczony na ***versioning*** w języku C# w projekcie.</span><span class="sxs-lookup"><span data-stu-id="ada03-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="ada03-116">Wiele języków programowania interesuje tego problemu, a w rezultacie, programy napisane w tych języków podziału częściej niż to konieczne, w przypadku nowszych wersjach zależne biblioteki zostały wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="ada03-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="ada03-117">Aspektów projektu C# w, które zostały bezpośrednio wpływa uwagi dotyczące wersji zawierać oddzielne `virtual` i `override` modyfikatorów, reguły dla rozwiązania przeciążenia metody i pomoc techniczna dla deklaracji elementu członkowskiego interfejsu jawnego.</span><span class="sxs-lookup"><span data-stu-id="ada03-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="ada03-118">W pozostałej części w tym rozdziale opisano podstawowe funkcje języka C#.</span><span class="sxs-lookup"><span data-stu-id="ada03-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="ada03-119">Mimo że dalszych rozdziałach opisano regułami i wyjątkami zorientowane na szczegółów i czasami matematyczne w sposób, w tym rozdziale dokłada starań uściślenia i zwięzłości kosztem informacje były kompletne.</span><span class="sxs-lookup"><span data-stu-id="ada03-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="ada03-120">Celem jest zapewnienie czytnik zawiera wprowadzenie do języka, który ułatwi pisanie programów wczesne i Odczyt rozdziały nowsze.</span><span class="sxs-lookup"><span data-stu-id="ada03-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="ada03-121">Cześć ludzie</span><span class="sxs-lookup"><span data-stu-id="ada03-121">Hello world</span></span>

<span data-ttu-id="ada03-122">Program "Hello, World" tradycyjnie umożliwia wprowadzenie języka programowania.</span><span class="sxs-lookup"><span data-stu-id="ada03-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="ada03-123">Poniżej przedstawiono w języku C#:</span><span class="sxs-lookup"><span data-stu-id="ada03-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="ada03-124">Pliki źródłowe C# zazwyczaj mają rozszerzenie pliku `.cs`.</span><span class="sxs-lookup"><span data-stu-id="ada03-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="ada03-125">Przy założeniu, że program "Hello, World" jest przechowywany w pliku `hello.cs`, program może być kompilowane przez kompilator Microsoft C# przy użyciu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="ada03-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="ada03-126">który tworzy zestaw pliku wykonywalnego o nazwie `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="ada03-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="ada03-127">Dane wyjściowe generowane przez tę aplikację po jej uruchomieniu</span><span class="sxs-lookup"><span data-stu-id="ada03-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="ada03-128">Program "Hello, World" rozpoczyna się od `using` dyrektywę, który odwołuje się do `System` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="ada03-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="ada03-129">Przestrzenie nazw zawierają hierarchiczny sposób organizowania programów C# i biblioteki.</span><span class="sxs-lookup"><span data-stu-id="ada03-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="ada03-130">Przestrzenie nazw zawierają typy i inne przestrzenie nazw — na przykład `System` przestrzeń nazw zawiera wiele typów, takie jak `Console` klasy, do którego odwołuje się program i wiele innych przestrzeniach nazw, takich jak `IO` i `Collections`.</span><span class="sxs-lookup"><span data-stu-id="ada03-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="ada03-131">A `using` dyrektywę, który odwołuje się do danej przestrzeni nazw umożliwia niekwalifikowanej korzystanie z typów, które są elementami członkowskimi tej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="ada03-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="ada03-132">Z powodu `using` dyrektywy, można użyć programu `Console.WriteLine` jako skrót dla `System.Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="ada03-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="ada03-133">`Hello` Klasy zadeklarowanej przez program "Hello, World" zawiera jeden element członkowski metodę o nazwie `Main`.</span><span class="sxs-lookup"><span data-stu-id="ada03-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="ada03-134">`Main` Metody jest zadeklarowana za pomocą `static` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="ada03-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="ada03-135">Podczas gdy metody wystąpienia można odwołania konkretnego wystąpienia obiektu otaczającej przy użyciu słowa kluczowego `this`, metody statyczne działać bez odwołania do określonego obiektu.</span><span class="sxs-lookup"><span data-stu-id="ada03-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="ada03-136">Zgodnie z Konwencją statyczną metodę o nazwie `Main` służy jako punkt wejścia programu.</span><span class="sxs-lookup"><span data-stu-id="ada03-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="ada03-137">Dane wyjściowe programu jest generowany przez `WriteLine` metody `Console` klasy w `System` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="ada03-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="ada03-138">Ta klasa jest zapewniana przez biblioteki klas .NET Framework, które domyślnie są automatycznie dołączane do kompilatora Microsoft C#.</span><span class="sxs-lookup"><span data-stu-id="ada03-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="ada03-139">Należy pamiętać, że C# sam nie ma biblioteki oddzielne środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="ada03-140">Zamiast tego program .NET Framework jest biblioteka środowiska uruchomieniowego języka C#.</span><span class="sxs-lookup"><span data-stu-id="ada03-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="ada03-141">Struktura programu</span><span class="sxs-lookup"><span data-stu-id="ada03-141">Program structure</span></span>

<span data-ttu-id="ada03-142">Kluczowe założenia organizacji w języku C# są ***programy***, ***przestrzenie nazw***, ***typy***, ***członków***, i ***zestawy***.</span><span class="sxs-lookup"><span data-stu-id="ada03-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="ada03-143">C# programy składają się z jednego lub więcej plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="ada03-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="ada03-144">Programy deklarują typy, które zawierają elementy członkowskie i mogą być organizowane w przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="ada03-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="ada03-145">Klasy i interfejsy są przykłady typów.</span><span class="sxs-lookup"><span data-stu-id="ada03-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="ada03-146">Pola, metody, właściwości i zdarzenia są przykłady elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="ada03-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="ada03-147">Po skompilowaniu C# programy są fizycznie spakowane do zestawów.</span><span class="sxs-lookup"><span data-stu-id="ada03-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="ada03-148">Zestawy zwykle z rozszerzeniem pliku `.exe` lub `.dll`, w zależności od tego, czy zaimplementować ***aplikacje*** lub ***biblioteki***.</span><span class="sxs-lookup"><span data-stu-id="ada03-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="ada03-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="ada03-149">The example</span></span>

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
<span data-ttu-id="ada03-150">deklaruje klasę o nazwie `Stack` w przestrzeni nazw o nazwie `Acme.Collections`.</span><span class="sxs-lookup"><span data-stu-id="ada03-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="ada03-151">W pełni kwalifikowana nazwa tej klasy to `Acme.Collections.Stack`.</span><span class="sxs-lookup"><span data-stu-id="ada03-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="ada03-152">Klasa zawiera kilka elementów członkowskich: pole o nazwie `top`, dwie metody o nazwie `Push` i `Pop`i klasę zagnieżdżoną o nazwie `Entry`.</span><span class="sxs-lookup"><span data-stu-id="ada03-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="ada03-153">`Entry` Dodatkowo klasa zawiera trzy elementy członkowskie: pole o nazwie `next`, pole o nazwie `data`i konstruktora.</span><span class="sxs-lookup"><span data-stu-id="ada03-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="ada03-154">Przy założeniu, że kod źródłowy przykładu znajduje się w pliku `acme.cs`, wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="ada03-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="ada03-155">kompiluje przykład jako biblioteki (kod bez `Main` punktu wejścia) i tworzy zestaw o nazwie `acme.dll`.</span><span class="sxs-lookup"><span data-stu-id="ada03-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="ada03-156">Zestawy zawierają kod wykonywalny w formie ***języka pośredniego*** instrukcje (IL), a także informacji o symbolach w formie ***metadanych***.</span><span class="sxs-lookup"><span data-stu-id="ada03-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="ada03-157">Przed wykonaniem jego kodu IL w zestawie jest automatycznie konwertowany na kod specyficzny dla procesora przez kompilator just in Time (JIT) środowiska uruchomieniowego języka wspólnego platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="ada03-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="ada03-158">Ponieważ zestaw jest samoopisujący jednostka zawierająca kod i metadanych funkcji, nie ma potrzeby dla `#include` dyrektyw i pliki nagłówkowe w języku C#.</span><span class="sxs-lookup"><span data-stu-id="ada03-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="ada03-159">Typy publiczne i elementów członkowskich znajdujących się w określonym zestawie są udostępniane w programie C# poprzez odwołanie do tego zestawu podczas kompilowania kodu programu.</span><span class="sxs-lookup"><span data-stu-id="ada03-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="ada03-160">Na przykład ten program używa `Acme.Collections.Stack` klasy z `acme.dll` zestawu:</span><span class="sxs-lookup"><span data-stu-id="ada03-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

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
<span data-ttu-id="ada03-161">Jeśli program jest przechowywany w pliku `test.cs`, gdy `test.cs` jest kompilowany, `acme.dll` zestawu można odwoływać się za pomocą kompilatora `/r` opcji:</span><span class="sxs-lookup"><span data-stu-id="ada03-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="ada03-162">Spowoduje to utworzenie pliku wykonywalnego zestaw o nazwie `test.exe`, który, po uruchomieniu tworzy dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="ada03-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="ada03-163">C# umożliwia tekst źródłowy program ma być przechowywany w kilku plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="ada03-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="ada03-164">Wielu plików języka C# program jest skompilowany, wszystkie pliki źródłowe są przetwarzane razem, gdy pliki źródłowe mogą swobodnie odwoływać się do siebie nawzajem — model jest tak, jakby wszystkie pliki źródłowe zostały połączone w jeden duży plik przed przetworzeniem.</span><span class="sxs-lookup"><span data-stu-id="ada03-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="ada03-165">Deklaracje przechodzenia do przodu nigdy nie są wymagane w języku C#, ponieważ z nielicznymi wyjątkami, kolejności deklaracji jest niewielka.</span><span class="sxs-lookup"><span data-stu-id="ada03-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="ada03-166">C# nie istnieje limit pliku źródłowego do deklarowania tylko jeden typ publiczny ani nie wymaga nazwy pliku źródłowego, aby dopasować typ zadeklarowany w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="ada03-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="ada03-167">Typy i zmienne</span><span class="sxs-lookup"><span data-stu-id="ada03-167">Types and variables</span></span>

<span data-ttu-id="ada03-168">Istnieją dwa rodzaje typów w języku C#: ***typy wartości*** i ***typy odwołań***.</span><span class="sxs-lookup"><span data-stu-id="ada03-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="ada03-169">Zmienne typu wartości zawierają bezpośrednio swoje dane, natomiast zmiennych typu referencyjnego są przechowywane odwołania do swoich danych, te ostatnie są nazywane obiektów.</span><span class="sxs-lookup"><span data-stu-id="ada03-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="ada03-170">W przypadku typów referencyjnych jest możliwe w dwóch zmiennych odwoływać się do tego samego obiektu i dlatego możliwe dla operacji na jednej zmiennej miały wpływ na obiekt odwołuje się druga zmienna.</span><span class="sxs-lookup"><span data-stu-id="ada03-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="ada03-171">Z typami wartości zmiennych każda ma własne kopię danych i nie jest możliwe dla operacji na jednym wpłynie na inne (z wyjątkiem w przypadku właściwości `ref` i `out` zmiennych parametrów).</span><span class="sxs-lookup"><span data-stu-id="ada03-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="ada03-172">Podzielono na typach wartości języka C# w ***typów prostych***, ***typach wyliczeniowych***, ***typy struktury***, i ***typów dopuszczających wartości zerowe***, a przez odwołanie w C# typy są podzielone na ***klasy typów***, ***typy interfejsów***, ***tablicy typów***, i ***typy delegatów***.</span><span class="sxs-lookup"><span data-stu-id="ada03-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="ada03-173">Poniższa tabela zawiera omówienie w systemie typu C#.</span><span class="sxs-lookup"><span data-stu-id="ada03-173">The following table provides an overview of C#'s type system.</span></span>

| __<span data-ttu-id="ada03-174">Kategoria</span><span class="sxs-lookup"><span data-stu-id="ada03-174">Category</span></span>__    |                 | __<span data-ttu-id="ada03-175">Opis</span><span class="sxs-lookup"><span data-stu-id="ada03-175">Description</span></span>__ |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="ada03-176">Typy wartości</span><span class="sxs-lookup"><span data-stu-id="ada03-176">Value types</span></span>     | <span data-ttu-id="ada03-177">Typy proste</span><span class="sxs-lookup"><span data-stu-id="ada03-177">Simple types</span></span>    | <span data-ttu-id="ada03-178">Podpisana całkowitego: `sbyte`, `short`, `int`,</span><span class="sxs-lookup"><span data-stu-id="ada03-178">Signed integral: `sbyte`, `short`, `int`,</span></span> `long` |
|                 |                 | <span data-ttu-id="ada03-179">Całkowite bez znaku: `byte`, `ushort`, `uint`,</span><span class="sxs-lookup"><span data-stu-id="ada03-179">Unsigned integral: `byte`, `ushort`, `uint`,</span></span> `ulong` |
|                 |                 | <span data-ttu-id="ada03-180">Znaki Unicode:</span><span class="sxs-lookup"><span data-stu-id="ada03-180">Unicode characters:</span></span> `char` |
|                 |                 | <span data-ttu-id="ada03-181">Liczba zmiennoprzecinkowa IEEE: `float`,</span><span class="sxs-lookup"><span data-stu-id="ada03-181">IEEE floating point: `float`,</span></span> `double` |
|                 |                 | <span data-ttu-id="ada03-182">Decimal wysokiej precyzji:</span><span class="sxs-lookup"><span data-stu-id="ada03-182">High-precision decimal:</span></span> `decimal` |
|                 |                 | <span data-ttu-id="ada03-183">Boolean:</span><span class="sxs-lookup"><span data-stu-id="ada03-183">Boolean:</span></span> `bool` |
|                 | <span data-ttu-id="ada03-184">Typach wyliczeniowych</span><span class="sxs-lookup"><span data-stu-id="ada03-184">Enum types</span></span>      | <span data-ttu-id="ada03-185">Typy zdefiniowane przez użytkownika w postaci</span><span class="sxs-lookup"><span data-stu-id="ada03-185">User-defined types of the form</span></span> `enum E {...}` |
|                 | <span data-ttu-id="ada03-186">Typy — struktura</span><span class="sxs-lookup"><span data-stu-id="ada03-186">Struct types</span></span>    | <span data-ttu-id="ada03-187">Typy zdefiniowane przez użytkownika w postaci</span><span class="sxs-lookup"><span data-stu-id="ada03-187">User-defined types of the form</span></span> `struct S {...}` |
|                 | <span data-ttu-id="ada03-188">Typy dopuszczające wartości null</span><span class="sxs-lookup"><span data-stu-id="ada03-188">Nullable types</span></span>  | <span data-ttu-id="ada03-189">Rozszerzenia z innych typów wartości za pomocą `null` wartość</span><span class="sxs-lookup"><span data-stu-id="ada03-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="ada03-190">Typy odwołań</span><span class="sxs-lookup"><span data-stu-id="ada03-190">Reference types</span></span> | <span data-ttu-id="ada03-191">Typy klas</span><span class="sxs-lookup"><span data-stu-id="ada03-191">Class types</span></span>     | <span data-ttu-id="ada03-192">Ultimate klasa bazowa innych typów:</span><span class="sxs-lookup"><span data-stu-id="ada03-192">Ultimate base class of all other types:</span></span> `object` |
|                 |                 | <span data-ttu-id="ada03-193">Ciągi Unicode:</span><span class="sxs-lookup"><span data-stu-id="ada03-193">Unicode strings:</span></span> `string` |
|                 |                 | <span data-ttu-id="ada03-194">Typy zdefiniowane przez użytkownika w postaci</span><span class="sxs-lookup"><span data-stu-id="ada03-194">User-defined types of the form</span></span> `class C {...}` |
|                 | <span data-ttu-id="ada03-195">Typy interfejsów</span><span class="sxs-lookup"><span data-stu-id="ada03-195">Interface types</span></span> | <span data-ttu-id="ada03-196">Typy zdefiniowane przez użytkownika w postaci</span><span class="sxs-lookup"><span data-stu-id="ada03-196">User-defined types of the form</span></span> `interface I {...}` |
|                 | <span data-ttu-id="ada03-197">Typy tablic</span><span class="sxs-lookup"><span data-stu-id="ada03-197">Array types</span></span>     | <span data-ttu-id="ada03-198">Jedno - i są one wielowymiarowe, na przykład `int[]` i</span><span class="sxs-lookup"><span data-stu-id="ada03-198">Single- and multi-dimensional, for example, `int[]` and</span></span> `int[,]` |
|                 | <span data-ttu-id="ada03-199">Typy delegatów</span><span class="sxs-lookup"><span data-stu-id="ada03-199">Delegate types</span></span>  | <span data-ttu-id="ada03-200">Typy zdefiniowane przez użytkownika w postaci np.</span><span class="sxs-lookup"><span data-stu-id="ada03-200">User-defined types of the form e.g.</span></span> `delegate int  D(...)` |

<span data-ttu-id="ada03-201">Osiem typów całkowitych zapewniają obsługę wartości 8-bitową, 16-bitowych, 32-bitowych i 64-bitowe podpisane lub niepodpisane formularza.</span><span class="sxs-lookup"><span data-stu-id="ada03-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="ada03-202">Polecenie dwóch liczb zmiennoprzecinkowych typów, `float` i `double`, są reprezentowane przy użyciu 32-bitowe o pojedynczej dokładności i 64-bitowe o podwójnej dokładności IEEE 754 formatów.</span><span class="sxs-lookup"><span data-stu-id="ada03-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="ada03-203">`decimal` Typ to typ danych 128-bitowych, odpowiedni do obliczeń finansowych i walutowych.</span><span class="sxs-lookup"><span data-stu-id="ada03-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="ada03-204">C# w `bool` typ jest używany do reprezentowania wartości logicznych — wartości, które są albo `true` lub `false`.</span><span class="sxs-lookup"><span data-stu-id="ada03-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="ada03-205">Znakowe i przetwarzania w języku C# przy użyciu kodowania Unicode.</span><span class="sxs-lookup"><span data-stu-id="ada03-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="ada03-206">`char` Typ reprezentuje jednostkę kodu UTF-16 i `string` typu reprezentuje sekwencję jednostki kodu UTF-16.</span><span class="sxs-lookup"><span data-stu-id="ada03-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="ada03-207">W poniższej tabeli przedstawiono typy liczbowe języka C# firmy.</span><span class="sxs-lookup"><span data-stu-id="ada03-207">The following table summarizes C#'s numeric types.</span></span>


| __<span data-ttu-id="ada03-208">Kategoria</span><span class="sxs-lookup"><span data-stu-id="ada03-208">Category</span></span>__      | __<span data-ttu-id="ada03-209">Bity</span><span class="sxs-lookup"><span data-stu-id="ada03-209">Bits</span></span>__ | __<span data-ttu-id="ada03-210">Typ</span><span class="sxs-lookup"><span data-stu-id="ada03-210">Type</span></span>__  | __<span data-ttu-id="ada03-211">Zakres/dokładności</span><span class="sxs-lookup"><span data-stu-id="ada03-211">Range/Precision</span></span>__ |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="ada03-212">Całkowite podpisem</span><span class="sxs-lookup"><span data-stu-id="ada03-212">Signed integral</span></span>   | <span data-ttu-id="ada03-213">8</span><span class="sxs-lookup"><span data-stu-id="ada03-213">8</span></span>        | `sbyte`   | <span data-ttu-id="ada03-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="ada03-214">-128...127</span></span> |
|                   | <span data-ttu-id="ada03-215">16</span><span class="sxs-lookup"><span data-stu-id="ada03-215">16</span></span>       | `short`   | <span data-ttu-id="ada03-216">-32,768...32,767</span><span class="sxs-lookup"><span data-stu-id="ada03-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="ada03-217">32</span><span class="sxs-lookup"><span data-stu-id="ada03-217">32</span></span>       | `int`     | <span data-ttu-id="ada03-218">-2,147,483,648...2,147,483,647</span><span class="sxs-lookup"><span data-stu-id="ada03-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="ada03-219">64</span><span class="sxs-lookup"><span data-stu-id="ada03-219">64</span></span>       | `long`    | <span data-ttu-id="ada03-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span><span class="sxs-lookup"><span data-stu-id="ada03-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="ada03-221">Całkowite bez znaku</span><span class="sxs-lookup"><span data-stu-id="ada03-221">Unsigned integral</span></span> | <span data-ttu-id="ada03-222">8</span><span class="sxs-lookup"><span data-stu-id="ada03-222">8</span></span>        | `byte`    | <span data-ttu-id="ada03-223">0...255</span><span class="sxs-lookup"><span data-stu-id="ada03-223">0...255</span></span> |
|                   | <span data-ttu-id="ada03-224">16</span><span class="sxs-lookup"><span data-stu-id="ada03-224">16</span></span>       | `ushort`  | <span data-ttu-id="ada03-225">0...65,535</span><span class="sxs-lookup"><span data-stu-id="ada03-225">0...65,535</span></span> |
|                   | <span data-ttu-id="ada03-226">32</span><span class="sxs-lookup"><span data-stu-id="ada03-226">32</span></span>       | `uint`    | <span data-ttu-id="ada03-227">0...4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="ada03-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="ada03-228">64</span><span class="sxs-lookup"><span data-stu-id="ada03-228">64</span></span>       | `ulong`   | <span data-ttu-id="ada03-229">0...18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="ada03-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="ada03-230">Liczba zmiennoprzecinkowa</span><span class="sxs-lookup"><span data-stu-id="ada03-230">Floating point</span></span>    | <span data-ttu-id="ada03-231">32</span><span class="sxs-lookup"><span data-stu-id="ada03-231">32</span></span>       | `float`   | <span data-ttu-id="ada03-232">1,5 x 10 ^ −45 do 3,4 x 10 ^ 38, dokładności 7 cyfr</span><span class="sxs-lookup"><span data-stu-id="ada03-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="ada03-233">64</span><span class="sxs-lookup"><span data-stu-id="ada03-233">64</span></span>       | `double`  | <span data-ttu-id="ada03-234">W wersji 5.0 x 10 ^ −324 do wersji 1.7 x 10 ^ 308, dokładności 15 cyfr</span><span class="sxs-lookup"><span data-stu-id="ada03-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="ada03-235">Wartość dziesiętna</span><span class="sxs-lookup"><span data-stu-id="ada03-235">Decimal</span></span>           | <span data-ttu-id="ada03-236">128</span><span class="sxs-lookup"><span data-stu-id="ada03-236">128</span></span>      | `decimal` | <span data-ttu-id="ada03-237">1.0 x 10 ^ −28 do 7,9 x 10 ^ 28, 28 cyfr precyzji</span><span class="sxs-lookup"><span data-stu-id="ada03-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="ada03-238">C# programy użyj ***wpisz deklaracje*** do tworzenia nowych typów.</span><span class="sxs-lookup"><span data-stu-id="ada03-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="ada03-239">Deklaracja typu Określa nazwę i elementy członkowskie nowego typu.</span><span class="sxs-lookup"><span data-stu-id="ada03-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="ada03-240">Pięć języka C# w kategorie typów są definiowane przez użytkownika: klasy, typy, typy struktury, typy interfejsów, typach wyliczeniowych i typy delegatów.</span><span class="sxs-lookup"><span data-stu-id="ada03-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="ada03-241">Typ klasy definiuje strukturę danych, który zawiera elementy członkowskie danych (pola) i składowe funkcji (metody, właściwości i inne).</span><span class="sxs-lookup"><span data-stu-id="ada03-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="ada03-242">Typy klas obsługuje pojedyncze dziedziczenie i polimorfizmu, mechanizmów, według której rozszerzać i specialize klas bazowych klas pochodnych.</span><span class="sxs-lookup"><span data-stu-id="ada03-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="ada03-243">Typ struktury jest podobny do typu klasy, w tym, że reprezentuje strukturę z elementów członkowskich danych i składowe funkcji.</span><span class="sxs-lookup"><span data-stu-id="ada03-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="ada03-244">W przeciwieństwie do klasy, struktury są typami wartości i nie wymagają alokacji sterty.</span><span class="sxs-lookup"><span data-stu-id="ada03-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="ada03-245">Typy struktury nie obsługują dziedziczenia określonych przez użytkownika, a wszystkie typy struktury niejawnie dziedziczą z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="ada03-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="ada03-246">Typ interfejsu definiuje kontrakt jako nazwany zestaw funkcji publicznych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="ada03-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="ada03-247">Klasa lub struktura, która implementuje interfejs należy podać implementacji interfejsu funkcji członków.</span><span class="sxs-lookup"><span data-stu-id="ada03-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="ada03-248">Interfejs może dziedziczyć z wielu interfejsach podstawowych, a klasa lub struktura może zaimplementować wiele interfejsów.</span><span class="sxs-lookup"><span data-stu-id="ada03-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="ada03-249">To typ delegowany reprezentuje odwołania do metod z określoną listą parametrów i typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="ada03-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="ada03-250">Delegatów można umożliwić traktować metod jako jednostki, które mogą być przypisane do zmiennych i przekazywane jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ada03-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="ada03-251">Delegaty są podobne do koncepcji wskaźników funkcji, w przeciwieństwie do innych języków, ale w przeciwieństwie do wskaźników funkcji, obiekty delegowane są zorientowane obiektowo i bezpieczny typowo.</span><span class="sxs-lookup"><span data-stu-id="ada03-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="ada03-252">Klasy, struktury, interfejsów i delegatów typy wszystkie elementy rodzajowe pomocy technicznej, według których mogą być parametryzowane przy użyciu innych typów.</span><span class="sxs-lookup"><span data-stu-id="ada03-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="ada03-253">Typ wyliczeniowy jest typem samodzielnym z nazwanych stałych.</span><span class="sxs-lookup"><span data-stu-id="ada03-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="ada03-254">Każdy typ wyliczenia ma typu podstawowego musi być jednym z ośmiu typów całkowitych.</span><span class="sxs-lookup"><span data-stu-id="ada03-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="ada03-255">Zbiór wartości Typ wyliczeniowy jest taka sama jak zestaw wartości typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="ada03-256">C# obsługuje dimensional jednym i wielu tablic dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="ada03-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="ada03-257">W przeciwieństwie do typów wymienionych powyżej typy tablic ma zadeklarowany, zanim będzie można ich użyć.</span><span class="sxs-lookup"><span data-stu-id="ada03-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="ada03-258">Zamiast tego typy tablicowe są konstruowane wykonując nazwę typu z nawiasami kwadratowymi.</span><span class="sxs-lookup"><span data-stu-id="ada03-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="ada03-259">Na przykład `int[]` to Jednowymiarowa tablica `int`, `int[,]` to dwuwymiarowa tablica `int`, i `int[][]` to Jednowymiarowa tablica tablice jednowymiarowe `int`.</span><span class="sxs-lookup"><span data-stu-id="ada03-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="ada03-260">Typy dopuszczające wartości zerowe również nie trzeba być zadeklarowany, zanim będzie można ich użyć.</span><span class="sxs-lookup"><span data-stu-id="ada03-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="ada03-261">Dla każdego typu wartości niedopuszczającym wartości `T` Brak odpowiedniego typu dopuszczającego wartość null `T?`, który może zawierać dodatkowe wartości `null`.</span><span class="sxs-lookup"><span data-stu-id="ada03-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="ada03-262">Na przykład `int?` to typ, który może zawierać żadnych 32-bitowej liczby całkowitej lub wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="ada03-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="ada03-263">W systemie typu C# jest jednolita w taki sposób, że wartość dowolnego typu może być traktowana jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="ada03-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="ada03-264">Każdy typ w języku C#, bezpośrednio lub pośrednio pochodzi z `object` typ, klasy i `object` jest ultimate klasą bazową wszystkich typów.</span><span class="sxs-lookup"><span data-stu-id="ada03-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="ada03-265">Wartości typu referencyjnego są traktowane jako obiekty poprzez wyświetlanie wartości jako typu `object`.</span><span class="sxs-lookup"><span data-stu-id="ada03-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="ada03-266">Wartości typu wartości są traktowane jako obiekty, wykonując ***pakowania*** i ***Rozpakowywanie*** operacji.</span><span class="sxs-lookup"><span data-stu-id="ada03-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="ada03-267">W poniższym przykładzie `int` wartość jest konwertowana na `object` i wykonać ich kopię ponownie do `int`.</span><span class="sxs-lookup"><span data-stu-id="ada03-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

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
<span data-ttu-id="ada03-268">Gdy wartość typu wartości jest konwertowany na typ `object`, wystąpienie obiektu, nazywany również "pola", jest przeznaczona do przechowywania wartości i kopiuje wartość do tego pola.</span><span class="sxs-lookup"><span data-stu-id="ada03-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="ada03-269">Z drugiej strony, gdy `object` odwołanie jest rzutowany na typ wartości, dokonuje, przywoływanego obiektu jest polem typu poprawnej wartości, i, jeśli sprawdzenie zakończy się powodzeniem, wartość w polu jest kopiowana.</span><span class="sxs-lookup"><span data-stu-id="ada03-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="ada03-270">Firmy ujednoliconego systemie typu C# oznacza, że typy wartości może stać się obiekty "na żądanie."</span><span class="sxs-lookup"><span data-stu-id="ada03-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="ada03-271">Ze względu na ujednolicenie ogólnego przeznaczenia biblioteki, które używają typu `object` mogą być używane z typami odwołań i typy wartości.</span><span class="sxs-lookup"><span data-stu-id="ada03-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="ada03-272">Istnieje kilka rodzajów z ***zmienne*** w języku C#, w tym pól, elementy tablicy, zmienne lokalne i parametry.</span><span class="sxs-lookup"><span data-stu-id="ada03-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="ada03-273">Zmienne reprezentują lokalizacje przechowywania, a co zmienna ma typ, który określa, jakie wartości mogą być przechowywane w zmiennej, jak pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ada03-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| __<span data-ttu-id="ada03-274">Typ zmiennej</span><span class="sxs-lookup"><span data-stu-id="ada03-274">Type of Variable</span></span>__    | __<span data-ttu-id="ada03-275">Zawartość możliwa</span><span class="sxs-lookup"><span data-stu-id="ada03-275">Possible Contents</span></span>__ |
|-------------------------|-----------------------|
| <span data-ttu-id="ada03-276">Typ wartości nieprzyjmujące</span><span class="sxs-lookup"><span data-stu-id="ada03-276">Non-nullable value type</span></span> | <span data-ttu-id="ada03-277">Wartość tego typu dokładnie</span><span class="sxs-lookup"><span data-stu-id="ada03-277">A value of that exact type</span></span> |
| <span data-ttu-id="ada03-278">Typ wartości null</span><span class="sxs-lookup"><span data-stu-id="ada03-278">Nullable value type</span></span>     | <span data-ttu-id="ada03-279">Wartość null lub wartość tego typu dokładnie</span><span class="sxs-lookup"><span data-stu-id="ada03-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="ada03-280">Odwołanie o wartości null, odwołanie do obiektu typu odwołania lub odwołania do wartości spakowanej dowolnego typu wartości</span><span class="sxs-lookup"><span data-stu-id="ada03-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="ada03-281">Typ klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-281">Class type</span></span>              | <span data-ttu-id="ada03-282">Odwołanie o wartości null, odwołanie do wystąpienia tego typu klasy lub odwołanie do wystąpienia klasy pochodnej z typu klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="ada03-283">Typ interfejsu</span><span class="sxs-lookup"><span data-stu-id="ada03-283">Interface type</span></span>          | <span data-ttu-id="ada03-284">Odwołanie o wartości null, odwołanie do typu klasy, która implementuje interfejs typu wystąpienia lub odwołania do wartości spakowanej typu wartości, która implementuje interfejs typu</span><span class="sxs-lookup"><span data-stu-id="ada03-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="ada03-285">Typ tablicy</span><span class="sxs-lookup"><span data-stu-id="ada03-285">Array type</span></span>              | <span data-ttu-id="ada03-286">Odwołanie o wartości null, odwołanie do wystąpienia tego typu tablicy lub odwołanie do wystąpienia typu tablicy zgodne</span><span class="sxs-lookup"><span data-stu-id="ada03-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="ada03-287">Typ delegata</span><span class="sxs-lookup"><span data-stu-id="ada03-287">Delegate type</span></span>           | <span data-ttu-id="ada03-288">Odwołanie o wartości null lub odwołanie do wystąpienia tego typu delegata</span><span class="sxs-lookup"><span data-stu-id="ada03-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="ada03-289">Wyrażenia</span><span class="sxs-lookup"><span data-stu-id="ada03-289">Expressions</span></span>

<span data-ttu-id="ada03-290">***Wyrażenia*** są konstruowane na podstawie ***operandów*** i ***operatorów***.</span><span class="sxs-lookup"><span data-stu-id="ada03-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="ada03-291">Operatory wyrażenia wskazują operacje do zastosowania dla operandów.</span><span class="sxs-lookup"><span data-stu-id="ada03-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="ada03-292">Przykłady operatorów to `+`, `-`, `*`, `/`i `new`.</span><span class="sxs-lookup"><span data-stu-id="ada03-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="ada03-293">Przykładami operandów są literały, pola, zmienne lokalne i wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="ada03-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="ada03-294">Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególne operatory.</span><span class="sxs-lookup"><span data-stu-id="ada03-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="ada03-295">Na przykład, wyrażenie `x + y * z` jest oceniane jako `x + (y * z)`, ponieważ operator `*` ma wyższy priorytet niż operator `+`.</span><span class="sxs-lookup"><span data-stu-id="ada03-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="ada03-296">Większość operatorów może być ***przeciążona***.</span><span class="sxs-lookup"><span data-stu-id="ada03-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="ada03-297">Przeciążanie operatora umożliwia określanie definiowanych przez użytkownika implementacji operatorów dla operacji, w których jeden lub oba operandy mają typ struktury lub klasy zdefiniowanej przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ada03-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="ada03-298">Poniższa tabela zawiera podsumowanie operatory C# firmy, lista kategorii operatora w kolejność pierwszeństwa od najwyższego do najniższego.</span><span class="sxs-lookup"><span data-stu-id="ada03-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="ada03-299">Operatory w tej samej kategorii mają takie samo pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="ada03-299">Operators in the same category have equal precedence.</span></span>


| __<span data-ttu-id="ada03-300">Kategoria</span><span class="sxs-lookup"><span data-stu-id="ada03-300">Category</span></span>__                     | __<span data-ttu-id="ada03-301">Wyrażenie</span><span class="sxs-lookup"><span data-stu-id="ada03-301">Expression</span></span>__    | __<span data-ttu-id="ada03-302">Opis</span><span class="sxs-lookup"><span data-stu-id="ada03-302">Description</span></span>__ |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="ada03-303">Podstawowy</span><span class="sxs-lookup"><span data-stu-id="ada03-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="ada03-304">Dostęp do elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="ada03-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="ada03-305">Wywołanie metody i delegata</span><span class="sxs-lookup"><span data-stu-id="ada03-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="ada03-306">Dostęp do tablicy i indeksatora</span><span class="sxs-lookup"><span data-stu-id="ada03-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="ada03-307">Postinkrementacja</span><span class="sxs-lookup"><span data-stu-id="ada03-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="ada03-308">Postdekrementacja</span><span class="sxs-lookup"><span data-stu-id="ada03-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="ada03-309">Utworzenie obiektu i delegata</span><span class="sxs-lookup"><span data-stu-id="ada03-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="ada03-310">Utworzenie obiektu za pomocą inicjatora</span><span class="sxs-lookup"><span data-stu-id="ada03-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="ada03-311">Inicjator obiektu anonimowego</span><span class="sxs-lookup"><span data-stu-id="ada03-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="ada03-312">Do utworzenia tablicy</span><span class="sxs-lookup"><span data-stu-id="ada03-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="ada03-313">Uzyskaj `System.Type` dla obiektu</span><span class="sxs-lookup"><span data-stu-id="ada03-313">Obtain `System.Type` object for</span></span> `T` |
|                                  | `checked(x)`      | <span data-ttu-id="ada03-314">Obliczenie wyrażenia w kontekście sprawdzanym</span><span class="sxs-lookup"><span data-stu-id="ada03-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="ada03-315">Obliczenie wyrażenia w kontekście niesprawdzanym</span><span class="sxs-lookup"><span data-stu-id="ada03-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="ada03-316">Uzyskanie wartości domyślnej typu</span><span class="sxs-lookup"><span data-stu-id="ada03-316">Obtain default value of type</span></span> `T` |
|                                  | `delegate {...}`  | <span data-ttu-id="ada03-317">Funkcja anonimowa (metoda anonimowa)</span><span class="sxs-lookup"><span data-stu-id="ada03-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="ada03-318">Jednoargumentowy</span><span class="sxs-lookup"><span data-stu-id="ada03-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="ada03-319">Tożsamość</span><span class="sxs-lookup"><span data-stu-id="ada03-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="ada03-320">Negacja</span><span class="sxs-lookup"><span data-stu-id="ada03-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="ada03-321">Negacja logiczna</span><span class="sxs-lookup"><span data-stu-id="ada03-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="ada03-322">Negacja bitowa</span><span class="sxs-lookup"><span data-stu-id="ada03-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="ada03-323">Preinkrementacja</span><span class="sxs-lookup"><span data-stu-id="ada03-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="ada03-324">Predekrementacja</span><span class="sxs-lookup"><span data-stu-id="ada03-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="ada03-325">Jawnie przekonwertować `x` na typ</span><span class="sxs-lookup"><span data-stu-id="ada03-325">Explicitly convert `x` to type</span></span> `T` |
|                                  | `await x`         | <span data-ttu-id="ada03-326">Asynchronicznie poczekaj na ukończenie `x`</span><span class="sxs-lookup"><span data-stu-id="ada03-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="ada03-327">Mnożeniowy</span><span class="sxs-lookup"><span data-stu-id="ada03-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="ada03-328">Mnożenie</span><span class="sxs-lookup"><span data-stu-id="ada03-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="ada03-329">Dzielenie</span><span class="sxs-lookup"><span data-stu-id="ada03-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="ada03-330">Reszta</span><span class="sxs-lookup"><span data-stu-id="ada03-330">Remainder</span></span> |
| <span data-ttu-id="ada03-331">Dodatku</span><span class="sxs-lookup"><span data-stu-id="ada03-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="ada03-332">Dodawanie, łączenie ciągów, łączenie delegatów</span><span class="sxs-lookup"><span data-stu-id="ada03-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="ada03-333">Odejmowanie, usuwanie delegata</span><span class="sxs-lookup"><span data-stu-id="ada03-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="ada03-334">Shift</span><span class="sxs-lookup"><span data-stu-id="ada03-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="ada03-335">Przesunięcie w lewo</span><span class="sxs-lookup"><span data-stu-id="ada03-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="ada03-336">Przesunięcie w prawo</span><span class="sxs-lookup"><span data-stu-id="ada03-336">Shift right</span></span> |
| <span data-ttu-id="ada03-337">Relacyjne i badania typu</span><span class="sxs-lookup"><span data-stu-id="ada03-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="ada03-338">Mniejsze niż</span><span class="sxs-lookup"><span data-stu-id="ada03-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="ada03-339">Większe niż</span><span class="sxs-lookup"><span data-stu-id="ada03-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="ada03-340">Mniejsze niż lub równe</span><span class="sxs-lookup"><span data-stu-id="ada03-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="ada03-341">Większe niż lub równe</span><span class="sxs-lookup"><span data-stu-id="ada03-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="ada03-342">Zwróć `true` Jeśli `x` jest `T`, `false` inaczej</span><span class="sxs-lookup"><span data-stu-id="ada03-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="ada03-343">Zwróć `x` wpisanych w formie `T`, lub `null` Jeśli `x` nie jest</span><span class="sxs-lookup"><span data-stu-id="ada03-343">Return `x` typed as `T`, or `null` if `x` is not a</span></span> `T` |
| <span data-ttu-id="ada03-344">Równości</span><span class="sxs-lookup"><span data-stu-id="ada03-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="ada03-345">Równa się</span><span class="sxs-lookup"><span data-stu-id="ada03-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="ada03-346">Nie równa się</span><span class="sxs-lookup"><span data-stu-id="ada03-346">Not equal</span></span> |
| <span data-ttu-id="ada03-347">Logicznego AND</span><span class="sxs-lookup"><span data-stu-id="ada03-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="ada03-348">Liczba całkowita bitowe i logicznych logiczne AND</span><span class="sxs-lookup"><span data-stu-id="ada03-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="ada03-349">Logicznego XOR</span><span class="sxs-lookup"><span data-stu-id="ada03-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="ada03-350">Bitowe XOR dla wartości całkowitych, logiczne XOR dla wartości binarnych</span><span class="sxs-lookup"><span data-stu-id="ada03-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="ada03-351">Logicznego OR</span><span class="sxs-lookup"><span data-stu-id="ada03-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="ada03-352">Bitowe OR dla wartości całkowitych, logiczne OR dla wartości binarnych</span><span class="sxs-lookup"><span data-stu-id="ada03-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="ada03-353">Warunkowego AND</span><span class="sxs-lookup"><span data-stu-id="ada03-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="ada03-354">Ocenia `y` tylko wtedy, gdy `x` jest</span><span class="sxs-lookup"><span data-stu-id="ada03-354">Evaluates `y` only if `x` is</span></span> `true` |
| <span data-ttu-id="ada03-355">Warunkowego OR</span><span class="sxs-lookup"><span data-stu-id="ada03-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="ada03-356">Ocenia `y` tylko wtedy, gdy `x` jest</span><span class="sxs-lookup"><span data-stu-id="ada03-356">Evaluates `y` only if `x` is</span></span> `false` |
| <span data-ttu-id="ada03-357">Łączenia wartości null</span><span class="sxs-lookup"><span data-stu-id="ada03-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="ada03-358">Daje w wyniku `y` Jeśli `x` jest `null`, `x` inaczej</span><span class="sxs-lookup"><span data-stu-id="ada03-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="ada03-359">Warunkowe</span><span class="sxs-lookup"><span data-stu-id="ada03-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="ada03-360">Ocenia `y` Jeśli `x` jest `true`, `z` Jeśli `x` jest</span><span class="sxs-lookup"><span data-stu-id="ada03-360">Evaluates `y` if `x` is `true`, `z` if `x` is</span></span> `false` |
| <span data-ttu-id="ada03-361">Przypisania lub funkcji anonimowej</span><span class="sxs-lookup"><span data-stu-id="ada03-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="ada03-362">Przypisanie</span><span class="sxs-lookup"><span data-stu-id="ada03-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="ada03-363">Złożone przypisanie; obsługiwane operatory to</span><span class="sxs-lookup"><span data-stu-id="ada03-363">Compound assignment; supported operators are</span></span> `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | <span data-ttu-id="ada03-364">Funkcja anonimowa (wyrażenie lambda)</span><span class="sxs-lookup"><span data-stu-id="ada03-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="ada03-365">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="ada03-365">Statements</span></span>

<span data-ttu-id="ada03-366">Akcje programu są wyrażone za pomocą ***instrukcji***.</span><span class="sxs-lookup"><span data-stu-id="ada03-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="ada03-367">C# obsługuje wiele rodzajów instrukcji, z których są definiowane zgodnie z osadzonych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="ada03-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="ada03-368">A ***bloku*** zezwala na wiele instrukcji, które ma zostać zapisany w kontekstach, których jest dozwolone pojedynczej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="ada03-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="ada03-369">Blok zawiera listę instrukcji, zapisywane między ogranicznikami `{` i `}`.</span><span class="sxs-lookup"><span data-stu-id="ada03-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="ada03-370">***Instrukcje deklaracji*** są używane do deklarowania stałe i zmienne lokalne.</span><span class="sxs-lookup"><span data-stu-id="ada03-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="ada03-371">***Instrukcje wyrażeń*** są używane do oceny wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="ada03-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="ada03-372">Wyrażenia, które mogą służyć jako stwierdzenia obejmują wywołań metod obiektu alokacje za pomocą `new` operator, za pomocą przypisań `=` i operatorów przypisania złożonego, operacje inkrementacji i dekrementacji przy użyciu `++`i `--` operatory i wyrażenia await.</span><span class="sxs-lookup"><span data-stu-id="ada03-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="ada03-373">***Instrukcje wyboru*** są używane, aby wybrać jeden z wielu możliwych instrukcji do wykonania na podstawie wartości w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="ada03-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="ada03-374">W tej grupie są `if` i `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="ada03-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="ada03-375">***Instrukcje iteracji*** są używane wielokrotnie wykonać osadzona instrukcja.</span><span class="sxs-lookup"><span data-stu-id="ada03-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="ada03-376">W tej grupie są `while`, `do`, `for`, i `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="ada03-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="ada03-377">***Instrukcje skoku*** służą do przekazywania kontroli.</span><span class="sxs-lookup"><span data-stu-id="ada03-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="ada03-378">W tej grupie są `break`, `continue`, `goto`, `throw`, `return`, i `yield` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="ada03-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="ada03-379">`try`... `catch` instrukcja jest używane do przechwytywania wyjątków, które występują podczas wykonywania blok, a `try`... `finally` instrukcja jest używane do określenia kod finalizacji, który jest zawsze wykonywana, czy wystąpił wyjątek, czy nie.</span><span class="sxs-lookup"><span data-stu-id="ada03-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="ada03-380">`checked` i `unchecked` instrukcje są używane do kontrolowania przepełnienia, sprawdzając kontekst dla typu całkowitego operacje arytmetyczne i konwersje.</span><span class="sxs-lookup"><span data-stu-id="ada03-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="ada03-381">`lock` Instrukcja jest używane do uzyskania blokadę wykluczania wzajemnego dla danego obiektu, wykonania instrukcji i następnie zwalnia blokadę.</span><span class="sxs-lookup"><span data-stu-id="ada03-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="ada03-382">`using` Instrukcja jest używane do uzyskania zasobu, należy wykonać instrukcję, a następnie usunąć tego zasobu.</span><span class="sxs-lookup"><span data-stu-id="ada03-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="ada03-383">Poniżej przedstawiono przykłady każdego rodzaju — instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-383">Below are examples of each kind of statement</span></span>

__<span data-ttu-id="ada03-384">Deklaracje zmiennych lokalnych</span><span class="sxs-lookup"><span data-stu-id="ada03-384">Local variable declarations</span></span>__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__<span data-ttu-id="ada03-385">Deklaracji stałej lokalnej</span><span class="sxs-lookup"><span data-stu-id="ada03-385">Local constant declaration</span></span>__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__<span data-ttu-id="ada03-386">Instrukcja wyrażeń</span><span class="sxs-lookup"><span data-stu-id="ada03-386">Expression statement</span></span>__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` <span data-ttu-id="ada03-387">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-387">statement</span></span>__

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


__`switch` <span data-ttu-id="ada03-388">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-388">statement</span></span>__

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

__`while` <span data-ttu-id="ada03-389">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-389">statement</span></span>__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` <span data-ttu-id="ada03-390">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-390">statement</span></span>__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` <span data-ttu-id="ada03-391">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-391">statement</span></span>__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` <span data-ttu-id="ada03-392">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-392">statement</span></span>__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` <span data-ttu-id="ada03-393">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-393">statement</span></span>__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` <span data-ttu-id="ada03-394">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-394">statement</span></span>__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` <span data-ttu-id="ada03-395">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-395">statement</span></span>__

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

__`return` <span data-ttu-id="ada03-396">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-396">statement</span></span>__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` <span data-ttu-id="ada03-397">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-397">statement</span></span>__

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

__`throw` <span data-ttu-id="ada03-398">i `try` instrukcji</span><span class="sxs-lookup"><span data-stu-id="ada03-398">and `try` statements</span></span>__

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

__`checked` <span data-ttu-id="ada03-399">i `unchecked` instrukcji</span><span class="sxs-lookup"><span data-stu-id="ada03-399">and `unchecked` statements</span></span>__

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

__`lock` <span data-ttu-id="ada03-400">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-400">statement</span></span>__

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

__`using` <span data-ttu-id="ada03-401">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="ada03-401">statement</span></span>__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="ada03-402">Klasy i obiekty</span><span class="sxs-lookup"><span data-stu-id="ada03-402">Classes and objects</span></span>

<span data-ttu-id="ada03-403">***Klasy*** są najbardziej podstawowe języka C# dla typów.</span><span class="sxs-lookup"><span data-stu-id="ada03-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="ada03-404">Klasa jest strukturą danych, która łączy stanu (pola) i akcje (metody i innych funkcji elementów członkowskich) w pojedynczą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="ada03-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="ada03-405">Klasa zawiera definicję dla tworzone dynamicznie ***wystąpień*** klasy, nazywana również ***obiektów***.</span><span class="sxs-lookup"><span data-stu-id="ada03-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="ada03-406">Klasy obsługi ***dziedziczenia*** i ***polimorfizm***, mechanizmów zgodnie z którą ***klasy pochodne*** pozwalają rozszerzyć i specjalizują się ***klasbazowych***.</span><span class="sxs-lookup"><span data-stu-id="ada03-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="ada03-407">Nowe klasy są tworzone za pomocą deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-407">New classes are created using class declarations.</span></span> <span data-ttu-id="ada03-408">Deklaracja klasy rozpoczyna się od nagłówka, określający, atrybuty i modyfikatorów klasy, nazwa klasy, klasy podstawowej (jeśli) i interfejsy implementowane przez klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="ada03-409">Nagłówek następuje treści klasy, która składa się z listy deklaracji elementu członkowskiego zapisywane między ogranicznikami `{` i `}`.</span><span class="sxs-lookup"><span data-stu-id="ada03-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="ada03-410">Poniżej przedstawiono deklarację klasie proste o nazwie `Point`:</span><span class="sxs-lookup"><span data-stu-id="ada03-410">The following is a declaration of a simple class named `Point`:</span></span>

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
<span data-ttu-id="ada03-411">Wystąpienia klas są tworzone przy użyciu `new` operatora, który przydziela pamięć dla nowego wystąpienia, wywołuje konstruktor do inicjowania wystąpienia i zwraca odwołanie do wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ada03-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="ada03-412">Poniższe instrukcje utworzysz dwa `Point` obiekty i przechowuje odwołania do tych obiektów w dwóch zmiennych:</span><span class="sxs-lookup"><span data-stu-id="ada03-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="ada03-413">Pamięć zajęta przez obiekt jest automatycznie odzyskana, gdy obiekt nie jest już używana.</span><span class="sxs-lookup"><span data-stu-id="ada03-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="ada03-414">Go nie ma potrzeby ani można jawnie cofnięcie przydziału obiektów w języku C#.</span><span class="sxs-lookup"><span data-stu-id="ada03-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="ada03-415">Elementy członkowskie</span><span class="sxs-lookup"><span data-stu-id="ada03-415">Members</span></span>

<span data-ttu-id="ada03-416">Elementy członkowskie klasy są albo ***statyczne elementy członkowskie*** lub ***wystąpieniami elementów członkowskich***.</span><span class="sxs-lookup"><span data-stu-id="ada03-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="ada03-417">Statyczne elementy członkowskie należą do klas i składowych wystąpienia należą do obiektów (wystąpienia klasy).</span><span class="sxs-lookup"><span data-stu-id="ada03-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="ada03-418">Poniższa tabela zawiera omówienie rodzajów elementów członkowskich, który może zawierać klasę.</span><span class="sxs-lookup"><span data-stu-id="ada03-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| __<span data-ttu-id="ada03-419">Element członkowski</span><span class="sxs-lookup"><span data-stu-id="ada03-419">Member</span></span>__   | __<span data-ttu-id="ada03-420">Opis</span><span class="sxs-lookup"><span data-stu-id="ada03-420">Description</span></span>__ |
|------------  |-----------------|
| <span data-ttu-id="ada03-421">Stałe</span><span class="sxs-lookup"><span data-stu-id="ada03-421">Constants</span></span>    | <span data-ttu-id="ada03-422">Skojarzony z klasą wartości stałych</span><span class="sxs-lookup"><span data-stu-id="ada03-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="ada03-423">Pola</span><span class="sxs-lookup"><span data-stu-id="ada03-423">Fields</span></span>       | <span data-ttu-id="ada03-424">Zmienne klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-424">Variables of the class</span></span> |
| <span data-ttu-id="ada03-425">Metody</span><span class="sxs-lookup"><span data-stu-id="ada03-425">Methods</span></span>      | <span data-ttu-id="ada03-426">Obliczeń i akcji, które mogą być wykonywane przez klasę</span><span class="sxs-lookup"><span data-stu-id="ada03-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="ada03-427">Właściwości</span><span class="sxs-lookup"><span data-stu-id="ada03-427">Properties</span></span>   | <span data-ttu-id="ada03-428">Akcje skojarzone z odczytywaniem i zapisywaniem nazwane właściwości klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="ada03-429">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="ada03-429">Indexers</span></span>     | <span data-ttu-id="ada03-430">Akcje skojarzone z indeksowania wystąpienia klasy, jak tablica</span><span class="sxs-lookup"><span data-stu-id="ada03-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="ada03-431">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="ada03-431">Events</span></span>       | <span data-ttu-id="ada03-432">Powiadomienia, które mogą być generowane przez klasę</span><span class="sxs-lookup"><span data-stu-id="ada03-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="ada03-433">Operatory</span><span class="sxs-lookup"><span data-stu-id="ada03-433">Operators</span></span>    | <span data-ttu-id="ada03-434">Konwersje i operatory wyrażeń obsługiwany przez klasę</span><span class="sxs-lookup"><span data-stu-id="ada03-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="ada03-435">Konstruktorów</span><span class="sxs-lookup"><span data-stu-id="ada03-435">Constructors</span></span> | <span data-ttu-id="ada03-436">Działania wymagane w celu zainicjowania wystąpienia klasy lub samej klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="ada03-437">Destruktory</span><span class="sxs-lookup"><span data-stu-id="ada03-437">Destructors</span></span>  | <span data-ttu-id="ada03-438">Akcje do wykonania przed wystąpienia klasy stałe są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="ada03-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="ada03-439">Types</span><span class="sxs-lookup"><span data-stu-id="ada03-439">Types</span></span>        | <span data-ttu-id="ada03-440">Zagnieżdżone typy zadeklarowane w klasie</span><span class="sxs-lookup"><span data-stu-id="ada03-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="ada03-441">Ułatwienia dostępu</span><span class="sxs-lookup"><span data-stu-id="ada03-441">Accessibility</span></span>

<span data-ttu-id="ada03-442">Każdy członek klasy ma skojarzone ułatwień dostępu, który kontroluje regionach tekst programu, które mogą uzyskiwać dostęp do elementu członkowskiego do.</span><span class="sxs-lookup"><span data-stu-id="ada03-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="ada03-443">Istnieje pięć możliwe formy ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="ada03-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="ada03-444">Ich podsumowanie można znaleźć w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ada03-444">These are summarized in the following table.</span></span>


| __<span data-ttu-id="ada03-445">Ułatwienia dostępu</span><span class="sxs-lookup"><span data-stu-id="ada03-445">Accessibility</span></span>__    | __<span data-ttu-id="ada03-446">Znaczenie</span><span class="sxs-lookup"><span data-stu-id="ada03-446">Meaning</span></span>__ |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="ada03-447">Nie ograniczając dostęp</span><span class="sxs-lookup"><span data-stu-id="ada03-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="ada03-448">Dostęp ograniczony do tej klasy lub klas pochodzących z tej klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="ada03-449">Dostęp ograniczony do tego programu</span><span class="sxs-lookup"><span data-stu-id="ada03-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="ada03-450">Dostęp ograniczony do tego programu lub klasy pochodne względem tej klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="ada03-451">Dostęp ograniczony do klasy</span><span class="sxs-lookup"><span data-stu-id="ada03-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="ada03-452">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="ada03-452">Type parameters</span></span>

<span data-ttu-id="ada03-453">Definicja klasy mogą określać zestaw parametrów typu wykonując nazwę klasy za pomocą nawias ostry otaczający listę nazwy parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="ada03-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="ada03-454">Parametry typu mogą być używane w treści deklaracji klasy do definiowania elementów członkowskich klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="ada03-455">W poniższym przykładzie parametry typu `Pair` są `TFirst` i `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="ada03-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="ada03-456">Typ klasy, który jest zadeklarowany przyjmują parametry typu nosi nazwę typu klasy ogólnej.</span><span class="sxs-lookup"><span data-stu-id="ada03-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="ada03-457">Ogólny może być również typy struktury, interfejsów i delegatów.</span><span class="sxs-lookup"><span data-stu-id="ada03-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="ada03-458">W przypadku klasy ogólnej argumentów typu należy określić dla każdego z parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="ada03-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="ada03-459">Typ ogólny z argumentami typu pod warunkiem, takie jak `Pair<int,string>
    ` powyżej, jest nazywany skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="ada03-459">A generic type with type arguments provided, like `Pair<int,string>
    ` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="ada03-460">Klas podstawowych</span><span class="sxs-lookup"><span data-stu-id="ada03-460">Base classes</span></span>

<span data-ttu-id="ada03-461">Deklaracji klasy może określać klasy bazowej, postępując zgodnie z parametrów nazwy i typu klasy, dwukropek i nazwą klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="ada03-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="ada03-462">Pominięcie specyfikacji klasy bazowej jest taka sama jak pochodząca z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="ada03-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="ada03-463">W poniższym przykładzie klasa bazowa `Point3D` jest `Point`i klasę bazową `Point` jest `object`:</span><span class="sxs-lookup"><span data-stu-id="ada03-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

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
<span data-ttu-id="ada03-464">Klasa dziedziczy członków klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="ada03-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="ada03-465">Dziedziczenie oznacza, że klasa niejawnie zawiera wszystkie elementy członkowskie klasy podstawowej, z wyjątkiem wystąpienia i statyczne konstruktory i destruktory klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="ada03-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="ada03-466">Klasy pochodne mogą dodawać nowych członków do tych, które dziedziczy, ale go nie można usunąć definicji dziedziczonego członka.</span><span class="sxs-lookup"><span data-stu-id="ada03-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="ada03-467">W poprzednim przykładzie `Point3D` dziedziczy `x` i `y` pola z `Point`, a następnie co `Point3D` wystąpienie zawiera trzy pola `x`, `y`, i `z`.</span><span class="sxs-lookup"><span data-stu-id="ada03-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="ada03-468">Istnieje niejawna konwersja z typu klasy dowolny z jej typów klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="ada03-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="ada03-469">W związku z tym zmienna typu klasy mogą odwoływać się do wystąpienia tej klasy lub wystąpienia klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="ada03-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="ada03-470">Na przykład, biorąc pod uwagę poprzedniej deklaracji klasy, zmienna typu `Point` może odwoływać się albo `Point` lub `Point3D`:</span><span class="sxs-lookup"><span data-stu-id="ada03-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="ada03-471">Pola</span><span class="sxs-lookup"><span data-stu-id="ada03-471">Fields</span></span>

<span data-ttu-id="ada03-472">Pole jest zmienną, która jest skojarzona z klasą lub przy użyciu wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="ada03-473">Pole jest zadeklarowane za pomocą `static` definiuje modyfikator ***pole statyczne***.</span><span class="sxs-lookup"><span data-stu-id="ada03-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="ada03-474">Pole statyczne identyfikuje dokładnie jednej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="ada03-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="ada03-475">Niezależnie od tego, jak wiele wystąpień klasy są tworzone istnieje tylko kiedykolwiek jedną kopię pole statyczne.</span><span class="sxs-lookup"><span data-stu-id="ada03-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="ada03-476">Pole zadeklarowana bez `static` definiuje modyfikator ***pola wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="ada03-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="ada03-477">Każde wystąpienie klasy zawiera osobną kopię wszystkie pola wystąpienia tej klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="ada03-478">W poniższym przykładzie, każde wystąpienie `Color` klasa ma osobną kopię `r`, `g`, i `b` wystąpienia pól, ale istnieje tylko jedna kopia `Black`, `White`, `Red`, `Green`, i `Blue` pola statyczne:</span><span class="sxs-lookup"><span data-stu-id="ada03-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

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
<span data-ttu-id="ada03-479">Jak pokazano w poprzednim przykładzie ***pola tylko do odczytu*** może być zadeklarowana z `readonly` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="ada03-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="ada03-480">Przypisanie do `readonly` pola mogą występować wyłącznie jako część deklaracji pola lub za pomocą konstruktora w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="ada03-481">Metody</span><span class="sxs-lookup"><span data-stu-id="ada03-481">Methods</span></span>

<span data-ttu-id="ada03-482">A ***metoda*** jest elementem członkowskim, który implementuje obliczenia lub akcji, które mogą być wykonywane przez obiekt lub klasa.</span><span class="sxs-lookup"><span data-stu-id="ada03-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="ada03-483">***Metody statyczne*** są dostępne za pośrednictwem klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="ada03-484">***Wystąpienie metody*** są dostępne za pośrednictwem wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="ada03-485">Metody mają (prawdopodobnie pusta) lista ***parametry***, zawierające wartości i odwołań do zmiennych przekazywany do metody i ***zwracany typ***, określający typ wartości, obliczana i zwrócony przez Metoda.</span><span class="sxs-lookup"><span data-stu-id="ada03-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="ada03-486">Typ zwracany metody jest `void` Jeśli nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="ada03-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="ada03-487">Podobnie jak typy metody mogą również mieć zestaw parametrów typu, dla których należy określić argumenty typu, gdy wywoływana jest metoda.</span><span class="sxs-lookup"><span data-stu-id="ada03-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="ada03-488">W przeciwieństwie do typów argumentów typu często można wywnioskować z argumentów wywołania metody i nie jest jawnie konieczne.</span><span class="sxs-lookup"><span data-stu-id="ada03-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="ada03-489">***Podpisu*** metody muszą być unikatowe w klasie, w którym zadeklarowany jest metoda.</span><span class="sxs-lookup"><span data-stu-id="ada03-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="ada03-490">Podpis metody zawiera nazwę metody, liczba parametrów typu i liczby, Modyfikatory i typy parametrów.</span><span class="sxs-lookup"><span data-stu-id="ada03-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="ada03-491">Podpis metody nie ma typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="ada03-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="ada03-492">Parametry</span><span class="sxs-lookup"><span data-stu-id="ada03-492">Parameters</span></span>

<span data-ttu-id="ada03-493">Parametry są używane do przekazywania wartości lub odwołania zmiennej do metody.</span><span class="sxs-lookup"><span data-stu-id="ada03-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="ada03-494">Parametry metody uzyskać wartości rzeczywiste z ***argumenty*** które są określone, gdy metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ada03-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="ada03-495">Istnieją cztery rodzaje parametrów: wartości parametrów, parametrów w formie odwołań, parametry wyjściowe i tablice parametrów.</span><span class="sxs-lookup"><span data-stu-id="ada03-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="ada03-496">A ***wartość parametru*** służy do przekazywania parametru wejściowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="ada03-497">Wartość parametru odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z argumentem, która została przekazana dla parametru.</span><span class="sxs-lookup"><span data-stu-id="ada03-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="ada03-498">Modyfikacje parametr wartości nie wpływają na argumentu, która została przekazana dla parametru.</span><span class="sxs-lookup"><span data-stu-id="ada03-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="ada03-499">Wartości parametrów można opcjonalnie, określając wartość domyślną, dzięki czemu można pominąć odpowiednie argumenty.</span><span class="sxs-lookup"><span data-stu-id="ada03-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="ada03-500">A ***odwołać się do parametru*** jest używany zarówno dla danych wejściowych i przekazywania parametru wyjściowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="ada03-501">Argument przekazana dla parametru odwołania musi być zmienną, a podczas wykonywania metody, parametr odniesienia reprezentuje tę samą lokalizację magazynu zmiennej argumentu.</span><span class="sxs-lookup"><span data-stu-id="ada03-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="ada03-502">Parametr przekazany przez odwołanie jest zadeklarowana za pomocą `ref` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="ada03-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="ada03-503">Poniższy przykład pokazuje użycie `ref` parametrów.</span><span class="sxs-lookup"><span data-stu-id="ada03-503">The following example shows the use of `ref` parameters.</span></span>

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
<span data-ttu-id="ada03-504">***Parametr wyjściowy*** służy do parametru wyjściowego przekazywania.</span><span class="sxs-lookup"><span data-stu-id="ada03-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="ada03-505">Parametr wyjściowy jest podobny do parametr przekazany przez odwołanie, z tą różnicą, że początkowa wartość argumentu dostarczane przez obiekt wywołujący jest "nieważne".</span><span class="sxs-lookup"><span data-stu-id="ada03-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="ada03-506">Parametr wyjściowy jest zadeklarowana za pomocą `out` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="ada03-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="ada03-507">Poniższy przykład pokazuje użycie `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="ada03-507">The following example shows the use of `out` parameters.</span></span>

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
<span data-ttu-id="ada03-508">A ***tablicy parametrów*** zezwala na zmienną liczbę argumentów, które zostaną przekazane do metody.</span><span class="sxs-lookup"><span data-stu-id="ada03-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="ada03-509">Tablica parametrów jest zadeklarowana za pomocą `params` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="ada03-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="ada03-510">Ostatni parametr metody może być tablicą parametrów i typ tablicy parametrów musi być typem tablicy jednowymiarowej.</span><span class="sxs-lookup"><span data-stu-id="ada03-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="ada03-511">`Write` i `WriteLine` metody `System.Console` klasy są dobrym przykładem użycia tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="ada03-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="ada03-512">Są deklarowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ada03-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="ada03-513">W metodzie, która korzysta z tablicy parametrów Tablica parametrów zachowuje się tak samo jak parametru regularnego typu tablicowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="ada03-514">Jednak w wywołaniu metody z tablicą parametrów, istnieje możliwość przekazania pojedynczy argument typu tablicy parametrów albo dowolnej liczby argumentów typu elementu tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="ada03-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="ada03-515">W tym ostatnim przypadku wystąpienie tablicy jest automatycznie tworzone i inicjowane z danego argumentów.</span><span class="sxs-lookup"><span data-stu-id="ada03-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="ada03-516">W tym przykładzie</span><span class="sxs-lookup"><span data-stu-id="ada03-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="ada03-517">jest odpowiednikiem pisania poniżej.</span><span class="sxs-lookup"><span data-stu-id="ada03-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="ada03-518">Treść metody i zmienne lokalne</span><span class="sxs-lookup"><span data-stu-id="ada03-518">Method body and local variables</span></span>

<span data-ttu-id="ada03-519">Treść metody określa instrukcji do wykonania, gdy metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ada03-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="ada03-520">Treść metody można zadeklarować zmienne, które są specyficzne dla wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="ada03-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="ada03-521">Tych zmiennych są nazywane ***zmienne lokalne***.</span><span class="sxs-lookup"><span data-stu-id="ada03-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="ada03-522">Deklaracji zmiennej lokalnej Określa nazwę typu, nazwa zmiennej i prawdopodobnie wartość początkową.</span><span class="sxs-lookup"><span data-stu-id="ada03-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="ada03-523">Poniższy przykład deklaruje zmienną lokalną `i` o wartości początkowej zero i zmienna lokalna `j` bez wartości początkowej.</span><span class="sxs-lookup"><span data-stu-id="ada03-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

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
<span data-ttu-id="ada03-524">Język C# wymaga lokalnej zmiennej jako ***zdecydowanie przypisywany*** przed jej wartość można uzyskać.</span><span class="sxs-lookup"><span data-stu-id="ada03-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="ada03-525">Na przykład jeśli deklaracja poprzedniego `i` nie zawiera wartości początkowej, kompilator może zgłosić błąd do późniejszego użycia `i` ponieważ `i` nie może zostać zdecydowanie przypisany w tych punktach w programie.</span><span class="sxs-lookup"><span data-stu-id="ada03-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="ada03-526">Można użyć metody `return` instrukcje, aby zwrócić sterowanie do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="ada03-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="ada03-527">W metodzie, zwracając `void`, `return` instrukcji nie można określić wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="ada03-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="ada03-528">W metodzie, zwracając non -`void`, `return` instrukcje mogą zawierać wyrażenie, które oblicza wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="ada03-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="ada03-529">Metody statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="ada03-529">Static and instance methods</span></span>

<span data-ttu-id="ada03-530">Metoda zadeklarowana za pomocą `static` modyfikator jest ***statycznej metody***.</span><span class="sxs-lookup"><span data-stu-id="ada03-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="ada03-531">Metoda statyczna nie będzie działać na określonym wystąpieniu i można uzyskać tylko bezpośredni dostęp do statycznych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="ada03-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="ada03-532">Metoda zadeklarowana bez `static` modyfikator jest ***metodę wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="ada03-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="ada03-533">Metoda wystąpienia działa na określonym wystąpieniu i można dostęp do statycznych i wystąpieniami elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="ada03-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="ada03-534">Wystąpienia, na której wywołano metodę wystąpienia, które może być jawnie dostępna jako `this`.</span><span class="sxs-lookup"><span data-stu-id="ada03-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="ada03-535">Jest to błąd do odwoływania się do `this` w metodzie statycznej.</span><span class="sxs-lookup"><span data-stu-id="ada03-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="ada03-536">Następujące `Entity` klasa ma statycznych i elementów członkowskich wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ada03-536">The following `Entity` class has both static and instance members.</span></span>

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
<span data-ttu-id="ada03-537">Każdy `Entity` wystąpienie zawiera numer seryjny (i prawdopodobnie niektóre inne informacje, które nie został tutaj pokazany).</span><span class="sxs-lookup"><span data-stu-id="ada03-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="ada03-538">`Entity` Konstruktora (co przypomina metodą wystąpienia) inicjuje nowe wystąpienie następny dostępny numer seryjny.</span><span class="sxs-lookup"><span data-stu-id="ada03-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="ada03-539">Ponieważ Konstruktor nie jest składową wystąpienia, jest dozwolony dostęp zarówno do `serialNo` pola wystąpienia i `nextSerialNo` pole statyczne.</span><span class="sxs-lookup"><span data-stu-id="ada03-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="ada03-540">`GetNextSerialNo` i `SetNextSerialNo` mogą uzyskiwać dostęp do metod statycznych `nextSerialNo` pole statyczne, ale będzie błąd dla nich bezpośredni dostęp `serialNo` pole wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ada03-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="ada03-541">Poniższy przykład pokazuje użycie `Entity` klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-541">The following example shows the use of the `Entity` class.</span></span>

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
<span data-ttu-id="ada03-542">Należy pamiętać, że `SetNextSerialNo` i `GetNextSerialNo` są wywoływane metody statyczne klasy, natomiast `GetSerialNo` metoda wystąpienia jest wywoływane na wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="ada03-543">Wirtualne zastąpienia i metody abstrakcyjne</span><span class="sxs-lookup"><span data-stu-id="ada03-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="ada03-544">Po deklaracji metody wystąpienia obejmuje `virtual` modyfikator, metoda jest nazywany ***metodę wirtualną***.</span><span class="sxs-lookup"><span data-stu-id="ada03-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="ada03-545">Gdy nie `virtual` modyfikator, metoda jest nazywany ***metody niewirtualnej***.</span><span class="sxs-lookup"><span data-stu-id="ada03-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="ada03-546">Po wywołaniu metody wirtualnej ***typu run-time*** wystąpienia, dla którego tego wywołania ma miejsce określa rzeczywista implementacja metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="ada03-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="ada03-547">W wywołaniu metody niewirtualnej ***typów w czasie kompilacji*** wystąpienia jest czynnikiem decydującym.</span><span class="sxs-lookup"><span data-stu-id="ada03-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="ada03-548">Metoda wirtualna może być ***zastąpione*** w klasie pochodnej.</span><span class="sxs-lookup"><span data-stu-id="ada03-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="ada03-549">Po deklaracji metody wystąpienia obejmuje `override` modyfikator, metoda zastępuje dziedziczoną metodę wirtualną przy użyciu tego samego podpisu.</span><span class="sxs-lookup"><span data-stu-id="ada03-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="ada03-550">Dlatego deklaracja metody wirtualnej wprowadzono nowe metody, deklaracji metody zastąpienie specjalizuje się istniejących dziedziczoną metodę wirtualną, podając nową metodę implementacji tej metody.</span><span class="sxs-lookup"><span data-stu-id="ada03-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="ada03-551">***Abstrakcyjne*** metoda jest metodą wirtualną bez wdrażania.</span><span class="sxs-lookup"><span data-stu-id="ada03-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="ada03-552">Metoda abstrakcyjna jest zadeklarowana za pomocą `abstract` modyfikator i jest dozwolona tylko w klasie, który także jest zadeklarowany `abstract`.</span><span class="sxs-lookup"><span data-stu-id="ada03-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="ada03-553">Metoda abstrakcyjna musi zostać zastąpiona w każdym nieabstrakcyjnej klasie pochodnej.</span><span class="sxs-lookup"><span data-stu-id="ada03-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="ada03-554">Poniższy przykład deklaruje klasę abstrakcyjną, `Expression`, który reprezentuje węzeł drzewa wyrażeń i trzy klasy, pochodne `Constant`, `VariableReference`, i `Operation`, który implementuje węzły drzewa wyrażeń stałych zmiennych odwołania, a operacje arytmetyczne.</span><span class="sxs-lookup"><span data-stu-id="ada03-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="ada03-555">(Jest to podobne do, ale nie należy mylić z typami drzewo wyrażenia, wprowadzona w [typy drzewa wyrażenie](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="ada03-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

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
<span data-ttu-id="ada03-556">Poprzednie cztery klasy może służyć do modelowania wyrażeniach arytmetycznych.</span><span class="sxs-lookup"><span data-stu-id="ada03-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="ada03-557">Na przykład za pomocą wystąpień tych klas, wyrażenie `x + 3` mogą być reprezentowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ada03-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="ada03-558">`Evaluate` Metody `Expression` wystąpienia jest wywoływane w celu oceny danemu wyrażeniu i generować `double` wartość.</span><span class="sxs-lookup"><span data-stu-id="ada03-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="ada03-559">Ta metoda przyjmuje jako argument `Hashtable` zawiera nazwy zmiennych (jako klucze, wpisy) i wartości (jako wartości wpisów).</span><span class="sxs-lookup"><span data-stu-id="ada03-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="ada03-560">`Evaluate` Metoda jest abstrakcyjna metody wirtualnej, co oznacza, że nieabstrakcyjnej klasy pochodne muszą przesłaniać z jego Podaj rzeczywistą implementację.</span><span class="sxs-lookup"><span data-stu-id="ada03-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="ada03-561">A `Constant`przez implementację `Evaluate` po prostu zwraca przechowywaną stała.</span><span class="sxs-lookup"><span data-stu-id="ada03-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="ada03-562">A `VariableReference`jego implementacja wyszukuje nazwę zmiennej w tablicy skrótów i zwraca wartość wynikową.</span><span class="sxs-lookup"><span data-stu-id="ada03-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="ada03-563">`Operation`w implementacji najpierw ocenia operandy lewy i prawy (cyklicznie, wywołanie ich `Evaluate` metod), a następnie wykonuje danej operacji arytmetycznej.</span><span class="sxs-lookup"><span data-stu-id="ada03-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="ada03-564">Następujący program używa `Expression` klasy można oszacować wyrażenia `x * (y + 2)` zależności od wartości `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="ada03-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

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

#### <a name="method-overloading"></a><span data-ttu-id="ada03-565">Przeciążenie metody</span><span class="sxs-lookup"><span data-stu-id="ada03-565">Method overloading</span></span>

<span data-ttu-id="ada03-566">Metoda ***przeciążenie*** zezwala na wiele sposobów w tej samej klasie w celu mają taką samą nazwę, tak długo, jak długo mają unikatowe podpisów.</span><span class="sxs-lookup"><span data-stu-id="ada03-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="ada03-567">Podczas kompilowania wywołanie metody przeciążonej, kompilator używa ***Rozpoznanie przeciążenia*** ustalenie określonej metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="ada03-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="ada03-568">Rozpoznanie przeciążenia umożliwia znalezienie jednej metody, najlepiej odpowiada argumenty lub zgłasza błąd, jeśli można znaleźć nie pojedyncze najlepsze dopasowanie.</span><span class="sxs-lookup"><span data-stu-id="ada03-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="ada03-569">Poniższy kod przedstawia obowiązuje przeciążeniu rozdzielczości.</span><span class="sxs-lookup"><span data-stu-id="ada03-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="ada03-570">Komentarz dla każdego wywołania w `Main` metoda ma pokazać, jakiej metody faktycznie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ada03-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

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
<span data-ttu-id="ada03-571">Jak pokazano na przykładzie, zawsze można wybrać określoną metodę przez jawne rzutowanie argumentów do typów parametru dokładne i/lub jawnie podanie argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="ada03-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="ada03-572">Inni członkowie — funkcja</span><span class="sxs-lookup"><span data-stu-id="ada03-572">Other function members</span></span>

<span data-ttu-id="ada03-573">Elementy członkowskie, które zawierają kod wykonywalny są nazywane zbiorczo ***funkcji elementów członkowskich*** klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="ada03-574">W poprzedniej sekcji opisano metody, które są podstawowym typem funkcji elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="ada03-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="ada03-575">W tej sekcji opisano inne rodzaje elementów członkowskich funkcji obsługiwanych przez C#: konstruktory, właściwości, indeksatory, zdarzenia, operatorów i destruktory.</span><span class="sxs-lookup"><span data-stu-id="ada03-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="ada03-576">Poniższy kod ilustruje klasę ogólną o nazwie `List<T>`, który implementuje growable listy obiektów.</span><span class="sxs-lookup"><span data-stu-id="ada03-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="ada03-577">Klasa zawiera kilka przykładów typowych rodzajów funkcji elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="ada03-577">The class contains several examples of the most common kinds of function members.</span></span>


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

#### <a name="constructors"></a><span data-ttu-id="ada03-578">Konstruktorów</span><span class="sxs-lookup"><span data-stu-id="ada03-578">Constructors</span></span>

<span data-ttu-id="ada03-579">C# obsługuje zarówno wystąpienia i konstruktorów statycznych.</span><span class="sxs-lookup"><span data-stu-id="ada03-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="ada03-580">***Konstruktora wystąpienia*** jest elementem członkowskim implementujący działania wymagane w celu zainicjowania wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="ada03-581">A ***statyczny Konstruktor*** jest elementem członkowskim, który implementuje działania wymagane w celu zainicjowania samej klasy po pierwszym załadowaniu.</span><span class="sxs-lookup"><span data-stu-id="ada03-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="ada03-582">Konstruktor jest zadeklarowany jak metody bez zwrotu typu i taką samą nazwę jak klasa zawierająca.</span><span class="sxs-lookup"><span data-stu-id="ada03-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="ada03-583">Jeśli deklaracja konstruktora zawiera `static` modyfikator, deklaruje Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="ada03-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="ada03-584">W przeciwnym razie deklaruje konstruktora wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ada03-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="ada03-585">Konstruktory wystąpień, mogą być przeciążone.</span><span class="sxs-lookup"><span data-stu-id="ada03-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="ada03-586">Na przykład `List<T>
` klasa deklaruje dwa konstruktory wystąpienia, jedno z bez parametrów, a ta, która przyjmuje `int` parametru.</span><span class="sxs-lookup"><span data-stu-id="ada03-586">For example, the `List<T>
` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="ada03-587">Konstruktory wystąpień są wywoływane przy użyciu `new` operatora.</span><span class="sxs-lookup"><span data-stu-id="ada03-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="ada03-588">Poniższe instrukcje przydzielić dwie `List<string>
` wystąpień każdej z konstruktorów z `List` klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-588">The following statements allocate two `List<string>
` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="ada03-589">W przeciwieństwie do innych członków konstruktorów wystąpienia nie są dziedziczone, a klasa nie ma konstruktorów wystąpienia innych niż rzeczywiście zgłoszonymi w klasie.</span><span class="sxs-lookup"><span data-stu-id="ada03-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="ada03-590">Jeśli żaden konstruktor wystąpienia zostanie podany dla klasy, następnie pustą bez parametrów jest dostarczana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="ada03-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="ada03-591">Właściwości</span><span class="sxs-lookup"><span data-stu-id="ada03-591">Properties</span></span>

<span data-ttu-id="ada03-592">***Właściwości*** to naturalne rozszerzenie pola.</span><span class="sxs-lookup"><span data-stu-id="ada03-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="ada03-593">Nazwanych elementów członkowskich skojarzonych typów i składnia do uzyskiwania dostępu do pola i właściwości jest taka sama.</span><span class="sxs-lookup"><span data-stu-id="ada03-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="ada03-594">Jednak w przeciwieństwie do pola, właściwości określa lokalizacji przechowywania.</span><span class="sxs-lookup"><span data-stu-id="ada03-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="ada03-595">Właściwości mają ***Akcesory*** określające instrukcji do wykonania, gdy ich wartości są odczytu lub zapisu.</span><span class="sxs-lookup"><span data-stu-id="ada03-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="ada03-596">Właściwość deklarowany jest jak pola, z tą różnicą, że deklaracja kończy się `get` metody dostępu i/lub `set` akcesor zapisywane między ogranicznikami `{` i `}` zamiast kończy się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="ada03-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="ada03-597">Właściwości, które ma obydwa `get` metody dostępu i `set` akcesor jest ***odczytu i zapisu właściwości***, właściwość, która ma tylko `get` akcesor jest ***właściwość tylko do odczytu***i Właściwość, która ma tylko `set` akcesor jest ***właściwość tylko do zapisu***.</span><span class="sxs-lookup"><span data-stu-id="ada03-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="ada03-598">A `get` akcesor odnosi się do metody bez parametrów, z wartością zwracaną z typem właściwości.</span><span class="sxs-lookup"><span data-stu-id="ada03-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="ada03-599">Z wyjątkiem elementem docelowym przypisania, gdy właściwość jest przywoływany w wyrażeniu, `get` metody dostępu właściwości jest wywoływana w celu obliczenia wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="ada03-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="ada03-600">A `set` akcesor odnosi się do metody z pojedynczym parametrem o nazwie `value` i bez zwrotu typu.</span><span class="sxs-lookup"><span data-stu-id="ada03-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="ada03-601">Gdy właściwość odwołuje się do jako element docelowy przypisania lub operand `++` lub `--`, `set` akcesor zostanie wywołana z nieprawidłowym argumentem, który zawiera nową wartość.</span><span class="sxs-lookup"><span data-stu-id="ada03-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="ada03-602">`List<T>
` Klasa deklaruje dwie właściwości `Count` i `Capacity`, które są tylko do odczytu i odczytu / zapisu, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="ada03-602">The `List<T>
` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="ada03-603">Oto przykład użycia tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="ada03-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="ada03-604">Podobnie jak pola i metody, C# obsługuje zarówno właściwości wystąpienia i właściwości statyczne.</span><span class="sxs-lookup"><span data-stu-id="ada03-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="ada03-605">Właściwości statyczne są uznane za pomocą `static` modyfikator i właściwości instancji, które są zadeklarowane bez niego.</span><span class="sxs-lookup"><span data-stu-id="ada03-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="ada03-606">Accessor(s) właściwości mogą być wirtualne.</span><span class="sxs-lookup"><span data-stu-id="ada03-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="ada03-607">Jeśli deklaracja właściwości zawiera `virtual`, `abstract`, lub `override` modyfikator, dotyczy ona accessor(s) właściwości.</span><span class="sxs-lookup"><span data-stu-id="ada03-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="ada03-608">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="ada03-608">Indexers</span></span>

<span data-ttu-id="ada03-609">***Indeksatora*** jest element członkowski, który umożliwia obiekty, które mają być indeksowane w taki sam sposób, w postaci tablicy.</span><span class="sxs-lookup"><span data-stu-id="ada03-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="ada03-610">Indeksator deklarowany jest jak właściwości, z tą różnicą, że nazwa elementu członkowskiego jest `this` następuje lista parametrów, zapisywane między ogranicznikami `[` i `]`.</span><span class="sxs-lookup"><span data-stu-id="ada03-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="ada03-611">Parametry są dostępne w accessor(s) indeksatora.</span><span class="sxs-lookup"><span data-stu-id="ada03-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="ada03-612">Podobnie jak właściwości, indeksatory można odczytu i zapisu, tylko do odczytu i tylko do zapisu, a accessor(s) indeksatora mogą być wirtualne.</span><span class="sxs-lookup"><span data-stu-id="ada03-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="ada03-613">`List` Klasa deklaruje pojedynczego indeksatora odczytu i zapisu, która przyjmuje `int` parametru.</span><span class="sxs-lookup"><span data-stu-id="ada03-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="ada03-614">Indeksator umożliwia indeksu `List` wystąpień z `int` wartości.</span><span class="sxs-lookup"><span data-stu-id="ada03-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="ada03-615">Na przykład</span><span class="sxs-lookup"><span data-stu-id="ada03-615">For example</span></span>

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
<span data-ttu-id="ada03-616">Indeksatory mogą być przeciążone, co oznacza, że klasy można zadeklarować wiele indeksatorów, tak długo, jak liczba lub rodzaju ich parametrów różnią się.</span><span class="sxs-lookup"><span data-stu-id="ada03-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="ada03-617">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="ada03-617">Events</span></span>

<span data-ttu-id="ada03-618">***Zdarzeń*** jest elementem członkowskim, który umożliwia klasa lub obiekt, do udostępniania powiadomień o.</span><span class="sxs-lookup"><span data-stu-id="ada03-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="ada03-619">Zdarzenie deklarowany jest jak pola, z tą różnicą, że deklaracja zawiera `event` — słowo kluczowe i typ musi być typem delegowanym.</span><span class="sxs-lookup"><span data-stu-id="ada03-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="ada03-620">W obrębie klasy, która deklaruje element członkowski zdarzenia zdarzenie zachowuje się tak samo jak pola typu delegata (pod warunkiem zdarzenie nie jest abstrakcyjna i nie deklaruje metody dostępu).</span><span class="sxs-lookup"><span data-stu-id="ada03-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="ada03-621">Pole zawiera odwołanie do delegata reprezentującego procedury obsługi zdarzeń, które zostały dodane do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="ada03-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="ada03-622">Jeśli nie obsługuje zdarzeń są obecne, pole jest `null`.</span><span class="sxs-lookup"><span data-stu-id="ada03-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="ada03-623">`List<T>
` Klasa deklaruje składową pojedyncze zdarzenie o nazwie `Changed`, co oznacza, że dodano nowy element do listy.</span><span class="sxs-lookup"><span data-stu-id="ada03-623">The `List<T>
` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="ada03-624">`Changed` Wydarzenie jest podniesione przez `OnChanged` metody wirtualnej, który po raz pierwszy sprawdza, czy zdarzenie jest `null` (co oznacza, że nie programów obsługi istnieje).</span><span class="sxs-lookup"><span data-stu-id="ada03-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="ada03-625">Pojęcie podnoszenie zdarzenia odpowiada dokładnie wywoływania delegata reprezentowanej przez zdarzenie — tak więc nie istnieją żadne konstrukcje specjalny język przeznaczony dla podnoszonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="ada03-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="ada03-626">Klienci reagowania na zdarzenia za pośrednictwem ***procedury obsługi zdarzeń***.</span><span class="sxs-lookup"><span data-stu-id="ada03-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="ada03-627">Programy obsługi zdarzeń dołączonych przy użyciu `+=` operatora i usunięte przy użyciu `-=` operatora.</span><span class="sxs-lookup"><span data-stu-id="ada03-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="ada03-628">Poniższy przykład dołącza program obsługi zdarzeń do `Changed` zdarzenia `List<string>
`.</span><span class="sxs-lookup"><span data-stu-id="ada03-628">The following example attaches an event handler to the `Changed` event of a `List<string>
`.</span></span>

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
<span data-ttu-id="ada03-629">W przypadku zaawansowanych scenariuszy, w którym pożądane jest formantu powiązanego magazynu zdarzenie, jawnie określić deklaracji zdarzenia `add` i `remove` metod dostępu, które są nieco podobne do `set` metody dostępu właściwości.</span><span class="sxs-lookup"><span data-stu-id="ada03-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="ada03-630">Operatory</span><span class="sxs-lookup"><span data-stu-id="ada03-630">Operators</span></span>

<span data-ttu-id="ada03-631">***Operator*** jest element członkowski, który definiuje znaczenie zastosowania operatora poszczególnych wyrażeń do wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="ada03-632">Można zdefiniować trzy rodzaje operatory: jednoargumentowe operatory, operatory binarne i operatory konwersji.</span><span class="sxs-lookup"><span data-stu-id="ada03-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="ada03-633">Wszystkie operatory musi być zadeklarowany jako `public` i `static`.</span><span class="sxs-lookup"><span data-stu-id="ada03-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="ada03-634">`List<T>
` Klasa deklaruje dwa operatory `operator==` i `operator!=`i dlatego zapewnia nowe znaczenie wyrażenia, które są stosowane te operatory, aby `List` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ada03-634">The `List<T>
` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="ada03-635">W szczególności operatorów definiowania równości dwóch `List<T>
` wystąpienia jako porównując każdej zawartych obiektów za pomocą ich `Equals` metody.</span><span class="sxs-lookup"><span data-stu-id="ada03-635">Specifically, the operators define equality of two `List<T>
` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="ada03-636">W poniższym przykładzie użyto `==` operatora do porównywania dwóch `List<int>
` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ada03-636">The following example uses the `==` operator to compare two `List<int>
` instances.</span></span>

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

<span data-ttu-id="ada03-637">Pierwszy `Console.WriteLine` generuje `True` ponieważ dwie listy zawiera taką samą liczbę obiektów o tej samej wartości w tej samej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ada03-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="ada03-638">Gdyby `List<T>
` Niezdefiniowany `operator==`, pierwszy `Console.WriteLine` będzie mieć danych wyjściowych `False` ponieważ `a` i `b` odwołanie do innego `List<int>
` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ada03-638">Had `List<T>
` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>
` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="ada03-639">Destruktory</span><span class="sxs-lookup"><span data-stu-id="ada03-639">Destructors</span></span>

<span data-ttu-id="ada03-640">A ***destruktor*** jest elementem członkowskim implementujący działania wymagane do niszczenia wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="ada03-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="ada03-641">Destruktory nie mogą mieć parametrów, nie mają modyfikatory dostępności i nie można wywołać jawnie.</span><span class="sxs-lookup"><span data-stu-id="ada03-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="ada03-642">Destruktor dla wystąpienia jest wywoływana automatycznie podczas wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="ada03-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="ada03-643">Moduł odśmiecania pamięci jest dozwolona szerokiego szerokości przy podejmowaniu decyzji podczas zbierania obiektów i uruchomić destruktorów.</span><span class="sxs-lookup"><span data-stu-id="ada03-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="ada03-644">Czas wywołania destruktora nie jest jednoznaczny i destruktory mogą być wykonywane na żadnym z wątków.</span><span class="sxs-lookup"><span data-stu-id="ada03-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="ada03-645">Dla tych i innych powodów klasy należy zaimplementować destruktory tylko wtedy, gdy nie inne rozwiązania są niewykonalne.</span><span class="sxs-lookup"><span data-stu-id="ada03-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="ada03-646">`using` Instrukcja oferuje lepszym rozwiązaniem do zniszczenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="ada03-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="ada03-647">Struktury</span><span class="sxs-lookup"><span data-stu-id="ada03-647">Structs</span></span>

<span data-ttu-id="ada03-648">Takie jak klasy, ***struktury*** są struktur danych, które mogą zawierać elementy członkowskie danych i składowe funkcji, ale w przeciwieństwie do klasy, struktury są typami wartości i nie wymagają alokacji sterty.</span><span class="sxs-lookup"><span data-stu-id="ada03-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="ada03-649">Zmienną typu struktury bezpośrednio przechowuje dane struktury, natomiast zmienna typu klasa przechowuje odwołania do obiektu przydzielanego dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="ada03-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="ada03-650">Typy struktury nie obsługują dziedziczenia określonych przez użytkownika, a wszystkie typy struktury niejawnie dziedziczą z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="ada03-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="ada03-651">Struktury są szczególnie przydatne w przypadku małych strukturach danych, które mają semantyki wartości.</span><span class="sxs-lookup"><span data-stu-id="ada03-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="ada03-652">Liczby zespolone, punkty w układzie współrzędnych lub par klucz wartość ze słownika są dobrym przykładem struktury.</span><span class="sxs-lookup"><span data-stu-id="ada03-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="ada03-653">Używanie struktur, zamiast klasy w małych strukturach danych ułatwia duża różnica w liczbie alokacji pamięci aplikacji wykonuje.</span><span class="sxs-lookup"><span data-stu-id="ada03-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="ada03-654">Na przykład następujący program tworzy i inicjuje tablicę 100 punktów.</span><span class="sxs-lookup"><span data-stu-id="ada03-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="ada03-655">Za pomocą `Point` implementowany jako klasa, 101 oddzielne obiekty są tworzone — jeden dla macierzy i jeden dla 100 elementów.</span><span class="sxs-lookup"><span data-stu-id="ada03-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

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
<span data-ttu-id="ada03-656">Alternatywą jest zapewnienie `Point` struktury.</span><span class="sxs-lookup"><span data-stu-id="ada03-656">An alternative is to make `Point` a struct.</span></span>

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
<span data-ttu-id="ada03-657">Teraz, zostanie uruchomiony tylko jeden obiekt — jeden dla tablicy — i `Point` wystąpienia są przechowywane w tekście w tablicy.</span><span class="sxs-lookup"><span data-stu-id="ada03-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="ada03-658">Struct — Konstruktorzy są wywoływane przy użyciu `new` operatora, ale nie oznacza, że pamięć jest przydzielane.</span><span class="sxs-lookup"><span data-stu-id="ada03-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="ada03-659">Zamiast dynamicznej alokacji obiektu i zwraca odwołanie do niej Konstruktor struktury po prostu zwraca wartość struct (zwykle znajduje się w lokalizacji tymczasowej na stosie), a ta wartość jest następnie kopiowana gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="ada03-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="ada03-660">W przypadku klas jest możliwe dwóch zmiennych odwoływać się do tego samego obiektu dla operacji na jednej zmiennej miały wpływ na obiekt odwołuje się druga zmienna zatem możliwe.</span><span class="sxs-lookup"><span data-stu-id="ada03-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="ada03-661">Przy użyciu struktury zmienne każda ma własne kopię danych, a nie jest możliwe dla operacji na jednym wpływa na drugi.</span><span class="sxs-lookup"><span data-stu-id="ada03-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="ada03-662">Na przykład dane wyjściowe generowane przez następujący fragment kodu jest zależna od tego, czy `Point` jest klasą lub strukturą.</span><span class="sxs-lookup"><span data-stu-id="ada03-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="ada03-663">Jeśli `Point` jest klasą, dane wyjściowe są `20` ponieważ `a` i `b` odwoływać się do tego samego obiektu.</span><span class="sxs-lookup"><span data-stu-id="ada03-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="ada03-664">Jeśli `Point` jest strukturą, dane wyjściowe są `10` ponieważ przypisanie `a` do `b` tworzona jest kopia wartości, i nie ma wpływu następne przypisanie do tej kopii `a.x`.</span><span class="sxs-lookup"><span data-stu-id="ada03-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="ada03-665">W poprzednim przykładzie wyróżniono dwa ograniczenia dotyczące struktury.</span><span class="sxs-lookup"><span data-stu-id="ada03-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="ada03-666">Po pierwsze całej strukturze jest to zazwyczaj mniej wydajne niż kopiowanie odwołanie do obiektu, dzięki czemu przekazywanie przypisania i wartość parametru może być bardziej kosztowne przy użyciu struktury niż w przypadku typów referencyjnych.</span><span class="sxs-lookup"><span data-stu-id="ada03-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="ada03-667">Drugi, z wyjątkiem `ref` i `out` parametrów, nie jest możliwe do utworzenia odwołania do struktury, która wyklucza ich użycia w różnych sytuacjach.</span><span class="sxs-lookup"><span data-stu-id="ada03-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="ada03-668">Tablice</span><span class="sxs-lookup"><span data-stu-id="ada03-668">Arrays</span></span>

<span data-ttu-id="ada03-669">***Tablicy*** to struktura danych, która zawiera szereg zmiennych, które są dostępne za pośrednictwem obliczanej indeksów.</span><span class="sxs-lookup"><span data-stu-id="ada03-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="ada03-670">Zmienne zawartych w tablicy, jest określana skrótem ***elementy*** tablicy, są wszystkie tego samego typu, a tego typu jest nazywana ***typ elementu*** tablicy.</span><span class="sxs-lookup"><span data-stu-id="ada03-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="ada03-671">Typy tablicowe są typami odwołań, a deklaracja zmiennej tablicy po prostu rezerwuje miejsce dla odwołania do wystąpienia tablicy.</span><span class="sxs-lookup"><span data-stu-id="ada03-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="ada03-672">Wystąpienia bieżącej tablicy są tworzone dynamicznie w czasie wykonywania za pomocą `new` operatora.</span><span class="sxs-lookup"><span data-stu-id="ada03-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="ada03-673">`new` Operacji określa ***długość*** nowego wystąpienia tablicy naprawiliśmy dla okresu istnienia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ada03-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="ada03-674">Indeksy elementów z zakresu tablicy `0` do `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="ada03-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="ada03-675">`new` Operator automatycznie inicjuje elementy tablicy, aby przywrócić wartości domyślne, na przykład są to wartości zero dla wszystkich typów liczbowych i `null` dla wszystkich typów odniesienia.</span><span class="sxs-lookup"><span data-stu-id="ada03-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="ada03-676">Poniższy przykład tworzy tablicę `int` elementów, inicjuje tablicy i wyświetla zawartość tablicy.</span><span class="sxs-lookup"><span data-stu-id="ada03-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

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
<span data-ttu-id="ada03-677">W tym przykładzie tworzy i działa na ***tablicy jednowymiarowej***.</span><span class="sxs-lookup"><span data-stu-id="ada03-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="ada03-678">C# obsługuje również ***Wielowymiarowe tablice***.</span><span class="sxs-lookup"><span data-stu-id="ada03-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="ada03-679">Wpisz liczbę wymiarów tablicy, znany także jako ***ranga*** typ tablicy jest jedną przesunięta liczbę przecinkami zapisywane między nawiasami kwadratowymi typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="ada03-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="ada03-680">Poniższy przykład przydziela jednowymiarową dwuwymiarowym i tablicę trójwymiarową.</span><span class="sxs-lookup"><span data-stu-id="ada03-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="ada03-681">`a1` Tablica zawiera 10 elementów `a2` tablica zawiera 50 (10 x 5) elementów, a `a3` tablica zawiera 100 (10 x 5 x 2) elementów.</span><span class="sxs-lookup"><span data-stu-id="ada03-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="ada03-682">Typ elementu tablicy może być dowolnego typu, w tym typu tablicowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="ada03-683">Tablicę z elementami typu tablicowego jest czasami nazywane ***tablicy nieregularnej*** ponieważ długości tablic elementu nie wszystkie muszą być takie same.</span><span class="sxs-lookup"><span data-stu-id="ada03-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="ada03-684">Poniższy przykład przydziela tablicy tablic `int`:</span><span class="sxs-lookup"><span data-stu-id="ada03-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="ada03-685">Pierwszy wiersz tworzy tablicę z trzech elementów, z których każdy typ `int[]` , a każdy z wartości początkowej `null`.</span><span class="sxs-lookup"><span data-stu-id="ada03-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="ada03-686">Kolejne wiersze następnie zainicjowanie trzech elementów z odwołaniami do wystąpień poszczególnych tablicy o różnej długości.</span><span class="sxs-lookup"><span data-stu-id="ada03-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="ada03-687">`new` Operator zezwala na wartość początkową elementów tablicy, należy określić przy użyciu ***inicjatora tablicy***, który znajduje się lista wyrażeń zapisywane między ogranicznikami `{` i `}`.</span><span class="sxs-lookup"><span data-stu-id="ada03-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="ada03-688">Poniższy przykład przydziela i inicjuje `int[]` za pomocą trzech elementów.</span><span class="sxs-lookup"><span data-stu-id="ada03-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="ada03-689">Należy pamiętać, że długość tablicy jest wnioskowany z liczba wyrażeń między `{` i `}`.</span><span class="sxs-lookup"><span data-stu-id="ada03-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="ada03-690">Zmienna lokalna i deklaracji pól można skrócony dalsze taki sposób, że typ tablicy nie ma być przekształcone.</span><span class="sxs-lookup"><span data-stu-id="ada03-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="ada03-691">W poprzednich przykładach są odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ada03-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="ada03-692">Interfejsy</span><span class="sxs-lookup"><span data-stu-id="ada03-692">Interfaces</span></span>

<span data-ttu-id="ada03-693">***Interfejsu*** definiuje kontrakt, który może być implementowany przez klas i struktur.</span><span class="sxs-lookup"><span data-stu-id="ada03-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="ada03-694">Interfejs może zawierać metody, właściwości, zdarzeń i indeksatorów.</span><span class="sxs-lookup"><span data-stu-id="ada03-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="ada03-695">Interfejs nie zawiera implementacji członków definiuje — jedynie określa elementy członkowskie, które muszą być dostarczane przez klasy lub struktury, które implementują interfejs.</span><span class="sxs-lookup"><span data-stu-id="ada03-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="ada03-696">Interfejsy mogą stosować ***wielokrotne dziedziczenie***.</span><span class="sxs-lookup"><span data-stu-id="ada03-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="ada03-697">W poniższym przykładzie interfejs `IComboBox` dziedziczy z obu `ITextBox` i `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="ada03-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

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
<span data-ttu-id="ada03-698">Klasy i struktury można zaimplementować wiele interfejsów.</span><span class="sxs-lookup"><span data-stu-id="ada03-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="ada03-699">W poniższym przykładzie klasa `EditBox` implementuje interfejsy `IControl` i `IDataBound`.</span><span class="sxs-lookup"><span data-stu-id="ada03-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

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
<span data-ttu-id="ada03-700">Gdy klasa lub struktura implementuje danego interfejsu, wystąpienia tej klasy lub struktury mogą być niejawnie konwertowane do tego typu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ada03-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="ada03-701">Na przykład</span><span class="sxs-lookup"><span data-stu-id="ada03-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="ada03-702">W przypadkach, gdy wystąpienie nie jest znany statycznie do implementowania określonego interfejsu służy rzutowania typu dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="ada03-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="ada03-703">Na przykład poniższe instrukcje użyć rzutowania typu dynamicznego do uzyskiwania obiektu `IControl` i `IDataBound` implementacji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ada03-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="ada03-704">Ponieważ jest rzeczywisty typ obiektu `EditBox`, rzutowania powiodło się.</span><span class="sxs-lookup"><span data-stu-id="ada03-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="ada03-705">W ciągu poprzednich `EditBox` klasy `Paint` metody z `IControl` interfejsu i `Bind` metody z `IDataBound` interfejs jest implementowany przy użyciu `public` elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="ada03-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="ada03-706">C# obsługuje również ***implementacji elementu członkowskiego interfejsu jawnego***, za pomocą której klasy lub struktury można uniknąć, dzięki czemu członkowie `public`.</span><span class="sxs-lookup"><span data-stu-id="ada03-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="ada03-707">Implementacja interfejsu jawnego członka jest zapisywany przy użyciu interfejsu w pełni kwalifikowana nazwa elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="ada03-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="ada03-708">Na przykład `EditBox` implementacji klasy `IControl.Paint` i `IDataBound.Bind` metod za pomocą jawnych implementacji elementu członkowskiego interfejsu w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ada03-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="ada03-709">Elementy członkowskie interfejsu jawnego zostać oceniony jedynie przez typ interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ada03-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="ada03-710">Na przykład implementacji `IControl.Paint` dostarczone przez poprzednie `EditBox` klasy może być wywoływany tylko przez uprzedniego przekonwertowania `EditBox` odwołanie do `IControl` typ interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ada03-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="ada03-711">Wyliczenia</span><span class="sxs-lookup"><span data-stu-id="ada03-711">Enums</span></span>

<span data-ttu-id="ada03-712">***Typu wyliczeniowego*** jest typem wartości odrębnych z szeregu nazwanych stałych.</span><span class="sxs-lookup"><span data-stu-id="ada03-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="ada03-713">Poniższy przykład deklaruje i korzysta z typu wyliczeniowego, o nazwie `Color` z trzech wartości stałych, `Red`, `Green`, i `Blue`.</span><span class="sxs-lookup"><span data-stu-id="ada03-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

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
<span data-ttu-id="ada03-714">Każdy typ wyliczenia ma odpowiedni typ całkowity, o nazwie ***typ bazowy*** typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="ada03-715">Typ wyliczeniowy, który jawnie deklaruj typu podstawowego jest podstawowym typem `int`.</span><span class="sxs-lookup"><span data-stu-id="ada03-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="ada03-716">Format magazynu i zakres możliwych wartości typu wyliczeniowego są określane przez jej typ podstawowy.</span><span class="sxs-lookup"><span data-stu-id="ada03-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="ada03-717">Zbiór wartości, które mogą przejąć typ wyliczeniowy nie jest ograniczona przez jej elementów członkowskich wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="ada03-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="ada03-718">W szczególności dowolną wartość w podstawowym typem wyliczenia mogą być rzutowane na typ wyliczeniowy i jest różne prawidłową wartością tego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="ada03-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="ada03-719">Poniższy przykład deklaruje typu wyliczeniowego, o nazwie `Alignment` z podstawowym typem `sbyte`.</span><span class="sxs-lookup"><span data-stu-id="ada03-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="ada03-720">Jak pokazano w poprzednim przykładzie, deklaracja elementu członkowskiego wyliczenia mogą zawierać wyrażenie stałe, które określa wartość elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="ada03-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="ada03-721">Stała wartość dla każdego elementu członkowskiego wyliczenia musi być w zakresie podstawowym typem wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="ada03-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="ada03-722">Element członkowski w deklaracji elementu członkowskiego wyliczenia bez określenia wartości, otrzymuje wartość zero (jeśli jest to pierwszy element członkowski w typie wyliczeniowym) lub wartość składowej wyliczenia w formie tekstu poprzedniego plus jeden.</span><span class="sxs-lookup"><span data-stu-id="ada03-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="ada03-723">Wartości wyliczenia może być przekonwertowany na całkowite wartości i odwrotnie przy użyciu typu rzutowania.</span><span class="sxs-lookup"><span data-stu-id="ada03-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="ada03-724">Na przykład</span><span class="sxs-lookup"><span data-stu-id="ada03-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="ada03-725">Wartość domyślna typu wyliczenia jest wartością typu całkowitego zero konwertowane na typ wyliczeniowy.</span><span class="sxs-lookup"><span data-stu-id="ada03-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="ada03-726">W przypadkach, gdy zmienne są automatycznie inicjowane na wartość domyślna to wartość wyliczenia typów zmiennych.</span><span class="sxs-lookup"><span data-stu-id="ada03-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="ada03-727">Aby wartość domyślną typu wyliczeniowego ma być łatwo dostępne, literał `0` niejawnie konwertuje do dowolnego typu wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="ada03-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="ada03-728">W związku z tym że jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="ada03-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="ada03-729">Delegaty</span><span class="sxs-lookup"><span data-stu-id="ada03-729">Delegates</span></span>

<span data-ttu-id="ada03-730">A ***typ delegowany*** reprezentuje odwołania do metod z określonego parametru listy i typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="ada03-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="ada03-731">Delegatów można umożliwić traktować metod jako jednostki, które mogą być przypisane do zmiennych i przekazywane jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ada03-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="ada03-732">Delegaty są podobne do koncepcji wskaźników funkcji, w przeciwieństwie do innych języków, ale w przeciwieństwie do wskaźników funkcji, obiekty delegowane są zorientowane obiektowo i bezpieczny typowo.</span><span class="sxs-lookup"><span data-stu-id="ada03-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="ada03-733">Poniższy przykład deklaruje i wykorzystuje typ delegata, o nazwie `Function`.</span><span class="sxs-lookup"><span data-stu-id="ada03-733">The following example declares and uses a delegate type named `Function`.</span></span>

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
<span data-ttu-id="ada03-734">Wystąpienie `Function` typ delegata można odwoływać się do dowolnej metody, która przyjmuje `double` argument i zwraca `double` wartość.</span><span class="sxs-lookup"><span data-stu-id="ada03-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="ada03-735">`Apply` Metoda dotyczy danego `Function` do elementów `double[]`, zwracanie `double[]` z wynikami.</span><span class="sxs-lookup"><span data-stu-id="ada03-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="ada03-736">W `Main` metody `Apply` służy do trzech różnych funkcji w celu stosowania `double[]`.</span><span class="sxs-lookup"><span data-stu-id="ada03-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="ada03-737">Delegat może odwoływać się statycznej metody (takie jak `Square` lub `Math.Sin` w poprzednim przykładzie) lub metodą wystąpienia (takie jak `m.Multiply` w poprzednim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="ada03-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="ada03-738">Delegat, który odwołuje się do metody wystąpienia odwołuje się konkretny obiekt, a gdy wywoływana jest metoda wystąpienia, za pośrednictwem delegata, staje się ten obiekt `this` w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="ada03-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="ada03-739">Delegatów można także tworzyć, używając funkcji anonimowych, które są "wbudowane metody", które są tworzone na bieżąco.</span><span class="sxs-lookup"><span data-stu-id="ada03-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="ada03-740">Funkcje anonimowe można zobaczyć zmienne lokalne otaczającym metody.</span><span class="sxs-lookup"><span data-stu-id="ada03-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="ada03-741">W związku z tym, w powyższym przykładzie Mnożnik można napisać tak, aby łatwiej bez korzystania z `Multiplier` klasy:</span><span class="sxs-lookup"><span data-stu-id="ada03-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="ada03-742">Interesujące i przydatne własności delegata jest, nie wiedzieć, i nie interesuje klasy, metody, która odwołuje się do; wszystko, co dla Ciebie ważne jest, do którego istnieje odwołanie metoda ma te same parametry oraz zwracany typ jak delegat.</span><span class="sxs-lookup"><span data-stu-id="ada03-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="ada03-743">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="ada03-743">Attributes</span></span>

<span data-ttu-id="ada03-744">Typy, członków i inne jednostki w programie C# obsługuje modyfikatory, które kontrolują niektóre aspekty swojego zachowania.</span><span class="sxs-lookup"><span data-stu-id="ada03-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="ada03-745">Na przykład dostępność metody jest kontrolowany przy użyciu `public`, `protected`, `internal`, i `private` modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="ada03-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="ada03-746">C# uogólnia tej możliwości w taki sposób, że typy zdefiniowane przez użytkownika deklaratywne informacji może być dołączony do programu jednostek i pobierane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ada03-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="ada03-747">Programy Określ dodatkowe informacje deklaratywne przez definiowanie i korzystanie z ***atrybuty***.</span><span class="sxs-lookup"><span data-stu-id="ada03-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="ada03-748">Poniższy przykład deklaruje `HelpAttribute` atrybut, który można umieścić na program jednostki zawierają łącza do ich dokumentacji skojarzone.</span><span class="sxs-lookup"><span data-stu-id="ada03-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

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
<span data-ttu-id="ada03-749">Wszystkie klasy atrybutu pochodzić od `System.Attribute` dostarczane przez program .NET Framework klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="ada03-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="ada03-750">Atrybuty mogą być stosowane, zapewniając ich nazw, wraz z żadnych argumentów, w nawiasach kwadratowych tuż przed skojarzone deklaracji.</span><span class="sxs-lookup"><span data-stu-id="ada03-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="ada03-751">Jeśli nazwa atrybutu, który kończy się na `Attribute`, gdy odwołuje się do atrybutu można pominąć tę część nazwy.</span><span class="sxs-lookup"><span data-stu-id="ada03-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="ada03-752">Na przykład `HelpAttribute` atrybut może być używany w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ada03-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="ada03-753">W tym przykładzie dołącza `HelpAttribute` do `Widget` klasy, a drugi `HelpAttribute` do `Display` metody w klasie.</span><span class="sxs-lookup"><span data-stu-id="ada03-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="ada03-754">Konstruktory publiczne klasy atrybutu kontrolować informacje, które należy podać, gdy atrybut jest dołączany do jednostki programu.</span><span class="sxs-lookup"><span data-stu-id="ada03-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="ada03-755">Można podać dodatkowe informacje, odwołując się do właściwości publiczne odczytu / zapisu atrybutu klasy (np. odwołanie do `Topic` właściwość wcześniej).</span><span class="sxs-lookup"><span data-stu-id="ada03-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="ada03-756">W poniższym przykładzie pokazano, jak można pobrać informacje o atrybutach dotyczące jednostki danego programu w czasie wykonywania przy użyciu odbicia.</span><span class="sxs-lookup"><span data-stu-id="ada03-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

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
<span data-ttu-id="ada03-757">Określony atrybut zleconą przez odbicie konstruktora dla klasy atrybutu jest wywoływana z informacji podanych w źródle programu, a wynikowy wystąpienie atrybutu jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="ada03-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="ada03-758">Dodatkowe informacje były udostępniane za pośrednictwem właściwości, te właściwości są ustawione na wartości podanej w przed zwróceniem wystąpienie atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ada03-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
