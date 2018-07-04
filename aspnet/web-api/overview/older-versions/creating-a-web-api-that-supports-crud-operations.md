---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Activation des opérations CRUD dans ASP.NET Web API 1 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel montre comment prendre en charge des opérations CRUD dans un service HTTP à l’aide d’API Web ASP.NET. Versions des logiciels utilisées dans le didacticiel Visual Studio 2012 Web point d’accès...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402926"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Activation des opérations CRUD dans ASP.NET Web API 1
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Ce didacticiel montre comment prendre en charge des opérations CRUD dans un service HTTP à l’aide d’API Web ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - Visual Studio 2012
> - Web API 1 (fonctionne également avec l’API Web 2)


CRUD est l’acronyme &quot;Create, Read, Update et Delete,&quot; qui sont les quatre opérations de base de données. De nombreux services HTTP également les opérations CRUD via REST ou API REST de type de modèle.

Dans ce didacticiel, vous allez générer une API pour gérer une liste de produits de web très simple. Chaque produit contient un nom, le prix et la catégorie (tel que &quot;toys&quot; ou &quot;matériel&quot;), ainsi que d’un ID de produit.

Les produits API exposera suivant des méthodes.

| Action | Méthode HTTP | URI relatif |
| --- | --- | --- |
| Obtenir une liste de tous les produits | GET | / api/produits |
| Obtenir un produit par ID | GET | /API/produits/*id* |
| Obtenir un produit par catégorie | GET | produits/api / ? catégorie =*catégorie* |
| Création d’un produit | PUBLIER | / api/produits |
| Mettre à jour un produit | PUT | /API/produits/*id* |
| Supprimer un produit | SUPPR | /API/produits/*id* |

Notez que certains des URI comprennent l’ID de produit dans le chemin d’accès. Par exemple, pour obtenir le produit dont l’ID est 28, le client envoie une demande GET pour `http://hostname/api/products/28`.

### <a name="resources"></a>Ressources

Les produits API définit l’URI pour deux types de ressources :

| Ressource | URI |
| --- | --- |
| La liste de tous les produits. | / api/produits |
| Un produit individuel. | /API/produits/*id* |

### <a name="methods"></a>Méthodes

Les quatre principales méthodes HTTP (GET, PUT, POST et DELETE) peuvent être mappés à des opérations CRUD comme suit :

- GET récupère la représentation sous forme de la ressource à un URI spécifié. GET ne doit avoir aucun effet secondaire sur le serveur.
- PUT met à jour une ressource à un URI spécifié. PUT peut également servir à créer une nouvelle ressource à un URI spécifié, si le serveur autorise les clients à spécifier de nouveaux URI. Pour ce didacticiel, l’API ne prendra pas en charge la création via PUT.
- POST crée une nouvelle ressource. Le serveur assigne l’URI pour le nouvel objet et retourne cet URI en tant que partie du message de réponse.
- DELETE supprime une ressource à un URI spécifié.

Remarque : La méthode PUT remplace l’entité de l’ensemble du produit. Autrement dit, le client est censé envoyer une représentation complète du produit mis à jour. Si vous souhaitez prendre en charge les mises à jour partielles, la méthode PATCH est préférée. Ce didacticiel n’implémente pas de correctif.

## <a name="create-a-new-web-api-project"></a>Créer un nouveau projet d’API Web

Commencez par exécuter Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page. Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet &quot;ProductStore&quot; et cliquez sur **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **API Web** et cliquez sur **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Ajout d’un modèle

Un *modèle* est un objet qui représente les données dans votre application. Dans l’API Web ASP.NET, vous pouvez utiliser des objets CLR fortement typés en tant que modèles, et ils sont automatiquement sérialisés au format XML ou JSON pour le client.

L’API ProductStore, nos données se composent de produits, nous allons créer une nouvelle classe nommée `Product`.

Si l’Explorateur de solutions n’est pas déjà visible, cliquez sur le **vue** menu et sélectionnez **l’Explorateur de solutions**. Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier. Dans le menu contextuel, sélectionnez **ajouter**, puis sélectionnez **classe**. Nommez la classe &quot;produit&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Ajoutez les propriétés suivantes à la `Product` classe.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Ajout d’un référentiel

Nous avons besoin de stocker une collection de produits. Il est judicieux de séparer la collecte à partir de notre implémentation de service. De cette façon, nous pouvons modifier le magasin de stockage sans réécrire la classe de service. Ce type de conception est appelé le *référentiel* modèle. Commencez par définir une interface générique pour le référentiel.

Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier. Sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le nœud c#. Dans c#, sélectionnez **Code**. Dans la liste des modèles de code, sélectionnez **Interface**. Nom de l’interface &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Ajoutez l’implémentation suivante :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Ajoutez maintenant une autre classe dans le dossier Modèles, nommé &quot;ProductRepository.&quot; Cette classe va implémenter l’interface `IProductRespository`. Ajoutez l’implémentation suivante :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Le référentiel conserve la liste dans la mémoire locale. Il s’agit OK pour obtenir un didacticiel, mais dans une application réelle, vous stockez les données de façon externe, une base de données ou dans le stockage cloud. Le modèle de référentiel facilitera modifier l’implémentation par la suite.

## <a name="adding-a-web-api-controller"></a>Ajout d’un contrôleur d’API Web

Si vous avez travaillé avec ASP.NET MVC, puis vous êtes déjà familiarisé avec les contrôleurs. Dans l’API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes HTTP à partir du client. L’Assistant Nouveau projet créé deux contrôleurs pour vous lors de la création du projet. Pour les consulter, développez le dossier contrôleurs dans l’Explorateur de solutions.

- HomeController est un contrôleur ASP.NET MVC traditionnel. Il est responsable du traitement des pages HTML pour le site et n’est pas directement liée à notre API web.
- ValuesController est un exemple de contrôleur WebAPI.

Continuez et supprimer ValuesController, en double-cliquant sur le fichier dans l’Explorateur de solutions et en sélectionnant **supprimer.** Maintenant, ajoutez un nouveau contrôleur, comme suit :

Dans **l’Explorateur de solutions**, cliquez sur le dossier contrôleurs. Sélectionnez **ajouter** , puis sélectionnez **contrôleur**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

Dans le **ajouter un contrôleur** Assistant, nommez le contrôleur &quot;ProductsController&quot;. Dans le **modèle** liste déroulante, sélectionnez **contrôleur d’API vide**. Cliquez ensuite sur **Ajouter**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Il n’est pas nécessaire de le placer vos contrôleurs dans un dossier nommé contrôleurs. Le nom du dossier n’est pas important ; Il est simplement un moyen pratique d’organiser vos fichiers sources.


Le **ajouter un contrôleur** Assistant va créer un fichier nommé ProductsController.cs dans le dossier contrôleurs. Si ce fichier n’est pas déjà ouvert, double-cliquez sur le fichier pour l’ouvrir. Ajoutez le code suivant **à l’aide de** instruction :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Ajouter un champ qui contient un **IProductRepository** instance.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Appel `new ProductRepository()` dans le contrôleur n’est pas la meilleure conception, parce qu’elle lie le contrôleur à une implémentation particulière de `IProductRepository`. Pour une meilleure approche, consultez [à l’aide du résolveur de dépendance d’API Web](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Obtention d’une ressource

L’API ProductStore expose plusieurs &quot;lire&quot; actions en tant que méthodes HTTP GET. Chaque action correspond à une méthode dans la `ProductsController` classe.

| Action | Méthode HTTP | URI relatif |
| --- | --- | --- |
| Obtenir une liste de tous les produits | GET | / api/produits |
| Obtenir un produit par ID | GET | /API/produits/*id* |
| Obtenir un produit par catégorie | GET | produits/api / ? catégorie =*catégorie* |

Pour obtenir la liste de tous les produits, ajoutez cette méthode à la `ProductsController` classe :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Le nom de la méthode commence par &quot;obtenir&quot;, par convention, il mappe aux demandes GET. En outre, étant donné que la méthode n’a aucun paramètre, il est mappé à un URI qui ne contient-elle pas une *&quot;id&quot;* segment dans le chemin d’accès.

Pour obtenir un produit par ID, ajoutez cette méthode à la `ProductsController` classe :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Nom de la méthode commence également par &quot;obtenir&quot;, mais la méthode a un paramètre nommé *id*. Ce paramètre est mappé à la &quot;id&quot; segment du chemin d’accès de l’URI. L’infrastructure de l’API Web ASP.NET convertit automatiquement l’ID pour le type de données correct (**int**) pour le paramètre.

La méthode GetProduct lève une exception de type **HttpResponseException** si *id* n’est pas valide. Cette exception sera traduite par l’infrastructure en une erreur 404 (introuvable).

Enfin, ajoutez une méthode pour rechercher des produits par catégorie :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Si l’URI de requête a une chaîne de requête, API Web tente de faire correspondre les paramètres de requête aux paramètres de la méthode de contrôleur. Par conséquent, un URI au format « api/products ? catégorie =*catégorie*» est mappé à cette méthode.

## <a name="creating-a-resource"></a>Création d’une ressource

Ensuite, nous allons ajouter une méthode à la `ProductsController` classe pour créer un nouveau produit. Voici une implémentation simple de la méthode :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Noter deux choses concernant cette méthode :

- Le nom de la méthode commence par &quot;Post... &quot;. Pour créer un nouveau produit, le client envoie une requête HTTP POST.
- La méthode prend un paramètre de type de produit. Dans l’API Web, les paramètres avec des types complexes sont désérialisés à partir du corps de demande. Par conséquent, nous pensons que le client envoie une représentation sérialisée d’un objet de produit, au format XML ou JSON.

Cette implémentation fonctionnera, mais il n’est pas tout à fait complet. Dans l’idéal, nous aimerions la réponse HTTP à inclure les éléments suivants :

- **Code de réponse :** par défaut, l’infrastructure API Web définit le code d’état de réponse 200 (OK). Mais, conformément au protocole HTTP/1.1, quand une demande POST entraîne la création d’une ressource, le serveur doit répondre avec l’état 201 (créé).
- **Emplacement :** lorsque le serveur crée une ressource, il doit inclure l’URI de la nouvelle ressource dans l’en-tête Location de la réponse.

API Web ASP.NET rend plus facile manipuler le message de réponse HTTP. Voici l’implémentation améliorée :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Notez que le type de retour de méthode est désormais **HttpResponseMessage**. En retournant un **HttpResponseMessage** au lieu d’un produit, nous pouvons contrôler les détails du message de réponse HTTP, y compris le code d’état et l’en-tête Location.

Le **CreateResponse** méthode crée un **HttpResponseMessage** et automatiquement écrit une représentation sérialisée de l’objet Product dans le corps de fo le message de réponse.

> [!NOTE]
> Cet exemple ne valide pas le `Product`. Pour plus d’informations sur la validation de modèle, consultez [Validation de modèle dans ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>La mise à jour une ressource

La mise à jour d’un produit avec la méthode PUT est simple :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Le nom de la méthode commence par &quot;placer... &quot;, de sorte que l’API Web correspondant à des demandes PUT. La méthode accepte deux paramètres, l’ID de produit et les produits mis à jour. Le *id* paramètre est issu le chemin d’accès de l’URI et la *produit* paramètre est désérialisé à partir du corps de demande. Par défaut, l’infrastructure de l’API Web ASP.NET effectue des types de paramètres simples à partir de l’itinéraire et les types complexes à partir du corps de demande.

## <a name="deleting-a-resource"></a>Suppression d’une ressource

Pour supprimer une ressource, définissez une méthode de « Supprimer... ».

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Si une demande de suppression réussit, elle peut retourner l’état 200 (OK) avec un corps d’entité qui décrit l’état ; état 202 (accepté) si la suppression est toujours en attente ; état ou 204 (aucun contenu) sans corps d’entité. Dans ce cas, le `DeleteProduct` méthode a un `void` type de retour, afin de l’API Web ASP.NET traduit automatiquement cette situation en état code 204 (aucun contenu).
