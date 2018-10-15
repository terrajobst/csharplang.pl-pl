# <a name="namespaces"></a><span data-ttu-id="be601-101">Namespaces</span><span class="sxs-lookup"><span data-stu-id="be601-101">Namespaces</span></span>

<span data-ttu-id="be601-102">C# programy są zorganizowane przy użyciu przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="be601-103">Przestrzenie nazw są używane zarówno jako "internal" organizacji system dla programu, jak i jako system "external" organizacji — sposób przedstawiania elementy programu, które są widoczne dla innych programów.</span><span class="sxs-lookup"><span data-stu-id="be601-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="be601-104">Za pomocą dyrektywy ([dyrektywy Using](namespaces.md#using-directives)) znajdują się w celu ułatwienia korzystania z przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="be601-105">Jednostki kompilacji</span><span class="sxs-lookup"><span data-stu-id="be601-105">Compilation units</span></span>

<span data-ttu-id="be601-106">A *compilation_unit* definiuje ogólną strukturę pliku źródłowego.</span><span class="sxs-lookup"><span data-stu-id="be601-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="be601-107">Jednostka kompilacji, który składa się z zero lub więcej *using_directive*s, po której następuje zero lub więcej *global_attributes* następuje zero lub więcej *namespace_member_declaration*s .</span><span class="sxs-lookup"><span data-stu-id="be601-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="be601-108">Program C#, który składa się z co najmniej jedną jednostkę kompilacji, każdy zawarte w pliku oddzielne źródło.</span><span class="sxs-lookup"><span data-stu-id="be601-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="be601-109">Gdy program C# jest skompilowany, wszystkie jednostki kompilacji są przetwarzane razem.</span><span class="sxs-lookup"><span data-stu-id="be601-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="be601-110">W efekcie jednostki kompilacji może zależeć od siebie, prawdopodobnie w sposób cykliczne.</span><span class="sxs-lookup"><span data-stu-id="be601-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="be601-111">*Using_directive*s wpływ jednostki kompilacji *global_attributes* i *namespace_member_declaration*s tej jednostki kompilacji, ale nie mają wpływu na inne jednostki kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="be601-112">*Global_attributes* ([atrybuty](attributes.md)) jednostki kompilacji zezwalać na specyfikację atrybuty dla zestawu docelowego i moduł.</span><span class="sxs-lookup"><span data-stu-id="be601-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="be601-113">Zestawów i modułów działają jak kontenery fizyczne dla typów.</span><span class="sxs-lookup"><span data-stu-id="be601-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="be601-114">Zestaw może składać się z kilku modułów fizycznie oddzielona.</span><span class="sxs-lookup"><span data-stu-id="be601-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="be601-115">*Namespace_member_declaration*s każda jednostka kompilacji programu współtworzyć elementów członkowskich do miejsca jednej deklaracji nazywanego globalnej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="be601-116">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-116">For example:</span></span>

<span data-ttu-id="be601-117">Plik `A.cs`:</span><span class="sxs-lookup"><span data-stu-id="be601-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="be601-118">Plik `B.cs`:</span><span class="sxs-lookup"><span data-stu-id="be601-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="be601-119">Jednostki kompilacji dwóch przyczyniają się do pojedynczego globalnej przestrzeni nazw, w tym przypadku deklarowania dwóch klas z w pełni kwalifikowane nazwy `A` i `B`.</span><span class="sxs-lookup"><span data-stu-id="be601-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="be601-120">Ponieważ jednostki kompilacji dwóch przyczynić się do tej samej przestrzeni deklaracji, byłoby błąd, jeśli każdy zawartych w deklaracji elementu członkowskiego o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="be601-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="be601-121">Deklaracji Namespace</span><span class="sxs-lookup"><span data-stu-id="be601-121">Namespace declarations</span></span>

<span data-ttu-id="be601-122">A *namespace_declaration* składa się z słowa kluczowego `namespace`, następuje nazwa przestrzeni nazw i treści, opcjonalnie następuje średnik.</span><span class="sxs-lookup"><span data-stu-id="be601-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

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

<span data-ttu-id="be601-123">A *namespace_declaration* może wystąpić jako deklaracja najwyższego poziomu w *compilation_unit* lub jako deklaracja elementu członkowskiego w innym *namespace_declaration*.</span><span class="sxs-lookup"><span data-stu-id="be601-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="be601-124">Gdy *namespace_declaration* występuje jako deklaracja najwyższego poziomu w *compilation_unit*, przestrzeń nazw stanie się członkiem globalnej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="be601-125">Gdy *namespace_declaration* występuje w innym *namespace_declaration*, wewnętrzny przestrzeni nazw, stanie się członkiem zewnętrzne przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="be601-126">W obu przypadkach nazwa przestrzeni nazw musi być unikatowa w obrębie zawierającej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="be601-127">Przestrzenie nazw są niejawnie `public` i deklaracji przestrzeni nazw nie może zawierać żadnych modyfikatory dostępu.</span><span class="sxs-lookup"><span data-stu-id="be601-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="be601-128">W ramach *namespace_body*, opcjonalny *using_directive*s zaimportować nazwami innych obszarów nazw, typów i członków, umożliwiając im się odwoływać bezpośrednio zamiast programu za pomocą nazwy kwalifikowane.</span><span class="sxs-lookup"><span data-stu-id="be601-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="be601-129">Opcjonalny *namespace_member_declaration*s współtworzyć elementów członkowskich przestrzeni deklaracji przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="be601-130">Należy pamiętać, że wszystkie *using_directive*s musi występować przed deklaracjami dowolnego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="be601-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="be601-131">*Qualified_identifier* z *namespace_declaration* może być pojedynczy identyfikator lub Sekwencja identyfikatorów oddzielone "`.`" tokenów.</span><span class="sxs-lookup"><span data-stu-id="be601-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="be601-132">Ostatnie formularza umożliwia programowi do definiowania zagnieżdżone przestrzenie nazw bez zagnieżdżania leksykalnie kilka deklaracji przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="be601-133">Na przykład</span><span class="sxs-lookup"><span data-stu-id="be601-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="be601-134">są semantycznie równoważne</span><span class="sxs-lookup"><span data-stu-id="be601-134">is semantically equivalent to</span></span>
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

<span data-ttu-id="be601-135">Przestrzenie nazw są udzielający i dwie deklaracje przestrzeni nazw o takiej samej nazwie FQDN przyczyniają się do tej samej przestrzeni deklaracji ([deklaracje](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="be601-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="be601-136">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-136">In the example</span></span>
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
<span data-ttu-id="be601-137">dwie deklaracje przestrzeni nazw powyżej przyczyniają się do tej samej przestrzeni deklaracji, w tym przypadku deklarowania dwóch klas z w pełni kwalifikowane nazwy `N1.N2.A` i `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="be601-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="be601-138">Ponieważ dwie deklaracje przyczynić się do tej samej przestrzeni deklaracji, byłoby czy błędu, jeśli każdy zawartych w deklaracji elementu członkowskiego o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="be601-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="be601-139">Aliasy extern</span><span class="sxs-lookup"><span data-stu-id="be601-139">Extern aliases</span></span>

<span data-ttu-id="be601-140">*Extern_alias_directive* wprowadza identyfikator, który służy jako alias dla przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="be601-141">Specyfikacja aliasu przestrzeni nazw jest zewnętrzne w stosunku do kodu źródłowego programu i stosuje się również do zagnieżdżone przestrzenie nazw aliasu przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="be601-142">Zakres *extern_alias_directive* rozciąga *using_directive*s, *global_attributes* i *namespace_member_declaration*s jego kompilacji natychmiast zawierający treść jednostki lub przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="be601-143">W ramach kompilacji zespołu lub przestrzeni nazw treść, która zawiera *extern_alias_directive*, identyfikator wynikające z *extern_alias_directive* można odwoływać się do aliasu przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="be601-144">Jest to błąd czasu kompilacji dla *identyfikator* wyrazu `global`.</span><span class="sxs-lookup"><span data-stu-id="be601-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="be601-145">*Extern_alias_directive* alias udostępnienie w treści jednostki lub przestrzeni nazw określonej kompilacji, ale nie wpływa na żadnych nowych elementów członkowskich do obszaru bazowego deklaracji.</span><span class="sxs-lookup"><span data-stu-id="be601-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="be601-146">Innymi słowy *extern_alias_directive* nie jest przechodnia, ale raczej dotyczy tylko kompilacji zespołu lub przestrzeni nazw treści w której występuje.</span><span class="sxs-lookup"><span data-stu-id="be601-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="be601-147">Następujący program deklaruje i wykorzystuje dwa extern aliasy, `X` i `Y`, każdy z reprezentujący głównego hierarchii unikatowych nazw:</span><span class="sxs-lookup"><span data-stu-id="be601-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
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

<span data-ttu-id="be601-148">Program deklaruje istnienie extern aliasy `X` i `Y`, ale aktualne definicje aliasów są zewnętrzne w stosunku do programu.</span><span class="sxs-lookup"><span data-stu-id="be601-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="be601-149">O identycznej nazwie `N.B` klasy można teraz przywoływać jako `X.N.B` i `Y.N.B`, lub za pomocą kwalifikator aliasu przestrzeni nazw, `X::N.B` i `Y::N.B`.</span><span class="sxs-lookup"><span data-stu-id="be601-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="be601-150">Błąd występuje, jeśli program deklaruje alias zewnętrzny, dla których dostępny jest Brak definicji zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="be601-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="be601-151">dyrektywy Using</span><span class="sxs-lookup"><span data-stu-id="be601-151">Using directives</span></span>

<span data-ttu-id="be601-152">***Dyrektywy Using*** ułatwienia korzystania z przestrzeni nazw i typów zdefiniowanych w innych przestrzeniach nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="be601-153">Za pomocą dyrektywy wpływu na proces rozpoznawania nazw dla *namespace_or_type_name*s ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) i *simple_name*s ([proste nazwy ](expressions.md#simple-names)), ale w przeciwieństwie do deklaracji, za pomocą dyrektywy nie przyczyniają się nowych członków do podstawowej deklaracji miejsca do magazynowania jednostki kompilacji lub przestrzeni nazw, w którym są używane.</span><span class="sxs-lookup"><span data-stu-id="be601-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="be601-154">A *using_alias_directive* ([alias dyrektywy Using](namespaces.md#using-alias-directives)) wprowadzono alias dla przestrzeni nazw lub typu.</span><span class="sxs-lookup"><span data-stu-id="be601-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="be601-155">A *using_namespace_directive* ([przy użyciu dyrektywy przestrzeni nazw](namespaces.md#using-namespace-directives)) importuje elementy członkowskie typu przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="be601-156">A *using_static_directive* ([statyczne dyrektywy Using](namespaces.md#using-static-directives)) importuje typów zagnieżdżonych i statyczne elementy członkowskie typu.</span><span class="sxs-lookup"><span data-stu-id="be601-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="be601-157">Zakres *using_directive* rozciąga *namespace_member_declaration*s jego kompilacji natychmiast zawierający treść jednostki lub przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="be601-158">Zakres *using_directive* specjalnie nie obejmuje jego elementów równorzędnych *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="be601-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="be601-159">W związku z tym, komunikacja równorzędna *using_directive*s nie wpływają na siebie nawzajem i kolejność, w którym są zapisywane, jest niewielka.</span><span class="sxs-lookup"><span data-stu-id="be601-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="be601-160">Alias dyrektyw Using</span><span class="sxs-lookup"><span data-stu-id="be601-160">Using alias directives</span></span>

<span data-ttu-id="be601-161">A *using_alias_directive* wprowadza identyfikator, który służy jako alias dla przestrzeni nazw lub typ treści bezpośrednio otaczającej kompilacji zespołu lub przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="be601-162">W deklaracji elementu członkowskiego w kompilacji zespołu lub przestrzeni nazw treść, która zawiera *using_alias_directive*, identyfikator wynikające z *using_alias_directive* może służyć do odwołania do danego przestrzeń nazw lub typu.</span><span class="sxs-lookup"><span data-stu-id="be601-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="be601-163">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-163">For example:</span></span>
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

<span data-ttu-id="be601-164">Powyżej, w ramach deklaracji elementu członkowskiego w `N3` przestrzeni nazw, `A` jest aliasem dla `N1.N2.A`i w związku z tym klasy `N3.B` pochodzi od klasy `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="be601-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="be601-165">Ten sam efekt można uzyskać, tworząc alias `R` dla `N1.N2` i następnie odwoływanie się do `R.A`:</span><span class="sxs-lookup"><span data-stu-id="be601-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="be601-166">*Identyfikator* z *using_alias_directive* muszą być unikatowe w obrębie przestrzeni deklaracji jednostki kompilacji lub przestrzeni nazw, która zawiera bezpośrednio *using_alias_directive* .</span><span class="sxs-lookup"><span data-stu-id="be601-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="be601-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-167">For example:</span></span>
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

<span data-ttu-id="be601-168">Powyżej, `N3` zawiera już element członkowski `A`, więc jest to błąd czasu kompilacji dla *using_alias_directive* do użycia tego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="be601-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="be601-169">Podobnie, jest to błąd czasu kompilacji, dla co najmniej dwóch *using_alias_directive*s w tej samej kompilacji zespołu lub przestrzeni nazw treści do deklarowania aliasów w tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="be601-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="be601-170">A *using_alias_directive* alias udostępnienie w treści jednostki lub przestrzeni nazw określonej kompilacji, ale nie wpływa na żadnych nowych elementów członkowskich do obszaru bazowego deklaracji.</span><span class="sxs-lookup"><span data-stu-id="be601-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="be601-171">Innymi słowy *using_alias_directive* nie jest przechodnia, ale raczej dotyczy tylko kompilacji zespołu lub przestrzeni nazw treści w której występuje.</span><span class="sxs-lookup"><span data-stu-id="be601-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="be601-172">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-172">In the example</span></span>
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
<span data-ttu-id="be601-173">zakres *using_alias_directive* powoduje to wprowadzenie `R` rozciąga się tylko do deklaracji elementu członkowskiego w treści przestrzeni nazw, w którym znajduje się więc `R` jest nieznany w drugim deklaracji przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="be601-174">Jednak umieszczenie *using_alias_directive* w kompilacji zawierającej jednostki powoduje, że alias staną się dostępne w obu deklaracje przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="be601-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
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

<span data-ttu-id="be601-175">Podobnie jak zwykłe elementy członkowskie, wynikające z nazwami *using_alias_directive*s są ukrywane o podobnej nazwie elementów członkowskich w zagnieżdżonych zakresów.</span><span class="sxs-lookup"><span data-stu-id="be601-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="be601-176">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="be601-177">Odwołanie do `R.A` w deklaracji `B` powoduje błąd w czasie kompilacji, ponieważ `R` odwołuje się do `N3.R`, a nie `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="be601-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="be601-178">Kolejność, w której *using_alias_directive*s są zapisywane, nie ma znaczenia i rozpoznawanie *namespace_or_type_name* odwołuje *using_alias_directive*nie dotyczy *using_alias_directive* sam lub przez inne *using_directive*s do kompilacji zawierającego treści jednostki lub przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="be601-179">Innymi słowy *namespace_or_type_name* z *using_alias_directive* nie zostanie rozwiązany tak, jakby natychmiast zawierający treść jednostki lub przestrzeni nazw kompilacji ma nie *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="be601-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="be601-180">A *using_alias_directive* można jednak wpływ *extern_alias_directive*s do kompilacji zawierającego treści jednostki lub przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="be601-181">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-181">In the example</span></span>
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
<span data-ttu-id="be601-182">ostatni *using_alias_directive* powoduje błąd w czasie kompilacji, ponieważ nie ma wpływu pierwszy *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="be601-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="be601-183">Pierwszy *using_alias_directive* nie powoduje błąd, ponieważ zakres alias zewnętrzny `E` obejmuje *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="be601-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="be601-184">A *using_alias_directive* można utworzyć aliasu przestrzeni nazw lub typ, włącznie z przestrzenią nazw, w którym pojawia się i wszelkie przestrzeń nazw lub typ zagnieżdżony w tej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="be601-185">Uzyskiwanie dostępu do przestrzeni nazw lub typ za pomocą aliasu daje ten sam wynik, jak uzyskiwanie dostępu do tej przestrzeni nazw lub typ za pośrednictwem jego zadeklarowana nazwa.</span><span class="sxs-lookup"><span data-stu-id="be601-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="be601-186">Na przykład biorąc pod uwagę</span><span class="sxs-lookup"><span data-stu-id="be601-186">For example, given</span></span>
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
<span data-ttu-id="be601-187">nazwy `N1.N2.A`, `R1.N2.A`, i `R2.A` są równoważne i wszystkie odwołują się do klasy, w których w pełni kwalifikowana nazwa to `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="be601-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="be601-188">Aliasy użycia może nazwę zamknięte skonstruowanego typu, ale nie nazwa deklaracji typu niepowiązanego ogólny bez podawania argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="be601-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="be601-189">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-189">For example:</span></span>
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

### <a name="using-namespace-directives"></a><span data-ttu-id="be601-190">Za pomocą dyrektywy przestrzeni nazw</span><span class="sxs-lookup"><span data-stu-id="be601-190">Using namespace directives</span></span>

<span data-ttu-id="be601-191">A *using_namespace_directive* importuje typy zawartych w przestrzeni nazw do natychmiastowo otaczającą kompilacji zespołu lub przestrzeni nazw treści, umożliwiając identyfikator każdego typu bez kwalifikacji można używać.</span><span class="sxs-lookup"><span data-stu-id="be601-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="be601-192">W deklaracji elementu członkowskiego w kompilacji zespołu lub przestrzeni nazw treść, która zawiera *using_namespace_directive*, typy zawarte w danej przestrzeni nazw może znajdować się bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="be601-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="be601-193">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-193">For example:</span></span>
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

<span data-ttu-id="be601-194">Powyżej, w ramach deklaracji elementu członkowskiego w `N3` przestrzeni nazw, typów elementów członkowskich `N1.N2` są bezpośrednio dostępne i klasy w związku z tym `N3.B` pochodzi od klasy `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="be601-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="be601-195">A *using_namespace_directive* importuje typy zawarte w danej przestrzeni nazw, ale specjalnie nie są importowane zagnieżdżone przestrzenie nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="be601-196">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-196">In the example</span></span>
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
<span data-ttu-id="be601-197">*using_namespace_directive* importuje typy zawarte w `N1`, ale nie przestrzenie nazw są zagnieżdżone w `N1`.</span><span class="sxs-lookup"><span data-stu-id="be601-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="be601-198">W związku z tym, odwołanie do `N2.A` w deklaracji `B` powoduje błąd w czasie kompilacji, ponieważ żaden z elementów członkowskich o nazwie `N2` znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="be601-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="be601-199">W odróżnieniu od *using_alias_directive*, *using_namespace_directive* mogą importować typy, których identyfikatory zostały już zdefiniowane w otaczającej treści jednostki lub przestrzeni nazw kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="be601-200">W efekcie nazw importowanych przez *using_namespace_directive* są ukryte przez członków o podobnej nazwie w otaczającej treści jednostki lub przestrzeni nazw kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="be601-201">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-201">For example:</span></span>
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

<span data-ttu-id="be601-202">W deklaracji elementu członkowskiego, w tym miejscu `N3` przestrzeni nazw, `A` odwołuje się do `N3.A` zamiast `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="be601-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="be601-203">Gdy więcej niż jeden obszar nazw lub typ zaimportowany przez *using_namespace_directive*s lub *using_static_directive*s w tę samą treść jednostki lub przestrzeni nazw kompilacji zawierają typy pod tą samą nazwą odwołania do tę nazwę jako *type_name* są traktowane jako niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="be601-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="be601-204">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-204">In the example</span></span>
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
<span data-ttu-id="be601-205">zarówno `N1` i `N2` zawierać składowej `A`, a ponieważ `N3` importuje zarówno odwołujące się do `A` w `N3` jest błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="be601-206">W takiej sytuacji konflikt można rozwiązać w albo za pośrednictwem kwalifikacji odwołania do `A`, lub wprowadzając *using_alias_directive* , wybiera określonego `A`.</span><span class="sxs-lookup"><span data-stu-id="be601-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="be601-207">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="be601-208">Ponadto, gdy więcej niż jeden obszar nazw lub typ zaimportowany przez *using_namespace_directive*s lub *using_static_directive*s w tę samą treść jednostki lub przestrzeni nazw kompilacji zawierają typy lub elementy członkowskie przez tej samej nazwie odwołuje się do tej nazwy, jako *simple_name* są traktowane jako niejednoznaczny.</span><span class="sxs-lookup"><span data-stu-id="be601-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="be601-209">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-209">In the example</span></span>
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
<span data-ttu-id="be601-210">`N1` zawiera element członkowski typu `A`, i `C` zawiera statycznej metody `A`, a ponieważ `N2` importuje zarówno odwołujące się do `A` jako *simple_name* jest niejednoznaczne i w czasie kompilacji Wystąpił błąd.</span><span class="sxs-lookup"><span data-stu-id="be601-210">`N1` contains a type member `A`, and `C` contains a static method `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="be601-211">Podobnie jak *using_alias_directive*, *using_namespace_directive* nie wpływa na żadnych nowych elementów członkowskich do podstawowej deklaracji przestrzeni jednostki kompilacji lub przestrzeni nazw, ale raczej ma wpływ tylko na Kompilacja zespołu lub przestrzeni nazw treść w której występuje.</span><span class="sxs-lookup"><span data-stu-id="be601-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="be601-212">*Namespace_name* odwołuje *using_namespace_directive* został rozwiązany w taki sam sposób jak *namespace_or_type_name* odwołuje  *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="be601-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="be601-213">W efekcie *using_namespace_directive*s w tę samą treść jednostki lub przestrzeni nazw kompilacji nie wpływają na siebie nawzajem i mogą być napisane w dowolnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="be601-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="be601-214">Statyczne dyrektyw Using</span><span class="sxs-lookup"><span data-stu-id="be601-214">Using static directives</span></span>

<span data-ttu-id="be601-215">A *using_static_directive* importuje typów zagnieżdżonych i statycznych elementów członkowskich znajdujących się bezpośrednio w deklaracji typu do natychmiastowo otaczającą kompilacji zespołu lub przestrzeni nazw treści, umożliwiając identyfikator każdego elementu członkowskiego i typu używane bez kwalifikacji.</span><span class="sxs-lookup"><span data-stu-id="be601-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="be601-216">W deklaracji elementu członkowskiego w kompilacji zespołu lub przestrzeni nazw treść, która zawiera *using_static_directive*, dostępne zagnieżdżone typy i składowe statyczne (z wyjątkiem metody rozszerzenia) znajdujących się bezpośrednio w deklaracji Podany typ można odwoływać się bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="be601-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="be601-217">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="be601-217">For example:</span></span>

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

<span data-ttu-id="be601-218">Powyżej, w ramach deklaracji elementu członkowskiego w `N2` przestrzeni nazw statycznych składowych i zagnieżdżone typy `N1.A` są bezpośrednio dostępne, a przez to metoda `N` może odwoływać się do zarówno `B` i `M` członkowie `N1.A`.</span><span class="sxs-lookup"><span data-stu-id="be601-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="be601-219">A *using_static_directive* specjalnie nie importuje metody rozszerzenia bezpośrednio jako metody statyczne, ale udostępnia je dla wywołania metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="be601-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="be601-220">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-220">In the example</span></span>

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
<span data-ttu-id="be601-221">*using_static_directive* importuje metody rozszerzenia `M` zawarte w `N1.A`, ale tylko jako metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="be601-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="be601-222">Dlatego pierwszego odwołania do `M` w treści `B.N` powoduje błąd w czasie kompilacji, ponieważ żaden z elementów członkowskich o nazwie `M` znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="be601-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="be601-223">A *using_static_directive* importuje jedynie członkowie i typy zadeklarowany bezpośrednio w danym typie, nie elementów członkowskich i typy zadeklarowane w klasach bazowych.</span><span class="sxs-lookup"><span data-stu-id="be601-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="be601-224">TODO: przykład</span><span class="sxs-lookup"><span data-stu-id="be601-224">TODO: Example</span></span>

<span data-ttu-id="be601-225">Niejednoznaczności między wieloma *using_namespace_directives* i *using_static_directives* zostały omówione w [przy użyciu dyrektywy przestrzeni nazw](namespaces.md#using-namespace-directives).</span><span class="sxs-lookup"><span data-stu-id="be601-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="be601-226">Elementy członkowskie Namespace</span><span class="sxs-lookup"><span data-stu-id="be601-226">Namespace members</span></span>

<span data-ttu-id="be601-227">A *namespace_member_declaration* jest *namespace_declaration* ([deklaracji Namespace](namespaces.md#namespace-declarations)) lub *type_declaration* () [Wpisz deklaracje](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="be601-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="be601-228">Jednostka kompilacji lub treści przestrzeni nazw może zawierać *namespace_member_declaration*s i takie deklaracje współtworzyć nowych elementów członkowskich na podstawowej deklaracji przestrzeń zawierający treść jednostki lub przestrzeni nazw kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="be601-229">Deklaracje typu</span><span class="sxs-lookup"><span data-stu-id="be601-229">Type declarations</span></span>

<span data-ttu-id="be601-230">A *type_declaration* jest *class_declaration* ([klasy deklaracje](classes.md#class-declarations)), *struct_declaration* ([— struktura deklaracje](structs.md#struct-declarations)), *interface_declaration* ([interfejsu deklaracje](interfaces.md#interface-declarations)), *enum_declaration* ([wyliczenia deklaracje](enums.md#enum-declarations)), lub *delegate_declaration* ([delegować deklaracje](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="be601-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="be601-231">A *type_declaration* może wystąpić jako deklaracji najwyższego poziomu w jednostce kompilacji lub deklaracja elementu członkowskiego, w ramach przestrzeni nazw, klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="be601-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="be601-232">Gdy deklaracji typu dla typu `T` występuje jako deklaracja najwyższego poziomu w jednostce kompilacji, w pełni kwalifikowaną nazwę nowo deklarowany typ jest po prostu `T`.</span><span class="sxs-lookup"><span data-stu-id="be601-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="be601-233">Gdy deklaracji typu dla typu `T` występuje w przestrzeni nazw, klasy lub struktury, w pełni kwalifikowaną nazwę nowo deklarowany typ jest `N.T`, gdzie `N` jest w pełni kwalifikowaną nazwą zawierającą przestrzeń nazw, klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="be601-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="be601-234">Typ zadeklarowany w klasie lub strukturze nosi nazwę typu zagnieżdżonego ([zagnieżdżone typy](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="be601-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="be601-235">Modyfikatory dostępu dozwolonych i dostęp do domyślnej deklaracji typu są zależne od kontekstu, w którym odbywa się deklaracji ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="be601-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="be601-236">Typy zadeklarowane w jednostkach kompilacji lub przestrzeni nazw mogą mieć `public` lub `internal` dostępu.</span><span class="sxs-lookup"><span data-stu-id="be601-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="be601-237">Wartość domyślna to `internal` dostępu.</span><span class="sxs-lookup"><span data-stu-id="be601-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="be601-238">Typy zadeklarowane w klasach mogą mieć `public`, `protected internal`, `protected`, `internal`, lub `private` dostępu.</span><span class="sxs-lookup"><span data-stu-id="be601-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="be601-239">Wartość domyślna to `private` dostępu.</span><span class="sxs-lookup"><span data-stu-id="be601-239">The default is `private` access.</span></span>
*  <span data-ttu-id="be601-240">Typy zadeklarowane w strukturach mogą mieć `public`, `internal`, lub `private` dostępu.</span><span class="sxs-lookup"><span data-stu-id="be601-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="be601-241">Wartość domyślna to `private` dostępu.</span><span class="sxs-lookup"><span data-stu-id="be601-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="be601-242">Kwalifikatory alias Namespace</span><span class="sxs-lookup"><span data-stu-id="be601-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="be601-243">***Kwalifikator aliasu przestrzeni nazw*** `::` umożliwia zagwarantowanie, że wyszukiwanie nazw typu nie ma wpływu na wprowadzenie nowych typów i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="be601-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="be601-244">Kwalifikator aliasu przestrzeni nazw jest zawsze wyświetlany między dwa identyfikatory określane jako identyfikatory po lewej stronie i po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="be601-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="be601-245">W odróżnieniu od zwykłych `.` kwalifikator, identyfikator po lewej stronie `::` tablicą kwalifikator tylko jako zewnętrznego lub za pomocą aliasu w górę.</span><span class="sxs-lookup"><span data-stu-id="be601-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="be601-246">A *qualified_alias_member* jest zdefiniowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="be601-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="be601-247">A *qualified_alias_member* mogą być używane jako *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) lub jako lewy operand w *member_access* ([Dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="be601-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="be601-248">A *qualified_alias_member* ma jedną z dwóch formach:</span><span class="sxs-lookup"><span data-stu-id="be601-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="be601-249">`N::I<A1, ..., Ak>`, gdzie `N` i `I` reprezentują identyfikatorów i `<A1, ..., Ak>` jest lista argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="be601-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="be601-250">(`K` jest zawsze co najmniej jeden.)</span><span class="sxs-lookup"><span data-stu-id="be601-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="be601-251">`N::I`, gdzie `N` i `I` reprezentują identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="be601-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="be601-252">(W tym przypadku `K` jest uważany za wartość zero.)</span><span class="sxs-lookup"><span data-stu-id="be601-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="be601-253">Za pomocą tej notacji, znaczenie *qualified_alias_member* jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="be601-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="be601-254">Jeśli `N` to identyfikator `global`, a następnie połączenie jest wyszukiwane na globalnej przestrzeni nazw `I`:</span><span class="sxs-lookup"><span data-stu-id="be601-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="be601-255">Jeśli globalna przestrzeń nazw zawiera przestrzeń nazw o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego obszaru nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="be601-256">W przeciwnym razie, jeśli globalna przestrzeń nazw zawiera typu nieogólnego, o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego typu.</span><span class="sxs-lookup"><span data-stu-id="be601-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="be601-257">W przeciwnym razie, jeśli globalna przestrzeń nazw zawiera typ o nazwie `I` zawierający `K` parametry typu, a następnie *qualified_alias_member* odwołuje się do tego typu skonstruowany przy użyciu argumentów danego typu.</span><span class="sxs-lookup"><span data-stu-id="be601-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="be601-258">W przeciwnym razie *qualified_alias_member* jest niezdefiniowana, i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="be601-259">W przeciwnym razie, począwszy od deklaracji przestrzeni nazw ([deklaracji Namespace](namespaces.md#namespace-declarations)) natychmiast zawierający *qualified_alias_member* (jeśli istnieje), kontynuowanie z każdego otaczającej deklarację przestrzeni nazw (jeśli istnieje), a kończąc na jednostkę kompilacji, zawierającą *qualified_alias_member*, poniższe kroki są oceniane, dopóki nie znajduje się jednostka:</span><span class="sxs-lookup"><span data-stu-id="be601-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="be601-260">Jeśli zawiera jednostki kompilacji lub deklaracji przestrzeni nazw *using_alias_directive* który kojarzy `N` z typem, a następnie *qualified_alias_member* jest niezdefiniowane i w czasie kompilacji występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="be601-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="be601-261">W przeciwnym razie, jeśli zawiera jednostki kompilacji lub deklaracji przestrzeni nazw *extern_alias_directive* lub *using_alias_directive* który kojarzy `N` z obszaru nazw, następnie:</span><span class="sxs-lookup"><span data-stu-id="be601-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="be601-262">Jeśli przestrzeń nazw skojarzony z `N` zawiera przestrzeń nazw o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego obszaru nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="be601-263">W przeciwnym razie, jeśli przestrzeń nazw skojarzony z `N` zawiera typu nieogólnego, o nazwie `I` i `K` wynosi zero, a następnie *qualified_alias_member* odwołuje się do tego typu.</span><span class="sxs-lookup"><span data-stu-id="be601-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="be601-264">W przeciwnym razie, jeśli przestrzeń nazw skojarzony z `N` zawiera typ o nazwie `I` zawierający `K` parametry typu, a następnie *qualified_alias_member* odwołuje się do tego typu skonstruowany przy użyciu danego typu argumenty.</span><span class="sxs-lookup"><span data-stu-id="be601-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="be601-265">W przeciwnym razie *qualified_alias_member* jest niezdefiniowana, i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="be601-266">W przeciwnym razie *qualified_alias_member* jest niezdefiniowana, i występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="be601-267">Należy pamiętać, że kwalifikator aliasu przestrzeni nazw przy użyciu aliasu, który odwołuje się do typu powoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="be601-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="be601-268">Należy również zauważyć, że jeśli identyfikator `N` jest `global`, wówczas wyszukiwania odbywa się w globalnej przestrzeni nazw, nawet jeśli dostępny jest za pomocą aliasu kojarzenie `global` przy użyciu typu lub przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="be601-269">Unikatowość nazw aliasów</span><span class="sxs-lookup"><span data-stu-id="be601-269">Uniqueness of aliases</span></span>

<span data-ttu-id="be601-270">Treść jednostki i przestrzeni nazw każdej kompilacji ma miejsce na oddzielnych deklaracji aliasy extern i aliasy użycia.</span><span class="sxs-lookup"><span data-stu-id="be601-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="be601-271">W związku z tym, Nazwa alias zewnętrzny lub za pomocą aliasu muszą być unikatowe w zbiorze aliasów extern i aliasy użycia zadeklarowanych w natychmiast zawierający treść jednostki lub przestrzeni nazw kompilacji, aliasu jest mogą mieć taką samą nazwę jak typ lub przestrzeń nazw tak długo, jak i t jest używany tylko z `::` kwalifikator.</span><span class="sxs-lookup"><span data-stu-id="be601-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="be601-272">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="be601-272">In the example</span></span>
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
<span data-ttu-id="be601-273">Nazwa `A` ma dwa znaczeń w drugim treści przestrzeni nazw, ponieważ zarówno klasy `A` i za pomocą aliasu `A` znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="be601-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="be601-274">Z tego powodu użycie `A` w kwalifikowanej nazwie `A.Stream` jest niejednoznaczne i powoduje błąd kompilacji występuje.</span><span class="sxs-lookup"><span data-stu-id="be601-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="be601-275">Jednak użycie `A` z `::` kwalifikatora nie jest to błąd, ponieważ `A` wyszukiwane są tylko jako alias przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="be601-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
