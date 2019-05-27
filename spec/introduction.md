---
ms.openlocfilehash: 201db57d243c9d0e22553366bc653d02e183aa4b
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193873"
---
# <a name="introduction"></a>Wprowadzenie

C# (wymowa: „si szarp”) to prosty, nowoczesny, zorientowany obiektowo i bezpieczny język programowania. C# ma korzenie jego rodziny C w językach i będzie znane w programowaniu w języku C, C++ i Java. C# jest standardowym by ECMA International jako ***ECMA 334*** standardowe i ISO/IEC jako ***23270 ISO/IEC*** standardowych. Kompilator języka C# firmy Microsoft dla programu .NET Framework jest odpowiadające wykonania obu tych standardach.

C# to język zorientowany obiektowo, ale obsługuje również programowanie ***zorientowane na składniki***. Tworzone dziś oprogramowanie w coraz większym stopniu opiera się na składnikach mających formę niezależnych i samoopisujących się pakietów funkcji. Kluczem są tutaj właściwości, metody i zdarzenia dostarczanie przez składniki. Składniki mają swoje atrybuty, które zapewniają informacje deklaratywne, a także obejmują dokumentację. C# zawiera konstrukcji języka, aby bezpośrednio obsługują te pojęcia wprowadzania C# bardzo języka naturalnego, w której do tworzenia i używania składników oprogramowania.

C# ma również bardzo pomocne funkcje do tworzenia niezawodnych i trwałych aplikacji. ***Wyrzucanie elementów bezużytecznych*** automatycznie odzyskuje pamięć zajęta przez nieużywanych obiektów; ***wyjątków*** ze strukturą i rozszerzalny podejście do wykrywania błędów i odzyskiwania; i ***bezpieczny*** projekt języka umożliwia odczytywanie niezainicjowane zmienne Indeksowanie tablic poza ich zakresem, lub wykonać unchecked typu rzutowania.

C# korzysta z ***ujednoliconego systemu typów***. Wszystkie typy w tym języku, w tym typy podstawowe takie jak `int` czy `double`, dziedziczą z jednego typu głównego typu `object`. W rezultacie wszystkie typy dzielą między sobą ten sam zestaw metod, a wartości dowolnego typu można zapisywać, przesyłać i wykorzystywać w jednolity sposób. C# obsługuje też typy odwołań i wartości zdefiniowanych przez użytkownika, co pozwala na dynamiczną alokację obiektów, a także na wbudowane przechowywanie uproszczonych struktur.

Aby upewnić się, że programy C# i biblioteki mogą z czasem ewoluować w sposób zgodny, znacznie większym naciskiem został umieszczony na ***versioning*** w języku C# w projekcie. Wiele języków programowania interesuje tego problemu, a w rezultacie, programy napisane w tych języków podziału częściej niż to konieczne, w przypadku nowszych wersjach zależne biblioteki zostały wprowadzone. Aspektów projektu C# w, które zostały bezpośrednio wpływa uwagi dotyczące wersji zawierać oddzielne `virtual` i `override` modyfikatorów, reguły dla rozwiązania przeciążenia metody i pomoc techniczna dla deklaracji elementu członkowskiego interfejsu jawnego.

W pozostałej części w tym rozdziale opisano podstawowe funkcje języka C#. Mimo że dalszych rozdziałach opisano regułami i wyjątkami zorientowane na szczegółów i czasami matematyczne w sposób, w tym rozdziale dokłada starań uściślenia i zwięzłości kosztem informacje były kompletne. Celem jest zapewnienie czytnik zawiera wprowadzenie do języka, który ułatwi pisanie programów wczesne i Odczyt rozdziały nowsze.

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

Pliki źródłowe C# zazwyczaj mają rozszerzenie `.cs`. Przy założeniu, że program "Hello, World" jest przechowywany w pliku `hello.cs`, program może być kompilowane przez kompilator Microsoft C# przy użyciu wiersza polecenia
```
csc hello.cs
```
który tworzy zestaw pliku wykonywalnego o nazwie `hello.exe`. Dane wyjściowe generowane przez tę aplikację po jej uruchomieniu
```
Hello, World
```

Program „Hello, World” rozpoczyna się od dyrektywy `using`, która odwołuje się do przestrzeni nazw `System`. Przestrzenie nazw zapewniają hierarchiczny sposób organizowania programów i bibliotek C#. Zawierają one typy i inne przestrzenie nazw — np. przestrzeń nazw `System` zawiera wiele typów (takich jak klasa `Console`, do której odwołuje się program) i wiele innych przestrzeni nazw, takich jak `IO` czy `Collections`. Dyrektywa `using` odwołująca się do danej przestrzeni nazw umożliwia niekwalifikowane korzystanie z typów, które są składowymi tej przestrzeni nazw. Dzięki skorzystaniu z dyrektywy `using` program może użyć polecenia `Console.WriteLine` jako skrótu dla `System.Console.WriteLine`.

Klasa `Hello` zadeklarowa przez program „Hello, World” zawiera jedną składową, którą jest metoda o nazwie `Main`. `Main` Metody jest zadeklarowana za pomocą `static` modyfikator. Podczas gdy metody wystąpień mogą odwoływać się do konkretnego wystąpienia obiektu otaczającego przy użyciu słowa kluczowego `this`, metody statyczne działają bez odwoływania się do konkretnego obiektu. Zgodnie z konwencją, metoda statyczna o nazwie `Main` służy jako punkt wejścia programu.

Dane wyjściowe programu są generowane przez metodę `WriteLine` klasy `Console` w przestrzeni nazw `System`. Ta klasa jest zapewniana przez biblioteki klas .NET Framework, które domyślnie są automatycznie dołączane do kompilatora Microsoft C#. Należy pamiętać, że C# sam nie ma biblioteki oddzielne środowiska uruchomieniowego. Zamiast tego program .NET Framework jest biblioteka środowiska uruchomieniowego języka C#.

## <a name="program-structure"></a>Struktura programu

Kluczowe założenia organizacji w języku C# są ***programy***, ***przestrzenie nazw***, ***typy***, ***członków***, i ***zestawy***. C# programy składają się z jednego lub więcej plików źródłowych. Programy deklarują typy, które zawierają elementy członkowskie i mogą być organizowane w przestrzeni nazw. Klasy i interfejsy są przykłady typów. Pola, metody, właściwości i zdarzenia są przykłady elementów członkowskich. Po skompilowaniu C# programy są fizycznie spakowane do zestawów. Zestawy zwykle z rozszerzeniem pliku `.exe` lub `.dll`, w zależności od tego, czy zaimplementować ***aplikacje*** lub ***biblioteki***.

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
deklaruje klasę o nazwie `Stack` w przestrzeni nazw o nazwie `Acme.Collections`. W pełni kwalifikowana nazwa tej klasy to `Acme.Collections.Stack`. Klasa zawiera kilka elementów członkowskich: pole o nazwie `top`, dwie metody o nazwie `Push` i `Pop`i klasę zagnieżdżoną o nazwie `Entry`. `Entry` Dodatkowo klasa zawiera trzy elementy członkowskie: pole o nazwie `next`, pole o nazwie `data`i konstruktora. Przy założeniu, że kod źródłowy przykładu znajduje się w pliku `acme.cs`, wiersza polecenia

```
csc /t:library acme.cs
```
kompiluje przykład jako biblioteki (kod bez `Main` punktu wejścia) i tworzy zestaw o nazwie `acme.dll`.

Zestawy zawierają kod wykonywalny w formie ***języka pośredniego*** instrukcje (IL), a także informacji o symbolach w formie ***metadanych***. Przed wykonaniem jego kodu IL w zestawie jest automatycznie konwertowany na kod specyficzny dla procesora przez kompilator just in Time (JIT) środowiska uruchomieniowego języka wspólnego platformy .NET.

Ponieważ zestaw jest samoopisujący jednostka zawierająca kod i metadanych funkcji, nie ma potrzeby dla `#include` dyrektyw i pliki nagłówkowe w języku C#. Typy publiczne i elementów członkowskich znajdujących się w określonym zestawie są udostępniane w programie C# poprzez odwołanie do tego zestawu podczas kompilowania kodu programu. Na przykład ten program używa `Acme.Collections.Stack` klasy z `acme.dll` zestawu:

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
Jeśli program jest przechowywany w pliku `test.cs`, gdy `test.cs` jest kompilowany, `acme.dll` zestawu można odwoływać się za pomocą kompilatora `/r` opcji:

```
csc /r:acme.dll test.cs
```
Spowoduje to utworzenie pliku wykonywalnego zestaw o nazwie `test.exe`, który, po uruchomieniu tworzy dane wyjściowe:

```
100
10
1
```
C# umożliwia tekst źródłowy program ma być przechowywany w kilku plików źródłowych. Wielu plików języka C# program jest skompilowany, wszystkie pliki źródłowe są przetwarzane razem, gdy pliki źródłowe mogą swobodnie odwoływać się do siebie nawzajem — model jest tak, jakby wszystkie pliki źródłowe zostały połączone w jeden duży plik przed przetworzeniem. Deklaracje przechodzenia do przodu nigdy nie są wymagane w języku C#, ponieważ z nielicznymi wyjątkami, kolejności deklaracji jest niewielka. C# nie istnieje limit pliku źródłowego do deklarowania tylko jeden typ publiczny ani nie wymaga nazwy pliku źródłowego, aby dopasować typ zadeklarowany w pliku źródłowym.

## <a name="types-and-variables"></a>Typy i zmienne

Istnieją dwa rodzaje typów w języku C#: ***typy wartości*** i ***typy odwołań***. Zmienne typu wartości zawierają bezpośrednio swoje dane, natomiast zmiennych typu referencyjnego są przechowywane odwołania do swoich danych, te ostatnie są nazywane obiektów. W przypadku typów referencyjnych jest możliwe w dwóch zmiennych odwoływać się do tego samego obiektu i dlatego możliwe dla operacji na jednej zmiennej miały wpływ na obiekt odwołuje się druga zmienna. Z typami wartości zmiennych każda ma własne kopię danych i nie jest możliwe dla operacji na jednym wpłynie na inne (z wyjątkiem w przypadku właściwości `ref` i `out` zmiennych parametrów).

Podzielono na typach wartości języka C# w ***typów prostych***, ***typach wyliczeniowych***, ***typy struktury***, i ***typów dopuszczających wartości zerowe***, a przez odwołanie w C# typy są podzielone na ***klasy typów***, ***typy interfejsów***, ***tablicy typów***, i ***typy delegatów***.

Poniższa tabela zawiera omówienie w systemie typu C#.

| __Kategoria__    |                 | __Opis__ |
|-----------------|-----------------|-----------------|
| Typy wartości     | Typy proste    | Podpisana całkowitego: `sbyte`, `short`, `int`, `long` |
|                 |                 | Całkowite bez znaku: `byte`, `ushort`, `uint`, `ulong` |
|                 |                 | Znaki Unicode: `char` |
|                 |                 | Liczba zmiennoprzecinkowa IEEE: `float`, `double` |
|                 |                 | Decimal wysokiej precyzji: `decimal` |
|                 |                 | Atrybut typu wartość logiczna: `bool` |
|                 | Typach wyliczeniowych      | Typy zdefiniowane przez użytkownika w postaci `enum E {...}` |
|                 | Typy — struktura    | Typy zdefiniowane przez użytkownika w postaci `struct S {...}` |
|                 | Typy dopuszczające wartości null  | Rozszerzenia z innych typów wartości za pomocą `null` wartość |
| Typy odwołań | Typy klas     | Ultimate klasa bazowa innych typów: `object` |
|                 |                 | Ciągi Unicode: `string` |
|                 |                 | Typy zdefiniowane przez użytkownika w postaci `class C {...}` |
|                 | Typy interfejsów | Typy zdefiniowane przez użytkownika w postaci `interface I {...}` |
|                 | Typy tablic     | Jedno - i są one wielowymiarowe, na przykład `int[]` i `int[,]` |
|                 | Typy delegatów  | Typy zdefiniowane przez użytkownika w postaci np. `delegate int  D(...)` |

Osiem typów całkowitych zapewniają obsługę wartości 8-bitową, 16-bitowych, 32-bitowych i 64-bitowe podpisane lub niepodpisane formularza.

Polecenie dwóch liczb zmiennoprzecinkowych typów, `float` i `double`, są reprezentowane przy użyciu 32-bitowe o pojedynczej dokładności i 64-bitowe o podwójnej dokładności IEEE 754 formatów.

`decimal` Typ to typ danych 128-bitowych, odpowiedni do obliczeń finansowych i walutowych.

C# w `bool` typ jest używany do reprezentowania wartości logicznych — wartości, które są albo `true` lub `false`.

Znakowe i przetwarzania w języku C# przy użyciu kodowania Unicode. `char` Typ reprezentuje jednostkę kodu UTF-16 i `string` typu reprezentuje sekwencję jednostki kodu UTF-16.

W poniższej tabeli przedstawiono typy liczbowe języka C# firmy.


| __Kategoria__      | __Usługa BITS__ | __Typ__  | __Zakres/dokładności__ |
|-------------------|----------|-----------|---------------------|
| Całkowite podpisem   | 8        | `sbyte`   | -128...127 |
|                   | 16       | `short`   | -32,768...32,767 |
|                   | 32       | `int`     | -2,147,483,648...2,147,483,647 |
|                   | 64       | `long`    | -9,223,372,036,854,775,808...9,223,372,036,854,775,807 |
| Całkowite bez znaku | 8        | `byte`    | 0...255 |
|                   | 16       | `ushort`  | 0...65,535 |
|                   | 32       | `uint`    | 0...4,294,967,295 |
|                   | 64       | `ulong`   | 0...18,446,744,073,709,551,615 |
| Liczba zmiennoprzecinkowa    | 32       | `float`   | 1,5 x 10 ^ −45 do 3,4 x 10 ^ 38, dokładności 7 cyfr |
|                   | 64       | `double`  | W wersji 5.0 x 10 ^ −324 do wersji 1.7 x 10 ^ 308, dokładności 15 cyfr |
| Wartość dziesiętna           | 128      | `decimal` | 1.0 x 10 ^ −28 do 7,9 x 10 ^ 28, 28 cyfr precyzji |

C# programy użyj ***wpisz deklaracje*** do tworzenia nowych typów. Deklaracja typu Określa nazwę i elementy członkowskie nowego typu. Pięć języka C# w kategorie typów są definiowane przez użytkownika: klasy, typy, typy struktury, typy interfejsów, typach wyliczeniowych i typy delegatów.

Typ klasy definiuje strukturę danych, który zawiera elementy członkowskie danych (pola) i składowe funkcji (metody, właściwości i inne). Typy klas obsługuje pojedyncze dziedziczenie i polimorfizmu, mechanizmów, według której rozszerzać i specialize klas bazowych klas pochodnych.

Typ struktury jest podobny do typu klasy, w tym, że reprezentuje strukturę z elementów członkowskich danych i składowe funkcji. W przeciwieństwie do klasy, struktury są typami wartości i nie wymagają alokacji sterty. Typy struktury nie obsługują dziedziczenia określonych przez użytkownika, a wszystkie typy struktury niejawnie dziedziczą z typu `object`.

Typ interfejsu definiuje kontrakt jako nazwany zestaw funkcji publicznych elementów członkowskich. Klasa lub struktura, która implementuje interfejs należy podać implementacji interfejsu funkcji członków. Interfejs może dziedziczyć z wielu interfejsach podstawowych, a klasa lub struktura może zaimplementować wiele interfejsów.

To typ delegowany reprezentuje odwołania do metod z określoną listą parametrów i typ zwracany. Delegatów można umożliwić traktować metod jako jednostki, które mogą być przypisane do zmiennych i przekazywane jako parametry. Delegaty są podobne do koncepcji wskaźników funkcji, w przeciwieństwie do innych języków, ale w przeciwieństwie do wskaźników funkcji, obiekty delegowane są zorientowane obiektowo i bezpieczny typowo.

Klasy, struktury, interfejsów i delegatów typy wszystkie elementy rodzajowe pomocy technicznej, według których mogą być parametryzowane przy użyciu innych typów.

Typ wyliczeniowy jest typem samodzielnym z nazwanych stałych. Każdy typ wyliczenia ma typu podstawowego musi być jednym z ośmiu typów całkowitych. Zbiór wartości Typ wyliczeniowy jest taka sama jak zestaw wartości typu podstawowego.

C# obsługuje dimensional jednym i wielu tablic dowolnego typu. W przeciwieństwie do typów wymienionych powyżej typy tablic ma zadeklarowany, zanim będzie można ich użyć. Zamiast tego typy tablicowe są konstruowane wykonując nazwę typu z nawiasami kwadratowymi. Na przykład `int[]` to Jednowymiarowa tablica `int`, `int[,]` to dwuwymiarowa tablica `int`, i `int[][]` to Jednowymiarowa tablica tablice jednowymiarowe `int`.

Typy dopuszczające wartości zerowe również nie trzeba być zadeklarowany, zanim będzie można ich użyć. Dla każdego typu wartości niedopuszczającym wartości `T` Brak odpowiedniego typu dopuszczającego wartość null `T?`, który może zawierać dodatkowe wartości `null`. Na przykład `int?` to typ, który może zawierać żadnych 32-bitowej liczby całkowitej lub wartość `null`.

W systemie typu C# jest jednolita w taki sposób, że wartość dowolnego typu może być traktowana jako obiekt. Każdy typ w języku C#, bezpośrednio lub pośrednio pochodzi z `object` typ, klasy i `object` jest ultimate klasą bazową wszystkich typów. Wartości typu referencyjnego są traktowane jako obiekty poprzez wyświetlanie wartości jako typu `object`. Wartości typu wartości są traktowane jako obiekty, wykonując ***pakowania*** i ***Rozpakowywanie*** operacji. W poniższym przykładzie `int` wartość jest konwertowana na `object` i wykonać ich kopię ponownie do `int`.

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
Gdy wartość typu wartości jest konwertowany na typ `object`, wystąpienie obiektu, nazywany również "pola", jest przeznaczona do przechowywania wartości i kopiuje wartość do tego pola. Z drugiej strony, gdy `object` odwołanie jest rzutowany na typ wartości, dokonuje, przywoływanego obiektu jest polem typu poprawnej wartości, i, jeśli sprawdzenie zakończy się powodzeniem, wartość w polu jest kopiowana.

Firmy ujednoliconego systemie typu C# oznacza, że typy wartości może stać się obiekty "na żądanie." Ze względu na ujednolicenie ogólnego przeznaczenia biblioteki, które używają typu `object` mogą być używane z typami odwołań i typy wartości.

Istnieje kilka rodzajów z ***zmienne*** w języku C#, w tym pól, elementy tablicy, zmienne lokalne i parametry. Zmienne reprezentują lokalizacje przechowywania, a co zmienna ma typ, który określa, jakie wartości mogą być przechowywane w zmiennej, jak pokazano w poniższej tabeli.


| __Typ zmiennej__    | __Zawartość możliwa__ |
|-------------------------|-----------------------|
| Typ wartości nieprzyjmujące | Wartość tego typu dokładnie |
| Typ wartości null     | Wartość null lub wartość tego typu dokładnie |
| `object`                | Odwołanie o wartości null, odwołanie do obiektu typu odwołania lub odwołania do wartości spakowanej dowolnego typu wartości |
| Typ klasy              | Odwołanie o wartości null, odwołanie do wystąpienia tego typu klasy lub odwołanie do wystąpienia klasy pochodnej z typu klasy |
| Typ interfejsu          | Odwołanie o wartości null, odwołanie do typu klasy, która implementuje interfejs typu wystąpienia lub odwołania do wartości spakowanej typu wartości, która implementuje interfejs typu |
| Typ tablicy              | Odwołanie o wartości null, odwołanie do wystąpienia tego typu tablicy lub odwołanie do wystąpienia typu tablicy zgodne |
| Typ delegata           | Odwołanie o wartości null lub odwołanie do wystąpienia tego typu delegata |

## <a name="expressions"></a>Wyrażenia

***Wyrażenia*** są konstruowane na podstawie ***operandów*** i ***operatorów***. Operatory wyrażenia wskazują operacje do zastosowania dla operandów. Przykłady operatorów to `+`, `-`, `*`, `/`i `new`. Przykładami operandów są literały, pola, zmienne lokalne i wyrażenia.

Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególne operatory. Na przykład, wyrażenie `x + y * z` jest oceniane jako `x + (y * z)`, ponieważ operator `*` ma wyższy priorytet niż operator `+`.

Większość operatorów może być ***przeciążona***. Przeciążanie operatora umożliwia określanie definiowanych przez użytkownika implementacji operatorów dla operacji, w których jeden lub oba operandy mają typ struktury lub klasy zdefiniowanej przez użytkownika.

Poniższa tabela zawiera podsumowanie operatory C# firmy, lista kategorii operatora w kolejność pierwszeństwa od najwyższego do najniższego. Operatory w tej samej kategorii mają takie samo pierwszeństwo.


| __Kategoria__                     | __Expression__    | __Opis__ |
|----------------------------------|-------------------|-----------------|
| Podstawowy                          | `x.m`             | Dostęp do elementu członkowskiego |
|                                  | `x(...)`          | Wywołanie metody i delegata |
|                                  | `x[...]`          | Dostęp do tablicy i indeksatora |
|                                  | `x++`             | Postinkrementacja |
|                                  | `x--`             | Postdekrementacja |
|                                  | `new T(...)`      | Utworzenie obiektu i delegata |
|                                  | `new T(...){...}` | Utworzenie obiektu za pomocą inicjatora |
|                                  | `new {...}`       | Inicjator obiektu anonimowego |
|                                  | `new T[...]`      | Do utworzenia tablicy |
|                                  | `typeof(T)`       | Uzyskaj `System.Type` dla obiektu `T` |
|                                  | `checked(x)`      | Obliczenie wyrażenia w kontekście sprawdzanym |
|                                  | `unchecked(x)`    | Obliczenie wyrażenia w kontekście niesprawdzanym |
|                                  | `default(T)`      | Uzyskanie wartości domyślnej typu `T` |
|                                  | `delegate {...}`  | Funkcja anonimowa (metoda anonimowa) |
| Jednoargumentowy                            | `+x`              | Tożsamość |
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
| Dodatku                         | `x + y`           | Dodawanie, łączenie ciągów, łączenie delegatów |
|                                  | `x - y`           | Odejmowanie, usuwanie delegata |
| Shift                            | `x << y`          | Przesunięcie w lewo |
|                                  | `x >> y`          | Przesunięcie w prawo |
| Relacyjne i badania typu      | `x < y`           | Mniejsze niż |
|                                  | `x > y`           | Większe niż |
|                                  | `x <= y`          | Mniejsze niż lub równe |
|                                  | `x >= y`          | Większe niż lub równe |
|                                  | `x is T`          | Zwróć `true` Jeśli `x` jest `T`, `false` inaczej |
|                                  | `x as T`          | Zwróć `x` wpisanych w formie `T`, lub `null` Jeśli `x` nie jest `T` |
| Równości                         | `x == y`          | Równa się      |
|                                  | `x != y`          | Nie równa się |
| Logicznego AND                      | `x & y`           | Liczba całkowita bitowe i logicznych logiczne AND |
| Logicznego XOR                      | `x ^ y`           | Bitowe XOR dla wartości całkowitych, logiczne XOR dla wartości binarnych |
| Logicznego OR                       | <code>x &#124; y</code> | Bitowe OR dla wartości całkowitych, logiczne OR dla wartości binarnych |
| Warunkowego AND                  | `x && y`          | Ocenia `y` tylko wtedy, gdy `x` jest `true` |
| Warunkowego OR                   | <code>x &#124;&#124; y</code> | Ocenia `y` tylko wtedy, gdy `x` jest `false` |
| Łączenia wartości null                  | `x ?? y`          | Daje w wyniku `y` Jeśli `x` jest `null`, `x` inaczej |
| Warunkowe                      | `x ? y : z`       | Ocenia `y` Jeśli `x` jest `true`, `z` Jeśli `x` jest `false` |
| Przypisania lub funkcji anonimowej | `x = y`           | Przypisanie |
|                                  | `x op= y`         | Przydział złożony; obsługiwane operatory to `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | Funkcja anonimowa (wyrażenie lambda) |

## <a name="statements"></a>Instrukcje

Akcje programu są wyrażone za pomocą ***instrukcji***. C# obsługuje wiele rodzajów instrukcji, z których są definiowane zgodnie z osadzonych instrukcji.

A ***bloku*** zezwala na wiele instrukcji, które ma zostać zapisany w kontekstach, których jest dozwolone pojedynczej instrukcji. Blok zawiera listę instrukcji, zapisywane między ogranicznikami `{` i `}`.

***Instrukcje deklaracji*** są używane do deklarowania stałe i zmienne lokalne.

***Instrukcje wyrażeń*** są używane do oceny wyrażenia. Wyrażenia, które mogą służyć jako stwierdzenia obejmują wywołań metod obiektu alokacje za pomocą `new` operator, za pomocą przypisań `=` i operatorów przypisania złożonego, operacje inkrementacji i dekrementacji przy użyciu `++`i `--` operatory i wyrażenia await.

***Instrukcje wyboru*** są używane, aby wybrać jeden z wielu możliwych instrukcji do wykonania na podstawie wartości w wyrażeniu. W tej grupie są `if` i `switch` instrukcji.

***Instrukcje iteracji*** są używane wielokrotnie wykonać osadzona instrukcja. W tej grupie są `while`, `do`, `for`, i `foreach` instrukcji.

***Instrukcje skoku*** służą do przekazywania kontroli. W tej grupie są `break`, `continue`, `goto`, `throw`, `return`, i `yield` instrukcji.

`try`... `catch` instrukcja jest używane do przechwytywania wyjątków, które występują podczas wykonywania blok, a `try`... `finally` instrukcja jest używane do określenia kod finalizacji, który jest zawsze wykonywana, czy wystąpił wyjątek, czy nie.

`checked` i `unchecked` instrukcje są używane do kontrolowania przepełnienia, sprawdzając kontekst dla typu całkowitego operacje arytmetyczne i konwersje.

`lock` Instrukcja jest używane do uzyskania blokadę wykluczania wzajemnego dla danego obiektu, wykonania instrukcji i następnie zwalnia blokadę.

`using` Instrukcja jest używane do uzyskania zasobu, należy wykonać instrukcję, a następnie usunąć tego zasobu.

Poniżej przedstawiono przykłady każdego rodzaju — instrukcja

__Deklaracje zmiennych lokalnych__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Deklaracji stałej lokalnej__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__Instrukcja wyrażeń__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` Instrukcja__

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


__`switch` Instrukcja__

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

__`while` Instrukcja__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` Instrukcja__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` Instrukcja__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` Instrukcja__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` Instrukcja__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` Instrukcja__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` Instrukcja__

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

__`return` Instrukcja__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` Instrukcja__

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

__`throw` i `try` instrukcji__

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

__`checked` i `unchecked` instrukcji__

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

__`lock` Instrukcja__

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

__`using` Instrukcja__

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

***Klasy*** są najbardziej podstawowym typem w języku C#. Klasa jest strukturą danych, która łączy stanu (pola) i akcje (metody i innych funkcji elementów członkowskich) w pojedynczą jednostkę. Klasa zawiera definicję dla tworzone dynamicznie ***wystąpień*** klasy, nazywana również ***obiektów***. Klasy obsługi ***dziedziczenia*** i ***polimorfizm***, mechanizmów zgodnie z którą ***klasy pochodne*** pozwalają rozszerzyć i specjalizują się ***klasbazowych***.

Nowe klasy są tworzone za pomocą deklaracji klasy. Deklaracja klasy rozpoczyna się od nagłówka, określający, atrybuty i modyfikatorów klasy, nazwa klasy, klasy podstawowej (jeśli) i interfejsy implementowane przez klasy. Nagłówek następuje treści klasy, która składa się z listy deklaracji elementu członkowskiego zapisywane między ogranicznikami `{` i `}`.

Poniżej przedstawiono deklarację klasie proste o nazwie `Point`:

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
Wystąpienia klas są tworzone przy użyciu `new` operatora, który przydziela pamięć dla nowego wystąpienia, wywołuje konstruktor do inicjowania wystąpienia i zwraca odwołanie do wystąpienia. Poniższe instrukcje utworzysz dwa `Point` obiekty i przechowuje odwołania do tych obiektów w dwóch zmiennych:

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
Pamięć zajęta przez obiekt jest automatycznie odzyskana, gdy obiekt nie jest już używana. Go nie ma potrzeby ani można jawnie cofnięcie przydziału obiektów w języku C#.

### <a name="members"></a>Elementy członkowskie

Elementy członkowskie klasy są albo ***statyczne elementy członkowskie*** lub ***wystąpieniami elementów członkowskich***. Statyczne elementy członkowskie należą do klas i składowych wystąpienia należą do obiektów (wystąpienia klasy).

Poniższa tabela zawiera omówienie rodzajów elementów członkowskich, który może zawierać klasę.


| __Element członkowski__   | __Opis__ |
|------------  |-----------------|
| Stałe    | Skojarzony z klasą wartości stałych |
| Pola       | Zmienne klasy |
| Metody      | Obliczeń i akcji, które mogą być wykonywane przez klasę |
| Właściwości   | Akcje skojarzone z odczytywaniem i zapisywaniem nazwane właściwości klasy |
| Indeksatory     | Akcje skojarzone z indeksowania wystąpienia klasy, jak tablica |
| Zdarzenia       | Powiadomienia, które mogą być generowane przez klasę |
| Operatory    | Konwersje i operatory wyrażeń obsługiwany przez klasę |
| Konstruktorów | Działania wymagane w celu zainicjowania wystąpienia klasy lub samej klasy |
| Destruktory  | Akcje do wykonania przed wystąpienia klasy stałe są odrzucane. |
| Types        | Zagnieżdżone typy zadeklarowane w klasie |

### <a name="accessibility"></a>Ułatwienia dostępu

Każdy członek klasy ma skojarzone ułatwień dostępu, który kontroluje regionach tekst programu, które mogą uzyskiwać dostęp do elementu członkowskiego do. Istnieje pięć możliwe formy ułatwień dostępu. Ich podsumowanie można znaleźć w poniższej tabeli.


| __Ułatwienia dostępu__    | __Znaczenie__ |
|----------------------|-----------------|
| `public`             | Nie ograniczając dostęp |
| `protected`          | Dostęp ograniczony do tej klasy lub klas pochodzących z tej klasy |
| `internal`           | Dostęp ograniczony do tego programu |
| `protected internal` | Dostęp ograniczony do tego programu lub klasy pochodne względem tej klasy |
| `private`            | Dostęp ograniczony do klasy |

### <a name="type-parameters"></a>Parametry typu

Definicja klasy mogą określać zestaw parametrów typu wykonując nazwę klasy za pomocą nawias ostry otaczający listę nazwy parametrów typu. Parametry typu mogą być używane w treści deklaracji klasy do definiowania elementów członkowskich klasy. W poniższym przykładzie parametry typu `Pair` są `TFirst` i `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Typ klasy, który jest zadeklarowany przyjmują parametry typu nosi nazwę typu klasy ogólnej. Ogólny może być również typy struktury, interfejsów i delegatów.

W przypadku klasy ogólnej argumentów typu należy określić dla każdego z parametrów typu:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Typ ogólny z argumentami typu pod warunkiem, takie jak `Pair<int,string>` powyżej, jest nazywany skonstruowanego typu.

### <a name="base-classes"></a>Klas podstawowych

Deklaracji klasy może określać klasy bazowej, postępując zgodnie z parametrów nazwy i typu klasy, dwukropek i nazwą klasy bazowej. Pominięcie specyfikacji klasy bazowej jest taka sama jak pochodząca z typu `object`. W poniższym przykładzie klasa bazowa `Point3D` jest `Point`i klasę bazową `Point` jest `object`:

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
Klasa dziedziczy członków klasy podstawowej. Dziedziczenie oznacza, że klasa niejawnie zawiera wszystkie elementy członkowskie klasy podstawowej, z wyjątkiem wystąpienia i statyczne konstruktory i destruktory klasy bazowej. Klasy pochodne mogą dodawać nowych członków do tych, które dziedziczy, ale go nie można usunąć definicji dziedziczonego członka. W poprzednim przykładzie `Point3D` dziedziczy `x` i `y` pola z `Point`, a następnie co `Point3D` wystąpienie zawiera trzy pola `x`, `y`, i `z`.

Istnieje niejawna konwersja z typu klasy dowolny z jej typów klasy bazowej. W związku z tym zmienna typu klasy mogą odwoływać się do wystąpienia tej klasy lub wystąpienia klasy pochodnej. Na przykład, biorąc pod uwagę poprzedniej deklaracji klasy, zmienna typu `Point` może odwoływać się albo `Point` lub `Point3D`:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Pola

Pole jest zmienną, która jest skojarzona z klasą lub przy użyciu wystąpienia klasy.

Pole jest zadeklarowane za pomocą `static` definiuje modyfikator ***pole statyczne***. Pole statyczne identyfikuje dokładnie jednej lokalizacji magazynu. Niezależnie od tego, jak wiele wystąpień klasy są tworzone istnieje tylko kiedykolwiek jedną kopię pole statyczne.

Pole zadeklarowana bez `static` definiuje modyfikator ***pola wystąpienia***. Każde wystąpienie klasy zawiera osobną kopię wszystkie pola wystąpienia tej klasy.

W poniższym przykładzie, każde wystąpienie `Color` klasa ma osobną kopię `r`, `g`, i `b` wystąpienia pól, ale istnieje tylko jedna kopia `Black`, `White`, `Red`, `Green`, i `Blue` pola statyczne:

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
Jak pokazano w poprzednim przykładzie ***pola tylko do odczytu*** może być zadeklarowana z `readonly` modyfikator. Przypisanie do `readonly` pola mogą występować wyłącznie jako część deklaracji pola lub za pomocą konstruktora w tej samej klasy.

### <a name="methods"></a>Metody

A ***metoda*** jest elementem członkowskim, który implementuje obliczenia lub akcji, które mogą być wykonywane przez obiekt lub klasa. ***Metody statyczne*** są dostępne za pośrednictwem klasy. ***Wystąpienie metody*** są dostępne za pośrednictwem wystąpienia klasy.

Metody mają (prawdopodobnie pusta) lista ***parametry***, zawierające wartości i odwołań do zmiennych przekazywany do metody i ***zwracany typ***, określający typ wartości, obliczana i zwrócony przez Metoda. Typ zwracany metody jest `void` Jeśli nie zwraca wartości.

Podobnie jak typy metody mogą również mieć zestaw parametrów typu, dla których należy określić argumenty typu, gdy wywoływana jest metoda. W przeciwieństwie do typów argumentów typu często można wywnioskować z argumentów wywołania metody i nie jest jawnie konieczne.

***Podpisu*** metody muszą być unikatowe w klasie, w którym zadeklarowany jest metoda. Podpis metody zawiera nazwę metody, liczba parametrów typu i liczby, Modyfikatory i typy parametrów. Podpis metody nie ma typ zwracany.

#### <a name="parameters"></a>Parametry

Parametry są używane do przekazywania wartości lub odwołania zmiennej do metody. Parametry metody uzyskać wartości rzeczywiste z ***argumenty*** które są określone, gdy metoda jest wywoływana. Istnieją cztery rodzaje parametrów: wartości parametrów, parametrów w formie odwołań, parametry wyjściowe i tablice parametrów.

A ***wartość parametru*** służy do przekazywania parametru wejściowego. Wartość parametru odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z argumentem, która została przekazana dla parametru. Modyfikacje parametr wartości nie wpływają na argumentu, która została przekazana dla parametru.

Wartości parametrów można opcjonalnie, określając wartość domyślną, dzięki czemu można pominąć odpowiednie argumenty.

A ***odwołać się do parametru*** jest używany zarówno dla danych wejściowych i przekazywania parametru wyjściowego. Argument przekazana dla parametru odwołania musi być zmienną, a podczas wykonywania metody, parametr odniesienia reprezentuje tę samą lokalizację magazynu zmiennej argumentu. Parametr przekazany przez odwołanie jest zadeklarowana za pomocą `ref` modyfikator. Poniższy przykład pokazuje użycie `ref` parametrów.

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
***Parametr wyjściowy*** służy do parametru wyjściowego przekazywania. Parametr wyjściowy jest podobny do parametr przekazany przez odwołanie, z tą różnicą, że początkowa wartość argumentu dostarczane przez obiekt wywołujący jest "nieważne". Parametr wyjściowy jest zadeklarowana za pomocą `out` modyfikator. Poniższy przykład pokazuje użycie `out` parametrów.

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
A ***tablicy parametrów*** zezwala na zmienną liczbę argumentów, które zostaną przekazane do metody. Tablica parametrów jest zadeklarowana za pomocą `params` modyfikator. Ostatni parametr metody może być tablicą parametrów i typ tablicy parametrów musi być typem tablicy jednowymiarowej. `Write` i `WriteLine` metody `System.Console` klasy są dobrym przykładem użycia tablicy parametrów. Są deklarowane w następujący sposób.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
W metodzie, która korzysta z tablicy parametrów Tablica parametrów zachowuje się tak samo jak parametru regularnego typu tablicowego. Jednak w wywołaniu metody z tablicą parametrów, istnieje możliwość przekazania pojedynczy argument typu tablicy parametrów albo dowolnej liczby argumentów typu elementu tablicy parametrów. W tym ostatnim przypadku wystąpienie tablicy jest automatycznie tworzone i inicjowane z danego argumentów. W tym przykładzie

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
jest odpowiednikiem pisania poniżej.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Treść metody i zmienne lokalne

Treść metody określa instrukcji do wykonania, gdy metoda jest wywoływana.

Treść metody można zadeklarować zmienne, które są specyficzne dla wywołania metody. Tych zmiennych są nazywane ***zmienne lokalne***. Deklaracji zmiennej lokalnej Określa nazwę typu, nazwa zmiennej i prawdopodobnie wartość początkową. Poniższy przykład deklaruje zmienną lokalną `i` o wartości początkowej zero i zmienna lokalna `j` bez wartości początkowej.

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
Język C# wymaga lokalnej zmiennej jako ***zdecydowanie przypisywany*** przed jej wartość można uzyskać. Na przykład jeśli deklaracja poprzedniego `i` nie zawiera wartości początkowej, kompilator może zgłosić błąd do późniejszego użycia `i` ponieważ `i` nie może zostać zdecydowanie przypisany w tych punktach w programie.

Można użyć metody `return` instrukcje, aby zwrócić sterowanie do obiektu wywołującego. W metodzie, zwracając `void`, `return` instrukcji nie można określić wyrażenie. W metodzie, zwracając non -`void`, `return` instrukcje mogą zawierać wyrażenie, które oblicza wartość zwracaną.

#### <a name="static-and-instance-methods"></a>Metody statyczne i wystąpienia

Metoda zadeklarowana za pomocą `static` modyfikator jest ***statycznej metody***. Metoda statyczna nie będzie działać na określonym wystąpieniu i można uzyskać tylko bezpośredni dostęp do statycznych elementów członkowskich.

Metoda zadeklarowana bez `static` modyfikator jest ***metodę wystąpienia***. Metoda wystąpienia działa na określonym wystąpieniu i można dostęp do statycznych i wystąpieniami elementów członkowskich. Wystąpienia, na której wywołano metodę wystąpienia, które może być jawnie dostępna jako `this`. Jest to błąd do odwoływania się do `this` w metodzie statycznej.

Następujące `Entity` klasa ma statycznych i elementów członkowskich wystąpienia.

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
Każdy `Entity` wystąpienie zawiera numer seryjny (i prawdopodobnie niektóre inne informacje, które nie został tutaj pokazany). `Entity` Konstruktora (co przypomina metodą wystąpienia) inicjuje nowe wystąpienie następny dostępny numer seryjny. Ponieważ Konstruktor nie jest składową wystąpienia, jest dozwolony dostęp zarówno do `serialNo` pola wystąpienia i `nextSerialNo` pole statyczne.

`GetNextSerialNo` i `SetNextSerialNo` mogą uzyskiwać dostęp do metod statycznych `nextSerialNo` pole statyczne, ale będzie błąd dla nich bezpośredni dostęp `serialNo` pole wystąpienia.

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
Należy pamiętać, że `SetNextSerialNo` i `GetNextSerialNo` są wywoływane metody statyczne klasy, natomiast `GetSerialNo` metoda wystąpienia jest wywoływane na wystąpienia klasy.

#### <a name="virtual-override-and-abstract-methods"></a>Wirtualne zastąpienia i metody abstrakcyjne

Po deklaracji metody wystąpienia obejmuje `virtual` modyfikator, metoda jest nazywany ***metodę wirtualną***. Gdy nie `virtual` modyfikator, metoda jest nazywany ***metody niewirtualnej***.

Po wywołaniu metody wirtualnej ***typu run-time*** wystąpienia, dla którego tego wywołania ma miejsce określa rzeczywista implementacja metody do wywołania. W wywołaniu metody niewirtualnej ***typów w czasie kompilacji*** wystąpienia jest czynnikiem decydującym.

Metoda wirtualna może być ***zastąpione*** w klasie pochodnej. Po deklaracji metody wystąpienia obejmuje `override` modyfikator, metoda zastępuje dziedziczoną metodę wirtualną przy użyciu tego samego podpisu. Dlatego deklaracja metody wirtualnej wprowadzono nowe metody, deklaracji metody zastąpienie specjalizuje się istniejących dziedziczoną metodę wirtualną, podając nową metodę implementacji tej metody.

***Abstrakcyjne*** metoda jest metodą wirtualną bez wdrażania. Metoda abstrakcyjna jest zadeklarowana za pomocą `abstract` modyfikator i jest dozwolona tylko w klasie, który także jest zadeklarowany `abstract`. Metoda abstrakcyjna musi zostać zastąpiona w każdym nieabstrakcyjnej klasie pochodnej.

Poniższy przykład deklaruje klasę abstrakcyjną, `Expression`, który reprezentuje węzeł drzewa wyrażeń i trzy klasy, pochodne `Constant`, `VariableReference`, i `Operation`, który implementuje węzły drzewa wyrażeń stałych zmiennych odwołania, a operacje arytmetyczne. (Jest to podobne do, ale nie należy mylić z typami drzewo wyrażenia, wprowadzona w [typy drzewa wyrażenie](types.md#expression-tree-types)).

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
Poprzednie cztery klasy może służyć do modelowania wyrażeniach arytmetycznych. Na przykład za pomocą wystąpień tych klas, wyrażenie `x + 3` mogą być reprezentowane w następujący sposób.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
`Evaluate` Metody `Expression` wystąpienia jest wywoływane w celu oceny danemu wyrażeniu i generować `double` wartość. Ta metoda przyjmuje jako argument `Hashtable` zawiera nazwy zmiennych (jako klucze, wpisy) i wartości (jako wartości wpisów). `Evaluate` Metoda jest abstrakcyjna metody wirtualnej, co oznacza, że nieabstrakcyjnej klasy pochodne muszą przesłaniać z jego Podaj rzeczywistą implementację.

A `Constant`przez implementację `Evaluate` po prostu zwraca przechowywaną stała. A `VariableReference`jego implementacja wyszukuje nazwę zmiennej w tablicy skrótów i zwraca wartość wynikową. `Operation`w implementacji najpierw ocenia operandy lewy i prawy (cyklicznie, wywołanie ich `Evaluate` metod), a następnie wykonuje danej operacji arytmetycznej.

Następujący program używa `Expression` klasy można oszacować wyrażenia `x * (y + 2)` zależności od wartości `x` i `y`.

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

#### <a name="method-overloading"></a>Przeciążenie metody

Metoda ***przeciążenie*** zezwala na wiele sposobów w tej samej klasie w celu mają taką samą nazwę, tak długo, jak długo mają unikatowe podpisów. Podczas kompilowania wywołanie metody przeciążonej, kompilator używa ***Rozpoznanie przeciążenia*** ustalenie określonej metody do wywołania. Rozpoznanie przeciążenia umożliwia znalezienie jednej metody, najlepiej odpowiada argumenty lub zgłasza błąd, jeśli można znaleźć nie pojedyncze najlepsze dopasowanie. Poniższy kod przedstawia obowiązuje przeciążeniu rozdzielczości. Komentarz dla każdego wywołania w `Main` metoda ma pokazać, jakiej metody faktycznie jest wywoływana.

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
Jak pokazano na przykładzie, zawsze można wybrać określoną metodę przez jawne rzutowanie argumentów do typów parametru dokładne i/lub jawnie podanie argumentów typu.

### <a name="other-function-members"></a>Inni członkowie — funkcja

Elementy członkowskie, które zawierają kod wykonywalny są nazywane zbiorczo ***funkcji elementów członkowskich*** klasy. W poprzedniej sekcji opisano metody, które są podstawowym typem funkcji elementów członkowskich. W tej sekcji opisano inne rodzaje elementów członkowskich funkcji obsługiwanych przez C#: konstruktory, właściwości, indeksatory, zdarzenia, operatorów i destruktory.

Poniższy kod ilustruje klasę ogólną o nazwie `List<T>`, który implementuje growable listy obiektów. Klasa zawiera kilka przykładów typowych rodzajów funkcji elementów członkowskich.


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

C# obsługuje zarówno wystąpienia i konstruktorów statycznych. ***Konstruktora wystąpienia*** jest elementem członkowskim implementujący działania wymagane w celu zainicjowania wystąpienia klasy. A ***statyczny Konstruktor*** jest elementem członkowskim, który implementuje działania wymagane w celu zainicjowania samej klasy po pierwszym załadowaniu.

Konstruktor jest zadeklarowany jak metody bez zwrotu typu i taką samą nazwę jak klasa zawierająca. Jeśli deklaracja konstruktora zawiera `static` modyfikator, deklaruje Konstruktor statyczny. W przeciwnym razie deklaruje konstruktora wystąpień.

Konstruktory wystąpień, mogą być przeciążone. Na przykład `List<T>` klasa deklaruje dwa konstruktory wystąpienia, jedno z bez parametrów, a ta, która przyjmuje `int` parametru. Konstruktory wystąpień są wywoływane przy użyciu `new` operatora. Poniższe instrukcje przydzielić dwie `List<string>` wystąpień każdej z konstruktorów z `List` klasy.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
W przeciwieństwie do innych członków konstruktorów wystąpienia nie są dziedziczone, a klasa nie ma konstruktorów wystąpienia innych niż rzeczywiście zgłoszonymi w klasie. Jeśli żaden konstruktor wystąpienia zostanie podany dla klasy, następnie pustą bez parametrów jest dostarczana automatycznie.

#### <a name="properties"></a>Właściwości

***Właściwości*** to naturalne rozszerzenie pola. Nazwanych elementów członkowskich skojarzonych typów i składnia do uzyskiwania dostępu do pola i właściwości jest taka sama. Jednak w przeciwieństwie do pola, właściwości określa lokalizacji przechowywania. Właściwości mają ***Akcesory*** określające instrukcji do wykonania, gdy ich wartości są odczytu lub zapisu.

Właściwość deklarowany jest jak pola, z tą różnicą, że deklaracja kończy się `get` metody dostępu i/lub `set` akcesor zapisywane między ogranicznikami `{` i `}` zamiast kończy się średnikiem. Właściwości, które ma obydwa `get` metody dostępu i `set` akcesor jest ***odczytu i zapisu właściwości***, właściwość, która ma tylko `get` akcesor jest ***właściwość tylko do odczytu***i Właściwość, która ma tylko `set` akcesor jest ***właściwość tylko do zapisu***.

A `get` akcesor odnosi się do metody bez parametrów, z wartością zwracaną z typem właściwości. Z wyjątkiem elementem docelowym przypisania, gdy właściwość jest przywoływany w wyrażeniu, `get` metody dostępu właściwości jest wywoływana w celu obliczenia wartości właściwości.

A `set` akcesor odnosi się do metody z pojedynczym parametrem o nazwie `value` i bez zwrotu typu. Gdy właściwość odwołuje się do jako element docelowy przypisania lub operand `++` lub `--`, `set` akcesor zostanie wywołana z nieprawidłowym argumentem, który zawiera nową wartość.

`List<T>` Klasa deklaruje dwie właściwości `Count` i `Capacity`, które są tylko do odczytu i odczytu / zapisu, odpowiednio. Oto przykład użycia tych właściwości.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Podobnie jak pola i metody, C# obsługuje zarówno właściwości wystąpienia i właściwości statyczne. Właściwości statyczne są uznane za pomocą `static` modyfikator i właściwości instancji, które są zadeklarowane bez niego.

Accessor(s) właściwości mogą być wirtualne. Jeśli deklaracja właściwości zawiera `virtual`, `abstract`, lub `override` modyfikator, dotyczy ona accessor(s) właściwości.

#### <a name="indexers"></a>Indeksatory

***Indeksatora*** jest element członkowski, który umożliwia obiekty, które mają być indeksowane w taki sam sposób, w postaci tablicy. Indeksator deklarowany jest jak właściwości, z tą różnicą, że nazwa elementu członkowskiego jest `this` następuje lista parametrów, zapisywane między ogranicznikami `[` i `]`. Parametry są dostępne w accessor(s) indeksatora. Podobnie jak właściwości, indeksatory można odczytu i zapisu, tylko do odczytu i tylko do zapisu, a accessor(s) indeksatora mogą być wirtualne.

`List` Klasa deklaruje pojedynczego indeksatora odczytu i zapisu, która przyjmuje `int` parametru. Indeksator umożliwia indeksu `List` wystąpień z `int` wartości. Na przykład

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
Indeksatory mogą być przeciążone, co oznacza, że klasy można zadeklarować wiele indeksatorów, tak długo, jak liczba lub rodzaju ich parametrów różnią się.

#### <a name="events"></a>Zdarzenia

***Zdarzeń*** jest elementem członkowskim, który umożliwia klasa lub obiekt, do udostępniania powiadomień o. Zdarzenie deklarowany jest jak pola, z tą różnicą, że deklaracja zawiera `event` — słowo kluczowe i typ musi być typem delegowanym.

W obrębie klasy, która deklaruje element członkowski zdarzenia zdarzenie zachowuje się tak samo jak pola typu delegata (pod warunkiem zdarzenie nie jest abstrakcyjna i nie deklaruje metody dostępu). Pole zawiera odwołanie do delegata reprezentującego procedury obsługi zdarzeń, które zostały dodane do zdarzenia. Jeśli nie obsługuje zdarzeń są obecne, pole jest `null`.

`List<T>` Klasa deklaruje składową pojedyncze zdarzenie o nazwie `Changed`, co oznacza, że dodano nowy element do listy. `Changed` Wydarzenie jest podniesione przez `OnChanged` metody wirtualnej, który po raz pierwszy sprawdza, czy zdarzenie jest `null` (co oznacza, że nie programów obsługi istnieje). Pojęcie podnoszenie zdarzenia odpowiada dokładnie wywoływania delegata reprezentowanej przez zdarzenie — tak więc nie istnieją żadne konstrukcje specjalny język przeznaczony dla podnoszonego zdarzenia.

Klienci reagowania na zdarzenia za pośrednictwem ***procedury obsługi zdarzeń***. Programy obsługi zdarzeń dołączonych przy użyciu `+=` operatora i usunięte przy użyciu `-=` operatora. Poniższy przykład dołącza program obsługi zdarzeń do `Changed` zdarzenia `List<string>`.

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
W przypadku zaawansowanych scenariuszy, w którym pożądane jest formantu powiązanego magazynu zdarzenie, jawnie określić deklaracji zdarzenia `add` i `remove` metod dostępu, które są nieco podobne do `set` metody dostępu właściwości.

#### <a name="operators"></a>Operatory

***Operator*** jest element członkowski, który definiuje znaczenie zastosowania operatora poszczególnych wyrażeń do wystąpienia klasy. Można zdefiniować trzy rodzaje operatory: jednoargumentowe operatory, operatory binarne i operatory konwersji. Wszystkie operatory musi być zadeklarowany jako `public` i `static`.

`List<T>` Klasa deklaruje dwa operatory `operator==` i `operator!=`i dlatego zapewnia nowe znaczenie wyrażenia, które są stosowane te operatory, aby `List` wystąpień. W szczególności operatorów definiowania równości dwóch `List<T>` wystąpienia jako porównując każdej zawartych obiektów za pomocą ich `Equals` metody. W poniższym przykładzie użyto `==` operatora do porównywania dwóch `List<int>` wystąpień.

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

Pierwszy `Console.WriteLine` generuje `True` ponieważ dwie listy zawiera taką samą liczbę obiektów o tej samej wartości w tej samej kolejności. Gdyby `List<T>` Niezdefiniowany `operator==`, pierwszy `Console.WriteLine` będzie mieć danych wyjściowych `False` ponieważ `a` i `b` odwołanie do innego `List<int>` wystąpień.

#### <a name="destructors"></a>Destruktory

A ***destruktor*** jest elementem członkowskim implementujący działania wymagane do niszczenia wystąpienia klasy. Destruktory nie mogą mieć parametrów, nie mają modyfikatory dostępności i nie można wywołać jawnie. Destruktor dla wystąpienia jest wywoływana automatycznie podczas wyrzucania elementów bezużytecznych.

Moduł odśmiecania pamięci jest dozwolona szerokiego szerokości przy podejmowaniu decyzji podczas zbierania obiektów i uruchomić destruktorów. Czas wywołania destruktora nie jest jednoznaczny i destruktory mogą być wykonywane na żadnym z wątków. Dla tych i innych powodów klasy należy zaimplementować destruktory tylko wtedy, gdy nie inne rozwiązania są niewykonalne.

`using` Instrukcja oferuje lepszym rozwiązaniem do zniszczenia obiektu.

## <a name="structs"></a>Struktury

Takie jak klasy, ***struktury*** są struktur danych, które mogą zawierać elementy członkowskie danych i składowe funkcji, ale w przeciwieństwie do klasy, struktury są typami wartości i nie wymagają alokacji sterty. Zmienną typu struktury bezpośrednio przechowuje dane struktury, natomiast zmienna typu klasa przechowuje odwołania do obiektu przydzielanego dynamicznie. Typy struktury nie obsługują dziedziczenia określonych przez użytkownika, a wszystkie typy struktury niejawnie dziedziczą z typu `object`.

Struktury są szczególnie przydatne w przypadku małych strukturach danych, które mają semantyki wartości. Liczby zespolone, punkty w układzie współrzędnych lub par klucz wartość ze słownika są dobrym przykładem struktury. Używanie struktur, zamiast klasy w małych strukturach danych ułatwia duża różnica w liczbie alokacji pamięci aplikacji wykonuje. Na przykład następujący program tworzy i inicjuje tablicę 100 punktów. Za pomocą `Point` implementowany jako klasa, 101 oddzielne obiekty są tworzone — jeden dla macierzy i jeden dla 100 elementów.

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
Alternatywą jest zapewnienie `Point` struktury.

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
Teraz, zostanie uruchomiony tylko jeden obiekt — jeden dla tablicy — i `Point` wystąpienia są przechowywane w tekście w tablicy.

Struct — Konstruktorzy są wywoływane przy użyciu `new` operatora, ale nie oznacza, że pamięć jest przydzielane. Zamiast dynamicznej alokacji obiektu i zwraca odwołanie do niej Konstruktor struktury po prostu zwraca wartość struct (zwykle znajduje się w lokalizacji tymczasowej na stosie), a ta wartość jest następnie kopiowana gdy jest to konieczne.

W przypadku klas jest możliwe dwóch zmiennych odwoływać się do tego samego obiektu dla operacji na jednej zmiennej miały wpływ na obiekt odwołuje się druga zmienna zatem możliwe. Przy użyciu struktury zmienne każda ma własne kopię danych, a nie jest możliwe dla operacji na jednym wpływa na drugi. Na przykład dane wyjściowe generowane przez następujący fragment kodu jest zależna od tego, czy `Point` jest klasą lub strukturą.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Jeśli `Point` jest klasą, dane wyjściowe są `20` ponieważ `a` i `b` odwoływać się do tego samego obiektu. Jeśli `Point` jest strukturą, dane wyjściowe są `10` ponieważ przypisanie `a` do `b` tworzona jest kopia wartości, i nie ma wpływu następne przypisanie do tej kopii `a.x`.

W poprzednim przykładzie wyróżniono dwa ograniczenia dotyczące struktury. Po pierwsze całej strukturze jest to zazwyczaj mniej wydajne niż kopiowanie odwołanie do obiektu, dzięki czemu przekazywanie przypisania i wartość parametru może być bardziej kosztowne przy użyciu struktury niż w przypadku typów referencyjnych. Drugi, z wyjątkiem `ref` i `out` parametrów, nie jest możliwe do utworzenia odwołania do struktury, która wyklucza ich użycia w różnych sytuacjach.

## <a name="arrays"></a>Tablice

***Tablica*** to struktura danych zawierająca pewną liczbę zmiennych, do których dostęp jest uzyskiwany za pomocą obliczonych indeksów. Zmienne zawartych w tablicy, jest określana skrótem ***elementy*** tablicy, są wszystkie tego samego typu, a tego typu jest nazywana ***typ elementu*** tablicy.

Typy tablicowe są typami odwołań, a deklaracja zmiennej tablicy po prostu rezerwuje miejsce dla odwołania do wystąpienia tablicy. Wystąpienia bieżącej tablicy są tworzone dynamicznie w czasie wykonywania za pomocą `new` operatora. `new` Operacji określa ***długość*** nowego wystąpienia tablicy naprawiliśmy dla okresu istnienia wystąpienia. Indeksy elementów z zakresu tablicy `0` do `Length - 1`. `new` Operator automatycznie inicjuje elementy tablicy, aby przywrócić wartości domyślne, na przykład są to wartości zero dla wszystkich typów liczbowych i `null` dla wszystkich typów odniesienia.

Poniższy przykład tworzy tablicę `int` elementów, inicjuje tablicy i wyświetla zawartość tablicy.

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
W tym przykładzie tworzy i działa na ***tablicy jednowymiarowej***. C# obsługuje również ***Wielowymiarowe tablice***. Wpisz liczbę wymiarów tablicy, znany także jako ***ranga*** typ tablicy jest jedną przesunięta liczbę przecinkami zapisywane między nawiasami kwadratowymi typu tablicy. Poniższy przykład przydziela jednowymiarową dwuwymiarowym i tablicę trójwymiarową.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
`a1` Tablica zawiera 10 elementów `a2` tablica zawiera 50 (10 x 5) elementów, a `a3` tablica zawiera 100 (10 x 5 x 2) elementów.

Typ elementu tablicy może być dowolnego typu, w tym typu tablicowego. Tablicę z elementami typu tablicowego jest czasami nazywane ***tablicy nieregularnej*** ponieważ długości tablic elementu nie wszystkie muszą być takie same. Poniższy przykład przydziela tablicy tablic `int`:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
Pierwszy wiersz tworzy tablicę z trzech elementów, z których każdy typ `int[]` , a każdy z wartości początkowej `null`. Kolejne wiersze następnie zainicjowanie trzech elementów z odwołaniami do wystąpień poszczególnych tablicy o różnej długości.

`new` Operator zezwala na wartość początkową elementów tablicy, należy określić przy użyciu ***inicjatora tablicy***, który znajduje się lista wyrażeń zapisywane między ogranicznikami `{` i `}`. Poniższy przykład przydziela i inicjuje `int[]` za pomocą trzech elementów.

```csharp
int[] a = new int[] {1, 2, 3};
```
Należy pamiętać, że długość tablicy jest wnioskowany z liczba wyrażeń między `{` i `}`. Zmienna lokalna i deklaracji pól można skrócony dalsze taki sposób, że typ tablicy nie ma być przekształcone.

```csharp
int[] a = {1, 2, 3};
```
W poprzednich przykładach są odpowiednikiem następujących czynności:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Interfejsy

***Interfejsu*** definiuje kontrakt, który może być implementowany przez klas i struktur. Interfejs może zawierać metody, właściwości, zdarzeń i indeksatorów. Interfejs nie zawiera implementacji członków definiuje — jedynie określa elementy członkowskie, które muszą być dostarczane przez klasy lub struktury, które implementują interfejs.

Interfejsy mogą stosować ***wielokrotne dziedziczenie***. W poniższym przykładzie interfejs `IComboBox` dziedziczy z obu `ITextBox` i `IListBox`.

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
Klasy i struktury można zaimplementować wiele interfejsów. W poniższym przykładzie klasa `EditBox` implementuje interfejsy `IControl` i `IDataBound`.

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
Gdy klasa lub struktura implementuje danego interfejsu, wystąpienia tej klasy lub struktury mogą być niejawnie konwertowane do tego typu interfejsu. Na przykład

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
W przypadkach, gdy wystąpienie nie jest znany statycznie do implementowania określonego interfejsu służy rzutowania typu dynamicznego. Na przykład poniższe instrukcje użyć rzutowania typu dynamicznego do uzyskiwania obiektu `IControl` i `IDataBound` implementacji interfejsu. Ponieważ jest rzeczywisty typ obiektu `EditBox`, rzutowania powiodło się.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
W ciągu poprzednich `EditBox` klasy `Paint` metody z `IControl` interfejsu i `Bind` metody z `IDataBound` interfejs jest implementowany przy użyciu `public` elementów członkowskich. C# obsługuje również ***implementacji elementu członkowskiego interfejsu jawnego***, za pomocą której klasy lub struktury można uniknąć, dzięki czemu członkowie `public`. Implementacja interfejsu jawnego członka jest zapisywany przy użyciu interfejsu w pełni kwalifikowana nazwa elementu członkowskiego. Na przykład `EditBox` implementacji klasy `IControl.Paint` i `IDataBound.Bind` metod za pomocą jawnych implementacji elementu członkowskiego interfejsu w następujący sposób.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Elementy członkowskie interfejsu jawnego zostać oceniony jedynie przez typ interfejsu. Na przykład implementacji `IControl.Paint` dostarczone przez poprzednie `EditBox` klasy może być wywoływany tylko przez uprzedniego przekonwertowania `EditBox` odwołanie do `IControl` typ interfejsu.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Wyliczenia

***Typ wyliczeniowy*** to odrębny typ wartości zawierający zestaw nazwanych stałych. Poniższy przykład deklaruje i korzysta z typu wyliczeniowego, o nazwie `Color` z trzech wartości stałych, `Red`, `Green`, i `Blue`.

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
Każdy typ wyliczenia ma odpowiedni typ całkowity, o nazwie ***typ bazowy*** typu wyliczeniowego. Typ wyliczeniowy, który jawnie deklaruj typu podstawowego jest podstawowym typem `int`. Format magazynu i zakres możliwych wartości typu wyliczeniowego są określane przez jej typ podstawowy. Zbiór wartości, które mogą przejąć typ wyliczeniowy nie jest ograniczona przez jej elementów członkowskich wyliczenia. W szczególności dowolną wartość w podstawowym typem wyliczenia mogą być rzutowane na typ wyliczeniowy i jest różne prawidłową wartością tego typu wyliczeniowego.

Poniższy przykład deklaruje typu wyliczeniowego, o nazwie `Alignment` z podstawowym typem `sbyte`.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Jak pokazano w poprzednim przykładzie, deklaracja elementu członkowskiego wyliczenia mogą zawierać wyrażenie stałe, które określa wartość elementu członkowskiego. Stała wartość dla każdego elementu członkowskiego wyliczenia musi być w zakresie podstawowym typem wyliczenia. Element członkowski w deklaracji elementu członkowskiego wyliczenia bez określenia wartości, otrzymuje wartość zero (jeśli jest to pierwszy element członkowski w typie wyliczeniowym) lub wartość składowej wyliczenia w formie tekstu poprzedniego plus jeden.

Wartości wyliczenia może być przekonwertowany na całkowite wartości i odwrotnie przy użyciu typu rzutowania. Na przykład

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
Wartość domyślna typu wyliczenia jest wartością typu całkowitego zero konwertowane na typ wyliczeniowy. W przypadkach, gdy zmienne są automatycznie inicjowane na wartość domyślna to wartość wyliczenia typów zmiennych. Aby wartość domyślną typu wyliczeniowego ma być łatwo dostępne, literał `0` niejawnie konwertuje do dowolnego typu wyliczenia. W związku z tym że jest dozwolone.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Delegaty

A ***typ delegowany*** reprezentuje odwołania do metod z określonego parametru listy i typ zwracany. Delegatów można umożliwić traktować metod jako jednostki, które mogą być przypisane do zmiennych i przekazywane jako parametry. Delegaty są podobne do koncepcji wskaźników funkcji, w przeciwieństwie do innych języków, ale w przeciwieństwie do wskaźników funkcji, obiekty delegowane są zorientowane obiektowo i bezpieczny typowo.

Poniższy przykład deklaruje i wykorzystuje typ delegata, o nazwie `Function`.

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
Wystąpienie `Function` typ delegata można odwoływać się do dowolnej metody, która przyjmuje `double` argument i zwraca `double` wartość. `Apply` Metoda dotyczy danego `Function` do elementów `double[]`, zwracanie `double[]` z wynikami. W `Main` metody `Apply` służy do trzech różnych funkcji w celu stosowania `double[]`.

Delegat może odwoływać się statycznej metody (takie jak `Square` lub `Math.Sin` w poprzednim przykładzie) lub metodą wystąpienia (takie jak `m.Multiply` w poprzednim przykładzie). Delegat, który odwołuje się do metody wystąpienia odwołuje się konkretny obiekt, a gdy wywoływana jest metoda wystąpienia, za pośrednictwem delegata, staje się ten obiekt `this` w wywołaniu.

Delegatów można także tworzyć, używając funkcji anonimowych, które są "wbudowane metody", które są tworzone na bieżąco. Funkcje anonimowe można zobaczyć zmienne lokalne otaczającym metody. W związku z tym, w powyższym przykładzie Mnożnik można napisać tak, aby łatwiej bez korzystania z `Multiplier` klasy:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Interesujące i przydatne własności delegata jest, nie wiedzieć, i nie interesuje klasy, metody, która odwołuje się do; wszystko, co dla Ciebie ważne jest, do którego istnieje odwołanie metoda ma te same parametry oraz zwracany typ jak delegat.

## <a name="attributes"></a>Atrybuty

Typy, członków i inne jednostki w programie C# obsługuje modyfikatory, które kontrolują niektóre aspekty swojego zachowania. Na przykład dostępność metody jest kontrolowany przy użyciu `public`, `protected`, `internal`, i `private` modyfikatorów. C# uogólnia tej możliwości w taki sposób, że typy zdefiniowane przez użytkownika deklaratywne informacji może być dołączony do programu jednostek i pobierane w czasie wykonywania. Programy Określ dodatkowe informacje deklaratywne przez definiowanie i korzystanie z ***atrybuty***.

Poniższy przykład deklaruje `HelpAttribute` atrybut, który można umieścić na program jednostki zawierają łącza do ich dokumentacji skojarzone.

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
Wszystkie klasy atrybutu pochodzić od `System.Attribute` dostarczane przez program .NET Framework klasy bazowej. Atrybuty mogą być stosowane, zapewniając ich nazw, wraz z żadnych argumentów, w nawiasach kwadratowych tuż przed skojarzone deklaracji. Jeśli nazwa atrybutu, który kończy się na `Attribute`, gdy odwołuje się do atrybutu można pominąć tę część nazwy. Na przykład `HelpAttribute` atrybut może być używany w następujący sposób.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
W tym przykładzie dołącza `HelpAttribute` do `Widget` klasy, a drugi `HelpAttribute` do `Display` metody w klasie. Konstruktory publiczne klasy atrybutu kontrolować informacje, które należy podać, gdy atrybut jest dołączany do jednostki programu. Można podać dodatkowe informacje, odwołując się do właściwości publiczne odczytu / zapisu atrybutu klasy (np. odwołanie do `Topic` właściwość wcześniej).

W poniższym przykładzie pokazano, jak można pobrać informacje o atrybutach dotyczące jednostki danego programu w czasie wykonywania przy użyciu odbicia.

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
Określony atrybut zleconą przez odbicie konstruktora dla klasy atrybutu jest wywoływana z informacji podanych w źródle programu, a wynikowy wystąpienie atrybutu jest zwracana. Dodatkowe informacje były udostępniane za pośrednictwem właściwości, te właściwości są ustawione na wartości podanej w przed zwróceniem wystąpienie atrybutu.
