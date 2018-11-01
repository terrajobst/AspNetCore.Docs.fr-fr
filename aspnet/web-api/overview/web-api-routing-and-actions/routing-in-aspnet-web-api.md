---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routage dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207301"
---
<a name="routing-in-aspnet-web-api"></a>Routage dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit comment les API Web ASP.NET achemine les requêtes HTTP aux contrôleurs.

> [!NOTE]
> Si vous êtes familiarisé avec ASP.NET MVC, le routage d’API Web est très similaire pour le routage MVC. La principale différence est que les API Web utilise le verbe HTTP, pas le chemin d’accès URI, pour sélectionner l’action. Vous pouvez également utiliser le routage de style MVC dans l’API Web. Cet article ne suppose pas aucune connaissance d’ASP.NET MVC.

## <a name="routing-tables"></a>Tables de routage

Dans l’API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes HTTP. Les méthodes publiques du contrôleur sont appelées *méthodes d’action* ou simplement *actions*. Lorsque l’infrastructure API Web reçoit une demande, il achemine la demande à une action.

Pour déterminer l’action à appeler, le framework utilise un *table de routage*. Le modèle de projet Visual Studio pour les API Web crée un itinéraire par défaut :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Cet itinéraire est défini dans le *WebApiConfig.cs* fichier, qui est placé dans le *application\_Démarrer* directory :

![](routing-in-aspnet-web-api/_static/image1.png)

Pour plus d’informations sur la `WebApiConfig` de classe, consultez [configuration ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Si vous hébergez des API Web, vous devez définir la table de routage directement sur le `HttpSelfHostConfiguration` objet. Pour plus d’informations, consultez [auto-héberger une API Web](../older-versions/self-host-a-web-api.md).

Chaque entrée dans la table de routage contient un *modèle d’itinéraire*. Le modèle d’itinéraire par défaut pour l’API Web est &quot;api / {controller} / {id}&quot;. Dans ce modèle, &quot;api&quot; est un segment de chemin d’accès littéral et {controller} et {id} sont des variables d’espace réservé.

Lorsque l’infrastructure API Web reçoit une requête HTTP, il tente de faire correspondre l’URI par rapport à un des modèles d’itinéraire dans la table de routage. Si aucun itinéraire ne correspond, le client reçoit une erreur 404. Par exemple, les URI suivants correspondent à l’itinéraire par défaut :

- / api/contacts
- /API/contacts/1
- /API/Products/gizmo1

Toutefois, l’URI suivant ne correspond pas, car il manque le &quot;api&quot; segment :

- / contacts/1

> [!NOTE]
> La raison pour l’utilisation de « api » dans l’itinéraire consiste à éviter les collisions avec routage ASP.NET MVC. De cette façon, vous pouvez avoir &quot;/contacte&quot; accédez à un contrôleur MVC, et &quot;/api/contacts&quot; accédez à un contrôleur d’API Web. Bien sûr, si vous n’aimez pas cette convention, vous pouvez modifier la table d’itinéraires par défaut.

Une fois qu’un itinéraire correspondant est trouvé, API Web sélectionne le contrôleur et l’action :

- Pour trouver le contrôleur, API Web ajoute &quot;contrôleur&quot; à la valeur de la *{controller}* variable.
- Pour rechercher l’action, API Web examine le verbe HTTP et recherche d’une action dont le nom commence par ce nom du verbe HTTP. Par exemple, avec une demande GET, API Web se présente pour le préfixe une action &quot;obtenir&quot;, tel que &quot;GetContact&quot; ou &quot;GetAllContacts&quot;. Cette convention s’applique uniquement pour GET, POST, PUT, DELETE, HEAD, OPTIONS et verbes des correctifs. Vous pouvez activer les autres verbes HTTP en utilisant des attributs sur votre contrôleur. Nous verrons un exemple plus tard.
- Autres variables d’espace réservé dans le modèle d’itinéraire, tel que *{id},* sont mappées aux paramètres d’action.

Examinons un exemple. Supposons que vous définissez le contrôleur suivant :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Voici certaines demandes HTTP possibles, ainsi que l’action qui est appelé pour chaque :

| Verbe HTTP | Chemin d’accès de l’URI | Action | Paramètre |
| --- | --- | --- | --- |
| GET | API/produits | GetAllProducts | *(aucun)* |
| GET | produits/API/4 | GetProductById | 4 |
| SUPPR | produits/API/4 | DeleteProduct | 4 |
| PUBLIER | API/produits | *(aucune correspondance trouvée)* |  |

Notez que le *{id}* segment de l’URI, le cas échéant, est mappé à la *id* paramètre de l’action. Dans cet exemple, le contrôleur définit deux méthodes GET, une avec un *id* paramètre et l’autre sans paramètres.

Notez également que la requête POST échouera, car le contrôleur ne définit pas un &quot;Post... &quot; (méthode).

## <a name="routing-variations"></a>Variantes de routage

La section précédente décrit le mécanisme de routage de base pour l’API Web ASP.NET. Cette section décrit certaines variantes.

### <a name="http-verbs"></a>Verbes HTTP

Au lieu d’utiliser la convention d’affectation de noms pour les verbes HTTP, vous pouvez spécifier explicitement le verbe HTTP pour une action en décorant la méthode d’action avec l’un des attributs suivants :

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Dans l’exemple suivant, la `FindProduct` méthode est mappée aux demandes GET :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Pour autoriser plusieurs verbes HTTP pour une action, ou pour autoriser les verbes HTTP autres que GET, PUT, POST, DELETE, HEAD, OPTIONS et PATCH, utilisez le `[AcceptVerbs]` attribut, qui prend une liste des verbes HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routage par nom d’Action

Avec le modèle de routage par défaut, les API Web utilise le verbe HTTP pour sélectionner l’action. Toutefois, vous pouvez également créer un itinéraire où le nom d’action est inclus dans l’URI :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

Dans ce modèle d’itinéraire, le *{action}* noms de paramètres de la méthode d’action sur le contrôleur. Avec ce style de routage, utilisez les attributs pour spécifier les verbes HTTP autorisés. Par exemple, supposons que votre contrôleur est la méthode suivante :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

Dans ce cas, une demande GET pour « api/produits/détails/1 » serait mappé à la `Details` (méthode). Ce style de routage est similaire à ASP.NET MVC et peut être approprié pour une API de style RPC.

Vous pouvez remplacer le nom d’action en utilisant le `[ActionName]` attribut. Dans l’exemple suivant, il existe deux actions qui mappent aux &quot;produits/api/miniature/*id*. Prend en charge GET et l’autre prend en charge POST :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Non-Actions

Pour éviter une méthode appelée en tant qu’action, utilisez la `[NonAction]` attribut. Cela signale à l’infrastructure que la méthode n’est pas une action, même si elle correspondrait sinon les règles de routage.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>informations supplémentaires

Cette rubrique fourni une vue d’ensemble du routage. Pour plus d’informations, consultez [routage et sélection d’Action](routing-and-action-selection.md), qui décrit exactement comment le framework correspond à un URI à un itinéraire, sélectionne un contrôleur, puis sélectionne l’action à appeler.
