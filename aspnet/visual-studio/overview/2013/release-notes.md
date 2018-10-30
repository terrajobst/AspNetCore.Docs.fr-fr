---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET et Web Tools pour Visual Studio 2013 Release Notes de publication | Microsoft Docs
author: microsoft
description: Ce document décrit la version de ASP.NET et Web Tools pour Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 43878bc101ef97e8bbb6c150f4125707da7660c9
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244955"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET et Web Tools pour Visual Studio 2013 Release Notes de publication
====================
par [Microsoft](https://github.com/microsoft)

> Ce document décrit la version de ASP.NET et Web Tools pour Visual Studio 2013.


## <a name="contents"></a>Sommaire

- [Notes d’installation](#TOC1)
- [Documentation](#TOC2)
- [Configuration logicielle requise](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nouvelles fonctionnalités dans ASP.NET et Web Tools pour Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Nouvelle expérience de projet Web](#newproj)
- [Génération de modèles automatique ASP.NET](#scaffold)
- [Lien du navigateur](#browser-link)
- [Améliorations de l’éditeur Web de Visual Studio](#web-editor)
- [Prise en charge des applications Azure App Service Web dans Visual Studio](#waws)
- [Améliorations de la publication sur le Web](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web Forms](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Composants Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [Razor ASP.NET 3](#TOC14)
- [ASP.NET App Suspend](#TOC15)
- [Problèmes connus et les modifications avec rupture](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notes d’installation

ASP.NET et Web Tools pour Visual Studio 2013 sont regroupés dans le programme d’installation principal et peut être téléchargé [ici](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentation

Didacticiels et autres informations sur ASP.NET et Web Tools pour Visual Studio 2013 sont disponibles à partir de la [site web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Configuration logicielle

ASP.NET et Web Tools requiert Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nouvelles fonctionnalités dans ASP.NET et Web Tools pour Visual Studio 2013

Les sections suivantes décrivent les fonctionnalités qui ont été introduites dans la version.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

Avec la version de Visual Studio 2013, nous avons adopté une étape vise à unifier l’expérience de l’utilisation de technologies d’ASP.NET, afin que vous pouvez facilement combiner et correspondent à ceux que vous souhaitez. Par exemple, vous pouvez démarrer un projet à l’aide de MVC et facilement ajouter ultérieurement des pages Web Forms au projet ou structurer des API Web dans un projet Web Forms. One ASP.NET concerne les rend plus facile pour vous en tant que développeur d’effectuer les opérations que vous aimez dans ASP.NET. Quelle que soit la technologie que vous choisissez, vous pouvez avoir confiance que vous générez sur l’infrastructure sous-jacente approuvé de One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nouvelle expérience de projet Web

Nous avons amélioré l’expérience de création de projets web dans Visual Studio 2013. Dans le **nouveau projet Web ASP.NET** dialogue, vous pouvez sélectionner le type de projet, vous souhaitez configurez n’importe quelle combinaison de technologies (Web Forms, MVC, Web API), configurez les options d’authentification et ajoutez un projet de test unitaire.

![Nouveau projet ASP.NET](release-notes/_static/image1.png)

La nouvelle boîte de dialogue vous permet de modifier les options d’authentification par défaut pour la plupart des modèles. Par exemple, lorsque vous créez un projet Web Forms ASP.NET vous pouvez sélectionner une des options suivantes :

- Aucune authentification
- Comptes d’utilisateur individuels (l’appartenance ASP.NET ou des journaux de fournisseur de réseau social dans)
- Comptes d’organisation (Active Directory dans une application internet)
- Authentification Windows (Active Directory dans une application intranet)

![Options d’authentification](release-notes/_static/image2.png)

Pour plus d’informations sur le nouveau processus pour la création de projets web, consultez [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Pour plus d’informations sur les options d’authentification, consultez [ASP.NET Identity](#TOC8) plus loin dans ce document.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

Génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET. Il est facile d’ajouter du code réutilisable à votre projet qui interagit avec un modèle de données.

Dans les versions précédentes de Visual Studio, génération de modèles automatique a été limitée pour les projets ASP.NET MVC. Avec Visual Studio 2013, vous pouvez maintenant utiliser la génération de modèles automatique pour un projet ASP.NET, y compris les Web Forms. Visual Studio 2013 ne prend pas en charge les pages de génération pour un projet Web Forms, mais vous pouvez toujours utiliser la structure avec les Web Forms en ajoutant des dépendances MVC au projet. Prise en charge pour la génération de pages pour Web Forms sera ajoutée dans une prochaine mise à jour.

Lorsque vous utilisez la génération de modèles automatique, nous nous assurer que tous les requis dépendances sont installées dans le projet. Par exemple, si vous commencez avec un projet Web Forms ASP.NET et ensuite utilisez la structure pour ajouter un contrôleur d’API Web, références et les packages NuGet requis sont ajoutés à votre projet automatiquement.

Pour ajouter la structure MVC à un projet Web Forms, ajoutez un **nouvel élément structuré** et sélectionnez **des dépendances MVC 5** dans la boîte de dialogue. Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète. Si vous sélectionnez Minimal, uniquement les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option complet, les dépendances minimales sont ajoutés, ainsi que les fichiers de contenu requis pour un projet MVC.

Prise en charge de la génération de modèles automatique des contrôleurs d’async utilise les nouvelles fonctionnalités asynchrones à partir d’Entity Framework 6.

Pour plus d’informations et des didacticiels, consultez [vue d’ensemble de la génération de modèles automatique ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Lien du navigateur : canal de SignalR entre le navigateur et de Visual Studio

La nouvelle [Browser Link](using-browser-link.md) fonctionnalité vous permet de connecter plusieurs navigateurs à Visual Studio et de les actualiser tout en cliquant sur un bouton dans la barre d’outils. Vous pouvez connecter plusieurs navigateurs sur votre site de développement, y compris des émulateurs mobiles et cliquez sur Actualiser pour actualiser tous les navigateurs tous en même temps. Lien du navigateur expose également une API pour permettre aux développeurs d’écrire des extensions de lien du navigateur.

![](release-notes/_static/image3.png)

En permettant aux développeurs de tirer parti de l’API de lien de navigateur, il devient possible de créer des scénarios très avancés qui traverse les limites entre Visual Studio et n’importe quel navigateur qui est connecté. Web Essentials tire parti de l’API pour créer une expérience intégrée entre Visual Studio et les outils de développement du navigateur, à distance le contrôle des émulateurs mobiles et bien plus encore.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Améliorations de l’éditeur Web de Visual Studio

Visual Studio 2013 inclut un nouvel éditeur HTML pour les fichiers Razor et les fichiers HTML dans les applications web. Le nouvel éditeur HTML fournit un schéma unifié unique basé sur HTML5. Fin d’accolade automatique, de jQuery UI et AngularJS attribut IntelliSense, l’attribut de regroupement d’IntelliSense, ID et nom de classe Intellisense et autres améliorations, y compris les meilleures performances, la mise en forme et les balises actives.

La capture d’écran suivante illustre l’utilisation d’amorçage attribut IntelliSense dans l’éditeur HTML.

![IntelliSense dans l’éditeur HTML](release-notes/_static/image4.png)

Visual Studio 2013 est également fourni avec les deux CoffeeScript et moins éditeurs intégrées. L’éditeur LESS est fourni avec toutes les fonctionnalités intéressantes à partir de l’éditeur CSS et est doté d’Intellisense spécifique pour les variables et les mixins dans tous les moins documents dans le @import chaîne.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Prise en charge des applications Azure App Service Web dans Visual Studio

Dans Visual Studio 2013 avec le SDK Azure pour .NET 2.2, vous pouvez utiliser **Explorateur de serveurs** d’interagir directement avec vos applications web à distance. Vous pouvez vous connecter à votre compte Azure, créer des applications web, configurer des applications, afficher les journaux en temps réel et bien plus encore. Bientôt peu après le Kit de développement logiciel 2.2 est publié, vous serez en mesure d’exécuter en mode débogage à distance dans Azure. La plupart des nouvelles fonctionnalités pour Azure App Service Web Apps fonctionne également dans Visual Studio 2012 lorsque vous installez la version actuelle du SDK Azure pour .NET.

Pour plus d'informations, reportez-vous aux ressources suivantes :

- [Créer une application web ASP.NET dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Améliorations de la publication sur le Web

Visual Studio 2013 inclut des fonctionnalités nouvelles et améliorées de publication Web. Voici quelques-uns d'entre eux :

- Facilement [automatiser le chiffrement des fichiers Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Ce lien et les deux suivants pointent vers la documentation sur MSDN qui ne sont peut-être pas disponible jusqu’en fin de la journée sur 10/17.)
- Facilement [automatiser en prenant une application en mode hors connexion pendant le déploiement](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurer Web Deploy pour [utiliser somme de contrôle de fichier au lieu de la date de dernière modification](https://go.microsoft.com/fwlink/?LinkId=325531) pour déterminer quels fichiers doivent être copiés vers le serveur.
- Publier rapidement des fichiers individuels sélectionnés (y compris le fichier Web.config) lorsque vous utilisez le protocole FTP ou de méthodes de publication du système de fichiers ainsi qu’avec Web Deploy.

Pour plus d’informations sur le déploiement de web ASP.NET, consultez [le site ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 inclut un ensemble complet de nouvelles fonctionnalités qui sont décrites en détail à [Notes de publication de NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Cette version de NuGet supprime également la nécessité de fournir un consentement explicite pour la fonctionnalité de restauration de package de NuGet télécharger les packages. Consentement (et la case à cocher associée dans la boîte de dialogue Préférences de NuGet) sont maintenant accordées par l’installation de NuGet. Restauration des packages simplement fonctionne désormais par défaut.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

### <a name="one-aspnet"></a>One ASP.NET

Les modèles de projet Web Forms intègrent en toute transparence à la nouvelle expérience de One ASP.NET. Vous pouvez ajouter la prise en charge de MVC et API Web à votre projet Web Forms, et vous pouvez configurer l’authentification à l’aide de l’Assistant de création de projet One ASP.NET. Pour plus d’informations, consultez [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Les modèles de projet Web Forms prend en charge la nouvelle infrastructure d’identité ASP.NET. En outre, les modèles prennent désormais en charge la création d’un projet d’intranet Web Forms. Pour plus d’informations, consultez [méthodes d’authentification](creating-web-projects-in-visual-studio.md#auth) dans **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="bootstrap"></a>Programme d’amorçage

Utilisent les modèles Web Forms [Bootstrap](http://twitter.github.io/bootstrap/) pour fournir une élégante et réactive apparence que vous pouvez facilement personnaliser. Pour plus d’informations, consultez [Bootstrap dans les modèles de projet web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Les modèles de projet Web MVC intègrent en toute transparence à la nouvelle expérience de One ASP.NET. Vous pouvez personnaliser votre projet MVC et configurer l’authentification à l’aide de l’Assistant de création de projet One ASP.NET. Vous trouverez un didacticiel d’introduction à ASP.NET MVC 5 dans [mise en route avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Pour plus d’informations sur la mise à niveau des projets MVC 4 vers MVC 5, consultez [comment mettre à jour un ASP.NET MVC 4 et le projet d’API Web ASP.NET MVC 5 et API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Les modèles de projet MVC ont été mis à jour pour utiliser ASP.NET Identity pour l’authentification et de gestion des identités. Vous trouverez un didacticiel présentant l’authentification Facebook et Google et la nouvelle API d’appartenance dans [créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) et [créer une application ASP.NET MVC avec authentification et Base de données SQL et les déployer sur Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Programme d’amorçage

Le modèle de projet MVC a été mis à jour pour utiliser [Bootstrap](http://getbootstrap.com/) pour fournir une élégante et réactive apparence que vous pouvez facilement personnaliser. Pour plus d’informations, consultez [Bootstrap dans les modèles de projet web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtres d’authentification

Filtres d’authentification sont un nouveau type de filtre dans ASP.NET MVC qui s’exécutent avant les filtres d’autorisation dans le pipeline ASP.NET MVC et vous permettent de spécifier l’authentification logique d’une action, par contrôleur, ou dans le monde entier pour tous les contrôleurs. Filtres d’authentification traitement les informations d’identification dans la demande et fournissent un principal correspondant. Filtres d’authentification peuvent également ajouter des demandes d’authentification en réponse aux demandes non autorisées.

### <a name="filter-overrides"></a>Remplacements de filtre

Vous pouvez maintenant remplacer les filtres à appliquer à une méthode d’action donné ou un contrôleur en spécifiant un filtre de remplacement. Filtres de remplacement de spécifier un ensemble de types de filtre ne doit pas être exécuté pour une étendue donnée (action ou contrôleur). Cela vous permet de vous permettent de configurer des filtres qui s’appliquent globalement mais puis exclure certains filtres globaux de l’application à des actions spécifiques ou des contrôleurs.

### <a name="attribute-routing"></a>Routage par attributs

ASP.NET MVC prend désormais en charge le routage par attributs, grâce à une contribution par Tim McCall, l’auteur de [ http://attributerouting.net ](http://attributerouting.net). Avec le routage par attributs, vous pouvez spécifier vos itinéraires en annotant vos actions et les contrôleurs.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>API web ASP.NET 2

### <a name="attribute-routing"></a>Routage par attributs

API Web ASP.NET prend désormais en charge le routage par attributs, grâce à une contribution par Tim McCall, l’auteur de [ http://attributerouting.net ](http://attributerouting.net). Avec le routage par attributs, vous pouvez spécifier des itinéraires de votre API Web en annotant vos actions et les contrôleurs comme suit :

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Routage par attributs vous donne davantage de contrôle sur les URI dans votre API web. Par exemple, vous pouvez facilement définir une hiérarchie de ressources à l’aide d’un contrôleur d’API unique :

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Routage par attributs également fournit une syntaxe pratique pour spécifier des paramètres facultatifs, les valeurs par défaut et les contraintes de routage :

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Pour plus d’informations sur le routage par attributs, consultez [routage par attributs dans Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Les modèles de projet API Web et des applications à Page unique prennent désormais en charge l’autorisation à l’aide d’OAuth 2.0. OAuth 2.0 est une infrastructure d’autorisation d’accès client aux ressources protégées. Il fonctionne pour une variété de clients, y compris les navigateurs et appareils mobiles.

Prise en charge d’OAuth 2.0 est basé sur le nouveau sécurité intergiciel (middleware) fournis par les composants OWIN de Microsoft pour l’authentification du porteur et le rôle de serveur d’autorisation de mise en œuvre. Vous pouvez également les clients peuvent être autorisés à l’aide d’un serveur d’autorisation d’organisation, comme Azure Active Directory ou AD FS dans Windows Server 2012 R2.

### <a name="odata-improvements"></a>Améliorations d’OData

**Développez de prise en charge pour $select, $, $batch et $value**

ASP.NET Web API OData prend désormais en charge complète pour $select, $expand et $value. Vous pouvez également utiliser $batch de demande de traitement par lot et le traitement des ensembles de modifications.

$Select et $développez options vous permettent de changer la forme des données qui sont retournées à partir d’un point de terminaison OData. Pour plus d’informations, consultez [présentation $select et $étendent la prise en charge dans Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Extensibilité améliorée**

Les formateurs OData sont désormais extensibles. Vous pouvez ajouter les métadonnées d’entrée Atom, prend en charge les entrées de lien stream et média nommées, ajouter des annotations d’instance et personnaliser la façon dont les liens sont générés.

**Prise en charge sans type**

Vous pouvez maintenant générer des services OData sans avoir à définir les types CLR pour vos types d’entité. Au lieu de cela, vos contrôleurs OData peuvent prendre ou retournent des instances de **IEdmObject**, qui sont les formateurs OData sérialiser/désérialiser.

**Réutiliser un modèle existant**

Si vous avez déjà un entity data model (EDM) existant, vous pouvez maintenant réutiliser il directement, au lieu de devoir créer un nouveau. Par exemple, si vous utilisez Entity Framework, vous pouvez utiliser le modèle EDM EF génère pour vous.

### <a name="request-batching"></a>Demander le traitement par lot

Demande de traitement par lot combine plusieurs opérations en une seule requête HTTP POST, pour réduire le trafic réseau et de fournir une simple, moins l’interface utilisateur bavardes. API Web ASP.NET prend désormais en charge plusieurs stratégies de demande de traitement par lot :

- Utilisez le point de terminaison $batch d’un service OData.
- Empaqueter plusieurs requêtes dans une seule requête en plusieurs parties MIME.
- Utiliser un format personnalisé de traitement par lot.

Pour activer la demande de traitement par lot, ajoutez simplement un itinéraire avec un gestionnaire de traitement par lot à votre configuration de l’API Web :

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Vous pouvez également contrôler si des demandes ou exécutées séquentiellement ou dans n’importe quel ordre.

### <a name="portable-aspnet-web-api-client"></a>Client d’API Web ASP.NET portable

Vous pouvez maintenant utiliser le Client de API Web ASP.NET pour créer des bibliothèques de classes portables qui fonctionnent ensemble de vos applications Windows Store et Windows Phone 8. Vous pouvez également créer des formateurs portables qui peuvent être partagées entre client et serveur.

### <a name="improved-testability"></a>Testabilité améliorée

Web API 2 facilite grandement unité tester vos contrôleurs d’API. Simplement instancier votre contrôleur d’API avec votre message de demande et de la configuration, puis appelez la méthode d’action que vous souhaitez tester. Il est également facile de simuler la **UrlHelper** (classe), pour les méthodes d’action qui effectuent la génération de lien.

### <a name="ihttpactionresult"></a>IHttpActionResult

Vous pouvez désormais implémenter IHttpActionResult pour encapsuler le résultat de vos méthodes d’action API Web. Un IHttpActionResult retourné à partir d’une méthode d’action API Web est exécutée par le runtime de l’API Web ASP.NET pour produire le message de réponse résultant. Un IHttpActionResult peut être retourné à partir de toute action de l’API Web pour simplifier l’unité de test de votre implémentation de l’API Web. Pour plus de commodité que plusieurs IHttpActionResult implémentations sont fournies prêtes à l’emploi, y compris les résultats pour retourner des codes d’état spécifiques, mise en forme de contenu ou la négociation de contenu des réponses.

### <a name="httprequestcontext"></a>HttpRequestContext

La nouvelle **HttpRequestContext** effectue le suivi de n’importe quel état qui est lié à la demande, mais n’est pas immédiatement disponible à partir de la demande. Par exemple, vous pouvez utiliser la **HttpRequestContext** pour obtenir des données d’itinéraire, le principal associé à la demande, le certificat client, le **UrlHelper** et la racine du chemin d’accès virtuel. Vous pouvez facilement créer un **HttpRequestContext** des fins de test d’unité.

Étant donné que l’entité de sécurité pour la demande est transmise à la demande au lieu d’utiliser **Thread.CurrentPrincipal**, le principal est désormais disponible tout au long de la durée de vie de la demande s’il est dans le pipeline de l’API Web.

### <a name="cors"></a>CORS

Grâce à une autre grande contribution à partir de Brock Allen, ASP.NET prend désormais entièrement en charge Cross-Origin demande de partage (CORS).

La sécurité du navigateur empêche une page web d’effectuer des demandes AJAX vers un autre domaine. [CORS](http://www.w3.org/TR/cors/) est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.

API Web 2 prend désormais en charge CORS, y compris la gestion automatique des requêtes préliminaires. Pour plus d’informations, consultez [activation des demandes de Cross-Origin dans ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtres d’authentification

Filtres d’authentification sont un nouveau type de filtre dans les API Web ASP.NET qui s’exécutent avant les filtres d’autorisation dans le pipeline de l’API Web ASP.NET et vous permettent de spécifier l’authentification logique d’une action, par contrôleur, ou dans le monde entier pour tous les contrôleurs. Filtres d’authentification traitement les informations d’identification dans la demande et fournissent un principal correspondant. Filtres d’authentification peuvent également ajouter des demandes d’authentification en réponse aux demandes non autorisées.

### <a name="filter-overrides"></a>Substitutions de filtre

Vous pouvez maintenant remplacer les filtres à appliquer à une méthode d’action donné ou un contrôleur, en spécifiant un filtre de remplacement. Filtres de remplacement de spécifier un ensemble de types de filtre ne doit pas s’exécuter pour une étendue donnée (action ou contrôleur). Cela vous permet à ajouter des filtres globaux, mais ensuite exclure certaines à partir des actions spécifiques ou des contrôleurs.

### <a name="owin-integration"></a>Intégration OWIN

API Web ASP.NET désormais entièrement prend en charge OWIN et peut être exécuté sur n’importe quel hôte compatible OWIN. Est également inclus un **HostAuthenticationFilter** qui assure une intégration avec le système d’authentification OWIN.

Avec l’intégration de OWIN, vous pouvez Self-host API Web dans votre propre processus en même temps que d’autres intergiciels (middleware) OWIN, telles que SignalR. Pour plus d’informations, consultez [OWIN d’utilisation pour auto-héberger ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>SignalR ASP.NET 2.0

Les sections suivantes décrivent les fonctionnalités de SignalR 2.0.

- [Basée sur OWIN](#builtonowin)
- [MapHubs et MapConnection sont désormais MapSignalR](#MapSignalR)
- [Prise en charge des domaines](#crossdomain)
- [iOS et Android prennent en charge par le biais de MonoTouch et MonoDroid](#mobile)
- [Client .NET portable](#portable)
- [Nouveau Package Self-Host](#selfhost)
- [Prise en charge du serveur de compatibilité descendante](#backwardcompat)
- [Supprimer la prise en charge du serveur pour .NET 4.0](#remove40)
- [Envoi d’un message à une liste de clients et de groupes](#messagelist)
- [Envoi d’un message à un utilisateur spécifique](#sendtouser)
- [Une meilleure prise en charge de gestion des erreurs](#errorhandling)
- [Unité plus facile de hubs de test](#unittesting)
- [Gestion des erreurs de JavaScript](#javascripterror)

Pour obtenir un exemple de mise à niveau d’un projet 1.x existant vers SignalR 2.0, consultez [la mise à niveau un SignalR 1.x projet](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basée sur OWIN

SignalR 2.0 repose entièrement sur [OWIN (l’Open Web Interface pour .NET)](http://owin.org/). Cette modification rend le processus d’installation pour SignalR beaucoup plus cohérente entre les applications SignalR auto-hébergé et hébergé sur le web, mais doit également un nombre de modifications de l’API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs et MapConnection sont désormais MapSignalR

Pour la compatibilité avec les normes OWIN, ces méthodes ont été renommées `MapSignalR`. `MapSignalR` appelée sans paramètres mappera tous les concentrateurs (en tant que `MapHubs` dans la version 1.x) ; pour mapper individuels **PersistentConnection** objets, spécifiez le type de connexion en tant que le paramètre de type et l’extension d’URL pour la connexion en tant que le premier argument.

Le `MapSignalR` méthode est appelée dans une classe de démarrage Owin. Visual Studio 2013 contient un nouveau modèle pour une classe de démarrage Owin ; Pour utiliser ce modèle, procédez comme suit :

1. Avec le bouton droit sur le projet
2. Sélectionnez **ajouter**, **un nouvel élément...**
3. Sélectionnez **classe de démarrage Owin**. Nommez la nouvelle classe **Startup.cs**.

Dans un **application Web,** la classe de démarrage de Owin contenant le `MapSignalR` méthode est ensuite ajoutée au processus de démarrage de Owin à l’aide d’une entrée dans le nœud de paramètres d’application du fichier Web.Config, comme indiqué ci-dessous.

Dans un **auto-hébergé application**, la classe de démarrage est passée comme paramètre de type de la `WebApp.Start` (méthode).

**Mappage des hubs et connexions dans SignalR 1.x (depuis le fichier d’application globale dans une application web) :** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mappage des hubs et connexions dans 2.0 SignalR (à partir d’un fichier de classe de démarrage Owin) :** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

Dans un **auto-hébergé application**, la classe de démarrage est passée comme paramètre de type pour le `WebApp.Start` (méthode), comme indiqué ci-dessous.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Prise en charge des domaines

Dans SignalR 1.x, des requêtes entre domaines ont été contrôlé par un indicateur EnableCrossDomain unique. Cet indicateur contrôlé demandes JSONP et CORS. Pour une flexibilité accrue, tous les CORS prennent en charge a été supprimée à partir du composant serveur de SignalR (langue du JavaScript toujours utiliser CORS normalement s’il est détecté que le navigateur prend en charge), et nouvel intergiciel (middleware) OWIN soit devenue disponible pour prendre en charge ces scénarios.

Dans SignalR 2.0, si JSONP est requis sur le client (pour prendre en charge les demandes inter-domaines dans les navigateurs plus anciens), il doit être activée explicitement en définissant `EnableJSONP` sur le `HubConfiguration` objet `true`, comme illustré ci-dessous. JSONP est désactivée par défaut, car il est moins sécurisé que CORS.

Pour ajouter le nouvel intergiciel (middleware) CORS dans SignalR 2.0, ajoutez le `Microsoft.Owin.Cors` bibliothèque à votre projet, puis appelez `UseCors` avant votre intergiciel SignalR, comme indiqué dans la section ci-dessous.

**Ajout de Microsoft.Owin.Cors à votre projet**: pour installer cette bibliothèque, exécutez la commande suivante dans la Console du Gestionnaire de Package :

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Cette commande ajoutera le 2.0.0 version du package à votre projet.

**Appeler UseCors**

Les extraits de code suivants montrent comment implémenter des connexions inter-domaines dans SignalR 1.x et 2.0.

**Implémentation de demandes inter-domaines dans SignalR 1.x (à partir du fichier d’application globale)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implémentation de demandes inter-domaines dans 2.0 SignalR (à partir d’un fichier de code c#)**

Le code suivant montre comment activer CORS ou JSONP dans un projet SignalR 2.0. Cet exemple de code utilise `Map` et `RunSignalR` au lieu de `MapSignalR`, de sorte que l’intergiciel (middleware) CORS s’exécute uniquement pour les demandes de SignalR qui nécessitent la prise en charge CORS (plutôt que pour tout le trafic sur le chemin spécifié dans `MapSignalR`.) `Map` peut également être utilisé pour n’importe quel autre intergiciel (middleware) qui doit s’exécuter pour un préfixe d’URL spécifique, plutôt que pour l’application entière.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS et Android prennent en charge par le biais de MonoTouch et MonoDroid

Prise en charge a été ajoutée pour les clients iOS et Android à l’aide de composants MonoTouch et MonoDroid à partir de la [bibliothèque Xamarin](https://xamarin.com/). Pour plus d’informations sur leur utilisation, consultez [à l’aide des composants Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Ces composants seront disponibles dans le [Xamarin Store](https://store.xamarin.com/) lorsque la version RTW de SignalR est disponible.

<a id="portable"></a> ### Portable client .NET

Pour mieux facilitent le développement multiplateforme, le Silverlight, WinRT et les clients Windows Phone ont été remplacés par un seul client .NET portable qui prend en charge les plateformes suivantes :

- NET 4.5
- Silverlight 5
- WinRT (.NET pour les applications du Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nouveau Package Self-Host

Il existe désormais un package NuGet pour faciliter la prise en main auto-hébergement de SignalR (SignalR les applications qui sont hébergées dans un processus ou autre application, plutôt que hébergé sur un serveur web). Pour mettre à niveau un projet Self-host généré avec SignalR 1.x, supprimer le package Microsoft.AspNet.SignalR.Owin et ajoutez le package Microsoft.AspNet.SignalR.SelfHost. Pour plus d’informations sur la mise en route avec le package Self-host, consultez [didacticiel : auto-hébergement de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Prise en charge du serveur de compatibilité descendante

Dans les versions précédentes de SignalR, les versions de package SignalR utilisé dans le client et le serveur que se devait d’être identiques. Pour prendre en charge les applications client lourd qui seraient difficiles à mettre à jour, SignalR 2.0 prend désormais en charge l’à l’aide d’une version plus récente de serveur avec un client plus ancien. **Remarque : SignalR 2.0 ne prend pas en charge les serveurs créés avec des versions plus anciennes avec nouveaux clients.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Supprimer la prise en charge du serveur pour .NET 4.0

SignalR 2.0 a supprimé la prise en charge pour l’interopérabilité de serveur avec .NET 4.0. .NET 4.5 doit être utilisé avec les serveurs de SignalR 2.0. Il existe toujours un client .NET 4.0 pour SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Envoi d’un message à une liste de clients et de groupes

Dans SignalR 2.0, il est possible d’envoyer un message à l’aide d’une liste de client et ID de groupe. Les extraits de code suivants montrent comment effectuer cette opération.

**Envoi d’un message à une liste de clients et des groupes à l’aide de PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Envoi d’un message à une liste de clients et des groupes à l’aide de Hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Envoi d’un message à un utilisateur spécifique

Cette fonctionnalité permet aux utilisateurs de spécifier quel est le nom d’utilisateur basé sur un IRequest via une nouvelle interface IUserIdProvider :

**L’interface IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Par défaut il y aura une implémentation qui utilise IPrincipal.Identity.Name l’utilisateur comme nom d’utilisateur.

Dans les hubs, vous serez en mesure d’envoyer des messages à ces utilisateurs via une nouvelle API :

**À l’aide de l’API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Une meilleure prise en charge de gestion des erreurs

Les utilisateurs peuvent désormais lever **HubException** à partir de n’importe quel appel de concentrateur. Le constructeur de la **HubException** peut prendre un message de type chaîne et un objet des données d’erreur supplémentaires. SignalR automatique-sérialiser l’exception et l’envoyer au client sur laquelle il est utilisé pour l’appel de méthode de concentrateur de rejet ou d’échec.

Le **afficher les exceptions détaillées hub** paramètre n’a aucune incidence sur **HubException** qui est envoyé au client ou non ; il est toujours envoyé.

**Code côté serveur illustrant l’envoi d’une HubException au client**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Code JavaScript client illustrant la réponse à une HubException envoyée depuis le serveur**

[!code-html[Main](release-notes/samples/sample16.html)]

**Code de client .NET illustrant répond à une HubException envoyée à partir du serveur**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Unité plus facile de hubs de test

SignalR 2.0 inclut une interface appelée `IHubCallerConnectionContext` sur les concentrateurs qui les rend plus facile de créer des appels de côté client fictif. Les extraits de code suivants illustrent l’utilisation de cette interface avec des ateliers de test populaires [xUnit.net](https://github.com/xunit/xunit) et [moq](https://code.google.com/p/moq/).

**Tests unitaires SignalR avec xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Tests unitaires SignalR avec moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Gestion des erreurs de JavaScript

Dans SignalR 2.0, tous les rappels de la gestion des erreurs JavaScript retournent des objets d’erreur JavaScript au lieu de chaînes brutes. Cela permet de SignalR transférer les informations plus riches à vos gestionnaires d’erreurs. Vous pouvez obtenir l’exception interne à partir de la `source` propriété de l’erreur.

**Code JavaScript client qui gère l’exception Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nouveau système d’appartenance ASP.NET

ASP.NET Identity est le nouveau système d’appartenance pour les applications ASP.NET. ASP.NET Identity vous permet de facilement intégrer des données de profil d’utilisateur avec les données d’application. ASP.NET Identity vous permet également de choisir le modèle de persistance pour les profils utilisateur dans votre application. Vous pouvez stocker les données dans une base de données SQL Server ou un autre magasin de données, y compris les magasins de données NoSQL telles que les Tables de stockage Azure. Pour plus d’informations, consultez [comptes d’utilisateur individuels](creating-web-projects-in-visual-studio.md#indauth) dans **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Authentification basée sur les revendications

ASP.NET prend désormais en charge l’authentification basée sur les revendications, où l’identité de l’utilisateur est représentée comme un ensemble de revendications à partir d’un émetteur approuvé. Les utilisateurs peuvent être authentifiés à l’aide d’un nom d’utilisateur et le mot de passe mis à jour dans une base de données d’application ou à l’aide de fournisseurs d’identité sociale (par exemple : Accounts Microsoft, Facebook, Google, Twitter), ou à l’aide de comptes de société via Azure Active Directory ou Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Intégration avec Azure Active Directory et Windows Server Active Directory

Vous pouvez désormais créer des projets ASP.NET qui utilisent Azure Active Directory ou Windows Server Active Directory (AD) pour l’authentification. Pour plus d’informations, consultez [comptes professionnels](creating-web-projects-in-visual-studio.md#orgauth) dans **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="owin-integration"></a>Intégration OWIN

Authentification ASP.NET est désormais basée sur l’intergiciel OWIN qui peut être utilisé sur n’importe quel hôte OWIN. Pour plus d’informations sur OWIN, consultez la rubrique suivante [des composants Microsoft OWIN](#TOC7) section.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Composants Microsoft OWIN

[Open Web Interface pour .NET](http://owin.org/) (OWIN) définit une abstraction entre les serveurs web de .NET et des applications web. OWIN dissocie l’application web à partir du serveur, rendre indépendant du web applications hôte. Par exemple, vous pouvez héberger une application web basée sur OWIN dans IIS ou auto-hébergée dans un processus personnalisé.

Modifications introduites dans les composants Microsoft OWIN (également connu sous le projet Katana) incluent nouveaux composants de serveur et l’hôte, nouvelles bibliothèques d’assistance intergiciel (middleware) et des nouveaux intergiciel d’authentification.

Pour plus d’informations sur OWIN et Katana, consultez [Nouveautés OWIN et Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Remarque : [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications ne peut pas s’exécuter en mode classique IIS ; ils doivent être exécutés en mode intégré.**

**Remarque : [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications doivent être exécutées avec une confiance totale.**

### <a name="new-servers-and-hosts"></a>Ordinateurs hôtes et les nouveaux serveurs

Avec cette version, les nouveaux composants ont été ajoutées pour activer des scénarios d’auto-hébergement. Ces composants incluent les packages NuGet suivants :

- **Microsoft.Owin.Host.HttpListener**. Fournit un serveur OWIN qui utilise **HttpListener** pour écouter les requêtes HTTP et les diriger au pipeline OWIN.
- **Microsoft.Owin.Hosting** fournit une bibliothèque pour les développeurs qui souhaitent auto-héberger un pipeline OWIN dans un processus personnalisé, par exemple une application console ou un service de Windows.
- **OwinHost**. Fournit un exécutable autonome qui encapsule `Microsoft.Owin.Hosting` et vous permet d’héberger automatiquement un pipeline OWIN sans avoir à écrire une application hôte personnalisé.

En outre, le `Microsoft.Owin.Host.SystemWeb` package permet désormais d’intergiciel (middleware) fournir des indications pour la **SystemWeb** serveur, indiquant que l’intergiciel (middleware) doit être appelée au cours d’une étape du pipeline ASP.NET spécifique. Cette fonctionnalité est particulièrement utile pour l’intergiciel d’authentification, qui doit s’exécuter très tôt dans le pipeline ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Intergiciel (middleware) et les bibliothèques d’assistance

Bien que vous pouvez écrire des composants OWIN à l’aide uniquement les définitions de fonction et le type de la spécification OWIN, la nouvelle `Microsoft.Owin` package fournit un ensemble plus convivial d’abstractions. Cette solution combine plusieurs packages antérieures (par exemple, `Owin.Extensions`, `Owin.Types`) dans un modèle d’objet unique et bien structurée peut ensuite être facilement utilisé par d’autres composants OWIN. En fait, la majorité des composants Microsoft OWIN maintenant utiliser ce package.

> [!NOTE]
> [OWIN](http://www.owin.org) applications ne peut pas s’exécuter en mode classique IIS ; ils doivent être exécutés en mode intégré.

> [!NOTE]
> [OWIN](http://www.owin.org) applications doivent être exécutées avec une confiance totale.

Cette version inclut également le package Microsoft.Owin.Diagnostics, qui inclut des intergiciels (middleware) pour valider une application OWIN en cours d’exécution, ainsi que des intergiciels (middleware) de la page d’erreur pour aider à analyser les échecs.

### <a name="authentication-components"></a>Composants d’authentification

Les composants d’authentification suivantes sont disponibles.

- **Microsoft.Owin.Security.ActiveDirectory**. Active l’authentification à l’aide des services d’annuaire sur site ou sur le cloud.
- **Microsoft.Owin.Security.Cookies** permet l’authentification à l’aide de cookies. Ce package s’appelait précédemment `Microsoft.Owin.Security.Forms`.
- **Microsoft.owin.Security.Facebook contient** permet l’authentification à l’aide des services de Facebook OAuth.
- **Microsoft.Owin.Security.Google** permet l’authentification à l’aide des services de Google OpenID.
- **Microsoft.Owin.Security.Jwt** permet l’authentification à l’aide de jetons JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** permet l’authentification à l’aide de comptes Microsoft.
- **Microsoft.Owin.Security.OAuth**. Fournit un serveur d’autorisation OAuth, mais aussi les intergiciels (middleware) pour l’authentification des jetons du porteur.
- **Microsoft.Owin.Security.Twitter** permet l’authentification à l’aide des services de Twitter OAuth.

Cette version inclut également le `Microsoft.Owin.Cors` package, qui contient l’intergiciel (middleware) pour le traitement des demandes HTTP de cross-origin.

> [!NOTE]
> Prise en charge pour la signature JWT a été supprimée dans la version finale de Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Pour obtenir la liste des nouvelles fonctionnalités et d’autres modifications dans Entity Framework 6, consultez [l’historique de Version Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>Razor ASP.NET 3

Razor ASP.NET 3 inclut les nouvelles fonctionnalités suivantes :

- Prise en charge pour la modification de l’onglet. Auparavant, le **mettre le Document** commande, la mise en retrait automatique et auto mise en forme dans Visual Studio n’a pas fonctionné correctement lorsque vous utilisez le **conserver les tabulations** option. Cette modification résout Visual Studio pour le code Razor pour l’onglet mise en forme de mise en forme.
- Prise en charge pour les règles de réécriture d’URL lors de la génération des liens.
- Suppression de l’attribut transparent de sécurité.
  > [!NOTE]
  > Cela est une modification avec rupture et rend Razor 3 incompatibles avec MVC 4 et versions antérieures, tandis que Razor 2 n’est pas compatible avec MVC5 ou des assemblys compilés avec MVC5.

Razor 3 les problèmes corrigés dans Visual Studio 2013 à partir de versions préliminaires [ici](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET App Suspend

ASP.NET App Suspend est une fonctionnalité révolutionnaires dans le .NET Framework 4.5.1 qui modifie radicalement l’expérience utilisateur et le modèle économique pour l’hébergement du grand nombre de sites d’ASP.NET sur un ordinateur unique. Pour plus d’informations, consultez [ASP.NET App Suspend – réactive partagé d’hébergement web .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et les modifications avec rupture

Cette section décrit les problèmes connus et les modifications avec rupture dans ASP.NET et Web Tools pour Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Restauration des packages ne fonctionne pas sur Mono lorsque vous utilisez les fichiers SLN](https://nuget.codeplex.com/workitem/3596) – sera résolu dans un téléchargement de nuget.exe à venir et [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) mettre à jour.
- [Restauration des packages ne fonctionne pas avec des projets Wix](https://nuget.codeplex.com/workitem/3598) – sera résolu dans un téléchargement de nuget.exe à venir et [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) mettre à jour.
- [Restauration de Package automatique ne fonctionne pas pour les projets dans un dossier de solution](https://nuget.codeplex.com/workitem/3625) – seront résolus dans NuGet 2.8.

### <a name="aspnet-web-api"></a>API web ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` ne retourne pas `IQueryable<T>` toujours, que nous avons ajouté la prise en charge de `$select` et `$expand`.

    Nos exemples antérieures pour `ODataQueryOptions<T>` toujours convertir la valeur de retour à partir de `ApplyTo` à `IQueryable<T>`. Celle-ci fonctionnait précédemment, car les options de la requête que nous avons pris en charge précédemment (`$filter`, `$orderby`, `$skip`, `$top`) ne changent pas la forme de la requête. Maintenant que nous prenons en charge `$select` et `$expand` la valeur de retour à partir de `ApplyTo` ne sera pas `IQueryable<T>` toujours.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Si vous utilisez l’exemple de code indiquée précédemment, il continuera de fonctionner si le client n’envoie pas `$select` et `$expand`. Toutefois, si vous souhaitez prendre en charge `$select` et `$expand` vous devez modifier le code pour cela.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url ou RequestContext.Url a la valeur null lors d’une demande de lot**

    Dans un scénario de traitement par lot, **UrlHelper** a la valeur null lors de l’accès à partir de **Request.Url** ou **RequestContext.Url**.

    Ce problème est suivi actuellement ici : [BatchRequestContext.Url a la valeur null pour le traitement par lots demande](http://aspnetwebstack.codeplex.com/workitem/1301).

    La solution de contournement pour ce problème consiste à créer une nouvelle instance de **UrlHelper**, comme dans l’exemple suivant :

    **Création d’une nouvelle instance de UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Lorsque vous utilisez MVC5 et OrgAuth, si vous avez vues qui effectuent une validation de AntiForgerToken, vous pouvez trouver l’erreur suivante lors de la validation des données à la vue :

    **Erreur**:

    *Erreur de serveur dans l’Application '/'.*

    <em>Une revendication de type «<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'ou'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' n’était pas présent sur le ClaimsIdentity fourni. Pour activer la prise en charge avec l’authentification basée sur les revendications de jeton anti-contrefaçon, vérifiez que le fournisseur de revendications configurée fournit les deux de ces revendications sur les instances ClaimsIdentity qu'il génère. Si le fournisseur de revendications configurée utilise à la place un type de revendication différent comme identificateur unique, il peut être configuré en définissant la propriété statique AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Solution de contournement**:

    Dans Global.asax pour résoudre ce problème, ajoutez la ligne suivante :

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Ce problème sera résolu pour la prochaine version.
2. Après la mise à niveau une application MVC 4 vers MVC5, générez la solution, puis lancez-le. Vous devez voir l’erreur suivante :

    [A] System.Web.WebPages.Razor.Configuration.HostSection ne peut pas être casté en [B]System.Web.WebPages.Razor.Configuration.HostSection. Type A provient « System.Web.WebPages.Razor, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' dans le contexte 'Default' à l’emplacement ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll ». Type B provient « System.Web.WebPages.Razor, Version = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' dans le contexte 'Default' à l’emplacement ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll ».

    Pour corriger l’erreur ci-dessus, ouvrez *tous les* les fichiers Web.config (y compris celles figurant dans le dossier Views) dans votre projet, puis procédez comme suit :

   1. Mettre à jour toutes les occurrences de la version « 4.0.0.0 » de « System.Web.Mvc » à « 5.0.0.0 ».
   2. Mettre à jour toutes les occurrences de la version « 2.0.0.0 » de « System.Web.Helpers », &quot;System.Web.WebPages&quot; et &quot;System.Web.WebPages.Razor&quot; à « 3.0.0.0 »

      Par exemple, après avoir apporté les modifications ci-dessus, les liaisons d’assembly doivent ressembler à ceci :

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Pour plus d’informations sur la mise à niveau des projets MVC 4 vers MVC 5, consultez [comment mettre à jour un ASP.NET MVC 4 et le projet d’API Web ASP.NET MVC 5 et API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Lorsque vous utilisez la validation côté client avec jQuery Unobtrusive Validation, le message de validation est parfois incorrect pour un élément input HTML avec type = 'number'. L’erreur de validation pour une valeur requise (« âge le champ est obligatoire ») s’affichent quand un nombre non valide est entré au lieu du message correct, qu’un nombre valid est requis.

    Ce problème est généralement trouvé avec le code généré automatiquement pour un modèle avec une propriété entière sur les vues Create et Edit.

    Pour contourner ce problème, modifiez l’application d’assistance de l’éditeur à partir de :

    `@Html.EditorFor(person => person.Age)`

    À :

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 ne gère plus de confiance partielle. Projets de liaison pour les fichiers binaires MVC ou API Web doivent supprimer le [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) attribut et la [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) attribut. Suppression de ces attributs permet d’éliminer les erreurs du compilateur semblable à celui-ci.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Notez que, comme un effet secondaire de ce que vous ne pouvez pas utiliser des assemblys 4.0 et 5.0 dans la même application. Vous devez mettre à jour tous les 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Modèle SPA avec l’autorisation de Facebook peut entraîner une instabilité dans Internet Explorer tandis que le site web est hébergée dans la zone intranet

Le modèle SPA fournit un journal externe avec Facebook. Lorsque le projet créé avec le modèle est en cours d’exécution localement, risque de IE à se bloquer la connexion.

Solution :

1. Héberger le site web dans la zone internet ; ou

2. Le scénario de test dans un navigateur autre qu’Internet Explorer.

### <a name="web-forms-scaffolding"></a>Web Forms de génération de modèles automatique

Génération de modèles automatique Web Forms a été supprimée de VS2013 et sera disponible dans une prochaine mise à jour pour Visual Studio. Toutefois, vous pouvez toujours utiliser la structure dans un projet Web Forms en ajoutant des dépendances MVC et la génération de génération de modèles automatique pour MVC. Votre projet contient une combinaison de Web Forms et MVC.

Pour ajouter MVC à votre projet Web Forms, ajouter un nouvel élément généré automatiquement et sélectionnez **des dépendances MVC 5**. Sélectionnez minimales ou complètes selon que vous devez tous les fichiers de contenu, tels que les scripts. Ensuite, ajoutez un élément généré automatiquement pour MVC, ce qui crée un contrôleur et les vues dans votre projet.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC et Web API génération de modèles automatique - erreur HTTP 404, erreur introuvable

Si une erreur s’est produite lors de l’ajout d’un élément généré automatiquement à un projet, il est possible de que votre projet reste dans un état incohérent. Certaines des modifications apportées à être de génération de modèles automatique seront restaurée, mais d’autres modifications, comme les packages NuGet installés, ne seront pas restaurées. Si les modifications de configuration de routage sont annulées, les utilisateurs recevront une erreur HTTP 404 lorsque accédant à structurée des éléments.

Solution de contournement :

- Pour corriger cette erreur pour MVC, ajoutez un nouvel élément généré automatiquement et sélectionnez des dépendances MVC 5 (complète ou minimale). Ce processus sera ajoutez toutes les modifications nécessaires à votre projet.
- Pour corriger cette erreur pour l’API Web :

  1. Ajoutez la classe WebApiConfig à votre projet.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configurer WebApiConfig.Register dans l’Application\_méthode Start dans Global.asax comme suit :

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
