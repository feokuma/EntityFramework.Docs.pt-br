---
title: Provedor de Banco de Dados do Microsoft SQL Server – EF Core
description: Documentação do provedor de banco de dados que permite que o Entity Framework Core seja usado com o Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 18a69789ff4ae013c1d60bb6d34ca5c27ee285c2
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824777"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Provedor de Banco de Dados EF Core do Microsoft SQL Server

Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Microsoft SQL Server (incluindo o Banco de Dados SQL do Azure). O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar o

Instale o [pacote NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Desde a versão 3.0.0, o provedor faz referência a Microsoft.Data.SqlClient (as versões anteriores dependem de System.Data.SqlClient). Se o seu projeto usa uma dependência direta no SqlClient, certifique-se de que ele faça referência ao pacote Microsoft.Data.SqlClient.

## <a name="supported-database-engines"></a>Mecanismos de banco de dados compatíveis

* Microsoft SQL Server (2012 em diante)
