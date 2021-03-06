---
title: Novidades – EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2ca4915fca515b4bdbfeb77bc7b02f15ce1704b6
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197733"
---
# <a name="whats-new-in-ef-core"></a>Novidades – EF Core

## <a name="recent-releases"></a>Versões recentes

- **EF Core 3.0** (última versão estável) 
  - [Novos recursos](xref:core/what-is-new/ef-core-3.0/index) 
  - [Alterações de falha](xref:core/what-is-new/ef-core-3.0/breaking-changes) que você deve conhecer ao atualizar
- [EF Core 2.2](xref:core/what-is-new/ef-core-2.2)
- [EF Core 2.1](xref:core/what-is-new/ef-core-2.1) (última versão de suporte de longo prazo)

## <a name="product-roadmap"></a>Roteiro do produto

> [!IMPORTANT]
> Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.

### <a name="future-releases"></a>Versões futuras

- **EF Core 3.1**  
  - Em desenvolvimento ativo
  - Conterá [pequenas melhorias de desempenho, qualidade e estabilidade](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.1.0+sort%3Areactions-desc)
  - Planejada como uma versão de LTS (suporte de longo prazo)
  - Planejada atualmente para dezembro de 2019
- **EF Core "vNext"**   
  - Próxima versão principal do EF Core a ser lançada junto com o .NET 5
  - O planejamento dessa versão ainda não começou, e nenhum recurso foi anunciado.  

### <a name="schedule"></a>Agendamento

A agenda de lançamento para o EF Core está em sincronia com a [agenda de lançamento do .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="backlog"></a>Lista de pendências

O [marco da lista de pendências](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) em nosso rastreador de problemas contém problemas nos quais esperamos trabalhar algum dia ou que achamos que alguém da comunidade poderia solucionar.
Incentivamos os clientes a enviar comentários e votos sobre esses problemas.
Encorajamos os colaboradores que procuram trabalhar em qualquer um desses problemas a começar uma discussão sobre como abordá-los.

Nunca há uma garantia de que trabalharemos em um recurso específico em uma versão específica do EF Core.
Como em todos os projetos, prioridades, cronogramas da versão e recursos disponíveis do software podem mudar a qualquer momento.
Mas, se pretendemos resolver um problema em um período de tempo específico, atribuiremos este problema a um marco de lançamento em vez de um marco da lista de pendências.
Podemos mover rotineiramente os problemas entre os marcos de lançamento e a lista de pendências como parte do nosso [processo de planejamento da versão](#release-planning-process).

Provavelmente vamos fechar um problema se não estivermos planejando abordá-lo.
Mas podemos reconsiderar um problema fechado anteriormente se houver novas informações sobre ele.

### <a name="release-planning-process"></a>Processo de planejamento da versão

Frequentemente, recebemos perguntas sobre como escolhemos os recursos específicos de uma versão.
É certo que nossa lista de pendências não se traduz automaticamente em planos de versão.
A presença de um recurso no EF6 também não significa que esse recurso será automaticamente implementado no EF Core.

É difícil detalhar todo o processo que seguimos para planejar uma versão.
Grande parte dele envolve a discussão de recursos específicos, oportunidades e prioridades, e o processo em si também evolui a cada versão.
No entanto, podemos resumir as perguntas comuns que tentamos responder ao decidir qual será a próxima tarefa:

1. **Quantos desenvolvedores achamos que usarão o recurso? Como ele vai aprimorar os aplicativos ou as experiência deles?** Para responder a essa pergunta, podemos coletar comentários de várias fontes – comentários e votos em problemas é uma dessas fontes.

2. **Quais são as soluções alternativas que podem ser usadas caso não implementemos este recurso?** Por exemplo, diversos desenvolvedores podem mapear uma tabela de junção para solucionar a falta de suporte nativo de muitos para muitos. Obviamente, nem todos os desenvolvedores desejam fazê-lo, mas muitos podem, e isso conta como um fator em nossa decisão.

3. **Implementar este recurso desenvolve a arquitetura do EF Core de forma que nos aproxime da implementação de outros recursos?** Favorecemos os recursos que atuam como blocos de construção para outros recursos. Por exemplo, entidades de recipiente da propriedades podem nos ajudar a mover na direção do suporte de muitos-para-muitos, e construtores de entidade habilitaram o nosso suporte a carregamento lento.

4. **O recurso é um ponto de extensibilidade?** Favorecemos os pontos de extensibilidade em relação aos recursos normais, pois eles permitem aos desenvolvedores conectar seus próprios comportamentos e compensar qualquer funcionalidade ausente.

5. **O que é a sinergia do recurso quando usado em combinação com outros produtos?** Favorecemos recursos que habilitam ou melhoram significativamente a experiência de usar o EF Core com outros produtos, como .NET Core, a versão mais recente do Visual Studio, Microsoft Azure e assim por diante.

6. **Quais são as habilidades das pessoas disponíveis para trabalhar em um recurso e como melhor aproveitá-los?** Cada membro da equipe do EF e os colaboradores da comunidade têm diferentes níveis de experiência em áreas distintas, portanto, temos que planejar adequadamente. Mesmo se quiséssemos que todos se empenhassem em trabalhar em um recurso específico, como as traduções do GroupBy ou muitos para muitos, isso não seria prático.

Como mencionado anteriormente, o processo evolui a cada versão.
No futuro, tentaremos dar mais oportunidades para membros da comunidade fornecerem contribuições sobre nossos planos de versão.
Por exemplo, facilitando a revisão dos rascunhos propostos dos recursos e do próprio plano de versão.