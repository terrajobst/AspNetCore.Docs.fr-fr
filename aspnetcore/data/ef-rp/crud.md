---
title: Pages Razor avec EF Core - CRUD - 2 de 8
author: rick-anderson
description: "Montre comment créer, lire, mettre à jour, supprimer avec EF de base"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Pages Razor avec EF Core dans ASP.NET Core - CRUD - (2 sur 8)

Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Dans ce didacticiel, le code des méthodes CRUD (créer, lire, mettre à jour, supprimer) généré automatiquement est revu et personnalisé.

Remarque : Pour réduire la complexité et conserver ces didacticiels axés sur EF Core, le code EF Core est utilisé dans les modèles de page des Pages Razor. Certains développeurs utilisent une couche de service ou un modèle de référentiel pour créer une couche d’abstraction entre l’interface utilisateur (Pages Razor) et la couche d’accès aux données.

Dans ce didacticiel, les pages Razor Create, Edit, Delete et Details du dossier *Student* sont modifiées.

Le code généré automatiquement utilise le modèle suivant pour les pages Create, Edit et Delete :

* Obtenir et afficher les données demandées avec la méthode HTTP GET `OnGetAsync`.
* Enregistrer les modifications dans les données avec la méthode HTTP POST `OnPostAsync`.

Les pages Index et Details obtiennent et affichent les données demandées avec la méthode HTTP GET`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Remplacer SingleOrDefaultAsync par FirstOrDefaultAsync

Le code généré utilise [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) pour extraire l’entité demandée. [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) est plus efficace pour l’extraction d’une entité :

* À moins que le code ne doive vérifier qu'il n’y a pas plusieurs entités retournées par la requête. 
* `SingleOrDefaultAsync` récupère plus de données et effectue un travail non nécessaire.
* `SingleOrDefaultAsync` lève une exception s’il existe plusieurs entités qui correspondent au filtre.
*  `FirstOrDefaultAsync` ne lève pas d'exception s’il existe plusieurs entités qui correspondent au filtre.

Remplacer `SingleOrDefaultAsync` avec `FirstOrDefaultAsync`. `SingleOrDefaultAsync` est utilisé dans des 5 emplacements :

* `OnGetAsync` dans la page Details.
* `OnGetAsync`et `OnPostAsync` dans les pages Edit et Delete.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

Dans une grande partie du code généré automatiquement, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) peut être utilisé à la place de `FirstOrDefaultAsync` ou `SingleOrDefaultAsync`. 

`FindAsync`:

* Recherche une entité avec la clé primaire (PK). Si une entité avec la clé est suivie par le contexte, elle est retournée sans une requête à la base de données.
* Est simple et concise.
* Est optimisée pour rechercher une entité unique.
* Peut avoir des avantages de performances dans certaines situations, mais ils interviennent rarement pour les scénarios web normaux.
* Utilise implicitement [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) au lieu de [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Toutefois, si vous souhaitez inclure d’autres entités, le Find n’est plus approprié. Cela signifie que vous devrez peut-être abandonner Find et passer à une requête au fur et à mesure que votre application progresse.

## <a name="customize-the-details-page"></a>Personnaliser la page de détails

Accédez à la page `Pages/Students`. Les liens **Edit**, **Details**, et **Delete** sont générés par le [Tag helper anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) dans le fichier *Pages/Student/Index.cshtml*.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Sélectionnez un lien de détails. L’URL est au format `http://localhost:5000/Students/Details?id=2`. L’ID est passé à l’aide d’une chaîne de requête (`?id=2`).

Mettez à jour les Pages Razor Edit, Details et Delete pour utiliser le modèle de route `"{id:int}"`. Remplacez la directive de chacune de ces pages (`@page "{id:int}"`) par `@page`.

Une demande à la page avec le modèle de route «{id:int}» qui n'inclut **pas** une valeur de route avec un entier retourne une erreur HTTP 404 (not found). Par exemple, `http://localhost:5000/Students/Details` retourne une erreur 404. Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :

 ```cshtml
@page "{id:int?}"
```

Exécutez l’application, cliquez sur un lien Détails et vérifiez que l’URL passe l’ID en tant que données de route (`http://localhost:5000/Students/Details/2`).

Ne changez pas `@page` en `@page "{id:int}"` globalement, cela casserait les liens vers les pages Home et Create.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Ajouter des données associées

Le code généré automatiquement pour la page Index des étudiants n’inclut pas la propriété `Enrollments`. Dans cette section, le contenu de la collection `Enrollments` s’affiche dans la page Details.

Le méthode `OnGetAsync` de *Pages/Students/Details.cshtml.cs* utilise la méthode `FirstOrDefaultAsync` pour récupérer une seule entité `Student`. Ajoutez le code en surbrillance suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

Les méthodes `Include` et `ThenInclude` font que le contexte charge la propriété de navigation `Student.Enrollments` et dans chaque inscription, la propriété de navigation `Enrollment.Course`. Ces méthodes sont examinées en détail dans le didacticiel sur la lecture des données.

La méthode `AsNoTracking` améliore les performances dans les scénarios lorsque les entités retournées ne sont pas mises à jour dans le contexte actuel. Le sujet `AsNoTracking` est abordé plus loin dans ce didacticiel.

### <a name="display-related-enrollments-on-the-details-page"></a>Afficher les inscriptions associées sur la page Details

Ouvrez *Pages/Students/Details.cshtml*. Ajoutez le code en surbrillance suivant pour afficher la liste des inscriptions :

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Si la mise en retrait du code est incorrecte, une fois que le code est collé, appuyez sur CTRL + K + D pour résoudre ce problème.

Le code précédent effectue une itération sur les entités dans la propriété de navigation `Enrollments`. Pour chaque inscription, il affiche le titre du cours et le niveau. Le titre du cours est récupéré à partir de l’entité de cours qui est stockée dans la propriété de navigation `Course` de l’entité Enrollments.

Exécutez l’application, sélectionnez l'onglet **Students**, puis cliquez sur le lien **Details** pour un étudiant. La liste des cours et les notes de l’étudiant sélectionné s’affiche.

## <a name="update-the-create-page"></a>Mise à jour de la page de création

Mettez à jour la méthode `OnPostAsync` dans *Pages/Students/Create.cshtml.cs* avec le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Examinez le code de [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) :

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

Dans le code précédent, `TryUpdateModelAsync<Student>` tente de mettre à jour le `emptyStudent` en utilisant les valeurs de formulaire publiées à partir de la propriété de l’objet [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) dans le [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync`met à jour uniquement les propriétés répertoriées (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

Dans l’exemple précédent :

* Le deuxième argument (` "student", // Prefix`) est le préfixe utilisé pour rechercher des valeurs. Il ne respecte pas la casse.
* Les valeurs de formulaire envoyées sont converties vers les types du modèle `Student` en utilisant la [liaison de modèle](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Overposting

Utiliser `TryUpdateModel` pour mettre à jour les champs avec les valeurs envoyées est une bonne pratique de sécurité, car elle empêche la sur-validation. Par exemple, supposons que l’entité Student inclut une propriété `Secret` que cette page web ne doit pas mettre à jour ou ajouter :

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Même si l’application n’a pas un champ `Secret` sur la Page Razor de création/mise à jour, un pirate peut définir la valeur `Secret` par sur-validation. Un pirate peut utiliser un outil comme Fiddler ou écrire du JavaScript pour envoyer une valeur de formulaire `Secret`. Le code d’origine ne limite pas les champs que le classeur de modèles utilise lorsqu’il crée une instance d’étudiant.

Quelle que soit la valeur spécifiée par le pirate pour le champ de formulaire `Secret`, elle est mise à jour dans la base de données. L’illustration suivante montre l’outil Fiddler ajoutant le champ `Secret` (avec la valeur « OverPost ») pour les valeurs de formulaire publiées.

![Champ de clé secrète d’ajout de Fiddler](../ef-mvc/crud/_static/fiddler.png)

La valeur « OverPost » est ajoutée avec succès à la propriété `Secret` de la ligne insérée. Le concepteur de l’application n'a jamais eu l'intention que la propriété `Secret` ne soit définie avec la page de création.

<a name="vm"></a>
### <a name="view-model"></a>Modèle d’affichage

Un modèle d’affichage contient généralement un sous-ensemble des propriétés incluses dans le modèle utilisé par l’application. Le modèle d’application est souvent appelé le modèle de domaine. En règle générale, le modèle de domaine contient toutes les propriétés requises par l’entité correspondante dans la base de données. Le modèle d’affichage contient uniquement les propriétés requises pour la couche d’interface utilisateur (par exemple, la page Créer). Outre le modèle de vue, certaines applications utilisent un modèle de liaison ou d’entrée pour passer des données entre la classe de modèle de page de Pages Razor et le navigateur. Considérez le view model `Student`  suivant :

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Les modèles de vue sont un autre moyen d’empêcher la sur-validation. Le modèle d’affichage contient uniquement les propriétés à afficher ou à mettre à jour.

Le code suivant utilise le view model `StudentVM` pour créer un nouvel étudiant :

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

La méthode [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) définit les valeurs de cet objet en lisant les valeurs d’un autre objet [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues). `SetValues` utilise la correspondance de nom de propriété. Le type de modèle de vue n’a pas besoin d’être liées pour le type de modèle, il a juste besoin d’avoir des propriétés qui correspondent.

Utiliser `StudentVM` requiert que [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) soit mis à jour pour utiliser `StudentVM` plutôt que `Student`.

Dans les Pages Razor, la classe `PageModel` dérivée est le modèle d’affichage. 

## <a name="update-the-edit-page"></a>Mise à jour de la page de modification

Mettre à jour le modèle de page de la page de modification :

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Les modifications de code sont similaires à la page Créer, à quelques exceptions près :

* `OnPostAsync` a un paramètre `id` optionnel.
* L'étudiant actuel est extrait de la base de données, au lieu de créer un étudiant vide.
* `FirstOrDefaultAsync` a été remplacé par [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync` est un bon choix lors de la sélection d’une entité à partir de la clé primaire. Consultez [FindAsync](#FindAsync) pour plus d’informations.

### <a name="test-the-edit-and-create-pages"></a>Tester les pages Edit et Create

Créer et modifier plusieurs entités de student.

## <a name="entity-states"></a>États de l’entité

Le contexte de base de données effectue le suivi pour vérifier si les entités en mémoire sont synchronisées avec les lignes correspondantes dans la base de données. Les informations de synchronisation de contexte de base de données déterminent ce qu'il se passe lorsque `SaveChanges` est appelée. Par exemple, lorsqu’une nouvelle entité est passée à la méthode `Add`, l’état de cette entité est défini sur `Added`. Lorsque `SaveChanges` est appelée, le contexte de base de données émet une commande SQL INSERT.

Une entité peut être l’un des états suivants :

* `Added`: L’entité n’existe pas encore dans la base de données. La méthode `SaveChanges` émet une instruction INSERT.

* `Unchanged`: Aucune modification ne doit être enregistrée avec cette entité. Une entité est dans cet état lorsqu’elle est lue à partir de la base de données.

* `Modified`: Tout ou partie des valeurs de propriété de l’entité ont été modifiées. La méthode `SaveChanges` émet une instruction UPDATE.

* `Deleted`: L’entité a été marquée pour suppression. La méthode `SaveChanges` émet une instruction DELETE.

* `Detached`: L’entité n’est pas suivie par le contexte de base de données.

Dans une application de bureau, les modifications d’état sont généralement définies automatiquement. Une entité est en lecture, de modifications sont apportées et l’état d’entité qui sera automatiquement remplacée par `Modified`. Appel de `SaveChanges` génère une instruction SQL UPDATE qui met à jour uniquement les propriétés modifiées.

Dans une application web, le `DbContext` qui lit une entité et affiche les données est supprimée après le rendu d’une page. Lorsque la méthode `OnPostAsync` d'une page est appelée, une nouvelle requête web est effectuée avec une nouvelle instance de `DbContext`. La relecture de l’entité dans ce nouveau contexte simule le traitement du poste de travail.

## <a name="update-the-delete-page"></a>Mise à jour de la page de suppression

Dans cette section, le code est ajouté pour implémenter une message d'erreur personnalisé lorsque l’appel à `SaveChanges` échoue. Ajoutez une chaîne pour contenir des messages d’erreur possibles :

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Remplacez la méthode `OnGetAsync` par le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Le code précédent contient le paramètre facultatif `saveChangesError`. `saveChangesError` indique si la méthode a été appelée après un échec de suppression de l’objet Student. L’opération de suppression peut échouer en raison des problèmes réseau temporaires. Les erreurs réseau transitoires sont probablement dans le cloud. `saveChangesError` a la valeur false lorsque la page de suppression `OnGetAsync` est appelée à partir de l’interface utilisateur. Lorsque `OnGetAsync` est appelée par `OnPostAsync` (parce que l’opération de suppression a échoué), le paramètre `saveChangesError` a la valeur true.

### <a name="the-delete-pages-onpostasync-method"></a>La méthode OnPostAsync des pages Delete

Remplacez le `OnPostAsync` par le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Le code précédent récupère l’entité sélectionnée, puis appelle la méthode `Remove` pour définir l’état de l’entité à `Deleted`. Lorsque `SaveChanges` est appelée, une commande SQL DELETE est générée. Si `Remove` échoue :

* L’exception de la base de données est interceptée.
* La méthode `OnGetAsync` des pages est appelée avec `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Mise à jour de la Page Razor Delete

Ajoutez le message d’erreur mis en surbrillance suivant à la Page Razor Delete.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Testez la suppression.

## <a name="common-errors"></a>Erreurs courantes

Student/Home ou d'autres liens ne fonctionnent pas :

Vérifiez que la Page Razor contient la bonne directive `@page`. Par exemple, la Page Razor Student/Home ne doit **pas** contenir un modèle de route :

```cshtml
@page "{id:int}"
```

Chaque Page Razor doit inclure la directive `@page`.

>[!div class="step-by-step"]
[Précédent](xref:data/ef-rp/intro)
[Suivant](xref:data/ef-rp/sort-filter-page)
