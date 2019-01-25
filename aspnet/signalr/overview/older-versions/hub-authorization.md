---
uid: signalr/overview/older-versions/hub-authorization
title: Authentification et autorisation pour SignalR Hubs (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7f4a76109111f19dc4381ad01e642afdabade336
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836897"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Authentification et autorisation pour SignalR Hubs (SignalR 1.x)
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur.


## <a name="overview"></a>Vue d'ensemble

Cette rubrique contient les sections suivantes :

- [Autoriser l’attribut](#authorizeattribute)
- [Exiger l’authentification pour tous les concentrateurs](#requireauth)
- [D’autorisation personnalisée](#custom)
- [Passer des informations d’authentification pour les clients](#passauth)
- [Options d’authentification pour les clients .NET](#authoptions)

    - [Cookie avec l’authentification par formulaire](#cookie)
    - [Authentification Windows](#windows)
    - [En-tête de connexion](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autoriser l’attribut

SignalR fournit le [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribut pour spécifier quels utilisateurs ou les rôles ont accès à une méthode ou le hub. Cet attribut se trouve dans le `Microsoft.AspNet.SignalR` espace de noms. Vous appliquez le `Authorize` d’attribut à un concentrateur ou des méthodes particulières dans un concentrateur. Lorsque vous appliquez le `Authorize` attribut à une classe de concentrateur, l’exigence d’autorisation spécifié est appliqué à toutes les méthodes dans le hub. Les différents types de conditions d’autorisation que vous pouvez appliquer sont présentées ci-dessous. Sans le `Authorize` attribut, toutes les méthodes publiques sur le hub sont disponibles pour un client qui est connecté au concentrateur.

Si vous avez défini un rôle nommé « Admin » dans votre application web, vous pouvez spécifier que seuls les utilisateurs de ce rôle peuvent accéder à un hub par le code suivant.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Ou bien, vous pouvez spécifier qu’un concentrateur contient une méthode qui est disponible pour tous les utilisateurs et une deuxième méthode qui est uniquement disponible pour les utilisateurs authentifiés, comme indiqué ci-dessous.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Les exemples suivants d’autorisation différents scénarios :

- `[Authorize]` : seuls les utilisateurs authentifiés
- `[Authorize(Roles = "Admin,Manager")]` : seuls les utilisateurs aux rôles spécifiés authentifiés
- `[Authorize(Users = "user1,user2")]` : seuls les utilisateurs avec des noms d’utilisateurs spécifiés authentifiés
- `[Authorize(RequireOutgoing=false)]` – les utilisateurs authentifiés peuvent appeler le concentrateur, mais les appels à partir du serveur aux clients ne sont pas limités par autorisation, telles que, lorsque seuls certains utilisateurs peuvent envoyer un message, mais tous les autres utilisateurs peuvent recevoir le message. La RequireOutgoing propriété peut uniquement être appliquée à l’ensemble du hub, pas sur les méthodes de personnes dans le hub. Lorsque RequireOutgoing n’est pas définie sur false, seuls les utilisateurs qui répond à la condition d’autorisation sont appelés à partir du serveur.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Exiger l’authentification pour tous les concentrateurs

Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant le [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) méthode lorsque l’application démarre. Vous pouvez utiliser cette méthode lorsque vous avez plusieurs concentrateurs et que vous souhaitez appliquer une exigence d’authentification pour toutes les. Avec cette méthode, vous ne pouvez pas spécifier de rôle, utilisateur ou l’autorisation sortante. Vous ne pouvez spécifier qu’accès aux méthodes de concentrateur l'est limité aux utilisateurs authentifiés. Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux hubs ou des méthodes pour spécifier des exigences supplémentaires. Toute condition que vous spécifiez dans les attributs est appliquée en plus de l’exigence d’authentification de base.

L’exemple suivant montre un fichier Global.asax, ce qui limite toutes les méthodes de concentrateur aux utilisateurs authentifiés.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Si vous appelez le `RequireAuthentication()` méthode après le traitement d’une requête SignalR, SignalR lève un `InvalidOperationException` exception. Cette exception est levée, car vous ne pouvez pas ajouter un module à HubPipeline après que le pipeline a été appelé. L’exemple précédent illustre l’appel la `RequireAuthentication` méthode dans le `Application_Start` méthode qui est exécutée une fois avant la première demande.

<a id="custom"></a>

## <a name="customized-authorization"></a>D’autorisation personnalisée

Si vous avez besoin personnaliser la façon dont l’autorisation est définie, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et remplacer le [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) (méthode). Cette méthode est appelée pour chaque demande afin de déterminer si l’utilisateur est autorisé à effectuer la demande. Dans la méthode substituée, vous fournissez la logique nécessaire pour votre scénario d’autorisation. L’exemple suivant montre comment appliquer l’autorisation via l’identité basée sur les revendications.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Passer des informations d’authentification pour les clients

Vous devrez peut-être utiliser les informations d’authentification dans le code qui s’exécute sur le client. Vous transmettez les informations requises lors de l’appel des méthodes sur le client. Par exemple, une méthode d’application de conversation peut passer comme paramètre le nom d’utilisateur de la personne à publier un message, comme indiqué ci-dessous.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Ou bien, vous pouvez créer un objet pour représenter les informations d’authentification et passez cet objet en tant que paramètre, comme indiqué ci-dessous.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Vous ne devez jamais passer les id de connexion d’un client à d’autres clients, car un utilisateur malveillant pourrait utiliser pour simuler une demande à partir de ce client.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Options d’authentification pour les clients .NET

Lorsque vous avez un client .NET, par exemple une application console, qui interagit avec un concentrateur est limité aux utilisateurs authentifiés, vous pouvez passer les informations d’identification de l’authentification dans un cookie, l’en-tête de connexion ou un certificat. Les exemples de cette section montrent comment utiliser ces méthodes différentes pour authentifier un utilisateur. Ils ne sont pas des applications SignalR entièrement fonctionnelles. Pour plus d’informations sur les clients .NET avec SignalR, consultez [Guide de l’API Hubs - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Quand votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion. Vous ajoutez le cookie à la `CookieContainer` propriété sur le [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objet. L’exemple suivant montre une application console qui Récupère un cookie d’authentification à partir d’une page web et ajoute ce cookie à la connexion. L’URL `https://www.contoso.com/RemoteLogin` dans l’exemple pointe vers une page web que vous aurez à créer. La page voulez-vous récupérer le nom d’utilisateur validée et le mot de passe et tentent de se connecter l’utilisateur avec les informations d’identification.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

L’application de console publie les informations d’identification à www.contoso.com/RemoteLogin qui peut faire référence à une page vide qui contient le fichier code-behind suivant.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Authentification Windows

Lorsque vous utilisez l’authentification Windows, vous pouvez passer des informations d’identification de l’utilisateur actuel à l’aide de la [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) propriété. Vous définissez les informations d’identification pour la connexion à la valeur de DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>En-tête de connexion

Si votre application n’utilise pas les cookies, vous pouvez passer des informations utilisateur dans l’en-tête de connexion. Par exemple, vous pouvez passer un jeton dans l’en-tête de connexion.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Puis, dans le hub, vérifiez que le jeton d’utilisateur.

<a id="certificate"></a>

### <a name="certificate"></a>Certificat

Vous pouvez transmettre un certificat client pour vérifier que l’utilisateur. Vous ajoutez le certificat lors de la création de la connexion. L’exemple suivant montre uniquement comment ajouter un certificat client à la connexion ; Il n’affiche pas l’application console complète. Il utilise le [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe qui fournit différentes façons de créer le certificat.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
