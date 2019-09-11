---
ms.openlocfilehash: 8bc4bf6310fb8a8457beee167f18d30aaca10a8e
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876903"
---
# <a name="introduction"></a>Wprowadzenie

C# (wymowa: „si szarp”) to prosty, nowoczesny, zorientowany obiektowo i bezpieczny język programowania. C#ma swoje elementy główne w rodzinie C i będzie od razu znane programistom języków C, C++i Java. C#jest ustandaryzowany przez ECMA International jako standard ***ECMA-334*** i ISO/IEC jako standard ***iso/IEC 23270*** . C# Kompilator firmy Microsoft dla .NET Framework jest zgodny z implementacją obu tych standardów.

C# to język zorientowany obiektowo, ale obsługuje również programowanie ***zorientowane na składniki***. Tworzone dziś oprogramowanie w coraz większym stopniu opiera się na składnikach mających formę niezależnych i samoopisujących się pakietów funkcji. Kluczem są tutaj właściwości, metody i zdarzenia dostarczanie przez składniki. Składniki mają swoje atrybuty, które zapewniają informacje deklaratywne, a także obejmują dokumentację. C#Program udostępnia konstrukcje językowe umożliwiające bezpośrednie wsparcie tych koncepcji, C# co sprawia, że jest to bardzo naturalny język, w którym można tworzyć i używać składników oprogramowania.

C# ma również bardzo pomocne funkcje do tworzenia niezawodnych i trwałych aplikacji. ***Wyrzucanie elementów bezużytecznych*** automatycznie odzyskuje pamięć zajmowaną przez nieużywane obiekty; ***Obsługa wyjątków*** zapewnia strukturalne i rozszerzalne podejście do wykrywania błędów i odzyskiwania. a ***bezpieczny typ*** projektu języka sprawia, że nie można odczytać z niezainicjowanych zmiennych, indeksować tablice poza granicami lub wykonywać rzutowania typu unchecked.

C# korzysta z ***ujednoliconego systemu typów***. Wszystkie typy w tym języku, w tym typy podstawowe takie jak `int` czy `double`, dziedziczą z jednego typu głównego typu `object`. W rezultacie wszystkie typy dzielą między sobą ten sam zestaw metod, a wartości dowolnego typu można zapisywać, przesyłać i wykorzystywać w jednolity sposób. C# obsługuje też typy odwołań i wartości zdefiniowanych przez użytkownika, co pozwala na dynamiczną alokację obiektów, a także na wbudowane przechowywanie uproszczonych struktur.

Aby upewnić C# się, że programy i biblioteki mogą się rozwijać w czasie w zgodnym zakresie, przeznaczenie ma duże znaczenie C#w projekcie ***wersji*** . W wielu językach programowania jest mało uwagi na ten problem, a w efekcie programy w tych językach są częściej częściej niż to konieczne, gdy są wprowadzane nowsze wersje bibliotek zależnych. C#Zagadnienia dotyczące projektu, które miały bezpośredni wpływ na wersje, obejmują oddzielność `virtual` i `override` modyfikatory, reguły rozpoznawania przeciążania metod oraz obsługę jawnych deklaracji elementów członkowskich interfejsu.

Pozostała część tego rozdziału zawiera opis najważniejszych funkcji C# języka. Chociaż dalsze rozdziały opisują reguły i wyjątki w szczegółowo zorientowanym i niekiedy w sposób matematyczny, ten rozdział dąży do przejrzystości i zwięzłości na koszt kompletności. Celem jest zapewnienie czytelnikowi wprowadzenia do języka, który ułatwia pisanie wczesnych programów i odczytywanie dalszych rozdziałów.

## <a name="hello-world"></a>Hello, World

„Hello, World” to program używany tradycyjnie jako wprowadzenie do języka programowania. Poniżej przedstawiono jego wersję w C#:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

Pliki źródłowe C# zazwyczaj mają rozszerzenie `.cs`. Przy założeniu, że program "Hello, World" jest przechowywany w `hello.cs`pliku, program można skompilować za pomocą kompilatora firmy C# Microsoft przy użyciu wiersza polecenia
```
csc hello.cs
```
tworzy zestaw wykonywalny o nazwie `hello.exe`. Dane wyjściowe generowane przez tę aplikację po jej uruchomieniu to
```
Hello, World
```

Program „Hello, World” rozpoczyna się od dyrektywy `using`, która odwołuje się do przestrzeni nazw `System`. Przestrzenie nazw zapewniają hierarchiczny sposób organizowania programów i bibliotek C#. Zawierają one typy i inne przestrzenie nazw — np. przestrzeń nazw `System` zawiera wiele typów (takich jak klasa `Console`, do której odwołuje się program) i wiele innych przestrzeni nazw, takich jak `IO` czy `Collections`. Dyrektywa `using` odwołująca się do danej przestrzeni nazw umożliwia niekwalifikowane korzystanie z typów, które są składowymi tej przestrzeni nazw. Dzięki skorzystaniu z dyrektywy `using` program może użyć polecenia `Console.WriteLine` jako skrótu dla `System.Console.WriteLine`.

Klasa `Hello` zadeklarowa przez program „Hello, World” zawiera jedną składową, którą jest metoda o nazwie `Main`. Metoda jest zadeklarowana `static` z modyfikatorem. `Main` Podczas gdy metody wystąpień mogą odwoływać się do konkretnego wystąpienia obiektu otaczającego przy użyciu słowa kluczowego `this`, metody statyczne działają bez odwoływania się do konkretnego obiektu. Zgodnie z konwencją, metoda statyczna o nazwie `Main` służy jako punkt wejścia programu.

Dane wyjściowe programu są generowane przez metodę `WriteLine` klasy `Console` w przestrzeni nazw `System`. Ta klasa jest dostarczana przez .NET Framework biblioteki klas, które domyślnie są przywoływane przez kompilator firmy Microsoft C# . Należy zauważyć C# , że sama nie ma oddzielnej biblioteki środowiska uruchomieniowego. Zamiast tego .NET Framework jest biblioteką środowiska uruchomieniowego C#programu.

## <a name="program-structure"></a>Struktura programu

Kluczowe koncepcje organizacyjne C# w programie to ***programy***, ***przestrzenie nazw***, ***typy***, ***elementy członkowskie***i ***zestawy***. C#programy składają się z co najmniej jednego pliku źródłowego. Programy deklarują typy, które zawierają składowe i mogą być zorganizowane w przestrzenie nazw. Klasy i interfejsy są przykładami typów. Pola, metody, właściwości i zdarzenia są przykładami elementów członkowskich. Gdy C# programy są kompilowane, są fizycznie spakowane do zestawów. Zestawy `.exe` zazwyczaj mają rozszerzenie pliku lub `.dll`, w zależności od tego, czy implementują ***aplikacje*** lub ***biblioteki***.

Przykład

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
deklaruje klasę o `Stack` nazwie w przestrzeni nazw `Acme.Collections`o nazwie. W pełni kwalifikowana nazwa tej klasy `Acme.Collections.Stack`to. Klasa zawiera kilka elementów członkowskich: pole o nazwie `top`, dwie metody o `Push` nazwie `Pop`i i zagnieżdżoną klasę o `Entry`nazwie. Klasa dodatkowo zawiera trzy elementy członkowskie: pole o nazwie `next`, pole o nazwie `data`i Konstruktor. `Entry` Przy założeniu, że kod źródłowy przykładu jest przechowywany w pliku `acme.cs`, wiersz polecenia

```
csc /t:library acme.cs
```
kompiluje przykład jako bibliotekę (kod bez `Main` punktu wejścia) i tworzy zestaw o nazwie. `acme.dll`

Zestawy zawierają kod wykonywalny w postaci instrukcji ***języka pośredniego*** (IL) i informacji symbolicznych w formie ***metadanych***. Przed wykonaniem kod IL w zestawie jest automatycznie konwertowany na kod specyficzny dla procesora przez kompilator just-in-Time (JIT) środowiska uruchomieniowego języka wspólnego platformy .NET.

Ponieważ zestaw jest samoopisującą się jednostką funkcji, która zawiera kod i metadane, nie ma potrzeby stosowania `#include` dyrektyw i plików nagłówkowych w programie. C# Typy publiczne i składowe zawarte w określonym zestawie są udostępniane w C# programie po prostu przez odwołanie się do tego zestawu podczas kompilowania programu. Na przykład ten program używa `Acme.Collections.Stack` klasy `acme.dll` z zestawu:

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
Jeśli program jest `test.cs`przechowywany w pliku, po `test.cs` skompilowaniu `acme.dll` `/r` zestawu można odwoływać się za pomocą opcji kompilatora:

```
csc /r:acme.dll test.cs
```
Spowoduje to utworzenie zestawu wykonywalnego `test.exe`o nazwie, który w przypadku uruchamiania generuje dane wyjściowe:

```
100
10
1
```
C#zezwala na przechowywanie tekstu źródłowego programu w kilku plikach źródłowych. W przypadku kompilowania programu C# z obsługą wielu plików wszystkie pliki źródłowe są przetwarzane razem, a pliki źródłowe mogą swobodnie odwoływać się do siebie nawzajem — jest to tak samo, jakby wszystkie pliki źródłowe zostały połączone w jeden duży plik przed przetworzeniem. Deklaracje przesyłania dalej nie są C# nigdy niewymagane w programie, w związku z czym z kilkoma wyjątkami, zamówienie deklaracji jest nieważne. C#nie ogranicza pliku źródłowego do deklarowania tylko jednego typu publicznego ani nie wymaga, aby nazwa pliku źródłowego była zgodna z typem zadeklarowanym w pliku źródłowym.

## <a name="types-and-variables"></a>Typy i zmienne

Istnieją dwa rodzaje typów w C#: ***typy wartości*** i ***typy odwołań***. Zmienne typów wartości bezpośrednio zawierają swoje dane, a zmienne typów referencyjnych przechowują odwołania do danych, które są znane jako obiekty. W przypadku typów referencyjnych istnieje możliwość, że dwie zmienne odwołują się do tego samego obiektu, w tym przypadku operacje na jednej zmiennej mają wpływ na obiekt, do którego odwołuje się inna zmienna. W przypadku typów wartości zmiennych każda z nich ma własną kopię danych i nie jest możliwe wykonywanie operacji na nich, aby wpływać na inne (z wyjątkiem `ref` zmiennych i `out` parametrów).

C#typy wartości są dalej podzielone na ***typy proste***, ***typy wyliczeniowe***, ***typy struktur***i ***Typy dopuszczające wartość null***, a C#typy odwołań są dalej podzielone na ***typy klas***, ***typy interfejsów***, ***Tablica typy***i ***typy delegatów***.

Poniższa tabela zawiera omówienie C#systemu typu.

| __Kategoria__    |                 | __Opis__ |
|-----------------|-----------------|-----------------|
| Typy wartości     | Typy proste    | Całkowita część ze `sbyte`znakiem `int`:, `short`,,`long` |
|                 |                 | Całka bez `byte`znaku `ushort`: `uint`,,,`ulong` |
|                 |                 | Znaki Unicode:`char` |
|                 |                 | Liczba zmiennoprzecinkowa IEEE `float`:,`double` |
|                 |                 | Liczba dziesiętna o dużej precyzji:`decimal` |
|                 |                 | Typu`bool` |
|                 | Typy wyliczeniowe      | Typy formularza zdefiniowane przez użytkownika`enum E {...}` |
|                 | Typy struktur    | Typy formularza zdefiniowane przez użytkownika`struct S {...}` |
|                 | Typy dopuszczające wartości null  | Rozszerzenia wszystkich innych typów wartości z `null` wartością |
| Typy odwołań | Typy klas     | Ostateczna Klasa bazowa dla wszystkich innych typów:`object` |
|                 |                 | Ciągi Unicode:`string` |
|                 |                 | Typy formularza zdefiniowane przez użytkownika`class C {...}` |
|                 | Typy interfejsów | Typy formularza zdefiniowane przez użytkownika`interface I {...}` |
|                 | Typy tablic     | Pojedyncze i wielowymiarowe, na przykład `int[]` i`int[,]` |
|                 | Typy delegatów  | Typy formularzy zdefiniowane przez użytkownika, np.`delegate int  D(...)` |

Osiem typów całkowitych zapewnia obsługę 8-bitowych, 16-bitowych, 32-bitowych i 64-bitowych wartości w postaci podpisanej lub niepodpisanej.

Dwa typy zmiennoprzecinkowe, `float` i `double`, są reprezentowane przy użyciu 32-bitowej pojedynczej precyzji i 64-bitowe formatów IEEE 754 o podwójnej precyzji.

`decimal` Typ jest 128-bitowym typem danych odpowiednim dla obliczeń finansowych i pieniężnych.

C#Typ jest używany do reprezentowania wartości logicznych — wartości, które `true` są lub `false`. `bool`

Przetwarzanie znaków i ciągów w C# programie używa kodowania Unicode. Typ reprezentuje jednostkę kodu UTF-16, `string` a typ reprezentuje sekwencję jednostek kodu UTF-16. `char`

Poniższa tabela zawiera podsumowanie C#typów liczbowych.


| __Kategoria__      | __Bitów__ | __Typ__  | __Zakres/precyzja__ |
|-------------------|----------|-----------|---------------------|
| Całkowita ze znakiem   | 8        | `sbyte`   | -128...127 |
|                   | 16       | `short`   | -32768... 32, 767 |
|                   | 32       | `int`     | -2147483648... 2, 147, 483 647 |
|                   | 64       | `long`    | -zakresu od... 9, 223, 372, 036, 854, 775, |
| Całkowita bez znaku | 8        | `byte`    | 0... 255 |
|                   | 16       | `ushort`  | 0... 65, 535 |
|                   | 32       | `uint`    | 0... 4, 294, 967, 295 |
|                   | 64       | `ulong`   | 0... 18, 446, 744, 073, 709, 551, 615 |
| Liczba zmiennoprzecinkowa    | 32       | `float`   | 1,5 × 10 ^ − 45 do 3,4 × 10 ^ 38, 7-cyfrowa precyzja |
|                   | 64       | `double`  | 5,0 × 10 ^ − 324 do 1,7 × 10 ^ 308, 15-cyfrowa precyzja |
| Wartość dziesiętna           | 128      | `decimal` | 1,0 × 10 ^ − 28 do 7,9 × 10 ^ 28, 28-cyfrowa precyzja |

C#programy używają ***deklaracji typu*** do tworzenia nowych typów. Deklaracja typu określa nazwę i składowe nowego typu. C#Pięć kategorii typów są definiowane przez użytkownika: typy klas, typy struktur, typy interfejsów, typy wyliczeniowe i typy delegatów.

Typ klasy definiuje strukturę danych, która zawiera składowe danych (pola) i składowe funkcji (metody, właściwości i inne). Typy klas obsługują pojedyncze dziedziczenie i polimorfizm, czyli mechanizmy, w których klasy pochodne mogą poszerzać i specjalizację klas bazowych.

Typ struktury jest podobny do typu klasy w tym, że reprezentuje strukturę z składowymi danych i składowymi funkcji. Jednak w przeciwieństwie do klas, struktury są typami wartości i nie wymagają przydziału sterty. Typy struktur nie obsługują dziedziczenia określonego przez użytkownika, a wszystkie typy struktur niejawnie dziedziczą `object`po typie.

Typ interfejsu definiuje kontrakt jako nazwany zestaw elementów członkowskich funkcji publicznych. Klasa lub struktura implementująca interfejs musi zapewniać implementacje składowych funkcji interfejsu. Interfejs może dziedziczyć z wielu interfejsów podstawowych, a Klasa lub struktura może zaimplementować wiele interfejsów.

Typ delegata reprezentuje odwołania do metod z określoną listą parametrów i zwracanym typem. Delegaty umożliwiają traktowanie metod jako jednostek, które mogą być przypisane do zmiennych i przekazane jako parametry. Delegaty są podobne do koncepcji wskaźników funkcji, które znajdują się w innych językach, ale w przeciwieństwie do wskaźników funkcji Delegaty są zorientowane obiektowo i są bezpieczne dla typów.

Typy klas, struktur, interfejsów i delegatów obsługują ogólne, dzięki czemu można je sparametryzowane z innymi typami.

Typ wyliczeniowy jest typem odrębnym o nazwanych stałych. Każdy typ wyliczeniowy ma typ podstawowy, który musi być jednym z ośmiu typów całkowitych. Zestaw wartości typu wyliczeniowego jest taki sam jak zestaw wartości typu podstawowego.

C#obsługuje tablice pojedynczych i wielowymiarowych dowolnego typu. W przeciwieństwie do typów wymienionych powyżej, typy tablicy nie muszą być zadeklarowane przed użyciem. Zamiast tego typy tablic są konstruowane przez następujące nazwy typu z nawiasami kwadratowymi. Na przykład `int[]` jest `int` `int` `int`tablicą jednowymiarową, `int[,]` która jest tablicą dwuwymiarową, i `int[][]` jest tablicą jednowymiarową tablic jednowymiarowych.

Typy dopuszczające wartość null nie muszą również być zadeklarowane, aby można było ich używać. Dla każdego typu `T` wartości niedopuszczających wartości null istnieje odpowiedni typ `T?`dopuszczający wartość null, który może `null`zawierać dodatkowe wartości. Na przykład `int?` jest typem, który może zawierać dowolną 32 bitową liczbę całkowitą lub wartość `null`.

C#System typów jest jednorodny tak, że wartość dowolnego typu może być traktowana jako obiekt. Każdy typ C# bezpośrednio lub pośrednio pochodzi od `object` typu klasy i `object` jest ostateczną klasą bazową wszystkich typów. Wartości typów referencyjnych są traktowane jako obiekty, po prostu wyświetlając wartości jako typ `object`. Wartości typów wartości są traktowane jako obiekty ***przez wykonywanie operacji*** ***pakowania i*** rozpakowywania. W poniższym przykładzie `int` wartość jest konwertowana na `object` i z powrotem do `int`.

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
Gdy wartość typu wartości jest konwertowana na typ `object`, wystąpienie obiektu, nazywane również "polem", jest przydzielone do przechowywania wartości, a wartość jest kopiowana do tego pola. Z drugiej strony, `object` gdy odwołanie jest rzutowane na typ wartości, jest przeprowadzane sprawdzenie, że przywoływany obiekt jest polem poprawnego typu wartości i, jeśli sprawdzenie zakończy się powodzeniem, wartość w polu jest kopiowana.

C#ujednolicony system typów efektywnie oznacza, że typy wartości mogą stać się obiektami "na żądanie". Ze względu na niezjednoczenie bibliotek ogólnego przeznaczenia, które używają `object` typu, można używać zarówno z typami referencyjnymi, jak i typami wartości.

W programie istnieją różne rodzaje ***zmiennych*** , w C#tym pola, elementy tablicy, zmienne lokalne i parametry. Zmienne reprezentują lokalizacje przechowywania, a Każda zmienna ma typ, który określa, jakie wartości mogą być przechowywane w zmiennej, jak pokazano w poniższej tabeli.


| __Typ zmiennej__    | __Możliwa zawartość__ |
|-------------------------|-----------------------|
| Typ wartości niedopuszczający wartości null | Wartość tego dokładnego typu |
| Typ wartości null     | Wartość null lub wartość tego dokładnego typu |
| `object`                | Odwołanie o wartości null, odwołanie do obiektu dowolnego typu odwołania lub odwołanie do wartości opakowanej dowolnego typu wartości |
| Typ klasy              | Odwołanie o wartości null, odwołanie do wystąpienia tego typu klasy lub odwołanie do wystąpienia klasy pochodzącej od tego typu klasy. |
| Typ interfejsu          | Odwołanie o wartości null, odwołanie do wystąpienia typu klasy implementującego ten typ interfejsu lub odwołanie do wartości opakowanej typu wartości implementującej ten typ interfejsu |
| Typ tablicy              | Odwołanie o wartości null, odwołanie do wystąpienia tego typu tablicy lub odwołanie do wystąpienia zgodnego typu tablicy |
| Typ delegata           | Odwołanie o wartości null lub odwołanie do wystąpienia tego typu delegata |

## <a name="expressions"></a>Wyrażenia

***Wyrażenia*** są konstruowane na podstawie ***operandów*** i ***operatorów***. Operatory wyrażenia wskazują operacje do zastosowania dla operandów. Przykłady operatorów to `+`, `-`, `*`, `/`i `new`. Przykładami operandów są literały, pola, zmienne lokalne i wyrażenia.

Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególne operatory. Na przykład, wyrażenie `x + y * z` jest oceniane jako `x + (y * z)`, ponieważ operator `*` ma wyższy priorytet niż operator `+`.

Większość operatorów może być ***przeciążona***. Przeciążanie operatora umożliwia określanie definiowanych przez użytkownika implementacji operatorów dla operacji, w których jeden lub oba operandy mają typ struktury lub klasy zdefiniowanej przez użytkownika.

Poniższa tabela podsumowuje C#operatory, wymieniając kategorie operatorów w kolejności od najwyższego do najniższego. Operatory w tej samej kategorii mają takie samo pierwszeństwo.


| __Kategoria__                     | __Expression__    | __Opis__ |
|----------------------------------|-------------------|-----------------|
| Podstawowy                          | `x.m`             | Dostęp do elementu członkowskiego |
|                                  | `x(...)`          | Wywołanie metody i delegata |
|                                  | `x[...]`          | Dostęp do tablicy i indeksatora |
|                                  | `x++`             | Postinkrementacja |
|                                  | `x--`             | Postdekrementacja |
|                                  | `new T(...)`      | Utworzenie obiektu i delegata |
|                                  | `new T(...){...}` | Tworzenie obiektu z inicjatorem |
|                                  | `new {...}`       | Inicjator obiektu anonimowego |
|                                  | `new T[...]`      | Tworzenie tablicy |
|                                  | `typeof(T)`       | Uzyskaj `System.Type` obiekt dla`T` |
|                                  | `checked(x)`      | Obliczenie wyrażenia w kontekście sprawdzanym |
|                                  | `unchecked(x)`    | Obliczenie wyrażenia w kontekście niesprawdzanym |
|                                  | `default(T)`      | Uzyskaj wartość domyślną typu`T` |
|                                  | `delegate {...}`  | Funkcja anonimowa (metoda anonimowa) |
| Jednostk                            | `+x`              | Tożsamość |
|                                  | `-x`              | Negacja |
|                                  | `!x`              | Negacja logiczna |
|                                  | `~x`              | Negacja bitowa |
|                                  | `++x`             | Preinkrementacja |
|                                  | `--x`             | Predekrementacja |
|                                  | `(T)x`            | Jawna konwersja `x` na typ `T` |
|                                  | `await x`         | Asynchronicznie poczekaj na ukończenie `x` |
| Mnożeniowy                   | `x * y`           | Mnożenie |
|                                  | `x / y`           | Dzielenie |
|                                  | `x % y`           | Reszta |
| Dana                         | `x + y`           | Dodawanie, łączenie ciągów, łączenie delegatów |
|                                  | `x - y`           | Odejmowanie, usuwanie delegata |
| Shift                            | `x << y`          | Przesunięcie w lewo |
|                                  | `x >> y`          | Przesunięcie w prawo |
| Testowanie relacyjne i typu      | `x < y`           | Mniejsze niż |
|                                  | `x > y`           | Większe niż |
|                                  | `x <= y`          | Mniejsze niż lub równe |
|                                  | `x >= y`          | Większe niż lub równe |
|                                  | `x is T`          | `true` Zwracaj `T`, jeśli `x` jest ,`false` w przeciwnym razie |
|                                  | `x as T`          | Zwraca `x` wartość wpisaną `T`jako, `null` lub `x` Jeśli nie jest`T` |
| Równości                         | `x == y`          | Równa się      |
|                                  | `x != y`          | Nie równa się |
| Logicznego AND                      | `x & y`           | Liczba całkowita bitowa, koniunkcja logiczna i |
| Logicznego XOR                      | `x ^ y`           | Bitowe XOR dla wartości całkowitych, logiczne XOR dla wartości binarnych |
| Logicznego OR                       | <code>x &#124; y</code> | Bitowe OR dla wartości całkowitych, logiczne OR dla wartości binarnych |
| Warunkowego AND                  | `x && y`          | Oblicza tylko wtedy, `x`gdyjest `y``true` |
| Warunkowego OR                   | <code>x &#124;&#124; y</code> | Oblicza tylko wtedy, `x`gdyjest `y``false` |
| Łączenia wartości null                  | `x ?? y`          | Jest wynikiem, Aby`x` w przeciwnym razie `null` `y` `x` |
| Warunkowe                      | `x ? y : z`       | `x` Daje w `y` przypadku, gdy`z` jest`x` `true``false` |
| Przypisania lub funkcji anonimowej | `x = y`           | Przypisanie |
|                                  | `x op= y`         | Przypisanie złożone; obsługiwane operatory to `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code> |
|                                  | `(T x) => y`      | Funkcja anonimowa (wyrażenie lambda) |

## <a name="statements"></a>Instrukcje

Akcje programu są wyrażane przy użyciu ***instrukcji***. C#obsługuje kilka różnych rodzajów instrukcji, które są zdefiniowane w zakresie osadzonych instrukcji.

***Blok*** umożliwia zapisanie wielu instrukcji w kontekstach, w których Pojedyncza instrukcja jest dozwolona. Blok składa się z listy instrukcji pisanych między ogranicznikami `{` i. `}`

***Instrukcje deklaracji*** są używane do deklarowania zmiennych lokalnych i stałych.

***Instrukcje wyrażeń*** są używane do obliczania wyrażeń. Wyrażenia, które mogą być używane jako instrukcje, obejmują wywołania metod, alokacje obiektów przy użyciu `new` operatora, przydziałów używających `=` i operatorów przypisania złożonego, operacji zwiększania i zmniejszania przy użyciu `++`operatory i`--` .

***Instrukcje wyboru*** są używane do wybierania jednej z wielu możliwych instrukcji do wykonania na podstawie wartości niektórych wyrażeń. W tej grupie są `if` instrukcje i. `switch`

***Instrukcje iteracji*** są używane do wielokrotnego wykonywania osadzonej instrukcji. W `while`tej grupie są instrukcje, `do`, `for`, i. `foreach`

***Instrukcje skoku*** są używane do transferowania kontroli. W tej `break`grupie są instrukcje, `continue`, `goto` `throw` `yield` , ,i.`return`

`try`... Instrukcja jest używana do przechwytywania wyjątków, które występują podczas wykonywania bloku, `try`i... `catch` `finally` Instrukcja służy do określania kodu finalizacji, który jest zawsze wykonywany, niezależnie od tego, czy wystąpił wyjątek, czy nie.

Instrukcje `checked` and`unchecked` są używane do kontrolowania kontekstu sprawdzania przepełnienia dla operacji arytmetycznych typu całkowitego i konwersji.

`lock` Instrukcja służy do uzyskiwania blokady wzajemnego wykluczania dla danego obiektu, wykonywania instrukcji i zwalniania blokady.

`using` Instrukcja służy do uzyskiwania zasobu, wykonywania instrukcji, a następnie usuwania tego zasobu.

Poniżej przedstawiono przykłady poszczególnych rodzajów instrukcji

__Deklaracje zmiennej lokalnej__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Lokalna deklaracja stała__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__Instrukcja wyrażenia__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if`Merge__

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


__`switch`Merge__

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

__`while`Merge__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do`Merge__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for`Merge__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach`Merge__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break`Merge__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue`Merge__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto`Merge__

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

__`return`Merge__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield`Merge__

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

__`throw`and `try` — instrukcje__

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

__`checked`and `unchecked` — instrukcje__

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

__`lock`Merge__

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

__`using`Merge__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>Klasy i obiekty

***Klasy*** są najbardziej podstawowym typem w języku C#. Klasa jest strukturą danych, która łączy stan (pola) i akcje (metody i inne elementy członkowskie funkcji) w jednej jednostce. Klasa zawiera definicję dla dynamicznie utworzonych ***wystąpień*** klasy, znanych również jako ***obiekty***. Klasy obsługują ***dziedziczenie*** i ***polimorfizm***, natomiast mechanizmy, w których ***klasy pochodne*** mogą poszerzać i specjalizację ***klas bazowych***.

Nowe klasy są tworzone za pomocą deklaracji klasy. Deklaracja klasy rozpoczyna się od nagłówka, który określa atrybuty i Modyfikatory klasy, nazwę klasy, klasę bazową (jeśli ma to zastosowanie) i interfejsy zaimplementowane przez klasę. Po tym nagłówku następuje treść klasy, która składa się z listy deklaracji elementów członkowskich, które są zapisywane między `{` ogranicznikami `}`i.

Poniżej znajduje się deklaracja klasy prostej o nazwie `Point`:

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
Wystąpienia klas są tworzone przy użyciu `new` operatora, który przydziela pamięć dla nowego wystąpienia, wywołuje konstruktora w celu zainicjowania wystąpienia i zwraca odwołanie do wystąpienia. Poniższe instrukcje tworzą dwa `Point` obiekty i przechowują odwołania do tych obiektów w dwóch zmiennych:

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
Pamięć zajęta przez obiekt jest automatycznie odzyskiwana, gdy obiekt nie jest już używany. Nie jest to konieczne ani możliwe, aby jawnie cofnąć alokację obiektów w programie C#.

### <a name="members"></a>Elementy członkowskie

Elementy członkowskie klasy są ***statycznymi elementami członkowskimi*** lub ***wystąpieniami***. Statyczne składowe należą do klas, a elementy członkowskie wystąpienia należą do obiektów (wystąpień klas).

Poniższa tabela zawiera omówienie rodzajów elementów członkowskich, które może zawierać Klasa.


| __Członkiem__   | __Opis__ |
|------------  |-----------------|
| Stałe    | Wartości stałe skojarzone z klasą |
| Pola       | Zmienne klasy |
| Metody      | Obliczenia i akcje, które mogą być wykonywane przez klasę |
| Właściwości   | Akcje skojarzone z odczytem i pisaniem nazwanych właściwości klasy |
| Indeksatory     | Akcje skojarzone z wystąpieniami indeksowania klasy, takimi jak tablica |
| Zdarzenia       | Powiadomienia, które mogą zostać wygenerowane przez klasę |
| Operatory    | Konwersje i operatory wyrażeń obsługiwane przez klasę |
| Konstruktorów | Akcje wymagane do zainicjowania wystąpień klasy lub samej klasy |
| Destruktory  | Akcje do wykonania przed trwałe odrzuceniem wystąpień klasy |
| Types        | Zagnieżdżone typy zadeklarowane przez klasę |

### <a name="accessibility"></a>Ułatwienia dostępu

Każdy element członkowski klasy ma skojarzoną dostępność, która kontroluje regiony tekstu programu, które mogą uzyskać dostęp do elementu członkowskiego. Istnieje pięć możliwych form ułatwień dostępu. Są one podsumowane w poniższej tabeli.


| __Ułatwienia dostępu__    | __Znaczenie__ |
|----------------------|-----------------|
| `public`             | Dostęp nie jest ograniczony |
| `protected`          | Dostęp ograniczony do tej klasy lub klas pochodnych od tej klasy |
| `internal`           | Dostęp ograniczony do tego programu |
| `protected internal` | Dostęp ograniczony do tego programu lub klas pochodzących od tej klasy |
| `private`            | Dostęp ograniczony do tej klasy |

### <a name="type-parameters"></a>Parametry typu

Definicja klasy może określać zestaw parametrów typu przez poniższą nazwę klasy z nawiasami kątowymi zawierającymi listę nazw parametrów typu. Parametry typu mogą być używane w treści deklaracji klasy do definiowania elementów członkowskich klasy. W poniższym przykładzie parametry `Pair` typu są `TFirst` i `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Typ klasy zadeklarowanej do wykonania parametrów typu jest nazywany typem klasy generycznej. Typy struktur, interfejsów i delegatów mogą być również rodzajowe.

Gdy używana jest Klasa generyczna, należy podać argumenty typu dla każdego z parametrów typu:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Typ ogólny z podanymi argumentami typu, `Pair<int,string>` jak powyżej, jest nazywany typem skonstruowanym.

### <a name="base-classes"></a>Klas podstawowych

Deklaracja klasy może określać klasę bazową, postępując według nazwy klasy i parametrów typu z dwukropkiem i nazwą klasy bazowej. Pominięcie specyfikacji klasy bazowej jest taka sama jak pochodna typu `object`. `Point3D` W poniższym przykładzie klasą bazową jest `Point`, `Point` a klasą bazową jest `object`:

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
Klasa dziedziczy elementy członkowskie swojej klasy bazowej. Dziedziczenie oznacza, że Klasa niejawnie zawiera wszystkie elementy członkowskie swojej klasy podstawowej, z wyjątkiem dla wystąpienia i konstruktorów statycznych i destruktorów klasy bazowej. Klasa pochodna może dodawać nowych członków do tych, które dziedziczy, ale nie może usunąć definicji dziedziczonego elementu członkowskiego. W poprzednim przykładzie `Point3D` `x` dziedziczy pola i `y` z `Point`, i każde `Point3D` wystąpienie zawiera trzy pola, `x`, `y`, i `z`.

Niejawna konwersja istnieje z typu klasy do dowolnego z jego typów klas podstawowych. W związku z tym zmienna typu klasy może odwoływać się do wystąpienia tej klasy lub wystąpienia dowolnej klasy pochodnej. Na przykład uwzględniając poprzednie deklaracje klas, zmienna typu `Point` może odwoływać się do `Point` lub `Point3D`:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Pola

Pole jest zmienną, która jest skojarzona z klasą lub wystąpieniem klasy.

Pole zadeklarowane z `static` modyfikatorem definiuje ***pole statyczne***. Pole statyczne identyfikuje dokładnie jedną lokalizację magazynu. Niezależnie od tego, ile wystąpień klasy zostało utworzonych, istnieje tylko jedna kopia pola statycznego.

Pole zadeklarowane bez `static` modyfikatora definiuje ***pole wystąpienia***. Każde wystąpienie klasy zawiera oddzielną kopię wszystkich pól wystąpienia tej klasy.

W `Color` poniższym przykładzie każde wystąpienie klasy ma oddzielną kopię `r` `g` `White`pól,, i `b` `Black`wystąpienia `Red`, ale istnieje tylko jedna kopia,,, `Green` i`Blue` pola statyczne:

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
Jak pokazano w poprzednim przykładzie ***pola tylko do odczytu*** mogą być zadeklarowane za pomocą `readonly` modyfikatora. Przypisanie do `readonly` pola może wystąpić tylko jako część deklaracji pola lub konstruktora w tej samej klasie.

### <a name="methods"></a>Metody

***Metoda*** to element członkowski implementujący obliczenia lub akcję, które mogą być wykonywane przez obiekt lub klasę. ***Metody statyczne*** są dostępne za pomocą klasy. ***Metody wystąpienia*** są dostępne za pomocą wystąpień klasy.

Metody mają (prawdopodobnie pustą) listę ***parametrów***reprezentujących wartości lub odwołania do zmiennych, które są przenoszone do metody i ***Typ zwracany***, który określa typ wartości obliczanej i zwracanej przez metodę. Zwracany typ metody to `void` , jeśli nie zwraca wartości.

Podobnie jak typy, metody mogą także mieć zestaw parametrów typu, dla których argumenty typu muszą być określone, gdy wywoływana jest metoda. W przeciwieństwie do typów, argumenty typu często można wywnioskować na podstawie argumentów wywołania metody i nie muszą być jawnie określone.

***Sygnatura*** metody musi być unikatowa w klasie, w której metoda jest zadeklarowana. Podpis metody składa się z nazwy metody, liczby parametrów typu oraz liczby, modyfikatorów i typów jego parametrów. Sygnatura metody nie zawiera typu zwracanego.

#### <a name="parameters"></a>Parametry

Parametry służą do przekazywania wartości lub odwołań do zmiennych do metod. Parametry metody pobierają rzeczywiste wartości z ***argumentów*** , które są określone podczas wywoływania metody. Istnieją cztery rodzaje parametrów: parametry wartości, parametry odwołania, parametry wyjściowe i tablice parametrów.

***Parametr value*** jest używany do przekazywania parametru wejściowego. Parametr value odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z argumentu, który został przesłany dla parametru. Modyfikacje parametru value nie wpływają na argument, który został przesłany dla parametru.

Parametry wartości mogą być opcjonalne, określając wartość domyślną, aby można było pominąć odpowiednie argumenty.

***Parametr Reference*** służy do przekazywania parametrów wejściowych i wyjściowych. Argument przesłany dla parametru odwołania musi być zmienną i podczas wykonywania metody parametr Reference reprezentuje tę samą lokalizację magazynu co zmienna argumentu. Parametr odwołania jest zadeklarowany z `ref` modyfikatorem. W poniższym przykładzie pokazano sposób użycia `ref` parametrów.

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
***Parametr wyjściowy*** jest używany do przekazywania parametrów wyjściowych. Parametr wyjściowy jest podobny do parametru odwołania, z tą różnicą, że początkowa wartość argumentu dostarczonego przez wywołującego jest nieważna. Parametr wyjściowy jest zadeklarowany z `out` modyfikatorem. W poniższym przykładzie pokazano sposób użycia `out` parametrów.

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
***Tablica parametrów*** umożliwia przekazanie zmiennej liczbie argumentów do metody. Tablica parametrów jest zadeklarowana z `params` modyfikatorem. Tylko ostatni parametr metody może być tablicą parametrów, a typ tablicy parametrów musi być typem tablicy jednowymiarowej. Metody `Write` i`WriteLine` klasy są dobrymi przykładami użycia tablicy parametrów. `System.Console` Są one deklarowane w następujący sposób.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
W metodzie, która używa tablicy parametrów, tablica parametrów zachowuje się dokładnie tak jak zwykły parametr typu tablicy. Jednak w wywołaniu metody z tablicą parametrów możliwe jest przekazanie jednego argumentu typu tablicy parametrów lub dowolnej liczby argumentów typu elementu tablicy parametrów w parametrze. W tym drugim przypadku wystąpienie tablicy jest automatycznie tworzone i inicjowane z podanym argumentami. Ten przykład

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
jest równoznaczny z zapisem poniżej.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Treść metody i zmienne lokalne

Treść metody Określa instrukcje do wykonania, gdy metoda jest wywoływana.

Treść metody może deklarować zmienne, które są specyficzne dla wywołania metody. Takie zmienne są nazywane ***zmiennymi lokalnymi***. Deklaracja zmiennej lokalnej określa nazwę typu, nazwę zmiennej i prawdopodobnie wartość początkową. Poniższy przykład deklaruje zmienną `i` lokalną z początkową wartością zero i zmienną `j` lokalną bez wartości początkowej.

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
C#wymaga, aby zmienna lokalna była ***przypisana ostatecznie*** przed uzyskaniem jej wartości. Na przykład jeśli deklaracja poprzedniej `i` nie zawierała wartości początkowej, kompilator zgłosi błąd dla kolejnych zastosowań `i` , ponieważ `i` nie zostanie on ostatecznie przypisany w tych punktach w programie.

Metoda może użyć `return` instrukcji do zwrócenia kontroli do obiektu wywołującego. W wyniku zwrócenia `void` `return` metody instrukcje nie mogą określać wyrażenia. W metodzie zwracającej`void`nie, `return` instrukcje muszą zawierać wyrażenie, które oblicza wartość zwracaną.

#### <a name="static-and-instance-methods"></a>Metody static i instance

Metoda zadeklarowana za pomocą `static` modyfikatora jest ***metodą statyczną***. Metoda statyczna nie działa w konkretnym wystąpieniu i może bezpośrednio uzyskiwać dostęp do statycznych elementów członkowskich.

Metoda zadeklarowana bez `static` modyfikatora jest ***metodą wystąpienia***. Metoda wystąpienia działa w konkretnym wystąpieniu i może uzyskiwać dostęp do elementów członkowskich static i instance. Wystąpienie, na którym wywołano metodę wystąpienia, można jawnie uzyskać do niego `this`dostęp. Wystąpił błąd podczas odwoływania się `this` do w metodzie statycznej.

Następująca `Entity` Klasa zawiera elementy członkowskie statyczne i wystąpienia.

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
Każde `Entity` wystąpienie zawiera numer seryjny (i najprawdopodobniej inne informacje, które nie są wyświetlane w tym miejscu). `Entity` Konstruktor (który przypomina metodę wystąpienia) Inicjuje nowe wystąpienie przy użyciu następnego dostępnego numeru seryjnego. Ponieważ Konstruktor jest członkiem wystąpienia, może uzyskać dostęp zarówno `serialNo` do pola wystąpienia, `nextSerialNo` jak i pola statycznego.

Metody `GetNextSerialNo` `serialNo` i `SetNextSerialNo`staticmogą uzyskać dostęp do pola statycznego,alemożetobyćbłąd,abyuzyskaćbezpośrednidostępdopolawystąpienia.`nextSerialNo`

Poniższy przykład pokazuje użycie `Entity` klasy.

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
Należy zauważyć, `SetNextSerialNo` że `GetNextSerialNo` metody i są wywoływane `GetSerialNo` dla klasy, podczas gdy metoda wystąpienia jest wywoływana w wystąpieniach klasy.

#### <a name="virtual-override-and-abstract-methods"></a>Metody wirtualne, przesłonięcia i abstrakcyjne

Gdy deklaracja metody wystąpienia zawiera `virtual` modyfikator, metoda jest uznawana za ***metodę wirtualną***. Gdy modyfikator `virtual` nie jest obecny, metoda jest uznawana za ***metodę niewirtualną***.

Gdy wywoływana jest metoda wirtualna, ***typem czasu wykonywania*** wystąpienia, dla którego odbywa się wywołanie określa rzeczywistą implementację metody do wywołania. W wywołaniu metody niewirtualnej ***Typ czasu kompilacji*** wystąpienia jest czynnikiem decydującym.

Metoda wirtualna może zostać ***przesłonięta*** w klasie pochodnej. Gdy deklaracja metody wystąpienia zawiera `override` modyfikator, metoda zastępuje dziedziczną metodę wirtualną z tą samą sygnaturą. Podczas gdy deklaracja metody wirtualnej wprowadza nową metodę, Deklaracja metody przesłonięcia specjalizacji istniejącej dziedziczonej metody wirtualnej, dostarczając nową implementację tej metody.

Metoda ***abstrakcyjna*** jest metodą wirtualną bez implementacji. Metoda abstrakcyjna jest zadeklarowana z `abstract` modyfikatorem i jest dozwolona tylko w klasie, która również jest `abstract`zadeklarowana. Metoda abstrakcyjna musi zostać przesłonięta w każdej nieabstrakcyjnej klasie pochodnej.

Poniższy przykład deklaruje klasę `Expression`abstrakcyjną, która reprezentuje węzeł drzewa wyrażenia i trzy `Constant`klasy pochodne,, `VariableReference`i `Operation`, które implementują węzły drzewa wyrażeń dla stałych, zmienna odwołania i operacje arytmetyczne. (Jest to podobne do, ale nie należy mylić z typami drzewa wyrażenia wprowadzonymi w [typach drzew wyrażeń](types.md#expression-tree-types)).

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
Poprzednie cztery klasy mogą służyć do modelowania wyrażeń arytmetycznych. Na przykład przy użyciu wystąpień tych klas wyrażenie `x + 3` może być reprezentowane w następujący sposób.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
Metoda wystąpienia jest wywoływana w celu obliczenia `double` danego wyrażenia i utworzenia wartości. `Expression` `Evaluate` Metoda przyjmuje jako argument a `Hashtable` , który zawiera nazwy zmiennych (jako klucze wpisów) i wartości (jako wartości wpisów). `Evaluate` Metoda jest wirtualną metodą abstrakcyjną, co oznacza, że nieabstrakcyjne klasy pochodne muszą zastąpić je, aby zapewnić rzeczywistą implementację.

`Constant` Implementacjapoprostu`Evaluate` zwraca przechowywaną stałą. Implementacja `VariableReference`programu wyszukuje nazwę zmiennej w elemencie Hashtable i zwraca wartość wynikową. Implementacja najpierw szacuje lewy i prawy operand (cyklicznie wywołując ich `Evaluate` metody), a następnie wykonuje daną operację arytmetyczną. `Operation`

Poniższy program używa `Expression` klas do obliczenia wyrażenia `x * (y + 2)` pod kątem różnych wartości `x` i `y`.

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

#### <a name="method-overloading"></a>Przeciążanie metody

***Przeciążanie*** metod pozwala wielu metodom w tej samej klasie mieć taką samą nazwę, o ile mają unikatowe podpisy. Podczas kompilowania wywołania przeciążonej metody kompilator używa ***rozdzielczości przeciążenia*** do określenia konkretnej metody do wywołania. Rozpoznanie przeciążenia umożliwia znalezienie jednej metody, która najlepiej pasuje do argumentów lub zgłasza błąd, jeśli nie można znaleźć pojedynczego najlepszego dopasowania. W poniższym przykładzie przedstawiono sposób rozwiązywania przeciążenia. Komentarz dla każdego wywołania `Main` metody pokazuje, która metoda jest faktycznie wywoływana.

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
Jak pokazano w przykładzie, dana metoda może być zawsze wybierana przez jawne rzutowanie argumentów na dokładne typy parametrów i/lub jawne dostarczenie argumentów typu.

### <a name="other-function-members"></a>Inne elementy członkowskie funkcji

Elementy członkowskie, które zawierają kod wykonywalny, są określane zbiorczo jako ***elementy członkowskie*** klasy. W poprzedniej sekcji opisano metody, które są podstawowym rodzajem elementów członkowskich funkcji. W tej sekcji opisano inne rodzaje składowych funkcji obsługiwane przez C#: konstruktory, właściwości, indeksatory, zdarzenia, operatory i destruktory.

Poniższy kod przedstawia klasę generyczną o nazwie `List<T>`, która implementuje rozwijaną listę obiektów. Klasa zawiera kilka przykładów typowych rodzajów elementów członkowskich funkcji.


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

#### <a name="constructors"></a>Konstruktorów

C#obsługuje zarówno konstruktory wystąpienia, jak i statyczne. ***Konstruktor wystąpienia*** jest członkiem, który implementuje akcje wymagane do zainicjowania wystąpienia klasy. ***Statyczny Konstruktor*** jest członkiem, który implementuje akcje wymagane do zainicjowania samej klasy podczas pierwszego ładowania.

Konstruktor jest zadeklarowany jak metoda bez zwracanego typu i o takiej samej nazwie jak zawierająca klasy. Jeśli deklaracja konstruktora zawiera `static` modyfikator, deklaruje Konstruktor statyczny. W przeciwnym razie deklaruje Konstruktor wystąpienia.

Konstruktory wystąpień mogą być przeciążone. Na przykład `List<T>` Klasa deklaruje dwa konstruktory wystąpień, jeden bez parametrów i jeden, który `int` pobiera parametr. Konstruktory wystąpień są wywoływane przy `new` użyciu operatora. Poniższe instrukcje przydzielą `List<string>` dwa wystąpienia przy użyciu każdego konstruktora `List` klasy.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
W przeciwieństwie do innych elementów członkowskich, konstruktory wystąpień nie są dziedziczone, a Klasa nie ma konstruktorów wystąpień innych niż zadeklarowane w klasie. Jeśli nie podano konstruktora wystąpienia dla klasy, zostanie automatycznie podana pusta wartość bez parametrów.

#### <a name="properties"></a>Właściwości

***Właściwości*** są naturalnym rozszerzeniem pól. Oba są nazwanymi członkami ze skojarzonymi typami, a składnia dostępu do pól i właściwości jest taka sama. Jednak w przeciwieństwie do pól właściwości nie oznacza lokalizacji magazynu. Zamiast tego właściwości mają metody ***dostępu*** określające instrukcje, które mają być wykonywane, gdy ich wartości są odczytywane lub zapisywane.

Właściwość jest zadeklarowana jako pole, z tą różnicą, że deklaracja kończy `get` się akcesorem i/ `set` lub akcesorem zapisanym `{` między ogranicznikami i `}` zamiast kończyć się średnikiem. Właściwość, która `get` ma zarówno akcesor, `set` jak i akcesora `get` , jest ***właściwością do odczytu i zapisu***, właściwość, która ma tylko metodę dostępu, jest ***właściwością tylko do odczytu***i właściwość, która `set` ma tylko metodę dostępu, jest ***Właściwość tylko do zapisu***.

Metoda `get` dostępu odpowiada metodzie bez parametrów z wartością zwracaną typu właściwości. Poza elementem docelowym przypisania, gdy właściwość jest przywoływana w wyrażeniu, `get` metoda dostępu do właściwości jest wywoływana w celu obliczenia wartości właściwości.

Metoda dostępu odpowiada metodzie z pojedynczym parametrem o nazwie `value` i bez zwracanego typu. `set` Gdy właściwość jest przywoływana jako element docelowy przypisania lub jako operand `++` lub `--`, `set` metoda dostępu jest wywoływana z argumentem, który dostarcza nową wartość.

Klasa deklaruje dwie `Count` właściwości i `Capacity`, które są tylko do odczytu i odczytu i zapisu. `List<T>` Poniżej przedstawiono przykład użycia tych właściwości.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Podobnie jak pola i metody, C# obsługuje zarówno właściwości wystąpienia, jak i właściwości statyczne. Właściwości statyczne są zadeklarowane za `static` pomocą modyfikatora, a właściwości wystąpienia są deklarowane bez użycia.

Metody dostępu właściwości mogą być wirtualne. Gdy Deklaracja właściwości zawiera `virtual`modyfikator, `abstract`lub `override` , ma zastosowanie do akcesorów właściwości.

#### <a name="indexers"></a>Indeksatory

***Indeksator*** jest członkiem, który umożliwia indeksowanie obiektów w taki sam sposób jak w przypadku tablicy. Indeksator jest zadeklarowany jak właściwość, z tą różnicą, że `this` po nazwie składowej następuje lista parametrów zapisywana między `[` ogranicznikami i `]`. Parametry są dostępne w metodach dostępu indeksatora. Podobnie jak w przypadku właściwości, indeksatory mogą być tylko do odczytu i zapisu, tylko do odczytu i do zapisu, a Akcesory dla indeksatora mogą być wirtualne.

Klasa deklaruje pojedynczy indeksator do odczytu i zapisu, który `int` pobiera parametr. `List` Indeksator umożliwia indeksowanie `List` wystąpień z `int` wartościami. Na przykład

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
Indeksatory mogą być przeciążone, co oznacza, że Klasa może deklarować wiele indeksatorów, o ile liczba lub typy ich parametrów różnią się.

#### <a name="events"></a>Zdarzenia

***Zdarzenie*** jest członkiem, który umożliwia klasy lub obiektowi dostarczanie powiadomień. Zdarzenie jest zadeklarowane jak pole, z tą różnicą, że `event` deklaracja zawiera słowo kluczowe i typ musi być typem delegata.

W obrębie klasy, która deklaruje element członkowski zdarzenia, zdarzenie zachowuje się podobnie jak pole typu delegata (pod warunkiem, że zdarzenie nie jest abstrakcyjne i nie deklaruje metod dostępu). W polu jest przechowywane odwołanie do delegata, który reprezentuje programy obsługi zdarzeń, które zostały dodane do zdarzenia. Jeśli nie ma żadnych dojść do zdarzeń, pole jest `null`.

Klasa deklaruje pojedynczy element członkowski zdarzenia o `Changed`nazwie, który wskazuje, że dodano nowy element do listy. `List<T>` Zdarzenie jest zgłaszane `OnChanged` przez metodę wirtualną, która najpierw sprawdza, czy zdarzenie jest `null` (oznacza, że nie ma żadnych programów obsługi). `Changed` Pojęcie związane z wywoływaniem zdarzenia jest dokładnie równoważne do wywołania delegata reprezentowanego przez zdarzenie, co oznacza, że nie istnieją żadne specjalne konstrukcje języka do wywoływania zdarzeń.

Klienci reagują na zdarzenia za poorednictwem ***programów obsługi zdarzeń***. Procedury obsługi zdarzeń są dołączane `+=` przy użyciu operatora i usuwane `-=` przy użyciu operatora. Poniższy przykład dołącza procedurę obsługi zdarzeń do `Changed` zdarzenia. `List<string>`

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
W przypadku zaawansowanych scenariuszy, w których wymagana jest kontrola bazowego magazynu zdarzenia, deklaracja zdarzenia może jawnie dostarczyć `add` i `remove` akcesorów, które `set` są nieco podobne do metody dostępu do właściwości.

#### <a name="operators"></a>Operatory

***Operator*** jest członkiem, który definiuje znaczenie zastosowania określonego operatora wyrażenia do wystąpienia klasy. Można zdefiniować trzy rodzaje operatorów: operatory jednoargumentowe, operatory binarne i operatory konwersji. Wszystkie operatory muszą być zadeklarowane `public` jako `static`i.

Klasa deklaruje dwa `operator==` operatory i `operator!=`i w ten sposób daje nowe znaczenie dla wyrażeń, które stosują te `List` operatory do wystąpień. `List<T>` W tym celu operatory definiują równość dwóch `List<T>` wystąpień w porównaniu z poszczególnymi obiektami zawartymi `Equals` przy użyciu metod. Poniższy przykład używa `==` operatora do porównywania dwóch `List<int>` wystąpień.

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

Pierwsze `Console.WriteLine` dane wyjściowe `True` , ponieważ dwie listy zawierają tę samą liczbę obiektów z tymi samymi wartościami w tej samej kolejności. `Console.WriteLine` `False` `List<int>` Nie zdefiniowano `operator==`, pierwsze miałoby wynik, ponieważ `a` i odwołuje się do `b` różnych wystąpień. `List<T>`

#### <a name="destructors"></a>Destruktory

***Destruktor*** jest członkiem, który implementuje akcje wymagane do zadestruktorowania wystąpienia klasy. Destruktory nie mogą mieć parametrów, nie mogą mieć modyfikatorów dostępności i nie mogą być wywoływane jawnie. Destruktor dla wystąpienia jest wywoływany automatycznie podczas wyrzucania elementów bezużytecznych.

Moduł wyrzucania elementów bezużytecznych jest dozwolony w przypadku podejmowania decyzji o zbieraniu obiektów i destruktorów uruchamiania. W odróżnieniu od czasu wywołania destruktorów nie jest deterministyczna, a destruktory mogą być wykonywane na dowolnym wątku. Z tych i innych powodów klasy powinny implementować destruktory tylko wtedy, gdy żadne inne rozwiązania nie są możliwe.

`using` Instrukcja zawiera lepsze podejście do niszczenia obiektów.

## <a name="structs"></a>Struktury

Podobnie jak klasy, ***struktury*** są strukturami danych, które mogą zawierać składowe danych i składowe funkcji, ale w przeciwieństwie do klas, struktury są typami wartości i nie wymagają przydziału sterty. Zmienna typu struktury bezpośrednio przechowuje dane struktury, podczas gdy zmienna typu klasy przechowuje odwołanie do obiektu przydzielanego dynamicznie. Typy struktur nie obsługują dziedziczenia określonego przez użytkownika, a wszystkie typy struktur niejawnie dziedziczą `object`po typie.

Struktury są szczególnie przydatne w przypadku małych struktur danych, które mają semantykę wartości. Liczby zespolone, punkty w układzie współrzędnych lub pary klucz-wartość w słowniku są wszystkimi dobrymi przykładami struktur. Użycie struktur zamiast klas dla małych struktur danych może spowodować znaczną różnicę liczby alokacji pamięci wykonywanej przez aplikację. Na przykład następujący program tworzy i Inicjuje tablicę z 100 punktów. Z `Point` zaimplementowaną klasą, 101 oddzielnych obiektów są tworzone wystąpienia — jeden dla tablicy i jeden dla elementów 100.

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
Alternatywą jest tworzenie `Point` struktury.

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
Teraz zostanie utworzony tylko jeden obiekt — jeden dla tablicy — i `Point` wystąpienia są przechowywane w wierszu tablicy.

Konstruktory struktury są wywoływane z `new` operatorem, ale nie oznacza to, że pamięć jest przyznana. Zamiast dynamicznie przydzielać obiektu i zwracać odwołanie do niego, Konstruktor struktury po prostu zwraca wartość struktury (zazwyczaj w tymczasowej lokalizacji na stosie), a następnie ta wartość jest kopiowana w razie potrzeby.

Klasy, istnieje możliwość, że dwie zmienne odwołują się do tego samego obiektu, a tym samym możliwe dla operacji na jednej zmiennej, aby wpływać na obiekt, do którego odwołuje się inna zmienna. W przypadku struktur zmienne każda z nich ma własną kopię danych i nie jest możliwe wykonywanie operacji na nich, aby wpływać na inne. Na przykład dane wyjściowe generowane przez Poniższy fragment kodu zależą od tego, czy `Point` jest klasą czy strukturą.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Jeśli `Point` jest klasą, dane wyjściowe są `20` spowodowane `a` `b` tym samym obiektem. Jeśli `Point` jest strukturą, dane wyjściowe wynikają `10` z faktu `a` , `b` że przypisanie do tworzy kopię wartości i ta kopia nie ma wpływ na kolejne przypisanie do `a.x`.

Poprzedni przykład wyróżnia dwa ograniczenia struktur. Najpierw kopiowanie całej struktury jest zwykle mniej wydajne niż kopiowanie odwołania do obiektu, dlatego przypisanie i wartość przekazywanie parametrów mogą być bardziej kosztowne z strukturami niż w przypadku typów referencyjnych. Po drugie, z `ref` wyjątkiem `out` parametrów i, nie jest możliwe tworzenie odwołań do struktur, które wywołują zasady ich użycia w wielu sytuacjach.

## <a name="arrays"></a>Tablice

***Tablica*** to struktura danych zawierająca pewną liczbę zmiennych, do których dostęp jest uzyskiwany za pomocą obliczonych indeksów. Zmienne zawarte w tablicy, nazywane również ***elementami*** tablicy, są tego samego typu, a ten typ jest nazywany ***typem elementu*** tablicy.

Typy tablic są typami odwołań, a deklaracja zmiennej tablicowej po prostu ustawia miejsce na odwołanie do wystąpienia tablicy. Rzeczywiste wystąpienia tablicy są tworzone dynamicznie w czasie wykonywania przy użyciu `new` operatora. Operacja określa długość nowego wystąpienia tablicy, które jest następnie naprawione dla okresu istnienia wystąpienia. `new` Indeksy elementów z zakresu tablicy od `0` do. `Length - 1` Operator automatycznie inicjuje elementy tablicy do ich wartości domyślnych, które na przykład są równe zero dla wszystkich typów liczbowych i `null` dla wszystkich typów odwołań. `new`

Poniższy przykład tworzy tablicę `int` elementów, Inicjuje tablicę i drukuje zawartość tablicy.

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
Ten przykład tworzy i działa na ***tablicy jednowymiarowej***. C#obsługuje również ***tablice***wielowymiarowe. Liczba wymiarów typu tablicy, znana również jako ***ranga*** typu tablicy, jest taka, a liczba przecinkiów zapisywana między nawiasami kwadratowymi typu tablicy. Poniższy przykład przydziela jednowymiarową, dwuwymiarową i trójwymiarową tablicę.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
Tablica zawiera 10 elementów `a2` , tablica zawiera 50 (10 × 5 `a3` ) elementów, a tablica zawiera 100 (10 × 5 x 2) elementów. `a1`

Typ elementu tablicy może być dowolnego typu, łącznie z typem tablicy. Tablica z elementami typu tablicy jest czasami nazywana ***tablicą nieregularną*** , ponieważ długości tablic elementów nie wszystkie muszą być takie same. Poniższy przykład alokuje tablicę `int`tablic:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
Pierwszy wiersz tworzy tablicę z trzema elementami, każdy typ `int[]` i każdy z początkową `null`wartością. Kolejne wiersze inicjują następnie trzy elementy z odwołaniami do poszczególnych wystąpień tablicy o różnej długości.

Operator zezwala na określenie wartości początkowych elementów tablicy przy użyciu ***inicjatora tablicy***, który jest listą wyrażeń pisanych ogranicznikami `{` i. `}` `new` Poniższy przykład przydziela i inicjuje `int[]` z trzema elementami.

```csharp
int[] a = new int[] {1, 2, 3};
```
Należy zauważyć, że długość tablicy jest wywnioskowana na podstawie liczby wyrażeń między `{` i. `}` Deklaracje zmiennej lokalnej i pola mogą być skrócone w taki sposób, aby nie trzeba było przestawiać tego typu tablicy.

```csharp
int[] a = {1, 2, 3};
```
Oba poprzednie przykłady są równoważne z następującymi:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Interfejsy

***Interfejs*** definiuje kontrakt, który może być zaimplementowany przez klasy i struktury. Interfejs może zawierać metody, właściwości, zdarzenia i indeksatory. Interfejs nie dostarcza implementacji elementów członkowskich, które definiuje — tylko określa elementy członkowskie, które muszą być dostarczone przez klasy lub struktury, które implementują interfejs.

Interfejsy mogą wykorzystywać ***wielokrotne dziedziczenie***. W poniższym przykładzie interfejs `IComboBox` dziedziczy z obu `ITextBox` i `IListBox`.

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
Klasy i struktury mogą implementować wiele interfejsów. W poniższym przykładzie Klasa `EditBox` implementuje zarówno `IControl` , jak i `IDataBound`.

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
Gdy Klasa lub struktura implementuje określony interfejs, wystąpienia tej klasy lub struktury mogą być niejawnie konwertowane na typ tego interfejsu. Na przykład

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
W przypadkach, gdy wystąpienie nie jest statycznie znane do implementacji określonego interfejsu, można użyć rzutowania typu dynamicznego. Na przykład następujące instrukcje używają rzutowania typu dynamicznego do uzyskiwania implementacji obiektu `IControl` i `IDataBound` interfejsu. Ponieważ rzeczywisty typ obiektu to `EditBox`, Rzutowanie powiedzie się.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
W `EditBox` poprzedniej `public` klasie `Paint` Metoda `IControl` `IDataBound` z interfejsu i Metodazinterfejsusąimplementowanezapomocąelementówczłonkowskich.`Bind` C#obsługuje również ***jawne implementacje elementu członkowskiego interfejsu***, za pomocą którego Klasa lub struktura może uniknąć `public`tworzenia elementów członkowskich. Implementacja jawnego elementu członkowskiego interfejsu jest zapisywana przy użyciu w pełni kwalifikowanej nazwy elementu członkowskiego interfejsu. Na przykład `EditBox` Klasa może `IControl.Paint` zaimplementować metody i `IDataBound.Bind` przy użyciu jawnych implementacji elementu członkowskiego interfejsu w następujący sposób.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Dostęp do jawnych elementów członkowskich interfejsu można uzyskać tylko za pośrednictwem typu interfejsu. Na przykład implementacja `IControl.Paint` dostarczonych przez poprzednią `EditBox` klasę może być `EditBox` wywoływana tylko przez pierwsze `IControl` przekonwertowanie odwołania do typu interfejsu.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Wyliczenia

***Typ wyliczeniowy*** to odrębny typ wartości zawierający zestaw nazwanych stałych. Poniższy przykład deklaruje i używa typu wyliczeniowego `Color` o nazwie z trzema `Red`stałymi wartościami `Blue`,, `Green`i.

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
Każdy typ wyliczeniowy ma odpowiedni typ całkowity nazywany ***typem podstawowym*** typu wyliczeniowego. Typ wyliczeniowy, który nie deklaruje jawnie typu podstawowego, ma typ `int`podstawowy. Format magazynu typu wyliczeniowego i zakres możliwych wartości są określane przez jego typ podstawowy. Zbiór wartości, dla których można zastosować typ wyliczeniowy, nie jest ograniczony przez elementy członkowskie wyliczenia. W szczególności każda wartość typu podstawowego wyliczenia może być rzutowana na typ wyliczeniowy i jest unikatową prawidłową wartością tego typu wyliczeniowego.

Poniższy przykład deklaruje typ wyliczeniowy o nazwie `Alignment` z `sbyte`typem podstawowym.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Jak pokazano w poprzednim przykładzie, Deklaracja elementu członkowskiego wyliczenia może zawierać wyrażenie stałe, które określa wartość elementu członkowskiego. Wartość stała dla każdego elementu członkowskiego wyliczenia musi znajdować się w zakresie bazowego typu wyliczenia. Jeśli deklaracja elementu członkowskiego wyliczenia nie określa jawnie wartości, element członkowski otrzymuje wartość zero (jeśli jest to pierwszy element członkowski typu enum) lub wartość w postaci jednokrotnie poprzedzającej składową wyliczenia plus jeden.

Wartości wyliczeniowe mogą być konwertowane na wartości całkowite i odwrotnie przy użyciu rzutowania typu. Na przykład

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
Wartość domyślna dowolnego typu wyliczeniowego jest wartością całkowitą zero przekonwertowaną na typ wyliczeniowy. W przypadkach, gdy zmienne są automatycznie inicjowane do wartości domyślnej, jest to wartość nadana zmiennym typów wyliczeniowych. Aby wartość domyślna typu wyliczeniowego była łatwo dostępna, literał `0` niejawnie konwertowany na dowolny typ wyliczeniowy. W ten sposób można używać następujących sposobów.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Delegaty

***Typ delegata*** reprezentuje odwołania do metod z określoną listą parametrów i zwracanym typem. Delegaty umożliwiają traktowanie metod jako jednostek, które mogą być przypisane do zmiennych i przekazane jako parametry. Delegaty są podobne do koncepcji wskaźników funkcji, które znajdują się w innych językach, ale w przeciwieństwie do wskaźników funkcji Delegaty są zorientowane obiektowo i są bezpieczne dla typów.

Poniższy przykład deklaruje i używa typu delegata o `Function`nazwie.

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
Wystąpienie `Function` typu delegata może odwoływać się do dowolnej metody, która `double` przyjmuje argument i zwraca `double` wartość. Metoda odnosi `Function` się do elementów a `double[]`, zwracając `double[]` wynik. `Apply` Metoda jest używana do zastosowania `double[]`trzech różnych funkcji do. `Apply` `Main`

Delegat może odwoływać się do metody statycznej (takiej `Square` jak `Math.Sin` lub w poprzednim przykładzie) lub metody `m.Multiply` wystąpienia (na przykład w poprzednim przykładzie). Delegat, który odwołuje się do metody wystąpienia, odwołuje się również do określonego obiektu i gdy metoda wystąpienia jest wywoływana za pomocą delegata `this` , ten obiekt zostaje w wywołaniu.

Delegatów można także tworzyć za pomocą funkcji anonimowych, które są "metodami wbudowanymi", które są tworzone na bieżąco. Funkcje anonimowe mogą zobaczyć zmienne lokalne otaczających metod. W ten sposób przykład mnożnika można napisać łatwiej, bez użycia `Multiplier` klasy:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Interesująca i przydatna właściwość delegata polega na tym, że nie wie ani nie posługuje się klasą metody, do której się odwołuje; wszystkie te kwestie polegają na tym, że metoda przywoływana ma te same parametry i zwracany typ jako delegat.

## <a name="attributes"></a>Atrybuty

Typy, elementy członkowskie i inne jednostki w C# programie obsługują Modyfikatory kontrolujące pewne aspekty ich zachowania. Na przykład dostępność metody jest `public`kontrolowana za pomocą modyfikatorów, `protected`, `internal`i `private` . C#służy do uogólniania tej funkcji w taki sposób, aby typy informacji deklaratywnych zdefiniowanych przez użytkownika mogły być dołączane do jednostek programu i pobierane w czasie wykonywania. Programy określają te dodatkowe informacje deklaracyjne przez definiowanie i używanie ***atrybutów***.

Poniższy przykład deklaruje `HelpAttribute` atrybut, który może być umieszczony w jednostkach programu, aby udostępnić linki do powiązanej dokumentacji.

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
Wszystkie klasy atrybutów pochodzą z `System.Attribute` klasy bazowej dostarczonej przez .NET Framework. Atrybuty mogą być stosowane, dając ich nazwę, wraz z dowolnymi argumentami, wewnątrz nawiasów kwadratowych tuż przed skojarzoną deklaracją. Jeśli nazwa atrybutu zostanie zakończona `Attribute`, ta część nazwy może zostać pominięta, gdy występuje odwołanie do atrybutu. Na przykład `HelpAttribute` atrybut może być używany w następujący sposób.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
Ten `HelpAttribute` przykład dołącza `Widget` do klasyi`HelpAttribute` drugi do metodywklasie.`Display` Publiczne konstruktory klasy atrybutów kontrolują informacje, które muszą być dostarczone, gdy atrybut jest dołączony do jednostki programu. Dodatkowe informacje można uzyskać, odwołując się do właściwości publicznego odczytu i zapisu klasy atrybutów (takich jak odwołanie do `Topic` właściwości wcześniej).

Poniższy przykład pokazuje, jak informacje o atrybutach danej jednostki programu mogą być pobierane w czasie wykonywania przy użyciu odbicia.

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
Gdy określony atrybut jest żądany przez odbicie, Konstruktor klasy atrybutu jest wywoływany z informacjami podanymi w źródle programu i zwracane jest wystąpienie atrybutu wynikowego. Jeśli dodatkowe informacje zostały przekazane za pomocą właściwości, te właściwości są ustawiane na podane wartości przed zwróceniem wystąpienia atrybutu.
