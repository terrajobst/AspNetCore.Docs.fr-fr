---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Créer un point de terminaison OData v4 à l’aide d’ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: L’Open Data Protocol (OData) est un protocole d’accès aux données pour le web. OData offre un moyen uniforme pour interroger et manipuler des jeux de données via des opérations CRUD...
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 48c1a78c96cb0ebfa0b053dfef84e76433112650
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795416"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Créer un point de terminaison OData v4 à l’aide d’ASP.NET Web API 2.2
====================
par [Mike Wasson](https://github.com/MikeWasson)

> L’Open Data Protocol (OData) est un protocole d’accès aux données pour le web. OData offre un moyen uniforme pour interroger et manipuler des jeux de données via des opérations CRUD (créer, lire, mettre à jour et supprimer).
>
> API Web ASP.NET prend en charge v3 et v4 du protocole. Vous pouvez même disposer d’un point de terminaison v4 qui s’exécute côte à côte avec un point de terminaison v3.
>
> Ce didacticiel montre comment créer un point de terminaison OData v4 qui prend en charge les opérations CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
> - Web API 2.2
> - OData v4
> - Visual Studio 2013 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versions de didacticiels
>
> Pour la Version 3 d’OData, consultez [création d’un point de terminaison OData v3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Dans Visual Studio, à partir de la **fichier** menu, sélectionnez **New** &gt; **projet**.

Développez **installé** &gt; **modèles** &gt; **Visual C#** &gt; **Web**, puis sélectionnez le  **Application Web ASP.NET** modèle. Nommez le projet &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

Dans le **nouveau projet** boîte de dialogue, sélectionnez le **vide** modèle. Sous &quot;ajouter des dossiers et les références principales... &quot;, cliquez sur **API Web**. Cliquez sur **OK**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Installer les Packages d’OData

Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** &gt; **Console du gestionnaire de package**. Dans la fenêtre de Console du Gestionnaire de Package, tapez :

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Cette commande installe les derniers packages NuGet d’OData.

## <a name="add-a-model-class"></a>Ajouter une classe de modèle

Un *modèle* est un objet qui représente une entité de données dans votre application.

Dans l’Explorateur de solutions, cliquez sur le dossier de modèles. Dans le menu contextuel, sélectionnez **ajouter** &gt; **classe**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Par convention, les classes de modèle sont placés dans le dossier Modèles, mais vous n’êtes pas obligé de suivre cette convention dans vos propres projets.


Nommez la classe `Product`. Dans le fichier Product.cs, remplacez le code réutilisable avec les éléments suivants :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

Le `Id` propriété est la clé d’entité. Les clients peuvent interroger des entités par clé. Par exemple, pour obtenir le produit avec l’ID de 5, l’URI est `/Products(5)`. Le `Id` propriété également sera la clé primaire dans la base de données back-end.

## <a name="enable-entity-framework"></a>Activer Entity Framework

Pour ce didacticiel, nous allons utiliser Code First de Entity Framework (EF) pour créer la base de données back-end.

> [!NOTE]
> API Web OData ne nécessite pas d’EF. Utilisez n’importe quelle couche d’accès aux données qui peut traduire des entités de base de données dans des modèles.


Tout d’abord, installez le package NuGet pour Entity Framework. Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** &gt; **Console du gestionnaire de package**. Dans la fenêtre de Console du Gestionnaire de Package, tapez :

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Ouvrez le fichier Web.config et ajoutez la section suivante à l’intérieur de la **configuration** élément, après le **configSections** élément.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Ce paramètre ajoute une chaîne de connexion pour une base de données de base de données locale. Cette base de données sera utilisée lorsque vous exécutez l’application localement.

Ensuite, ajoutez une classe nommée `ProductsContext` dans le dossier Modèles :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Dans le constructeur, `"name=ProductsContext"` donne le nom de la chaîne de connexion.

## <a name="configure-the-odata-endpoint"></a>Configurer le point de terminaison OData

Ouvrez le fichier App\_Start/WebApiConfig.cs. Ajoutez le code suivant **à l’aide de** instructions :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Puis ajoutez le code suivant à la **inscrire** méthode :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Ce code effectue deux opérations :

- Crée un Entity Data Model (EDM).
- Ajoute un itinéraire.

Un modèle EDM est un modèle abstrait des données. Le modèle EDM est utilisé pour créer le document de métadonnées de service. Le **ODataConventionModelBuilder** classe crée un modèle EDM à l’aide des conventions d’affectation de noms par défaut. Cette approche nécessite le moins de code. Si vous souhaitez davantage de contrôle sur le modèle EDM, vous pouvez utiliser la **ODataModelBuilder** classe pour créer le modèle EDM en ajoutant explicitement les propriétés, les clés et les propriétés de navigation.

Un *itinéraire* indique les API Web comment router les requêtes HTTP vers le point de terminaison. Pour créer un itinéraire d’OData v4, appelez le **MapODataServiceRoute** méthode d’extension.

Si votre application comporte plusieurs points de terminaison OData, créez un itinéraire distinct pour chacune. Un nom d’itinéraire unique et un préfixe, donnez à chaque itinéraire.

## <a name="add-the-odata-controller"></a>Ajouter le contrôleur OData

Un *contrôleur* est une classe qui gère les requêtes HTTP. Vous créez un contrôleur séparé pour chaque jeu d’entités dans votre service OData. Dans ce didacticiel, vous allez créer un seul contrôleur, pour le `Product` entité.

Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs, puis sélectionnez **ajouter** &gt; **classe**. Nommez la classe `ProductsController`.

> [!NOTE]
> La version de ce didacticiel pour OData v3 utilise le **ajouter un contrôleur** génération de modèles automatique. Actuellement, il n’existe aucune génération de modèles automatique pour OData v4.


Remplacez le code réutilisable dans ProductsController.cs avec les éléments suivants.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Le contrôleur emploie le `ProductsContext` classe pour accéder à la base de données à l’aide d’Entity Framework. Notez que le contrôleur substitue le **Dispose** méthode pour supprimer le **ProductsContext**.

Il s’agit de point de départ pour le contrôleur. Ensuite, nous allons ajouter des méthodes pour toutes les opérations CRUD.

## <a name="querying-the-entity-set"></a>Interroger le jeu d’entités

Ajoutez les méthodes suivantes pour `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La version sans paramètre de la `Get` méthode retourne l’ensemble de produits. Le `Get` méthode avec un *clé* paramètre recherche un produit par sa clé (dans ce cas, le `Id` propriété).

Le **[EnableQuery]** attribut permet aux clients modifier la requête, à l’aide des options de requête tels que $filter, $sort et $page. Pour plus d’informations, consultez [prenant en charge des Options de requête OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Ajout d’une entité au jeu d’entités

Pour permettre aux clients d’ajouter un nouveau produit à la base de données, ajoutez la méthode suivante à `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Mise à jour d'une entité

OData prend en charge les deux une sémantique différente pour la mise à jour une entité, PATCH et PUT.

- CORRECTIF effectue une mise à jour partielle. Le client spécifie uniquement les propriétés à mettre à jour.
- PUT remplace l’intégralité de l’entité.

L’inconvénient de PUT est que le client doit envoyer des valeurs pour toutes les propriétés de l’entité, y compris les valeurs qui ne changent pas. Le [spécification OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indique que le correctif est préféré.

Dans tous les cas, voici le code pour les méthodes PATCH et PUT :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Dans le cas de correctif, le contrôleur utilise le **Delta&lt;T&gt;**  type pour le suivi des modifications.

## <a name="deleting-an-entity"></a>Suppression d'une entité

Pour activer les clients supprimer un produit à partir de la base de données, ajoutez la méthode suivante à `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
