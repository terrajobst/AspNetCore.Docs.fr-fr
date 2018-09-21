---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: Résolution des problèmes de SignalR | Microsoft Docs
author: pfletcher
description: Cet article décrit les problèmes courants avec le développement des applications SignalR.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 9ef69ece2558260f0409e9388751f77ac84a06f1
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523075"
---
<a name="signalr-troubleshooting"></a>Résolution des problèmes de SignalR
====================
par [Patrick Fletcher](https://github.com/pfletcher)

> Ce document décrit la résolution des problèmes courants avec SignalR.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versions des logiciels utilisées dans cette rubrique
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR version 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de cette rubrique
> 
> Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


Ce document contient les sections suivantes.

- [Appel de méthodes entre le client et le serveur en mode silencieux échoue](#connection)
- [Configuration des websockets IIS ping/pong pour détecter un client inactif](#pong)
- [Autres problèmes de connexion](#other)
- [Erreurs de compilation et côté serveur](#server)
- [Problèmes de Visual Studio](#vs)
- [Problèmes d’Internet Information Services](#iis)
- [Problèmes de Microsoft Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Appel de méthodes entre le client et le serveur en mode silencieux échoue

Cette section décrit les causes possibles d’un appel de méthode entre client et serveur échoue sans message d’erreur explicite. Dans une application SignalR, le serveur dispose d’aucune information sur les méthodes que le client implémente ; Lorsque le serveur appelle une méthode du client, les données de nom et le paramètre de méthode sont envoyées au client, et la méthode est exécutée uniquement si elle existe dans le format que le serveur spécifié. Si aucune méthode correspondante est trouvée sur le client, rien ne se produit, et aucun message d’erreur n’est déclenchée sur le serveur.

Pour examiner en détail les méthodes client ne pas appelées, vous pouvez activer la journalisation avant d’appeler la méthode start sur le concentrateur pour voir les appels sont provenant du serveur. Pour activer la journalisation dans une application JavaScript, consultez [comment activer la journalisation côté client (version du client JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Pour activer la journalisation dans une application cliente .NET, consultez [comment activer la journalisation côté client (version de Client .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Méthode mal orthographié, signature de méthode incorrect ou nom de hub incorrect

Si le nom ou la signature d’une méthode appelée ne correspond pas exactement à une méthode appropriée sur le client, l’appel échoue. Vérifiez que le nom de la méthode appelé par le serveur correspond au nom de la méthode sur le client. SignalR crée également le proxy de hub à l’aide des méthodes de casse mixte, en l’état approprié dans JavaScript, par conséquent, une méthode appelée `SendMessage` sur le serveur est appelé `sendMessage` dans le proxy client. Si vous utilisez le `HubName` attribut dans votre code côté serveur, vérifiez que le nom utilisé correspond au nom utilisé pour créer le hub sur le client. Si vous n’utilisez pas le `HubName` attribut, vérifiez que le nom du hub dans un client JavaScript est une casse mixte, par exemple chatHub au lieu de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nom de la méthode en double sur le client

Vérifiez que vous n’avez pas une méthode en double sur le client qui diffère uniquement par la casse. Si votre application cliente a une méthode appelée `sendMessage`, vérifiez qu’il n’est pas également une méthode appelée `SendMessage` également.

### <a name="missing-json-parser-on-the-client"></a>Analyseur JSON manquant sur le client

SignalR requiert un analyseur JSON pour être présents pour sérialiser les appels entre le serveur et le client. Si votre client n’a pas un analyseur JSON intégré (par exemple, Internet Explorer 7), vous devez inclure dans votre application. Vous pouvez télécharger l’analyseur JSON [ici](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Le mélange des syntaxes de Hub et PersistentConnection

SignalR utilise deux modèles de communication : concentrateurs et PersistentConnections. La syntaxe d’appel de ces modèles de deux communication est différente dans le code client. Si vous avez ajouté un hub dans votre code serveur, vérifiez que tout votre code client utilise la syntaxe de hub approprié.

**Code JavaScript client qui crée une PersistentConnection dans un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Code JavaScript client qui crée un Proxy de Hub dans un client Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Code de serveur c# qui mappe un itinéraire à une PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Code de serveur C# qui mappe un itinéraire vers un Hub ou à plusieurs hubs si vous possédez plusieurs applications**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>Connexion démarrée avant que les abonnements sont ajoutés.

Si la connexion de concentrateur est démarrée avant que les méthodes qui peuvent être appelées à partir du serveur sont ajoutées au proxy, les messages ne seront pas reçus. Le code JavaScript suivant ne démarre pas correctement le hub :

**Code JavaScript client incorrect qui ne permet pas de recevoir des messages de Hubs**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Au lieu de cela, ajoutez les abonnements de la méthode avant d’appeler Start :

**Code JavaScript client qui ajoute correctement les abonnements à un concentrateur**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nom de la méthode manquante sur le concentrateur proxy

Vérifiez que la méthode définie sur le serveur est abonnée sur le client. Même si le serveur définit la méthode, il doit encore être ajouté pour le proxy client. Méthodes peuvent être ajoutés pour le proxy client comme suit (Notez que la méthode est ajoutée à la `client` membre du concentrateur, pas le concentrateur directement) :

**Code JavaScript client qui ajoute des méthodes à un proxy de hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Hub ou méthodes de concentrateur ne pas déclarés comme Public

Pour être visible sur le client, les méthodes et l’implémentation du concentrateur doivent être déclarées comme `public`.

### <a name="accessing-hub-from-a-different-application"></a>L’accès au hub à partir d’une autre application

Les concentrateurs SignalR est uniquement accessible via des applications qui implémentent les clients SignalR. SignalR ne peut pas interagir avec d’autres bibliothèques de communication (par exemple, SOAP ou WCF web services). Si aucun client SignalR n’est disponible pour votre plateforme cible, vous ne pouvez pas accéder directement à point de terminaison du serveur.

### <a name="manually-serializing-data"></a>Sérialisation des données manuellement

SignalR utilisent automatiquement JSON pour sérialiser votre méthode sans devoir de paramètres-là le faire vous-même.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Méthode de concentrateur à distance ne pas exécutée sur le client dans OnDisconnected (fonction)

Ce comportement est inhérent à la conception. Lorsque `OnDisconnected` est appelée, le concentrateur a déjà entré le `Disconnected` état, ce qui n’autorise pas davantage à appeler des méthodes de concentrateur.

**C# code de serveur qui exécute le code dans l’événement OnDisconnected correctement**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>OnDisconnect n’est ne pas exécuté à des moments cohérents

Ce comportement est inhérent à la conception. Lorsqu’un utilisateur tente de quitter une page avec une connexion SignalR active, le client SignalR effectue ensuite une meilleure tentative possible pour notifier le serveur que la connexion cliente va être arrêtée. Si le client SignalR du meilleur effort tentative ne parvient pas à atteindre le serveur, le serveur supprime de la connexion après un configurable `DisconnectTimeout` plus tard, moment auquel le `OnDisconnected` événement se déclenche. Si le client de SignalR de meilleur effort tentative réussit, le `OnDisconnected` événement déclenche immédiatement.

Pour plus d’informations sur la configuration de la `DisconnectTimeout` définition, consultez [gestion des événements de durée de vie de connexion : DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).

### <a name="connection-limit-reached"></a>Limite de connexion atteinte

Lorsque vous utilisez la version complète d’IIS sur un système d’exploitation client Windows 7, une limite de 10 connexions est imposée. Lorsque vous utilisez un système d’exploitation client, utilisez IIS Express au lieu de cela pour éviter cette limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Connexion inter-domaines ne pas correctement configuré

Si une connexion entre domaines (une connexion pour lequel l’URL de SignalR n’est pas dans le même domaine que la page d’hébergement) n’est pas configurée correctement, la connexion peut échouer sans message d’erreur. Pour plus d’informations sur l’activation de la communication entre domaines, consultez [comment établir une connexion entre domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Connexion à l’aide de NTLM (Active Directory) ne fonctionne ne pas dans le client .NET

Une connexion dans une application cliente .NET qui utilise la sécurité de domaine peut échouer si la connexion n’est pas configurée correctement. Pour utiliser SignalR dans un environnement de domaine, définissez la propriété de connexion requis comme suit :

**Code de client c# qui implémente les informations d’identification de connexion**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>Configuration des websockets IIS ping/pong pour détecter un client inactif

Serveurs de SignalR ne connaissez pas si le client est mort ou non, et elles s’appuient sur la notification à partir de websocket sous-jacent pour les échecs de connexion, autrement dit, le `OnClose` rappel. Une solution à ce problème consiste à configurer les websockets IIS pour effectuer le test ping/pong pour vous. Cela garantit que votre connexion se ferme si elle s’arrête inopinément. Pour plus d’informations, consultez [ce billet stackoverflow](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).

<a id="other"></a>

## <a name="other-connection-issues"></a>Autres problèmes de connexion

Cette section décrit les causes et solutions pour les problèmes spécifiques ou des messages d’erreur qui se produisent lors d’une connexion.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Erreur « Début doit être appelé avant l’envoi de données »

Cette erreur se produit couramment si le code référence des objets de SignalR avant le démarrage de la connexion. Le wireup pour les gestionnaires et les autres que seront appellent des méthodes définies sur le serveur doit être ajoutées une fois la connexion terminée. Notez que l’appel à `Start` étant asynchrone, code une fois que l’appel peut être exécuté avant qu’elle se termine. La meilleure façon d’ajouter des gestionnaires d’après le début d’une connexion complètement consiste à les placer dans une fonction de rappel qui est passée en tant que paramètre à la méthode start :

**Code JavaScript client qui ajoute correctement des gestionnaires d’événements qui font référence à des objets de SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Cette erreur apparaîtra également si une connexion s’arrête alors que les objets de SignalR sont toujours référencés.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>« 301 déplacé définitivement » ou une erreur « 302 Déplacé temporairement »

Cette erreur peut se produire si le projet contient un dossier appelé SignalR, qui interfère avec le proxy créé automatiquement. Pour éviter cette erreur, n’utilisez pas un dossier appelé `SignalR` dans votre application, ou activer la génération de proxy automatique désactivé. Consultez [le Proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) pour plus d’informations.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Erreur « 403 Interdit » dans le client .NET ou Silverlight

Cette erreur peut se produire dans les environnements de domaines où la communication inter-domaines n’est pas activée correctement. Pour plus d’informations sur l’activation de la communication entre domaines, consultez [comment établir une connexion entre domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Pour établir une connexion entre domaines dans un client Silverlight, consultez [connexions inter-domaines à partir de clients Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Erreur « 404 not Found »

Il existe plusieurs causes possibles de ce problème. Vérifiez que tous les éléments suivants :

- **Référence d’adresse de proxy de hub ne pas correctement mis en forme :** cette erreur se produite souvent si la référence à l’adresse de proxy de hub généré n’est pas formatée correctement. Vérifiez que la référence à l’adresse de concentrateur est correctement effectuée. Consultez [comment référencer le proxy généré dynamiquement](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) pour plus d’informations.
- **Ajout d’itinéraires à l’application avant d’ajouter l’itinéraire de concentrateur :** si votre application utilise d’autres itinéraires, vérifiez que le premier itinéraire ajouté est l’appel à `MapSignalR`.
- **À l’aide d’IIS 7 ou 7.5 sans la mise à jour des URL sans extension :** à l’aide de IIS 7 ou 7.5 nécessite une mise à jour des URL sans extension afin que le serveur peut fournir l’accès aux définitions de hub au `/signalr/hubs`. Vous pouvez trouver la mise à jour [ici](https://support.microsoft.com/kb/980368).
- **IIS mettre en cache obsolète ou endommagé :** pour vérifier que le contenu du cache n’est pas obsolètes, entrez la commande suivante dans une fenêtre PowerShell pour effacer le cache :

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>« Erreur serveur interne 500 »

Il s’agit d’une erreur très générique qui peut avoir un large éventail de causes. Les détails de l’erreur doivent apparaître dans le journal des événements du serveur, ou sont accessible via le débogage du serveur. Informations d’erreur plus détaillées peuvent être obtenues en activant les erreurs détaillées sur le serveur. Pour plus d’informations, consultez [comment gérer les erreurs dans la classe de concentrateur](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

Cette erreur se produit généralement si un pare-feu ou un proxy n’est pas configuré correctement, à l’origine à réécrire les en-têtes de demande. La solution consiste à vous assurer que le port 80 est activé sur le pare-feu ou le proxy.

### <a name="unexpected-response-code-500"></a>« Code de réponse inattendu : 500 »

Cette erreur peut se produire si la version du .NET framework est utilisé dans l’application ne correspond pas à la version spécifiée dans le fichier Web.Config. La solution consiste à vérifier que .NET 4.5 est utilisé dans les paramètres d’application et le fichier Web.Config.

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>« TypeError : &lt;hubType&gt; n’est pas défini « erreur

Cette erreur se produit si l’appel à `MapSignalR` n’est pas effectuée correctement. Consultez [comment inscrire les SignalR Middleware et de configurer les options de SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) pour plus d’informations.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException a pas été gérée par le code utilisateur

Vérifiez que les paramètres que vous envoyez à vos méthodes n’incluent pas les types non sérialisable (tels que les descripteurs de fichiers ou des connexions de base de données). Si vous avez besoin d’utiliser des membres sur un objet côté serveur que vous ne souhaitez pas être envoyé au client (soit pour la sécurité ou pour des raisons de sérialisation), utilisez le `JSONIgnore` attribut.

### <a name="protocol-error-unknown-transport-error"></a>« Erreur de protocole : transport inconnu « erreur

Cette erreur peut se produire si le client ne prend pas en charge les transports SignalR utilise. Consultez [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports) pour plus d’informations sur lequel les navigateurs peuvent être utilisés avec SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>« Génération de proxy JavaScript Hub a été désactivée. »

Cette erreur se produira si `DisableJavaScriptProxies` est définie durant l’inclusion également une référence au proxy généré dynamiquement à `signalr/hubs`. Pour plus d’informations sur la création du proxy manuellement, consultez [le proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>« L’ID de connexion est dans un format incorrect » ou « l’identité de l’utilisateur ne peut pas changer pendant une connexion SignalR active » erreur

Cette erreur peut se produire si l’authentification est utilisée, et que le client est déconnecté avant l’arrêt de la connexion. La solution consiste à arrêter la connexion SignalR avant de déconnecter le client.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>« Non interceptée erreur : SignalR : jQuery introuvable. Erreur « Vérifiez que jQuery est référencé avant le fichier SignalR.js »

Le client SignalR JavaScript nécessite jQuery à exécuter. Vérifiez que votre référence à jQuery est correct, que le chemin d’accès utilisé est valide et que la référence à jQuery est avant la référence à SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>« Non interceptée TypeError : ne peut pas lire de propriété '&lt;propriété&gt;« undefined » erreur

Cette erreur se produit à partir de n’ayant ne pas jQuery ou le proxy de hubs convenablement référencées. Vérifiez que votre référence à jQuery et le proxy de hubs est correct, que le chemin d’accès utilisé est valide et que la référence à jQuery est avant la référence au proxy de hubs. La référence par défaut pour le proxy de hubs doit se présenter comme suit :

**Code côté client HTML qui référence correctement le proxy de Hubs**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Erreur « RuntimeBinderException a pas été gérée par le code utilisateur »

Cette erreur peut se produire lorsque la surcharge incorrecte de `Hub.On` est utilisé. Si la méthode a une valeur de retour, le type de retour doit être spécifié en tant que paramètre de type générique :

**Méthode définie sur le client (sans proxy généré)**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>ID de connexion est incohérent ou connexion sauts entre les chargements de page

Ce comportement est inhérent à la conception. Étant donné que le hub est hébergé dans l’objet de la page, le concentrateur est détruit lorsque la page est actualisée. Une application MultiPage doit maintenir des associations entre les utilisateurs et les ID de connexion afin qu’elles seront cohérentes entre les chargements de page. L’ID de connexion peuvent être stockée sur le serveur dans l’un `ConcurrentDictionary` objet ou une base de données.

### <a name="value-cannot-be-null-error"></a>Erreur « La valeur ne peut pas être null »

Les méthodes côté serveur avec des paramètres facultatifs ne sont pas actuellement pris en charge ; Si le paramètre facultatif est omis, la méthode échoue. Pour plus d’informations, consultez [paramètres facultatifs](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>« Firefox ne peut pas établir une connexion au serveur à &lt;adresse&gt;« erreur dans le Firebug

Ce message d’erreur peut être constaté dans le Firebug si la négociation du transport WebSocket échoue et un autre transport est utilisé à la place. Ce comportement est inhérent à la conception.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>« Le certificat distant n’est pas valide selon la procédure de validation » une erreur dans l’application cliente .NET

Si votre serveur requiert des certificats de client personnalisé, vous pouvez ajouter un x509certificate à la connexion avant que la demande est effectuée. Ajouter le certificat à la connexion à l’aide `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Connexion supprime après l’expiration de l’authentification

Ce comportement est inhérent à la conception. Informations d’identification d’authentification ne peut pas être modifiées lorsqu’une connexion est active ; Pour actualiser les informations d’identification, la connexion doit être arrêtée et redémarrée.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected est appelée deux fois lors de l’utilisation de jQuery Mobile

jQuery Mobile `initializePage` fonction force les scripts dans chaque page d’être exécutée à nouveau, créant ainsi une deuxième connexion. Solutions pour ce problème sont les suivantes :

- Inclure la référence à jQuery Mobile avant votre fichier JavaScript.
- Désactiver la `initializePage` fonction en définissant `$.mobile.autoInitializePage = false`.
- Attendez que la page pour terminer l’initialisation avant de démarrer la connexion.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Les messages sont retardées dans les applications Silverlight à l’aide d’événements envoyés du serveur

Les messages sont différées quand à l’aide du serveur d’envoi des événements sur Silverlight. Pour forcer l’interrogation pour être utilisé à la place longue, utilisez la commande suivante lors du démarrage de la connexion :

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>À l’aide de « Permission Denied » Frame indéfiniment de protocole

Il s’agit d’un problème connu, décrit [ici](https://github.com/SignalR/SignalR/issues/1963). Ce problème peut se produire à l’aide de la dernière bibliothèque JQuery ; la solution de contournement consiste à mettre à niveau votre application à JQuery 1.8.2.

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>« InvalidOperationException : pas une demande de socket web valide.

Cette erreur peut se produire si le protocole WebSocket est utilisé, mais que le proxy réseau modifie les en-têtes de demande. La solution consiste à configurer le serveur proxy pour autoriser WebSocket sur le port 80.

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>« Exception : &lt;nom de la méthode&gt; méthode n’a pas pu être résolue » lorsque client appelle la méthode sur le serveur

Cette erreur peut être dû à l’aide des types de données qui ne peut pas être détectés dans une charge utile JSON, tels que tableau. La solution de contournement consiste à utiliser un type de données qui peuvent être découvertes par JSON, par exemple IList. Pour plus d’informations, consultez [Client .NET Impossible d’appeler des méthodes de concentrateur avec les paramètres de tableau](https://github.com/SignalR/SignalR/issues/2672).

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Erreurs de compilation et côté serveur

 La section suivante contient des solutions possibles au compilateur et les erreurs d’exécution du côté serveur. 

### <a name="reference-to-hub-instance-is-null"></a>Référence à l’instance de concentrateur a la valeur null

Dans la mesure où une instance de concentrateur est créée pour chaque connexion, Impossible de créer une instance d’un concentrateur dans votre code vous-même. Pour appeler des méthodes sur un concentrateur à l’extérieur du hub lui-même, consultez [comment appeler des méthodes de client et de gérer des groupes extérieurs à la classe de concentrateur](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) pour savoir comment obtenir une référence pour le contexte du concentrateur.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session a la valeur null

Ce comportement est inhérent à la conception. SignalR ne prend pas en charge l’état de session ASP.NET, dans la mesure où l’activation de l’état de session ne fonctionneraient pas la messagerie duplex.

### <a name="no-suitable-method-to-override"></a>Aucune méthode appropriée pour remplacer

Vous pouvez voir cette erreur si vous utilisez le code à partir de la documentation plus ancienne ou blogs. Vérifiez que vous faites référence pas les noms des méthodes qui ont été modifiés ou obsolètes (comme `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl a la valeur null

Ce comportement est inhérent à la conception. Ce membre est déconseillé et ne doit pas être utilisé.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Erreur « un itinéraire nommé « signalr.hubs » est déjà dans la collection de routes »

Cette erreur se produira si `MapSignalR` est appelée deux fois par votre application. Certaines applications exemple d’appel `MapSignalR` directement dans la classe de démarrage ; d’autres effectuer l’appel dans une classe wrapper. Assurez-vous que votre application n’effectue pas les deux.

### <a name="websocket-is-not-used"></a>WebSocket n’est pas utilisé.

Si vous avez vérifié que votre serveur et les clients répondent à la configuration requise pour le WebSocket (répertoriées dans le [plateformes prises en charge](../getting-started/supported-platforms.md) document), vous devrez activer WebSocket sur votre serveur. Instructions pour effectuer cette opération se trouvent [ici](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

### <a name="connection-is-undefined"></a>$.connection n’est pas défini

Cette erreur indique que les scripts sur une page ne sont pas chargés correctement, ou le concentrateur proxy n’est pas accessible ou est accédé incorrectement. Vérifiez que les références de script sur votre page correspondent aux scripts chargés dans votre projet, et que /signalr/hubs accessibles dans un navigateur lorsque le serveur est en cours d’exécution.

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>Impossible de trouver un ou plusieurs des types requis pour compiler une expression dynamique

Cette erreur indique que le `Microsoft.CSharp` bibliothèque est manquante. Ajoutez-le dans le **assemblys -&gt;Framework** onglet.

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>État de l’appelant n’est pas accessible à partir de Clients.Caller dans Visual Basic ou dans un concentrateur fortement typé ; Erreur « La conversion de type 'Task (Of Object)' en type 'String' n’est pas valide »

Pour accéder à l’état de l’appelant dans Visual Basic ou dans un concentrateur fortement typé, vous devez utiliser le `Clients.CallerState` propriété (introduite dans SignalR 2.1) au lieu de `Clients.Caller`.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problèmes de Visual Studio

Cette section décrit les problèmes rencontrés dans Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Nœud de Documents de script n’apparaît pas dans l’Explorateur de solutions

Certains de nos didacticiels vous permet de vous diriger vers le nœud « Documents de Script » dans l’Explorateur de solutions pendant le débogage. Ce nœud est généré par le débogueur JavaScript et ne s’affiche que pendant le débogage des clients de navigateur dans Internet Explorer ; le nœud n’apparaîtra pas si Chrome ou Firefox est utilisé. Le débogueur JavaScript également exécutera pas si un autre débogueur client est en cours d’exécution, telle que le débogueur Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR ne fonctionne pas sur Visual Studio 2008 ou une version antérieure

Ce comportement est inhérent à la conception. SignalR nécessite .NET Framework 4 ou version ultérieure ; Cela nécessite que les applications de SignalR être développées dans Visual Studio 2010 ou version ultérieure. Le composant serveur de SignalR nécessite .NET Framework 4.5.

<a id="iis"></a>

## <a name="iis-issues"></a>Problèmes avec IIS

Cette section contient des problèmes avec Internet Information Services.

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>SignalR fonctionne sur le serveur de développement Visual Studio, mais pas dans IIS

SignalR est pris en charge sur IIS 7.0 et 7.5, mais la prise en charge des URL sans extension doivent être ajoutés. Pour ajouter la prise en charge des URL sans extension, consultez [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR nécessite ASP.NET doit être installé sur le serveur (ASP.NET n'est pas installé sur IIS par défaut). Pour installer ASP.NET, consultez [téléchargements ASP.NET](https://www.asp.net/downloads).

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Problèmes de Microsoft Azure

Cette section contient des problèmes avec Microsoft Azure.

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException lors de l’hébergement SignalR dans un rôle de travail Azure

Hébergement SignalR dans un rôle de travail Azure peut entraîner l’exception « Impossible de charger le fichier ou l’assembly ' Microsoft.Owin, Version = 2.0.0.0 ». Il s’agit d’un problème connu avec NuGet ; Redirections de liaison ne sont pas ajoutées automatiquement dans les projets de rôle de travail Azure. Pour résoudre ce problème, vous pouvez ajouter manuellement les redirections de liaison. Ajoutez les lignes suivantes à la `app.config` fichier de votre projet de rôle de travail.

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Les messages ne sont pas reçus via l’infrastructure d’intégration Azure après la modification des noms de rubrique

Les rubriques utilisés par l’infrastructure d’intégration Azure sont conservées en interne ; ils ne sont pas destinés à être configurables par l’utilisateur.
