---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Actions et fonctions dans OData v4 à l’aide d’ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Dans OData, les fonctions et les actions sont un moyen pour ajouter des comportements côté serveur qui ne sont pas facilement définies en tant qu’opérations CRUD sur les entités. Ce didacticiel montre comment...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 4e61c6b4bf59792b6570e32e6d24635d4f5e5ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837403"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Actions et fonctions dans OData v4 à l’aide d’ASP.NET Web API 2.2
====================
par [Mike Wasson](https://github.com/MikeWasson)

> Dans OData, les fonctions et les actions sont un moyen pour ajouter des comportements côté serveur qui ne sont pas facilement définies en tant qu’opérations CRUD sur les entités. Ce didacticiel montre comment ajouter des fonctions et les actions à un point de terminaison de v4 OData, à l’aide de Web API 2.2. Le didacticiel s’appuie sur le didacticiel [créer une Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versions de didacticiels
> 
> Pour OData Version 3, consultez [Actions OData dans ASP.NET Web API 2](../odata-v3/odata-actions.md).


La différence entre *actions* et *fonctions* est que les actions peuvent avoir des effets secondaires, et n’est pas le cas des fonctions. Les actions et les fonctions peuvent retourner des données. Certaines utilisations pour les actions sont les suivantes :

- Transactions complexes.
- Manipulation de plusieurs entités à la fois.
- Ce qui permet des mises à jour uniquement certaines propriétés d’une entité.
- Envoi de données qui n’est pas une entité.

Les fonctions sont utiles pour renvoyer des informations qui ne correspondant pas directement à une entité ou une collection.

Une action (ou une fonction) peut cibler une seule entité ou une collection. Dans la terminologie d’OData, il s’agit du *liaison*. Vous pouvez également avoir &quot;indépendant&quot; actions/fonctions, qui sont appelées en tant qu’opérations statiques sur le service.

## <a name="example-adding-an-action"></a>Exemple : Ajout d’une Action

Nous allons définir une action pour évaluer un produit.

> [!NOTE]
> Ce didacticiel s’appuie sur le didacticiel [créer une Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md)


Tout d’abord, ajoutez un `ProductRating` modèle pour représenter les évaluations.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Ajoutez également un **DbSet** à la `ProductsContext` classe, afin qu’EF crée une table de contrôle d’accès dans la base de données.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Ajoutez l’Action à l’EDM

Dans WebApiConfig.cs, ajoutez le code suivant :

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Le **EntityTypeConfiguration.Action** méthode ajoute une action à l’entity data model (EDM). Le **paramètre** méthode spécifie un paramètre typé pour l’action.

Ce code définit également l’espace de noms pour le modèle EDM. L’espace de noms est important, car l’URI pour l’action inclut le nom d’action complet :

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> Dans une configuration IIS par défaut, le point dans cette URL entraîne IIS retourner l’erreur 404. Vous pouvez résoudre ce problème en ajoutant la section suivante à votre fichier Web.Config :

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Ajouter une méthode de contrôleur pour l’Action

Pour activer la &quot;taux&quot; action, ajoutez la méthode suivante à `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Notez que le nom de méthode correspond au nom de l’action. Le **[HttpPost]** attribut spécifie la méthode est une méthode HTTP POST.

Pour appeler l’action, le client envoie une requête HTTP POST comme suit :

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Le &quot;taux&quot; action est liée aux instances de produit, par conséquent, l’URI de l’action est le nom d’action complet ajouté à l’URI de l’entité. (Souvenez-vous que nous définissons l’espace de noms EDM &quot;ProductService&quot;, de sorte que le nom d’action complet est &quot;ProductService.Rate&quot;.)

Le corps de la demande contient les paramètres d’action comme une charge utile JSON. API Web convertit automatiquement la charge utile JSON pour un **ODataActionParameters** objet, qui est simplement un dictionnaire de valeurs de paramètre. Utilisez ce dictionnaire pour accéder aux paramètres dans votre méthode de contrôleur.

Si le client envoie les paramètres d’action au bon format, la valeur de **ModelState.IsValid** a la valeur false. Cochez cette case dans votre méthode de contrôleur et renvoie une erreur si **IsValid** a la valeur false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Exemple : Ajout d’une fonction

Maintenant nous allons ajouter une fonction OData qui retourne le produit le plus cher. Comme avant, la première étape est Ajout de la fonction à l’EDM. Dans WebApiConfig.cs, ajoutez le code suivant.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

Dans ce cas, la fonction est liée à la collection de produits, plutôt que des instances individuelles de produit. Les clients appeler la fonction en envoyant une demande GET :

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Voici la méthode de contrôleur pour cette fonction :

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Notez que le nom de méthode correspond au nom de fonction. Le **[HttpGet]** attribut spécifie la méthode est une méthode HTTP GET.

Voici la réponse HTTP :

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Exemple : Ajout d’une fonction non liée

L’exemple précédent a été une fonction liée à une collection. Dans l’exemple suivant, nous allons créer un *indépendant* (fonction). Fonctions indépendantes sont appelées en tant qu’opérations statiques sur le service. La fonction dans cet exemple retourne la taxe pour un code postal donné.

Dans le fichier WebApiConfig, ajoutez la fonction à l’EDM :

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Notez que nous appelons **fonction** directement sur le **ODataModelBuilder**, plutôt qu’au type d’entité ou de la collection. Le Générateur de modèles vous indique que la fonction est indépendante.

Voici la méthode de contrôleur qui implémente la fonction :

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Peu importe quel contrôleur API Web vous placer cette méthode dans. Vous pouvez le placer `ProductsController`, ou définir un contrôleur séparé. Le **[ODataRoute]** attribut définit le modèle d’URI pour la fonction.

Voici un exemple de demande client :

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

La réponse HTTP :

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
