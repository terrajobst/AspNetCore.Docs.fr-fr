---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (inclut avril 2011 mise à jour des outils) ASP.NET MVC 3 est une infrastructure pour générer des applications web évolutive et basée sur des normes à l’aide du modèle de conception bien établis...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 82d18865815568c5df9768fd9dd403f11ebd1714
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830978"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(inclut avril 2011 mise à jour des outils)*
> 
> ASP.NET MVC 3 est une infrastructure pour générer des applications web évolutive et basée sur des normes à l’aide de modèles de conception bien établis et la puissance de ASP.NET et .NET Framework.
> 
> Il s’installe côte à côte avec ASP.NET MVC 2, donc l’utiliser dès aujourd'hui !
> 
> Téléchargez le [programme d’installation ici](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Fonctionnalités principales

- Système de génération de modèles automatique intégré extensible via NuGet
- Modèles de projet activé 5 HTML
- Vues expressifs, y compris le nouveau moteur d’affichage Razor
- Raccordements puissants avec l’Injection de dépendances et les filtres d’Action globale
- Prise en charge de JavaScript de riches avec JavaScript discret, jQuery Validation et la liaison de JSON
- *Lire la liste complète des fonctionnalités [ci-dessous](#overview)*

## <a name="top-links"></a>Principaux liens

Quelles sont les nouveautés dans ASP.NET MVC 3

- Phil Haack : [ASP.NET MVC 3 est disponible](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman : [ASP.NET MVC3, WebMatrix, NuGet, IIS Express et Orchard nouveau tout chaud – la version Web de janvier de Microsoft dans le contexte](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie : [annonce de version d’ASP.NET MVC 3, IIS Express, SQL CE 4, infrastructure de batterie de serveurs Web, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notes de publication pour ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installation et l’aide

- Installation à l’aide de ASP.NET MVC 3 le [Web Platform Installer (recommandé)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installation à l’aide de ASP.NET MVC 3 le [programme d’installation exécutable](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installer [ASP.NET MVC 3 pour Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lire le [Introduction à ASP.NET MVC 3 du didacticiel](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtenir de l’aide et de discuter d’ASP.NET MVC 3 dans le [forums](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Vue d’ensemble d’ASP.NET MVC 3

ASP.NET MVC 3 s’appuie sur ASP.NET MVC 1 et 2, en ajoutant des fonctionnalités exceptionnelles que les deux simplifieront votre code et permettent l’extensibilité plus approfondie. Cette rubrique fournit une vue d’ensemble d’un grand nombre des nouvelles fonctionnalités qui sont incluses dans cette version, dans les sections suivantes :

- [Structure extensible avec l’intégration de MvcScaffold](#BM_MvcScaffolding)
- [Modèles de projet activé 5 HTML](#BM_HTML5)
- [Le moteur d’affichage Razor](#BM_TheRazorViewEngine)
- [Prise en charge de plusieurs moteurs d’affichage](#BM_Support_for_Multiple_View_Engines)
- [Améliorations de contrôleur](#BM_Controller_Improvements)
- [JavaScript et Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Améliorations de Validation de modèle](#BM_Model_Validation_Improvements)
- [Améliorations de l’Injection de dépendance](#BM_Dependency_Injection_Improvements)
- [Autres nouvelles fonctionnalités](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Structure extensible avec l’intégration de MvcScaffold

Le nouveau système de génération de modèles automatique rend plus facile à assimiler et commencer à utiliser de manière productive, si vous êtes entièrement novice dans l’infrastructure et à automatiser les tâches de développement courantes si vous êtes expérimenté et savez déjà ce que vous faites.

Cela est pris en charge par NuGet nouvelle *la structure* package appelé **MvcScaffolding**. Le terme « Génération de modèles automatique » est utilisée par nombreuses technologies de logiciels comme signifiant « générer rapidement une présentation générale de votre logiciel que vous pouvez ensuite modifier et personnaliser ». Le package de la structure que nous créons pour ASP.NET MVC est considérablement avantageux dans plusieurs scénarios :

- **Si vous êtes apprentissage de MVC ASP.NET pour la première fois**, car elle vous offre un moyen rapide pour obtenir un code de travail utile, que vous pouvez ensuite modifier et adapter en fonction de vos besoins. Il permet d’éviter de l’urgence de la recherche à une page vide et aucune idée où commencer !
- **Si vous connaissez bien les ASP.NET MVC et explorez maintenant de nouvelles technologies de module complémentaire** comme un mappeur objet-relationnel, un moteur d’affichage, une bibliothèque de test, etc., étant donné que le créateur de cette technologie peut également avoir créé un package de génération de modèles automatique pour celui-ci.
- **Si votre travail implique la création à plusieurs reprises similaires de classes ou de fichiers quelconque**, car vous pouvez créer des générateurs de structure personnalisés qui produisent des contextes de test, de scripts de déploiement ou de tout ce qui vous avez besoin. Tous les membres de votre équipe peuvent utiliser vos générations de modèles automatiques personnalisées, trop.

Autres fonctionnalités de MvcScaffolding incluent :

- Prise en charge pour les projets c# et VB
- Prise en charge de Razor les ASPX afficher moteurs
- Prend en charge la génération de modèles automatique dans des zones d’ASP.NET MVC et à l’aide de la vue personnalisée des dispositions/maîtres
- Vous pouvez facilement personnaliser la sortie en modifiant les modèles T4
- Vous pouvez ajouter les générateurs de structure entièrement nouvelles à l’aide d’une logique personnalisée PowerShell et les modèles T4 personnalisés. Ces (et les paramètres personnalisés que vous leur avez donné) s’affichent automatiquement dans la liste de saisie semi-automatique par tabulation de console.
- Vous pouvez obtenir les packages NuGet contenant les générateurs de structure supplémentaires pour les différentes technologies (par exemple, il existe une preuve de concept une pour LINQ to SQL désormais) et de combiner les ensemble

ASP.NET MVC 3 Tools Update comprend excellente prise en charge de Visual Studio pour ce système de génération de modèles automatique, telles que :

- Ajouter la que boîte de dialogue contrôleur prend désormais en charge la génération de modèles automatique complète automatique des actions de contrôleur Create, Read, Update et Delete et des vues correspondantes. Par défaut, cette structure de code d’accès aux données à l’aide d’Entity Framework Code First.
- Prend en charge de la boîte de dialogue contrôleur *squelettes extensibles* via NuGet packages tels que *MvcScaffolding*. Cela permet le branchement de squelettes personnalisés dans la boîte de dialogue qui vous permet de créer des squelettes pour d’autres technologies d’accès aux données comme NHibernate ou même JET avec ODBCDirect si vous le souhaitez !

Pour plus d’informations sur la génération de modèles automatique dans ASP.NET MVC 3, consultez les ressources suivantes :

- Steve Sanderson valider des séries, notamment : 

    1. [Introduction : Structurer votre projet ASP.NET MVC 3 avec le package MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [L’utilisation standard : scénarios d’utilisation classiques et les options](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relations un-à-plusieurs](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Actions de génération de modèles automatique et les Tests unitaires](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Substitution de modèles T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Ce billet : création des générateurs de structure personnalisés](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Billet de Scott Hanselman à partir de sa session PDC 2010 [création d’un Blog avec Microsoft « Sans nom Package de Web amour »](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notes de mise à jour de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modèles de projet 5 HTML

La boîte de dialogue Nouveau projet inclut une case à cocher Activer HTML 5 des versions de modèles de projet. Ces modèles utiliser Modernizr 1.7 pour fournir la prise en charge de compatibilité de HTML 5 et 3 de CSS dans les navigateurs de bas niveau.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Le moteur d’affichage Razor

ASP.NET MVC 3 est fourni avec un nouveau moteur d’affichage nommé Razor qui offre les avantages suivants :

- Syntaxe Razor est claire et concise, nécessitant un nombre minimal de séquences de touches.
- Razor est facile à apprendre, en partie, car il est basé sur les langages existants, tels que c# et Visual Basic.
- Visual Studio inclut la colorisation de code et IntelliSense pour la syntaxe Razor.
- Les vues Razor peuvent être des tests unitaires sans devoir exécuter l’application ou lancer un serveur web.

Certaines nouvelles fonctionnalités de Razor sont les suivantes :

- `@model` syntaxe pour spécifier le type transmis à la vue.
- `@* *@` syntaxe de commentaire.
- La possibilité de spécifier les valeurs par défaut (tel que `layoutpage`) une fois pour un site entier.
- Le `Html.Raw` méthode pour afficher du texte sans codage HTML il.
- Prise en charge de partager du code entre plusieurs vues (*\_viewstart.cshtml* ou  *\_viewstart.vbhtml* fichiers).

Razor inclut également les nouveaux programmes d’assistance HTML, telles que les éléments suivants :

- `Chart`. Affiche un graphique, offre les mêmes fonctionnalités que le contrôle chart dans ASP.NET 4.
- `WebGrid`. Affiche une grille de données complète avec la fonctionnalité de pagination et de tri.
- `Crypto`. Utilise le hachage d’algorithmes pour créer correctement salés et hachés les mots de passe.
- `WebImage`. Restitue une image.
- `WebMail`. Envoie un e-mail.

Pour plus d’informations sur Razor, consultez les ressources suivantes :

- [Billet de blog Guthrie présentation de Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Présentation de billet de blog Guthrie le @model mot clé](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Présentation des dispositions Razor de billet de blog de Guthrie](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Référence rapide de l’API de Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notes de mise à jour de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Prise en charge de plusieurs moteurs d’affichage

Le **ajouter une vue** boîte de dialogue dans ASP.NET MVC 3 vous permet de choisir le moteur d’affichage que vous souhaitez utiliser, et le **nouveau projet** boîte de dialogue vous permet de spécifier le moteur d’affichage par défaut pour un projet. Vous pouvez choisir le moteur d’affichage Web Forms (ASPX), Razor ou un moteur d’affichage de l’open source comme [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), ou [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Améliorations de contrôleur

### <a name="global-action-filters"></a>Filtres d’Action globaux

Parfois, vous souhaitez exécuter la logique avant l’exécution d’une méthode d’action ou après l’exécution d’une méthode d’action. Pour ce faire, ASP.NET MVC 2 fourni des filtres d’action. Filtres d’action sont des attributs personnalisés qui fournissent un moyen déclaratif pour ajouter un comportement pré et post-action aux méthodes d’action de contrôleur spécifique. Toutefois, dans certains cas, vous souhaiterez spécifient le comportement d’une action préalable ou une post-action s’applique à toutes les méthodes d’action. MVC 3 vous permet de spécifier des filtres globaux en les ajoutant à la `GlobalFilters` collection. Pour plus d’informations sur les filtres d’action globaux, consultez les ressources suivantes :

- [Blog Guthrie sur la version d’évaluation MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrage dans ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nouvelle propriété « ViewBag »

Prise en charge des contrôleurs MVC 2 un `ViewData` propriété qui vous permet de passer des données à un modèle de vue à l’aide d’un API de dictionnaire à liaison tardive. Dans MVC 3, vous pouvez également utiliser une syntaxe un peu plus simple avec le `ViewBag` propriété pour accomplir le même but. Par exemple, au lieu d’écrire `ViewData["Message"]="text"`, vous pouvez écrire `ViewBag.Message="text"`. Vous n’avez pas besoin de définir des classes fortement typées à utiliser le `ViewBag` propriété. S’agissant d’une propriété dynamique, vous pouvez simplement obtenir ou définir les propriétés et les résoudre dynamiquement au moment de l’exécution. En interne, `ViewBag` propriétés sont stockées sous forme de paires nom/valeur dans la `ViewData` dictionnaire. (Remarque : dans la plupart des versions préliminaires de MVC 3, le `ViewBag` propriété s’appelait la `ViewModel` propriété.)

### <a name="new-actionresult-types"></a>Nouveaux Types de « ActionResult »

Ce qui suit `ActionResult` types et les méthodes d’assistance correspondants sont nouvelles ou améliorées dans MVC 3 :

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Retourne un code d’état HTTP 404 au client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Retourne une redirection temporaire (code d’état HTTP 302) ou une redirection permanente (code d’état HTTP 301), en fonction d’un paramètre booléen. Conjointement avec cette modification, le [contrôleur](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) classe possède maintenant trois méthodes pour effectuer des redirections permanentes : `RedirectPermanent`, `RedirectToRoutePermanent`, et `RedirectToActionPermanent`. Ces méthodes retournent une instance de `RedirectResult` avec la `Permanent` propriété définie sur `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Retourne un code d’état HTTP spécifié par l’utilisateur.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript et les améliorations Ajax

Par défaut, les programmes d’assistance Ajax et la validation dans MVC 3 utilisent une approche JavaScript non obstrusive. JavaScript non obstrusif évite l’injection de code JavaScript intégré dans HTML. Cela rend votre code HTML plus petits et moins encombrée et le rend plus facile à permuter, ou personnaliser des bibliothèques JavaScript. Programmes d’assistance de validation dans MVC 3 utilisent également le `jQueryValidate` plug-in par défaut. Si vous souhaitez que le comportement de MVC 2, vous pouvez désactiver JavaScript non obstrusive à l’aide un *web.config* fichier de paramètre. Pour plus d’informations sur les améliorations de JavaScript et Ajax, consultez les ressources suivantes :

- [Présentation générale du JavaScript discret sur le site de Wikipédia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Billet de JavaScript discret de Brad Wilson.](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Billet de Validation JavaScript non Obstrusif de Brad Wilson.](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Création d’une Application MVC 3 avec Razor et JavaScript non Obstrusif](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (didacticiel sur le site ASP.NET)
- [Notes de mise à jour de MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validation côté client activée par défaut

Dans les versions antérieures de MVC, vous devez appeler explicitement la `Html.EnableClientValidation` méthode à partir d’une vue afin d’activer la validation côté client. Dans MVC 3 cela n’est plus nécessaire, car la validation côté client est activée par défaut. (Vous pouvez le désactiver à l’aide d’un paramètre dans le *web.config* fichier.)

Dans l’ordre pour la validation côté client fonctionne, vous devez toujours référencer les jQuery approprié et les bibliothèques de Validation jQuery dans votre site. Vous pouvez héberger ces bibliothèques sur votre propre serveur ou les référencer à partir d’un réseau de distribution de contenu (CDN) comme les CDN à partir de Microsoft ou Google.

### <a name="remote-validator"></a>Programme de validation à distance

ASP.NET MVC 3 prend en charge la nouvelle [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) prise en charge du programme de validation à distance de la classe qui vous permet de tirer parti de la Validation jQuery du plug-in. Ainsi, la bibliothèque de validation côté client appeler automatiquement une méthode personnalisée que vous définissez sur le serveur afin d’exécuter la logique de validation peut uniquement être effectuée côté serveur.

Dans l’exemple suivant, le `Remote` attribut spécifie que la validation côté client appellera une action nommée `UserNameAvailable` sur le `UsersController` classe afin de valider le `UserName` champ.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

L’exemple suivant montre le contrôleur correspondant.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Pour plus d’informations sur l’utilisation de la `Remote` d’attribut, consultez [Comment : implémenter la Validation à distance dans ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) dans MSDN library.

### <a name="json-binding-support"></a>Prise en charge de liaison de JSON

ASP.NET MVC 3 inclut JSON liaison prise en charge intégrée qui permet aux méthodes d’action recevoir des données JSON et de lier par modèle il aux paramètres de méthode d’action. Cette fonctionnalité est utile dans les scénarios impliquant des modèles de client et la liaison de données. (Les modèles client permettent à mettre en forme et afficher un élément de données unique ou un ensemble d’éléments de données à l’aide de modèles qui s’exécutent sur le client.) MVC 3 vous permet de connecter facilement des modèles de client avec les méthodes d’action sur le serveur qui envoient et reçoivent des données JSON. Pour plus d’informations sur la prise en charge de liaison de JSON, consultez le **JavaScript et AJAX améliorations** section de [billet de blog Guthrie MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Améliorations de Validation de modèle

### <a name="dataannotations-metadata-attributes"></a>Attributs de métadonnées « DataAnnotations »

ASP.NET MVC 3 prend en charge `DataAnnotations` attributs de métadonnées tels que `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe de « ValidationAttribute »

Le `ValidationAttribute` classe a été améliorée dans le .NET Framework 4 pour prendre en charge un nouveau `IsValid` surcharge qui fournit plus d’informations sur le contexte de validation actuel, telles que l’objet est en cours de validation. Cela permet des scénarios plus riches dans lequel vous pouvez valider la valeur actuelle selon une autre propriété du modèle. Par exemple, la nouvelle `CompareAttribute` attribut vous permet de comparer les valeurs des deux propriétés d’un modèle. Dans l’exemple suivant, le `ComparePassword` propriété doit correspondre à la `Password` champ afin d’être valide.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validation

Le [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interface vous permet d’effectuer la validation au niveau du modèle, et il vous permet de fournir la validation des messages d’erreur qui sont spécifiques à l’état de l’intégralité du modèle, ou entre deux propriétés dans le modèle . MVC 3 récupère maintenant les erreurs à partir de la `IValidatableObject` interface lors de la liaison de modèle et automatiquement les indicateurs ou les points importants de champs dans une vue à l’aide de helpers de formulaire HTML intégrés affectées.

Le [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interface permet à ASP.NET MVC à découvrir au moment de l’exécution si un validateur prise en charge de la validation côté client. Cette interface a été conçue afin qu’elle peut être intégrée avec un large éventail de frameworks de validation.

Pour plus d’informations sur les interfaces de validation, consultez le **améliorations de Validation de modèle** section de [billet de blog Guthrie MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Toutefois, notez que la référence à « IValidateObject » dans le blog doit être « IValidatableObject »).

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Améliorations de l’Injection de dépendance

ASP.NET MVC 3 fournit une meilleure prise en charge pour l’application d’Injection de dépendance (DI) et pour l’intégration avec des conteneurs d’Injection de dépendance ou d’Inversion de contrôle (IOC). Prise en charge pour l’injection de dépendances a été ajoutée dans les domaines suivants :

- Contrôleurs (l’inscription et injecter des fabriques de contrôleurs, injection de contrôleurs).
- Vues (l’inscription et injecter des moteurs d’affichage, l’injection de dépendances dans les pages de vue).
- Filtres d’action (localisation et d’injection de filtres).
- Classeurs de modèles (l’inscription et injecter).
- Fournisseurs de validation de modèle (l’inscription et injecter).
- Fournisseurs de métadonnées de modèle (l’inscription et injecter).
- Fournisseurs de valeurs (l’inscription et injecter).

MVC 3 prend en charge la [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) bibliothèque et n’importe quel conteneur d’injection de dépendance qui prend en charge de cette bibliothèque `IServiceLocator` interface. Il prend également en charge un nouveau `IDependencyResolver` interface qui rend plus facile à intégrer les infrastructures d’injection de dépendances.

Pour plus d’informations sur l’injection de dépendances dans MVC 3, consultez les ressources suivantes :

- [Série de Brad Wilson de billets de blog sur l’emplacement du Service](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notes de mise à jour de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Autres nouvelles fonctionnalités

### <a name="nuget-integration"></a>Intégration de NuGet

ASP.NET MVC 3 installe automatiquement et permet de NuGet dans le cadre de son installation. NuGet est un gestionnaire de package open source gratuit qui permet de facilement rechercher, installer et utiliser les outils et bibliothèques .NET dans vos projets. Il fonctionne avec tous les types de projet Visual Studio (y compris les Web Forms ASP.NET et ASP.NET MVC).

NuGet permet aux développeurs qui maintiennent les projets open source (par exemple, les projets tels que Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks et Elmah) pour leurs bibliothèques de package et les inscrire dans une galerie en ligne. Il est facile pour les développeurs .NET qui souhaitent utiliser une de ces bibliothèques pour rechercher le package et l’installer dans les projets que sur lesquels ils travaillent.

La mise à jour ASP.NET 3 outils, modèles de projets incluent JavaScript bibliothèques packages NuGet préinstallés, afin qu’ils soient actualisables via NuGet. Entity Framework Code First est également préinstallé comme package NuGet.

Pour plus d'informations sur NuGet, consultez la [documentation NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>La mise en cache de sortie de Page partielle

ASP.NET MVC a pris en charge la sortie mise en cache des réponses de la page entière depuis la version 1. MVC 3 prend également en charge la mise en cache de sortie de page partielle, qui vous permet de facilement les régions de cache ou des fragments d’une réponse. Pour plus d’informations sur la mise en cache, consultez le **mise en cache partielle Page sortie** section de [billet de blog Guthrie sur la version release candidate de MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) et **cache de sortie d’Action enfant** section de la [MVC 3 Release Notes](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Contrôle granulaire sur la Validation de la demande

ASP.NET MVC a la validation de la demande intégrée qui protège automatiquement contre les attaques par injection XSS et HTML. Cependant, parfois vous souhaitez explicitement désactiver validation de la demande, par exemple si vous souhaitez permettre aux utilisateurs de republier du HTML contenu (par exemple, dans les entrées de blog ou contenu CMS). Vous pouvez maintenant ajouter un [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) aux modèles d’attribut ou d’afficher des modèles pour désactiver la validation de demande sur une fonction de la propriété lors de la liaison de modèle. Pour plus d’informations sur la validation de la demande, consultez les ressources suivantes :

- Le **JavaScript non obstructive et Validation** section [billet de blog Guthrie sur la version release candidate de MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notes de mise à jour de MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensible « nouveau projet » boîte de dialogue

Dans ASP.NET MVC 3, vous pouvez ajouter des modèles de projet, les moteurs d’affichage, et les infrastructures de projet de test unitaire la **nouveau projet** boîte de dialogue.

### <a name="template-scaffolding-improvements"></a>Améliorations de la structure de modèle

Modèles de génération de modèles automatique ASP.NET MVC 3 préférables identifiant les propriétés de clé primaire sur les modèles et les gérer en conséquence que dans les versions antérieures de MVC. (Par exemple, les modèles de génération de modèles automatique Vérifiez maintenant que la clé primaire n’est pas structurée comme un champ de formulaire modifiable.)

Par défaut, les squelettes Create et Edit utilisent désormais le `Html.EditorFor` helper au lieu du `Html.TextBoxFor` helper. Cela améliore la prise en charge des métadonnées sur le modèle sous la forme de données des attributs d’annotation lorsque le **ajouter une vue** boîte de dialogue génère une vue.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nouvelles surcharges pour « Html.LabelFor » et « Html.LabelForModel »

Nouvelles surcharges de méthode ont été ajoutés pour la `LabelFor` et `LabelForModel` méthodes d’assistance. Les nouvelles surcharges permettent de spécifier ou de remplacer le texte d’étiquette.

### <a name="sessionless-controller-support"></a>Prise en charge du contrôleur sessionless

Dans ASP.NET MVC 3 vous pouvez indiquer si vous souhaitez une classe de contrôleur d’utiliser l’état de session et si tel est le cas, si l’état de session doit être en lecture/écriture ou en lecture seule. Pour plus d’informations sur la prise en charge du contrôleur sans session, consultez [MVC 3 Release Notes](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nouvelle classe « AdditionalMetadataAttribute »

Vous pouvez utiliser la [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) attribut pour remplir le `ModelMetadata.AdditionalValues` dictionnaire pour une propriété de modèle. Par exemple, si un modèle de vue a une propriété qui doit être affichée uniquement à un administrateur, vous pouvez annoter cette propriété comme indiqué dans l’exemple suivant :

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Ces métadonnées sont accessible à n’importe quel modèle d’affichage ou de l’éditeur lors du rendu d’un modèle de vue du produit. Il vous incombe d’interpréter les informations de métadonnées.

### <a name="accountcontroller-improvements"></a>Améliorations du contrôle AccountController

Contrôle AccountController dans le modèle de projet Internet a été considérablement améliorée.

### <a name="new-intranet-project-template"></a>Nouveau modèle de projet Intranet

Un nouveau modèle de projet Intranet est inclus qui active l’authentification Windows et supprime le AccountController.
