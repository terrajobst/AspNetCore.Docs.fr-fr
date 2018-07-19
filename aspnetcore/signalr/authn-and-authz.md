---
title: Authentification et autorisation dans ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment utiliser l’authentification et autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: fceae37ce53a0d5a219e6dc466e9cc6df0277494
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123770"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="cfd20-103">Authentification et autorisation dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="cfd20-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="cfd20-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="cfd20-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="cfd20-105">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="cfd20-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="cfd20-106">Authentifier les utilisateurs qui se connectent à un concentrateur SignalR</span><span class="sxs-lookup"><span data-stu-id="cfd20-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="cfd20-107">SignalR peut être utilisé avec [ASP.NET Core authentification](xref:security/authentication/index) pour associer un utilisateur à chaque connexion.</span><span class="sxs-lookup"><span data-stu-id="cfd20-107">SignalR can be used with [ASP.NET Core Authentication](xref:security/authentication/index) to associate a user with each connection.</span></span> <span data-ttu-id="cfd20-108">Dans un concentrateur, données d’authentification sont accessibles à partir de la [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) propriété.</span><span class="sxs-lookup"><span data-stu-id="cfd20-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="cfd20-109">L’authentification permet le hub appeler des méthodes sur toutes les connexions associées à un utilisateur (voir [gérer les utilisateurs et groupes dans SignalR](xref:signalr/groups) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="cfd20-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="cfd20-110">Plusieurs connexions peuvent être associées à un seul utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cfd20-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="cfd20-111">Authentification des cookies</span><span class="sxs-lookup"><span data-stu-id="cfd20-111">Cookie authentication</span></span>

<span data-ttu-id="cfd20-112">Dans une application basée sur navigateur, l’authentification par cookie permet à vos informations d’identification utilisateur existantes à circuler automatiquement vers les connexions SignalR.</span><span class="sxs-lookup"><span data-stu-id="cfd20-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="cfd20-113">Lorsque vous utilisez le navigateur client, aucune configuration supplémentaire est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="cfd20-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="cfd20-114">Si l’utilisateur est connecté à votre application, la connexion SignalR hérite automatiquement de cette authentification.</span><span class="sxs-lookup"><span data-stu-id="cfd20-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="cfd20-115">L’authentification par cookie n’est pas recommandée, sauf si l’application a uniquement besoin d’authentifier les utilisateurs à partir du client de navigateur.</span><span class="sxs-lookup"><span data-stu-id="cfd20-115">Cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="cfd20-116">Lorsque vous utilisez le [Client .NET](xref:signalr/dotnet-client), le `Cookies` propriété peut être configurée dans le `.WithUrl` appel afin de fournir un cookie.</span><span class="sxs-lookup"><span data-stu-id="cfd20-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="cfd20-117">Toutefois, à l’aide de l’authentification des cookies à partir du Client .NET nécessite l’application de fournir une API pour échanger des données de l’authentification d’un cookie.</span><span class="sxs-lookup"><span data-stu-id="cfd20-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="cfd20-118">Authentification du jeton du porteur</span><span class="sxs-lookup"><span data-stu-id="cfd20-118">Bearer token authentication</span></span>

<span data-ttu-id="cfd20-119">Authentification du jeton du porteur est l’approche recommandée lors de l’utilisation de clients autres que le navigateur client.</span><span class="sxs-lookup"><span data-stu-id="cfd20-119">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span> <span data-ttu-id="cfd20-120">Dans cette approche, le client fournit un jeton d’accès que le serveur valide et qu’il utilise pour identifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cfd20-120">In this approach, the client provides an access token that the server validates and uses to identify the user.</span></span> <span data-ttu-id="cfd20-121">Les détails de l’authentification du jeton du porteur sont dépasse le cadre de ce document.</span><span class="sxs-lookup"><span data-stu-id="cfd20-121">The details of bearer token authentication are beyond the scope of this document.</span></span> <span data-ttu-id="cfd20-122">Sur le serveur, l’authentification du jeton du porteur est configurée à l’aide de la [intergiciel (middleware) du porteur JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="cfd20-122">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="cfd20-123">Dans le client JavaScript, le jeton peut être fourni à l’aide de la [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span><span class="sxs-lookup"><span data-stu-id="cfd20-123">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="cfd20-124">Dans le client .NET, il revient un [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) propriété qui peut être utilisée pour configurer le jeton :</span><span class="sxs-lookup"><span data-stu-id="cfd20-124">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => _myAccessToken;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="cfd20-125">La fonction jeton d’accès que vous fournissez est appelée avant **chaque** demande HTTP adressée par SignalR.</span><span class="sxs-lookup"><span data-stu-id="cfd20-125">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="cfd20-126">Si vous devez renouveler le jeton afin de maintenir la connexion active (car il peut expirer pendant la connexion), le faire à partir de cette fonction et retourner le jeton mis à jour.</span><span class="sxs-lookup"><span data-stu-id="cfd20-126">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="cfd20-127">Dans l’API web standard, les jetons du porteur sont envoyés dans un en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="cfd20-127">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="cfd20-128">Toutefois, SignalR est impossible de définir ces en-têtes dans les navigateurs lors de l’utilisation de certains transports.</span><span class="sxs-lookup"><span data-stu-id="cfd20-128">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="cfd20-129">Lorsque vous utilisez le protocole WebSocket et les événements, le jeton est transmis comme paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="cfd20-129">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="cfd20-130">Pour prendre en charge cela sur le serveur, une configuration supplémentaire est requise :</span><span class="sxs-lookup"><span data-stu-id="cfd20-130">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="windows-authentication"></a><span data-ttu-id="cfd20-131">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="cfd20-131">Windows authentication</span></span>

<span data-ttu-id="cfd20-132">Si [l’authentification Windows](xref:security/authentication/windowsauth) est configuré dans votre application, SignalR permettre utiliser cette identité pour sécuriser les hubs.</span><span class="sxs-lookup"><span data-stu-id="cfd20-132">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="cfd20-133">Toutefois, pour pouvoir envoyer des messages à des utilisateurs individuels, vous devez ajouter un fournisseur d’ID d’utilisateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="cfd20-133">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="cfd20-134">Il s’agit, car le système d’authentification Windows ne fournit pas la revendication de « Identificateur de nom » SignalR utilise pour déterminer le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cfd20-134">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="cfd20-135">Ajoutez une nouvelle classe qui implémente `IUserIdProvider` et récupérer une des revendications à partir de l’utilisateur à utiliser comme identificateur.</span><span class="sxs-lookup"><span data-stu-id="cfd20-135">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="cfd20-136">Par exemple, pour utiliser la revendication « Name » (qui est le nom d’utilisateur Windows sous la forme `[Domain]\[Username]`), créez la classe suivante :</span><span class="sxs-lookup"><span data-stu-id="cfd20-136">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="cfd20-137">Au lieu de `ClaimTypes.Name`, vous pouvez utiliser n’importe quelle valeur à partir de la `User` (telles que l’identificateur SID de Windows, etc.).</span><span class="sxs-lookup"><span data-stu-id="cfd20-137">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="cfd20-138">La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système.</span><span class="sxs-lookup"><span data-stu-id="cfd20-138">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="cfd20-139">Sinon, un message destiné à un utilisateur pourrait aboutiront à un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cfd20-139">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="cfd20-140">Enregistrer ce composant dans votre `Startup.ConfigureServices` méthode **après** l’appel à `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="cfd20-140">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="cfd20-141">Dans le Client .NET, l’authentification Windows doit être activée en définissant le [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) propriété :</span><span class="sxs-lookup"><span data-stu-id="cfd20-141">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="cfd20-142">L’authentification Windows est uniquement pris en charge par le client de navigateur à l’aide de Microsoft Internet Explorer ou Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="cfd20-142">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="cfd20-143">Autoriser les utilisateurs à des hubs d’accès et les méthodes de concentrateur</span><span class="sxs-lookup"><span data-stu-id="cfd20-143">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="cfd20-144">Par défaut, toutes les méthodes dans un concentrateur peuvent être appelées par un utilisateur non authentifié.</span><span class="sxs-lookup"><span data-stu-id="cfd20-144">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="cfd20-145">Pour exiger l’authentification, appliquer la [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribut au hub :</span><span class="sxs-lookup"><span data-stu-id="cfd20-145">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="cfd20-146">Vous pouvez utiliser les arguments de constructeur et les propriétés de la `[Authorize]` attribut pour restreindre l’accès aux seuls utilisateurs correspondance spécifique [stratégies d’autorisation](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="cfd20-146">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="cfd20-147">Par exemple, si vous avez une stratégie d’autorisation personnalisée appelée `MyAuthorizationPolicy` vous pouvez vous assurer que seuls les utilisateurs correspondant à cette stratégie peuvent accéder à du hub en utilisant le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cfd20-147">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="cfd20-148">Méthodes de concentrateur individuel peuvent avoir la `[Authorize]` attribut également appliqué.</span><span class="sxs-lookup"><span data-stu-id="cfd20-148">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="cfd20-149">Si l’utilisateur actuel ne correspond pas à la stratégie appliquée à la méthode, une erreur est retournée à l’appelant :</span><span class="sxs-lookup"><span data-stu-id="cfd20-149">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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
