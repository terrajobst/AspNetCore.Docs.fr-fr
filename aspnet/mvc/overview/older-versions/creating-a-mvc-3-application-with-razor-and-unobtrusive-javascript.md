---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Création d’un MVC 3 Application avec Razor et JavaScript non Obstrusif | Microsoft Docs
author: microsoft
description: L’exemple d’application web liste utilisateur montre combien il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor. Les opérations de mappage application exemple...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 39ed35c1b7d5c702ffea6908daeac5ca12f1693e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398011"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Création d’un MVC 3 Application avec Razor et JavaScript non Obstrusif
====================
par [Microsoft](https://github.com/microsoft)

> L’exemple d’application web liste utilisateur montre combien il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor. L’exemple d’application montre comment utiliser le nouveau moteur d’affichage Razor avec ASP.NET MVC version 3 et Visual Studio 2010 pour créer un site Web liste utilisateur fictif qui inclut des fonctionnalités telles que la création, affichage, modification et suppression d’utilisateurs.
> 
> Ce didacticiel décrit les étapes qui ont été prises pour générer la liste des utilisateurs exemple ASP.NET MVC 3 d’application. Un projet Visual Studio avec code source c# et VB est disponible pour accompagner cette rubrique : [télécharger](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Si vous avez des questions à propos de ce didacticiel, veuillez les valider pour le [forum MVC](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Vue d'ensemble

L’application que vous créez est un site Web de liste utilisateur simple. Les utilisateurs peuvent entrer, afficher et mettre à jour les informations utilisateur.

![Exemple de site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Vous pouvez télécharger le projet achevé VB et c# [ici](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Création de l’Application Web

Pour démarrer le didacticiel, ouvrez Visual Studio 2010 et créer un projet à l’aide du *Application Web de ASP.NET MVC 3* modèle. Nommez l’application &quot;Mvc3Razor&quot;.

[![Nouveau projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Dans le **nouveau projet ASP.NET MVC 3** boîte de dialogue, sélectionnez **Application Internet**, sélectionnez le moteur d’affichage Razor, puis cliquez sur **OK**.

![Boîte de dialogue Nouveau projet ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

Dans ce didacticiel vous ne serez pas utiliser le fournisseur d’appartenances ASP.NET, donc vous pouvez supprimer tous les fichiers associés d’ouverture de session et d’appartenance. Dans **l’Explorateur de solutions**, supprimez les fichiers et répertoires suivants :

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (et tous les fichiers dans ce répertoire)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Modifier le  <em>\_Layout.cshtml</em> fichier et remplacez le balisage à l’intérieur de la `<div>` élément nommé `logindisplay` avec le message <em>&quot;</em>connexion désactivé&quot;. L’exemple suivant montre le balisage :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Ajout du modèle

Dans **l’Explorateur de solutions**, avec le bouton droit le *modèles* dossier, sélectionnez **ajouter**, puis cliquez sur **classe**.

![Nouvelle classe utilisateur Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nommez la classe `UserModel`. Remplacez le contenu de la *UserModel* fichier par le code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

Le `UserModel` classe représente les utilisateurs. Chaque membre de la classe est annoté avec le [requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribut à partir de la [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms. Les attributs dans le [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms fournissent une validation côté client et serveur automatique pour les applications web.

Ouvrir le `HomeController` classe et ajoutez un `using` directive afin que vous pouvez accéder à la `UserModel` et `Users` classes :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Juste après le `HomeController` déclaration, ajoutez le commentaire suivant et la référence à un `Users` classe :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

Le `Users` classe est un magasin de données simplifiée, en mémoire que vous utiliserez dans ce didacticiel. Dans une application réelle, vous utiliseriez une base de données pour stocker les informations utilisateur. Les premières lignes de la `HomeController` fichier sont affichés dans l’exemple suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Générez l’application afin que le modèle utilisateur sera disponible pour l’Assistant génération de modèles automatique à l’étape suivante.

## <a name="creating-the-default-view"></a>Création de la vue par défaut

L’étape suivante consiste à ajouter une méthode d’action et pour afficher les utilisateurs.

Supprimez le *Views\Home\Index* fichier. Vous allez créer un nouveau *Index* fichier pour afficher les utilisateurs.

Dans le `HomeController` de classe, remplacez le contenu de la `Index` méthode avec le code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Avec le bouton droit à l’intérieur de la `Index` (méthode), puis cliquez sur **ajouter une vue**.

![Ajouter une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Sélectionnez le **créer une vue fortement typée** option. Pour **afficher la classe de données**, sélectionnez **Mvc3Razor.Models.UserModel**. (Si vous ne voyez pas **Mvc3Razor.Models.UserModel** dans le **afficher la classe de données** boîte, vous avez besoin générer le projet.) Assurez-vous que le moteur d’affichage est défini sur **Razor**. Définissez **afficher le contenu** à **liste** puis cliquez sur **ajouter**.

![Ajouter une vue d’Index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nouvelle vue de structure automatiquement les données utilisateur qui sont passées à la `Index` vue. Examiner le nouvellement généré *Views\Home\Index* fichier. Le **créer un nouveau**, **modifier**, **détails**, et **supprimer** liens ne fonctionnent pas, mais le reste de la page est fonctionnel. Exécutez la page. Vous voyez une liste d’utilisateurs.

![Page d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Ouvrez le *Index.cshtml* fichier et remplacez le `ActionLink` balisage pour **modifier**, **détails**, et **supprimer** par le code suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Le nom d’utilisateur est utilisé comme ID pour rechercher l’enregistrement sélectionné dans le **modifier**, **détails**, et **supprimer** des liens.

## <a name="creating-the-details-view"></a>Création de la vue Détails

L’étape suivante consiste à ajouter un `Details` méthode d’action et de la vue afin d’afficher les détails de l’utilisateur.

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Ajoutez le code suivant `Details` méthode au contrôleur home :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Avec le bouton droit à l’intérieur de la `Details` (méthode), puis sélectionnez <strong>ajouter une vue</strong>. Vérifiez que le <strong>afficher la classe de données</strong> boîte contient <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Définissez <strong>afficher le contenu</strong> à <strong>détails</strong> puis cliquez sur <strong>ajouter</strong>.

![Ajouter une vue de détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Exécutez l’application et sélectionnez un lien Détails. La génération de modèles automatique montre chaque propriété dans le modèle.

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Création de la vue de modification

Ajoutez le code suivant `Edit` méthode au contrôleur home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Ajouter une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **modifier**.

![Ajouter une vue de modification](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Exécutez l’application et modifier le prénom et le nom d’un des utilisateurs. Si vous violez l’une `DataAnnotation` contraintes qui ont été appliqués à la `UserModel` (classe), lorsque vous envoyez le formulaire, vous verrez des erreurs de validation qui se sont produites par le code serveur. Par exemple, si vous modifiez le nom de la première &quot;Ann&quot; à &quot;A&quot;, lorsque vous envoyez le formulaire, l’erreur suivante s’affiche sur le formulaire :

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

Dans ce didacticiel, vous êtes en traitant le nom d’utilisateur comme clé primaire. Par conséquent, la propriété de nom d’utilisateur ne peut pas être modifiée. Dans le *Edit.cshtml* de fichiers, juste après la `Html.BeginForm` instruction, définissez le nom d’utilisateur doit être un champ masqué. Cela entraîne la propriété devant être transmis dans le modèle. Le fragment de code suivant montre le placement de la `Hidden` instruction :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Remplacez le `TextBoxFor` et `ValidationMessageFor` balisage pour le nom d’utilisateur avec un `DisplayFor` appeler. Le `DisplayFor` méthode affiche la propriété sous la forme d’un élément en lecture seule. L’exemple suivant montre le balisage complet. La version d’origine `TextBoxFor` et `ValidationMessageFor` appels sont mis en commentaire avec les caractères de commentaire begin et end-commentaire Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>L’activation de la Validation côté Client

Pour activer la validation côté client dans ASP.NET MVC 3, vous devez définir deux indicateurs, et vous devez inclure les trois fichiers JavaScript.

Ouvrez l’application *Web.config* fichier. Vérifiez `that ClientValidationEnabled` et `UnobtrusiveJavaScriptEnabled` sont définies sur true dans les paramètres d’application. Le fragment suivant à partir de la racine *Web.config* fichier montre les paramètres appropriés :

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Paramètre `UnobtrusiveJavaScriptEnabled` sur vrai Ajax discrète active et validation client non obstructive. Lorsque vous utilisez la validation non obstrusive, les règles de validation sont transformées en attributs HTML5. Noms d’attributs HTML5 peuvent contenir uniquement des lettres minuscules, chiffres et des tirets.

Paramètre `ClientValidationEnabled` à la valeur true permet la validation côté client. En définissant ces clés dans l’application *Web.config* fichier, vous activez la validation côté client et JavaScript non obstrusif pour l’application entière. Vous pouvez également activer ou désactiver ces paramètres dans les vues individuelles ou les méthodes de contrôleur en utilisant le code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Vous devez également inclure plusieurs fichiers JavaScript dans l’affichage. Un moyen simple d’inclure le code JavaScript dans toutes les vues consiste à les ajouter à la *Views\Shared\\_Layout.cshtml* fichier. Remplacez le `<head>` élément de la  *\_Layout.cshtml* fichier par le code suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Les deux premiers scripts jQuery sont hébergés par le Microsoft Ajax Content Delivery Network (CDN). En tirant parti du CDN Microsoft Ajax, vous pouvez considérablement améliorer les performances du premier accès de vos applications.

Exécutez l’application et cliquez sur un lien de modification. Afficher le code source dans le navigateur. La source du navigateur montre beaucoup d’attributs sous la forme `data-val` (pour la validation de données). Lors de la validation côté client et le code JavaScript non obstrusif est activé, les champs d’entrée avec une règle de validation côté client contiennent le `data-val="true"` attribut pour déclencher la validation client non obstructive. Par exemple, le `City` champ dans le modèle a été décorée avec le [requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribut, ce qui se traduit dans le code HTML indiqué dans l’exemple suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Pour chaque règle de validation côté client, un attribut est ajouté qui présente sous la forme `data-val-rulename="message"`. À l’aide de la `City` champ exemple montré précédemment, la règle de validation côté client nécessaire génère le `data-val-required` attribut et le message &quot;champ de la ville est obligatoire&quot;. Exécutez l’application, modifiez un des utilisateurs et effacer le `City` champ. Si vous le déplacez hors du champ, vous voyez un message d’erreur de validation côté client.

![Ville requis](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

De même, pour chaque paramètre dans la règle de validation côté client, un attribut est ajouté qui présente sous la forme `data-val-rulename-paramname=paramvalue`. Par exemple, le `FirstName` propriété est annotée avec le [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) d’attribut et spécifie une longueur minimale de 3 et une longueur maximale de 8. La règle de validation de données nommée `length` porte le nom de paramètre `max` et la valeur du paramètre 8. L’exemple suivant montre le code HTML généré pour le `FirstName` champ lorsque vous modifiez un des utilisateurs :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Pour plus d’informations sur la validation client non obstructive, consultez l’entrée [Unobtrusive validation côté Client dans ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) dans le blog de Brad Wilson.

> [!NOTE]
> Dans la version bêta d’ASP.NET MVC 3, vous avez parfois besoin envoyer le formulaire afin de démarrer la validation côté client. Cela peut être modifié pour la version finale.


## <a name="creating-the-create-view"></a>Création de la vue Create

L’étape suivante consiste à ajouter un `Create` méthode d’action et de la vue afin de permettre à l’utilisateur créer un nouvel utilisateur. Ajoutez le code suivant `Create` méthode au contrôleur home :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Ajouter une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **créer**.

![Créer une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Exécutez l’application, sélectionnez le **créer** lier, puis ajoutez un nouvel utilisateur. Le `Create` méthode tire automatiquement parti de validation côté client et côté serveur. Essayez d’entrer un nom d’utilisateur qui contient un espace blanc, tel que &quot;Ben X&quot;. Lorsque vous quittez le champ nom d’utilisateur, une erreur de validation côté client (`White space is not allowed`) s’affiche.

## <a name="add-the-delete-method"></a>Ajoutez la méthode Delete

Pour suivre ce didacticiel, ajoutez le code suivant `Delete` méthode au contrôleur home :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Ajouter un `Delete` vue comme dans les étapes précédentes, en définissant **afficher le contenu** à **supprimer**.

![Supprimer la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Vous disposez maintenant d’une application ASP.NET MVC 3 simple mais totalement opérationnelle avec la validation.
