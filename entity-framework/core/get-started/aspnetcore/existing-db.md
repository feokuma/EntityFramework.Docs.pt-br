---
title: Introdução ao ASP.NET Core – Banco de dados existente – EF Core
author: rowanmiller
description: Introdução ao EF Core no ASP.NET Core com um Banco de Dados existente
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: eeebd75bebe85994c6439e06243e113f2bda814c
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565235"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="6b158-103">Introdução ao EF Core no ASP.NET Core com um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="6b158-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="6b158-104">Neste tutorial, você compila um aplicativo MVC do ASP.NET Core que realiza acesso básico a dados usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6b158-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="6b158-105">Realize a engenharia reversa de um banco de dados existente para criar um modelo do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6b158-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="6b158-106">[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="6b158-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b158-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="6b158-107">Prerequisites</span></span>

<span data-ttu-id="6b158-108">Instale o software a seguir:</span><span class="sxs-lookup"><span data-stu-id="6b158-108">Install the following software:</span></span>

* <span data-ttu-id="6b158-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:</span><span class="sxs-lookup"><span data-stu-id="6b158-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="6b158-110">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="6b158-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="6b158-111">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="6b158-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="6b158-112">[SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="6b158-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="6b158-113">Criar banco de dados de blog</span><span class="sxs-lookup"><span data-stu-id="6b158-113">Create Blogging database</span></span>

<span data-ttu-id="6b158-114">Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6b158-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="6b158-115">Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.</span><span class="sxs-lookup"><span data-stu-id="6b158-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="6b158-116">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b158-116">Open Visual Studio</span></span>
* <span data-ttu-id="6b158-117">**Ferramentas > Conectar ao Banco de Dados...**</span><span class="sxs-lookup"><span data-stu-id="6b158-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="6b158-118">Selecione **Microsoft SQL Server** e clique em **Continuar**</span><span class="sxs-lookup"><span data-stu-id="6b158-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="6b158-119">Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**</span><span class="sxs-lookup"><span data-stu-id="6b158-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="6b158-120">Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="6b158-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="6b158-121">O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="6b158-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="6b158-122">Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**</span><span class="sxs-lookup"><span data-stu-id="6b158-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="6b158-123">Copie o script listado abaixo para o editor de consultas</span><span class="sxs-lookup"><span data-stu-id="6b158-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="6b158-124">Clique com o botão direito do mouse no editor de consultas e selecione **Executar**</span><span class="sxs-lookup"><span data-stu-id="6b158-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="6b158-125">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="6b158-125">Create a new project</span></span>

* <span data-ttu-id="6b158-126">Abra o Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6b158-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="6b158-127">**Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="6b158-127">**File > New > Project...**</span></span>
* <span data-ttu-id="6b158-128">No menu à esquerda, selecione **Instalado > Visual C# > Web**</span><span class="sxs-lookup"><span data-stu-id="6b158-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="6b158-129">Selecione o modelo de projeto **Aplicativo Web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="6b158-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="6b158-130">Insira **EFGetStarted.AspNetCore.ExistingDb** como o nome (ele deve corresponder exatamente ao namespace usado posteriormente no código) e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="6b158-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="6b158-131">Aguarde a caixa de diálogo **Novo Aplicativo Web ASP.NET Core** aparecer</span><span class="sxs-lookup"><span data-stu-id="6b158-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="6b158-132">Verifique se a lista suspensa da estrutura de destino está definida como **.NET Core** e a lista suspensa da versão definida como **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="6b158-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="6b158-133">Selecione o modelo **Aplicativo Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="6b158-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="6b158-134">Certifique-se de que **Autenticação** esteja definida como **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="6b158-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="6b158-135">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="6b158-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="6b158-136">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6b158-136">Install Entity Framework Core</span></span>

<span data-ttu-id="6b158-137">Para instalar o EF Core, instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="6b158-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="6b158-138">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="6b158-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="6b158-139">Para este tutorial, não é necessário instalar um pacote de provedor, porque o tutorial usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6b158-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="6b158-140">O pacote de provedor do SQL Server está incluído no [metapacote Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="6b158-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="6b158-141">Fazer engenharia reversa em seu modelo</span><span class="sxs-lookup"><span data-stu-id="6b158-141">Reverse engineer your model</span></span>

<span data-ttu-id="6b158-142">Agora é hora de criar o modelo EF com base em seu banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6b158-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="6b158-143">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="6b158-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="6b158-144">Execute o seguinte comando para criar um modelo do banco de dados existente:</span><span class="sxs-lookup"><span data-stu-id="6b158-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="6b158-145">Se você receber um erro indicando `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b158-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="6b158-146">Especifique para quais tabelas você deseja gerar entidades adicionando o argumento `-Tables` para o comando acima.</span><span class="sxs-lookup"><span data-stu-id="6b158-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="6b158-147">Por exemplo, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="6b158-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="6b158-148">O processo de engenharia reversa criou classes de entidade (`Blog.cs` & `Post.cs`) e um contexto derivado (`BloggingContext.cs`) com base no esquema do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6b158-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="6b158-149">As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar.</span><span class="sxs-lookup"><span data-stu-id="6b158-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="6b158-150">Veja as classes de entidade `Blog` e `Post`:</span><span class="sxs-lookup"><span data-stu-id="6b158-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="6b158-151">Para habilitar o carregamento lento, crie propriedades de navegação `virtual` (Blog.Post e Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="6b158-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="6b158-152">O contexto representa uma sessão com o banco de dados e permite que você veja e salve as instâncias das classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="6b158-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="6b158-153">Registre seu contexto com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="6b158-153">Register your context with dependency injection</span></span>

<span data-ttu-id="6b158-154">O conceito de injeção de dependência é central ao ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b158-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="6b158-155">Serviços (como `BloggingContext`) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6b158-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6b158-156">Agora, os componentes que exigem esses serviços (como os controladores de MVC) os recebem por meio de propriedades ou parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="6b158-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="6b158-157">Para saber mais sobre injeção de dependência, veja o artigo [Injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) no site do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b158-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="6b158-158">Registrar e configurar o contexto em Startup.cs</span><span class="sxs-lookup"><span data-stu-id="6b158-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="6b158-159">Para disponibilizar `BloggingContext` para os controladores do MVC, registre-o como um serviço.</span><span class="sxs-lookup"><span data-stu-id="6b158-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="6b158-160">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="6b158-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="6b158-161">Adicione as declarações `using` a seguir ao início do método</span><span class="sxs-lookup"><span data-stu-id="6b158-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="6b158-162">Agora é possível usar o método `AddDbContext(...)` para registrá-lo como um serviço.</span><span class="sxs-lookup"><span data-stu-id="6b158-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="6b158-163">Localize o método `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="6b158-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="6b158-164">Adicionar o seguinte código realçado para registrar o contexto como um serviço</span><span class="sxs-lookup"><span data-stu-id="6b158-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="6b158-165">Em um aplicativo real, normalmente você colocaria a cadeia de conexão em um arquivo de configuração ou variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="6b158-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="6b158-166">Para simplificar, este tutorial define isso no código.</span><span class="sxs-lookup"><span data-stu-id="6b158-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="6b158-167">Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="6b158-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="6b158-168">Criar um controlador e exibições</span><span class="sxs-lookup"><span data-stu-id="6b158-168">Create a controller and views</span></span>

* <span data-ttu-id="6b158-169">Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador...**</span><span class="sxs-lookup"><span data-stu-id="6b158-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="6b158-170">Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Ok**</span><span class="sxs-lookup"><span data-stu-id="6b158-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="6b158-171">Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="6b158-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="6b158-172">Clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="6b158-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="6b158-173">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="6b158-173">Run the application</span></span>

<span data-ttu-id="6b158-174">Agora, você pode executar o aplicativo para vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="6b158-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="6b158-175">**Depurar > Iniciar Sem Depuração**</span><span class="sxs-lookup"><span data-stu-id="6b158-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="6b158-176">O aplicativo é compilado e aberto em um navegador da Web</span><span class="sxs-lookup"><span data-stu-id="6b158-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="6b158-177">Navegue até `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="6b158-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="6b158-178">Clique em **Criar Novo**</span><span class="sxs-lookup"><span data-stu-id="6b158-178">Click **Create New**</span></span>
* <span data-ttu-id="6b158-179">Insira uma **Url** para o novo blog e clique em **Criar**</span><span class="sxs-lookup"><span data-stu-id="6b158-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Criar página](_static/create.png)

  ![Página de índice](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="6b158-182">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="6b158-182">Next steps</span></span>

<span data-ttu-id="6b158-183">Para obter mais informações sobre como criar o scaffolding de um contexto e classes de entidade, veja os seguintes artigos:</span><span class="sxs-lookup"><span data-stu-id="6b158-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="6b158-184">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="6b158-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="6b158-185">Referência de ferramentas do Entity Framework Core – CLI do .NET</span><span class="sxs-lookup"><span data-stu-id="6b158-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="6b158-186">Referência de ferramentas do Entity Framework Core – Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="6b158-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
