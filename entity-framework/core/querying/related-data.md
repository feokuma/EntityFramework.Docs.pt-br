---
title: Como carregar dados relacionados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 590d16902329ffb3fff8026f8dfdcfc887f6dea3
ms.sourcegitcommit: eefcab31142f61a7aaeac03ea90dcd39f158b8b8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873198"
---
# <a name="loading-related-data"></a><span data-ttu-id="2df70-102">Como carregar dados relacionados</span><span class="sxs-lookup"><span data-stu-id="2df70-102">Loading Related Data</span></span>

<span data-ttu-id="2df70-103">O Entity Framework Core permite que você use as propriedades de navegação em seu modelo para carregar as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="2df70-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="2df70-104">Há três padrões de O/RM comuns usados para carregar os dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="2df70-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="2df70-105">**Carregamento adiantado** significa que os dados relacionados são carregados do banco de dados como parte da consulta inicial.</span><span class="sxs-lookup"><span data-stu-id="2df70-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="2df70-106">**Carregamento explícito** significa que os dados relacionados são explicitamente carregados do banco de dados em um momento posterior.</span><span class="sxs-lookup"><span data-stu-id="2df70-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="2df70-107">**Carregamento lento** significa que os dados relacionados são carregados de modo transparente do banco de dados quando a propriedade de navegação é acessada.</span><span class="sxs-lookup"><span data-stu-id="2df70-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="2df70-108">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="2df70-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="2df70-109">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="2df70-109">Eager loading</span></span>

<span data-ttu-id="2df70-110">Você pode usar o método `Include` para especificar os dados relacionados a serem incluídos nos resultados da consulta.</span><span class="sxs-lookup"><span data-stu-id="2df70-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="2df70-111">No exemplo a seguir, os blogs que são retornados nos resultados terão suas propriedades `Posts` preenchidas com as postagens relacionadas.</span><span class="sxs-lookup"><span data-stu-id="2df70-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="2df70-112">O Entity Framework Core corrigirá automaticamente as propriedades de navegação para outras entidades que forem carregadas anteriormente na instância do contexto.</span><span class="sxs-lookup"><span data-stu-id="2df70-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="2df70-113">Então, mesmo se você não incluir de forma explícita os dados para a propriedade de navegação, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2df70-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="2df70-114">Você pode incluir dados relacionados de várias relações em uma única consulta.</span><span class="sxs-lookup"><span data-stu-id="2df70-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="2df70-115">Incluindo vários níveis</span><span class="sxs-lookup"><span data-stu-id="2df70-115">Including multiple levels</span></span>

<span data-ttu-id="2df70-116">Você pode fazer uma busca detalhada por meio de relações para incluir vários níveis de dados relacionados usando o método `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="2df70-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="2df70-117">O exemplo a seguir carrega todos os blogs, suas postagens relacionadas e o autor de cada postagem.</span><span class="sxs-lookup"><span data-stu-id="2df70-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="2df70-118">As versões atuais do Visual Studio oferecem opções de conclusão de código incorreto e pode fazer as expressões corretas serem sinalizadas com erros de sintaxe ao usar o método `ThenInclude` após uma propriedade de navegação da coleção.</span><span class="sxs-lookup"><span data-stu-id="2df70-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="2df70-119">Este é um sintoma de um bug do IntelliSense controlado no https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="2df70-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="2df70-120">É seguro ignorar esses erros de sintaxe artificiais desde que o código esteja correto e possa ser compilado com êxito.</span><span class="sxs-lookup"><span data-stu-id="2df70-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="2df70-121">É possível encadear chamadas múltiplas a `ThenInclude` para continuar incluindo outros níveis de dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="2df70-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="2df70-122">Você pode combinar tudo isso para incluir dados relacionados de vários níveis e várias raízes na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="2df70-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="2df70-123">Você talvez queira incluir várias entidades relacionadas para uma das entidades que está sendo incluída.</span><span class="sxs-lookup"><span data-stu-id="2df70-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="2df70-124">Por exemplo, ao consultar os `Blogs`, você inclui os `Posts` e, em seguida, o `Author` e as `Tags` dos `Posts`.</span><span class="sxs-lookup"><span data-stu-id="2df70-124">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="2df70-125">Para fazer isso, você precisará especificar cada uma para incluir o caminho a partir da raiz.</span><span class="sxs-lookup"><span data-stu-id="2df70-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="2df70-126">Por exemplo, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="2df70-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="2df70-127">Isso não significa que você obterá junções redundantes. Na maioria dos casos, o EF consolidará as junções ao gerar o SQL.</span><span class="sxs-lookup"><span data-stu-id="2df70-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="2df70-128">Incluir para tipos derivados</span><span class="sxs-lookup"><span data-stu-id="2df70-128">Include on derived types</span></span>

<span data-ttu-id="2df70-129">Você pode incluir dados relacionados de navegações definidas apenas em um tipo derivado usando `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="2df70-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="2df70-130">Com o seguinte modelo:</span><span class="sxs-lookup"><span data-stu-id="2df70-130">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="2df70-131">Conteúdo da navegação em `School` de todas as pessoas que são alunos pode ser carregada de maneira adiantada usando um número de padrões:</span><span class="sxs-lookup"><span data-stu-id="2df70-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="2df70-132">usando conversão</span><span class="sxs-lookup"><span data-stu-id="2df70-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="2df70-133">usando o operador `as`</span><span class="sxs-lookup"><span data-stu-id="2df70-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="2df70-134">usando a sobrecarga de `Include` que usa o parâmetro de tipo `string`</span><span class="sxs-lookup"><span data-stu-id="2df70-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="2df70-135">Ignorado inclui</span><span class="sxs-lookup"><span data-stu-id="2df70-135">Ignored includes</span></span>

<span data-ttu-id="2df70-136">Se você alterar a consulta para que ela não retorne instâncias do tipo de entidade com o qual a consulta é iniciada, os operadores de inclusão serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="2df70-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="2df70-137">No exemplo a seguir, os operadores de inclusão se baseiam no `Blog`, mas, em seguida, o operador `Select` é usado para alterar a consulta para retornar um tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="2df70-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="2df70-138">Nesse caso, os operadores de inclusão não têm efeito.</span><span class="sxs-lookup"><span data-stu-id="2df70-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="2df70-139">Por padrão, o núcleo de EF registrará um aviso quando operadores de inclusão forem ignorados.</span><span class="sxs-lookup"><span data-stu-id="2df70-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="2df70-140">Confira [Registro em log](../miscellaneous/logging.md) para saber mais sobre como visualizar as saídas do registro em log.</span><span class="sxs-lookup"><span data-stu-id="2df70-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="2df70-141">Você pode alterar o comportamento quando um operador de inclusão for ignorado para gerar ou não agir.</span><span class="sxs-lookup"><span data-stu-id="2df70-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="2df70-142">Isso ocorre ao configurar as opções para seu contexto, geralmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2df70-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="2df70-143">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="2df70-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="2df70-144">Essa funcionalidade foi introduzida no EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="2df70-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="2df70-145">Você pode carregar explicitamente uma propriedade de navegação pela API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="2df70-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="2df70-146">Você também pode carregar explicitamente uma propriedade de navegação executando uma consulta separada que retorna as entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="2df70-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="2df70-147">Se o controle de alterações estiver habilitado, ao carregar uma entidade, o EF Core automaticamente definirá as propriedades de navegação da entidade recém-carregada para se referirem a alguma entidade já carregada e definir as propriedades de navegação das entidades já carregadas para se referirem à entidade recém-carregada.</span><span class="sxs-lookup"><span data-stu-id="2df70-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="2df70-148">Como consultar entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="2df70-148">Querying related entities</span></span>

<span data-ttu-id="2df70-149">Você também pode obter uma consulta LINQ que representa o conteúdo de uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="2df70-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="2df70-150">Isso permite que você faça coisas como executar um operador de agregação sobre as entidades relacionadas sem carregá-las na memória.</span><span class="sxs-lookup"><span data-stu-id="2df70-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="2df70-151">Você também pode filtrar quais entidades relacionadas são carregadas na memória.</span><span class="sxs-lookup"><span data-stu-id="2df70-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="2df70-152">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="2df70-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="2df70-153">Essa funcionalidade foi introduzida no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2df70-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="2df70-154">A maneira mais simples para usar o carregamento lento é instalando o pacote [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) e habilitá-lo com uma chamada para `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="2df70-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="2df70-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2df70-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="2df70-156">Ou ao usar AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="2df70-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="2df70-157">O EF Core, em seguida, habilitará o carregamento lento para qualquer propriedade de navegação que pode ser substituída – ou seja, deverá ser `virtual` e em uma classe que pode ser herdada.</span><span class="sxs-lookup"><span data-stu-id="2df70-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="2df70-158">Por exemplo, nas entidades a seguir, as propriedades de navegação `Post.Blog` e `Blog.Posts` serão de carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="2df70-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="2df70-159">Carregamento lento sem proxies</span><span class="sxs-lookup"><span data-stu-id="2df70-159">Lazy loading without proxies</span></span>

<span data-ttu-id="2df70-160">Os proxies de carregamento lento funcionam inserindo o serviço `ILazyLoader` em uma entidade, conforme descrito em [Construtores de tipo de entidade](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="2df70-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="2df70-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2df70-161">For example:</span></span>
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="2df70-162">Isso não exige que os tipos de entidade sejam herdados de ou as propriedades de navegação sejam virtuais e permitam que as instâncias da entidade criadas com `new` sejam carregados de maneira lenta assim que forem conectadas a um contexto.</span><span class="sxs-lookup"><span data-stu-id="2df70-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="2df70-163">No entanto, isso exige referência ao serviço `ILazyLoader`, que é definido no pacote [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/).</span><span class="sxs-lookup"><span data-stu-id="2df70-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="2df70-164">Este pacote contém um conjunto mínimo de tipos, portanto, há pouco impacto em depender dele.</span><span class="sxs-lookup"><span data-stu-id="2df70-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="2df70-165">No entanto, para evitar completamente a dependência de todos os pacotes do EF Core em tipos de entidade, é possível injetar o método `ILazyLoader.Load` como um delegado.</span><span class="sxs-lookup"><span data-stu-id="2df70-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="2df70-166">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2df70-166">For example:</span></span>
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="2df70-167">O código acima usa um método de extensão `Load` para deixar o representante um pouco mais limpo:</span><span class="sxs-lookup"><span data-stu-id="2df70-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="2df70-168">O parâmetro de construtor para o representante de carregamento lento deve ser chamado de "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="2df70-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="2df70-169">A configuração para usar um nome diferente é planejada para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="2df70-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="2df70-170">Serialização e dados relacionados</span><span class="sxs-lookup"><span data-stu-id="2df70-170">Related data and serialization</span></span>

<span data-ttu-id="2df70-171">Como o EF Core automaticamente corrigirá as propriedades de navegação, você poderá acabar com ciclos em seu gráfico de objeto.</span><span class="sxs-lookup"><span data-stu-id="2df70-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="2df70-172">Por exemplo, o carregamento de um blog e suas postagens relacionadas resultará em um objeto de blog que referencia uma coleção de postagens.</span><span class="sxs-lookup"><span data-stu-id="2df70-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="2df70-173">Cada uma dessas postagens terá uma referência de volta para o blog.</span><span class="sxs-lookup"><span data-stu-id="2df70-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="2df70-174">Algumas estruturas de serialização não permitem esses ciclos.</span><span class="sxs-lookup"><span data-stu-id="2df70-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="2df70-175">Por exemplo, Json.NET gerará a exceção a seguir se for encontrado um ciclo.</span><span class="sxs-lookup"><span data-stu-id="2df70-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="2df70-176">Newtonsoft.Json.JsonSerializationException: loop autorreferenciado detectado para a propriedade 'Blog' com o tipo 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="2df70-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="2df70-177">Se você estiver usando o ASP.NET Core, poderá configurar o Json.NET para ignorar os ciclos que encontrar no gráfico de objeto.</span><span class="sxs-lookup"><span data-stu-id="2df70-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="2df70-178">Isso é feito no método `ConfigureServices(...)` no `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="2df70-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

<span data-ttu-id="2df70-179">Outra alternativa é decorar uma das propriedades de navegação com o atributo `[JsonIgnore]`, que instrui o Json.NET para não percorrer essa propriedade de navegação durante a serialização.</span><span class="sxs-lookup"><span data-stu-id="2df70-179">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
