---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ce document décrit la version de la version bêta d’ASP.NET MVC 4 pour Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: ''
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 62bd641b1c5c926ec33ed3948854365aea99e979
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390606"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Ce document décrit la version de la version bêta d’ASP.NET MVC 4 pour Visual Studio 2010.
> 
> > [!NOTE]
> > Cela n’est pas la version actuelle. Les notes de publication d’ASP.NET MVC 4 RC sont disponibles [ici](mvc4-release-notes.md).


- [Notes d’installation](#_Toc303253802)
- [Documentation](#_Toc303253803)
- [Prise en charge](#_Toc303253804)
- [Configuration logicielle requise](#_Toc303253805)
- [La mise à niveau d’un projet ASP.NET MVC 3 vers ASP.NET MVC 4](#_Toc303253806)
- [Nouvelles fonctionnalités dans la version bêta d’ASP.NET MVC 4](#_Toc303253807)

    - [API Web ASP.NET](#_Toc317096197)
    - [Application de Page ASP.NET unique](#_Toc317096198)
    - [Améliorations apportées aux modèles de projet par défaut](#_Toc303253808)
    - [Modèle de projet mobile](#_Toc303253809)
    - [Modes d’affichage](#_Toc303253810)
    - [jQuery Mobile, le sélecteur de vue et la substitution de navigateur](#_Toc303253811)
    - [Recettes pour la génération de Code dans Visual Studio](#_Toc303253812)
    - [Prise en charge de la tâche pour les contrôleurs asynchrones](#_Toc303253813)
    - [Kit de développement logiciel Azure](#_Toc303253814)
    - [Problèmes connus et les modifications avec rupture](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notes d’installation

ASP.NET MVC 4 Beta pour Visual Studio 2010 peut être installé à partir de la [page d’accueil ASP.NET MVC 4](../mvc/mvc4.md) à l’aide de Web Platform Installer.

Vous devez désinstaller les aperçus précédemment installées d’ASP.NET MVC 4 avant d’installer la version bêta d’ASP.NET MVC 4.

Cette version n’est pas compatible avec l’aperçu pour développeurs .NET Framework 4.5. Vous devez désinstaller l’aperçu pour développeurs .NET 4.5 avant d’installer la version bêta d’ASP.NET MVC 4.

ASP.NET MVC 4 peut être installé et peut s’exécuter côte à côte avec ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentation

Documentation pour ASP.NET MVC est disponible sur le site Web MSDN à l’adresse suivante :

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Didacticiels et autres informations sur ASP.NET MVC sont disponibles sur la page MVC 4 du site Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Assistance

Ceci est une version préliminaire et n’est pas officiellement pris en charge. Si vous avez des questions sur l’utilisation de cette version, les publier sur le forum ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), où les membres de la Communauté ASP.NET peuvent souvent fournir un support informel.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Configuration logicielle

Les composants ASP.NET MVC 4 pour Visual Studio requièrent PowerShell 2.0 et Visual Studio 2010 avec Service Pack 1 ou Visual Web Developer Express 2010 avec Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>La mise à niveau d’un projet ASP.NET MVC 3 vers ASP.NET MVC 4

ASP.NET MVC 4 peut être installé côte à côte avec ASP.NET MVC 3 sur le même ordinateur, ce qui vous donne la souplesse dans le choix quand mettre à niveau une application ASP.NET MVC 3 vers ASP.NET MVC 4.

La façon la plus simple pour mettre à niveau est pour créer un nouveau projet ASP.NET MVC 4 et la copie toutes les vues, contrôleurs, code et du contenu fichiers à partir du projet MVC 3 existant vers le nouveau projet et pour mettre à jour de l’assembly de référence dans le nouveau projet pour faire correspondre l’ancien projet. Si vous avez apporté des modifications au fichier Web.config dans le projet MVC 3, vous devez également fusionner ces modifications dans le fichier Web.config dans le projet MVC 4.

Pour mettre manuellement à une application ASP.NET MVC 3 existante vers la version 4, procédez comme suit :

1. Dans tous les fichiers Web.config dans le projet (il existe à la racine du projet, l’autre dans le dossier Views et l’autre dans le dossier Views de chaque zone dans votre projet), remplacez chaque instance de ce qui suit :

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    avec le texte correspondant suivant :

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Dans le fichier Web.config racine, mettez à jour le *webPages:Version* élément « 2.0.0.0 », puis ajoutez une nouvelle *PreserveLoginUrl* clé qui a la valeur « true » :

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Dans l’Explorateur de solutions, supprimez la référence à *System.Web.Mvc* (qui pointe vers la version DLL 3). Puis ajoutez une référence à *System.Web.Mvc* (v4.0.0.0). En particulier, apporter les modifications suivantes pour mettre à jour les références d’assembly. Voici les détails :

    1. Dans l’Explorateur de solutions, supprimez les références aux assemblys suivants : 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Ajoutez une référence aux assemblys suivants : 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. Dans l’Explorateur de solutions, cliquez sur le nom de projet, puis sélectionnez Décharger le projet. Cliquez à nouveau sur le nom, puis sélectionnez Modifier *nom_projet*.csproj.
5. Recherchez le *ProjectTypeGuids* élément et remplacez {E53F8FEA-EAE0-44A6-8774-FFD645390401} par {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Enregistrer les modifications, fermez le fichier de projet (.csproj) que vous avez modifiée, cliquez sur le projet, puis sélectionnez recharger le projet.
7. Si le projet fait référence à toutes les bibliothèques tierces qui sont compilés à l’aide de versions précédentes d’ASP.NET MVC, ouvrez le fichier Web.config racine et ajoutez les trois suivantes *bindingRedirect* éléments sous la  *configuration* section : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nouvelles fonctionnalités dans la version bêta d’ASP.NET MVC 4

Cette section décrit les fonctionnalités qui ont été introduites dans la version de la version bêta d’ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API web ASP.NET

ASP.NET MVC 4 inclut désormais les API Web ASP.NET, une nouvelle infrastructure pour la création de services HTTP qui peut atteindre un large éventail de clients, y compris les navigateurs et appareils mobiles. API Web ASP.NET est également une plate-forme idéale pour la création de services RESTful.

API Web ASP.NET prend en charge les fonctionnalités suivantes :

- **Modèle de programmation HTTP moderne :** directement accéder et manipuler des requêtes HTTP et les réponses dans vos API Web à l’aide d’un modèle d’objet HTTP nouvelle, fortement typé. Le même modèle de programmation pipeline HTTP est symétriquement disponible sur le client via le nouveau type de HttpClient.
- **Prise en charge pour les itinéraires complète**: API Web prennent désormais en charge l’ensemble complet des fonctionnalités de routage ont toujours été une partie de la pile de Web, y compris les paramètres d’itinéraire et des contraintes. En outre, le mappage à des actions a prise en charge complète pour les conventions, donc vous n’avez plus besoin d’appliquer des attributs tels que [HttpPost] à vos classes et les méthodes.
- **Négociation de contenu**: le client et le serveur peuvent collaborer pour déterminer le format correct pour les données renvoyées à partir d’une API. Nous offrons un support par défaut pour XML, JSON et formats codée URL de formulaire, et vous pouvez étendre cette prise en charge en ajoutant vos propres formateurs, ou même remplacer la stratégie de négociation de contenu par défaut.
- **Liaison de modèle et la validation :** binders de modèles fournissent un moyen simple pour extraire des données de différentes parties d’une requête HTTP et de convertir ces parties de message en objets .NET qui peuvent être utilisées par les actions de l’API Web.
- **Filtres :** API Web prend désormais en charge les filtres, notamment les filtres bien connus tels que l’attribut [Authorize]. Vous pouvez créer et incorporer dans vos propres filtres pour les actions, d’autorisation et la gestion des exceptions.
- **Composition de la requête :** par un retour IQueryable&lt;T&gt;, votre API Web prendra en charge l’interrogation via les conventions d’URL OData.
- **Amélioration de testabilité de détails HTTP :** au lieu de définir des détails HTTP dans les objets de contexte statique, les actions de l’API Web peuvent désormais travailler avec des instances de HttpRequestMessage et HttpResponseMessage. Il existe également des versions génériques de ces objets pour vous permettre d’utiliser vos types personnalisés en plus des types HTTP.
- **Amélioration d’Inversion de contrôle (IoC) via DependencyResolver :** API Web utilise désormais le modèle de localisateur de service implémenté par le résolveur de dépendance de MVC pour obtenir des instances de nombreuses fonctionnalités différentes.
- **Configuration basée sur le code :** configuration de l’API Web s’effectue uniquement par le biais de code, de nettoyer les fichiers en laissant votre fichier config.
- **Self-host :** API Web peuvent être hébergés dans votre propre processus outre par IIS, tout en utilisant toute la puissance d’itinéraires et d’autres fonctionnalités de l’API Web.

Pour plus d’informations sur l’API Web ASP.NET, consultez [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Application de Page ASP.NET unique

ASP.NET MVC 4 inclut désormais une version préliminaire de l’expérience pour la création d’applications à page unique avec des interactions côté client significatives à l’aide de JavaScript et des API Web. Cette prise en charge inclut :

- Un ensemble de bibliothèques JavaScript pour des interactions plus riches locales avec les données mises en cache
- Composants Web API supplémentaires pour l’unité de travail et la prise en charge de la couche DAL
- Un modèle de projet MVC avec la génération de modèles automatique pour démarrer rapidement

Pour plus d’informations sur l’Application à Page unique prise en charge dans ASP.NET MVC 4, consultez [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Améliorations apportées aux modèles de projet par défaut

Le modèle qui est utilisé pour créer des projets ASP.NET MVC 4 a été mis à jour pour créer un site Web plus modernes :

![](mvc4-beta-release-notes/_static/image1.png)

Outre les améliorations de cosmétiques, il présente des fonctions améliorées dans le nouveau modèle. Le modèle utilise une technique appelée rendu adaptatif pour s’afficher correctement dans les navigateurs de bureau et les navigateurs mobiles sans aucune personnalisation.

![](mvc4-beta-release-notes/_static/image2.png)

Pour afficher le rendu adaptatif en action, vous pouvez utiliser un émulateur mobile ou simplement essayez de redimensionner la fenêtre de navigateur de bureau pour être plus petit. Lorsque la fenêtre du navigateur est suffisamment petite, la disposition de la page change.

Une autre amélioration apportée au modèle de projet par défaut est l’utilisation de JavaScript pour fournir une interface utilisateur plus riche. Les liens de connexion et inscription sont utilisés dans le modèle sont des exemples montrant comment utiliser la boîte de dialogue de l’interface utilisateur jQuery pour présenter un écran de connexion riches :

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modèle de projet mobile

Si vous démarrez un nouveau projet et que vous souhaitez créer un site en particulier pour les mobiles et navigateurs de Tablet PC, vous pouvez utiliser le nouveau modèle de projet d’Application Mobile. Cela est basée sur jQuery Mobile, une bibliothèque open source pour la création de l’interface utilisateur de tactile optimisée :

![](mvc4-beta-release-notes/_static/image4.png)

Ce modèle contient la même application que le modèle d’Application Internet (et le code du contrôleur est pratiquement identique), mais c’est un style à l’aide de jQuery Mobile sont de bonne qualité et se comportent correctement sur les appareils mobiles basés sur les fonctions tactiles. Pour en savoir plus sur la structure et le style de l’interface utilisateur mobile, consultez le [site Web de projet Mobile jQuery](http://jquerymobile.com/).

Si vous avez déjà un site orientée bureau que vous souhaitez ajouter des affichages optimisés pour mobile à, ou si vous souhaitez créer un site unique qui fait Office de vues différemment de style pour les navigateurs de bureau et mobiles, vous pouvez utiliser la nouvelle fonctionnalité de Modes d’affichage. (Voir la section suivante.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modes d’affichage

La nouvelle fonctionnalité de Modes d’affichage permet à une application de sélectionner les vues selon le navigateur qui effectue la demande. Par exemple, si un navigateur de bureau demande la page d’accueil, l’application peut utiliser le modèle Views\Home\Index.cshtml. Si un navigateur mobile demande la page d’accueil, l’application peut retourner le modèle Views\Home\Index.mobile.cshtml.

Dispositions et des vues partielles peuvent également être remplacés pour les types de navigateur particulier. Exemple :

- Si votre dossier Views\Shared contient à la fois le \_Layout.cshtml et \_modèles Layout.mobile.cshtml, par défaut, l’application utilisera \_Layout.mobile.cshtml pendant les requêtes provenant des navigateurs mobiles et de \_Layout.cshtml lors des autres demandes.
- Si un dossier contient à la fois \_MyPartial.cshtml et \_MyPartial.mobile.cshtml, l’instruction @Html.Partial(«\_MyPartial ») sera rendu \_MyPartial.mobile.cshtml lors des demandes à partir de mobile navigateurs, et \_MyPartial.cshtml lors des autres demandes.

Si vous souhaitez créer des vues plus spécifiques, de présentations ou des vues partielles pour d’autres appareils, vous pouvez inscrire un nouveau *DefaultDisplayMode* instance pour spécifier le nom à rechercher lorsqu’une demande satisfait aux conditions particulières. Par exemple, vous pouvez ajouter le code suivant à la *Application\_Démarrer* méthode dans le fichier Global.asax pour enregistrer la chaîne « iPhone » comme mode d’affichage qui s’applique lorsque le navigateur Apple iPhone qui effectue une requête :

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Une fois ce code s’exécute lorsqu’un navigateur Apple iPhone qui effectue une demande, votre application utilisera le Views\Shared\\_Layout.iPhone.cshtml disposition (si elle existe).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, le sélecteur de vue et la substitution de navigateur

jQuery Mobile est une bibliothèque open source pour la création de l’interface utilisateur web tactile optimisée. Si vous souhaitez utiliser jQuery Mobile avec une application ASP.NET MVC 4, vous pouvez télécharger et installer un package NuGet qui vous permet de commencer. Pour l’installer à partir de la Console du Gestionnaire de Package Visual Studio, tapez la commande suivante :

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Cette opération installe jQuery Mobile et des fichiers d’assistance, notamment les suivantes :

- Views/Shared/\_Layout.Mobile.cshtml, qui est une disposition jQuery Mobile.
- Un composant de sélecteur de vue, qui se compose de laViews/Shared/\_vue partielle ViewSwitcher.cshtml et le contrôleur ViewSwitcherController.cs.

Après avoir installé le package, exécutez votre application à l’aide d’un navigateur mobile (ou équivalent, telles que le Firefox [sélecteur d’Agent utilisateur](http://chrispederick.com/work/user-agent-switcher/) module complémentaire). Vous verrez que vos pages sembler assez différents, étant donné que jQuery Mobile gère la disposition et le style. Pour tirer parti de cette possibilité, vous pouvez procédez comme suit :

- Créer des remplacements de l’affichage mobile comme décrit sous [Modes d’affichage](#_Toc303253810) précédemment (par exemple, créez Views\Home\Index.mobile.cshtml pour remplacer Views\Home\Index.cshtml pour les navigateurs mobiles).
- Lire le [jQuery Mobile documentation](http://jquerymobile.com/) pour en savoir plus sur l’ajout d’éléments d’interface utilisateur tactile optimisée sur les périphériques mobiles.

Une convention pour les pages web optimisés pour mobile consiste à ajouter un lien dont le texte est quelque chose comme vue bureau ou le mode de site complète qui permet aux utilisateurs de basculer vers une version de bureau de la page. Le package jQuery.Mobile.MVC comprend un exemple de composant de sélecteur de vue à cet effet. Il est utilisé dans la valeur par défaut Views\Shared\\_Layout.Mobile.cshtml vue, et il ressemble à ceci lorsque la page est affichée :

![](mvc4-beta-release-notes/_static/image5.png)

Si les visiteurs sur le lien, ils basculez vers la version bureau de la même page.

Étant donné que la disposition de votre bureau n’inclut pas d’un sélecteur de vue par défaut, les visiteurs n’auront pas un moyen d’accéder à la mode mobile. Pour ce faire, ajoutez la référence suivante à  *\_ViewSwitcher* à disposition de votre bureau, juste à l’intérieur de la *&lt;corps&gt;* élément :

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Le sélecteur de vue utilise une nouvelle fonctionnalité appelée la substitution de navigateur. Cette fonctionnalité permet à votre application de traiter les demandes comme si elles provenaient d’un navigateur (agent utilisateur) que celui ils sont en fait à partir de. Le tableau suivant répertorie les méthodes fournies par la substitution de navigateur.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Remplace la valeur de l’agent utilisateur réelle de la demande à l’aide de l’agent utilisateur spécifié. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Retourne la valeur de remplacement de l’agent utilisateur de la demande, ou la chaîne d’agent utilisateur réelle si aucune substitution n’a été spécifiée. |
| `HttpContext.GetOverriddenBrowser()` | Retourne un *HttpBrowserCapabilitiesBase* instance qui correspond à l’agent utilisateur actuellement défini pour la demande (réelle ou substituée). Vous pouvez utiliser cette valeur pour obtenir des propriétés telles que *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Supprime tout agent utilisateur substitué pour la requête actuelle. |

Substitution de navigateur est une fonctionnalité principale d’ASP.NET MVC 4 et est disponible même si vous n’installez pas le package jQuery.Mobile.MVC. Toutefois, il affecte uniquement affichage, la disposition et sélection de la vue partielle, il n’affecte pas une autre fonctionnalité d’ASP.NET qui dépend de la *Request.Browser* objet.

Par défaut, le remplacement de l’agent utilisateur est stocké à l’aide d’un cookie. Si vous souhaitez stocker le remplacement ailleurs (par exemple, dans une base de données), vous pouvez remplacer le fournisseur par défaut (*BrowserOverrideStores.Current*). Documentation pour ce fournisseur sera disponible pour accompagner d’une version ultérieure d’ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Recettes pour la génération de Code dans Visual Studio

La nouvelle fonctionnalité de recettes permet à Visual Studio générer le code spécifique à la solution basée sur les packages que vous pouvez installer à l’aide de NuGet. Le framework de recettes facilite aux développeurs d’écrire des plug-ins de génération de code, qui vous permet également de remplacer les générateurs de code intégré pour ajouter une zone, ajouter un contrôleur et ajouter une vue. Étant donné que les recettes sont déployées sous forme de packages NuGet, ils peuvent facilement être archivés dans le contrôle de code source et partagées avec tous les développeurs sur le projet automatiquement. Ils sont également disponibles sur un cas par cas.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Prise en charge de la tâche pour les contrôleurs asynchrones

Vous pouvez maintenant écrire des méthodes d’action asynchrones des méthodes comme unique qui retournent un objet de type *tâche* ou *tâche&lt;ActionResult&gt;*.

Par exemple, si vous utilisez Visual c# 5 (ou à l’aide de la [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), vous pouvez créer une méthode d’action asynchrone qui ressemble à ceci :

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

Dans la méthode d’action précédente, les appels à *newsService.GetHeadlinesAsync* et *sportsService.GetScoresAsync* sont appelées de façon asynchrone et ne bloquent pas un thread du pool de threads.

Méthodes d’action asynchrones qui retournent *tâche* instances peuvent prendre également en charge les délais d’attente. Pour rendre votre méthode d’action annulable, ajoutez un paramètre de type *CancellationToken* à la signature de méthode d’action. L’exemple suivant montre une méthode d’action asynchrone qui a un délai d’expiration de 2500 millisecondes et qui affiche un *TimedOut* afficher au client si un délai d’expiration se produit.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Kit de développement logiciel Azure

Version bêta d’ASP.NET MVC 4 prend en charge la version 1.5 de septembre 2011 de Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et les modifications avec rupture

- **Après avoir installé la version bêta d’ASP.NET MVC 4, l’éditeur CSHTML/VBHTML dans l’éditeur de Visual Studio 2010 Service Pack 1 CSHTML/VBHTML peut mettre en pause pendant un certain temps après avoir tapé un extrait ou JavaScript fichiers cshtml et vbhtml.** Cela se produit uniquement dans les applications ASP.NET MVC 4 qui vient d’être créées et n’ont pas encore été compilées.

    La solution de contournement consiste à compiler le projet pour obtenir les assemblys dans le dossier bin. Notez que si vous nettoyez le projet, ce qui supprime les assemblys dans le dossier bin, le problème de l’éditeur reviendra.

    Cela sera corrigé dans la prochaine version.
- **Modèles de projet c# pour Visual Studio 11 Beta contiennent une chaîne de connexion incorrecte dans Global.asax.cs.** La connexion par défaut spécifiée dans l’Application\_Start (méthode) pour les projets créés dans Visual Studio 11 Beta contiennent une chaîne de connexion de base de données locale qui contient une barre oblique inverse sans séquence d’échappement (\) caractère. Cela entraîne une erreur de connexion lors de tentatives d’accès d’un DbContext d’Entity Framework, ce qui génère une erreur SqlException.

    Pour corriger ce problème, la séquence d’échappement le caractère barre oblique inverse dans l’application\_Start, méthode de Global.asax.cs afin qu’elle s’affiche comme suit :

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Les applications ASP.NET MVC 4 qui ciblent le .NET 4.5 lèvera une FileLoadException lors de la tentative d’accès à l’assembly System.Net.Http.dll lorsque vous utilisez .NET 4.0.** Les applications ASP.NET MVC 4 créées sous .NET 4.5 contient une liaison de redirection qui entraîne une FileLoadException qui indique « Impossible de charger fichier ou l’assembly 'System.Net.Http' ou une de ses dépendances. » Lorsque l’application est exécutée sur un système avec .NET 4.0 est installé. Pour corriger ce problème, supprimez la redirection de liaison suivantes à partir de web.config :

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    L’élément de liaison d’assembly dans le fichier web.config modifié doit apparaître comme suit :

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Le modèle d’élément « Ajouter un contrôleur » dans les projets Visual Basic génère un espace de noms incorrect lorsqu’elle est appelée</strong><strong>à partir d’à l’intérieur d’une zone.</strong> Lorsque vous ajoutez un contrôleur à une zone dans un projet ASP.NET MVC qui utilise Visual Basic, le modèle d’élément insère l’espace de noms incorrect dans le contrôleur. Le résultat est une erreur « fichier introuvable » lorsque vous accédez à une action dans le contrôleur.  
  
  L’espace de noms généré omet tous les éléments après l’espace de noms racine. Par exemple, l’espace de noms généré est *RootNamespace* mais doit être *RootNamespace.Areas.AreaName.Controllers* .
- **Modifications avec rupture dans le moteur d’affichage Razor.** Dans le cadre d’une réécriture de l’analyseur Razor, les types suivants ont été supprimés à partir de *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Les méthodes suivantes ont été également supprimées : 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Lorsque WebMatrix.WebData.dll est inclus dans le répertoire "/ bin" de des applications ASP.NET MVC 4, il adopte l’URL pour l’authentification par formulaire.** Ajout de l’assembly WebMatrix.WebData.dll à votre application (par exemple, en sélectionnant « ASP.NET Web Pages avec syntaxe Razor » lors de l’utilisation de la boîte de dialogue Ajouter des dépendances déployables) remplacera la redirection de connexion d’authentification vers/compte d’ouverture de session au lieu de / compte de connexion comme prévu par le contrôleur de compte ASP.NET MVC par défaut. Pour éviter ce comportement et utiliser l’URL spécifiée déjà dans la section authentification de web.config, vous pouvez ajouter un appSetting appelé PreserveLoginUrl et affectez-lui la valeur true : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Le Gestionnaire de package NuGet ne parvient pas à installer lorsque vous tentez d’installer ASP.NET MVC 4 pour les installations côte à côte de Visual Studio 2010 et Visual Web Developer 2010.** Pour exécuter Visual Studio 2010 et Visual Web Developer 2010 côte à côte avec ASP.NET MVC 4, vous devez installer ASP.NET MVC 4 une fois que les deux versions de Visual Studio ont déjà été installées.
- **Désinstallation d’ASP.NET MVC 4 échoue si les conditions préalables ont déjà été désinstallés.** Pour désinstaller correctement ASP.NET MVC 4Vous devez désinstaller ASP.NET MVC 4 avant de désinstaller Visual Studio.
- **Exécution d’un projet d’API Web par défaut affiche les instructions qui incorrectement diriger l’utilisateur d’ajouter des itinéraires à l’aide de la méthode RegisterApis, qui n’existe pas.** Itinéraires doivent être ajoutés à la méthode RegisterRoutes à l’aide de la table de routage ASP.NET.
- **Installer la version bêta d’ASP.NET MVC 4, les applications ASP.NET MVC 3 RTM s’arrête.** Les applications ASP.NET MVC 3 qui ont été créées avec la version RTM (pas dont la version ASP.NET MVC 3 Tools Update) nécessitent les modifications suivantes afin de fonctionner côte à côte avec la version bêta d’ASP.NET MVC 4. Génération du projet sans apporter de ces résultats des mises à jour dans les erreurs de compilation. 

    **Mises à jour requises**

  1. Dans le fichier racine Web.config, ajoutez un nouveau *&lt;appSettings&gt;* entrée avec la clé *webPages:Version* et la valeur *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. Dans l’Explorateur de solutions, cliquez sur le nom de projet, puis sélectionnez Décharger le projet. Cliquez à nouveau sur le nom, puis sélectionnez Modifier *nom_projet*.csproj.
  3. Recherchez les références d’assembly suivantes : 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Remplacez-les par les éléments suivants :

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Enregistrer les modifications, fermez le fichier de projet (.csproj), vous avez modifiée, puis cliquez sur le projet et sélectionnez recharger.
