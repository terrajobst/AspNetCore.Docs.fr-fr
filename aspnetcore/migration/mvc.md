---
title: Migrer de ASP.NET MVC vers ASP.NET Core MVC
author: ardalis
description: Découvrez comment commencer à migrer un projet ASP.NET MVC vers ASP.NET Core MVC.
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: 6c9449fb43960d05db8aa6dcba64d3d830834cdb
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661167"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrer de ASP.NET MVC vers ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/)et [Scott Addie](https://scottaddie.com)

Cet article explique comment prendre en main la migration d’un projet ASP.NET MVC vers [ASP.net Core MVC](../mvc/overview.md). Dans le processus, il met en évidence un grand nombre des éléments qui ont été modifiés par rapport à ASP.NET MVC. La migration à partir de ASP.NET MVC est un processus en plusieurs étapes et cet article couvre l’installation initiale, les contrôleurs de base et les vues, le contenu statique et les dépendances côté client. D’autres articles traitent de la migration du code de configuration et d’identité trouvé dans de nombreux projets MVC ASP.NET.

> [!NOTE]
> Les numéros de version dans les exemples ne sont peut-être pas ceux en cours. Vous devrez peut-être mettre à jour vos projets en conséquence.

## <a name="create-the-starter-aspnet-mvc-project"></a>Créer le projet MVC ASP.NET de démarrage

Pour illustrer la mise à niveau, nous allons commencer par la création d’une application ASP.NET MVC. Créez-le avec le nom *application Web 1* afin que l’espace de noms corresponde au projet ASP.net Core que nous créons à l’étape suivante.

![Boîte de dialogue Nouveau projet Visual Studio](mvc/_static/new-project.png)

![Boîte de dialogue nouvelle application Web : modèle de projet MVC sélectionné dans le panneau modèles ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Facultatif :* Remplacez le nom de la solution *application Web 1* par *Mvc5*. Visual Studio affiche le nouveau nom de la solution (*Mvc5*), ce qui vous permet d’indiquer plus facilement ce projet à partir du projet suivant.

## <a name="create-the-aspnet-core-project"></a>Créer le projet ASP.NET Core

Créez une application Web ASP.NET Core *vide* portant le même nom que le projet précédent (*application Web 1*) de sorte que les espaces de noms des deux projets correspondent. Avoir le même espace de noms rend plus facile la copie de code entre les deux projets. Vous devez créer ce projet dans un autre répertoire que le projet précédent pour utiliser le même nom.

![Boîte de dialogue Nouveau projet](mvc/_static/new_core.png)

![Boîte de dialogue nouvelle application Web ASP.NET : modèle de projet vide sélectionné dans ASP.NET Core panneau modèles](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Facultatif :* Créez une nouvelle ASP.NET Core application à l’aide du modèle de projet d' *application Web* . Nommez le projet *application Web 1*, puis sélectionnez l’option d’authentification des **comptes d’utilisateur individuels**. Renommez cette application en *FullAspNetCore*. La création de ce projet vous fait gagner du temps lors de la conversion. Vous pouvez consulter le code généré par le modèle pour afficher le résultat final ou copier le code dans le projet de conversion. Cela est également utile lorsque vous vous rendez bloqué sur une étape de conversion pour comparer avec le projet généré par modèle.

## <a name="configure-the-site-to-use-mvc"></a>Configurer le site pour utiliser MVC

::: moniker range=">= aspnetcore-2.1"

* Quand vous ciblez .NET Core, le [AspNetCore Microsoft. app](xref:fundamentals/metapackage-app) est référencé par défaut. Ce package contient des packages couramment utilisés par les applications MVC. Si vous ciblez .NET Framework, les références de package doivent être listées individuellement dans le fichier projet.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Quand vous ciblez .NET Core, le [AspNetCore Microsoft. All](xref:fundamentals/metapackage) est référencé par défaut. Ce package contient les packages les plus couramment utilisés par les applications MVC. Si vous ciblez .NET Framework, les références de package doivent être listées individuellement dans le fichier projet.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* Quand vous ciblez .NET Core ou .NET Framework, les packages couramment utilisés par les applications MVC sont répertoriés individuellement dans le fichier projet.

::: moniker-end

`Microsoft.AspNetCore.Mvc` est l’infrastructure MVC ASP.NET Core. `Microsoft.AspNetCore.StaticFiles` est le gestionnaire de fichiers statiques. Le runtime ASP.NET Core est modulaire, et vous devez explicitement choisir de servir des fichiers statiques (voir [fichiers statiques](xref:fundamentals/static-files)).

* Ouvrez le fichier *Startup.cs* et modifiez le code pour qu’il corresponde à ce qui suit :

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

La méthode d’extension `UseStaticFiles` ajoute le gestionnaire de fichiers statiques. Comme mentionné précédemment, le runtime ASP.NET est modulaire, et vous devez explicitement autoriser les fichiers statiques. La méthode d’extension `UseMvc` ajoute le routage. Pour plus d’informations, consultez démarrage et [routage](xref:fundamentals/routing)de l' [application](xref:fundamentals/startup) .

## <a name="add-a-controller-and-view"></a>Ajouter un contrôleur et une vue

Dans cette section, vous allez ajouter un contrôleur minimal et la vue comme des espaces réservés pour le contrôleur ASP.NET MVC et les vues que vous allez migrer dans la section suivante.

* Ajoutez un dossier *Controllers* .

* Ajoutez une **classe de contrôleur** nommée *HomeController.cs* au dossier *Controllers* .

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/add_mvc_ctl.png)

* Ajoutez un dossier *views* .

* Ajoutez un dossier *views/de départ* .

* Ajoutez une **vue Razor** nommée *index. cshtml* au dossier *views/de départ* .

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/view.png)

La structure du projet est indiquée ci-dessous :

![Explorateur de solutions de l’indication des fichiers et des dossiers de application Web 1](mvc/_static/project-structure-controller-view.png)

Remplacez le contenu du fichier *views/page d’index. cshtml* par le suivant :

```html
<h1>Hello world!</h1>
```

Exécutez l'application.

![Application Web ouverte dans Microsoft Edge](mvc/_static/hello-world.png)

Pour plus d’informations, consultez [contrôleurs](xref:mvc/controllers/actions) et [vues](xref:mvc/views/overview) .

Maintenant que nous avons un projet ASP.NET Core minimal, nous pouvons commencer la migration des fonctionnalités à partir du projet ASP.NET MVC. Nous devons déplacer les éléments suivants :

* Contenu côté client (CSS, polices et scripts)

* controllers

* views

* modèles

* Regroupement

* filtres

* Connexion/sortie, identité (cette opération est effectuée dans le didacticiel suivant).

## <a name="controllers-and-views"></a>Contrôleurs et vues

* Copiez chacune des méthodes du `HomeController` MVC ASP.NET vers le nouvel `HomeController`. Notez que dans ASP.NET MVC, le type de retour de la méthode d’action de contrôleur du modèle intégré est [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); dans ASP.NET Core MVC, les méthodes d’action retournent `IActionResult` à la place. `ActionResult` implémente `IActionResult`, il n’est donc pas nécessaire de modifier le type de retour de vos méthodes d’action.

* Copiez les fichiers de vue Razor *about. cshtml*, *contact. cshtml*et *index. cshtml* du projet MVC ASP.net dans le projet ASP.net core.

* Exécutez l’application ASP.NET Core et chaque méthode de test. Nous n’avons pas encore migré le fichier de disposition ou les styles, de sorte que les vues rendues contiennent uniquement le contenu des fichiers de vue. Vous n’avez pas de liens générés par le fichier de disposition pour les vues `About` et `Contact`. vous devrez donc les appeler à partir du navigateur (remplacez **4492** par le numéro de port utilisé dans votre projet).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Page de contact](mvc/_static/contact-page.png)

Notez l’absence de styles et d’éléments de menu. Nous le corrigerons dans la section suivante.

## <a name="static-content"></a>Contenu statique

Dans les versions précédentes de ASP.NET MVC, le contenu statique était hébergé à partir de la racine du projet Web et a été mélangé aux fichiers côté serveur. Dans ASP.NET Core, le contenu statique est hébergé dans le dossier *wwwroot* . Vous pouvez copier le contenu statique de votre ancienne application ASP.NET MVC dans le dossier *wwwroot* de votre projet ASP.net core. Dans cette exemple de conversion :

* Copiez le fichier *favicon. ico* de l’ancien projet MVC dans le dossier *wwwroot* du projet ASP.net core.

L’ancien projet MVC ASP.NET utilise [bootstrap](https://getbootstrap.com/) pour son style et stocke les fichiers de démarrage dans les dossiers *content* et *scripts* . Le modèle, qui a généré l’ancien projet MVC ASP.NET, référence le bootstrap dans le fichier de disposition (*Views/Shared/_Layout. cshtml*). Vous pouvez copier les fichiers *bootstrap. js* et *bootstrap. CSS* à partir du projet MVC ASP.net dans le dossier *wwwroot* dans le nouveau projet. Au lieu de cela, nous ajouterons la prise en charge des démarrages (et d’autres bibliothèques côté client) à l’aide de CDN dans la section suivante.

## <a name="migrate-the-layout-file"></a>Migrer le fichier de disposition

* Copiez le fichier *_ViewStart. cshtml* du dossier *views* de l’ancien projet MVC ASP.net dans le dossier *views* du projet ASP.net core. Le fichier *_ViewStart. cshtml* n’a pas été modifié dans ASP.net Core Mvc.

* Créez un dossier *Views/Shared* .

* *Facultatif :* Copiez *_ViewImports. cshtml* du dossier *views* du projet MVC *FullAspNetCore* dans le dossier *views* du projet ASP.net core. Supprimez toute déclaration d’espace de noms dans le fichier *_ViewImports. cshtml* . Le fichier *_ViewImports. cshtml* fournit des espaces de noms pour tous les fichiers de vue et insère les [balises](xref:mvc/views/tag-helpers/intro). Les tag helpers sont utilisés dans le nouveau fichier de disposition. Le fichier *_ViewImports. cshtml* est nouveau pour ASP.net core.

* Copiez le fichier *_Layout. cshtml* à partir de l’ancien dossier *Views/Shared* du projet MVC ASP.net dans le dossier *Views/Shared* du projet ASP.net core.

Ouvrez *_Layout fichier. cshtml* et apportez les modifications suivantes (le code complet est indiqué ci-dessous) :

* Remplacez `@Styles.Render("~/Content/css")` par un élément `<link>` pour charger *bootstrap. CSS* (voir ci-dessous).

* Supprimez `@Scripts.Render("~/bundles/modernizr")`.

* Commentez la ligne de `@Html.Partial("_LoginPartial")` (entourez la ligne de `@*...*@`). Pour plus d’informations [, consultez migrer l’authentification et l’identité vers ASP.net Core](xref:migration/identity)

* Remplacez `@Scripts.Render("~/bundles/jquery")` par un élément `<script>` (voir ci-dessous).

* Remplacez `@Scripts.Render("~/bundles/bootstrap")` par un élément `<script>` (voir ci-dessous).

Balisage de remplacement pour l’inclusion CSS de démarrage :

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

Balisage de remplacement pour l’inclusion de jQuery et du code d’amorçage JavaScript :

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

Le fichier *_Layout. cshtml* mis à jour est affiché ci-dessous :

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Affichez le site dans le navigateur. Elle doit maintenant se charger correctement, avec les styles attendus en place.

* *Facultatif :* Vous souhaiterez peut-être essayer d’utiliser le nouveau fichier de disposition. Pour ce projet, vous pouvez copier le fichier de disposition à partir du projet *FullAspNetCore* . Le nouveau fichier de disposition utilise [tag Helper](xref:mvc/views/tag-helpers/intro) et présente d’autres améliorations.

## <a name="configure-bundling-and-minification"></a>Configurer le regroupement et la minimisation

Pour plus d’informations sur la configuration du regroupement et de la minimisation, consultez [regroupement et minimisation](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Résoudre les erreurs HTTP 500

De nombreux problèmes peuvent provoquer un message d’erreur HTTP 500 qui ne donne aucune information sur la source du problème. Par exemple, si le fichier *views/_ViewImports. cshtml* contient un espace de noms qui n’existe pas dans votre projet, vous obtiendrez une erreur HTTP 500. Par défaut, dans ASP.NET Core Apps, l’extension de `UseDeveloperExceptionPage` est ajoutée au `IApplicationBuilder` et exécutée lors du *développement*de la configuration. Ce code est détaillé dans le code suivant :

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core convertit les exceptions non gérées dans une application Web en réponses d’erreur HTTP 500. Normalement, les détails de l’erreur ne sont pas inclus dans ces réponses pour empêcher la divulgation d’informations potentiellement sensibles sur le serveur. Pour plus d’informations, consultez **utilisation de la page d’exception de développeur** dans gérer les [Erreurs](../fundamentals/error-handling.md) .

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
