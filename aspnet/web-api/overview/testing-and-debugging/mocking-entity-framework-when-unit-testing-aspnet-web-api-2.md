---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulation d’Entity Framework lors de l’API Web ASP.NET 2 de tests unitaires | Microsoft Docs
author: Rick-Anderson
description: Ce guide et l’application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise Entity Framework. Il montre comment modifier le...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f1799b3f9d698053c397e57da3f33ff900ec4013
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021025"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simulation d’Entity Framework lors de l’API Web ASP.NET 2 de tests unitaires
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Ce guide et l’application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise Entity Framework. Il montre comment modifier le contrôleur généré automatiquement pour activer la transmission d’un objet de contexte pour le test et comment créer des objets de test qui fonctionnent avec Entity Framework.
>
> Pour une introduction aux tests unitaires avec l’API Web ASP.NET, consultez [Unit Testing avec ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
>
> Ce didacticiel suppose que vous êtes familiarisé avec les concepts de base de l’API Web ASP.NET. Pour un didacticiel d’introduction, consultez [mise en route avec ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2

## <a name="in-this-topic"></a>Dans cette rubrique

Cette rubrique contient les sections suivantes :

- [Composants requis](#prereqs)
- [Télécharger le code](#download)
- [Créer des applications avec le projet de test unitaire](#appwithunittest)
- [Créer la classe de modèle](#modelclass)
- [Ajouter le contrôleur](#controller)
- [Ajouter l’injection de dépendances](#dependency)
- [Installer les packages NuGet dans le projet de test](#testpackages)
- [Créer le contexte de test](#testcontext)
- [Créer des tests](#tests)
- [Exécuter des tests](#runtests)

Si vous avez déjà effectué les étapes décrites dans [Unit Testing avec ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), vous pouvez passer à la section [ajouter le contrôleur de](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prérequis

Édition Visual de Studio 2017 Community, Professional ou Enterprise

<a id="download"></a>
## <a name="download-code"></a>Télécharger le code

Téléchargez le [projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Le projet téléchargeable inclut le code de test unitaire pour cette rubrique et pour le [Unit Test API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) rubrique.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Créer des applications avec le projet de test unitaire

Vous pouvez créer un projet de test unitaire lors de la création de votre application ou ajouter un projet de test unitaire à une application existante. Ce didacticiel montre la création d’un projet de test unitaire lors de la création de l’application.

Créer une Application Web ASP.NET nommé **StoreApp**.

Dans la fenêtre Nouveau projet ASP.NET, sélectionnez le **vide** modèle et ajouter des dossiers et les références principales pour l’API Web. Sélectionnez le **ajouter des tests unitaires** option. Le projet de test unitaire est automatiquement nommé **StoreApp.Tests**. Vous pouvez conserver ce nom.

![créer le projet de test unitaire](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Après avoir créé l’application, vous constaterez qu’il contient deux projets : **StoreApp** et **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Créer la classe de modèle

Dans votre projet StoreApp, ajoutez un fichier de classe pour le **modèles** dossier nommé **Product.cs**. Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Générez la solution.

<a id="controller"></a>
## <a name="add-the-controller"></a>Ajouter le contrôleur

Cliquez sur le dossier contrôleurs et sélectionnez **ajouter** et **nouvel élément structuré**. Sélectionnez le contrôleur Web API 2 avec actions, à l’aide d’Entity Framework.

![Ajouter un nouveau contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Définissez les valeurs suivantes :

- Nom du contrôleur : **ProductController**
- Classe de modèle : **produit**
- Classe de contexte de données : [Sélectionnez **nouveau contexte de données** bouton qui remplit les valeurs indiqués ci-dessous]

![spécifier le contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Cliquez sur **ajouter** pour créer le contrôleur avec le code généré automatiquement. Le code inclut des méthodes pour créer, récupérer, la mise à jour et suppression d’instances de la classe Product. Le code suivant illustre la méthode pour ajouter un produit. Notez que la méthode retourne une instance de **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult est une des nouvelles fonctionnalités dans Web API 2, et il simplifie le développement de test unitaire.

Dans la section suivante, vous personnaliserez le code généré pour faciliter le passage d’objets de test au contrôleur.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Ajouter l’injection de dépendances

Actuellement, la classe ProductController est codé en dur pour utiliser une instance de la classe StoreAppContext. Vous allez utiliser un modèle appelé l’injection de dépendances pour modifier votre application et de supprimer cette dépendance codée en dur. En divisant cette dépendance, vous pouvez passer dans un objet factice lors du test.

Cliquez sur le **modèles** dossier et ajoutez une nouvelle interface nommée **IStoreAppContext**.

Remplacez le code par celui-ci :

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Ouvrez le fichier StoreAppContext.cs et apportez les modifications suivantes en surbrillance. Les modifications importantes à noter sont :

- StoreAppContext classe implémente l’interface de IStoreAppContext
- MarkAsModified méthode est implémentée.


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Ouvrez le fichier ProductController.cs. Modifier le code existant pour faire correspondre le code en surbrillance. Ces modifications rompre la dépendance sur StoreAppContext et permettent à d’autres classes passer un objet différent pour la classe de contexte. Cette modification vous permettra de transmettre un contexte de test au cours des tests unitaires.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Il existe une modification plus, que vous devez apporter dans ProductController. Dans le **PutProduct** (méthode), remplacez la ligne qui définit l’état d’entité à modifié par un appel à la méthode MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Générez la solution.

Vous êtes maintenant prêt à configurer le projet de test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installer les packages NuGet dans le projet de test

Lorsque vous utilisez le modèle vide pour créer une application, le projet de test unitaire (StoreApp.Tests) n’inclut pas tous les packages NuGet installés. Autres modèles, tels que le modèle API Web, incluent certains packages NuGet dans le projet de test unitaire. Pour ce didacticiel, vous devez inclure le package d’Entity Framework et le package de Microsoft ASP.NET Web API 2 Core au projet de test.

Cliquez sur le projet StoreApp.Tests et sélectionnez **gérer les Packages NuGet**. Vous devez sélectionner le projet StoreApp.Tests pour ajouter les packages à ce projet.

![gérer les packages](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

À partir des packages en ligne, rechercher et installer le package EntityFramework (version 6.0 ou version ultérieure). S’il apparaît que le package EntityFramework est déjà installé, il pouvez que vous avez sélectionné le projet StoreApp plutôt qu’au projet StoreApp.Tests.

![Ajouter Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Rechercher et installer le package Microsoft ASP.NET Web API 2 Core.

![installer le package de core api web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Fermez la fenêtre Gérer les Packages NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Créer le contexte de test

Ajoutez une classe nommée **TestDbSet** au projet de test. Cette classe sert de classe de base pour votre jeu de données de test. Remplacez le code par celui-ci :

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Ajoutez une classe nommée **TestProductDbSet** au projet de test qui contient le code suivant.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Ajoutez une classe nommée **TestStoreAppContext** et remplacez le code existant par le code suivant.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Créer des tests

Par défaut, votre projet de test inclut un fichier de test vide nommé **UnitTest1.cs**. Ce fichier montre les attributs que vous permet de créer des méthodes de test. Pour ce didacticiel, vous pouvez supprimer ce fichier, car vous allez ajouter une nouvelle classe de test.

Ajoutez une classe nommée **TestProductController** au projet de test. Remplacez le code par celui-ci :

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Exécuter les tests

Vous êtes maintenant prêt à exécuter les tests. Tous de la méthode qui sont marqués avec le **TestMethod** attribut sera testé. À partir de la **Test** élément de menu, exécuter les tests.

![exécuter des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Ouvrez le **Explorateur de tests** fenêtre et notez les résultats des tests.

![résultats des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
