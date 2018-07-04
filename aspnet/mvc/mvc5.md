---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 est une infrastructure pour générer des applications web évolutive et basée sur des normes à l’aide de modèles de conception bien établis et la puissance de AS....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: f3a81d249f186a2bdea9c25a3e178e13fd2bc830
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403030"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Quelles sont les nouveautés dans ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Les modèles de projet Web MVC intègrent en toute transparence à la nouvelle expérience de One ASP.NET. Vous pouvez personnaliser votre projet MVC et configurer l’authentification à l’aide de l’Assistant de création de projet One ASP.NET. Vous trouverez un didacticiel d’introduction à ASP.NET MVC 5 dans [mise en route avec ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Pour plus d’informations sur la mise à niveau des projets MVC 4 vers MVC 5, consultez [comment mettre à jour un ASP.NET MVC 4 et le projet d’API Web ASP.NET MVC 5 et API Web 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Les modèles de projet MVC ont été mis à jour pour utiliser ASP.NET Identity pour l’authentification et de gestion des identités. Vous trouverez un didacticiel présentant l’authentification Facebook et Google et la nouvelle API d’appartenance dans [créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et OpenID Sign-on](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) et [déployer une application ASP.NET MVC sécurisée avec L’appartenance, OAuth et base de données SQL à un Site Web Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Programme d’amorçage

Le modèle de projet MVC a été mis à jour pour utiliser [Bootstrap](http://getbootstrap.com/) pour fournir une élégante et réactive apparence que vous pouvez facilement personnaliser. Pour plus d’informations, consultez [Bootstrap dans les modèles de projet web Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtres d’authentification

[Filtres d’authentification](http://www.dotnetcurry.com/showarticle.aspx?ID=957) sont un nouveau type de filtre dans ASP.NET MVC qui s’exécutent avant les filtres d’autorisation dans le pipeline ASP.NET MVC et vous permettent de spécifier l’authentification logique d’une action, par contrôleur, ou dans le monde entier pour tous les contrôleurs. Filtres d’authentification traitement les informations d’identification dans la demande et fournissent un principal correspondant. Filtres d’authentification peuvent également ajouter des demandes d’authentification en réponse aux demandes non autorisées. Consultez [filtres d’authentification ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtres d’authentification dans ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Remplacements de filtre

Vous pouvez maintenant remplacer les filtres à appliquer à une méthode d’action donné ou un contrôleur en spécifiant un [remplacer filtre](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Filtres de remplacement de spécifier un ensemble de types de filtre ne doit pas être exécuté pour une étendue donnée (action ou contrôleur). Cela vous permet de vous permettent de configurer des filtres qui s’appliquent globalement mais puis exclure certains filtres globaux de l’application à des actions spécifiques ou des contrôleurs. Consultez [nouveaux remplacements de filtre de fonctionnalité dans ASP.NET MVC 5 et API Web ASP.NET 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [comment utiliser la fonctionnalité de remplacements de filtre ASP.NET MVC 5](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), et [remplacements de filtre dans ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Routage par attributs

ASP.NET MVC prend désormais en charge [routage par attributs](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), grâce à une contribution par Tim McCall, l’auteur de [ http://attributerouting.net ](http://attributerouting.net). Avec le routage par attributs, vous pouvez spécifier vos itinéraires en annotant vos actions et les contrôleurs.

## <a name="new-web-project-experience"></a>Nouvelle expérience de projet Web

Nous avons amélioré l’expérience de création de projets web dans Visual Studio 2013. Dans le **nouveau projet Web ASP.NET** dialogue, vous pouvez sélectionner le type de projet, vous souhaitez configurez n’importe quelle combinaison de technologies (Web Forms, MVC, Web API), configurez les options d’authentification et ajoutez un projet de test unitaire.

![Nouveau projet ASP.NET](mvc5/_static/image1.png)

La nouvelle boîte de dialogue vous permet de modifier les options d’authentification par défaut pour la plupart des modèles. Par exemple, lorsque vous créez un projet Web Forms ASP.NET vous pouvez sélectionner une des options suivantes :

- Aucune authentification
- Comptes d’utilisateur individuels (l’appartenance ASP.NET ou des journaux de fournisseur de réseau social dans)
- Comptes d’organisation (Active Directory dans une application internet)
- Authentification Windows (Active Directory dans une application intranet)

![Options d’authentification](mvc5/_static/image2.png)

Pour plus d’informations sur le nouveau processus pour la création de projets web, consultez [Creating ASP.NET Web Projects in Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Pour plus d’informations sur les options d’authentification, consultez [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

Génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET. Il est facile d’ajouter du code réutilisable à votre projet qui interagit avec un modèle de données.

Dans les versions précédentes de Visual Studio, génération de modèles automatique a été limitée pour les projets ASP.NET MVC. Avec Visual Studio 2013, vous pouvez maintenant utiliser la génération de modèles automatique pour un projet ASP.NET, y compris les Web Forms. Visual Studio 2013 ne prend pas en charge les pages de génération pour un projet Web Forms, mais vous pouvez toujours utiliser la structure avec les Web Forms en ajoutant des dépendances MVC au projet. Prise en charge pour la génération de pages pour Web Forms sera ajoutée dans une prochaine mise à jour.

Lorsque vous utilisez la génération de modèles automatique, nous nous assurer que tous les requis dépendances sont installées dans le projet. Par exemple, si vous commencez avec un projet Web Forms ASP.NET et ensuite utilisez la structure pour ajouter un contrôleur d’API Web, références et les packages NuGet requis sont ajoutés à votre projet automatiquement.

Pour ajouter la structure MVC à un projet Web Forms, ajoutez un **nouvel élément structuré** et sélectionnez **des dépendances MVC 5** dans la boîte de dialogue. Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète. Si vous sélectionnez Minimal, uniquement les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option complet, les dépendances minimales sont ajoutés, ainsi que les fichiers de contenu requis pour un projet MVC.

Prise en charge de la génération de modèles automatique des contrôleurs d’async utilise les nouvelles fonctionnalités asynchrones à partir d’Entity Framework 6.

Pour plus d’informations et des didacticiels, consultez [vue d’ensemble de la génération de modèles automatique ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Obtention d’aide et de signaler des problèmes

- [Problèmes connus et la liste de modifications avec rupture](../visual-studio/overview/2013/release-notes.md#knownissues)
- Obtenir de l’aide et de discuter d’ASP.NET MVC 5 dans le [forums](https://forums.asp.net/1146.aspx)
- [Signaler un bogue dans ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Effectuer une demande de fonctionnalité](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>La mise à niveau à partir d’ASP.NET MVC 4

Consultez [comment mettre à niveau un ASP.NET MVC 4 et Web de projet d’API ASP.NET MVC 5 et API Web 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
