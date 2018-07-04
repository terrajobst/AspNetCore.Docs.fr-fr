---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Création de projets Web ASP.NET dans Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: Cette rubrique explique les options de création de projets web ASP.NET dans Visual Studio 2013 avec Update 3 ici sont certaines des nouvelles fonctionnalités pour c de développement web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 6033163b7b8e9e1d0524935217dc53f938470cb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363466"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Création de projets Web ASP.NET dans Visual Studio 2013
====================
par [Tom Dykstra](https://github.com/tdykstra)

> Cette rubrique explique les options de création de projets web ASP.NET dans Visual Studio 2013 avec Update 3
> 
> Voici quelques-unes des nouvelles fonctionnalités pour le développement web par rapport aux versions antérieures de Visual Studio :
> 
> - Une interface utilisateur simple pour la création de projets de cette offre [prennent en charge plusieurs infrastructures ASP.NET](#add) (Web Forms, MVC et API Web).
> - [ASP.NET Identity](#indauth), un nouveau système d’appartenance ASP.NET qui fonctionne de la même dans toutes les infrastructures ASP.NET et fonctionne avec des logiciels autres que IIS d’hébergement web.
> - L’utilisation de [Bootstrap](#bootstrap) pour fournir des capacités de création et de thèmes réactives.
> - Nouvelles fonctionnalités de Web Forms qui permet uniquement proposées pour MVC, tel que [la création de projet de test automatique](#testproj) et un [modèle de site Intranet](#winauth).
> 
> Pour plus d’informations sur la création de projets web pour les Services Cloud Azure ou Azure Mobile Services, consultez [prise en main Azure Cloud Services et ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) et [création d’une application de Leaderboard avec Azure Mobile Services .NET Serveur principal](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prérequis

Cet article s’applique aux [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) avec [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installé.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projets d’application Web et projets de site web

ASP.NET vous offre le choix entre deux types de projets web : *projets d’application web* et *projets de site web*. Nous vous recommandons de projets d’application web pour tout nouveau développement, et cet article s’applique uniquement aux projets d’application web. Pour plus d’informations, consultez [projets d’Application Web et projets de Site Web dans Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) sur le site MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Vue d’ensemble de la création de projet d’application web

Les étapes suivantes montrent comment créer un projet web :

1. Cliquez sur **nouveau projet** dans le **Démarrer** page ou dans le **fichier** menu.
2. Dans le **nouveau projet** boîte de dialogue, cliquez sur **Web** dans le volet gauche et **Application Web ASP.NET** dans le volet central.

    ![Boîte de dialogue Nouveau projet](creating-web-projects-in-visual-studio/_static/image1.png)

    Vous pouvez choisir **Cloud** dans le volet gauche pour créer un [Azure Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Service Mobile Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), ou [une tâche Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Cette rubrique n’aborde pas ces modèles.
3. Dans le volet droit, cliquez sur le **ajouter Application Insights au projet** case à cocher si vous souhaitez que l’intégrité et la surveillance de l’utilisation de votre application. Pour plus d’informations, consultez [surveiller les performances dans les applications web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Spécifier le projet **nom**, **emplacement**et autres options, puis cliquez sur **OK**.

    Le **nouveau projet ASP.NET** boîte de dialogue apparaît.

    ![Boîte de dialogue Nouveau projet](creating-web-projects-in-visual-studio/_static/image2.png)
5. Cliquez sur un modèle.

    ![Sélectionnez un modèle](creating-web-projects-in-visual-studio/_static/image3.png)
6. Si vous souhaitez ajouter la prise en charge des infrastructures supplémentaires non inclus dans le modèle, cliquez sur la case à cocher appropriée. (Dans l’exemple, vous pouvez ajouter MVC et/ou API Web à un projet Web Forms.)

    ![Ajouter des infrastructures](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Si vous souhaitez ajouter un projet de test unitaire, cliquez sur **ajouter des tests unitaires**.

    ![Ajouter des tests unitaires](creating-web-projects-in-visual-studio/_static/image5.png)
8. Si vous souhaitez une autre méthode d’authentification que ce que fournit le modèle par défaut, cliquez sur **modifier l’authentification**.

    ![Configurer l’authentification bouton](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurer la boîte de dialogue authentification](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Créer une application web ou une machine virtuelle dans Azure

Visual Studio inclut des fonctionnalités qui facilitent l’utilisation des services Azure pour l’hébergement d’applications web. Par exemple, vous pouvez effectuer toutes les conditions suivantes directement à partir de l’IDE Visual Studio :

- Créer et gérer des applications web ou des machines virtuelles qui rendent votre application disponible sur Internet.
- Afficher les journaux créés par l’application en cours d’exécution dans le cloud.
- Exécuter en mode débogage à distance alors que l’application s’exécute dans le cloud.
- Viiew et gérer d’autres services tels que des bases de données SQL Azure.

Vous pouvez [créer un compte Azure](https://www.windowsazure.com/pricing/free-trial/) qui inclut des services de base telles que les applications web gratuitement, et si vous êtes abonné à MSDN, vous pouvez [activer les avantages](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) qui fournissent des crédits mensuels vers Azure supplémentaires Services. 

Par défaut le **nouveau projet ASP.NET** boîte de dialogue vous permet de créer une application web ou une machine virtuelle pour un nouveau projet web. Si vous ne souhaitez pas créer une nouvelle application web ou une machine virtuelle, désactivez le **hôte dans le cloud** case à cocher.

![Créer des ressources distantes](creating-web-projects-in-visual-studio/_static/image8.png)

La légende de la case à cocher peut être **hôte dans le cloud** ou **créer des ressources distantes**, et dans les deux cas, l’effet est le même. Si vous laissez la case à cocher, Visual Studio crée une application web dans Azure App Service par défaut. Vous pouvez utiliser la zone de liste déroulante pour la remplacer par un **Machine virtuelle** si vous préférez. Si vous n’êtes pas déjà connecté à Azure, vous êtes invité pour les informations d’identification Azure. Après que vous être connecté, une boîte de dialogue vous permet de configurer les ressources que Visual Studio va créer pour votre projet. L’illustration suivante montre la boîte de dialogue pour une application web ; différentes options s’affichent si vous choisissez de créer une machine virtuelle.

![Configurer les paramètres d’application Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Pour plus d’informations sur l’utilisation de ce processus pour la création de ressources Azure, consultez [prise en main Azure et ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) et [création d’une machine virtuelle pour un site web avec Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Le reste de cet article fournit des informations sur les modèles disponibles et leurs options. Il présente également framework Bootstrap, la disposition et le thème utilisé dans les modèles.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modèles de projet Web Visual Studio 2013

Visual Studio utilise des modèles pour créer des projets web. Un modèle de projet peut créer des fichiers et dossiers dans le nouveau projet, installez les packages NuGet et fournit un exemple de code pour une application opérationnelle rudimentaire. Les modèles implémentent les dernières normes web et sont destinés à illustrer les meilleures pratiques pour savoir comment utiliser les technologies ASP.NET ainsi que pour vous donner un saut démarrer sur la création de votre propre application.

Visual Studio 2013 fournit les options suivantes pour les modèles de projet web pour les projets qui ciblent le .NET 4.5 ou versions ultérieures du .NET framework :

- [Modèle vide](#empty)
- [Modèle de formulaires Web](#wf)
- [Modèle MVC](#mvc)
- [Modèle d’API Web](#webapi)
- [Modèle d’Application à Page unique](#spa)
- [Modèle de Service Mobile Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modèles de Visual Studio 2012](#vs2012)

Vous pouvez également installer une extension Visual Studio qui fournit un [Facebook modèle](#facebook).

Pour plus d’informations sur la création de projets qui ciblent le .NET 4, consultez [modèles de Visual Studio 2012](#vs2012) plus loin dans cette rubrique.

Pour plus d’informations sur la façon de créer des applications ASP.NET pour les clients mobiles, consultez [prise en charge Mobile dans ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modèle vide

Le modèle vide fournit les dossiers au minimum un système et les fichiers d’une application web ASP.NET, tel qu’un fichier de projet (*.csproj* ou. *vbproj*) et un *Web.config* fichier. Vous pouvez ajouter la prise en charge des Web Forms, MVC, et/ou une API Web en utilisant les cases à cocher sous la **ajouter des dossiers et les références principales pour :** étiquette.

Pour le modèle vide, aucune option d’authentification n’est disponibles. Fonctionnalité d’authentification est implémentée dans les exemples d’applications, et le modèle vide ne crée pas un exemple d’application.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modèle de formulaires Web

Le framework fournit les fonctionnalités suivantes qui vous permettent de rapidement créer des sites web qui sont riches en données et de l’interface utilisateur Web Forms accéder aux fonctionnalités :

- Un outil de conception dans Visual Studio.
- Contrôles serveur qui a rendu HTML et que vous pouvez personnaliser en définissant des propriétés et des styles.
- Un large éventail de contrôles pour l’accès aux données et l’affichage de données.
- Un modèle d’événement qui expose des événements dans lequel vous pouvez programmer tels que vous devez programmer une application cliente telles que WPF.
- Conservation automatique de l’état (données) entre les requêtes HTTP.

En général, la création d’une application Web Forms nécessite moins d’efforts programmation que la création de la même application à l’aide de l’infrastructure ASP.NET MVC. Toutefois, Web Forms n’est pas seulement pour le développement rapide d’applications. Il existe de nombreuses applications commerciales complexes et infrastructures reposant sur des Web Forms.

Comme une page Web Forms et les contrôles sur la page génèrent automatiquement une grande partie de la balise qui est envoyée au navigateur, vous n’avez le type d’un contrôle affiné sur le code HTML qui offre d’ASP.NET MVC. Le modèle déclaratif pour la configuration des pages et contrôles réduit la quantité de code, vous devez écrire mais masque une partie du comportement du code HTML et HTTP. Par exemple, il n’est pas toujours possible de spécifier exactement quelles balises peuvent être générés par un contrôle.

L’infrastructure Web Forms ne se prête pas aussi facilement qu’ASP.NET MVC à des pratiques de développement basé sur des modèles tels que [développement piloté par test](http://en.wikipedia.org/wiki/Test-driven_development), [séparation des préoccupations](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversion de contrôle](http://en.wikipedia.org/wiki/Inversion_of_control), et [l’injection de dépendances](http://en.wikipedia.org/wiki/Dependency_injection). Si vous souhaitez écrire de code prises en charge de cette façon, vous pouvez ; Il n’est pas aussi automatique car il s’agit de l’infrastructure ASP.NET MVC. Le [ASP.NET Web Forms MVP](http://webformsmvp.com/) projet montre une approche qui facilite la séparation des intérêts et de la testabilité, tout en conservant le développement rapide Web Forms a été conçu pour offrir. Microsoft SharePoint s’appuie sur des Web Forms MVP.

Le modèle Web Forms crée un exemple d’application Web Forms qui utilise [Bootstrap](#bootstrap) pour fournir des fonctionnalités de création et de thèmes réactives. L’illustration suivante montre la page d’accueil.

![Page d’accueil application modèle Web Forms](creating-web-projects-in-visual-studio/_static/image10.png)

Pour plus d’informations sur les Web Forms, consultez [ASP.NET Web Forms](https://asp.net/web-forms). Pour plus d’informations sur ce que fait le modèle Web Forms pour vous, consultez [générer une application Web Forms de base à l’aide de Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modèle MVC

ASP.NET MVC a été conçu pour faciliter les pratiques de développement basé sur des modèles tels que [développement piloté par test](http://en.wikipedia.org/wiki/Test-driven_development), [séparation des préoccupations](http://en.wikipedia.org/wiki/Separation_of_concerns), [l’inversion de contrôle](http://en.wikipedia.org/wiki/Inversion_of_control), et [l’injection de dépendances](http://en.wikipedia.org/wiki/Dependency_injection). Cette infrastructure encourage en séparant la couche de logique métier d’une application web à partir de sa couche de présentation. En divisant l’application dans les modèles (M), des vues (V) et des contrôleurs (C), ASP.NET MVC peut rendre plus facile à gérer la complexité dans les applications de grande taille.

Avec ASP.NET MVC, vous travaillez plus directement avec HTML et HTTP que dans Web Forms. Par exemple, Web Forms peut conserver automatiquement l’état entre les requêtes HTTP, mais vous avez vers du code qui explicitement dans MVC. L’avantage du modèle MVC est qu’il vous permet de prendre le contrôle complet sur exactement ce que fait votre application et comment elle se comporte dans l’environnement web. L’inconvénient est que vous avez à écrire davantage de code.

MVC a été conçu pour être extensible, en fournissant aux développeurs de power la possibilité de personnaliser le framework pour leurs besoins de l’application. En outre, le code source ASP.NET MVC est disponible sous licence OSI.

Le modèle MVC crée un exemple d’application MVC 5 qui utilise [Bootstrap](#bootstrap) pour fournir des fonctionnalités de création et de thèmes réactives. L’illustration suivante montre la page d’accueil.

![Exemple d’application MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Pour plus d’informations sur MVC, consultez [ASP.NET MVC](https://asp.net/mvc). Pour plus d’informations sur la façon de sélectionner le modèle MVC 4, consultez [modèles Visual Studio 2012](#vs2012) plus loin dans cet article.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modèle d’API Web

Le modèle API Web crée un exemple de service web basé sur les API Web, y compris les pages d’aide API basées sur MVC.

API Web ASP.NET est une infrastructure qui facilite la création de services HTTP qui atteignent une large gamme de clients, y compris les navigateurs et appareils mobiles. API Web ASP.NET est une plateforme idéale pour créer des services RESTful sur le .NET Framework.

Le modèle API Web crée un exemple de service web. Les illustrations suivantes montrent des exemples de pages aide.

![Page d’aide API Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Page d’aide API Web pour obtenir des API](creating-web-projects-in-visual-studio/_static/image13.png)

Pour plus d’informations sur l’API Web, consultez [API Web ASP.NET](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Modèle d’Application monopage

Le modèle d’Application à Page unique (SPA) crée un exemple d’application qui utilise JavaScript, HTML 5, et [KnockoutJS](http://knockoutjs.com/) sur le client et ASP.NET Web API sur le serveur.

Est la seule option d’authentification pour le modèle SPA [comptes d’utilisateur individuels](#indauth).

L’illustration suivante montre l’état initial de l’exemple d’application qui génère le modèle SPA.

![Exemple d’application SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Pour plus d’informations sur la création d’une application en utilisant le modèle SPA, consultez [API Web - Services d’authentification externe](../../../web-api/overview/security/external-authentication-services.md).

Pour plus d’informations sur les Applications à Page unique ASP.NET et les modèles SPA supplémentaires qui utilisent des infrastructures JavaScript autre que KnockoutJS, consultez les ressources suivantes :

- [ASP.NET Single Page Application](../../../single-page-application/index.md).
- [Présentation des fonctionnalités de sécurité dans le modèle SPA pour VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Applications à Page unique : Créer des applications Web modernes et réactives avec ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modèle pour Facebook

Vous pouvez installer un [extension Visual Studio qui fournit un modèle de Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Ce modèle crée un exemple d’application qui est conçu pour s’exécuter dans le site web Facebook. Il est basé sur ASP.NET MVC et utilise les API Web pour la fonctionnalité de mise à jour en temps réel.

Aucune option d’authentification n’est disponibles pour le modèle de Facebook, car les applications Facebook s’exécutent dans le site Facebook et s’appuient sur l’authentification de Facebook.

Pour plus d’informations sur les applications ASP.NET Facebook, consultez [mise à jour de l’API de Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modèles de Visual Studio 2012

La boîte de dialogue Création de projet web Visual Studio 2013 ne donne pas accès à certains modèles qui étaient disponibles dans Visual Studio 2012. Si vous souhaitez utiliser un de ces modèles, vous pouvez cliquer sur le nœud Visual Studio 2012 dans le volet gauche de la boîte de dialogue Nouveau projet Visual Studio.

![Modèles de Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Le **Visual Studio 2012** nœud vous permet de choisir les modèles web suivants que vous n’avez pas accès à la liste par défaut des modèles pour Visual Studio 2013 :

- Application Web ASP.NET MVC 4
- Application Web ASP.NET Dynamic Data Entities
- Contrôle serveur ASP.NET AJAX
- Extendeur de contrôle serveur ASP.NET AJAX
- Contrôle serveur ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Données d’amorçage dans les modèles de projet web Visual Studio 2013

Utilisent les modèles de projet Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), une infrastructure de mise en page et des thèmes créée par Twitter. Programme d’amorçage utilise CSS3 pour fournir une conception réactive, ce qui signifie que des dispositions puissent s’adapter dynamiquement aux tailles de fenêtre de navigateur différents. Par exemple, dans une fenêtre de navigateur large la page d’accueil créée par le modèle Web Forms ressemble à l’illustration suivante :

![Page d’accueil application modèle Web Forms](creating-web-projects-in-visual-studio/_static/image16.png)

Rétrécir la fenêtre, et les colonnes sont organisés horizontalement déplacement dans une disposition verticale :

![Disposition de colonne verticale d’amorçage](creating-web-projects-in-visual-studio/_static/image17.png)

Affiner un peu plus de la fenêtre, et le menu supérieur horizontal se transforme en une icône que vous pouvez cliquer pour développer dans un menu orienté verticalement :

![Icône de menu d’amorçage](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu vertical d’amorçage](creating-web-projects-in-visual-studio/_static/image19.png)

Vous pouvez également utiliser la fonctionnalité de thèmes de Bootstrap pour facilement effectuer un changement dans l’apparence de l’application. Par exemple, peut procéder comme suit pour modifier le thème.

1. Dans votre navigateur, accédez à [ http://Bootswatch.com ](http://Bootswatch.com), choisissez un thème, puis cliquez sur **télécharger**. (Cette opération télécharge *Bootstrap.min.CSS* par défaut ; si vous souhaitez examiner le code CSS, obtenez *bootstrap.css* au lieu de la version minifiée.)
2. Copiez le contenu du fichier CSS téléchargé.
3. Dans Visual Studio, créez un nouveau **feuille de Style** fichier nommé *bootstrap-Theme.CSS* dans le *contenu* dossier et collez le code CSS téléchargé code dedans.
4. Ouvrez *application\_Start/Bundle.config* et modifiez *bootstrap.css* à *bootstrap-Theme.CSS*.

Exécuter à nouveau le projet, et l’application a un nouveau look. L’illustration suivante montre l’effet du thème Amelia :

![Thème d’amorçage Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Plusieurs thèmes d’amorce sont disponibles, les versions gratuites et premium. Bootstrap offre également une grande variété de composants d’interface utilisateur, tel que [listes déroulantes](http://twitter.github.io/bootstrap/components.html#dropdowns), [bouton groupes](http://twitter.github.io/bootstrap/components.html#buttonGroups), et [icônes](http://twitter.github.io/bootstrap/base-css.html#images). Pour plus d’informations sur les données d’amorçage, consultez [le site d’amorçage](http://twitter.github.io/bootstrap/).

Si vous utilisez le Concepteur Web Forms dans Visual Studio, notez que le concepteur ne prend en charge CSS3, il n’affiche pas correctement tous les effets des thèmes d’amorçage ou changements de disposition dynamique. Toutefois, les pages Web Forms seront affichent correctement lorsqu’ils sont affichés avec un navigateur.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Ajout de prise en charge des infrastructures supplémentaires

Lorsque vous sélectionnez un modèle, la case à cocher pour les ou les frameworks utilisés par le modèle est automatiquement sélectionné. Par exemple, si vous sélectionnez le **Web Forms** modèle, le **Web Forms** case à cocher est activée et vous ne pouvez pas la désactiver.

![Sélectionnez un modèle](creating-web-projects-in-visual-studio/_static/image21.png)

![Ajouter des infrastructures](creating-web-projects-in-visual-studio/_static/image22.png)

Vous pouvez sélectionner la case à cocher pour une infrastructure qui n’est pas incluse dans le modèle afin d’ajouter la prise en charge ce framework lorsque le projet est créé. Par exemple, pour activer l’utilisation des Web Forms *.aspx* pages lorsque vous avez sélectionné le modèle MVC, sélectionnez le **Web Forms** case à cocher. Ou pour permettre à MVC lorsque vous utilisez le modèle Web Forms, cliquez sur le **MVC** case à cocher. Ajout d’un framework permet la prise en charge au moment du design, ainsi que des temps d’exécution. Par exemple, si vous ajoutez la prise en charge MVC à un projet Web Forms, vous serez en mesure de générer automatiquement des contrôleurs et des vues.

Si vous combinez des Web Forms et MVC dans un projet et activez [URL conviviales](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) dans Web Forms, il peut y avoir inattendue de problèmes de routage où une URL a plusieurs cibles possibles. Les itinéraires définis tout d’abord seront prioritaire. Par exemple, si vous avez un `Home` contrôleur et un *Home.aspx* page, le `http://contoso.com/home` URL ira à *Home.aspx* si vous appelez le `EnableFriendlyUrls` méthode avant d’appeler le `MapRoute`méthode dans *RouteConfig.cs*, ou la même URL passera à l’affichage par défaut pour votre `Home` contrôleur si vous appelez `MapRoute` avant `EnableFriendlyUrls`.

Ajout d’un framework n’ajoute pas les fonctionnalités de l’application exemple. Par exemple, si vous ajoutez des Web Forms prend en charge lorsque vous avez sélectionné le modèle MVC, ne *Default.aspx* page d’accueil du fichier est créée. Seuls les dossiers, les fichiers et les références requises pour prendre en charge l’infrastructure sont ajoutés. Pour cette raison, ajout d’infrastructures ne change pas les options d’authentification, qui sont implémentées par du code dans les exemples d’applications créés par les modèles. Par exemple, si vous sélectionnez le modèle vide et ajoutez des Web Forms ou MVC prend en charge, le **configurer l’authentification** bouton restera désactivé.

Les sections suivantes décrivent brièvement l’effet de chaque case à cocher.

### <a name="add-web-forms-support"></a>Ajouter la prise en charge de Web Forms

Crée un vide *application\_données* et *modèles* dossiers et un *Global.asax* fichier. Ceux-ci sont déjà créés par tous les modèles autres que le modèle vide, donc en sélectionnant la case à cocher Web Forms ne fait aucune différence pour les autres modèles.

Le modèle Web Forms active les URL conviviales par défaut, mais lorsque vous ajoutez des que Web Forms prend en charge à d’autres modèles en sélectionnant la case à cocher Web Forms Qu'url conviviales ne sont pas activés automatiquement.

### <a name="add-mvc-support"></a>Ajouter la prise en charge MVC

Installe les packages MVC, Razor et WebPages NuGet, crée vide *application\_données*, *contrôleurs*, *modèles*, et *vues*dossiers, crée *application\_Démarrer* dossier avec *RouteConfig.cs* de fichiers et crée *Global.asax* fichier.

### <a name="add-web-api-support"></a>Ajouter la prise en charge des API Web

Installe les packages WebApi et Newtonsoft.Json NuGet, crée vide *application\_données*, *contrôleurs*, et *modèles* dossiers, crée  *Application\_Démarrer* dossier avec *WebApiConfig.cs* de fichiers et crée *Global.asax* fichier.

<a id="auth"></a>
## <a name="authentication-methods"></a>Méthodes d’authentification

Visual Studio 2013 offre plusieurs options d’authentification pour les modèles Web Forms, MVC et API Web :

- [Aucune authentification](#noauth)
- [Comptes d’utilisateur individuels](#indauth) (ASP.NET Identity, anciennement appelé l’appartenance ASP.NET)
- [Comptes de société](#orgauth) (Windows Server Active Directory ou Azure Active Directory)
- [L’authentification Windows](#winauth) (Intranet)

![Configurer la boîte de dialogue authentification](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Aucune authentification

Si vous sélectionnez **aucune authentification**, l’exemple d’application ne contiendra aucune page web pour la connexion, ne l’interface utilisateur qui indique qui est connecté, aucune entité de classes pour une base de données d’appartenance et aucune chaîne de connexion pour une base de données d’appartenance.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Comptes d’utilisateur individuels

Si vous sélectionnez **comptes d’utilisateur individuels**, l’exemple d’application sera être configuré pour utiliser ASP.NET Identity (anciennement appelé l’appartenance ASP.NET) pour l’authentification utilisateur. ASP.NET Identity permet à un utilisateur à inscrire un compte, en créant un nom d’utilisateur et le mot de passe sur le site ou en vous connectant avec des fournisseurs de réseaux sociaux tels que Facebook, Google, Microsoft Account ou Twitter. Le magasin de données par défaut pour les profils utilisateur dans ASP.NET Identity est une base de données SQL Server LocalDB, vous pouvez déployer vers SQL Server ou de la base de données SQL Azure pour le site de production.

Dans Visual Studio 2013, ces fonctionnalités sont les mêmes que dans Visual Studio 2012, mais le code sous-jacent pour le système d’appartenance ASP.NET a été réécrit. Avantages de la nouvelle base de code sont les suivants :

- Le nouveau système d’appartenance est basé sur [OWIN](http://owin.org/) plutôt que le module d’authentification par formulaire ASP.NET. Cela signifie que vous pouvez utiliser le même mécanisme d’authentification si vous utilisez des Web Forms ou MVC dans IIS, ou vous êtes l’auto-hébergement API Web ou SignalR.
- La nouvelle base de données d’appartenance est gérée par Entity Framework Code First, et toutes les tables sont représentées par des classes d’entité que vous pouvez modifier. Cela signifie que vous pouvez facilement personnaliser le schéma de base de données et les web liés au profil de l’interface utilisateur en fonction de vos propres besoins, et vous pouvez facilement déployer vos mises à jour à l’aide des Migrations Code First.

Le nouveau système d’appartenance est implémenté automatiquement dans les nouveaux modèles, et il peut être implémentées manuellement dans un projet qui cible le .NET 4.5 ou version ultérieure.

ASP.NET Identity est un bon choix si vous créez un site Internet qui est principalement pour les clients externes. Si votre organisation utilise Active Directory ou Office 365 et que vous souhaitez créer un projet qui permet à l’authentification unique pour les employés et partenaires commerciaux, les **comptes professionnels** option peut être un meilleur choix.

Pour plus d’informations sur l’option comptes d’utilisateur individuels, consultez les ressources suivantes :

- [www.ASP.NET/Identity](../../../identity/index.md). Documentation sur l’identité ASP.NET sur le site web ASP.NET.
- [Créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Montre également comment personnaliser les données de profil utilisateur.
- [API Web - service d’authentification externe](../../../web-api/overview/security/external-authentication-services.md)
- [Ajout de connexions externes à votre application ASP.NET dans Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Comptes professionnels

Si vous sélectionnez **comptes professionnels**, l’exemple d’application sera être configuré pour utiliser Windows Identity Foundation (WIF) pour l’authentification basée sur les comptes d’utilisateur dans Azure Active Directory (Azure AD, qui inclut Office 365) ou Windows Server Active Directory. Pour plus d’informations, consultez [options d’authentification de compte de société](#orgauthoptions) plus loin dans cette rubrique.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Authentification Windows

Si vous sélectionnez **l’authentification Windows**, l’exemple d’application sera être configuré pour utiliser le module IIS de l’authentification Windows pour l’authentification. L’application affiche le domaine et l’ID utilisateur d’Active directory ou du compte d’ordinateur local qui est connecté à Windows, mais ne comprennent l’inscription utilisateur ou journal dans l’interface utilisateur. Cette option est destinée aux sites web Intranet.

Vous pouvez également créer un site Intranet qui utilise l’authentification Active Directory en choisissant le [option sous des comptes professionnels sur site](#orgauthonprem). L’option On-Premises utilise Windows Identity Foundation (WIF) au lieu du module d’authentification Windows. Quelques étapes supplémentaires sont nécessaires pour configurer l’option On-Premises, mais WIF Active les fonctionnalités qui ne sont pas disponibles avec le module d’authentification Windows. Par exemple, avec WIF, vous pouvez configurer des accès aux applications dans les données d’annuaire Active Directory et de la requête.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Options d’authentification de compte professionnel

Le **configurer l’authentification** boîte de dialogue vous propose plusieurs options pour l’authentification de compte de Windows Server Active Directory (AD) ou Azure Active Directory (Azure AD, qui inclut Office 365) :

- [Cloud - organisation unique](#orgauthsingle) (Azure AD ou AD à l’aide d’intégration d’annuaire avec Azure AD)
- [Cloud - Multi organisation](#orgauthmulti) (Azure AD ou AD à l’aide d’intégration d’annuaire avec Azure AD)
- [On-Premises](#orgauthonprem) (AD)

Si vous souhaitez essayer une des options d’Azure AD mais que vous n’avez pas encore, un compte [cliquez ici pour vous inscrire à un compte Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Si vous choisissez une des options d’Azure AD, votre projet nécessite une base de données et vous devez vous connecter à un compte d’administrateur général pour votre locataire Azure AD. Entrez le nom et le mot de passe pour un compte professionnel (par exemple, admin@contoso.onmicrosoft.com) qui dispose des autorisations d’administration pour votre client Azure AD.
> 
> **N’entrez des informations d’identification pour un compte Microsoft (par exemple, contoso@hotmail.com) dans la boîte de dialogue de connexion.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - authentification d’organisation unique

![Authentification d’organisation unique](creating-web-projects-in-visual-studio/_static/image24.png)

Choisissez cette option si vous souhaitez activer l’authentification pour les comptes d’utilisateur qui sont définies dans un annuaire Azure AD [locataire](https://technet.microsoft.com/library/jj573650.aspx). Par exemple, le site est contoso.com et elle sera disponible aux employés de la société Contoso qui se trouvent dans le locataire contoso.onmicrosoft.com. Vous ne pourrez pas configurer Azure AD pour permettre aux utilisateurs d’autres locataires d’accéder à l’application.

#### <a name="domain"></a>Domaine

Entrez le domaine Azure AD que vous souhaitez configurer l’application, par exemple : `contoso.onmicrosoft.com`. Si vous avez un [domaine personnalisé](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), tel que `contoso.com` au lieu de `contoso.onmicrosoft.com`, vous pouvez le saisir ici.

#### <a name="access-level"></a>Niveau d’accès

Si l’application doit interroger ou mettre à jour les informations d’annuaire à l’aide de l’API Graph, choisissez **Single Sign-On, les données d’annuaire en lecture** ou **Single Sign-On, lire et écrire des données de répertoire**. Sinon, choisissez **Single Sign-On**. Pour plus d’informations, consultez [niveaux d’accès Application](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) et [à l’aide de l’API Graph pour interroger Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI ID d’application

Par défaut, le modèle crée une URI ID d’application pour vous en ajoutant le nom du projet au domaine Azure AD. Par exemple, si le nom du projet est `Example` et le domaine est `contoso.onmicrosoft.com`, l’application devient URI ID `https://contoso.onmicrosoft.com/Example`. Si vous souhaitez spécifier manuellement l’URI ID d’application, développez le **plus d’Options** section et entrez l’URI ID d’application dans la zone de texte. L’application ID URI doit commencer par `https://`.

Par défaut, si une application qui est déjà approvisionnée dans Azure AD a la même URI ID d’application que celui qui est à l’aide de Visual Studio pour le projet, le projet est connecté à l’application existante au lieu d’approvisionner un nouveau. Si vous souhaitez une nouvelle application à être configurés dans ce cas, désactivez le **remplacer l’entrée de l’application si un avec le même ID existe déjà** case à cocher.

Si le **remplacer** case à cocher est désactivée et Visual Studio recherche une application existante avec la même URI ID d’application, il crée un URI en ajoutant un numéro à l’URI il allait à utiliser. Par exemple, supposons que le nom du projet est `Example`, vous ne renseignez la zone de texte, vous désactivez le **remplacer** case à cocher et le client Azure AD a déjà une application avec l’URI `https://contoso.onmicrosoft.com/Example`. Dans ce cas, une nouvelle application est préparée avec une application que les URI d’ID `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Approvisionnement de l’application dans Azure AD

Pour configurer l’application dans Azure AD ou de connexion du projet à une application existante, Visual Studio nécessite les informations d’identification d’un administrateur général pour le domaine. Lorsque vous cliquez sur **OK** dans le **configurer l’authentification** boîte de dialogue, vous êtes invité au nom d’utilisateur et le mot de passe d’un administrateur général pour le domaine que vous avez spécifié. Ensuite, lorsque vous cliquez sur **créer un projet** dans le **nouveau projet ASP.NET** boîte de dialogue, Visual Studio configure l’application dans Azure AD. Notez que dans le cadre de ce processus Visual Studio incorpore des valeurs secrètes de client dans le fichier Web.config qui expirent un an après sa création.

Pour plus d’informations sur la création d’applications qui utilisent **Cloud - organisation unique** l’authentification, consultez les ressources suivantes :

- [Authentification Azure](../2012/windows-azure-authentication.md)
- [Ajout de l’authentification à votre Application Web à l’aide d’Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Développement d’applications ASP.NET avec Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Sécuriser les API Web ASP.NET avec Azure AD et les composants Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Les didacticiels n’ont pas encore été mis à jour pour Visual Studio 2013 ; les didacticiels vous invitent à effectuer manuellement certaines est automatisé dans Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud - Multi organisation Authentication

![Authentification d’organisation multiple](creating-web-projects-in-visual-studio/_static/image25.png)

Choisissez cette option si vous souhaitez activer l’authentification pour les comptes d’utilisateur qui sont définis dans Azure AD plusieurs [locataires](https://technet.microsoft.com/library/jj573650.aspx). Par exemple, le site est contoso.com, et il sera disponible pour les employés de la société Contoso qui se trouvent dans le locataire contoso.onmicrosoft.com et les employés de la société Fabrikam qui se trouvent dans le locataire de fabrikam.onmicrosoft.com.

Les paramètres que vous entrez et l’application d’étape de configuration sont similaires aux [l’authentification unique organisation](#orgauthsingle).

Pour plus d’informations sur la création d’applications qui utilisent **Cloud - Multi organisation** l’authentification, consultez les ressources suivantes :

- [Intégration facile des applications Web avec Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) sur le blog de l’équipe Active Directory.
- [Développement d’Applications Web d’architecture mutualisée avec Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) didacticiel. Le didacticiel n’a pas encore été mis à jour pour Visual Studio 2013 ; ce que le didacticiel vous indique comment effectuer manuellement certaines est automatisé dans Visual Studio 2013.
- [Vous devez inscrire avant de pouvez vous connecter avec votre propre application ASP.NET de plusieurs organisations](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci qui explique comment résoudre un personnes problème commun rencontrer lors de la création d’un projet qui utilise l’authentification de plusieurs organisation.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Authentification d’organisation en local

![Authentification d’organisation en local](creating-web-projects-in-visual-studio/_static/image26.png)

Choisissez cette option si vous souhaitez activer l’authentification pour les comptes d’utilisateur qui sont définies dans Windows Server Active Directory (AD), et vous ne souhaitez pas utiliser Azure AD. Vous pouvez utiliser cette option pour créer un site Intranet ou un site Internet. Pour un site Internet, utilisez Active Directory Federation Services (ADFS) pour fournir l’accès à Active Directory. Pour plus d’informations, consultez [utiliser l’Option d’authentification d’organisation (ADFS) On-Premises avec ASP.NET dans Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Pour un site Intranet, en guise d’alternative que vous pouvez choisir [l’authentification Windows](#winauth) au lieu de cette option. Pour l’option d’authentification de Windows vous n’êtes pas obligé de fournir une URL de document de métadonnées. Toutefois, l’authentification Windows ne vous donne pas la possibilité pour contrôler l’accès application dans Active Directory ou pour interroger des données de répertoire.

#### <a name="on-premises-authority"></a>Autorité locale

Entrez une URL qui pointe vers le document de métadonnées. Le document de métadonnées contient les coordonnées de l’autorité. L’application utilisera ces coordonnées pour piloter le flux d’authentification web.

#### <a name="application-id-uri"></a>URI ID d’application

Spécifiez un URI unique AD peut utiliser pour identifier cette application, ou laissez vide pour permettre à Visual Studio de créer un.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Étapes suivantes

Ce document propose une aide de base pour la création d’un projet web ASP.NET dans Visual Studio 2013. Pour plus d’informations sur l’utilisation de Visual Studio pour le développement web, consultez [ https://www.asp.net/visual-studio/ ](../../index.md).
