---
title: Como funciona a consulta – EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: ba0d68469530e6272ffbb51946d7856122a261c7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656248"
---
# <a name="how-queries-work"></a><span data-ttu-id="634ac-102">Como funciona a consulta</span><span class="sxs-lookup"><span data-stu-id="634ac-102">How Queries Work</span></span>

<span data-ttu-id="634ac-103">O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="634ac-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="634ac-104">O LINQ permite que você use C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas com base em suas classes derivadas de contexto e entidade.</span><span class="sxs-lookup"><span data-stu-id="634ac-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="634ac-105">A vida de uma consulta</span><span class="sxs-lookup"><span data-stu-id="634ac-105">The life of a query</span></span>

<span data-ttu-id="634ac-106">Veja a seguir uma visão geral de alto nível do processo pelo qual cada consulta passa.</span><span class="sxs-lookup"><span data-stu-id="634ac-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="634ac-107">A consulta LINQ é processada pelo Entity Framework Core para criar uma representação que está pronta para ser processada pelo provedor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="634ac-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="634ac-108">O resultado é armazenado em cache para que esse processamento não precise ser feito sempre que a consulta for executada</span><span class="sxs-lookup"><span data-stu-id="634ac-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="634ac-109">O resultado é passado para o provedor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="634ac-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="634ac-110">O provedor do banco de dados identifica quais partes da consulta podem ser avaliadas no banco de dados</span><span class="sxs-lookup"><span data-stu-id="634ac-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="634ac-111">Essas partes da consulta são convertidas em linguagem de consulta específica de banco de dados (por exemplo, SQL para um banco de dados relacional)</span><span class="sxs-lookup"><span data-stu-id="634ac-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="634ac-112">Uma ou mais consultas são enviadas para o banco de dados e o conjunto de resultados retornado (resultados são valores do banco de dados, não as instâncias de entidade)</span><span class="sxs-lookup"><span data-stu-id="634ac-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="634ac-113">Para cada item no conjunto de resultados</span><span class="sxs-lookup"><span data-stu-id="634ac-113">For each item in the result set</span></span>
   1. <span data-ttu-id="634ac-114">Se for um rastreamento de consulta, o EF verificará se os dados representam uma entidade já no controlador de alterações para a instância de contexto</span><span class="sxs-lookup"><span data-stu-id="634ac-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="634ac-115">Nesse caso, a entidade existente será retornada</span><span class="sxs-lookup"><span data-stu-id="634ac-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="634ac-116">Caso contrário, uma nova entidade será criada, o controle de alterações será configurado e a nova entidade será retornada</span><span class="sxs-lookup"><span data-stu-id="634ac-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="634ac-117">Se for uma consulta sem rastreamento, o EF verificará se os dados representam uma entidade já no conjunto de resultados para essa consulta</span><span class="sxs-lookup"><span data-stu-id="634ac-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="634ac-118">Nesse caso, a entidade existente será retornada <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="634ac-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="634ac-119">Caso contrário, uma nova entidade será criada e retornada</span><span class="sxs-lookup"><span data-stu-id="634ac-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="634ac-120"><sup>(1)</sup> as consultas sem controle usam referências fracas para manter o controle das entidades que já foram retornadas.</span><span class="sxs-lookup"><span data-stu-id="634ac-120"><sup>(1)</sup> No-tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="634ac-121">Se um resultado anterior com a mesma identidade sai do escopo e a coleta de lixo é executada, você poderá receber uma nova instância da entidade.</span><span class="sxs-lookup"><span data-stu-id="634ac-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="634ac-122">Quando as consultas são executadas</span><span class="sxs-lookup"><span data-stu-id="634ac-122">When queries are executed</span></span>

<span data-ttu-id="634ac-123">Quando você chama operadores LINQ, está criando simplesmente uma representação na memória da consulta.</span><span class="sxs-lookup"><span data-stu-id="634ac-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="634ac-124">A consulta é enviada somente para o banco de dados quando os resultados são consumidos.</span><span class="sxs-lookup"><span data-stu-id="634ac-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="634ac-125">As operações mais comuns que resultam na consulta que está sendo enviada para o banco de dados são:</span><span class="sxs-lookup"><span data-stu-id="634ac-125">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="634ac-126">Iteração dos resultados em um loop `for`</span><span class="sxs-lookup"><span data-stu-id="634ac-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="634ac-127">Como usar um operador como `ToList`, `ToArray`, `Single`, `Count`</span><span class="sxs-lookup"><span data-stu-id="634ac-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="634ac-128">Associação de dados de resultados de uma consulta para uma interface de usuário</span><span class="sxs-lookup"><span data-stu-id="634ac-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="634ac-129">**Sempre validar a entrada do usuário:** enquanto o EF Core protege contra ataques de injeção SQL usando parâmetros e ignorando literais em consultas, ele não valida entradas.</span><span class="sxs-lookup"><span data-stu-id="634ac-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="634ac-130">Validação apropriada, conforme os requisitos do aplicativo, deve ser executada antes de valores de fontes não confiáveis serem usados em consultas LINQ, atribuídos às propriedades da entidade ou transmitidos para outras APIs do EF Core.</span><span class="sxs-lookup"><span data-stu-id="634ac-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="634ac-131">Isso inclui qualquer entrada do usuário usada para construir consultas dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="634ac-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="634ac-132">Mesmo ao usar o LINQ, se você está aceitando entrada do usuário para criar expressões, precisa para garantir que apenas as expressões pretendidas possam ser criadas.</span><span class="sxs-lookup"><span data-stu-id="634ac-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>