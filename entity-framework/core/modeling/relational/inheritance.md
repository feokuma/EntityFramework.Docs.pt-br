---
title: Herança (banco de dados relacional) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2aaceb05bbc1b0eb5c116b3dc1fb33c90c115a70
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419673"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="827d8-102">Herança (banco de dados relacional)</span><span class="sxs-lookup"><span data-stu-id="827d8-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="827d8-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="827d8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="827d8-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="827d8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="827d8-105">Herança no modelo do EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="827d8-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="827d8-106">Atualmente, o tabela por hierarquia (TPH) padrão somente é implementado no EF Core.</span><span class="sxs-lookup"><span data-stu-id="827d8-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="827d8-107">Outros padrões comuns, como a tabela por tipo (TPT) e tabela-por--tipo concreto (TPC) ainda não estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="827d8-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="827d8-108">Convenções</span><span class="sxs-lookup"><span data-stu-id="827d8-108">Conventions</span></span>

<span data-ttu-id="827d8-109">Por convenção, a herança será mapeada usando o padrão de tabela por hierarquia (TPH).</span><span class="sxs-lookup"><span data-stu-id="827d8-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="827d8-110">TPH usa uma única tabela para armazenar os dados para todos os tipos na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="827d8-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="827d8-111">Uma coluna de discriminador é usada para identificar qual tipo cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="827d8-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="827d8-112">O EF Core só irá configurar a herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo (consulte [herança](../inheritance.md) para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="827d8-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="827d8-113">Abaixo está um exemplo que mostra um cenário simples de herança e os dados armazenados em uma tabela de banco de dados relacional usando o padrão TPH.</span><span class="sxs-lookup"><span data-stu-id="827d8-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="827d8-114">O *discriminador* coluna identifica qual tipo de *Blog* é armazenada em cada linha.</span><span class="sxs-lookup"><span data-stu-id="827d8-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![imagem](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="827d8-116">Colmmns de banco de dados são feitas automaticamente que permite valor nulo, conforme necessário ao usar o mapeamento TPH.</span><span class="sxs-lookup"><span data-stu-id="827d8-116">Database colmmns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="827d8-117">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="827d8-117">Data Annotations</span></span>

<span data-ttu-id="827d8-118">É possível usar as anotações de dados para configurar a herança.</span><span class="sxs-lookup"><span data-stu-id="827d8-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="827d8-119">API fluente</span><span class="sxs-lookup"><span data-stu-id="827d8-119">Fluent API</span></span>

<span data-ttu-id="827d8-120">Você pode usar a API Fluent para configurar o nome e tipo de coluna de discriminador e os valores que são usados para identificar cada tipo na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="827d8-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="827d8-121">Configurando a propriedade de discriminador</span><span class="sxs-lookup"><span data-stu-id="827d8-121">Configuring the discriminator property</span></span>

<span data-ttu-id="827d8-122">Nos exemplos acima, o discriminador é criado como uma [propriedade de sombra](xref:core/modeling/shadow-properties) sobre a entidade básica da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="827d8-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="827d8-123">Uma vez que ele é uma propriedade no modelo, ele pode ser configurado assim como outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="827d8-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="827d8-124">Por exemplo, para definir o tamanho máximo quando estiver sendo usado o padrão, o discriminador por convenção:</span><span class="sxs-lookup"><span data-stu-id="827d8-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="827d8-125">O discriminador também pode ser mapeado para uma propriedade CLR real em sua entidade.</span><span class="sxs-lookup"><span data-stu-id="827d8-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="827d8-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="827d8-126">For example:</span></span>
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="827d8-127">Combinar essas duas coisas é possível mapear o discriminador para uma propriedade real tanto configurá-lo:</span><span class="sxs-lookup"><span data-stu-id="827d8-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
