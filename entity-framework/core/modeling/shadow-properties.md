---
title: Propriedades de sombra – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 4029539f3642f539a427f5901577d4df96c00f30
ms.sourcegitcommit: 119058fefd7f35952048f783ada68be9aa612256
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749697"
---
# <a name="shadow-properties"></a><span data-ttu-id="b6a8c-102">Propriedades de sombra</span><span class="sxs-lookup"><span data-stu-id="b6a8c-102">Shadow Properties</span></span>

<span data-ttu-id="b6a8c-103">Propriedades de sombra são propriedades que não estão definidas na sua classe de entidade do .NET, mas são definidas para esse tipo de entidade no modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="b6a8c-104">O valor e o estado dessas propriedades é mantida puramente no Rastreador de alteração.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="b6a8c-105">Propriedades de sombra são úteis quando há dados no banco de dados que não deve ser exposto nos tipos de entidade mapeada.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="b6a8c-106">Eles geralmente são usados para propriedades de chave estrangeira, onde a relação entre duas entidades é representada por um valor de chave estrangeiro no banco de dados, mas a relação é gerenciada nos tipos de entidade usando as propriedades de navegação entre os tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="b6a8c-107">Os valores de propriedade de sombra podem ser obtidos e alterados por meio de `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="b6a8c-108">Propriedades de sombra podem ser referenciadas em consultas LINQ por meio de `EF.Property` método estático.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="b6a8c-109">Convenções</span><span class="sxs-lookup"><span data-stu-id="b6a8c-109">Conventions</span></span>

<span data-ttu-id="b6a8c-110">Propriedades de sombra podem ser criadas por convenção, quando uma relação é descoberta, mas nenhuma propriedade de chave estrangeira for encontrada na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="b6a8c-111">Nesse caso, uma propriedade de chave estrangeira de sombra será introduzida.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="b6a8c-112">A propriedade de chave estrangeira de sombra será denominada `<navigation property name><principal key property name>` (a navegação na entidade dependente, que aponta para a entidade principal, é usada para a nomeação).</span><span class="sxs-lookup"><span data-stu-id="b6a8c-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="b6a8c-113">Se o nome da propriedade de chave de entidade de segurança inclui o nome da propriedade de navegação, o nome será apenas `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="b6a8c-114">Se não houver nenhuma propriedade de navegação na entidade dependente, o nome do tipo de entidade de segurança é usado em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="b6a8c-115">Por exemplo, a listagem de código a seguir resultará em uma `BlogId` que está sendo introduzida para a propriedade shadow o `Post` entidade.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="b6a8c-116">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="b6a8c-116">Data Annotations</span></span>

<span data-ttu-id="b6a8c-117">Propriedades de sombra não podem ser criadas com anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b6a8c-118">API fluente</span><span class="sxs-lookup"><span data-stu-id="b6a8c-118">Fluent API</span></span>

<span data-ttu-id="b6a8c-119">Você pode usar a API Fluent para configurar as propriedades de sombra.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="b6a8c-120">Depois de ter chamado a sobrecarga de cadeia de caracteres de `Property` é possível encadear qualquer uma das chamadas de configuração que você faria para outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="b6a8c-121">Se o nome fornecido para o `Property` método corresponde ao nome de uma propriedade existente (uma propriedade de sombra ou um definido na classe de entidade) e, em seguida, o código irá configurar essa propriedade existente em vez de introduzir uma nova propriedade de sombra.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
