---
title: Pages Razor avec EF Core dans ASP.NET Core - CRUD - 2 sur 8
author: rick-anderson
description: Montre comment créer, lire, mettre à jour et supprimer avec EF Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 0a8d386104d9c62f37d0a94e66344d3f1e23bae9
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063349"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>Pages Razor avec EF Core dans ASP.NET Core - CRUD - 2 sur 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Dans ce didacticiel, nous allons examiner et personnaliser le code CRUD (créer, lire, mettre à jour, supprimer) généré automatiquement.

Pour que ces tutoriels soient moins complexes et traitent exclusivement d’EF Core, nous avons utilisé le code EF Core dans les modèles de page. Certains développeurs utilisent un modèle de référentiel ou de couche de service pour créer une couche d’abstraction entre l’interface utilisateur (Pages Razor) et la couche d’accès aux données.

Dans ce tutoriel, nous examinons les pages Razor Create, Edit, Delete et Details dans le dossier *Student*.

Le code généré automatiquement utilise le modèle suivant pour les pages Create, Edit et Delete :

* Obtenir et afficher les données demandées avec la méthode HTTP GET `OnGetAsync`.
* Enregistrer les modifications dans les données avec la méthode HTTP POST `OnPostAsync`.

Les pages Index et Details obtiennent et affichent les données demandées avec la méthode HTTP GET `OnGetAsync`.

## <a name="singleordefaultasync-vs-firstordefaultasync"></a>SingleOrDefaultAsync vs. FirstOrDefaultAsync

Le code généré utilise [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), qui est généralement préféré à [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

 `FirstOrDefaultAsync` est plus efficace que `SingleOrDefaultAsync` pour récupérer une entité :

* Sauf si le code doit vérifier que la requête ne retourne pas plusieurs entités.
* `SingleOrDefaultAsync` récupère davantage de données et effectue un travail inutile.
* `SingleOrDefaultAsync` lève une exception si plusieurs entités correspondent à la partie filtre.
* `FirstOrDefaultAsync` ne lève pas d’exception si plusieurs entités correspondent à la partie filtre.

<a name="FindAsync"></a>

### <a name="findasync"></a>FindAsync

Dans une grande partie du code généré automatiquement, vous pouvez utiliser [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) à la place de `FirstOrDefaultAsync`.

`FindAsync`:

* Recherche une entité avec la clé primaire. Si une entité avec la clé primaire est suivie par le contexte, elle est retournée sans qu’une requête soit envoyée à la base de données.
* Est simple et concise.
* Est optimisée pour rechercher une entité unique.
* Peut avoir des avantages de performances dans certaines situations, mais c’est rarement le cas pour les applications web standard.
* Utilise implicitement [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) au lieu de [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

Toutefois, si vous voulez exécuter `Include` sur d’autres entités, `FindAsync` n’est plus approprié. Cela signifie que vous devez peut-être abandonner `FindAsync` et passer à une requête à mesure que votre application progresse.

## <a name="customize-the-details-page"></a>Personnaliser la page Details

Accédez à la page `Pages/Students`. Les liens **Edit**, **Details**, et **Delete** sont générés par le [Tag helper anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) dans le fichier *Pages/Student/Index.cshtml*.

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

Exécutez l’application et sélectionnez un lien **Details**. L’URL est au format `http://localhost:5000/Students/Details?id=2`. L’ID d’étudiant est transmis à l’aide d’une chaîne de requête (`?id=2`).

Mettez à jour les Pages Razor Edit, Details et Delete pour utiliser le modèle de route `"{id:int}"`. Remplacez la directive de chacune de ces pages (`@page`) par `@page "{id:int}"`.

Une demande à la page avec le modèle de route «{id:int}» qui n'inclut **pas** une valeur de route avec un entier retourne une erreur HTTP 404 (not found).  Par exemple, `http://localhost:5000/Students/Details` retourne une erreur 404. Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :

 ```cshtml
@page "{id:int?}"
```

Exécutez l’application, cliquez sur un lien Détails et vérifiez que l’URL passe l’ID en tant que données de route (`http://localhost:5000/Students/Details/2`).

Ne changez pas `@page` en `@page "{id:int}"` globalement, cela casserait les liens vers les pages Home et Create.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Ajouter des données associées

Le code généré automatiquement pour la page Index des étudiants n’inclut pas la propriété `Enrollments`. Dans cette section, le contenu de la collection `Enrollments` s’affiche dans la page Details.

La méthode `OnGetAsync` de *Pages/Students/Details.cshtml.cs* utilise la méthode `FirstOrDefaultAsync` pour récupérer une seule entité `Student`. Ajoutez le code en surbrillance suivant :

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

Les méthodes [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) et [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) forcent le contexte à charger la propriété de navigation `Student.Enrollments` et, dans chaque inscription, la propriété de navigation `Enrollment.Course`. Ces méthodes sont examinées en détail dans le tutoriel sur la lecture des données associées.

La méthode [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) améliore les performances dans les scénarios où les entités retournées ne sont pas mises à jour dans le contexte actuel. Le sujet `AsNoTracking` est abordé plus loin dans ce didacticiel.

### <a name="display-related-enrollments-on-the-details-page"></a>Afficher les inscriptions associées sur la page Details

Ouvrez *Pages/Students/Details.cshtml*. Ajoutez le code en surbrillance suivant pour afficher la liste des inscriptions :

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

Si la mise en retrait du code est incorrecte, une fois que le code est collé, appuyez sur CTRL + K + D pour résoudre ce problème.

Le code précédent effectue une itération sur les entités dans la propriété de navigation `Enrollments`. Pour chaque inscription, il affiche le titre du cours et le niveau. Le titre du cours est récupéré à partir de l’entité de cours qui est stockée dans la propriété de navigation `Course` de l’entité Enrollments.

Exécutez l’application, sélectionnez l'onglet **Students**, puis cliquez sur le lien **Details** pour un étudiant. La liste des cours et les notes de l’étudiant sélectionné s’affiche.

## <a name="update-the-create-page"></a>Mettre à jour la page Create

Mettez à jour la méthode `OnPostAsync` dans *Pages/Students/Create.cshtml.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Examinez le code [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) :

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

Dans le code précédent, `TryUpdateModelAsync<Student>` tente de mettre à jour l’objet `emptyStudent` en utilisant les valeurs de formulaire publiées à partir de la propriété [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) dans le [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). `TryUpdateModelAsync` met uniquement à jour les propriétés répertoriées (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

Dans l’exemple précédent :

* Le deuxième argument (`"student", // Prefix`) est le préfixe utilisé pour rechercher des valeurs. Il ne respecte pas la casse.
* Les valeurs de formulaire publiées sont converties vers les types dans le modèle `Student` à l’aide de la [liaison de modèle](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>

### <a name="overposting"></a>Sur-publication

L’utilisation de `TryUpdateModel` pour mettre à jour des champs avec des valeurs publiées est une bonne pratique de sécurité, car cela empêche la sur-publication. Par exemple, supposez que l’entité Student comprend une propriété `Secret` que cette page web ne doit pas mettre à jour ou ajouter :

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Même si l’application n’a pas de champ `Secret` dans la page Razor de création/mise à jour, un pirate pourrait définir la valeur `Secret` par sur-publication. Un pirate pourrait utiliser un outil tel que Fiddler, ou écrire du JavaScript, pour publier une valeur de formulaire `Secret`. Le code d’origine ne limite pas les champs que le classeur de modèles utilise quand il crée une instance de Student.

La valeur spécifiée par le pirate pour le champ de formulaire `Secret`, quelle qu’elle soit, est mise à jour dans la base de données. L’illustration suivante montre l’outil Fiddler en train d’ajouter le champ `Secret` (avec la valeur « OverPost ») aux valeurs de formulaire publiées.

![Fiddler ajoutant un champ Secret](../ef-mvc/crud/_static/fiddler.png)

La valeur « OverPost » est ajoutée avec succès à la propriété `Secret` de la ligne insérée. Le concepteur de l’application n’a jamais prévu que la propriété `Secret` soit définie avec la page Create.

<a name="vm"></a>

### <a name="view-model"></a>Modèle d’affichage

Un modèle d’affichage contient généralement un sous-ensemble des propriétés incluses dans le modèle utilisé par l’application. Le modèle d’application est souvent appelé modèle de domaine. En règle générale, le modèle de domaine contient toutes les propriétés requises par l’entité correspondante dans la base de données. Le modèle d’affichage contient uniquement les propriétés nécessaires pour la couche d’interface utilisateur (par exemple, la page Create). En plus du modèle d’affichage, certaines applications utilisent un modèle de liaison ou d’entrée pour transmettre des données entre la classe de modèles de pages de Pages Razor et le navigateur. Considérez le modèle d’affichage `Student` suivant :

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

Les modèles d’affichage fournissent une alternative pour empêcher la sur-publication. Le modèle d’affichage contient uniquement les propriétés à afficher ou à mettre à jour.

Le code suivant utilise le modèle d’affichage `StudentVM` pour créer un étudiant :

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

La méthode [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) définit les valeurs de cet objet en lisant les valeurs d’un autre objet [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues). `SetValues` utilise la correspondance de nom de propriété. Le type de modèle d’affichage ne doit pas nécessairement être lié au type de modèle. Il doit simplement avoir des propriétés qui correspondent.

L’utilisation de `StudentVM` exige que [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) soit mis à jour pour utiliser `StudentVM` plutôt que `Student`.

Dans les pages Razor, la classe dérivée `PageModel` est le modèle d’affichage.

## <a name="update-the-edit-page"></a>Mettre à jour la page Edit

Mettez à jour le modèle de page pour la page Edit. Les modifications principales apparaissent en surbrillance :

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Les modifications de code sont semblables à celles de la page Create, à quelques exceptions près :

* `OnPostAsync` a un paramètre facultatif `id`.
* L’étudiant actuel est récupéré à partir de la base de données (on ne crée pas un étudiant vide).
* `FirstOrDefaultAsync` a été remplacée par [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync). `FindAsync` est un bon choix lors de la sélection d’une entité à partir de la clé primaire. Pour plus d’informations, consultez [FindAsync](#FindAsync).

### <a name="test-the-edit-and-create-pages"></a>Tester les pages Edit et Create

Créez et modifiez quelques entités d’étudiants.

## <a name="entity-states"></a>États des entités

Le contexte de base de données effectue un suivi afin de savoir si les entités en mémoire sont synchronisées avec les lignes correspondantes dans la base de données. Les informations de synchronisation du contexte de base de données déterminent ce qui se produit quand [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) est appelé. Par exemple, quand une nouvelle entité est passée à la méthode [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync), l’état de cette entité est défini sur [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added). Quand `SaveChangesAsync` est appelée, le contexte de base de données émet une commande SQL INSERT.

Une entité peut être dans l’un des [états suivants](/dotnet/api/microsoft.entityframeworkcore.entitystate) :

* `Added` : L’entité n’existe pas encore dans la base de données. La méthode `SaveChanges` émet une instruction INSERT.

* `Unchanged` : Aucune modification ne doit être enregistrée avec cette entité. Une entité est dans cet état quand elle est lue à partir de la base de données.

* `Modified` : Tout ou une partie des valeurs de propriété de l’entité ont été modifiées. La méthode `SaveChanges` émet une instruction UPDATE.

* `Deleted` : L’entité a été marquée pour suppression. La méthode `SaveChanges` émet une instruction DELETE.

* `Detached` : L’entité n’est pas suivie par le contexte de base de données.

Dans une application de bureau, les changements d’état sont généralement définis automatiquement. Une entité est lue, des modifications sont apportées et l’état d’entité passe automatiquement à `Modified`. L’appel de `SaveChanges` génère une instruction SQL UPDATE qui met à jour uniquement les propriétés modifiées.

Dans une application web, le `DbContext` qui lit une entité et affiche les données est supprimé après le rendu d’une page. Quand la méthode `OnPostAsync` d’une page est appelée, une nouvelle requête web est faite avec une nouvelle instance du `DbContext`. La relecture de l’entité dans ce nouveau contexte simule le traitement de bureau.

## <a name="update-the-delete-page"></a>Mettre à jour la page Delete

Dans cette section, nous ajoutons du code pour implémenter un message d’erreur personnalisé quand l’appel à `SaveChanges` échoue. Ajoutez une chaîne contenant les messages d’erreur possibles :

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Remplacez la méthode `OnGetAsync` par le code suivant :

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Le code précédent contient le paramètre facultatif `saveChangesError`. `saveChangesError` indique si la méthode a été appelée après un échec de suppression de l’objet Student. L’opération de suppression peut échouer en raison de problèmes réseau temporaires. Les erreurs réseau temporaires sont probablement dans le cloud. `saveChangesError` a la valeur false quand la méthode `OnGetAsync` de la page Delete est appelée à partir de l’interface utilisateur. Quand `OnGetAsync` est appelée par `OnPostAsync` (car l’opération de suppression a échoué), le paramètre `saveChangesError` a la valeur true.

### <a name="the-delete-pages-onpostasync-method"></a>La méthode OnPostAsync des pages Delete

Remplacez `OnPostAsync` par le code suivant :

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Le code précédent récupère l’entité sélectionnée, puis appelle la méthode [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) pour définir l’état de l’entité sur `Deleted`. Lorsque `SaveChanges` est appelée, une commande SQL DELETE est générée. Si `Remove` échoue :

* L’exception de la base de données est interceptée.
* La méthode `OnGetAsync` des pages est appelée avec `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Mise à jour de la Page Razor Delete

Ajoutez le message d’erreur mis en surbrillance suivant à la Page Razor Delete.
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Testez la suppression.

## <a name="common-errors"></a>Erreurs courantes

Les liens Student/Index ou autres ne fonctionnent pas :

Vérifiez que la Page Razor contient la bonne directive `@page`. Par exemple, la page Razor Student/Index ne doit **pas** contenir de modèle de route :

```cshtml
@page "{id:int}"
```

Chaque page Razor doit inclure la directive `@page`.

::: moniker-end

> [!div class="step-by-step"]
> [Précédent](xref:data/ef-rp/intro)
> [Suivant](xref:data/ef-rp/sort-filter-page)
