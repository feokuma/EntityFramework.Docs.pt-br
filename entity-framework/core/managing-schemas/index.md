---
title: Gerenciar Esquemas de Banco de Dados – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655643"
---
# <a name="managing-database-schemas"></a>Gerenciar Esquemas de Banco de Dados

O EF Core oferece duas maneiras principais de manter seu esquema de banco de dados e modelo do EF Core em sincronia. Para escolher entre as duas, decida se seu modelo do EF Core ou o esquema de banco de dados é a fonte da verdade.

Se quiser que seu modelo do EF Core seja a fonte da verdade, use [Migrações][1]. Uma vez que você pode fazer alterações ao seu modelo do EF Core, essa abordagem aplica incrementalmente as alterações de esquema correspondentes ao banco de dados de modo que permaneçam compatíveis com o modelo do EF Core.

Use [Engenharia Reversa][2] se quiser que o esquema de banco de dados seja a fonte da verdade. Essa abordagem permite que você faça o scaffold de um DbContext e das classes de tipo de entidade realizando engenharia reversa do esquema de banco de dados para um modelo do EF Core.

> [!NOTE]
> A opção [criar e remover APIs][3] também pode criar o esquema de banco de dados usando seu modelo do EF Core. No entanto, eles são principalmente para teste, criação de protótipos e outros cenários em que remover o banco de dados é aceitável.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
