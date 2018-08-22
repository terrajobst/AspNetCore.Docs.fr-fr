---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Cette série de didacticiels pas à pas vous présente les principes de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: ad5e97cd596e146f742c4c5e882d3938005070d1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830142"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels pas à pas vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. [Web Forms ASP.NET questionnaire](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Testez vos connaissances et approfondir les concepts clés en prenant le questionnaire d’ASP.NET Web Forms. Ce questionnaire a été spécifiquement conçu de contenu de cette série de didacticiels. Chaque question du test fournit une explication ainsi que des liens vers des informations supplémentaires.


## <a name="introduction"></a>Introduction

Cette série de didacticiels vous guide à travers les étapes requises pour créer une application Web Forms ASP.NET à l’aide de Visual Studio Express 2013 pour le Web et ASP.NET 4.5.

L’application que vous allez créer est nommée **Wingtip Toys**. Il est un exemple simplifié d’un site web avant de magasin qui vend des éléments en ligne. Cette série de didacticiels met en évidence les nouvelles fonctionnalités disponibles dans ASP.NET 4.5.

Commentaires sont les bienvenus, et nous allons s’efforcer de mettre à jour cette série de didacticiels en fonction de vos suggestions.

### <a name="download-completed-project"></a>Projet de téléchargement terminé

Vous pouvez télécharger un projet c# qui contient la fin du didacticiel.

- [Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Passez en revue le contenu en prenant le questionnaire de Web Forms ASP.NET connexe

Après avoir terminé ce didacticiel, testez vos connaissances et approfondir les concepts clés en prenant le [ASP.NET Web Forms questionnaire](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Ce questionnaire a été spécifiquement conçu de contenu de cette série de didacticiels. Chaque question du test fournit une explication ainsi que des liens vers des informations supplémentaires.

- [Web Forms ASP.NET questionnaire](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Public

Est de cette série de didacticiels destiné aux développeurs expérimentés qui débutent avec ASP.NET Web Forms. Un développeur intéressé à cette série de didacticiels doit avoir les compétences suivantes :

- Familiarisés avec un objet orientée langage de programmation (OOP)
- Maîtriser les concepts de développement Web (HTML, CSS et JavaScript)
- Maîtriser les concepts de base de données relationnelle
- Maîtriser les concepts d’architecture multiniveau

Si vous souhaitez passer en revue les éléments mentionnés ci-dessus, examinez le contenu suivant :

- [Mise en route avec Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Développement Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
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
- Contrôles de données fortement typés liaison, les Annotations de données, de modèle et que la valeur des fournisseurs
- SSL et OAuth
- ASP.NET Identity, Configuration et autorisation
- Validation non obstrusive
- Routage
- Gestion des erreurs ASP.NET

### <a name="application-scenarios-and-tasks"></a>Tâches et des scénarios d’application

Tâches décrites dans cette série sont les suivantes :

- Création, la révision et le nouveau projet en cours d’exécution
- Création de la structure de base de données
- L’initialisation et l’amorçage de la base de données
- Personnalisation de l’interface utilisateur à l’aide de styles, de graphiques et d’une page maître
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

Si vous débutez avec ASP.NET Web Forms, mais êtes familiarisé avec les concepts de programmation, vous avez le didacticiel de droite. Si vous êtes déjà familiarisé avec ASP.NET Web Forms, vous pouvez bénéficier de cette série de didacticiels par les nouvelles fonctionnalités disponibles dans ASP.NET 4.5. Si vous n’êtes pas familiarisé avec les concepts de programmation et ASP.NET Web Forms, consultez les didacticiels supplémentaires fournies dans les formulaires Web [mise en route](../../../index.md) section sur le site Web ASP.NET.

Spécifique au **dernière** ASP.NET 4.5 fonctionnalités fournies dans les formulaires Web cette série de didacticiels inclure les éléments suivants :

- Une interface utilisateur simple pour la création de projets de cette offre [prennent en charge plusieurs infrastructures ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), une infrastructure de mise en page et des thèmes qui fournit des fonctionnalités de création et de thèmes réactives.
- [ASP.NET Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.NET qui fonctionne de la même dans toutes les infrastructures ASP.NET et fonctionne avec des logiciels autres que IIS d’hébergement web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), une mise à jour Entity Framework qui vous permet de récupérez et manipuler des données de manière fortement typée d’objets, accéder aux données de façon asynchrone, gérer les défaillances temporaires de connexion et consigner les instructions SQL.

Pour obtenir une liste complète des fonctionnalités de ASP.NET 4.5, consultez [ASP.NET et Web Tools pour Visual Studio 2013 Release Notes de publication](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>L’exemple d’Application Wingtip Toys

Les captures d’écran suivantes fournissent un aperçu rapide de l’application Web forms ASP.NET que vous allez créer dans cette série de didacticiels. Lorsque vous exécutez l’application à partir de Visual Studio Express 2013 pour le Web, vous verrez la page d’accueil web suivantes.

![Wingtip Toys - page par défaut](introduction-and-overview/_static/image1.png)

Vous pouvez enregistrer en tant qu’un nouvel utilisateur, ou connectez-vous en tant qu’un utilisateur existant. Navigation est fournie en haut de chaque catégorie de produits en récupérant les produits disponibles à partir de la base de données.

En sélectionnant le lien de produits, vous serez en mesure de voir une liste de tous les produits disponibles.

![Wingtip Toys - produits](introduction-and-overview/_static/image2.png)

Vous pouvez également voir les détails du produit individuels en sélectionnant un des produits répertoriés.

![Wingtip Toys - détails du produit](introduction-and-overview/_static/image3.png)

En tant qu’utilisateur, vous pouvez inscrire et connectez-vous à l’aide de la fonctionnalité par défaut du modèle Web Forms. Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant. En outre, vous pouvez vous connecter en tant que l’administrateur pour ajouter et supprimer des produits à partir de la base de données.

![Wingtip Toys - connectez-vous](introduction-and-overview/_static/image4.png)

Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et de validation avec PayPal. Notez que cet exemple d’application est conçue pour fonctionner avec sandbox du développeur de PayPal. Aucune transaction argent n’aura lieu.

![Wingtip Toys - panier d’achat](introduction-and-overview/_static/image5.png)

PayPal confirmera votre compte, l’ordre et les informations de paiement.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Après le renvoi de PayPal, vous pouvez consulter et terminer votre commande.

![Wingtip Toys - ordre révision](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Prérequis

Avant de commencer, assurez-vous que vous avez installé sur votre ordinateur les logiciels suivants :

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Le .NET Framework est installé automatiquement.

Cette série de didacticiels utilise Microsoft Visual Studio Express 2013 pour le Web. Vous pouvez utiliser soit Microsoft Visual Studio Express 2013 pour le Web ou Microsoft Visual Studio 2013 pour terminer cette série de didacticiels.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web seront souvent être appelé Visual Studio tout au long de cette série de didacticiels.


Si vous avez déjà installée une version de Visual Studio, le processus d’installation installe Visual Studio 2013 ou Microsoft Visual Studio Express 2013 pour le Web en regard de la version existante. Les sites que vous avez créé dans les versions antérieures peuvent être ouverts dans Visual Studio 2013 et continuent à ouvrir dans les versions précédentes.

> [!NOTE] 
> 
> Cette procédure pas à pas suppose que vous avez choisi le *développement Web* collection de paramètres de la première fois que vous avez démarré Visual Studio. Pour plus d’informations, consultez [Comment : sélectionner des paramètres d’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Télécharger l’exemple d’Application

Après avoir installé les composants requis, vous êtes prêt à commencer à créer le projet Web est présenté dans cette série de didacticiels. Si vous souhaitez **éventuellement** exécuter l’exemple d’application qui crée cette série de didacticiels, vous pouvez le télécharger à partir du site d’exemples MSDN. Ce téléchargement contient les éléments suivants :

- L’exemple d’application dans le *WingtipToys* dossier.
- Les ressources utilisées pour créer l’exemple d’application dans le *WingtipToys-ressources* dossier dans le *WingtipToys* dossier.

#### <a name="download-the-file-from-msdn-samples-site"></a>Téléchargez le fichier à partir du site d’exemples de MSDN :

[Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

Le téléchargement est un <em>.zip</em> fichier. Pour voir le projet terminé qui crée cette série de didacticiels, recherchez et sélectionnez le <em>c#</em>dossier dans le <em>.zip</em> fichier. Enregistrer le <em>c#</em> dossier dans le dossier que vous utilisez pour travailler avec des projets Visual Studio 2013. Par défaut, le dossier de projets Visual Studio 2013 est le suivant :

<strong>C:\Users&#92;</strong><strong><em>&lt;nom d’utilisateur&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

Renommer le ***c#*** dossier ***WingtipToys***.

> [!NOTE]
> Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de renommer le *c#* dossier *WingtipToys*.


Pour exécuter le projet terminé, ouvrez le *WingtipToys* dossier et double-cliquez sur le *WingtipToys.sln* fichier. Visual Studio 2013 s’ouvre le projet. Ensuite, cliquez sur le *Default.aspx* fichier dans la fenêtre Explorateur de solutions et cliquez sur Afficher dans le navigateur dans le menu contextuel.

### <a name="tutorial-support-and-comments"></a>Commentaires et didacticiel prise en charge

Utilisez la section de questions-réponses incluse avec le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) exemple (c#) pour vos questions ou commentaires.

Commentaires sur cette série de didacticiels sont les bienvenus, et lorsque la mise à jour de cette série de didacticiels chaque effort sera fait tenir des corrections de compte ou des suggestions d’améliorations qui sont fournies dans les commentaires de didacticiel.

Quand une erreur se produit pendant le développement, ou si le site Web ne s’exécute pas correctement, les messages d’erreur peuvent donner des indices complexes à la source du problème ou ne peuvent pas expliquer comment la corriger. Pour vous aider avec quelques scénarios courants de problème, vous pouvez également utiliser le [forums ASP.NET](https://forums.asp.net/) ou la section de questions-réponses inclus avec le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) exemple. Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez les didacticiels, veillez à vérifier les emplacements ci-dessus.

> [!div class="step-by-step"]
> [Next](create-the-project.md)
