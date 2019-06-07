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
# <a name="shadow-properties"></a>Propriedades de sombra

Propriedades de sombra são propriedades que não estão definidas na sua classe de entidade do .NET, mas são definidas para esse tipo de entidade no modelo do EF Core. O valor e o estado dessas propriedades é mantida puramente no Rastreador de alteração.

Propriedades de sombra são úteis quando há dados no banco de dados que não deve ser exposto nos tipos de entidade mapeada. Eles geralmente são usados para propriedades de chave estrangeira, onde a relação entre duas entidades é representada por um valor de chave estrangeiro no banco de dados, mas a relação é gerenciada nos tipos de entidade usando as propriedades de navegação entre os tipos de entidade.

Os valores de propriedade de sombra podem ser obtidos e alterados por meio de `ChangeTracker` API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Propriedades de sombra podem ser referenciadas em consultas LINQ por meio de `EF.Property` método estático.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenções

Propriedades de sombra podem ser criadas por convenção, quando uma relação é descoberta, mas nenhuma propriedade de chave estrangeira for encontrada na classe de entidade dependente. Nesse caso, uma propriedade de chave estrangeira de sombra será introduzida. A propriedade de chave estrangeira de sombra será denominada `<navigation property name><principal key property name>` (a navegação na entidade dependente, que aponta para a entidade principal, é usada para a nomeação). Se o nome da propriedade de chave de entidade de segurança inclui o nome da propriedade de navegação, o nome será apenas `<principal key property name>`. Se não houver nenhuma propriedade de navegação na entidade dependente, o nome do tipo de entidade de segurança é usado em seu lugar.

Por exemplo, a listagem de código a seguir resultará em uma `BlogId` que está sendo introduzida para a propriedade shadow o `Post` entidade.

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

## <a name="data-annotations"></a>Anotações de dados

Propriedades de sombra não podem ser criadas com anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar as propriedades de sombra. Depois de ter chamado a sobrecarga de cadeia de caracteres de `Property` é possível encadear qualquer uma das chamadas de configuração que você faria para outras propriedades.

Se o nome fornecido para o `Property` método corresponde ao nome de uma propriedade existente (uma propriedade de sombra ou um definido na classe de entidade) e, em seguida, o código irá configurar essa propriedade existente em vez de introduzir uma nova propriedade de sombra.

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
