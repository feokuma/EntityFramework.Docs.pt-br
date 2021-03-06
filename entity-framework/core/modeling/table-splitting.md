---
title: Divisão de tabela-EF Core
description: Como configurar a divisão de tabela usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824561"
---
# <a name="table-splitting"></a>Divisão de tabela

>[!NOTE]
> Esse recurso é novo no EF Core 2,0.

EF Core permite mapear duas ou mais entidades para uma única linha. Isso é chamado de _divisão de tabela_ ou compartilhamento de _tabela_.

## <a name="configuration"></a>Configuração do

Para usar a divisão de tabela, os tipos de entidade precisam ser mapeados para a mesma tabela, ter as chaves primárias mapeadas para as mesmas colunas e pelo menos uma relação configurada entre a chave primária de um tipo de entidade e outra na mesma tabela.

Um cenário comum para divisão de tabela é usar apenas um subconjunto das colunas na tabela para maior desempenho ou encapsulamento.

Neste exemplo `Order` representa um subconjunto de `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Além da configuração necessária, chamamos `Property(o => o.Status).HasColumnName("Status")` para mapear `DetailedOrder.Status` para a mesma coluna que `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obter mais contexto.

## <a name="usage"></a>Medição de

Salvar e consultar entidades usando divisão de tabela é feito da mesma maneira que outras entidades. E, a partir do EF Core 3,0, a referência de entidade dependente pode ser `null`. Se todas as colunas usadas pela entidade dependente forem `NULL` é o banco de dados, nenhuma instância para ela será criada quando consultada. Isso também aconteceria que todas as propriedades fossem opcionais e definidas como `null`, o que pode não ser esperado.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokens de simultaneidade

Se qualquer um dos tipos de entidade que compartilham uma tabela tiver um token de simultaneidade, ele deverá ser incluído em todos os outros tipos de entidade para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela for atualizada.

Para evitar expô-lo ao código de consumo, é possível criar um no estado de sombra.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
