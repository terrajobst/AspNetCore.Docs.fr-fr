---
title: Considérations de conception d’API de SignalR
author: anurse
description: Apprenez à concevoir SignalR APIs pour assurer la compatibilité entre les versions de votre application.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571551"
---
# <a name="signalr-api-design-considerations"></a>Considérations de conception d’API de SignalR

Par [Andrew Stanton-Nurse](https://twitter.com/anurse)

Cet article fournit des conseils pour la création d’API basées sur SignalR.

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>Utiliser les paramètres de l’objet personnalisé pour garantir la compatibilité descendante

Ajout de paramètres à une méthode de concentrateur SignalR (sur le client ou le serveur) est un *modification avec rupture*. Cela signifie que les serveurs/clients plus anciens obtenez des erreurs lorsqu’ils tentent d’appeler la méthode sans le nombre approprié de paramètres. Toutefois, l’ajout de propriétés à un paramètre d’objet personnalisé est **pas** une modification avec rupture. Cela peut être utilisé pour concevoir des API compatibles qui résistent aux modifications sur le client ou le serveur.

Par exemple, considérez une API côté serveur comme suit :

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

Le client JavaScript appelle cette méthode à l’aide `invoke` comme suit :

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

Si vous ajoutez ultérieurement un deuxième paramètre à la méthode de serveur, les clients plus anciens ne fournissent cette valeur de paramètre. Exemple :

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

Lorsque l’ancien client tente d’appeler cette méthode, il obtiendra une erreur ressemblant à ceci :

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

Sur le serveur, vous verrez un message de journal comme suit :

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

L’ancien client envoyé uniquement un seul paramètre, mais le API de serveur plus récent requis deux paramètres. Utilisation d’objets personnalisés en tant que paramètres offre plus de souplesse. Nous allons reconcevoir l’API d’origine pour utiliser un objet personnalisé :

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

À présent, le client utilise un objet pour appeler la méthode :

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

Au lieu d’ajouter un paramètre, ajoutez une propriété à la `TotalLengthRequest` objet :

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

Lorsque l’ancien client envoie un seul paramètre, le fichier extra `Param2` propriété restera `null`. Vous pouvez détecter un message envoyé par un client plus ancien en vérifiant la `Param2` pour `null` et appliquer une valeur par défaut. Un nouveau client peut envoyer les deux paramètres.

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

La même technique fonctionne pour les méthodes définies sur le client. Vous pouvez envoyer un objet personnalisé à partir du serveur :

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

Du côté client, vous accéder à la `Message` propriété plutôt qu’à l’aide d’un paramètre :

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

Si vous décidez d’ajouter l’expéditeur du message à la charge utile, ajoutez une propriété à l’objet :

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

Les clients plus anciens ne sont pas en avoir besoin le `Sender` valeur, donc ils amèneront l’ignorer. Un nouveau client peut l’accepter en mettant à jour pour lire la nouvelle propriété :

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

Dans ce cas, le nouveau client est également à tolérance de panne d’un ancien serveur qui ne fournit pas la `Sender` valeur. Étant donné que l’ancien serveur ne fournit le `Sender` valeur, le client vérifie si elle existe avant d’y accéder.
