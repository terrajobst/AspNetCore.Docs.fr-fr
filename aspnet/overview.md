---
uid: overview
title: Vue d’ensemble d’ASP.NET | Microsoft Docs
author: rick-anderson
description: Introduction à ASP.NET, une infrastructure gratuite permettant de créer des sites Web, les applications web et les API web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: ''
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: de094258984e3ca09e4eadf21169cad9bfc67b22
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362260"
---
# <a name="aspnet-overview"></a>Vue d’ensemble d’ASP.NET

ASP.NET est une infrastructure web gratuite pour créer des sites Web attrayants et des applications web à l’aide de HTML, CSS et JavaScript. Vous pouvez également créer des API Web et utiliser des technologies en temps réel telles que Web Sockets.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) est une alternative à ASP.NET.  Consultez le [expliquant comment choisir entre ASP.NET et ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Prise en main

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), un gratuit IDE pour ASP.NET sur Windows.

## <a name="websites-and-web-applications"></a>Sites et applications web

 ASP.NET propose trois frameworks pour créer des applications web : Web Forms, ASP.NET MVC et ASP.NET Web Pages. Tous les trois frameworks sont stable et mature, et vous pouvez créer des applications web exceptionnelles avec un d’eux. Peu importe quelle infrastructure que vous choisissez, vous obtiendrez tous les avantages et fonctionnalités d’ASP.NET partout.

Chaque framework cible un style de développement différents. Celle que vous choisissez dépend d’une combinaison de vos ressources de programmation (base de connaissances, compétences et expérience de développement), le type d’application que vous créez et vous êtes familiarisé avec l’approche de développement.

Voici une vue d’ensemble de chacun des frameworks et quelques idées pour savoir comment choisir entre eux. Si vous préférez une présentation vidéo, consultez [rendre les sites Web avec ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) et [What ' s Web Tools ?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Si vous êtes habitué | Style de développement | Expertise | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF, .NET | Développement rapide à l’aide d’une riche bibliothèque de contrôles qui encapsulent le balisage HTML | RAD de niveau intermédiaire et avancée |
| MVC       | Ruby on Rails, .NET  | Contrôle total sur le balisage HTML, code et de balise séparé et facile à écrire des tests. Le meilleur choix pour les applications mobiles et de page unique (SPA). | Niveau intermédiaire et avancée |
| Pages web  | Classic ASP, PHP     | Balisage HTML et votre code ensemble dans le même fichier | Nouveau, niveau intermédiaire |

### <a name="web-forms"></a>Web Forms

Avec ASP.NET Web Forms, vous pouvez créer des sites Web dynamiques à l’aide d’un modèle familier de glisser-déplacer, pilotée par événements. Une aire de conception et des centaines de composants et contrôles vous permettent de créer rapidement des sites gérés par l’interface utilisateur sophistiqués et puissants avec accès aux données. 

[En savoir plus sur Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC offre un moyen puissant, basé sur des modèles pour créer des sites Web dynamiques qui permet une séparation claire des préoccupations et qui vous donne un contrôle total sur le balisage pour le développement agile et plus agréable. ASP.NET MVC inclut de nombreuses fonctionnalités qui permettent un développement rapide TDD rapide et convivial pour la création d’applications sophistiquées qui utilisent les dernières normes web. 

[En savoir plus sur MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Pages web ASP.NET

Les Pages Web ASP.NET et la syntaxe Razor fournissent un moyen rapide, abordable et simple de combiner du code serveur avec HTML, pour créer du contenu web dynamique. Se connecter aux bases de données, ajouter une vidéo, lier à des sites de réseau social et incluent la plupart de davantage de fonctionnalités qui vous aident à créer des sites attrayants qui sont conformes aux dernières normes web.

[En savoir plus sur les Pages Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Remarques sur les Web Forms, MVC et Web Pages

Tous les trois frameworks ASP.NET sont basés sur le .NET Framework et partagent des fonctionnalités essentielles de .NET et d’ASP.NET. Par exemple, tous les trois frameworks offrent un modèle de sécurité de connexion basé sur l’appartenance et les trois partagent les mêmes sites qui font partie de la fonctionnalité ASP.NET principale pour la gestion des demandes, la gestion des sessions et ainsi de suite.

En outre, les trois frameworks ne sont pas entièrement indépendantes, et en choisissant une n’exclut pas à l’aide d’un autre. Dans la mesure où les infrastructures peuvent coexister dans la même application web, il n’est pas rare de voir des composants individuels des applications écrites à l’aide de différentes infrastructures. Par exemple, destinée aux clients des parties d’une application peuvent être développées dans MVC pour optimiser le balisage, tandis que l’accès aux données et les parties d’administration sont développées dans Web Forms pour tirer parti des contrôles de données et accès aux données simple.

## <a name="web-apis"></a>API web

API Web ASP.NET est une infrastructure qui facilite la création de services HTTP qui atteignent une large gamme de clients, y compris les navigateurs et appareils mobiles. L'API Web ASP.NET est une plate-forme idéale pour générer des applications RESTful sur le .NET Framework.

[En savoir plus sur l’API Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Technologies en temps réel

ASP.NET SignalR est une nouvelle bibliothèque pour les développeurs ASP.NET qui facilite le développement des fonctionnalités web en temps réel. SignalR permet la communication bidirectionnelle entre serveur et client. Serveurs peuvent pousser le contenu aux clients connectés instantanément dès qu’elles sont disponible. SignalR prend en charge Web Sockets et revient à d’autres techniques compatibles pour les navigateurs plus anciens. SignalR inclut les API pour la gestion des connexions (par exemple, vous connecter et déconnecter les événements), regroupement de connexions et l’autorisation.

[En savoir plus sur SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Sites et applications mobiles 

ASP.NET peut alimenter des applications mobiles natives avec une API Web principale, ainsi que des sites web mobiles à l’aide des infrastructures de conception réactive comme Twitter Bootstrap. Si vous générez une application mobile native, il est facile de créer une API Web JSON pour le handle de l’accès aux données, l’authentification et notifications push pour votre application. Si vous créez un site mobile réactif, vous pouvez utiliser toute infrastructure CSS ou un système de grille open vous préférez, ou sélectionnez un système puissant mobile comme jQuery Mobile ou Sencha et des applications mobiles avec PhoneGap.

[En savoir plus sur le développement d’application et de site mobile](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Applications à page unique 

Application de Page ASP.NET unique (SPA) vous permet de créer des applications qui incluent des interactions côté client significatives à l’aide de HTML 5, 3 de CSS et JavaScript. Visual Studio comprend un modèle pour la création d’applications à page unique à l’aide de knockout.js et API Web ASP.NET. Outre le modèle SPA intégré, créés par la Communauté modèles sont également disponibles au téléchargement.

[En savoir plus sur le développement d’application à page unique](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

WebHooks est un modèle HTTP léger en fournissant un modèle pub/sub simple pour associer ensemble les API Web et des services SaaS. Quand un événement se produit dans un service, une notification est envoyée sous la forme d’une requête HTTP POST aux abonnés inscrits. La demande POST contient des informations sur l’événement qui rend possible pour le récepteur d’agir en conséquence.

WebHooks sont exposés par un grand nombre de services, notamment la Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello et bien plus encore. Par exemple, un WebHook peut indiquer qu’un fichier a changé dans Dropbox, ou une modification de code a été validée dans GitHub, ou un paiement a été lancé dans PayPal ou une carte a été créée dans Trello.

[En savoir plus sur les WebHooks](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
