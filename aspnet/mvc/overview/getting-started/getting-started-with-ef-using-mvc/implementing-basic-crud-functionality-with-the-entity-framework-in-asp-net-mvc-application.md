---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implémentation de la fonctionnalité CRUD de base avec Entity Framework dans une Application ASP.NET MVC | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eeed2b572ddecb66c3b95926b57dd783c26d5f0f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390215"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implémentation de la fonctionnalité CRUD de base avec Entity Framework dans une Application ASP.NET MVC
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Dans le didacticiel précédent, vous avez créé une application MVC qui stocke et affiche des données à l’aide d’Entity Framework et SQL Server LocalDB. Dans ce didacticiel, vous allez examiner et personnaliser le CRUD (créer, lire, mettre à jour, supprimer) le code que la génération de modèles automatique MVC crée automatiquement pour vous dans les contrôleurs et les vues.

> [!NOTE]
> Il est courant d’implémenter le modèle de référentiel pour créer une couche d’abstraction entre votre contrôleur et la couche d’accès aux données. Pour conserver ces didacticiels simples et orientés vers l’apprentissage de l’utilisation d’Entity Framework proprement dit, ils n’utilisent pas de référentiels. Pour plus d’informations sur la façon d’implémenter des dépôts, consultez le [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).


Dans ce didacticiel, vous allez créer les pages web suivantes :

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Créer une Page de détails

Le code généré automatiquement pour les étudiants `Index` page omis le `Enrollments` propriété, car elle contient une collection. Dans le `Details` page que vous affichez le contenu de la collection dans un tableau HTML.

 Dans *Controllers\StudentController.cs*, la méthode d’action pour le `Details` afficher utilise le [trouver](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) méthode pour récupérer une seule `Student` entité. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

La valeur de clé est passée à la méthode comme le `id` paramètre et proviennent de *acheminer les données* dans le **détails** lien hypertexte sur la page d’Index.

### <a name="tip-route-data"></a>Conseil : **acheminer les données**

Données d’itinéraire sont données qui le binder de modèle se trouvée dans un segment d’URL spécifié dans la table de routage. Par exemple, l’itinéraire par défaut spécifie `controller`, `action`, et `id` segments :

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Dans l’URL suivante, mappe l’itinéraire par défaut `Instructor` en tant que le `controller`, `Index` en tant que le `action` et 1 comme le `id`; il s’agit des valeurs de données d’itinéraire.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

« ? courseID = 2021 » est une valeur de chaîne de requête. Le binder de modèle fonctionne également si vous passez le `id` en tant que valeur de chaîne de requête :

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Les URL sont créés par `ActionLink` instructions dans la vue Razor. Dans le code suivant, le `id` paramètre correspond à l’itinéraire par défaut, par conséquent, `id` est ajouté pour les données d’itinéraire.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Dans le code suivant, `courseID` ne correspond pas à un paramètre dans l’itinéraire par défaut, donc il est ajouté en tant qu’une chaîne de requête.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Ouvrez *Views\Student\Details.cshtml*. Chaque champ est affiché avec un `DisplayFor` helper, comme indiqué dans l’exemple suivant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Après le `EnrollmentDate` champ et immédiatement avant la fermeture `</dl>` , ajoutez le code en surbrillance pour afficher une liste d’inscriptions, comme indiqué dans l’exemple suivant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Si l’indentation du code est incorrecte une fois le code collé, appuyez sur Ctrl-K-D pour la corriger.

    Ce code parcourt en boucle les entités dans la propriété de navigation `Enrollments`. Pour chaque `Enrollment` entité dans la propriété, il affiche le titre du cours et la qualité. Le titre du cours est récupéré à partir de la `Course` entité qui est stockée dans le `Course` propriété de navigation de la `Enrollments` entité. Toutes ces données est récupéré à partir de la base de données automatiquement lorsqu’il est nécessaire. (En d’autres termes, vous utilisez le chargement différé ici. Vous n’avez pas spécifié *chargement hâtif* pour le `Courses` propriété de navigation, donc les inscriptions n’ont pas été récupérées dans la même requête qui a obtenu les étudiants. Au lieu de cela, la première fois que vous essayez d’accéder à la `Enrollments` propriété de navigation, une nouvelle requête est envoyée à la base de données pour récupérer les données. Vous trouverez plus d’informations sur le chargement différé et le chargement hâtif dans le [lecture de données connexes](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) didacticiel plus loin dans cette série.)
3. Exécuter la page en sélectionnant le **étudiants** onglet et en cliquant sur un **détails** lien de Alexander Carson. (Si vous appuyez sur CTRL + F5 pendant que le fichier Details.cshtml est ouvert, vous obtiendrez une erreur HTTP 400, car Visual Studio essaie d’exécuter la page de détails, mais il n’a pas été atteint à partir d’un lien qui spécifie l’étudiant à afficher. Dans ce cas, simplement supprimer « Student/détails » de l’URL et réessayez, ou fermez le navigateur, cliquez sur le projet et cliquez sur **vue**, puis cliquez sur **afficher dans le navigateur**.)

    Vous voyez la liste des cours et des notes de l’étudiant sélectionné :

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Mettre à jour de la Page Create

1. Dans *Controllers\StudentController.cs*, remplacez le `HttpPost``Create` méthode d’action avec le code suivant pour ajouter un `try-catch` bloquer et supprimer des `ID` à partir de la [attribut Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) pour la méthode généré automatiquement :

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Ce code ajoute la `Student` entité créée par le binder de modèle ASP.NET MVC à le `Students` entité définie et puis enregistre les modifications apportées à la base de données. (*Binder de modèle* fait référence à la fonctionnalité d’ASP.NET MVC qui facilite la plus facile pour vous permettent de travailler avec des données soumises par un formulaire d’un classeur de modèles convertit validée formulaire valeurs aux types CLR et les transmet à la méthode d’action dans les paramètres. In this case, le classeur de modèles instancie un `Student` entité pour vous à l’aide de la propriété valeurs à partir de la `Form` collection.)

    Vous avez supprimé `ID` à partir de la liaison d’attribut, car `ID` est la valeur de clé primaire SQL Server définira automatiquement lorsque la ligne est insérée. Entrée de l’utilisateur ne définit pas la `ID` valeur.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Avertissement de sécurité - le `ValidateAntiForgeryToken` empêche l’attribut [falsification de requête intersites](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) les attaques. Elle nécessite un correspondant `Html.AntiForgeryToken()` instruction dans la vue, vous verrez plus tard.

    Le `Bind` attribut est une méthode de protection contre *sur-publication* de créer des scénarios. Par exemple, supposons que le `Student` entité inclut un `Secret` propriété que vous ne souhaitez pas cette page web à définir.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Même si vous n’avez pas un `Secret` champ sur la page web, un pirate pourrait utiliser un outil tel que [fiddler](http://fiddler2.com/home), ou écrire du JavaScript, pour valider un `Secret` valeur de formulaire. Sans le [lier](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribut limitant les champs que le classeur de modèles utilise lorsqu’il crée un `Student` instance<em>,</em> le classeur de modèles choisit `Secret` valeur de formulaire et utilisez-le pour créer le `Student` instance d’entité. Ensuite, la valeur spécifiée par le hacker pour le champ de formulaire `Secret`, quelle qu’elle soit, est mise à jour dans la base de données. L’illustration suivante montre le fiddler Ajout de l’outil le `Secret` champ (avec la valeur « OverPost ») pour les valeurs de formulaire publiées.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    La valeur « OverPost » serait correctement ajoutée à la propriété `Secret` de la ligne insérée, même si vous n’aviez jamais prévu que la page web puisse définir cette propriété.

    Il est recommandé d’utiliser le `Include` paramètre avec le `Bind` attribut *liste verte* champs. Il est également possible d’utiliser le `Exclude` paramètre *blacklist* champs que vous souhaitez exclure. La raison pour laquelle `Include` est plus sécurisé est que lorsque vous ajoutez une nouvelle propriété à l’entité, le nouveau champ n'est pas automatiquement protégé par un `Exclude` liste.

    Vous pouvez empêcher la survalidation dans les scénarios de modification consiste à commencer par lire l’entité à partir de la base de données, puis en appelant `TryUpdateModel`, en passant dans une liste de propriétés explicitement autorisées. C’est la méthode utilisée dans ces didacticiels.

    Une autre façon d’empêcher la survalidation qui est préférée par de nombreux développeurs consiste à utiliser des modèles de vue au lieu des classes d’entité avec la liaison de modèle. Incluez seulement les propriétés que vous voulez mettre à jour dans le modèle de vue. Une fois le classeur de modèles MVC terminée, copiez les propriétés de modèle de vue à l’instance d’entité, en utilisant éventuellement un outil tel que [AutoMapper](http://automapper.org/). Utiliser la base de données. Entrée sur l’instance d’entité pour définir son état à « Unchanged », puis définissez Property("PropertyName"). IsModified sur true sur chaque propriété d’entité qui est incluse dans le modèle de vue. Cette méthode fonctionne à la fois dans les scénarios de modification et de création.

    Autre que le `Bind` attribut, le `try-catch` bloc est la seule modification que vous avez apportées au code généré automatiquement. Si une exception qui dérive de [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) est interceptée pendant que les modifications sont enregistrées, un message d’erreur générique s’affiche. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions sont parfois dues à quelque chose d’externe à l’application plutôt qu’une erreur de programmation, l’utilisateur est donc recommandé pour réessayer. Bien que ceci ne soit pas implémenté dans cet exemple, une application destinée à la production doit consigner l’exception. Pour plus d’informations, consultez la section **Journal pour obtenir un aperçu** de [Surveillance et télémétrie (génération d’applications Cloud du monde réel avec Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Le code dans *Views\Student\Create.cshtml* est similaire à ce que vous avez vu dans *Details.cshtml*, sauf que `EditorFor` et `ValidationMessageFor` helpers sont utilisés pour chaque champ au lieu de `DisplayFor`. Voici le code :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* inclut également `@Html.AntiForgeryToken()`, qui fonctionne avec le `ValidateAntiForgeryToken` attribut dans le contrôleur permettant d’empêcher [falsification de requête intersites](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) les attaques.

    Aucune modification n’est nécessaire dans *Create.cshtml*.
2. Exécuter la page en sélectionnant le **étudiants** onglet et en cliquant sur **créer un nouveau**.
3. Entrez les noms et une date non valide, puis cliquez sur **créer** pour voir le message d’erreur.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Il s’agit de la validation côté serveur que vous obtenez par défaut ; dans un didacticiel suivant, vous verrez comment ajouter des attributs qui génèrent du code également pour la validation côté client. Le code en surbrillance suivant illustre la vérification de validation de modèle dans le **créer** (méthode).

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Changez la date en une valeur valide, puis cliquez sur **Create** pour voir apparaître le nouvel étudiant dans la page **Index**.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Mise à jour la méthode HttpPost Edit

Dans *Controllers\StudentController.cs*, le `HttpGet` `Edit` (méthode) (celle sans le `HttpPost` attribut) utilise le `Find` méthode pour récupérer le texte sélectionné `Student` entité, comme vous l’avez vu dans le `Details` (méthode). Vous n’avez pas besoin de modifier cette méthode.

Toutefois, remplacez le `HttpPost` `Edit` méthode d’action avec le code suivant :

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Ces modifications implémentent une sécurité pour empêcher [sur-validation](#overpost), le Générateur de modèles automatique a généré un `Bind` d’attribut et ajouté l’entité créée par le binder de modèle pour le jeu d’entités portant un indicateur modifié. Que code n’est plus recommandé, car le `Bind` attribut efface toutes les données préexistantes dans les champs non répertoriés dans le `Include` paramètre. À l’avenir, le Générateur de modèles automatique MVC contrôleur sera mise à jour afin qu’il ne génère pas `Bind` attributs pour les méthodes de modification.

Le nouveau code lit l’entité existante et les appels [TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) pour mettre à jour les champs à partir de l’entrée d’utilisateur dans les données de formulaire publiées. Suivi des ensembles de modifications automatique d’Entity Framework le [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) indicateur sur l’entité. Lorsque le [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) méthode est appelée, le `Modified` indicateur, Entity Framework créer des instructions SQL pour mettre à jour de la ligne de base de données. [Conflits d’accès concurrentiel](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) sont ignorées, et toutes les colonnes de la ligne de base de données sont mises à jour, y compris celles que l’utilisateur n’a pas changé. (Un didacticiel suivant montre comment gérer les conflits d’accès concurrentiel, et si vous souhaitez uniquement des champs individuels à mettre à jour dans la base de données, vous pouvez définir l’entité à « Unchanged » et définir des champs sur Modified.)

Recommandé pour empêcher la survalidation, les champs que vous souhaitez être mis à jour par la page de modification sont dans la liste verte dans la `TryUpdateModel` paramètres. Actuellement, vous ne protégez aucun champ supplémentaire, mais le fait de répertorier les champs que vous voulez que le classeur de modèles lie garantit que si vous ajoutez ultérieurement des champs au modèle de données, ils seront automatiquement protégés jusqu’à ce que vous les ajoutiez explicitement ici.

À la suite de ces modifications, la signature de méthode de la méthode HttpPost Edit est identique à la méthode edit HttpGet ; Par conséquent, vous avez renommé la méthode EditPost.

> [!TIP] 
> 
> **États des entités et les attacher et les méthodes de SaveChanges**
> 
> Le contexte de base de données effectue le suivi de la synchronisation ou non des entités en mémoire avec leurs lignes correspondantes dans la base de données, et ces informations déterminent ce qui se passe quand vous appelez la méthode `SaveChanges`. Par exemple, lorsque vous passez une nouvelle entité à la [ajouter](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) (méthode), que l’état de l’entité est définie sur `Added`. Ensuite, quand vous appelez le [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) (méthode), le contexte de base de données émet une SQL `INSERT` commande.
> 
> Une entité peut être dans un de le[suivant états](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`. L’entité n’existe pas encore dans la base de données. Le `SaveChanges` méthode doit émettre un `INSERT` instruction.
> - `Unchanged`. La méthode `SaveChanges` ne doit rien faire avec cette entité. Quand vous lisez une entité dans la base de données, l’entité a d’abord cet état.
> - `Modified`. Tout ou partie des valeurs de propriété de l’entité ont été modifiées. Le `SaveChanges` méthode doit émettre un `UPDATE` instruction.
> - `Deleted`. L’entité a été marquée pour suppression. Le `SaveChanges` méthode doit émettre un `DELETE` instruction.
> - `Detached`. L’entité n’est pas suivie par le contexte de base de données.
> 
> Dans une application de poste de travail, les changements d’état sont généralement définis automatiquement. Dans un type d’application de bureau, vous lisez une entité et apportez des modifications à certaines de ses valeurs de propriété. Son état passe alors automatiquement à `Modified`. Lorsque vous appelez `SaveChanges`, Entity Framework génère une instance SQL `UPDATE` instruction qui met à jour uniquement les propriétés que vous avez modifié.
> 
> N’autorise pas la nature déconnectée des applications web pour cette séquence continue. Le [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) qui lit une entité est supprimée après le rendu d’une page. Lorsque le `HttpPost` `Edit` méthode d’action est appelée, une nouvelle requête est effectuée et vous avez une nouvelle instance de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), de sorte que vous devez définir manuellement l’état d’entité `Modified.` puis lorsque vous appelez `SaveChanges`, Entity Framework met à jour toutes les colonnes de la ligne de base de données, car le contexte n’a aucun moyen de savoir quelles propriétés vous avez modifié.
> 
> Si vous souhaitez que le code SQL `Update` instruction pour mettre à jour uniquement les champs que l’utilisateur a été modifié en fait, vous pouvez enregistrer les valeurs d’origine d’une certaine façon (par exemple, les champs masqués) afin qu’ils soient disponibles lorsque le `HttpPost` `Edit` méthode est appelée. Vous pouvez ensuite créer un `Student` entité en utilisant les valeurs d’origine, l’appel de la `Attach` méthode avec cette version d’origine de l’entité, mettre à jour les valeurs de l’entité avec les nouvelles valeurs, puis appelez `SaveChanges.` pour plus d’informations, consultez [ États des entités et SaveChanges](https://msdn.microsoft.com/data/jj592676) et [données locales](https://msdn.microsoft.com/data/jj592872) dans le centre de développement de données MSDN.


Le code HTML et Razor dans *Views\Student\Edit.cshtml* est similaire à ce que vous avez vu dans *Create.cshtml*, et aucune modification n’est requise.

Exécuter la page en sélectionnant le **étudiants** onglet et puis en cliquant sur un **modifier** lien hypertexte.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Changez quelques données et cliquez sur **Save**. Vous voyez les données modifiées dans la page d’Index.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>La mise à jour de la Page Delete

Dans *Controllers\StudentController.cs*, le code du modèle pour le `HttpGet` `Delete` méthode utilise le `Find` méthode pour récupérer le texte sélectionné `Student` entité, comme vous l’avez vu dans la `Details` et `Edit` méthodes. Cependant, pour implémenter un message d’erreur personnalisé quand l’appel à `SaveChanges` échoue, vous devez ajouter des fonctionnalités à cette méthode et à sa vue correspondante.

Comme vous l’avez vu pour les opérations de mise à jour et de création, les opérations de suppression nécessitent deux méthodes d’action. La méthode est appelée en réponse à une demande GET affiche une vue qui permet à l’utilisateur à approuver ou d’annuler l’opération de suppression. Si l’utilisateur l’approuve, une demande POST est créée. Lorsque cela se produit, le `HttpPost` `Delete` méthode est appelée et puis cette méthode effectue l’opération de suppression.

Vous ajouterez un `try-catch` bloquer à la `HttpPost` `Delete` méthode pour gérer les erreurs qui peuvent se produire lorsque la base de données est mis à jour. Si une erreur se produit, le `HttpPost` `Delete` les appels de méthode le `HttpGet` `Delete` méthode, en lui passant un paramètre qui indique qu’une erreur s’est produite. Le `HttpGet Delete` méthode réaffiche ensuite la page de confirmation, ainsi que le message d’erreur, donnant à l’utilisateur une possibilité d’annuler ou réessayez.

1. Remplacez le `HttpGet` `Delete` méthode d’action avec le code suivant, qui gère le rapport d’erreurs : 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Ce code accepte un [paramètre facultatif](https://msdn.microsoft.com/library/dd264739.aspx) qui indique si la méthode a été appelée après une défaillance pour enregistrer les modifications. Ce paramètre est `false` lorsque le `HttpGet` `Delete` méthode est appelée sans un échec. Lorsqu’elle est appelée le `HttpPost` `Delete` en réponse à une erreur de mise à jour de base de données, le paramètre est `true` et un message d’erreur est passé à la vue.
2. Remplacez le `HttpPost` `Delete` méthode d’action (nommé `DeleteConfirmed`) avec le code suivant, qui effectue l’opération de suppression réelle et intercepte les erreurs de mise à jour de base de données.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     Ce code récupère l’entité sélectionnée, puis appelle la [supprimer](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) méthode pour définir l’état de l’entité `Deleted`. Lorsque `SaveChanges` est appelée, une instance SQL `DELETE` commande est générée. Vous avez également changé le nom de la méthode d’action de `DeleteConfirmed` en `Delete`. Le code structuré nommé le `HttpPost` `Delete` méthode `DeleteConfirmed` afin de donner la `HttpPost` méthode une signature unique. (Le CLR nécessite des méthodes surchargées doivent avoir des paramètres de méthode différents.) Maintenant que les signatures sont uniques, vous pouvez cantonner à la convention MVC et utiliser le même nom pour le `HttpPost` et `HttpGet` supprimer les méthodes.

     Si l’amélioration des performances dans une application de haut volume est une priorité, vous pouvez éviter une requête SQL inutile pour récupérer la ligne en remplaçant les lignes de code qui appellent le `Find` et `Remove` méthodes avec le code suivant :

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     Ce code instancie un `Student` entité à l’aide de la valeur de clé primaire, puis définir l’état d’entité à `Deleted`. C’est tout ce dont a besoin Entity Framework pour pouvoir supprimer l’entité.

     Comme indiqué, le `HttpGet` `Delete` méthode ne supprime pas les données. Exécution d’une opération de suppression en réponse à une opération GET demander (ou d’ailleurs, exécutez des opérations de modification, opération de création ou toute autre opération qui modifie des données) crée un risque de sécurité. Pour plus d’informations, consultez [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sur le blog de Stephen Walther.
3. Dans *Views\Student\Delete.cshtml*, ajouter un message d’erreur entre la `h2` titre et le `h3` titre, comme indiqué dans l’exemple suivant :

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     Exécuter la page en sélectionnant le **étudiants** onglet et en cliquant sur un **supprimer** lien hypertexte :

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. Cliquez sur **Delete**. La page Index s’affiche sans l’étudiant supprimé. (Vous verrez un exemple de l’erreur de gestion du code en action dans le [didacticiel accès concurrentiel](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Fermeture des connexions de base de données

Pour fermer les connexions de base de données et libérer les ressources qu’ils détiennent dès que possible, supprimez l’instance de contexte lorsque vous avez terminé avec lui. Autrement dit pourquoi le code structuré fournit un [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) méthode à la fin de la `StudentController` classe dans *StudentController.cs*, comme illustré dans l’exemple suivant :

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La base de `Controller` déjà la classe implémente le `IDisposable` interface, ce code ajoute simplement un remplacement pour le `Dispose(bool)` méthode explicitement supprimer l’instance de contexte.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Gestion des transactions

Par défaut, Entity Framework implémente implicitement les transactions. Dans les scénarios où vous apportez des modifications à plusieurs lignes ou les tables, puis appelez `SaveChanges`, Entity Framework garantit automatiquement que toutes vos modifications réussissent ou tous échouer. Si certaines modifications sont effectuées en premier puis qu’une erreur se produit, ces modifications sont automatiquement annulées. Pour les scénarios où vous avez besoin de plus de contrôle, par exemple, si vous souhaitez inclure des opérations effectuées en dehors d’Entity Framework dans une transaction, consultez [utilisation de Transactions](https://msdn.microsoft.com/data/dn456843) sur MSDN.

## <a name="summary"></a>Récapitulatif

Vous disposez maintenant d’un ensemble complet de pages qui effectuent des opérations CRUD simples pour `Student` entités. Programmes d’assistance MVC vous permet de générer des éléments d’interface utilisateur pour les champs de données. Pour plus d’informations sur les programmes d’assistance MVC, consultez [rendu d’un formulaire à l’aide de HTML Helpers](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (la page est de MVC 3, mais est toujours d’actualité pour MVC 5).

Dans le didacticiel suivant, vous allez développer les fonctionnalités de la page d’Index en ajoutant le tri et de pagination.

Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer. Vous pouvez également demander de nouvelles rubriques à [afficher les leçons de Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vous trouverez des liens vers d’autres ressources Entity Framework dans [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Suivant](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
