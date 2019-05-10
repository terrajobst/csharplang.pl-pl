---
ms.openlocfilehash: 3232163ed91d9d8bb6b0babf94c4282bfd60976c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488935"
---
# <a name="namespaces"></a>Namespaces

C# programy są zorganizowane przy użyciu przestrzeni nazw. Przestrzenie nazw są używane zarówno jako "internal" organizacji system dla programu, jak i jako system "external" organizacji — sposób przedstawiania elementy programu, które są widoczne dla innych programów.

Za pomocą dyrektywy ([dyrektywy Using](namespaces.md#using-directives)) znajdują się w celu ułatwienia korzystania z przestrzeni nazw.

## <a name="compilation-units"></a>Jednostki kompilacji

A *compilation_unit* definiuje ogólną strukturę pliku źródłowego. Jednostka kompilacji, który składa się z zero lub więcej *using_directive*s, po której następuje zero lub więcej *global_attributes* następuje zero lub więcej *namespace_member_declaration*s .

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

Program C#, który składa się z co najmniej jedną jednostkę kompilacji, każdy zawarte w pliku oddzielne źródło. Gdy program C# jest skompilowany, wszystkie jednostki kompilacji są przetwarzane razem. W efekcie jednostki kompilacji może zależeć od siebie, prawdopodobnie w sposób cykliczne.

*Using_directive*s wpływ jednostki kompilacji *global_attributes* i *namespace_member_declaration*s tej jednostki kompilacji, ale nie mają wpływu na inne jednostki kompilacji.

*Global_attributes* ([atrybuty](attributes.md)) jednostki kompilacji zezwalać na specyfikację atrybuty dla zestawu docelowego i moduł. Zestawów i modułów działają jak kontenery fizyczne dla typów. Zestaw może składać się z kilku modułów fizycznie oddzielona.

*Namespace_member_declaration*s każda jednostka kompilacji programu współtworzyć elementów członkowskich do miejsca jednej deklaracji nazywanego globalnej przestrzeni nazw. Na przykład:

Plik `A.cs`:
```csharp
class A {}
```

Plik `B.cs`:
```csharp
class B {}
```

Jednostki kompilacji dwóch przyczyniają się do pojedynczego globalnej przestrzeni nazw, w tym przypadku deklarowania dwóch klas z w pełni kwalifikowane nazwy `A` i `B`. Ponieważ jednostki kompilacji dwóch przyczynić się do tej samej przestrzeni deklaracji, byłoby błąd, jeśli każdy zawartych w deklaracji elementu członkowskiego o takiej samej nazwie.

## <a name="namespace-declarations"></a>Deklaracji Namespace

A *namespace_declaration* składa się z słowa kluczowego `namespace`, następuje nazwa przestrzeni nazw i treści, opcjonalnie następuje średnik.

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

A *namespace_declaration* może wystąpić jako deklaracja najwyższego poziomu w *compilation_unit* lub jako deklaracja elementu członkowskiego w innym *namespace_declaration*. Gdy *namespace_declaration* występuje jako deklaracja najwyższego poziomu w *compilation_unit*, przestrzeń nazw stanie się członkiem globalnej przestrzeni nazw. Gdy *namespace_declaration* występuje w innym *namespace_declaration*, wewnętrzny przestrzeni nazw, stanie się członkiem zewnętrzne przestrzeni nazw. W obu przypadkach nazwa przestrzeni nazw musi być unikatowa w obrębie zawierającej przestrzeni nazw.

Przestrzenie nazw są niejawnie `public` i deklaracji przestrzeni nazw nie może zawierać żadnych modyfikatory dostępu.

W ramach *namespace_body*, opcjonalny *using_directive*s zaimportować nazwami innych obszarów nazw, typów i członków, umożliwiając im się odwoływać bezpośrednio zamiast programu za pomocą nazwy kwalifikowane. Opcjonalny *namespace_member_declaration*s współtworzyć elementów członkowskich przestrzeni deklaracji przestrzeni nazw. Należy pamiętać, że wszystkie *using_directive*s musi występować przed deklaracjami dowolnego elementu członkowskiego.

*Qualified_identifier* z *namespace_declaration* może być pojedynczy identyfikator lub Sekwencja identyfikatorów oddzielone "`.`" tokenów. Ostatnie formularza umożliwia programowi do definiowania zagnieżdżone przestrzenie nazw bez zagnieżdżania leksykalnie kilka deklaracji przestrzeni nazw. Na przykład

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
są semantycznie równoważne
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

Przestrzenie nazw są udzielający i dwie deklaracje przestrzeni nazw o takiej samej nazwie FQDN przyczyniają się do tej samej przestrzeni deklaracji ([deklaracje](basic-concepts.md#declarations)). W przykładzie
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
dwie deklaracje przestrzeni nazw powyżej przyczyniają się do tej samej przestrzeni deklaracji, w tym przypadku deklarowania dwóch klas z w pełni kwalifikowane nazwy `N1.N2.A` i `N1.N2.B`. Ponieważ dwie deklaracje przyczynić się do tej samej przestrzeni deklaracji, byłoby czy błędu, jeśli każdy zawartych w deklaracji elementu członkowskiego o takiej samej nazwie.

## <a name="extern-aliases"></a>Aliasy extern

*Extern_alias_directive* wprowadza identyfikator, który służy jako alias dla przestrzeni nazw. Specyfikacja aliasu przestrzeni nazw jest zewnętrzne w stosunku do kodu źródłowego programu i stosuje się również do zagnieżdżone przestrzenie nazw aliasu przestrzeni nazw.

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

Zakres *extern_alias_directive* rozciąga *using_directive*s, *global_attributes* i *namespace_member_declaration*s jego kompilacji natychmiast zawierający treść jednostki lub przestrzeni nazw.

W ramach kompilacji zespołu lub przestrzeni nazw treść, która zawiera *extern_alias_directive*, identyfikator wynikające z *extern_alias_directive* można odwoływać się do aliasu przestrzeni nazw. Jest to błąd czasu kompilacji dla *identyfikator* wyrazu `global`.

*Extern_alias_directive* alias udostępnienie w treści jednostki lub przestrzeni nazw określonej kompilacji, ale nie wpływa na żadnych nowych elementów członkowskich do obszaru bazowego deklaracji. Innymi słowy *extern_alias_directive* nie jest przechodnia, ale raczej dotyczy tylko kompilacji zespołu lub przestrzeni nazw treści w której występuje.

Następujący program deklaruje i wykorzystuje dwa extern aliasy, `X` i `Y`, każdy z reprezentujący głównego hierarchii unikatowych nazw:
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

Program deklaruje istnienie extern aliasy `X` i `Y`, ale aktualne definicje aliasów są zewnętrzne w stosunku do programu. O identycznej nazwie `N.B` klasy można teraz przywoływać jako `X.N.B` i `Y.N.B`, lub za pomocą kwalifikator aliasu przestrzeni nazw, `X::N.B` i `Y::N.B`. Błąd występuje, jeśli program deklaruje alias zewnętrzny, dla których dostępny jest Brak definicji zewnętrznych.

## <a name="using-directives"></a>dyrektywy Using

***Dyrektywy Using*** ułatwienia korzystania z przestrzeni nazw i typów zdefiniowanych w innych przestrzeniach nazw. Za pomocą dyrektywy wpływu na proces rozpoznawania nazw dla *namespace_or_type_name*s ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) i *simple_name*s ([proste nazwy ](expressions.md#simple-names)), ale w przeciwieństwie do deklaracji, za pomocą dyrektywy nie przyczyniają się nowych członków do podstawowej deklaracji miejsca do magazynowania jednostki kompilacji lub przestrzeni nazw, w którym są używane.

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

A *using_alias_directive* ([alias dyrektywy Using](namespaces.md#using-alias-directives)) wprowadzono alias dla przestrzeni nazw lub typu.

A *using_namespace_directive* ([przy użyciu dyrektywy przestrzeni nazw](namespaces.md#using-namespace-directives)) importuje elementy członkowskie typu przestrzeni nazw.

A *using_static_directive* ([statyczne dyrektywy Using](namespaces.md#using-static-directives)) importuje typów zagnieżdżonych i statyczne elementy członkowskie typu.

Zakres *using_directive* rozciąga *namespace_member_declaration*s jego kompilacji natychmiast zawierający treść jednostki lub przestrzeni nazw. Zakres *using_directive* specjalnie nie obejmuje jego elementów równorzędnych *using_directive*s. W związku z tym, komunikacja równorzędna *using_directive*s nie wpływają na siebie nawzajem i kolejność, w którym są zapisywane, jest niewielka.

### <a name="using-alias-directives"></a>Alias dyrektyw Using

A *using_alias_directive* wprowadza identyfikator, który służy jako alias dla przestrzeni nazw lub typ treści bezpośrednio otaczającej kompilacji zespołu lub przestrzeni nazw.

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

W deklaracji elementu członkowskiego w kompilacji zespołu lub przestrzeni nazw treść, która zawiera *using_alias_directive*, identyfikator wynikające z *using_alias_directive* może służyć do odwołania do danego przestrzeń nazw lub typu. Na przykład:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

Powyżej, w ramach deklaracji elementu członkowskiego w `N3` przestrzeni nazw, `A` jest aliasem dla `N1.N2.A`i w związku z tym klasy `N3.B` pochodzi od klasy `N1.N2.A`. Ten sam efekt można uzyskać, tworząc alias `R` dla `N1.N2` i następnie odwoływanie się do `R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

*Identyfikator* z *using_alias_directive* muszą być unikatowe w obrębie przestrzeni deklaracji jednostki kompilacji lub przestrzeni nazw, która zawiera bezpośrednio *using_alias_directive* . Na przykład:
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

Powyżej, `N3` zawiera już element członkowski `A`, więc jest to błąd czasu kompilacji dla *using_alias_directive* do użycia tego identyfikatora. Podobnie, jest to błąd czasu kompilacji, dla co najmniej dwóch *using_alias_directive*s w tej samej kompilacji zespołu lub przestrzeni nazw treści do deklarowania aliasów w tej samej nazwie.

A *using_alias_directive* alias udostępnienie w treści jednostki lub przestrzeni nazw określonej kompilacji, ale nie wpływa na żadnych nowych elementów członkowskich do obszaru bazowego deklaracji. Innymi słowy *using_alias_directive* nie jest przechodnia, ale raczej dotyczy tylko kompilacji zespołu lub przestrzeni nazw treści w której występuje. W przykładzie
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
zakres *using_alias_directive* powoduje to wprowadzenie `R` rozciąga się tylko do deklaracji elementu członkowskiego w treści przestrzeni nazw, w którym znajduje się więc `R` jest nieznany w drugim deklaracji przestrzeni nazw. Jednak umieszczenie *using_alias_directive* w kompilacji zawierającej jednostki powoduje, że alias staną się dostępne w obu deklaracje przestrzeni nazw:
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

Podobnie jak zwykłe elementy członkowskie, wynikające z nazwami *using_alias_directive*s są ukrywane o podobnej nazwie elementów członkowskich w zagnieżdżonych zakresów. W przykładzie
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
Odwołanie do `R.A` w deklaracji `B` powoduje błąd w czasie kompilacji, ponieważ `R` odwołuje się do `N3.R`, a nie `N1.N2`.

Kolejność, w której *using_alias_directive*s są zapisywane, nie ma znaczenia i rozpoznawanie *namespace_or_type_name* odwołuje *using_alias_directive*nie dotyczy *using_alias_directive* sam lub przez inne *using_directive*s do kompilacji zawierającego treści jednostki lub przestrzeni nazw. Innymi słowy *namespace_or_type_name* z *using_alias_directive* nie zostanie rozwiązany tak, jakby natychmiast zawierający treść jednostki lub przestrzeni nazw kompilacji ma nie *using_directive*s. A *using_alias_directive* można jednak wpływ *extern_alias_directive*s do kompilacji zawierającego treści jednostki lub przestrzeni nazw. W przykładzie
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
ostatni *using_alias_directive* powoduje błąd w czasie kompilacji, ponieważ nie ma wpływu pierwszy *using_alias_directive*. Pierwszy *using_alias_directive* nie powoduje błąd, ponieważ zakres alias zewnętrzny `E` obejmuje *using_alias_directive*.

A *using_alias_directive* można utworzyć aliasu przestrzeni nazw lub typ, włącznie z przestrzenią nazw, w którym pojawia się i wszelkie przestrzeń nazw lub typ zagnieżdżony w tej przestrzeni nazw.

Uzyskiwanie dostępu do przestrzeni nazw lub typ za pomocą aliasu daje ten sam wynik, jak uzyskiwanie dostępu do tej przestrzeni nazw lub typ za pośrednictwem jego zadeklarowana nazwa. Na przykład biorąc pod uwagę
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
nazwy `N1.N2.A`, `R1.N2.A`, i `R2.A` są równoważne i wszystkie odwołują się do klasy, w których w pełni kwalifikowana nazwa to `N1.N2.A`.

Aliasy użycia może nazwę zamknięte skonstruowanego typu, ale nie nazwa deklaracji typu niepowiązanego ogólny bez podawania argumentów typu. Na przykład:
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a>Za pomocą dyrektywy przestrzeni nazw

A *using_namespace_directive* importuje typy zawartych w przestrzeni nazw do natychmiastowo otaczającą kompilacji zespołu lub przestrzeni nazw treści, umożliwiając identyfikator każdego typu bez kwalifikacji można używać.

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

W deklaracji elementu członkowskiego w kompilacji zespołu lub przestrzeni nazw treść, która zawiera *using_namespace_directive*, typy zawarte w danej przestrzeni nazw może znajdować się bezpośrednio. Na przykład:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

Powyżej, w ramach deklaracji elementu członkowskiego w `N3` przestrzeni nazw, typów elementów członkowskich `N1.N2` są bezpośrednio dostępne i klasy w związku z tym `N3.B` pochodzi od klasy `N1.N2.A`.

A *using_namespace_directive* importuje typy zawarte w danej przestrzeni nazw, ale specjalnie nie są importowane zagnieżdżone przestrzenie nazw. W przykładzie
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
*using_namespace_directive* importuje typy zawarte w `N1`, ale nie przestrzenie nazw są zagnieżdżone w `N1`. W związku z tym, odwołanie do `N2.A` w deklaracji `B` powoduje błąd w czasie kompilacji, ponieważ żaden z elementów członkowskich o nazwie `N2` znajdują się w zakresie.

W odróżnieniu od *using_alias_directive*, *using_namespace_directive* mogą importować typy, których identyfikatory zostały już zdefiniowane w otaczającej treści jednostki lub przestrzeni nazw kompilacji. W efekcie nazw importowanych przez *using_namespace_directive* są ukryte przez członków o podobnej nazwie w otaczającej treści jednostki lub przestrzeni nazw kompilacji. Na przykład:
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

W deklaracji elementu członkowskiego, w tym miejscu `N3` przestrzeni nazw, `A` odwołuje się do `N3.A` zamiast `N1.N2.A`.

Gdy więcej niż jeden obszar nazw lub typ zaimportowany przez *using_namespace_directive*s lub *using_static_directive*s w tę samą treść jednostki lub przestrzeni nazw kompilacji zawierają typy pod tą samą nazwą odwołania do tę nazwę jako *type_name* są traktowane jako niejednoznaczny. W przykładzie
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
zarówno `N1` i `N2` zawierać składowej `A`, a ponieważ `N3` importuje zarówno odwołujące się do `A` w `N3` jest błąd w czasie kompilacji. W takiej sytuacji konflikt można rozwiązać w albo za pośrednictwem kwalifikacji odwołania do `A`, lub wprowadzając *using_alias_directive* , wybiera określonego `A`. Na przykład:
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

Ponadto, gdy więcej niż jeden obszar nazw lub typ zaimportowany przez *using_namespace_directive*s lub *using_static_directive*s w tę samą treść jednostki lub przestrzeni nazw kompilacji zawierają typy lub elementy członkowskie przez tej samej nazwie odwołuje się do tej nazwy, jako *simple_name* są traktowane jako niejednoznaczny. W przykładzie
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
`N1` zawiera element członkowski typu `A`, i `C` zawiera pole statyczne `A`, a ponieważ `N2` importuje zarówno odwołujące się do `A` jako *simple_name* jest niejednoznaczne i w czasie kompilacji Wystąpił błąd. 

Podobnie jak *using_alias_directive*, *using_namespace_directive* nie wpływa na żadnych nowych elementów członkowskich do podstawowej deklaracji przestrzeni jednostki kompilacji lub przestrzeni nazw, ale raczej ma wpływ tylko na Kompilacja zespołu lub przestrzeni nazw treść w której występuje.

*Namespace_name* odwołuje *using_namespace_directive* został rozwiązany w taki sam sposób jak *namespace_or_type_name* odwołuje  *using_alias_directive*. W efekcie *using_namespace_directive*s w tę samą treść jednostki lub przestrzeni nazw kompilacji nie wpływają na siebie nawzajem i mogą być napisane w dowolnej kolejności.

### <a name="using-static-directives"></a>Statyczne dyrektyw Using

A *using_static_directive* importuje typów zagnieżdżonych i statycznych elementów członkowskich znajdujących się bezpośrednio w deklaracji typu do natychmiastowo otaczającą kompilacji zespołu lub przestrzeni nazw treści, umożliwiając identyfikator każdego elementu członkowskiego i typu używane bez kwalifikacji.

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

W deklaracji elementu członkowskiego w kompilacji zespołu lub przestrzeni nazw treść, która zawiera *using_static_directive*, dostępne zagnieżdżone typy i składowe statyczne (z wyjątkiem metody rozszerzenia) znajdujących się bezpośrednio w deklaracji Podany typ można odwoływać się bezpośrednio. Na przykład:

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

Powyżej, w ramach deklaracji elementu członkowskiego w `N2` przestrzeni nazw statycznych składowych i zagnieżdżone typy `N1.A` są bezpośrednio dostępne, a przez to metoda `N` może odwoływać się do zarówno `B` i `M` członkowie `N1.A`.

A *using_static_directive* specjalnie nie importuje metody rozszerzenia bezpośrednio jako metody statyczne, ale udostępnia je dla wywołania metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)). W przykładzie

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
*using_static_directive* importuje metody rozszerzenia `M` zawarte w `N1.A`, ale tylko jako metodę rozszerzenia. Dlatego pierwszego odwołania do `M` w treści `B.N` powoduje błąd w czasie kompilacji, ponieważ żaden z elementów członkowskich o nazwie `M` znajdują się w zakresie.

A *using_static_directive* importuje jedynie członkowie i typy zadeklarowany bezpośrednio w danym typie, nie elementów członkowskich i typy zadeklarowane w klasach bazowych.

TODO: Przykład

Niejednoznaczności między wieloma *using_namespace_directives* i *using_static_directives* zostały omówione w [przy użyciu dyrektywy przestrzeni nazw](namespaces.md#using-namespace-directives).

## <a name="namespace-members"></a>Elementy członkowskie Namespace

A *namespace_member_declaration* jest *namespace_declaration* ([deklaracji Namespace](namespaces.md#namespace-declarations)) lub *type_declaration* () [Wpisz deklaracje](namespaces.md#type-declarations)).

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

Jednostka kompilacji lub treści przestrzeni nazw może zawierać *namespace_member_declaration*s i takie deklaracje współtworzyć nowych elementów członkowskich na podstawowej deklaracji przestrzeń zawierający treść jednostki lub przestrzeni nazw kompilacji.

## <a name="type-declarations"></a>Deklaracje typu

A *type_declaration* jest *class_declaration* ([klasy deklaracje](classes.md#class-declarations)), *struct_declaration* ([— struktura deklaracje](structs.md#struct-declarations)), *interface_declaration* ([interfejsu deklaracje](interfaces.md#interface-declarations)), *enum_declaration* ([wyliczenia deklaracje](enums.md#enum-declarations)), lub *delegate_declaration* ([delegować deklaracje](delegates.md#delegate-declarations)).

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

A *type_declaration* może wystąpić jako deklaracji najwyższego poziomu w jednostce kompilacji lub deklaracja elementu członkowskiego, w ramach przestrzeni nazw, klasy lub struktury.

Gdy deklaracji typu dla typu `T` występuje jako deklaracja najwyższego poziomu w jednostce kompilacji, w pełni kwalifikowaną nazwę nowo deklarowany typ jest po prostu `T`. Gdy deklaracji typu dla typu `T` występuje w przestrzeni nazw, klasy lub struktury, w pełni kwalifikowaną nazwę nowo deklarowany typ jest `N.T`, gdzie `N` jest w pełni kwalifikowaną nazwą zawierającą przestrzeń nazw, klasy lub struktury.

Typ zadeklarowany w klasie lub strukturze nosi nazwę typu zagnieżdżonego ([zagnieżdżone typy](classes.md#nested-types)).

Modyfikatory dostępu dozwolonych i dostęp do domyślnej deklaracji typu są zależne od kontekstu, w którym odbywa się deklaracji ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)):

*  Typy zadeklarowane w jednostkach kompilacji lub przestrzeni nazw mogą mieć `public` lub `internal` dostępu. Wartość domyślna to `internal` dostępu.
*  Typy zadeklarowane w klasach mogą mieć `public`, `protected internal`, `protected`, `internal`, lub `private` dostępu. Wartość domyślna to `private` dostępu.
*  Typy zadeklarowane w strukturach mogą mieć `public`, `internal`, lub `private` dostępu. Wartość domyślna to `private` dostępu.

## <a name="namespace-alias-qualifiers"></a>Kwalifikatory alias Namespace

***Kwalifikator aliasu przestrzeni nazw*** `::` umożliwia zagwarantowanie, że wyszukiwanie nazw typu nie ma wpływu na wprowadzenie nowych typów i elementów członkowskich. Kwalifikator aliasu przestrzeni nazw jest zawsze wyświetlany między dwa identyfikatory określane jako identyfikatory po lewej stronie i po prawej stronie. W odróżnieniu od zwykłych `.` kwalifikator, identyfikator po lewej stronie `::` tablicą kwalifikator tylko jako zewnętrznego lub za pomocą aliasu w górę.

A *qualified_alias_member* jest zdefiniowana w następujący sposób:

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

A *qualified_alias_member* mogą być używane jako *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) lub jako lewy operand w *member_access* ([Dostęp do elementu członkowskiego](expressions.md#member-access)).

A *qualified_alias_member* ma jedną z dwóch formach:

*  `N::I<A1, ..., Ak>`, gdzie `N` i `I` reprezentują identyfikatorów i `<A1, ..., Ak>` jest lista argumentów typu. (`K` jest zawsze co najmniej jeden.)
*  `N::I`, gdzie `N` i `I` reprezentują identyfikatorów. (W tym przypadku `K` jest uważany za wartość zero.)

Za pomocą tej notacji, znaczenie *qualified_alias_member* jest określany w następujący sposób:

*  Jeśli `N` to identyfikator `global`, a następnie połączenie jest wyszukiwane na globalnej przestrzeni nazw `I`:
   * Jeśli globalna przestrzeń nazw zawiera przestrzeń nazw o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego obszaru nazw.
   * W przeciwnym razie, jeśli globalna przestrzeń nazw zawiera typu nieogólnego, o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego typu.
   * W przeciwnym razie, jeśli globalna przestrzeń nazw zawiera typ o nazwie `I` zawierający `K`  parametry typu, a następnie *qualified_alias_member* odwołuje się do tego typu skonstruowany przy użyciu argumentów danego typu.
   * W przeciwnym razie *qualified_alias_member* jest niezdefiniowana, i występuje błąd kompilacji.

*  W przeciwnym razie, począwszy od deklaracji przestrzeni nazw ([deklaracji Namespace](namespaces.md#namespace-declarations)) natychmiast zawierający *qualified_alias_member* (jeśli istnieje), kontynuowanie z każdego otaczającej deklarację przestrzeni nazw (jeśli istnieje), a kończąc na jednostkę kompilacji, zawierającą *qualified_alias_member*, poniższe kroki są oceniane, dopóki nie znajduje się jednostka:

   * Jeśli zawiera jednostki kompilacji lub deklaracji przestrzeni nazw *using_alias_directive* który kojarzy `N` z typem, a następnie *qualified_alias_member* jest niezdefiniowane i w czasie kompilacji występuje błąd.
   * W przeciwnym razie, jeśli zawiera jednostki kompilacji lub deklaracji przestrzeni nazw *extern_alias_directive* lub *using_alias_directive* który kojarzy `N` z obszaru nazw, następnie:
     * Jeśli przestrzeń nazw skojarzony z `N` zawiera przestrzeń nazw o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego obszaru nazw.
     * W przeciwnym razie, jeśli przestrzeń nazw skojarzony z `N` zawiera typu nieogólnego, o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego typu.
     * W przeciwnym razie, jeśli przestrzeń nazw skojarzony z `N` zawiera typ o nazwie `I` zawierający `K`  parametry typu, a następnie *qualified_alias_member* dotyczy, że typ jest zbudowany z argumenty danego typu.
     * W przeciwnym razie *qualified_alias_member* jest niezdefiniowana, i występuje błąd kompilacji.
*  W przeciwnym razie *qualified_alias_member* jest niezdefiniowana, i występuje błąd kompilacji.

Należy pamiętać, że kwalifikator aliasu przestrzeni nazw przy użyciu aliasu, który odwołuje się do typu powoduje błąd kompilacji. Należy również zauważyć, że jeśli identyfikator `N` jest `global`, wówczas wyszukiwania odbywa się w globalnej przestrzeni nazw, nawet jeśli dostępny jest za pomocą aliasu kojarzenie `global` przy użyciu typu lub przestrzeni nazw.

### <a name="uniqueness-of-aliases"></a>Unikatowość nazw aliasów

Treść jednostki i przestrzeni nazw każdej kompilacji ma miejsce na oddzielnych deklaracji aliasy extern i aliasy użycia. W związku z tym, Nazwa alias zewnętrzny lub za pomocą aliasu muszą być unikatowe w zbiorze aliasów extern i aliasy użycia zadeklarowanych w natychmiast zawierający treść jednostki lub przestrzeni nazw kompilacji, aliasu jest mogą mieć taką samą nazwę jak typ lub przestrzeń nazw tak długo, jak i t jest używany tylko z `::` kwalifikator.

W przykładzie
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
Nazwa `A` ma dwa znaczeń w drugim treści przestrzeni nazw, ponieważ zarówno klasy `A` i za pomocą aliasu `A` znajdują się w zakresie. Z tego powodu użycie `A` w kwalifikowanej nazwie `A.Stream` jest niejednoznaczne i powoduje błąd kompilacji występuje. Jednak użycie `A` z `::` kwalifikatora nie jest to błąd, ponieważ `A` wyszukiwane są tylko jako alias przestrzeni nazw.
