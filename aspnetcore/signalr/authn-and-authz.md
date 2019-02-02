---
title: Authentification et autorisation dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment utiliser l’authentification et les autorisations dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/31/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: 5d4574775606b4354ec099b6b32e05294d9f0e45
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667308"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="ca215-103">Authentification et autorisation dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ca215-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="ca215-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="ca215-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="ca215-105">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="ca215-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="ca215-106">Authentifier les utilisateurs qui se connectent à un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="ca215-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="ca215-107">SignalR peut être utilisé avec [authentification ASP.NET Core](xref:security/authentication/identity) pour associer un utilisateur à chaque connexion.</span><span class="sxs-lookup"><span data-stu-id="ca215-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="ca215-108">Dans un hub, les données d’authentification sont accessibles à partir de la propriété [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user).</span><span class="sxs-lookup"><span data-stu-id="ca215-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="ca215-109">L’authentification permet au hub d'appeler des méthodes sur toutes les connexions associées à un utilisateur (voir [gérer les utilisateurs et groupes dans SignalR](xref:signalr/groups) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="ca215-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="ca215-110">Plusieurs connexions peuvent être associées à un seul utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="ca215-111">Authentification des cookies</span><span class="sxs-lookup"><span data-stu-id="ca215-111">Cookie authentication</span></span>

<span data-ttu-id="ca215-112">Dans une application basée sur navigateur, l’authentification par cookie permet à vos informations d’identification utilisateur existantes de circuler automatiquement vers les connexions SignalR.</span><span class="sxs-lookup"><span data-stu-id="ca215-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="ca215-113">Lorsque vous utilisez le navigateur client, aucune configuration supplémentaire n'est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="ca215-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="ca215-114">Si l’utilisateur est connecté à votre application, la connexion SignalR hérite automatiquement de cette authentification.</span><span class="sxs-lookup"><span data-stu-id="ca215-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="ca215-115">Les cookies sont un moyen de spécifiques au navigateur d’envoyer des jetons d’accès, mais les clients non-Web peuvent les envoyer.</span><span class="sxs-lookup"><span data-stu-id="ca215-115">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="ca215-116">Lorsque vous utilisez le [Client .NET](xref:signalr/dotnet-client), la propriété `Cookies` peut être configurée dans l'appel à `.WithUrl` afin de fournir un cookie.</span><span class="sxs-lookup"><span data-stu-id="ca215-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="ca215-117">Toutefois, utiliser l’authentification par cookie à partir du Client .NET nécessite que l’application fournisse une API pour échanger des données d’authentification pour un cookie.</span><span class="sxs-lookup"><span data-stu-id="ca215-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="ca215-118">Authentification du jeton du porteur</span><span class="sxs-lookup"><span data-stu-id="ca215-118">Bearer token authentication</span></span>

<span data-ttu-id="ca215-119">Le client peut fournir un jeton d’accès au lieu d’utiliser un cookie.</span><span class="sxs-lookup"><span data-stu-id="ca215-119">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="ca215-120">Le serveur valide le jeton et l’utilise pour identifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-120">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="ca215-121">Cette validation est effectuée uniquement quand la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="ca215-121">This validation is done only when the connection is established.</span></span> <span data-ttu-id="ca215-122">Pendant la durée de vie de la connexion, le serveur ne revalide automatiquement pour vérifier la révocation du jeton.</span><span class="sxs-lookup"><span data-stu-id="ca215-122">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="ca215-123">Sur le serveur, l’authentification du jeton du porteur est configurée à l’aide de l'[intergiciel (middleware) du porteur JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="ca215-123">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="ca215-124">Dans le client JavaScript, le jeton peut être fourni à l’aide de l'option [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication).</span><span class="sxs-lookup"><span data-stu-id="ca215-124">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="ca215-125">Dans le client .NET, il y a une propriété [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) similaire qui peut être utilisée pour configurer le jeton :</span><span class="sxs-lookup"><span data-stu-id="ca215-125">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="ca215-126">La fonction jeton d’accès que vous fournissez est appelée avant **chaque** demande HTTP adressée par SignalR.</span><span class="sxs-lookup"><span data-stu-id="ca215-126">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="ca215-127">Si vous devez renouveler le jeton afin de maintenir la connexion active (car il peut expirer pendant la connexion), faites-le à partir de cette fonction et retournez le jeton mis à jour.</span><span class="sxs-lookup"><span data-stu-id="ca215-127">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="ca215-128">Dans l’API web standard, les jetons du porteur sont envoyés dans un en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="ca215-128">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="ca215-129">Toutefois, SignalR est impossible de définir ces en-têtes dans les navigateurs lors de l’utilisation de certains transports.</span><span class="sxs-lookup"><span data-stu-id="ca215-129">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="ca215-130">Lorsque vous utilisez le protocole WebSocket et les événements, le jeton est transmis comme paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="ca215-130">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="ca215-131">Pour prendre en charge cela sur le serveur, une configuration supplémentaire est requise :</span><span class="sxs-lookup"><span data-stu-id="ca215-131">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="ca215-132">Cookies et les jetons du porteur</span><span class="sxs-lookup"><span data-stu-id="ca215-132">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="ca215-133">Étant donné que les cookies sont spécifiques aux navigateurs, les envoyer à partir d’autres types de clients augmente la complexité par rapport à l’envoi de jetons du porteur.</span><span class="sxs-lookup"><span data-stu-id="ca215-133">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="ca215-134">Pour cette raison, l’authentification par cookie n’est pas recommandée, sauf si l’application a uniquement besoin d’authentifier les utilisateurs à partir du client de navigateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-134">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="ca215-135">Authentification du jeton du porteur est l’approche recommandée lors de l’utilisation de clients autres que le navigateur client.</span><span class="sxs-lookup"><span data-stu-id="ca215-135">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="ca215-136">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="ca215-136">Windows authentication</span></span>

<span data-ttu-id="ca215-137">Si [l’authentification Windows](xref:security/authentication/windowsauth) est configuré dans votre application, SignalR permet d'utiliser cette identité pour sécuriser les hubs.</span><span class="sxs-lookup"><span data-stu-id="ca215-137">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="ca215-138">Toutefois, pour pouvoir envoyer des messages à des utilisateurs individuels, vous devez ajouter un fournisseur d’ID d’utilisateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ca215-138">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="ca215-139">C'est parce que le système d’authentification Windows ne fournit pas la demande « Name Identifier » que SignalR utilise pour déterminer le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-139">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="ca215-140">Ajoutez une nouvelle classe qui implémente `IUserIdProvider` et récupérez une des demandes à partir de l’utilisateur à utiliser comme identificateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-140">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="ca215-141">Par exemple, pour utiliser la demande « Name » (le nom d’utilisateur Windows sous la forme `[Domain]\[Username]`), créez la classe suivante :</span><span class="sxs-lookup"><span data-stu-id="ca215-141">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="ca215-142">Au lieu de `ClaimTypes.Name`, vous pouvez utiliser n’importe quelle valeur à partir du `User` (telles que l’identificateur SID de Windows, etc.).</span><span class="sxs-lookup"><span data-stu-id="ca215-142">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="ca215-143">La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système.</span><span class="sxs-lookup"><span data-stu-id="ca215-143">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="ca215-144">Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-144">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="ca215-145">Enregistrer ce composant dans votre `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ca215-145">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="ca215-146">Dans le Client .NET, l’authentification Windows doit être activée en définissant la propriété [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :</span><span class="sxs-lookup"><span data-stu-id="ca215-146">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="ca215-147">L’authentification Windows est uniquement prise en charge par le navigateur client en utilisant Microsoft Internet Explorer ou Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="ca215-147">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="ca215-148">Utilisation des revendications pour personnaliser la gestion d’identité</span><span class="sxs-lookup"><span data-stu-id="ca215-148">Use claims to customize identity handling</span></span>

<span data-ttu-id="ca215-149">Une application qui authentifie les utilisateurs peut dériver des ID d’utilisateur SignalR de revendications d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-149">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="ca215-150">Pour spécifier comment SignalR crée des ID utilisateur, vous devez implémenter `IUserIdProvider` et inscrire l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="ca215-150">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="ca215-151">L’exemple de code montre comment vous utiliseriez des revendications pour sélectionner l’adresse de messagerie de l’utilisateur en tant que la propriété d’identification.</span><span class="sxs-lookup"><span data-stu-id="ca215-151">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="ca215-152">La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système.</span><span class="sxs-lookup"><span data-stu-id="ca215-152">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="ca215-153">Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ca215-153">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="ca215-154">L’inscription de compte ajoute une revendication avec le type `ClaimsTypes.Email` à la base de données ASP.NET identity.</span><span class="sxs-lookup"><span data-stu-id="ca215-154">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="ca215-155">Enregistrer ce composant dans votre `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ca215-155">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="ca215-156">Autoriser les utilisateurs à accéder à des hubs et à des méthodes de hub</span><span class="sxs-lookup"><span data-stu-id="ca215-156">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="ca215-157">Par défaut, toutes les méthodes d'un hub peuvent être appelées par un utilisateur non authentifié.</span><span class="sxs-lookup"><span data-stu-id="ca215-157">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="ca215-158">Pour exiger l’authentification, appliquez l'attribut [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) sur le hub :</span><span class="sxs-lookup"><span data-stu-id="ca215-158">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="ca215-159">Vous pouvez utiliser les arguments de constructeur et les propriétés de l'attribut `[Authorize]` pour restreindre l’accès aux seuls utilisateurs correspondant aux [stratégies d’autorisation](xref:security/authorization/policies) spécifiques.</span><span class="sxs-lookup"><span data-stu-id="ca215-159">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="ca215-160">Par exemple, si vous avez une stratégie d’autorisation personnalisée appelée `MyAuthorizationPolicy` vous pouvez vous assurer que seuls les utilisateurs correspondant à cette stratégie peuvent accéder au hub en utilisant le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ca215-160">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="ca215-161">Les méthodes de hub individuel peuvent avoir l'attribut `[Authorize]` également appliqué.</span><span class="sxs-lookup"><span data-stu-id="ca215-161">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="ca215-162">Si l’utilisateur actuel ne correspond pas à la stratégie appliquée à la méthode, une erreur est retournée à l’appelant :</span><span class="sxs-lookup"><span data-stu-id="ca215-162">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ca215-163">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ca215-163">Additional resources</span></span>

* [<span data-ttu-id="ca215-164">Authentification du jeton du porteur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca215-164">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
