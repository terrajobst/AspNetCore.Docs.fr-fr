---
title: Authentification et autorisation dans ASP.NET Core SignalR
author: rachelappel
description: Découvrez comment utiliser l’authentification et autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: 32e5fcf2fd3f888e0e131fa47bd9a74eede3c26d
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028461"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Authentification et autorisation dans ASP.NET Core SignalR

Par [Andrew Stanton-Nurse](https://twitter.com/anurse)

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Authentifier les utilisateurs qui se connectent à un concentrateur SignalR

SignalR peut être utilisé avec [ASP.NET Core authentification](xref:security/authentication/index) pour associer un utilisateur à chaque connexion. Dans un concentrateur, données d’authentification sont accessibles à partir de la [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) propriété. L’authentification permet le hub appeler des méthodes sur toutes les connexions associées à un utilisateur (voir [gérer les utilisateurs et groupes dans SignalR](xref:signalr/groups) pour plus d’informations). Plusieurs connexions peuvent être associées à un seul utilisateur.

### <a name="cookie-authentication"></a>Authentification des cookies

Dans une application basée sur navigateur, l’authentification par cookie permet à vos informations d’identification utilisateur existantes à circuler automatiquement vers les connexions SignalR. Lorsque vous utilisez le navigateur client, aucune configuration supplémentaire est nécessaire. Si l’utilisateur est connecté à votre application, la connexion SignalR hérite automatiquement de cette authentification.

L’authentification par cookie n’est pas recommandée, sauf si l’application a uniquement besoin d’authentifier les utilisateurs à partir du client de navigateur. Lorsque vous utilisez le [Client .NET](xref:signalr/dotnet-client), le `Cookies` propriété peut être configurée dans le `.WithUrl` appel afin de fournir un cookie. Toutefois, à l’aide de l’authentification des cookies à partir du Client .NET nécessite l’application de fournir une API pour échanger des données de l’authentification d’un cookie.

### <a name="bearer-token-authentication"></a>Authentification du jeton du porteur

Authentification du jeton du porteur est l’approche recommandée lors de l’utilisation de clients autres que le navigateur client. Dans cette approche, le client fournit un jeton d’accès que le serveur valide et qu’il utilise pour identifier l’utilisateur. Les détails de l’authentification du jeton du porteur sont dépasse le cadre de ce document. Sur le serveur, l’authentification du jeton du porteur est configurée à l’aide de la [intergiciel (middleware) du porteur JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

Dans le client JavaScript, le jeton peut être fourni à l’aide de la [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

Dans le client .NET, il revient un [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) propriété qui peut être utilisée pour configurer le jeton :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => _myAccessToken;
    })
    .Build();
```

> [!NOTE]
> La fonction jeton d’accès que vous fournissez est appelée avant **chaque** demande HTTP adressée par SignalR. Si vous devez renouveler le jeton afin de maintenir la connexion active (car il peut expirer pendant la connexion), le faire à partir de cette fonction et retourner le jeton mis à jour.

Dans l’API web standard, les jetons du porteur sont envoyés dans un en-tête HTTP. Toutefois, SignalR est impossible de définir ces en-têtes dans les navigateurs lors de l’utilisation de certains transports. Lorsque vous utilisez le protocole WebSocket et les événements, le jeton est transmis comme paramètre de chaîne de requête. Pour prendre en charge cela sur le serveur, une configuration supplémentaire est requise :

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?range=33-34,42-80,90)]

### <a name="windows-authentication"></a>Authentification Windows

Si [l’authentification Windows](xref:security/authentication/windowsauth) est configuré dans votre application, SignalR permettre utiliser cette identité pour sécuriser les hubs. Toutefois, pour pouvoir envoyer des messages à des utilisateurs individuels, vous devez ajouter un fournisseur d’ID d’utilisateur personnalisé. Il s’agit, car le système d’authentification Windows ne fournit pas la revendication de « Identificateur de nom » SignalR utilise pour déterminer le nom d’utilisateur.

Ajoutez une nouvelle classe qui implémente `IUserIdProvider` et récupérer une des revendications à partir de l’utilisateur à utiliser comme identificateur. Par exemple, pour utiliser la revendication « Name » (qui est le nom d’utilisateur Windows sous la forme `[Domain]\[Username]`), créez la classe suivante :

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

Au lieu de `ClaimTypes.Name`, vous pouvez utiliser n’importe quelle valeur à partir de la `User` (telles que l’identificateur SID de Windows, etc.).

> [!NOTE]
> La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système. Sinon, un message destiné à un utilisateur pourrait aboutiront à un autre utilisateur.

Enregistrer ce composant dans votre `Startup.ConfigureServices` méthode **après** l’appel à `.AddSignalR`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Autoriser les utilisateurs à des hubs d’accès et les méthodes de concentrateur

Par défaut, toutes les méthodes dans un concentrateur peuvent être appelées par un utilisateur non authentifié. Pour exiger l’authentification, appliquer la [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribut au hub :

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Vous pouvez utiliser les arguments de constructeur et les propriétés de la `[Authorize]` attribut pour restreindre l’accès aux seuls utilisateurs correspondance spécifique [stratégies d’autorisation](xref:security/authorization/policies). Par exemple, si vous avez une stratégie d’autorisation personnalisée appelée `MyAuthorizationPolicy` vous pouvez vous assurer que seuls les utilisateurs correspondant à cette stratégie peuvent accéder à du hub en utilisant le code suivant :

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

Méthodes de concentrateur individuel peuvent avoir la `[Authorize]` attribut également appliqué. Si l’utilisateur actuel ne correspond pas à la stratégie appliquée à la méthode, une erreur est retournée à l’appelant :

```csharp
[Authorize]
public class ChatHub: Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```
