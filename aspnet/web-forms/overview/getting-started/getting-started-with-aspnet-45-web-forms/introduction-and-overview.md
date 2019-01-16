---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Prise en main 4.7 Web Forms ASP.NET et Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Cette série de didacticiels pas à pas vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de Microsoft Visual Studio et ASP.NET 4.7
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341550"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2017
====================

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Cette série de didacticiels vous montre comment créer une application Web Forms ASP.NET avec ASP.NET 4.5 et Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Introduction

Cette série de didacticiels vous guide tout au long de la création d’une application Web Forms ASP.NET à l’aide de Visual Studio 2017 et ASP.NET 4.5. Vous allez créer une application nommée **Wingtip Toys** : un site web storefront simplifiée vente d’articles en ligne. Lors de la série, les nouvelles fonctionnalités de ASP.NET 4.5 sont mises en surbrillance.

### <a name="target-audience"></a>Public cible

Les développeurs qui découvrent les Web Forms ASP.NET sont le public cible de cette série de didacticiels.

Vous devez avoir des connaissances dans les domaines suivants :

- Programmation orientée objet (OOP) et les langues
- Développement Web (HTML, CSS et JavaScript)
- bases de données relationnelles
- Architecture multiniveau

Pour passer en revue ces zones, envisagez d’étudier le contenu suivant :

- [Mise en route avec Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Base de données relationnelle](http://en.wikipedia.org/wiki/Relational_database)
- [Architecture à plusieurs niveaux](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Fonctionnalités de l’application

Les fonctionnalités de Web Form ASP.NET présentées dans cette série incluent :

- Le projet d’Application Web (pas le projet de Site Web)
- Web Forms
- Pages maîtres, Configuration
- Programme d’amorçage
- Entity Framework Code First, base de données locale
- Validation de la demande
- Contrôles de données fortement typées
- Liaison de modèle
- Annotations de données
- Fournisseurs de valeurs
- SSL et OAuth
- ASP.NET Identity, Configuration et autorisation
- Validation non obstrusive
- Routage
- Gestion des erreurs ASP.NET

### <a name="application-scenarios-and-tasks"></a>Tâches et des scénarios d’application

Tâches de la série de didacticiels :

- Création, révision et un nouveau projet en cours d’exécution
- Création d’une structure de base de données
- L’initialisation et l’amorçage d’une base de données
- Personnalisation de l’interface utilisateur avec des styles, des graphiques et une page maître
- Ajout de pages et la navigation
- Affichage des détails de menu et des données de produit
- Création d’un panier d’achat
- Prise en charge SSL Ajout et OAuth
- Ajout d’une méthode de paiement
- Y compris un rôle d’administrateur et un utilisateur à l’application
- Restreindre l’accès à des pages spécifiques et de dossier
- Téléchargement d’un fichier à l’application web
- Implémentation de la validation des entrées
- Inscription des itinéraires pour l’application web
- Implémentation de la gestion des erreurs et journalisation des erreurs

## <a name="overview"></a>Vue d'ensemble

Cette série de didacticiels est destinée à une personne connaissant les concepts de programmation, mais aucun nouveau Web Forms ASP.NET. Si vous êtes déjà familiarisé avec ASP.NET Web Forms, cette série peut toujours vous aider à en savoir plus sur les nouvelles fonctionnalités de ASP.NET 4.5. Pour les lecteurs êtes pas familiarisés avec les concepts de programmation et ASP.NET Web Forms, consultez les didacticiels de Web Forms supplémentaires fournies dans le [mise en route](../../../index.md) section sur le site Web ASP.NET.

Le fournies dans cette série de didacticiels ASP.NET 4.5 inclut les fonctionnalités suivantes :

- Une interface utilisateur simple pour créer des projets qui offre [prise en charge de nombreuses infrastructures ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), une disposition, thèmes et framework de conception réactive.
- [ASP.NET Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.NET qui fonctionne de la même dans toutes les infrastructures ASP.NET et fonctionne avec des logiciels autres que IIS d’hébergement web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Une mise à jour vers Entity Framework, ce qui vous permet :
  - Récupérer et manipuler des données en tant qu’objets fortement typés
  - Accéder aux données de façon asynchrone
  - Gérer les défaillances temporaires de connexion
  - Instructions de journalisation SQL

Pour une liste complète des fonctionnalités ASP.NET 4.5, consultez [ASP.NET et Web Tools pour Visual Studio 2013 Release Notes de publication](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>L’exemple d’application Wingtip Toys

Les captures d’écran suivantes proviennent de l’application Web Forms ASP.NET que vous créez dans cette série de didacticiels. Lorsque vous exécutez l’application dans Visual Studio, la page d’accueil web suivante s’affiche.

![Wingtip Toys - page par défaut](introduction-and-overview/_static/image1.png)

Vous pouvez enregistrer en tant qu’un nouvel utilisateur, ou connectez-vous en tant qu’un utilisateur existant. Le volet de navigation supérieur propose des liens vers les catégories de produits et de leurs produits à partir de la base de données.

Si vous sélectionnez **produits**, tous les produits disponibles sont affichés. 

![Wingtip Toys - produits](introduction-and-overview/_static/image2.png)

Si vous sélectionnez un produit spécifique, les détails du produit sont affichés.


![Wingtip Toys - détails du produit](introduction-and-overview/_static/image3.png)

En tant qu’utilisateur, vous pouvez inscrire et se connecter avec des fonctionnalités par défaut de modèle Web Forms. Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant. En outre, vous pouvez vous connecter en tant que l’administrateur pour ajouter et supprimer des produits à partir de la base de données.

![Wingtip Toys - connectez-vous](introduction-and-overview/_static/image4.png)

Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et de validation avec PayPal. L’exemple d’application est conçu pour fonctionner dans un sandbox du développeur de PayPal. Aucune transaction money réelle n’a lieu.

![Wingtip Toys - panier d’achat](introduction-and-overview/_static/image5.png)

PayPal confirme vos informations de compte, de commande et de paiement.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Après le renvoi de PayPal, vous pouvez consulter et terminer votre commande.

![Wingtip Toys - ordre révision](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Prérequis

Avant de commencer, assurez-vous que le logiciel suivant est installé sur votre ordinateur :

- [Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

Le .NET Framework est installé automatiquement.

Cette série de didacticiels utilise Microsoft Visual Studio Community 2017. Vous pouvez utiliser qu’ou Microsoft Visual Studio 2017 pour terminer cette série de didacticiels.

Notez les points suivants concernant Visual Studio :

* Microsoft Visual Studio 2017 et Microsoft Visual Studio Community 2017 sont appelés *Visual Studio* tout au long de cette série de didacticiels.

* Visual Studio 2017 est installé en regard les anciennes versions déjà installées. Les sites créés dans les versions antérieures peuvent être ouverts dans Visual Studio 2017 et continuent à ouvrir dans les versions précédentes.

* La première fois que vous avez démarré Visual Studio, il est supposé que vous avez sélectionné le *développement Web* paramètres. Pour plus d'informations, voir [Procédure : Sélectionnez les paramètres d’environnement de développement de Web](https://msdn.microsoft.com/library/ff521558.aspx).

Après avoir installé les composants requis, vous êtes prêt à commencer à créer le projet Web présenté dans cette série de didacticiels.

## <a name="download-the-sample-application"></a>Télécharger l’exemple d’application

 Vous pouvez télécharger l’exemple terminé applicatiion à tout moment à partir du site d’exemples MSDN :

[Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

 Ce téléchargement comprend les éléments suivants :

- L’exemple d’application dans le *WingtipToys* dossier.
- Les ressources utilisées pour créer l’exemple d’application dans le *WingtipToys-ressources* dossier dans le *WingtipToys* dossier.

Le téléchargement est un *.zip* fichier. Pour voir le projet terminé qui crée cette série de didacticiels, recherchez et sélectionnez le *C#* dossier dans le fichier .zip. Enregistrer le C# vers le dossier vous permet de travailler avec les projets Visual Studio. Par défaut, le dossier de projets Visual Studio 2017 est :

<strong>C:\Users&#92;</strong><strong><em>&lt;nom d’utilisateur&gt;</em></strong><strong>\source\repos</strong>

Renommer le ***c#*** dossier ***WingtipToys***.

> [!NOTE]
> Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de renommer le *c#* dossier *WingtipToys*.

Pour exécuter le projet terminé, ouvrez le *WingtipToys* dossier et double-cliquez sur le *WingtipToys.sln* fichier. Visual Studio 2017 ouvre le projet. Ensuite, cliquez sur le *Default.aspx* fichier **l’Explorateur de solutions** et sélectionnez **afficher dans le navigateur**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Participez à un questionnaire de Web Forms ASP.NET pour passer en revue le contenu

À l’issue de la série de didacticiels, participez à un questionnaire pour tester vos connaissances et de renforcer les concepts clés. Chaque question fournit une explication et les liens vers des informations supplémentaires.

 * [Web Forms ASP.NET questionnaire](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Commentaires et didacticiel prise en charge

Pour les questions et vos commentaires, utilisez la section de questions-réponses incluse sur le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) exemple de page.

Commentaires sur cette série de didacticiels sont les bienvenus. Lorsque la mise à jour de cette série de didacticiels, tout est mis en œuvre à prendre en compte des corrections ou des suggestions d’améliorations.

Si une erreur se produit, les messages d’erreur correspondante peut entraîner une confus, sans aucune bonne explication sur la façon de résoudre le problème. Pour une aide, vous pouvez vérifier le [forums ASP.NET](https://forums.asp.net/). Une autre bonne source est la section de questions-réponses dans le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) exemple de page. 

> [!div class="step-by-step"]
> [Next](create-the-project.md)
