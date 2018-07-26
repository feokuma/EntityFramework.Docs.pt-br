---
title: Relações, as propriedades de navegação e chaves estrangeiras - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
caps.latest.revision: 3
ms.openlocfilehash: 0cc18ee8d1b9d1633535e4b8186ffc4c68daf32b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119736"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="59bc9-102">Relações, as propriedades de navegação e chaves estrangeiras</span><span class="sxs-lookup"><span data-stu-id="59bc9-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="59bc9-103">Este tópico fornece uma visão geral de como o Entity Framework gerencia os relacionamentos entre entidades.</span><span class="sxs-lookup"><span data-stu-id="59bc9-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="59bc9-104">Ele também fornece algumas diretrizes sobre como mapear e manipular as relações.</span><span class="sxs-lookup"><span data-stu-id="59bc9-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="59bc9-105">Relações no EF</span><span class="sxs-lookup"><span data-stu-id="59bc9-105">Relationships in EF</span></span>

<span data-ttu-id="59bc9-106">Em bancos de dados relacionais (também chamadas de associações) de relações entre tabelas são definidas por meio de chaves estrangeiras.</span><span class="sxs-lookup"><span data-stu-id="59bc9-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="59bc9-107">Uma chave estrangeira (FK) é uma coluna ou combinação de colunas que é usada para estabelecer e impor um link entre os dados nas duas tabelas.</span><span class="sxs-lookup"><span data-stu-id="59bc9-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="59bc9-108">Normalmente, há três tipos de relações: um para um, um-para-muitos e muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="59bc9-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="59bc9-109">Em uma relação um-para-muitos, a chave estrangeira é definida na tabela que representa o final de muitos da relação.</span><span class="sxs-lookup"><span data-stu-id="59bc9-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="59bc9-110">A relação muitos-para-muitos envolve a definição de uma terceira tabela (chamada de uma tabela de junção ou de junção) cuja chave primária é composto de chaves estrangeiras de ambas as tabelas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="59bc9-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="59bc9-111">Em uma relação um para um, a chave primária atua Além disso, como uma chave estrangeira e não há nenhuma coluna de chave estrangeira separada para qualquer uma das tabelas.</span><span class="sxs-lookup"><span data-stu-id="59bc9-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="59bc9-112">A imagem a seguir mostra duas tabelas que participam no relacionamento um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="59bc9-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="59bc9-113">O **curso** tabela é a tabela dependente, porque ela contém o **DepartmentID** coluna que vincula-o para o **departamento** tabela.</span><span class="sxs-lookup"><span data-stu-id="59bc9-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Database2](~/ef6/media/database2.png)

<span data-ttu-id="59bc9-115">No Entity Framework, uma entidade pode estar relacionada a outras entidades por meio de uma associação ou relação.</span><span class="sxs-lookup"><span data-stu-id="59bc9-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="59bc9-116">Todo relacionamento contém duas extremidades que descrevem o tipo de entidade e a multiplicidade do tipo (um, zero-ou-um ou muitos) para as duas entidades nessa relação.</span><span class="sxs-lookup"><span data-stu-id="59bc9-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="59bc9-117">A relação pode ser regida por uma restrição referencial, que descreve qual ponta da relação é uma função principal e o que é uma função dependente.</span><span class="sxs-lookup"><span data-stu-id="59bc9-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="59bc9-118">Propriedades de navegação fornecem uma maneira de navegar de uma associação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="59bc9-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="59bc9-119">Cada objeto pode ter uma propriedade de navegação para cada relação da qual ele participa.</span><span class="sxs-lookup"><span data-stu-id="59bc9-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="59bc9-120">Propriedades de navegação permitem navegar e gerenciar relações em ambas as direções, retornando um objeto de referência (se a multiplicidade for deles, zero ou um ou mais) ou uma coleção (se a multiplicidade de muitos).</span><span class="sxs-lookup"><span data-stu-id="59bc9-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="59bc9-121">Você também pode optar por ter unidirecional de navegação, caso em que você definir a propriedade de navegação em apenas um dos tipos que participa da relação e não em ambos.</span><span class="sxs-lookup"><span data-stu-id="59bc9-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="59bc9-122">É recomendável para incluir as propriedades no modelo que são mapeados para chaves estrangeiras no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="59bc9-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="59bc9-123">Com propriedades de chave estrangeira incluídas, você pode criar ou alterar uma relação, modificando o valor de chave estrangeira em um objeto dependente.</span><span class="sxs-lookup"><span data-stu-id="59bc9-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="59bc9-124">Esse tipo de associação é chamado de uma associação de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="59bc9-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="59bc9-125">Usar chaves estrangeiras é ainda mais essencial ao trabalhar com entidades desconectadas.</span><span class="sxs-lookup"><span data-stu-id="59bc9-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="59bc9-126">Observe que, quando trabalhar com 1 para 1 ou 1 para 0... relações 1, não há nenhuma coluna de chave estrangeira separada, a propriedade de chave primária atua como a chave estrangeira e é sempre incluída no modelo.</span><span class="sxs-lookup"><span data-stu-id="59bc9-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="59bc9-127">Quando colunas de chave estrangeira não são incluídas no modelo, as informações de associação são gerenciadas como um objeto independente.</span><span class="sxs-lookup"><span data-stu-id="59bc9-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="59bc9-128">As relações são controladas por meio de referências de objeto, em vez de propriedades de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="59bc9-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="59bc9-129">Esse tipo de associação é chamado um *associação independente*.</span><span class="sxs-lookup"><span data-stu-id="59bc9-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="59bc9-130">A maneira mais comum para modificar uma *associação independente* é modificar as propriedades de navegação que são geradas para cada entidade que participa da associação.</span><span class="sxs-lookup"><span data-stu-id="59bc9-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="59bc9-131">Você pode optar por usar um ou ambos os tipos de associações em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="59bc9-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="59bc9-132">No entanto, se você tiver uma relação muitos-para-muitos pura que está conectada por uma tabela de junção que contém apenas as chaves estrangeiras, o EF usará uma associação independente para gerenciar essa relação de muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="59bc9-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="59bc9-133">A imagem a seguir mostra um modelo conceitual que foi criado com o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="59bc9-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="59bc9-134">O modelo contém duas entidades que participam da relação um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="59bc9-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="59bc9-135">Ambas as entidades têm propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="59bc9-135">Both entities have navigation properties.</span></span> <span data-ttu-id="59bc9-136">**Curso** é a entidade depend e tem o **DepartmentID** propriedade de chave estrangeira definida.</span><span class="sxs-lookup"><span data-stu-id="59bc9-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="59bc9-138">O trecho de código a seguir mostra o mesmo modelo que foi criado com o Code First.</span><span class="sxs-lookup"><span data-stu-id="59bc9-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class DepartmentID
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="59bc9-139">Configurando ou relações de mapeamento</span><span class="sxs-lookup"><span data-stu-id="59bc9-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="59bc9-140">O restante desta página aborda como acessar e manipular dados de uso de relações.</span><span class="sxs-lookup"><span data-stu-id="59bc9-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="59bc9-141">Para obter informações sobre como configurar as relações em seu modelo, consulte as páginas a seguir.</span><span class="sxs-lookup"><span data-stu-id="59bc9-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="59bc9-142">Para configurar as relações no Code First, consulte [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) e [API Fluent – relações](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="59bc9-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="59bc9-143">Para configurar as relações usando o Entity Framework Designer, consulte [relacionamentos com o EF Designer](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="59bc9-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="59bc9-144">Criando e modificando relações</span><span class="sxs-lookup"><span data-stu-id="59bc9-144">Creating and modifying relationships</span></span>

<span data-ttu-id="59bc9-145">Em um *associação de chave estrangeira*, quando você altera a relação, o estado de um objeto dependente com um EntityState.Unchanged estado muda para EntityState.Modified.</span><span class="sxs-lookup"><span data-stu-id="59bc9-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an EntityState.Unchanged state changes to EntityState.Modified.</span></span> <span data-ttu-id="59bc9-146">Em uma relação independente, alterar a relação não atualiza o estado do objeto dependente.</span><span class="sxs-lookup"><span data-stu-id="59bc9-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="59bc9-147">Os exemplos a seguir mostram como usar as propriedades de chave estrangeira e propriedades de navegação para associar os objetos relacionados.</span><span class="sxs-lookup"><span data-stu-id="59bc9-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="59bc9-148">Com associações de chave estrangeiras, você pode usar qualquer um dos métodos para alterar, criar ou modificar relações.</span><span class="sxs-lookup"><span data-stu-id="59bc9-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="59bc9-149">Com associações independentes, é possível usar a propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="59bc9-149">With independent associations, you cannot use the foreign key property.</span></span>

-   <span data-ttu-id="59bc9-150">Ao atribuir um novo valor a uma propriedade de chave estrangeira, como no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="59bc9-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   <span data-ttu-id="59bc9-151">O código a seguir remove uma relação, definindo a chave estrangeira como **nulo**.</span><span class="sxs-lookup"><span data-stu-id="59bc9-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="59bc9-152">Observe que a propriedade de chave estrangeira deve ser anulável.</span><span class="sxs-lookup"><span data-stu-id="59bc9-152">Note, that the foreign key property must be nullable.</span></span>  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > <span data-ttu-id="59bc9-153">Se a referência está no estado adicionado (no exemplo, o objeto de curso), a propriedade de navegação de referência não será sincronizada com os valores de chave de um novo objeto até que o SaveChanges é chamado.</span><span class="sxs-lookup"><span data-stu-id="59bc9-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="59bc9-154">Sincronização não ocorrerá porque o contexto de objeto não contém chaves permanentes para objetos adicionados até que eles são salvos.</span><span class="sxs-lookup"><span data-stu-id="59bc9-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="59bc9-155">Se você precisar ter novos objetos totalmente sincronizados, assim que você definir a relação, use um dos methods.\* a seguir</span><span class="sxs-lookup"><span data-stu-id="59bc9-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

-   <span data-ttu-id="59bc9-156">Ao atribuir um novo objeto a uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="59bc9-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="59bc9-157">O código a seguir cria uma relação entre um curso e um `department`.</span><span class="sxs-lookup"><span data-stu-id="59bc9-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="59bc9-158">Se os objetos serão anexados ao contexto, o `course` também é adicionado ao `department.Courses` coleção e a chave estrangeira correspondente propriedade no `course` objeto é definido como o valor da propriedade de chave do departamento.</span><span class="sxs-lookup"><span data-stu-id="59bc9-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
    ``` csharp
    course.Department = department;
    ```

 -   <span data-ttu-id="59bc9-159">Para excluir a relação, defina a propriedade de navegação como `null`.</span><span class="sxs-lookup"><span data-stu-id="59bc9-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="59bc9-160">Se você estiver trabalhando com o Entity Framework é baseado no .NET 4.0, fim relacionado precisa ser carregado antes de você defini-lo como nulo.</span><span class="sxs-lookup"><span data-stu-id="59bc9-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="59bc9-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="59bc9-161">For example:</span></span>  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    <span data-ttu-id="59bc9-162">Começando com o Entity Framework 5.0, que é baseado no .NET 4.5, você pode definir a relação como nulo sem carregar final relacionado.</span><span class="sxs-lookup"><span data-stu-id="59bc9-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="59bc9-163">Você também pode definir o valor atual como nulo usando o método a seguir.</span><span class="sxs-lookup"><span data-stu-id="59bc9-163">You can also set the current value to null using the following method.</span></span>  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   <span data-ttu-id="59bc9-164">Excluindo ou adição de um objeto em uma coleção de entidades.</span><span class="sxs-lookup"><span data-stu-id="59bc9-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="59bc9-165">Por exemplo, você pode adicionar um objeto do tipo `Course` para o `department.Courses` coleção.</span><span class="sxs-lookup"><span data-stu-id="59bc9-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="59bc9-166">Esta operação cria uma relação entre um determinado **curso** e um determinado `department`.</span><span class="sxs-lookup"><span data-stu-id="59bc9-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="59bc9-167">Se os objetos serão anexados ao contexto, a referência de departamento e a propriedade de chave estrangeira sobre a **curso** objeto será definido como apropriado `department`.</span><span class="sxs-lookup"><span data-stu-id="59bc9-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- <span data-ttu-id="59bc9-168">Usando o `ChangeRelationshipState` método para alterar o estado da relação especificada entre dois objetos de entidade.</span><span class="sxs-lookup"><span data-stu-id="59bc9-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="59bc9-169">Esse método é mais comumente usado ao trabalhar com aplicativos de N camadas e um *associação independente* (ele não pode ser usado com uma associação de chave estrangeira).</span><span class="sxs-lookup"><span data-stu-id="59bc9-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="59bc9-170">Além disso, usar esse método você deve descartar para baixo até `ObjectContext`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="59bc9-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="59bc9-171">No exemplo a seguir, há uma relação muitos-para-muitos entre os cursos e instrutores.</span><span class="sxs-lookup"><span data-stu-id="59bc9-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="59bc9-172">Chamar o `ChangeRelationshipState` método e passar a `EntityState.Added` parâmetro, permite que o `SchoolContext` sabe que foi adicionado um relacionamento entre os dois objetos:</span><span class="sxs-lookup"><span data-stu-id="59bc9-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="59bc9-173">Sincronizando as alterações entre as propriedades de navegação e chaves estrangeiras</span><span class="sxs-lookup"><span data-stu-id="59bc9-173">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="59bc9-174">Quando você altera a relação dos objetos anexado ao contexto, usando um dos métodos descritos acima, o Entity Framework precisa manter as chaves estrangeiras, referências e coleções em sincronia. Entity Framework gerencia automaticamente essa sincronização (também conhecido como relação correção-up) para as entidades POCO com proxies.</span><span class="sxs-lookup"><span data-stu-id="59bc9-174">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="59bc9-175">Para obter mais informações, consulte [trabalhar com Proxies](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="59bc9-175">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="59bc9-176">Se você estiver usando as entidades POCO sem proxies, certifique-se de que o **DetectChanges** método é chamado para sincronizar os objetos relacionados no contexto.</span><span class="sxs-lookup"><span data-stu-id="59bc9-176">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="59bc9-177">Observe que as seguintes APIs disparam automaticamente um **DetectChanges** chamar.</span><span class="sxs-lookup"><span data-stu-id="59bc9-177">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="59bc9-178">Executando um LINQ consulta em relação a um `DbSet`</span><span class="sxs-lookup"><span data-stu-id="59bc9-178">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="59bc9-179">Objetos relacionados ao carregamento</span><span class="sxs-lookup"><span data-stu-id="59bc9-179">Loading related objects</span></span>

<span data-ttu-id="59bc9-180">No Entity Framework, que você pode usar mais comumente use as propriedades de navegação para carregar entidades relacionadas à entidade retornada pela associação definida.</span><span class="sxs-lookup"><span data-stu-id="59bc9-180">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="59bc9-181">Para obter mais informações, consulte [Carregando objetos relacionados](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="59bc9-181">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="59bc9-182">Em uma associação de chave estrangeira, quando você carrega uma extremidade relacionada de um objeto dependente, o objeto relacionado será carregado com base no valor de chave estrangeira do dependente que está atualmente na memória:</span><span class="sxs-lookup"><span data-stu-id="59bc9-182">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="59bc9-183">Em uma associação independente, a extremidade relacionada de um objeto dependente será consultada com base no valor de chave estrangeira que está atualmente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="59bc9-183">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="59bc9-184">No entanto, se a relação foi modificada e a propriedade de referência sobre o objeto dependente aponta para um objeto de entidade diferente que é carregado no contexto de objeto, Entity Framework tentará criar uma relação como ele é definido no cliente.</span><span class="sxs-lookup"><span data-stu-id="59bc9-184">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="59bc9-185">Gerenciando a simultaneidade</span><span class="sxs-lookup"><span data-stu-id="59bc9-185">Managing concurrency</span></span>

<span data-ttu-id="59bc9-186">Na chave estrangeira e associações independentes, verificações de simultaneidade baseiam-se sobre as chaves de entidade e outras propriedades da entidade que são definidas no modelo.</span><span class="sxs-lookup"><span data-stu-id="59bc9-186">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="59bc9-187">Ao usar o EF Designer para criar um modelo, defina as `ConcurrencyMode` de atributo para **fixo** para especificar que a propriedade deve ser verificada para simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="59bc9-187">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="59bc9-188">Ao usar o Code First para definir um modelo, use o `ConcurrencyCheck` anotação em propriedades que você deseja a ser verificado para simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="59bc9-188">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="59bc9-189">Ao trabalhar com o Code First, também, você pode usar o `TimeStamp` anotação para especificar que a propriedade deve ser verificada para simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="59bc9-189">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="59bc9-190">Você pode ter apenas uma propriedade de carimbo de hora em uma determinada classe.</span><span class="sxs-lookup"><span data-stu-id="59bc9-190">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="59bc9-191">Primeiro mapas de código dessa propriedade para um campo não anuláveis no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="59bc9-191">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="59bc9-192">É recomendável que você sempre use a associação de chave estrangeira ao trabalhar com entidades que participam de verificação de simultaneidade e a resolução.</span><span class="sxs-lookup"><span data-stu-id="59bc9-192">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="59bc9-193">Para obter mais informações, consulte [tratamento de conflitos de simultaneidade](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="59bc9-193">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="59bc9-194">Trabalhando com chaves sobrepostas</span><span class="sxs-lookup"><span data-stu-id="59bc9-194">Working with overlapping Keys</span></span>

<span data-ttu-id="59bc9-195">Chaves sobrepostas são chaves compostas em que algumas propriedades na chave também são parte de outra chave na entidade.</span><span class="sxs-lookup"><span data-stu-id="59bc9-195">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="59bc9-196">Você não pode ter uma chave de sobreposição em uma associação independente.</span><span class="sxs-lookup"><span data-stu-id="59bc9-196">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="59bc9-197">Para alterar uma associação de chave estrangeira que inclui chaves sobrepostas, é recomendável que você modifique os valores de chave estrangeiros em vez de usar as referências de objeto.</span><span class="sxs-lookup"><span data-stu-id="59bc9-197">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>