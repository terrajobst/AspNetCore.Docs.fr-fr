---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implémentation de la fonctionnalité CRUD de base avec Entity Framework dans une Application ASP.NET MVC (2 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 999a598b6f9c9a16c596cb6c8d7bb46439876f01
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834396"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implémentation de la fonctionnalité CRUD de base avec Entity Framework dans une Application ASP.NET MVC (2 sur 10)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels à partir du début ou [télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et commencez ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [télécharger le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire votre problème. Vous trouverez généralement la solution au problème en comparant votre code pour le code complet. Pour certaines erreurs courantes et comment les résoudre, consultez [erreurs et des solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Dans le didacticiel précédent, vous avez créé une application MVC qui stocke et affiche des données à l’aide d’Entity Framework et SQL Server LocalDB. Dans ce didacticiel, vous allez examiner et personnaliser le CRUD (créer, lire, mettre à jour, supprimer) le code que la génération de modèles automatique MVC crée automatiquement pour vous dans les contrôleurs et les vues.

> [!NOTE]
> Il est courant d’implémenter le modèle de référentiel pour créer une couche d’abstraction entre votre contrôleur et la couche d’accès aux données. Pour simplifier ces didacticiels, vous ne sont pas implémenter un référentiel jusqu'à un didacticiel plus loin dans cette série.


Dans ce didacticiel, vous allez créer les pages web suivantes :

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Création d’une Page de détails

Le code généré automatiquement pour les étudiants `Index` page omis le `Enrollments` propriété, car elle contient une collection. Dans le `Details` page que vous affichez le contenu de la collection dans un tableau HTML.

 Dans *Controllers\StudentController.cs*, la méthode d’action pour le `Details` afficher utilise le `Find` méthode pour récupérer une seule `Student` entité. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 La valeur de clé est passée à la méthode comme le `id` paramètre et proviennent de données d’itinéraire dans le **détails** lien hypertexte sur la page d’Index. 

1. Ouvrez *Views\Student\Details.cshtml*. Chaque champ est affiché avec un `DisplayFor` helper, comme indiqué dans l’exemple suivant : 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Après le `EnrollmentDate` champ et immédiatement avant la fermeture `fieldset` , ajoutez le code pour afficher une liste d’inscriptions, comme indiqué dans l’exemple suivant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Ce code parcourt en boucle les entités dans la propriété de navigation `Enrollments`. Pour chaque `Enrollment` entité dans la propriété, il affiche le titre du cours et la qualité. Le titre du cours est récupéré à partir de la `Course` entité qui est stockée dans le `Course` propriété de navigation de la `Enrollments` entité. Toutes ces données est récupéré à partir de la base de données automatiquement lorsqu’il est nécessaire. (En d’autres termes, vous utilisez le chargement différé ici. Vous n’avez pas spécifié *chargement hâtif* pour le `Courses` la propriété de navigation, donc la première fois que vous essayez d’accéder à cette propriété, une requête est envoyée à la base de données pour récupérer les données. Vous trouverez plus d’informations sur le chargement différé et le chargement hâtif dans le [lecture de données connexes](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) didacticiel plus loin dans cette série.)
3. Exécuter la page en sélectionnant le **étudiants** onglet et en cliquant sur un **détails** lien de Alexander Carson. Vous voyez la liste des cours et des notes de l’étudiant sélectionné :

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>La mise à jour de la Page Create

1. Dans *Controllers\StudentController.cs*, remplacez le `HttpPost``Create` méthode d’action avec le code suivant pour ajouter un `try-catch` bloc et le [attribut Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) à la méthode généré automatiquement : 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Ce code ajoute la `Student` entité créée par le binder de modèle ASP.NET MVC à le `Students` entité définie et puis enregistre les modifications apportées à la base de données. (*Binder de modèle* fait référence à la fonctionnalité d’ASP.NET MVC qui facilite la plus facile pour vous permettent de travailler avec des données soumises par un formulaire d’un classeur de modèles convertit validée formulaire valeurs aux types CLR et les transmet à la méthode d’action dans les paramètres. In this case, le classeur de modèles instancie un `Student` entité pour vous à l’aide de la propriété valeurs à partir de la `Form` collection.)

    Le `ValidateAntiForgeryToken` empêche l’attribut [falsification de requête intersites](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) les attaques.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Exécuter la page en sélectionnant le **étudiants** onglet et en cliquant sur **créer un nouveau**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Une validation des données fonctionne par défaut. Entrez les noms et une date non valide, puis cliquez sur **créer** pour voir le message d’erreur.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Le code en surbrillance suivant montre le contrôle de validation de modèle.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Modifier la date à une valeur valide tel que 1/9/2005, cliquez sur **créer** pour afficher le nouvel étudiant dans la **Index** page.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>La mise à jour de la Page Modifier le billet

Dans *Controllers\StudentController.cs*, le `HttpGet` `Edit` (méthode) (celle sans le `HttpPost` attribut) utilise le `Find` méthode pour récupérer le texte sélectionné `Student` entité, comme vous l’avez vu dans le `Details` (méthode). Vous n’avez pas besoin de modifier cette méthode.

Toutefois, remplacez le `HttpPost` `Edit` méthode d’action avec le code suivant pour ajouter un `try-catch` bloc et le [attribut Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Ce code est similaire à ce que vous avez vu dans la `HttpPost` `Create` (méthode). Toutefois, au lieu d’ajouter l’entité créée par le binder de modèle pour le jeu d’entités, ce code définit un indicateur sur l’entité indiquant qu’il a été modifié. Lorsque le [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) méthode est appelée, le [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) indicateur, Entity Framework créer des instructions SQL pour mettre à jour de la ligne de base de données. Toutes les colonnes de la ligne de base de données seront actualisés, y compris celles que l’utilisateur n’avez pas modifié, et conflits d’accès concurrentiel sont ignorés. (Vous allez apprendre à gérer l’accès concurrentiel dans un didacticiel plus loin dans cette série.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>États des entités et les attacher et les méthodes de SaveChanges

Le contexte de base de données effectue le suivi de la synchronisation ou non des entités en mémoire avec leurs lignes correspondantes dans la base de données, et ces informations déterminent ce qui se passe quand vous appelez la méthode `SaveChanges`. Par exemple, lorsque vous passez une nouvelle entité à la [ajouter](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) (méthode), que l’état de l’entité est définie sur `Added`. Ensuite, quand vous appelez le [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) (méthode), le contexte de base de données émet une SQL `INSERT` commande.

Une entité peut être dans un de le[suivant états](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. L’entité n’existe pas encore dans la base de données. Le `SaveChanges` méthode doit émettre un `INSERT` instruction.
- `Unchanged`. La méthode `SaveChanges` ne doit rien faire avec cette entité. Quand vous lisez une entité dans la base de données, l’entité a d’abord cet état.
- `Modified`. Tout ou partie des valeurs de propriété de l’entité ont été modifiées. Le `SaveChanges` méthode doit émettre un `UPDATE` instruction.
- `Deleted`. L’entité a été marquée pour suppression. Le `SaveChanges` méthode doit émettre un `DELETE` instruction.
- `Detached`. L’entité n’est pas suivie par le contexte de base de données.

Dans une application de poste de travail, les changements d’état sont généralement définis automatiquement. Dans un type d’application de bureau, vous lisez une entité et apportez des modifications à certaines de ses valeurs de propriété. Son état passe alors automatiquement à `Modified`. Lorsque vous appelez `SaveChanges`, Entity Framework génère une instance SQL `UPDATE` instruction qui met à jour uniquement les propriétés que vous avez modifié.

N’autorise pas la nature déconnectée des applications web pour cette séquence continue. Le [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) qui lit une entité est supprimée après le rendu d’une page. Lorsque le `HttpPost` `Edit` méthode d’action est appelée, une nouvelle requête est effectuée et vous avez une nouvelle instance de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), de sorte que vous devez définir manuellement l’état d’entité `Modified.` puis lorsque vous appelez `SaveChanges`, Entity Framework met à jour toutes les colonnes de la ligne de base de données, car le contexte n’a aucun moyen de savoir quelles propriétés vous avez modifié.

Si vous souhaitez que le code SQL `Update` instruction pour mettre à jour uniquement les champs que l’utilisateur a été modifié en fait, vous pouvez enregistrer les valeurs d’origine d’une certaine façon (par exemple, les champs masqués) afin qu’ils soient disponibles lorsque le `HttpPost` `Edit` méthode est appelée. Vous pouvez ensuite créer un `Student` entité en utilisant les valeurs d’origine, l’appel de la `Attach` méthode avec cette version d’origine de l’entité, mettre à jour les valeurs de l’entité avec les nouvelles valeurs, puis appelez `SaveChanges.` pour plus d’informations, consultez [ États des entités et SaveChanges](https://msdn.microsoft.com/data/jj592676) et [données locales](https://msdn.microsoft.com/data/jj592872) dans le centre de développement de données MSDN.

Le code dans *Views\Student\Edit.cshtml* est similaire à ce que vous avez vu dans *Create.cshtml*, et aucune modification n’est requise.

Exécuter la page en sélectionnant le **étudiants** onglet et puis en cliquant sur un **modifier** lien hypertexte.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Changez quelques données et cliquez sur **Save**. Vous voyez les données modifiées dans la page d’Index.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>La mise à jour de la Page Delete

Dans *Controllers\StudentController.cs*, le code du modèle pour le `HttpGet` `Delete` méthode utilise le `Find` méthode pour récupérer le texte sélectionné `Student` entité, comme vous l’avez vu dans la `Details` et `Edit` méthodes. Cependant, pour implémenter un message d’erreur personnalisé quand l’appel à `SaveChanges` échoue, vous devez ajouter des fonctionnalités à cette méthode et à sa vue correspondante.

Comme vous l’avez vu pour les opérations de mise à jour et de création, les opérations de suppression nécessitent deux méthodes d’action. La méthode est appelée en réponse à une demande GET affiche une vue qui permet à l’utilisateur à approuver ou d’annuler l’opération de suppression. Si l’utilisateur l’approuve, une demande POST est créée. Lorsque cela se produit, le `HttpPost` `Delete` méthode est appelée et puis cette méthode effectue l’opération de suppression.

Vous ajouterez un `try-catch` bloquer à la `HttpPost` `Delete` méthode pour gérer les erreurs qui peuvent se produire lorsque la base de données est mis à jour. Si une erreur se produit, le `HttpPost` `Delete` les appels de méthode le `HttpGet` `Delete` méthode, en lui passant un paramètre qui indique qu’une erreur s’est produite. Le `HttpGet Delete` méthode réaffiche ensuite la page de confirmation, ainsi que le message d’erreur, donnant à l’utilisateur une possibilité d’annuler ou réessayez.

1. Remplacez le `HttpGet` `Delete` méthode d’action avec le code suivant, qui gère le rapport d’erreurs : 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Ce code accepte un [facultatif](https://msdn.microsoft.com/library/dd264739.aspx) paramètre booléen qui indique si elle a été appelée après une défaillance pour enregistrer les modifications. Ce paramètre est `false` lorsque le `HttpGet` `Delete` méthode est appelée sans un échec. Lorsqu’elle est appelée le `HttpPost` `Delete` en réponse à une erreur de mise à jour de base de données, le paramètre est `true` et un message d’erreur est passé à la vue.
2. Remplacez le `HttpPost` `Delete` méthode d’action (nommé `DeleteConfirmed`) avec le code suivant, qui effectue l’opération de suppression réelle et intercepte les erreurs de mise à jour de base de données.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Ce code récupère l’entité sélectionnée, puis appelle la [supprimer](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) méthode pour définir l’état de l’entité `Deleted`. Lorsque `SaveChanges` est appelée, une instance SQL `DELETE` commande est générée. Vous avez également changé le nom de la méthode d’action de `DeleteConfirmed` en `Delete`. Le code structuré nommé le `HttpPost` `Delete` méthode `DeleteConfirmed` afin de donner la `HttpPost` méthode une signature unique. (Le CLR nécessite des méthodes surchargées doivent avoir des paramètres de méthode différents.) Maintenant que les signatures sont uniques, vous pouvez cantonner à la convention MVC et utiliser le même nom pour le `HttpPost` et `HttpGet` supprimer les méthodes.

     Si l’amélioration des performances dans une application de haut volume est une priorité, vous pouvez éviter une requête SQL inutile pour récupérer la ligne en remplaçant les lignes de code qui appellent le `Find` et `Remove` méthodes avec le code suivant comme indiqué en jaune Mettez en surbrillance :

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Ce code instancie un `Student` entité à l’aide de la valeur de clé primaire, puis définir l’état d’entité à `Deleted`. C’est tout ce dont a besoin Entity Framework pour pouvoir supprimer l’entité.

     Comme indiqué, le `HttpGet` `Delete` méthode ne supprime pas les données. Exécution d’une opération de suppression en réponse à une opération GET demander (ou d’ailleurs, exécutez des opérations de modification, opération de création ou toute autre opération qui modifie des données) crée un risque de sécurité. Pour plus d’informations, consultez [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sur le blog de Stephen Walther.
3. Dans *Views\Student\Delete.cshtml*, ajouter un message d’erreur entre la `h2` titre et le `h3` titre, comme indiqué dans l’exemple suivant :

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Exécuter la page en sélectionnant le **étudiants** onglet et en cliquant sur un **supprimer** lien hypertexte :

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Cliquez sur **Delete**. La page Index s’affiche sans l’étudiant supprimé. (Vous verrez un exemple de l’erreur de gestion du code en action dans le [gère l’accès concurrentiel](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) didacticiel plus loin dans cette série.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>S’assurer que les connexions de base de données ne sont pas laissées ouverts

Pour vous assurer que les connexions de base de données sont fermées correctement et que les ressources qu’ils détiennent libérer, vous devriez voir lui que l’instance de contexte est supprimé. Autrement dit pourquoi le code structuré fournit un [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) méthode à la fin de la `StudentController` classe dans *StudentController.cs*, comme illustré dans l’exemple suivant :

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

La base de `Controller` déjà la classe implémente le `IDisposable` interface, ce code ajoute simplement un remplacement pour le `Dispose(bool)` méthode explicitement supprimer l’instance de contexte.

## <a name="summary"></a>Récapitulatif

Vous disposez maintenant d’un ensemble complet de pages qui effectuent des opérations CRUD simples pour `Student` entités. Programmes d’assistance MVC vous permet de générer des éléments d’interface utilisateur pour les champs de données. Pour plus d’informations sur les programmes d’assistance MVC, consultez [rendu d’un formulaire à l’aide de HTML Helpers](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (la page est de MVC 3, mais est toujours d’actualité pour MVC 4).

Dans le didacticiel suivant, vous allez développer les fonctionnalités de la page d’Index en ajoutant le tri et de pagination.

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Suivant](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
