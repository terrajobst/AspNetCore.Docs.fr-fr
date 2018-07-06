---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Guide de l’API ASP.NET SignalR Hubs - serveur (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Ce document fournit une introduction à la programmation côté serveur de l’API des concentrateurs SignalR ASP.NET pour SignalR version 1.1, avec demonstratin des exemples de code...
ms.author: aspnetcontent
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 15d2f4e9eaff0d47a3bdce489dfc1d19177423f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826411"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>Guide de l’API ASP.NET SignalR Hubs - serveur (SignalR 1.x)
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Ce document fournit une introduction à la programmation côté serveur de l’API des concentrateurs SignalR ASP.NET pour SignalR version 1.1, avec des exemples de code illustrant les options courantes.
> 
> L’API de concentrateurs SignalR vous permet de vous permettent d’effectuer des appels de procédure distante (RPC) à partir d’un serveur aux clients connectés et à partir de clients sur le serveur. Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client. Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur. SignalR s’occupe de tous les éléments client-serveur pour vous.
> 
> SignalR offre également une API de niveau inférieur appelée connexions persistantes. Pour une introduction à SignalR Hubs et connexions persistantes, ou pour obtenir un didacticiel qui montre comment générer une application de SignalR complète, consultez [SignalR - mise en route](index.md).


## <a name="overview"></a>Vue d'ensemble

Ce document contient les sections suivantes :

- [Comment inscrire l’itinéraire de SignalR et de configurer les options de SignalR](#route)

    - [L’URL /signalr](#signalrurl)
    - [Configuration des options de SignalR](#options)
- [Comment créer et utiliser des classes de Hub](#hubclass)

    - [Durée de vie d’objet Hub](#transience)
    - [Casse mixte des noms de Hub dans les clients JavaScript](#hubnames)
    - [Plusieurs concentrateurs](#multiplehubs)
- [Comment définir des méthodes dans la classe de concentrateur, les clients peuvent appeler](#hubmethods)

    - [Casse mixte de noms de méthodes dans les clients JavaScript](#methodnames)
    - [Pour exécuter de façon asynchrone](#asyncmethods)
    - [Définir des surcharges](#overloads)
- [Comment appeler des méthodes de client à partir de la classe de concentrateur](#callfromhub)

    - [En sélectionnant les clients qui recevront le RPC](#selectingclients)
    - [Aucune validation de la compilation pour les noms de méthode](#dynamicmethodnames)
    - [Correspondance de noms de non-respect de la casse (méthode)](#caseinsensitive)
    - [Exécution asynchrone](#asyncclient)
- [Comment gérer l’appartenance au groupe à partir de la classe de concentrateur](#groupsfromhub)

    - [Exécution asynchrone de méthodes Add et Remove](#asyncgroupmethods)
    - [Persistance de l’appartenance au groupe](#grouppersistence)
    - [Groupes d’utilisateur unique](#singleusergroups)
- [Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur](#connectionlifetime)

    - [Lorsque les OnConnected, OnDisconnected et OnReconnected sont appelées](#onreconnected)
    - [État de l’appelant ne pas remplie](#nocallerstate)
- [Comment obtenir des informations sur le client à partir de la propriété de contexte](#contextproperty)
- [Comment passer l’état entre les clients et de la classe de concentrateur](#passstate)
- [Comment gérer les erreurs dans la classe de concentrateur](#handleErrors)
- [Comment appeler des méthodes de client et de gérer des groupes extérieurs à la classe de concentrateur](#callfromoutsidehub)

    - [Appel de méthodes de client](#callingclientsoutsidehub)
    - [La gestion de l’appartenance au groupe](#managinggroupsoutsidehub)
- [Comment activer le suivi](#tracing)
- [Comment personnaliser le pipeline de Hubs](#hubpipeline)

Pour obtenir une documentation sur comment les clients du programme, consultez les ressources suivantes :

- [Guide de l’API SignalR Hubs - Client JavaScript](index.md)
- [Guide de l’API SignalR Hubs - Client .NET](index.md)

Liens vers des rubriques de référence de l’API sont à la version de .NET 4.5 de l’API. Si vous utilisez .NET 4, consultez [la version de .NET 4 des rubriques API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>Comment inscrire l’itinéraire de SignalR et de configurer les options de SignalR

Pour définir l’itinéraire que les clients utiliseront pour se connecter à votre Hub, appelez le [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) méthode lorsque l’application démarre. `MapHubs` est un [méthode d’extension](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) pour la `System.Web.Routing.RouteCollection` classe. L’exemple suivant montre comment définir l’itinéraire de concentrateurs SignalR dans les *Global.asax* fichier.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

Si vous ajoutez des fonctionnalités de SignalR à une application ASP.NET MVC, assurez-vous que l’itinéraire de SignalR est ajouté avant les autres itinéraires. Pour plus d’informations, consultez [didacticiel : bien démarrer avec SignalR et MVC 4](index.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>L’URL /signalr

Par défaut, l’URL de routage qui les clients utiliseront pour se connecter à votre Hub est « / signalr ». (Ne confondez pas cette URL avec l’URL « / signalr hubs », ce qui concerne le fichier JavaScript généré automatiquement. Pour plus d’informations sur le proxy généré, consultez [SignalR Guide de l’API Hubs - JavaScript Client - le proxy généré et ce qu’il fait pour vous](index.md).)

Il peut y avoir des circonstances extraordinaires qui rendent cette URL de base non utilisables pour SignalR ; par exemple, vous avez un dossier dans votre projet nommé *signalr* et vous ne souhaitez pas modifier le nom. Dans ce cas, vous pouvez modifier l’URL de base, comme indiqué dans les exemples suivants (remplacez « / signalr » dans l’exemple de code avec votre URL de votre choix).

**Code qui spécifie l’URL du serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

**Code JavaScript client qui spécifie l’URL (avec le proxy généré)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Code JavaScript client qui spécifie l’URL (sans le proxy généré)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

**Code de client .NET qui spécifie l’URL**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configuration des Options de SignalR

Les surcharges de la `MapHubs` méthode vous permettre de spécifier une URL personnalisée, un résolveur de dépendance personnalisées et les options suivantes :

- Activer les appels interdomaines à partir de clients de navigateur.

    En général, si le navigateur charge d’une page `http://contoso.com`, la connexion de SignalR est dans le même domaine, `http://contoso.com/signalr`. Si la page à partir de `http://contoso.com` établit une connexion à `http://fabrikam.com/signalr`, qui est une connexion entre domaines. Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut. Pour plus d’informations, consultez [Guide de l’API ASP.NET SignalR Hubs - JavaScript Client - comment établir une connexion entre domaines](index.md).
- Activer les messages d’erreur détaillés.

    Lorsque des erreurs se produisent, le comportement par défaut de SignalR consiste à envoyer aux clients un message de notification sans plus d’informations sur ce qui est arrivé. Envoi des informations détaillées sur les clients n’est pas recommandée en production, car les utilisateurs malveillants peuvent être en mesure d’utiliser les informations dans les attaques contre votre application. Pour le dépannage, vous pouvez utiliser cette option pour activer temporairement le rapport d’erreurs plus explicites.
- Désactiver les fichiers de proxy JavaScript générés automatiquement.

    Par défaut, un fichier JavaScript avec les proxys pour vos classes Hub est généré en réponse à l’URL « / signalr hubs ». Si vous ne souhaitez pas utiliser les proxys JavaScript, ou si vous souhaitez générer ce fichier manuellement et faire référence à un fichier physique de vos clients, vous pouvez utiliser cette option pour désactiver la génération de proxy. Pour plus d’informations, consultez [SignalR Guide de l’API Hubs - JavaScript Client - proxy généré de la création d’un fichier physique pour SignalR](index.md).

L’exemple suivant montre comment spécifier l’URL de connexion SignalR et ces options dans un appel à la `MapHubs` (méthode). Pour spécifier une URL personnalisée, remplacez « / signalr » dans l’exemple avec l’URL que vous souhaitez utiliser.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Comment créer et utiliser des classes de Hub

Pour créer un concentrateur, créez une classe qui dérive de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). L’exemple suivant montre une classe de concentrateur simple pour une application de conversation.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

Dans cet exemple, un client connecté peut appeler le `NewContosoChatMessage` (méthode), et lorsque cela arrive, les données reçues sont diffusées à tous les clients connectés.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Durée de vie d’objet Hub

Vous n’instanciez la classe de concentrateur ou appelez ses méthodes à partir de votre propre code sur le serveur ; tout cela est fait pour vous par le pipeline de concentrateurs SignalR. SignalR crée une nouvelle instance de votre classe Hub chaque fois qu’il faut gérer une opération de concentrateur, comme lorsqu’un client se connecte, se déconnecte ou effectue une appel de méthode sur le serveur.

Étant donné que les instances de la classe de concentrateur sont temporaires, vous ne pouvez pas les utiliser pour gérer l’état à partir d’un appel de méthode à l’autre. Chaque fois que le serveur reçoit un appel de méthode à partir d’un client, une nouvelle instance de votre processus de classe de concentrateur le message. Pour conserver l’état via plusieurs connexions et les appels de méthode, utilisez une autre méthode, tel qu’une base de données, ou une variable statique sur la classe de concentrateur ou une autre classe ne dérive pas de `Hub`. Si vous conservez les données en mémoire, à l’aide d’une méthode telle qu’une variable statique sur la classe de concentrateur, les données seront perdues lors du recyclage du domaine d’application.

Si vous souhaitez envoyer des messages aux clients à partir de votre propre code qui s’exécute en dehors de la classe de concentrateur, ne peut pas procéder en instanciant une instance de classe de concentrateur, mais vous pouvez le faire en obtenant une référence à l’objet de contexte de SignalR pour votre classe de concentrateur. Pour plus d’informations, consultez [comment appeler des méthodes de client et de gérer des groupes extérieurs à la classe de concentrateur](#callfromoutsidehub) plus loin dans cette rubrique.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Casse mixte des noms de Hub dans les clients JavaScript

Par défaut, les clients JavaScript font référence aux Hubs en utilisant une version de casse mixte du nom de classe. SignalR crée automatiquement ce changement afin que le code JavaScript peut être conforme aux conventions de JavaScript. L’exemple précédent est désigné en tant que `contosoChatHub` dans le code JavaScript.

**Serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**Client JavaScript à l’aide du proxy généré**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

Si vous souhaitez spécifier un nom différent pour les clients à utiliser, ajoutez le `HubName` attribut. Lorsque vous utilisez un `HubName` attribut, il n’existe aucun changement de nom en casse mixte sur les clients JavaScript.

**Serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**Client JavaScript à l’aide du proxy généré**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Plusieurs concentrateurs

Vous pouvez définir plusieurs classes de Hub dans une application. Lorsque vous procédez ainsi, la connexion est partagée, mais les groupes sont distincts :

- Tous les clients utilisera la même URL pour établir une connexion de SignalR avec votre service (« / signalr » ou l’URL personnalisée si vous avez spécifié un), et que la connexion est utilisée pour tous les Hubs définies par le service.

    Il n’existe aucune différence de performances pour plusieurs concentrateurs par rapport à la définition de toutes les fonctionnalités de Hub dans une classe unique.
- Tous les concentrateurs d’obtenir les mêmes informations de demande HTTP.

    Étant donné que tous les Hubs partagent la même connexion, les seules informations de demande HTTP qui obtient le serveur sont ce qui est fourni dans la requête HTTP d’origine qui établit la connexion de SignalR. Si la demande de connexion vous permet de transmettre des informations à partir du client au serveur en spécifiant une chaîne de requête, vous ne peut pas fournir des chaînes de requête différentes à différents concentrateurs. Tous les Hubs reçoit les mêmes informations.
- Le fichier de proxys JavaScript généré contiendra des proxys pour tous les Hubs dans un seul fichier.

    Pour plus d’informations sur les proxys JavaScript, consultez [SignalR Guide de l’API Hubs - JavaScript Client - le proxy généré et ce qu’il fait pour vous](index.md).
- Les groupes sont définis dans les Hubs.

    Dans SignalR, que vous pouvez définir des groupes nommés à diffusion à des sous-ensembles de clients connectés. Les groupes sont gérées séparément pour chaque concentrateur. Par exemple, un groupe nommé « Administrateurs » comprend un ensemble de clients pour votre `ContosoChatHub` classe et le nom du groupe même faites référence à un autre ensemble de clients pour votre `StockTickerHub` classe.

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Comment définir des méthodes dans la classe de concentrateur, les clients peuvent appeler

Pour exposer une méthode sur le Hub que vous souhaitez pouvoir être appelée à partir du client, déclarez une méthode publique, comme indiqué dans les exemples suivants.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Vous pouvez spécifier un type de retour et paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#. Toutes les données que vous recevez dans les paramètres ou retourner à l’appelant sont communiquées entre le client et le serveur à l’aide de JSON et SignalR gère la liaison d’objets complexes et des tableaux d’objets automatiquement.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Casse mixte de noms de méthodes dans les clients JavaScript

Par défaut, les clients JavaScript font référence aux méthodes de concentrateur à l’aide d’une version de casse mixte du nom de méthode. SignalR crée automatiquement ce changement afin que le code JavaScript peut être conforme aux conventions de JavaScript.

**Serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Client JavaScript à l’aide du proxy généré**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

Si vous souhaitez spécifier un nom différent pour les clients à utiliser, ajoutez le `HubMethodName` attribut.

**Serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Client JavaScript à l’aide du proxy généré**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Pour exécuter de façon asynchrone

Si la méthode sera être longs ou effectuer le travail qui serait impliquent en attente, par exemple une recherche de base de données ou un appel de service web, la méthode de concentrateur asynchrones en retournant un [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (à la place de `void` retourner) ou [ Tâche&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objet (à la place de `T` type de retour). Lorsque vous retournez un `Task` objet à partir de la méthode, SignalR attend le `Task` pour terminer, puis envoie le résultat désencapsulé au client, donc il n’existe aucune différence dans la façon dont vous codez l’appel de méthode dans le client.

Effectue une méthode de concentrateur asynchrone évite de bloquer la connexion lorsqu’il utilise le transport WebSocket. Quand une méthode de concentrateur exécute de façon synchrone et le transport est WebSocket, les appels suivants de méthodes sur le concentrateur à partir du même client sont bloquées jusqu'à la fin de la méthode de concentrateur.

L’exemple suivant montre la même méthode codée pour exécuter de façon synchrone ou asynchrone, suivi par le code client JavaScript qui fonctionne pour appeler des versions.

**Synchrone**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**Asynchrone - ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Client JavaScript à l’aide du proxy généré**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

Pour plus d’informations sur l’utilisation de méthodes asynchrones dans ASP.NET 4.5, consultez [à l’aide de méthodes asynchrones dans ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Définir des surcharges

Si vous souhaitez définir de surcharges pour une méthode, le nombre de paramètres dans chaque surcharge doit être différent. Si vous différencier une surcharge simplement en spécifiant des différents types de paramètres, votre classe de concentrateur compilera, mais le service SignalR lève une exception au moment de l’exécution lorsque les clients essaient pour appeler l’une des surcharges.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Comment appeler des méthodes de client à partir de la classe de concentrateur

Pour appeler des méthodes de client à partir du serveur, utilisez le `Clients` propriété dans une méthode dans votre classe de concentrateur. L’exemple suivant montre le code de serveur qui appelle `addNewMessageToPage` sur tous les clients connectés et le code client qui définit la méthode dans un client JavaScript.

**Serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**Client JavaScript à l’aide du proxy généré**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

Impossible d’obtenir une valeur de retour à partir d’une méthode du client ; la syntaxe `int x = Clients.All.add(1,1)` ne fonctionne pas.

Vous pouvez spécifier les types complexes et des tableaux pour les paramètres. L’exemple suivant passe un type complexe au client dans un paramètre de méthode.

**Code de serveur qui appelle une méthode de client à l’aide d’un objet complexe**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**Code de serveur qui définit l’objet complexe**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**Client JavaScript à l’aide du proxy généré**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>En sélectionnant les clients qui recevront le RPC

La propriété retourne de Clients un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objet qui fournit plusieurs options permettant de spécifier les clients qui recevront le RPC :

- Tous les clients connectés.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- Client appelant.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- Tous les clients à l’exception du client appelant.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- Un client spécifique identifié par l’ID de connexion.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    Cet exemple appelle `addContosoChatMessageToPage` sur le client appelant et a le même effet que l’utilisation de `Clients.Caller`.
- Tous les clients connectés à l’exception des clients spécifiés, identifiés par l’ID de connexion.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- Tous les clients connectés dans un groupe spécifié.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifié par l’ID de connexion.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- Tous les clients connectés dans un groupe spécifié à l’exception du client appelant.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Aucune validation de la compilation pour les noms de méthode

Le nom de la méthode que vous spécifiez est interprété comme un objet dynamique, ce qui signifie qu’aucun IntelliSense ou la validation au moment de la compilation pour celui-ci. L’expression est évaluée au moment de l’exécution. Lorsque l’appel de méthode s’exécute, SignalR envoie le nom de méthode et les valeurs de paramètre au client, et si le client a une méthode qui correspond au nom que la méthode est appelée et les valeurs de paramètre sont passés. Si aucune méthode correspondante est trouvée sur le client, aucune erreur n’est générée. Pour plus d’informations sur le format des données SignalR transmet au client en arrière-plan lorsque vous appelez une méthode de client, consultez [Introduction à SignalR](index.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Correspondance de noms de non-respect de la casse (méthode)

Correspondance de noms de méthode respecte la casse. Par exemple, `Clients.All.addContosoChatMessageToPage` sur le serveur s’exécutera `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, ou `addContosoChatMessageToPage` sur le client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Exécution asynchrone

La méthode que vous appelez exécute de façon asynchrone. Tout code qui vient après qu’un appel de méthode à un client doit être exécutée immédiatement sans attendre SignalR terminer la transmission des données aux clients, sauf si vous spécifiez que les lignes suivantes de code doivent attendre d’achèvement de la méthode. Les exemples de code suivants montrent comment exécuter séquentiellement les deux méthodes de client à l’aide de code qui fonctionne dans .NET 4.5 et à l’aide de code qui fonctionne dans .NET 4.

**Exemple de .NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**Exemple de .NET 4**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

Si vous utilisez `await` ou `ContinueWith` pour attendre qu’une méthode du client se termine avant la ligne de code suivante s’exécute, cela ne signifie pas dans que les clients seront réellement le message avant l’exécution de la ligne de code suivante. « Exécution » d’un appel de méthode client signifie uniquement que SignalR a fait tous les éléments nécessaires pour envoyer le message. Si vous avez besoin que les clients reçu le message de vérification, vous devez programmer ce mécanisme vous-même. Par exemple, vous pourriez coder un `MessageReceived` méthode sur le concentrateur et dans le `addContosoChatMessageToPage` méthode sur le client, vous pouvez appeler `MessageReceived` une fois que vous le faites tout ce qui fonctionne, vous devez effectuer sur le client. Dans `MessageReceived` dans le Hub, vous pouvez effectuer le travail dépend de celle de la réception d’effective du client et le traitement de l’appel de méthode d’origine.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Comment utiliser une variable de chaîne comme nom de la méthode

Si vous souhaitez appeler une méthode de client à l’aide d’une variable de chaîne en tant que nom de la méthode, casté `Clients.All` (ou `Clients.Others`, `Clients.Caller`, etc.) à `IClientProxy` , puis appelez [Invoke (Nom_méthode, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Comment gérer l’appartenance au groupe à partir de la classe de concentrateur

Groupes dans SignalR fournissent une méthode pour diffuser des messages à des sous-ensembles spécifiés de clients connectés. Un groupe peut avoir n’importe quel nombre de clients, et un client peut être un membre de n’importe quel nombre de groupes.

Pour gérer l’appartenance au groupe, utilisez le [ajouter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) et [supprimer](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) méthodes fournies par le `Groups` propriété de la classe de concentrateur. L’exemple suivant montre le `Groups.Add` et `Groups.Remove` méthodes utilisées dans les méthodes de concentrateur qui sont appelées par le code client, suivi par le code client JavaScript qui les appelle.

**Serveur**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**Client JavaScript à l’aide du proxy généré**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

Vous n’êtes pas obligé de créer explicitement des groupes. En effet un groupe est créé automatiquement la première fois que vous spécifiez son nom dans un appel à `Groups.Add`, et il est supprimé lorsque vous supprimez la dernière connexion de l’appartenance à elle.

Il n’existe aucune API pour obtenir une liste d’appartenance de groupe ou une liste de groupes. SignalR envoie des messages vers des clients et des groupes basés sur un [modèle pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), et le serveur ne conserve pas les listes de groupes ou des appartenances aux groupes. Cela permet de maximiser l’évolutivité, car chaque fois que vous ajoutez un nœud à une batterie de serveurs web, n’importe quel état SignalR tient à jour doit être propagée vers le nouveau nœud.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Exécution asynchrone de méthodes Add et Remove

Le `Groups.Add` et `Groups.Remove` méthodes exécuter de façon asynchrone. Si vous souhaitez ajouter un client à un groupe et d’envoyer immédiatement un message au client en utilisant le groupe, vous devez vous assurer que le `Groups.Add` méthode se termine en premier. Les exemples de code suivants montrent comment procéder, un à l’aide de code qui fonctionne dans .NET 4.5 et l’autre à l’aide de code qui fonctionne dans .NET 4

**Exemple de .NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**Exemple de .NET 4**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistance de l’appartenance au groupe

SignalR effectue le suivi des connexions, pas les utilisateurs, par conséquent, si vous souhaitez qu’un utilisateur se trouver dans le même groupe chaque fois que l’utilisateur établit une connexion, vous devez appeler `Groups.Add` chaque fois que l’utilisateur établit une nouvelle connexion.

Après une perte temporaire de connectivité, parfois SignalR peut restaurer la connexion automatiquement. Dans ce cas, SignalR restaure la même connexion, ne pas l’établissement d’une nouvelle connexion, et par conséquent, l’appartenance de groupe du client est automatiquement restaurée. Cela est possible même lorsque l’arrêt temporaire est le résultat d’un redémarrage du serveur ou d’échec, car l’état de connexion pour chaque client, y compris les appartenances de groupe, est un aller-retour vers le client. Si un serveur tombe en panne et est remplacé par un nouveau serveur avant l’expiration de la connexion, un client puisse se reconnecter automatiquement vers le nouveau serveur et réinscrire dans les groupes, qu'il est membre.

Lorsqu’une connexion ne peut pas être restaurée automatiquement après une perte de connectivité, lorsque la connexion arrive à expiration, ou la déconnexion du client (par exemple, lorsqu’un navigateur accède à une nouvelle page), les appartenances aux groupes sont perdues. La prochaine fois que l’utilisateur se connecte sera une nouvelle connexion. Pour gérer l’appartenance au groupe lorsque le même utilisateur établit une nouvelle connexion, votre application doit suivre les associations entre les utilisateurs et groupes et restaurer les appartenances aux groupes chaque fois qu’un utilisateur établit une nouvelle connexion.

Pour plus d’informations sur les connexions et de reconnexion, consultez [comment gérer les événements de durée de vie de connexion dans la classe de concentrateur](#connectionlifetime) plus loin dans cette rubrique.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Groupes d’utilisateur unique

Applications qui utilisent SignalR généralement faire suivre les associations entre les utilisateurs et connexions afin de savoir quel utilisateur a envoyé un message et les utilisateurs doivent recevoir un message. Groupes sont utilisés dans un des deux modèles couramment utilisés pour effectuer cette opération.

- Groupes d’utilisateur unique.

    Vous pouvez spécifier le nom d’utilisateur en tant que le nom du groupe et ajouter l’ID de connexion actuel au groupe chaque fois que l’utilisateur se connecte ou se reconnecte. Pour envoyer des messages à l’utilisateur que vous envoyez au groupe. L’inconvénient de cette méthode est que le groupe ne vous fournit un moyen de savoir si l’utilisateur est en ligne ou hors connexion.
- Effectuer le suivi des associations entre les noms d’utilisateur et ID de connexion.

    Vous pouvez stocker une association entre chaque nom d’utilisateur et un ou plusieurs ID de connexion dans un dictionnaire ou une base de données et mettre à jour les données stockées chaque fois que l’utilisateur se connecte ou se déconnecte. Pour envoyer des messages à l’utilisateur, vous spécifiez l’ID de connexion. L’inconvénient de cette méthode est qu’il faut davantage de mémoire.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur

Les raisons courantes de gestion des événements de durée de vie de connexion sont à suivre si un utilisateur est connecté ou non et effectuer le suivi de l’association entre les noms d’utilisateur et ID de connexion. Pour exécuter votre propre code quand les clients connecter ou déconnecter, remplacer le `OnConnected`, `OnDisconnected`, et `OnReconnected` méthodes virtuelles du Hub de classe, comme indiqué dans l’exemple suivant.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Lorsque les OnConnected, OnDisconnected et OnReconnected sont appelées

Chaque fois qu’un navigateur accède à une nouvelle page, une nouvelle connexion doit être établie, ce qui signifie que SignalR exécutera le `OnDisconnected` méthode suivie par le `OnConnected` (méthode). SignalR crée toujours un nouvel ID de connexion lorsqu’une nouvelle connexion est établie.

Le `OnReconnected` méthode est appelée en cas d’un arrêt temporaire de connectivité SignalR peut récupérer automatiquement à partir, par exemple lorsqu’un câble est temporairement déconnecté et reconnecté avant l’expiration de la connexion. Le `OnDisconnected` méthode est appelée lorsque le client est déconnecté et SignalR ne peut pas se reconnecter automatiquement, par exemple quand un navigateur accède à une nouvelle page. Par conséquent, une séquence d’événements pour un client donné est `OnConnected`, `OnReconnected`, `OnDisconnected`; ou `OnConnected`, `OnDisconnected`. Vous ne verrez pas la séquence `OnConnected`, `OnDisconnected`, `OnReconnected` pour une connexion donnée.

Le `OnDisconnected` (méthode) n’est pas appelée dans certains scénarios, tels que lorsqu’un serveur tombe en panne ou le domaine d’application obtient recyclé. Lorsqu’un autre serveur est en ligne ou le domaine d’application se termine son recyclage, certains clients peuvent être en mesure de se reconnecter et de déclencher le `OnReconnected` événement.

Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie des connexions dans SignalR](index.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>État de l’appelant ne pas remplie

Les méthodes de gestionnaire de connexion durée de vie des événements sont appelés à partir du serveur, ce qui signifie que n’importe quel état que vous placez dans le `state` objet sur le client ne sera pas renseigné dans le `Caller` propriété sur le serveur. Pour plus d’informations sur la `state` objet et le `Caller` propriété, consultez [comment passer l’état entre les clients et de la classe de concentrateur](#passstate) plus loin dans cette rubrique.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Comment obtenir des informations sur le client à partir de la propriété de contexte

Pour obtenir des informations sur le client, utilisez le `Context` propriété de la classe de concentrateur. Le `Context` propriété retourne un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objet qui fournit l’accès aux informations suivantes :

- L’ID de connexion du client appelant.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    L’ID de connexion est un GUID qui est affecté par SignalR (vous ne pouvez pas spécifier la valeur dans votre propre code). Il existe un identifiant de connexion pour chaque connexion et même QU'ID est utilisé par tous les concentrateurs, si vous avez plusieurs concentrateurs dans votre application.
- Données d’en-tête HTTP.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    Vous pouvez également obtenir des en-têtes HTTP à partir de `Context.Headers`. La raison de plusieurs références à la même chose est que `Context.Headers` a été créé en premier, le `Context.Request` propriété a été ajoutée à une version ultérieure, et `Context.Headers` a été conservée pour compatibilité descendante.
- Interroger les données de chaîne.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    Vous pouvez également obtenir des données de chaîne de requête à partir de `Context.QueryString`.

    La chaîne de requête que vous obtenez dans cette propriété est celui qui a été utilisé avec la demande HTTP ayant établi la connexion de SignalR. Vous pouvez ajouter des paramètres de chaîne de requête dans le client en configurant la connexion, qui est un moyen pratique pour passer des données sur le client à partir du client au serveur. L’exemple suivant montre comment ajouter une chaîne de requête dans un client JavaScript lorsque vous utilisez le proxy généré.

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    Pour plus d’informations sur la définition des paramètres de chaîne de requête, consultez les guides de l’API pour le [JavaScript](index.md) et [.NET](index.md) les clients.

    Vous pouvez trouver la méthode de transport utilisée pour la connexion dans les données de chaîne de requête, ainsi que certains autres valeurs utilisées en interne par SignalR :

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    La valeur de `transportMethod` sera « webSockets », « serverSentEvents », « foreverFrame » ou « longPolling ». Notez que si vous cochez cette valeur la `OnConnected` méthode de gestionnaire d’événements, dans certains scénarios, vous pouvez initialement obtenir une valeur de transport qui n’est pas la méthode de transport négociée final pour la connexion. Dans ce cas, la méthode lève une exception et sera appelée ultérieurement lorsque le mode de transport finale est établi.
- Cookies.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Vous pouvez également obtenir les cookies de `Context.RequestCookies`.
- Informations de l’utilisateur.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- L’objet HttpContext pour la demande :

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    Utilisez cette méthode au lieu d’obtenir `HttpContext.Current` pour obtenir le `HttpContext` objet pour la connexion de SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Comment passer l’état entre les clients et de la classe de concentrateur

Le proxy client fournit un `state` objet dans lequel vous pouvez stocker des données que vous souhaitez être transmises au serveur avec chaque appel de méthode. Sur le serveur, vous pouvez accéder à ces données dans le `Clients.Caller` propriété dans les méthodes de concentrateur qui sont appelées par les clients. Le `Clients.Caller` propriété n’est pas remplie pour les méthodes de gestionnaire de connexion durée de vie des événements `OnConnected`, `OnDisconnected`, et `OnReconnected`.

Création ou mise à jour des données dans le `state` objet et le `Clients.Caller` propriété fonctionne dans les deux sens. Vous pouvez mettre à jour les valeurs dans le serveur et qu’ils sont transmis au client.

L’exemple suivant montre le code JavaScript client qui stocke l’état de transmission au serveur avec chaque appel de méthode.

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

L’exemple suivant montre le code équivalent dans un client .NET.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

Dans votre classe de concentrateur, vous pouvez accéder à ces données dans le `Clients.Caller` propriété. L’exemple suivant montre le code qui Récupère l’état fait référence dans l’exemple précédent.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> Ce mécanisme de persistance de l’état n’est pas destiné pour de grandes quantités de données, depuis tout ce que vous placez dans le `state` ou `Clients.Caller` propriété est un aller-retour avec chaque appel de méthode. Il est utile pour les éléments plus petits tels que les noms d’utilisateur ou de compteurs.


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Comment gérer les erreurs dans la classe de concentrateur

Pour gérer les erreurs qui se produisent dans vos méthodes de classe de concentrateur, utilisent au moins des méthodes suivantes :

- Encapsuler votre code de méthode dans les blocs try-catch et les journaux de l’objet exception. Pour le débogage, vous pouvez envoyer l’exception au client, mais pour la sécurité raisons envoyant des informations détaillées sur les clients en production ne sont pas recommandées.
- Créer un module de pipeline de Hubs qui gère la [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) (méthode). L’exemple suivant montre un module de pipeline qui enregistre les erreurs, suivies du code dans Global.asax qui injecte le module dans le pipeline de Hubs.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

Pour plus d’informations sur les modules de pipeline Hub, consultez [comment personnaliser le pipeline de Hubs](#hubpipeline) plus loin dans cette rubrique.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Comment activer le suivi

Pour activer le suivi côté serveur, ajoutez un élément system.diagnostics à votre fichier Web.config, comme illustré dans cet exemple :

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

Lorsque vous exécutez l’application dans Visual Studio, vous pouvez afficher les journaux dans le **sortie** fenêtre.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Comment appeler des méthodes de client et de gérer des groupes extérieurs à la classe de concentrateur

Pour appeler des méthodes client d’une autre classe que votre classe de concentrateur, obtenir une référence à l’objet de contexte de SignalR pour le Hub et l’utiliser pour appeler des méthodes sur le client ou gérer des groupes.

L’exemple suivant `StockTicker` classe obtient l’objet de contexte, il stocke dans une instance de la classe, stocke l’instance de classe dans une propriété statique et utilise le contexte de l’instance de la classe singleton pour appeler le `updateStockPrice` sur les clients (méthode) connecté à un Hub nommé `StockTickerHub`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

Si vous avez besoin d’utiliser les contexte plusieurs fois dans un objet de longue durée, obtenir la référence une seule fois et enregistrer il plutôt que de l’obtenir à chaque fois. Obtention du contexte une fois garantit que SignalR envoie des messages aux clients dans l’ordre dans lequel vos méthodes de concentrateur effectuent client les appels de méthode. Pour obtenir un didacticiel qui montre comment utiliser le contexte de SignalR pour un Hub, consultez [diffusion par le serveur avec ASP.NET SignalR](index.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Appel de méthodes de client

Vous pouvez spécifier les clients qui recevront le RPC, mais vous avez moins d’options que lorsque vous appelez à partir d’une classe de concentrateur. La raison à cela est que le contexte n’est pas associé à un appel particulier à partir d’un client, donc toutes les méthodes qui nécessitent des connaissances de l’ID de connexion actuel, telles que `Clients.Others`, ou `Clients.Caller`, ou `Clients.OthersInGroup`, ne sont pas disponibles. Les options ci-dessous sont disponibles :

- Tous les clients connectés.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- Un client spécifique identifié par l’ID de connexion.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- Tous les clients connectés à l’exception des clients spécifiés, identifiés par l’ID de connexion.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- Tous les clients connectés dans un groupe spécifié.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifié par l’ID de connexion.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

Si vous appelez dans votre classe non-Hub à partir de méthodes dans votre classe de concentrateur, vous pouvez entrer l’ID de connexion en cours et l’utiliser avec `Clients.Client`, `Clients.AllExcept`, ou `Clients.Group` pour simuler `Clients.Caller`, `Clients.Others`, ou `Clients.OthersInGroup`. Dans l’exemple suivant, le `MoveShapeHub` classe passe l’ID de connexion à la `Broadcaster` classe afin que le `Broadcaster` classe peut simuler `Clients.Others`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>La gestion de l’appartenance au groupe

Pour la gestion des groupes, vous avez les mêmes options comme vous le feriez dans une classe de concentrateur.

- Ajouter un client à un groupe

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- Supprimer un client d’un groupe

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Comment personnaliser le pipeline de Hubs

SignalR vous permet d’injecter votre propre code dans le pipeline de concentrateur. L’exemple suivant montre un module de pipeline de Hub personnalisé qui enregistre chaque appel de méthode entrant reçu à partir du client et l’appel de méthode sortant appelé sur le client :

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

Le code suivant dans le *Global.asax* fichier enregistre le module à s’exécuter dans le pipeline de Hub :

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

Il existe de nombreuses méthodes différentes que vous pouvez substituer. Pour obtenir la liste complète, consultez [HubPipelineModule méthodes](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
