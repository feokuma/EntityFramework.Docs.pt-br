---
title: Cliente versus Avaliação do Servidor – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921708"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="b6364-102">Cliente versus Avaliação do Servidor</span><span class="sxs-lookup"><span data-stu-id="b6364-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="b6364-103">O Entity Framework Core permite que partes da consulta sejam avaliadas no cliente e que partes dela sejam enviadas por push para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b6364-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="b6364-104">Cabe ao provedor do banco de dados determinar quais partes da consulta serão avaliadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b6364-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="b6364-105">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="b6364-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="b6364-106">Avaliação do cliente</span><span class="sxs-lookup"><span data-stu-id="b6364-106">Client evaluation</span></span>

<span data-ttu-id="b6364-107">No exemplo a seguir, um método auxiliar é usado para padronizar URLs para blogs que são retornados de um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6364-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="b6364-108">Como o provedor do SQL Server não tem nenhuma informação sobre como esse método é implementado, não é possível traduzi-lo para SQL.</span><span class="sxs-lookup"><span data-stu-id="b6364-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="b6364-109">Todos os outros aspectos da consulta são avaliados no banco de dados, mas a transmissão do `URL` retornado por esse método é executada no cliente.</span><span class="sxs-lookup"><span data-stu-id="b6364-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="b6364-110">Problemas de desempenho de avaliação do cliente</span><span class="sxs-lookup"><span data-stu-id="b6364-110">Client evaluation performance issues</span></span>

<span data-ttu-id="b6364-111">Embora a avaliação do cliente possa ser muito útil em alguns casos, ela pode resultar em um desempenho ruim.</span><span class="sxs-lookup"><span data-stu-id="b6364-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="b6364-112">Considere a consulta a seguir, em que o método auxiliar agora é usado em um filtro.</span><span class="sxs-lookup"><span data-stu-id="b6364-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="b6364-113">Como isso não pode ser executado no banco de dados, todos os dados são extraídos para a memória e o filtro é aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="b6364-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="b6364-114">Dependendo da quantidade de dados e de quantos desses dados são filtrados, isso pode resultar em uma perda de desempenho.</span><span class="sxs-lookup"><span data-stu-id="b6364-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="b6364-115">Registro em log de avaliação do cliente</span><span class="sxs-lookup"><span data-stu-id="b6364-115">Client evaluation logging</span></span>

<span data-ttu-id="b6364-116">Por padrão, o EF Core registrará um alerta quando a avaliação do cliente for realizada.</span><span class="sxs-lookup"><span data-stu-id="b6364-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="b6364-117">Confira [Registro em log](../miscellaneous/logging.md) para saber mais sobre como visualizar as saídas do registro em log.</span><span class="sxs-lookup"><span data-stu-id="b6364-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="b6364-118">Comportamento opcional: gerar uma exceção para avaliação do cliente</span><span class="sxs-lookup"><span data-stu-id="b6364-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="b6364-119">Quando a avaliação do cliente ocorrer, você pode alterar o comportamento para gerar ou não agir.</span><span class="sxs-lookup"><span data-stu-id="b6364-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="b6364-120">Isso ocorre ao configurar as opções para seu contexto, geralmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6364-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
