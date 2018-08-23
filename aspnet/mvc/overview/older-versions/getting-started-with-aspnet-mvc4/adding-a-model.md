---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Ajout d’un modèle | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827826"
---
<a name="adding-a-model"></a>Ajout d’un modèle
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.


Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données. Ces classes seront les &quot;modèle&quot; dans le cadre de l’application ASP.NET MVC.

Vous allez utiliser une technologie d’accès aux données .NET Framework appelée le [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) à définir et utiliser ces classes de modèle. Le prend en charge Entity Framework (souvent appelée EF) un paradigme de développement appelé *Code First*. Code tout d’abord vous permet de créer des objets de modèle en écrivant des classes simples. (Ils sont également appelés classes POCO, à partir de &quot;objet CLR traditionnel.&quot;) Vous avez ensuite la base de données créé à la volée à partir de vos classes, ce qui permet un flux de travail de développement très concis et rapide.

## <a name="adding-model-classes"></a>Ajout de Classes de modèle

Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le *modèles* dossier, sélectionnez **ajouter**, puis sélectionnez **classe**.

![](adding-a-model/_static/image1.png)

Entrez le *classe* nom &quot;film&quot;.

Ajoutez les propriétés suivantes de cinq à la `Movie` classe :

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Nous allons utiliser le `Movie` classe pour représenter des films dans une base de données. Chaque instance d’un `Movie` objet correspond à une ligne d’une table de base de données et chaque propriété de la `Movie` classe est mappé à une colonne dans la table.

Dans le même fichier, ajoutez le code suivant `MovieDBContext` classe :

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Le `MovieDBContext` classe représente le contexte de base de données de film de Entity Framework, qui gère l’extraction, le stockage et la mise à jour `Movie` instances dans une base de données de la classe. Le `MovieDBContext` dérive le `DbContext` fourni par Entity Framework de classe de base.

Afin de pouvoir faire référence à `DbContext` et `DbSet`, vous devez ajouter le code suivant `using` instruction en haut du fichier :

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

L’ensemble *Movie.cs* fichier est présenté ci-dessous. (Plusieurs instructions using qui ne sont pas nécessaires ont été supprimé.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Création d’une chaîne de connexion et l’utilisation de SQL Server LocalDB

Le `MovieDBContext` classe que vous avez créé gère la tâche de connexion à la base de données et de mappage `Movie` objets aux enregistrements de base de données. Une question que vous vous demandez peut-être, cependant, consiste à spécifier quelle base de données, il se connecte à. Vous le ferez en ajoutant des informations de connexion dans le *Web.config* fichier de l’application.

Ouvrez la racine de l’application *Web.config* fichier. (Pas le *Web.config* de fichiers dans le *vues* dossier.) Ouvrez le *Web.config* fichier surlignée en rouge.

![](adding-a-model/_static/image2.png)

Ajoutez la chaîne de connexion suivante à la `<connectionStrings>` élément dans le *Web.config* fichier.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

L’exemple suivant montre une partie de la *Web.config* fichier avec la nouvelle chaîne de connexion ajoutée :

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Cette petite quantité de code et le XML est tout ce que vous devez écrire pour représenter et de stocker les données de films dans une base de données.

Ensuite, vous allez générer une nouvelle `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer de nouvelles listes de film.

> [!div class="step-by-step"]
> [Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)
