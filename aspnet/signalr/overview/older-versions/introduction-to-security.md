---
uid: signalr/overview/older-versions/introduction-to-security
title: Introduction à la sécurité de SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Décrit les problèmes de sécurité que vous devez prendre en compte lorsque vous développez une application de SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: b50b96bd4d1bf84c431f53f18747fe26a629d98a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370508"
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Introduction à la sécurité de SignalR (SignalR 1.x)
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit les problèmes de sécurité que vous devez prendre en compte lorsque vous développez une application de SignalR.


## <a name="overview"></a>Vue d'ensemble

Ce document contient les sections suivantes :

- [Concepts de sécurité de SignalR](#concepts)

    - [Authentification et autorisation](#authentication)
    - [Jeton de connexion](#connectiontoken)
    - [Réintégration des groupes lors de la reconnexion](#rejoingroup)
- [Comment SignalR empêche Cross-Site Request Forgery](#csrf)
- [Recommandations de sécurité de SignalR](#recommendations)

    - [Protocole SSL (Socket Layers) sécurisé](#ssl)
    - [N’utilisez pas de groupes en tant que mécanisme de sécurité](#groupsecurity)
    - [Gérer en toute sécurité les entrées à partir de clients](#input)
    - [Rapprocher un changement d’état utilisateur avec une connexion active](#reconcile)
    - [Fichiers de proxy JavaScript générés automatiquement](#autogen)
    - [Exceptions](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Concepts de sécurité de SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Authentification et autorisation

SignalR est conçu pour être intégré à la structure d’authentification existante pour une application. Il ne fournit pas toutes les fonctionnalités d’authentification des utilisateurs. Au lieu de cela, vous authentifier les utilisateurs que vous le feriez normalement dans votre application, puis utiliser les résultats de l’authentification dans votre code de SignalR. Par exemple, vous pouvez authentifier vos utilisateurs avec l’authentification par formulaire ASP.NET et ensuite dans votre hub, appliquer les utilisateurs ou rôles sont autorisés à appeler une méthode. Dans votre hub, vous pouvez également passer des informations d’authentification, telles que le nom d’utilisateur ou si un utilisateur appartient à un rôle, au client.

SignalR fournit le [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribut pour spécifier quels utilisateurs ont accès à une méthode ou le hub. Vous appliquez l’attribut Authorize à un concentrateur ou des méthodes particulières dans un concentrateur. Sans l’attribut Authorize, toutes les méthodes publiques sur le hub sont disponibles pour un client qui est connecté au concentrateur. Pour plus d’informations sur les concentrateurs, consultez [l’authentification et autorisation pour SignalR Hubs](../security/hub-authorization.md).

Le `Authorize` attribut est utilisé uniquement avec les hubs. Pour appliquer des règles d’autorisation lorsque vous utilisez un `PersistentConnection` vous devez substituer la `AuthorizeRequest` (méthode). Pour plus d’informations sur les connexions persistantes, consultez [authentification et autorisation pour les connexions persistantes SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Jeton de connexion

SignalR atténue le risque de l’exécution des commandes nuisibles en validant l’identité de l’expéditeur. Un jeton de connexion, qui contient l’id de connexion et le nom d’utilisateur pour les utilisateurs authentifiés, est transmis entre le client et le serveur pour chaque demande. L’id de connexion est un identificateur unique générée de façon aléatoire par le serveur lorsqu’une nouvelle connexion est créée et est rendue persistante pour la durée de la connexion. Le nom d’utilisateur est fournie par le mécanisme d’authentification pour l’application web. Le jeton de connexion est protégé par chiffrement et une signature numérique.

![](introduction-to-security/_static/image2.png)

Pour chaque demande, le serveur valide le contenu du jeton pour vous assurer que la demande provient de l’utilisateur spécifié. Le nom d’utilisateur doit correspondre à l’id de connexion. En validant l’id de connexion et le nom d’utilisateur, SignalR empêche un utilisateur malveillant de facilement emprunter l’identité d’un autre utilisateur. Si le serveur ne peut pas valider le jeton de connexion, la demande échoue.

![](introduction-to-security/_static/image4.png)

Car l’id de connexion fait partie du processus de vérification, pas révéler l’id de connexion d’un utilisateur à d’autres utilisateurs ou si vous stockez la valeur sur le client, comme dans un cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Réintégration des groupes lors de la reconnexion

Par défaut, l’application SignalR nouveau attribuera automatiquement un utilisateur aux groupes appropriés lors de la reconnexion à partir d’une interruption temporaire, par exemple quand une connexion est supprimée et rétablie avant l’expiration de la connexion. Lors de la reconnexion, le client passe un jeton de groupe qui inclut l’id de connexion et les groupes affectés. Le jeton de groupe est numériquement signé et chiffré. Le client conserve le même id de connexion après une reconnexion ; Par conséquent, l’id de connexion transmise par le client reconnecté doit correspondre à l’id de connexion précédente utilisée par le client. Cette vérification empêche tout utilisateur malveillant de transmettre les demandes d’adhésion des groupes non autorisées lors de la reconnexion.

Toutefois, il est important de noter que le jeton de groupe n’expire pas. Si un utilisateur appartenait à un groupe dans le passé, mais a été interdit à partir de ce groupe, cet utilisateur peut être en mesure de reproduire un jeton de groupe qui inclut le groupe interdit. Si vous avez besoin de gérer en toute sécurité les utilisateurs qui appartiennent à quels groupes, vous devez stocker ces données sur le serveur, comme dans une base de données. Ensuite, ajoutez une logique à votre application qui vérifie sur le serveur si un utilisateur appartient à un groupe. Pour obtenir un exemple de vérification de l’appartenance au groupe, consultez [utilisation de groupes de](../guide-to-the-api/working-with-groups.md).

Réintégration des groupes s’applique uniquement lorsqu’une connexion est reconnectée après une interruption temporaire. Si un utilisateur se déconnecte par la vous obliger à quitter l’application ou le redémarrage de l’application, votre application doit gérer l’ajout de cet utilisateur aux groupes appropriés. Pour plus d’informations, consultez [utilisation de groupes de](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Comment SignalR empêche Cross-Site Request Forgery

Falsification de demande intersites (CSRF) est une attaque où un site malveillant envoie une demande à un site vulnérable, où l’utilisateur est actuellement connecté. SignalR empêche CSRF en le rendant très peu probable d’un site malveillant pour créer une requête valide pour votre application SignalR.

### <a name="description-of-csrf-attack"></a>Description de l’attaque CSRF

Voici un exemple d’une attaque CSRF :

1. Un utilisateur se connecte www.example.com, à l’aide de l’authentification par formulaire.
2. Le serveur authentifie l’utilisateur. La réponse du serveur inclut un cookie d’authentification.
3. Sans la déconnexion, l’utilisateur visite un site web malveillant. Ce site malveillant contient le formulaire HTML suivant : 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Notez que l’action de formulaire valide vers le site vulnérable, pas pour le site malveillant. Il s’agit de la partie « cross-site » de falsification de requête Intersites.
4. L’utilisateur clique sur le bouton Envoyer. Le navigateur inclut le cookie d’authentification avec la demande.
5. La demande s’exécute sur le serveur example.com avec le contexte de l’utilisateur d’authentification et peut faire tout ce qu’un utilisateur authentifié est autorisé à faire.

Bien que cet exemple nécessite l’utilisateur clique sur le bouton du formulaire, la page malveillante pourrait aussi facilement exécuter un script qui envoie une requête AJAX à votre application de SignalR. En outre, à l’aide de SSL n’empêche pas une attaque CSRF, car le site malveillant peut envoyer une demande de « https:// ».

En règle générale, les attaques CSRF sont possibles contre les sites web qui utilisent des cookies pour l’authentification, étant donné que les navigateurs envoient tous les cookies utiles pour le site de destination. Toutefois, les attaques CSRF ne sont pas limités à exploiter les cookies. Par exemple, l’authentification de base et Digest sont également vulnérables. Une fois un utilisateur se connecte avec l’authentification de base ou Digest, le navigateur envoie automatiquement les informations d’identification jusqu'à ce que la session se termine.

### <a name="csrf-mitigations-taken-by-signalr"></a>Atténuations CSRF prises par SignalR

SignalR entreprend les actions suivantes afin d’éviter un site malveillant à partir de la création de demandes de valides à votre application de SignalR. Ces étapes sont effectuées par défaut et ne nécessitent pas d’action dans votre code.

- **Désactiver des requêtes entre domaines**  
 Par défaut, inter-domaines les demandes sont désactivés dans une application de SignalR pour empêcher les utilisateurs à partir de l’appel d’un point de terminaison SignalR à partir d’un domaine externe. Toute demande qui provient d’un domaine externe est automatiquement considérée comme non valide et est bloqué. Il est recommandé de conserver ce comportement par défaut ; Sinon, un site malveillant pourrait tromper les utilisateurs d’envoyer des commandes à votre site. Si vous avez besoin d’utiliser des requêtes entre domaines, consultez [comment établir une connexion entre domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passer un jeton de connexion dans la chaîne de requête, pas de cookie**  
 SignalR transmet le jeton de connexion en tant que valeur de chaîne de requête, plutôt que sous forme de cookie. En ne stockant ne pas le jeton de connexion en tant que cookie, le jeton de connexion n’est pas par inadvertance transféré par le navigateur lorsqu’un code malveillant est détecté. En outre, le jeton de connexion n’est pas conservé au-delà de la connexion actuelle. Par conséquent, un utilisateur malveillant ne peut pas effectuer une requête avec informations d’identification de l’authentification d’un autre utilisateur.
- **Vérifier le jeton de connexion**  
 Comme décrit dans la [jeton de connexion](#connectiontoken) section, le serveur sait quels id de connexion est associé à chaque utilisateur authentifié. Le serveur ne traite pas de toute demande d’un id de connexion qui ne correspond pas le nom d’utilisateur. Il est peu probable qu'un utilisateur malveillant permettant de deviner une requête valide, car l’utilisateur malveillant devra connaître le nom d’utilisateur et l’id généré de façon aléatoire la connexion actuelle. Cet id de connexion n’est plus valide dès que la connexion est terminée. Les utilisateurs anonymes ne doivent pas avoir accès à toutes les informations sensibles.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recommandations de sécurité de SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocole SSL (Socket Layers) sécurisé

Le protocole SSL utilise le chiffrement pour sécuriser le transport des données entre un client et le serveur. Si votre application SignalR transmet des informations sensibles entre le client et le serveur, utilisez SSL pour le transport. Pour plus d’informations sur la configuration de SSL, consultez [comment configurer SSL sur IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>N’utilisez pas de groupes en tant que mécanisme de sécurité

Les groupes sont un moyen pratique de collecter les utilisateurs liés, mais ils ne sont pas un mécanisme sécurisé pour limiter l’accès à des informations sensibles. Cela est particulièrement vrai lorsque les utilisateurs peuvent rejoindre automatiquement les groupes pendant une reconnexion. Au lieu de cela, envisagez d’ajout d’utilisateurs disposant de privilèges à un rôle et en limitant l’accès à une méthode de concentrateur que les membres de ce rôle. Pour obtenir un exemple de limiter l’accès basé sur un rôle, consultez [l’authentification et autorisation pour SignalR Hubs](../security/hub-authorization.md). Pour obtenir un exemple de vérification de l’accès utilisateur aux groupes lors de la reconnexion, consultez [utilisation de groupes de](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Gérer en toute sécurité les entrées à partir de clients

Toutes les entrées à partir de clients qui sont prévue pour la diffusion à d’autres clients doivent être codées pour vous assurer qu’un utilisateur malveillant n’envoie pas de script à d’autres utilisateurs. Il est préférable encoder les messages sur les clients de réception plutôt que sur le serveur, car votre application SignalR peut-être avoir différents types de clients. Par conséquent, l’encodage HTML fonctionne pour un client web, mais pas pour d’autres types de clients. Par exemple, une méthode de client web pour afficher un message de conversation serait gérer en toute sécurité le nom d’utilisateur et le message en appelant le `html()` (fonction).

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Rapprocher un changement d’état utilisateur avec une connexion active

Si l’état d’un utilisateur change alors qu’une connexion active existe, l’utilisateur recevra une erreur indiquant que, « l’identité de l’utilisateur ne peut pas modifier pendant une connexion SignalR active ». Dans ce cas, votre application doit reconnecter au serveur pour vous assurer que le nom d’utilisateur et un id de connexion sont coordonnées. Par exemple, si votre application permet à l’utilisateur à se déconnecter, alors qu’une connexion active existe, le nom d’utilisateur pour la connexion ne correspondront plus le nom qui est passé pour la demande suivante. Vous souhaitez arrêter la connexion avant que l’utilisateur se déconnecte et puis redémarrez-le.

Toutefois, il est important de noter que la plupart des applications ne devrez pas arrêter et démarrer la connexion manuellement. Si votre application redirige les utilisateurs vers une page séparée après la connexion, tels que le comportement par défaut dans une application Web Forms ou MVC ou actualise la page active après la déconnexion, la connexion active est automatiquement déconnectée et pas nécessitent aucune action supplémentaire.

L’exemple suivant montre comment arrêter et démarrer une connexion lorsque l’état de l’utilisateur a changé.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Ou bien, l’état d’authentification de l’utilisateur peut changer si votre site utilise expiration décalée avec l’authentification par formulaire, et il n’existe aucune activité pour conserver le cookie d’authentification valide. Dans ce cas, l’utilisateur sera déconnecté et le nom d’utilisateur ne correspondront plus le nom d’utilisateur dans le jeton de connexion. Vous pouvez résoudre ce problème en ajoutant un script qui demande périodiquement une ressource sur le serveur web pour conserver le cookie d’authentification valide. L’exemple suivant montre comment demander une ressource toutes les 30 minutes.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Fichiers de proxy JavaScript générés automatiquement

Si vous ne souhaitez pas inclure tous les concentrateurs et méthodes dans le fichier de proxy JavaScript pour chaque utilisateur, vous pouvez désactiver la génération automatique du fichier. Vous pouvez choisir cette option si vous disposez de plusieurs concentrateurs et méthodes, mais que vous ne souhaitez pas que tous les utilisateurs de connaître toutes les méthodes. Vous désactivez la génération automatique en définissant **EnableJavaScriptProxies** à **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Pour plus d’informations sur les fichiers de proxy JavaScript, consultez [le proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Exceptions

Vous devez éviter de passer des objets d’exception aux clients, car les objets peuvent exposer des informations sensibles aux clients. Au lieu de cela, appelez une méthode sur le client qui affiche le message d’erreur pertinents.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
