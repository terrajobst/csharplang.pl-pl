---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281947"
---
# <a name="simple-programs"></a>Proste programy

* [x] proponowane
* [x] prototyp: uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zezwalaj na wykonywanie sekwencji *instrukcji* bezpośrednio przed *namespace_member_declaration*s *compilation_unit* (tj. plik źródłowy).

Semantyką jest to, że jeśli taka sekwencja *instrukcji* jest obecna, zostanie wyemitowana następująca deklaracja typu, modulo rzeczywistą nazwę typu i nazwę metody.

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Zobacz również https://github.com/dotnet/csharplang/issues/3117.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Istnieje pewna ilość zwyczajowa otaczająca nawet najprostszych programów, z powodu potrzeby jawnej metody `Main`. Jest to zgodne z sposobem uczenia się i programowania w języku. W związku z tym głównym celem tej funkcji jest umożliwienie C# programom, które nie są niepotrzebnymi standardami, w celu uzyskania informacji na temat użytkowników i przejrzystości kodu.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

### <a name="syntax"></a>Składnia

Jedyną dodatkową składnią jest umożliwienie sekwencji *instrukcji*s w jednostce kompilacji tuż przed *namespace_member_declaration*s:

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

Tylko jeden *compilation_unit* może mieć *instrukcję*s. 

Przykład:

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

### <a name="semantics"></a>Semantyki

Jeśli jakiekolwiek instrukcje najwyższego poziomu są obecne w dowolnej jednostce kompilacji programu, znaczenie jest tak samo, jakby były łączone w treści bloku metody `Main` klasy `Program` w globalnej przestrzeni nazw, w następujący sposób:

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Należy zauważyć, że nazwy "program" i "Main" są używane tylko w celach ilustracji, rzeczywiste nazwy używane przez kompilator są zależne od implementacji, a nie typu ani nie można odwoływać się do niego przy użyciu nazwy z kodu źródłowego.

Metoda jest oznaczona jako punkt wejścia programu. Jawnie zadeklarowane metody, które Konwencją mogą być traktowane jako kandydatów punktu wejścia są ignorowane. Ostrzeżenie jest zgłaszane, gdy wystąpi. Wystąpił błąd podczas określania przełącznika kompilatora `-main:<type>`, gdy istnieją instrukcje najwyższego poziomu.

Operacje asynchroniczne są dozwolone w instrukcjach najwyższego poziomu do zakresu, w którym są dozwolone w instrukcjach w ramach zwykłej asynchronicznej metody punktu wejścia. Jednak nie są one wymagane, jeśli `await` wyrażenia i inne operacje asynchroniczne są pomijane, żadne ostrzeżenie nie zostanie wygenerowane. Zamiast tego sygnatura wygenerowanej metody punktu wejścia jest równoważna z 
``` c#
    static void Main()
```

W powyższym przykładzie zostanie przedstawiony następujący `$Main` deklaracji metody:

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

W tym samym czasie przykład podobny do tego:
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

da:
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

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>Zakres zmiennych lokalnych najwyższego poziomu i lokalnych funkcji

Mimo że lokalne zmienne i funkcje najwyższego poziomu są "opakowane" do wygenerowanej metody punktu wejścia, nadal powinny one znajdować się w zakresie w całym programie w każdej jednostce kompilacji.
Na potrzeby oceny prostej nazwy, gdy zostanie osiągnięta globalna przestrzeń nazw:
- Najpierw podjęto próbę oszacowania nazwy w wygenerowanej metodzie punktu wejścia i tylko wtedy, gdy ta próba nie powiedzie się. 
- Trwa wykonywanie "regularnej" oceny w globalnej deklaracji przestrzeni nazw. 

Może to prowadzić do przesłania nazw i typów zadeklarowanych w globalnej przestrzeni nazw oraz do przesłaniania zaimportowanych nazw.

Jeśli prosta Ocena nazw występuje poza instrukcjami najwyższego poziomu, a Ocena zwraca zmienną lokalną lub funkcję najwyższego poziomu, która powinna prowadzić do błędu.

W ten sposób chronimy naszą przyszłą możliwość lepszego korzystania z "funkcji najwyższego poziomu" (scenariusz 2 w https://github.com/dotnet/csharplang/issues/3117)i że można zapewnić przydatną diagnostykę użytkownikom, którzy w niedrogi sposób uważają je za obsługiwaną.

