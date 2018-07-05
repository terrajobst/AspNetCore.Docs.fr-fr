---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Vue d’ensemble d’ASP.NET MVC | Microsoft Docs
author: microsoft
description: En savoir plus sur les différences entre l’application ASP.NET MVC et les applications Web Forms ASP.NET. Découvrez comment déterminer quand créer une application ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 2a83f77815c4e55ac2b9bac30dc5019b329baeac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399816"
---
<a name="aspnet-mvc-overview"></a>Vue d’ensemble d’ASP.NET MVC
====================
par [Microsoft](https://github.com/microsoft)

> En savoir plus sur les différences entre l’application ASP.NET MVC et les applications Web Forms ASP.NET. Découvrez comment déterminer quand créer une application ASP.NET MVC.


Le modèle d’architecture Model-View-Controller (MVC) sépare une application en trois composants principaux : le modèle, la vue et le contrôleur. L’infrastructure ASP.NET MVC fournit une alternative au modèle Web Forms ASP.NET pour la création d’applications Web basées sur MVC. L’infrastructure ASP.NET MVC est une infrastructure de présentation simple et facilement testable, qui (comme les applications basées sur des Web Forms) est intégré avec des fonctionnalités ASP.NET existantes, telles que les pages maîtres et l’authentification basée sur l’appartenance. L’infrastructure MVC est définie dans le **System.Web.Mvc** espace de noms, est une partie essentielle et pris en charge de la **System.Web** espace de noms.   
  
MVC est un modèle de conception standard que de nombreux développeurs connaissent. Certains types d’applications Web bénéficieront de l’infrastructure MVC. D’autres continueront à utiliser le modèle d’application ASP.NET traditionnel qui est basé sur des Web Forms et les publications (postback). Autres types d’applications Web combine les deux approches ; aucune de ces approches exclut l’autre.   
  
L’infrastructure MVC inclut les composants suivants :


[![Appel d’une action de contrôleur qui attend une valeur de paramètre](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figure 01**: appel d’une action de contrôleur qui attend une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-overview/_static/image2.png))


- **Modèles**. Objets de modèle sont les parties de l’application qui implémentent la logique du domaine d’application s données. Souvent, les objets de modèle récupèrent et stockent l’état de modèle dans une base de données. Par exemple, un objet Product peut récupérer des informations à partir d’une base de données, exploiter, puis réécrire les informations mises à jour à une table Products dans SQL Server.

Dans les petites applications, le modèle est souvent une séparation conceptuelle plutôt que physique. Par exemple, si l’application lit un jeu de données uniquement et il envoie à la vue, l’application n’a pas une couche de modèle physique et des classes associées. Dans ce cas, le jeu de données joue le rôle d’un objet de modèle.

- **Vues**. Les vues sont les composants qui affichent l’interface utilisateur s (IU). En règle générale, cette interface utilisateur est créée à partir des données de modèle. Un exemple serait une vue d’édition d’une table Products affichant des zones de texte, des listes déroulantes et des cases à cocher en fonction de l’état actuel d’un objet de produits.

- **Contrôleurs**. Les contrôleurs sont les composants qui gèrent l’interaction utilisateur, utiliser le modèle et finalement sélectionnent une vue qui affiche l’interface utilisateur à afficher. Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond. Par exemple, le contrôleur gère les valeurs de chaîne de requête et transmet ces valeurs au modèle, qui à son tour interroge la base de données en utilisant les valeurs.

Le modèle MVC vous permet de créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), tout en assurant un couplage lâche entre ces éléments. Le modèle spécifie l’emplacement de chaque type de logique dans l’application. La logique de l’interface utilisateur appartient à la vue. La logique d’entrée appartient au contrôleur. La logique métier appartient au modèle. Cette séparation vous permet de gérer la complexité lorsque vous générez une application, car elle vous permet de se concentrer sur l’un des aspects de l’implémentation à la fois. Par exemple, vous pouvez vous concentrer sur la vue sans dépendre de la logique métier.   
  
Outre la gestion de la complexité, le modèle MVC rend plus facile de tester les applications qu’il est de tester une application Web ASP.NET basée sur des Web Forms. Par exemple, dans une application Web ASP.NET basée sur des Web Forms, une classe unique est utilisée pour afficher la sortie et répondre aux entrées utilisateur. Écriture de tests automatisés pour les applications ASP.NET basées sur des Web Forms peut être complexe, car pour tester une page individuelle, vous devez instancier la classe de page, tous ses contrôles enfants et les autres classes dépendantes dans l’application. Étant donné que c’est le cas de nombreuses classes sont instanciées pour exécuter la page, il peut être difficile d’écrire des tests centrés exclusivement sur des parties individuelles de l’application. Tests pour les applications ASP.NET basées sur des Web Forms peuvent donc être plus difficiles à implémenter que les tests dans une application MVC. En outre, les tests dans une application ASP.NET basée sur des Web Forms requièrent un serveur Web. L’infrastructure MVC dissocie les composants et une utilisation intensive des interfaces, ce qui rend possible de tester des composants individuels en les isolant du reste de l’infrastructure.   
  
Le couplage lâche entre les trois principaux composants d’une application MVC favorise également le développement en parallèle. Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur et un troisième peut se concentrer sur la logique métier dans le modèle.

## <a name="deciding-when-to-create-an-mvc-application"></a>Quand faut-il créer une Application MVC

Vous devez déterminer avec soin s’il faut implémenter une application Web à l’aide de l’infrastructure ASP.NET MVC ou le modèle Web Forms ASP.NET. L’infrastructure MVC ne remplace pas le modèle Web Forms ; Vous pouvez utiliser un framework pour les applications Web. (Si vous avez des applications Web Forms existantes, ils continuent de fonctionner exactement comme ils ont toujours.)   
  
Avant de décider d’utiliser l’infrastructure MVC ou le modèle Web Forms pour un site Web spécifique, évaluez les avantages de chaque approche.

### <a name="advantages-of-an-mvc-based-web-application"></a>Avantages d’une Application Web basée sur MVC

L’infrastructure ASP.NET MVC offre les avantages suivants :

- Il est plus facile à gérer la complexité en décomposant une application dans le modèle, la vue et le contrôleur.
- Il n’utilise pas l’état d’affichage ou de formulaires basés sur le serveur. L’infrastructure MVC est donc idéal pour les développeurs qui veulent un contrôle total sur le comportement d’une application.
- Il utilise un modèle de contrôleur frontal qui traite les demandes d’application Web via un seul contrôleur. Cela vous permet de concevoir une application qui prend en charge une riche infrastructure de routage. Pour plus d’informations, consultez [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") sur le site Web MSDN.
- Il fournit une meilleure prise en charge pour le développement piloté par tests (TDD).
- Il fonctionne bien pour les applications Web qui sont pris en charge par de grandes équipes de développeurs et concepteurs Web qui ont besoin d’un degré élevé de contrôle sur le comportement de l’application.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Avantages d’une Application Web basée sur les formulaires de Web

L’infrastructure basée sur des Web Forms offre les avantages suivants :

- Il prend en charge un modèle d’événement qui conserve l’état via HTTP, ce qui améliore le développement d’applications Web line-of-business. L’application basée sur des Web Forms fournit des dizaines d’événements qui sont pris en charge dans des centaines de contrôles serveur.
- Elle utilise un modèle de contrôleur de Page qui ajoute des fonctionnalités à des pages individuelles. Pour plus d’informations, consultez [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") sur le site Web MSDN.
- Elle utilise l’état d’affichage ou les formulaires de serveur, qui peuvent faciliter la gestion des informations d’état.
- Il fonctionne bien pour les petites équipes de développeurs et concepteurs qui souhaitent tirer parti du grand nombre de composants disponibles pour le développement rapide d’applications Web.
- En général, il est moins complexe pour le développement d’applications, car les composants (la **Page** classe, les contrôles, etc.) sont étroitement intégrés et requièrent habituellement moins de code que le modèle MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Fonctionnalités de l’infrastructure ASP.NET MVC

L’infrastructure ASP.NET MVC fournit les fonctionnalités suivantes :

- Séparation des tâches de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), testabilité et développement piloté par tests (TDD) par défaut. Tous les contrats de base de l’infrastructure MVC sont basés sur une interface et peuvent être testés à l’aide d’objets fictifs, qui sont des objets simulés qui simulent le comportement d’objets réels de l’application. Vous pouvez les effectuer l’application sans avoir à exécuter les contrôleurs dans un processus ASP.NET, ce qui rend les tests unitaires rapide et flexible. Vous pouvez utiliser toute infrastructure de test unitaire qui est compatible avec le .NET Framework.
- Une infrastructure extensible et enfichable. Les composants de l’infrastructure ASP.NET MVC sont conçus afin que peut être facilement remplacées ou personnalisés. Vous pouvez incorporer dans votre propre moteur d’affichage, de stratégie de routage d’URL, de sérialisation de paramètre de méthode d’action et d’autres composants. L’infrastructure ASP.NET MVC prend également en charge l’utilisation des modèles de conteneur d’Injection de dépendance (DI) et Inversion de contrôle (IOC). L’injection de dépendance vous permet d’injecter des objets dans une classe, au lieu d’utiliser la classe pour créer l’objet lui-même. Inversion de contrôle spécifie que si un objet requiert un autre objet, le premier objet doit obtenir le deuxième objet à partir d’une source externe, tel qu’un fichier de configuration. Cela facilite les tests.
- Un composant de mappage d’URL puissant qui vous permet de créer des applications ayant des URLs compréhensibles et découvrables. URL est inutile d’inclure des extensions de nom de fichier et sont conçues pour prendre en charge les modèles d’affectation de noms d’URL qui fonctionnent bien pour la recherche du moteur d’optimisation (SEO) et transfert d’état representational (REST) d’adressage.
- Prise en charge à l’aide de la balise dans la page ASP.NET existante (fichiers .aspx), le contrôle utilisateur (fichiers .ascx) et fichiers de balisage (fichiers .master) page maître en tant que modèles de vue. Vous pouvez utiliser les fonctionnalités ASP.NET existantes avec l’infrastructure ASP.NET MVC, telles que les pages maîtres imbriquées, les expressions en ligne (&lt;% = %&gt;), les contrôles serveur déclaratifs, les modèles, liaison de données, localisation et ainsi de suite.
- Prise en charge pour les fonctionnalités ASP.NET existantes. ASP.NET MVC vous permet d’utiliser des fonctionnalités telles que l’authentification par formulaire et l’authentification Windows, l’autorisation d’URL, l’appartenance et rôles, sortie et la mise en cache des données, gestion d’état de session et de profil, l’analyse du fonctionnement, le système de configuration et le fournisseur architecture.
