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
# <a name="client-vs-server-evaluation"></a>Cliente versus Avaliação do Servidor

O Entity Framework Core permite que partes da consulta sejam avaliadas no cliente e que partes dela sejam enviadas por push para o banco de dados. Cabe ao provedor do banco de dados determinar quais partes da consulta serão avaliadas no banco de dados.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="client-evaluation"></a>Avaliação do cliente

No exemplo a seguir, um método auxiliar é usado para padronizar URLs para blogs que são retornados de um banco de dados do SQL Server. Como o provedor do SQL Server não tem nenhuma informação sobre como esse método é implementado, não é possível traduzi-lo para SQL. Todos os outros aspectos da consulta são avaliados no banco de dados, mas a transmissão do `URL` retornado por esse método é executada no cliente.

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

## <a name="client-evaluation-performance-issues"></a>Problemas de desempenho de avaliação do cliente

Embora a avaliação do cliente possa ser muito útil em alguns casos, ela pode resultar em um desempenho ruim. Considere a consulta a seguir, em que o método auxiliar agora é usado em um filtro. Como isso não pode ser executado no banco de dados, todos os dados são extraídos para a memória e o filtro é aplicado no cliente. Dependendo da quantidade de dados e de quantos desses dados são filtrados, isso pode resultar em uma perda de desempenho.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Registro em log de avaliação do cliente

Por padrão, o EF Core registrará um alerta quando a avaliação do cliente for realizada. Confira [Registro em log](../miscellaneous/logging.md) para saber mais sobre como visualizar as saídas do registro em log. 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Comportamento opcional: gerar uma exceção para avaliação do cliente

Quando a avaliação do cliente ocorrer, você pode alterar o comportamento para gerar ou não agir. Isso ocorre ao configurar as opções para seu contexto, geralmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
