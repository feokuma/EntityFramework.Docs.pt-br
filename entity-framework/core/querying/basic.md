---
title: Consultas básicas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 49daa0d37175244617993cc6e911cbd59d27079f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921747"
---
# <a name="basic-queries"></a>Consultas básicas

Saiba como carregar entidades do banco de dados usando LINQ (Consulta Integrada à Linguagem).

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="101-linq-samples"></a>101 exemplos do LINQ

Esta página mostra alguns exemplos para realizar tarefas comuns com o Entity Framework Core. Para ter acesso a um conjunto amplo de exemplos mostrando o que é possível com o LINQ, confira [101 exemplos do LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

## <a name="loading-all-data"></a>Como carregar todos os dados

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a>Como carregar uma única entidade

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a>Filtragem

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
