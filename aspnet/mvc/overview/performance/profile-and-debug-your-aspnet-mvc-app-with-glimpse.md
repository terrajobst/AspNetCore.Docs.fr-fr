---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profiler et déboguer votre application ASP.NET MVC avec Glimpse | Microsoft Docs
author: Rick-Anderson
description: Aperçu est un plein essor et en constante évolution de la famille de packages NuGet d’open source qui fournit les données de performances détaillées, débogage et informations de diagnostic pour ASP.NET un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: ef65b512645d3f013ea34d3557da93254425c419
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391068"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profiler et déboguer votre application ASP.NET MVC avec Glimpse
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Aperçu est plein essor et en constante évolution de la famille de packages NuGet d’open source qui fournit les données de performances détaillées, débogage et des informations de diagnostic pour les applications ASP.NET. Il est très facile à installer, léger et ultra rapide et affiche les mesures de performances clés en bas de chaque page. Il vous permet d’approfondir votre application lorsque vous avez besoin savoir ce qui se passait au niveau du serveur. Aperçu fournit des informations précieuses tellement que nous vous recommandons de qu'utiliser cela dans votre cycle de développement, y compris votre environnement de test Azure. Tandis que [Fiddler](http://www.telerik.com/fiddler) et [les outils de développement F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) fournissent un côté client vue Aperçu fournit une vue détaillée à partir du serveur. Ce didacticiel aborde l’utilisation de l’aperçu ASP.NET MVC et les packages d’EF, mais de nombreux autres packages sont disponibles. Lorsque cela est possible de lier sera à approprié [apercevoir docs](http://getglimpse.com/Docs/) dont j’ai aider à maintenir. Aperçu est un projet open source, vous pouvez trop contribuer au code source et la documentation.


- [L’installation d’aperçu](#ig)
- [Activer l’aperçu pour localhost](#eg)
- [L’onglet de la chronologie](#Time)
- [Liaison de données](#mb)
- [Itinéraires](#route)
- [À l’aide de Glimpse sur Azure](#da)
- [Ressources supplémentaires pour MSBuild](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>L’installation d’aperçu

Vous pouvez installer aperçu à partir de la console de gestionnaire de package NuGet ou de la **gérer les Packages NuGet** console. Pour cette démonstration, j’installe les packages Mvc5 et EF6 :

![installer l’aperçu à partir de NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Recherchez *Glimpse.EF*

![Glimpse.EF à partir de NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

En sélectionnant **packages installés**, vous pouvez voir les modules dépendants aperçu installés :

![Packages de Glimpse installés à partir de DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Les commandes suivantes installent les modules de Glimpse MVC5 et EF6 à partir de la console du Gestionnaire de package :

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Activer l’aperçu pour localhost

Accédez à http://localhost:&lt; port #&gt;/glimpse.axd et cliquez sur le <strong>activer un aperçu sur</strong> bouton.

![Page d’aperçu axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Si vous avez votre barre des favoris affiché, vous pouvez faire glisser et déposer les boutons d’aperçu et ajoutez-les en tant que bookmarklets :

![Internet Explorer avec Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Vous pouvez désormais parcourir votre application et le **têtes d’affichage** (HUD) s’affiche en bas de la page.

![Page Gestionnaire de contacts avec HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

Le [page d’aperçu HUD](http://getglimpse.com/Docs/Heads-up-Display) décrit en détail les informations de minutage indiquées ci-dessus. Les affichages de données le HUD performances discrète peuvent vous avertir d’un problème immédiatement - avant de commencer le cycle de test. En cliquant sur le &quot;g&quot; dans le coin inférieur droit permet d’afficher le volet d’aperçu :

![Volet d’aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Dans l’image ci-dessus, le [onglet exécution](http://getglimpse.com/Docs/Execution-Tab) est sélectionnée, qui affiche les détails de minutage des actions et des filtres dans le pipeline. Vous pouvez voir mon [minuteur de filtre d’arrêter un espion](http://www.nuget.org/packages/StopWatch/) commencent à l’étape 6 du pipeline. Mon minuteur léger peut contribuer utile profil/synchronisation de données, il n’arrive pas tout le temps passé en matière d’autorisation et de rendu de l’affichage. Vous pouvez lire sur mon minuterie à [Profiler et l’heure de votre application ASP.NET MVC jusqu'à Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Le [onglets](http://getglimpse.com/Docs/Tabs) page fournit des liens vers des informations détaillées sur chaque onglet.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>L’onglet de la chronologie

J’ai modifié de Tom Dykstra en suspens [didacticiel sur EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) par le code suivant, remplacez par le contrôleur instructors :

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Le code ci-dessus me permet de passer dans la chaîne de requête (`eager`) au contrôle hâtif ou explicite le chargement de données. Dans l’image ci-dessous, le chargement explicite est utilisé et la page de minutage affiche chaque inscription chargée dans le `Index` méthode d’action :

![chargement explicite](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Dans le code suivant, hâtif est spécifié, et chaque inscription est extrait après le `Index` vue est appelée :

![Eager est spécifié.](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Vous pouvez pointer sur un segment de temps pour obtenir des informations de minutage détaillées :

![pointage pour voir de temporisation détaillées](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Liaison de modèle

Le [onglet de liaison de modèle](http://getglimpse.com/Docs/Model-Binding-Tab) fournit une mine d’informations pour vous aider à comprendre comment vos variables de formulaire sont liés et pourquoi certaines ne sont pas liés comme peut l’attendre. L’image ci-dessous montre le **?** icône, vous pouvez cliquer sur pour afficher la page d’aide Aperçu pour cette fonctionnalité.

![visualiser la vue de liaison de modèle](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Routes

 L’onglet Aperçu itinéraires sera peuvent vous aider à déboguer et comprendre le routage. Dans l’image ci-dessous, l’itinéraire de produit est sélectionné (et elle est affichée en vert, une convention d’aperçu). ![nom du produit sélectionné](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) jetons contraintes, les zones et les données d’itinéraire sont également affichés. Consultez [Glimpse itinéraires](http://getglimpse.com/Docs/Routes-Tab) et [routage par attributs dans ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) pour plus d’informations. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>À l’aide de Glimpse sur Azure

La stratégie de sécurité d’aperçu par défaut autorise uniquement les données d’aperçu à afficher à partir de l’hôte local. Vous pouvez modifier cette stratégie de sécurité pour pouvoir afficher ces données sur un serveur distant (par exemple, une application web sur Azure). Pour les environnements de test sur Azure, ajouter la marque en surbrillance jusqu'à la partie inférieure de la *web.confg* fichier pour activer l’aperçu :

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Avec cette modification uniquement, n’importe quel utilisateur peut voir vos données d’aperçu sur un site distant. Envisagez d’ajouter le balisage ci-dessus à un profil de publication afin qu’il a déployé uniquement un appliqués lorsque vous utilisez ce profil de publication (par exemple, votre proifle test Azure.) Pour restreindre les données d’aperçu, nous allons ajouter le `canViewGlimpseData` rôle et d’autoriser uniquement les utilisateurs à ce rôle pour afficher les données d’aperçu.

Supprimez les commentaires de la *GlimpseSecurityPolicy.cs* du fichier et changer la [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) appeler à partir de `Administrator` à la `canViewGlimpseData` rôle :

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Sécurité - données riches fournies par l’aperçu peut exposer la sécurité de votre application. Microsoft n’a pas effectué un audit de sécurité de l’aperçu pour une utilisation sur les applications de production.


Pour plus d’informations sur l’ajout de rôles, consultez mon [déployer une application web de Secure ASP.NET MVC 5 avec appartenance, OAuth et base de données SQL dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) didacticiel.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et base de données SQL dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Configuration de glimpse](http://getglimpse.com/Docs/Configuration) -page de documentation sur la configuration des onglets, la stratégie runtime, journalisation et bien plus encore.
