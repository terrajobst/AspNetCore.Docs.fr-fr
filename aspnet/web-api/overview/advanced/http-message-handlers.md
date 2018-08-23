---
uid: web-api/overview/advanced/http-message-handlers
title: Gestionnaires de messages HTTP dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 0b0d7b4c543dc4e597c6c472083898f3a8095a83
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838052"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Gestionnaires de messages HTTP dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et renvoie une réponse HTTP. Gestionnaires de messages de dérivent de la classe abstraite **HttpMessageHandler** classe.

En règle générale, une série de gestionnaires de messages sont chaînées ensemble. Le premier gestionnaire reçoit une requête HTTP effectue un traitement et donne la demande au gestionnaire suivant. À un moment donné, la réponse est créée et remonte la chaîne. Ce modèle est appelé un *délégation* gestionnaire.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Gestionnaires de messages côté serveur

Sur le côté serveur, le pipeline API Web utilise des gestionnaires de messages intégrés :

- **Ftpserver** Obtient la demande à partir de l’hôte.
- **HttpRoutingDispatcher** distribue la demande en fonction de l’itinéraire.
- **HttpControllerDispatcher** envoie la demande à un contrôleur d’API Web.

Vous pouvez ajouter des gestionnaires personnalisés au pipeline. Gestionnaires de messages conviennent pour les problèmes transversaux qui opèrent au niveau de HTTP messages (plutôt que les actions de contrôleur). Par exemple, un gestionnaire de messages peut :

- Lire ou modifier les en-têtes de demande.
- Ajouter un en-tête de réponse aux réponses.
- Valider les demandes avant qu’ils atteignent le contrôleur.

Ce diagramme illustre deux gestionnaires personnalisés insérés dans le pipeline :

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Du côté client, HttpClient utilise également des gestionnaires de messages. Pour plus d’informations, consultez [gestionnaires de messages HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Gestionnaires de messages personnalisés

Pour écrire un gestionnaire de messages personnalisés, dérivez de **System.Net.Http.DelegatingHandler** et remplacer le **SendAsync** (méthode). Cette méthode a la signature suivante :

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

La méthode accepte un **HttpRequestMessage** comme entrée et retourne de façon asynchrone un **HttpResponseMessage**. Une implémentation classique effectue les opérations suivantes :

1. Traiter le message de demande.
2. Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.
3. Le gestionnaire interne retourne un message de réponse. (Cette étape est asynchrone).
4. Traiter la réponse et le renvoyer à l’appelant.

Voici un exemple simple :

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> L’appel à `base.SendAsync` est asynchrone. Si le gestionnaire effectue tout travail après cet appel, utilisez le **await** mot clé, comme indiqué.


Un gestionnaire de délégation peut également ignorer le gestionnaire interne et créer directement la réponse :

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Si une délégation de gestionnaire crée la réponse sans appeler `base.SendAsync`, la requête ignore le reste du pipeline. Cela peut être utile pour un gestionnaire qui valide la demande (création d’une réponse d’erreur).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Ajout d’un gestionnaire au Pipeline

Pour ajouter un gestionnaire de messages côté serveur, ajoutez le gestionnaire à la **HttpConfiguration.MessageHandlers** collection. Si vous avez utilisé le modèle « Application Web ASP.NET MVC 4 » pour créer le projet, vous pouvez effectuer ceci dans le **WebApiConfig** classe :

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Gestionnaires de messages sont appelés dans l’ordre dans lequel ils apparaissent dans **MessageHandlers** collection. Car ils sont imbriqués, le message de réponse sont transmises dans l’autre direction. Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.

Notez que vous n’avez pas besoin de définir des gestionnaires internes ; l’infrastructure API Web connecte automatiquement les gestionnaires de messages.

Si vous êtes [auto-hébergement](../older-versions/self-host-a-web-api.md), créez une instance de la **HttpSelfHostConfiguration** classe et ajoutez les gestionnaires pour le **MessageHandlers** collection.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Maintenant nous allons examiner quelques exemples de gestionnaires de messages personnalisés.

## <a name="example-x-http-method-override"></a>Exemple : X-HTTP-Method-Override

X-HTTP-Method-Override est un en-tête HTTP non standard. Il est conçu pour les clients qui ne peuvent pas envoyer de certains types de demande HTTP, telles que PUT ou DELETE. Au lieu de cela, le client envoie une demande POST et définit l’en-tête X-HTTP-Method-Override à la méthode de votre choix. Exemple :

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Voici un gestionnaire de messages qui ajoute la prise en charge de X-HTTP-Method-Override :

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

Dans le **SendAsync** (méthode), le gestionnaire vérifie si le message de demande est une demande POST, et s’il contient l’en-tête X-HTTP-Method-Override. Dans ce cas, il valide la valeur d’en-tête, puis modifie la méthode de demande. Enfin, le gestionnaire appelle `base.SendAsync` pour transférer le message au gestionnaire suivant.

Lorsque la demande atteint le **HttpControllerDispatcher** (classe), **HttpControllerDispatcher** achemine la demande selon la méthode de demande de mise à jour.

## <a name="example-adding-a-custom-response-header"></a>Exemple : Ajout d’un en-tête de réponse personnalisée

Voici un gestionnaire de messages qui ajoute un en-tête personnalisé à chaque message de réponse :

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Tout d’abord, le gestionnaire appelle `base.SendAsync` de transmettre la demande au Gestionnaire de messages interne. Le gestionnaire interne retourne un message de réponse, mais il le fait à l’aide de manière asynchrone un **tâche&lt;T&gt;**  objet. Le message de réponse n’est pas disponible tant que `base.SendAsync` se termine de façon asynchrone.

Cet exemple utilise le **await** mot clé pour effectuer le travail asynchrone après `SendAsync` se termine. Si vous ciblez .NET Framework 4.0, utilisez le **tâche**&lt;T&gt;**. ContinueWith** méthode :

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Exemple : La vérification pour obtenir une clé API

Certains services web nécessitent des clients à inclure une clé API dans leur demande. L’exemple suivant montre comment un gestionnaire de messages peut vérifier les demandes pour une clé API valide :

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Ce gestionnaire recherche la clé API dans la chaîne de requête URI. (Pour cet exemple, nous partons du principe que la clé est une chaîne statique. Une implémentation réelle utiliseriez sans doute une validation plus complexe.) Si la chaîne de requête contient la clé, le gestionnaire transmet la demande au gestionnaire interne.

Si la demande n’a pas d’une clé valide, le gestionnaire crée un message de réponse avec état 403, interdit. Dans ce cas, le gestionnaire n’appelle pas `base.SendAsync`, par conséquent, le gestionnaire interne ne reçoit jamais de la demande, de même le contrôleur. Par conséquent, le contrôleur peut supposer que toutes les demandes entrantes ont une clé API valide.

> [!NOTE]
> Si la clé d’API s’applique uniquement à certaines actions de contrôleur, envisagez d’utiliser un filtre d’action au lieu d’un gestionnaire de messages. Filtres d’action exécutent après l’exécution de routage de l’URI.


## <a name="per-route-message-handlers"></a>Gestionnaires de messages d’itinéraire

Gestionnaires dans le **HttpConfiguration.MessageHandlers** collection s’appliquent globalement.

Ou bien, vous pouvez ajouter un gestionnaire de messages à un itinéraire spécifique lorsque vous définissez l’itinéraire :

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

Dans cet exemple, si l’URI de demande correspond à « Route2 », la demande est distribuée à `MessageHandler2`. Le diagramme suivant illustre le pipeline pour ces deux itinéraires :

![](http-message-handlers/_static/image4.png)

Notez que `MessageHandler2` remplace la valeur par défaut **HttpControllerDispatcher**. Dans cet exemple, `MessageHandler2` crée la réponse, et les requêtes correspondant à « Route2 » accédez jamais à un contrôleur. Cela vous permet de remplacer l’ensemble du mécanisme contrôleur API Web avec votre propre point de terminaison personnalisé.

Vous pouvez également un gestionnaire de messages par route peut déléguer au **HttpControllerDispatcher**, qui ensuite envoie à un contrôleur.

![](http-message-handlers/_static/image5.png)

Le code suivant montre comment configurer cet itinéraire :

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
