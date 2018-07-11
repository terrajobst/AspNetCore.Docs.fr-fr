---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Ajout d’un modèle | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211052"
---
<a name="adding-a-model"></a>Ajout d’un modèle
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données. Ces classes seront les &quot;modèle&quot; dans le cadre de l’application ASP.NET MVC.

Vous allez utiliser une technologie d’accès aux données .NET Framework appelée le [Entity Framework](https://docs.microsoft.com/ef/) à définir et utiliser ces classes de modèle. Le prend en charge Entity Framework (souvent appelée EF) un paradigme de développement appelé *Code First*. Code tout d’abord vous permet de créer des objets de modèle en écrivant des classes simples. (Ils sont également appelés classes POCO, à partir de &quot;objet CLR traditionnel.&quot;) Vous avez ensuite la base de données créé à la volée à partir de vos classes, ce qui permet un flux de travail de développement très concis et rapide. Si vous devez d’abord créer la base de données, vous pouvez toujours suivre ce didacticiel pour en savoir plus sur le développement d’applications MVC et Entity Framework. Vous pouvez ensuite suivre Tom Fizmakens [génération de modèles automatique ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) didacticiel, qui couvre la première approche de base de données.

## <a name="adding-model-classes"></a>Ajout de Classes de modèle

Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.

![](adding-a-model/_static/image1.png)

Entrez le *classe* nom &quot;film&quot;.

Ajoutez les propriétés suivantes de cinq à la `Movie` classe :

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Nous allons utiliser le `Movie` classe pour représenter des films dans une base de données. Chaque instance d’un `Movie` objet correspond à une ligne d’une table de base de données et chaque propriété de la `Movie` classe est mappé à une colonne dans la table.

Remarque : Pour pouvoir utiliser System.Data.Entity et la classe connexe, vous devez installer le [le NuGet Package Entity Framework](https://www.nuget.org/packages/EntityFramework/). Suivez le lien pour obtenir des instructions.

Dans le même fichier, ajoutez le code suivant `MovieDBContext` classe :

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

Le `MovieDBContext` classe représente le contexte de base de données de film de Entity Framework, qui gère l’extraction, le stockage et la mise à jour `Movie` instances dans une base de données de la classe. Le `MovieDBContext` dérive le `DbContext` fourni par Entity Framework de classe de base.

Afin de pouvoir faire référence à `DbContext` et `DbSet`, vous devez ajouter le code suivant `using` instruction en haut du fichier :

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Vous pouvez le faire en ajoutant manuellement à l’aide de l’instruction, ou vous pouvez survoler les lignes ondulées rouges, cliquez sur `Show potential fixes` et cliquez sur `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Remarque : Plusieurs inutilisé `using` instructions ont été supprimées. Visual Studio affiche les dépendances inutilisées en gris. Vous pouvez supprimer les dépendances inutilisées en pointant sur les dépendances en gris, cliquez sur `Show potential fixes` et cliquez sur **supprimer les instructions Using inutilisées.**

![](adding-a-model/_static/image3.png)

Nous avons enfin ajouté un modèle (M dans MVC). Dans la section suivante, vous allez utiliser avec la chaîne de connexion de base de données.

> [!div class="step-by-step"]
> [Précédent](adding-a-view.md)
> [Suivant](creating-a-connection-string.md)
