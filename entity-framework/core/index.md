---
title: Visão geral do Entity Framework Core – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e736251753134b716e64f24f6c517ed9f66a7db4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181323"
---
# <a name="entity-framework-core"></a>Entity Framework Core

O EF (Entity Framework) Core é uma versão leve, extensível, de [software livre](https://github.com/aspnet/EntityFrameworkCore) e multiplataforma da popular tecnologia de acesso a dados do Entity Framework.

O EF Core pode servir como um ORM (Mapeador de Objeto Relacional), permitindo que os desenvolvedores de .NET trabalhem com um banco de dados usando objetos do .NET e eliminando a necessidade de grande parte do código de acesso aos dados que eles geralmente precisam escrever.

O EF Core é compatível com vários mecanismos de banco de dados, consulte detalhes em [Provedores de Banco de Dados](providers/index.md).

## <a name="the-model"></a>O modelo

Com o EF Core, o acesso a dados é executado usando um modelo. Um modelo é composto por classes de entidade e um objeto de contexto que representa uma sessão com o banco de dados, o que permite consultar e salve dados. Consulte [Criar um modelo](modeling/index.md) para saber mais.

Você pode gerar um modelo de um banco de dados existente, codificar manualmente um modelo para coincidir com o banco de dados ou usar [migrações da classe EF](managing-schemas/migrations/index.md) para criar um banco de dados do modelo e, em seguida, desenvolvê-lo à medida que o modelo for alterado com o tempo.

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(
                @"Server=(localdb)\mssqllocaldb;Database=Blogging;Integrated Security=True");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

## <a name="querying"></a>Consultar

Instâncias de suas classes de entidade são recuperadas do banco de dados usando a LINQ (Consulta Integrada à Linguagem). Consulte [Consultar dados](querying/index.md) para saber mais.

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Salvar Dados

Dados são criados, excluídos e modificados no banco de dados usando as instâncias de suas classes de entidade. Consulte [Salvar dados](saving/index.md) para saber mais.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a>Próximas etapas

Para ver tutoriais introdutórios, confira [Introdução ao Entity Framework Core](get-started/index.md).

