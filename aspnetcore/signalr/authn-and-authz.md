---
title: Authentification et autorisation dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment utiliser l’authentification et les autorisations dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 05/09/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e8f9dc48be780fb91bdec6ea4d579f5e4f16197b
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516942"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="017d2-103">Authentification et autorisation dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="017d2-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="017d2-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="017d2-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="017d2-105">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="017d2-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="017d2-106">Authentifier les utilisateurs qui se connectent à un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="017d2-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="017d2-107">SignalR peut être utilisé avec [authentification ASP.NET Core](xref:security/authentication/identity) pour associer un utilisateur à chaque connexion.</span><span class="sxs-lookup"><span data-stu-id="017d2-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="017d2-108">Dans un hub, les données d’authentification sont accessibles à partir de la propriété [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user).</span><span class="sxs-lookup"><span data-stu-id="017d2-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="017d2-109">L’authentification permet au hub d'appeler des méthodes sur toutes les connexions associées à un utilisateur (voir [gérer les utilisateurs et groupes dans SignalR](xref:signalr/groups) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="017d2-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="017d2-110">Plusieurs connexions peuvent être associées à un seul utilisateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="017d2-111">Voici un exemple de `Startup.Configure` qui utilise l’authentification de SignalR et ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="017d2-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();
    
    app.UseAuthentication();

    app.UseSignalR(hubs =>
    {
        hubs.MapHub<ChatHub>("/chat");
    });

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

> [!NOTE]
> <span data-ttu-id="017d2-112">L’ordre dans lequel vous inscrivez le middleware d’authentification SignalR et ASP.NET Core est important.</span><span class="sxs-lookup"><span data-stu-id="017d2-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="017d2-113">Appelez toujours `UseAuthentication` avant `UseSignalR` afin que SignalR a un utilisateur le `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="017d2-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="017d2-114">Authentification des cookies</span><span class="sxs-lookup"><span data-stu-id="017d2-114">Cookie authentication</span></span>

<span data-ttu-id="017d2-115">Dans une application basée sur navigateur, l’authentification par cookie permet à vos informations d’identification utilisateur existantes de circuler automatiquement vers les connexions SignalR.</span><span class="sxs-lookup"><span data-stu-id="017d2-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="017d2-116">Lorsque vous utilisez le navigateur client, aucune configuration supplémentaire n'est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="017d2-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="017d2-117">Si l’utilisateur est connecté à votre application, la connexion SignalR hérite automatiquement de cette authentification.</span><span class="sxs-lookup"><span data-stu-id="017d2-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="017d2-118">Les cookies sont un moyen de spécifiques au navigateur d’envoyer des jetons d’accès, mais les clients non-Web peuvent les envoyer.</span><span class="sxs-lookup"><span data-stu-id="017d2-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="017d2-119">Lorsque vous utilisez le [Client .NET](xref:signalr/dotnet-client), la propriété `Cookies` peut être configurée dans l'appel à `.WithUrl` afin de fournir un cookie.</span><span class="sxs-lookup"><span data-stu-id="017d2-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="017d2-120">Toutefois, utiliser l’authentification par cookie à partir du Client .NET nécessite que l’application fournisse une API pour échanger des données d’authentification pour un cookie.</span><span class="sxs-lookup"><span data-stu-id="017d2-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="017d2-121">Authentification du jeton du porteur</span><span class="sxs-lookup"><span data-stu-id="017d2-121">Bearer token authentication</span></span>

<span data-ttu-id="017d2-122">Le client peut fournir un jeton d’accès au lieu d’utiliser un cookie.</span><span class="sxs-lookup"><span data-stu-id="017d2-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="017d2-123">Le serveur valide le jeton et l’utilise pour identifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="017d2-124">Cette validation est effectuée uniquement quand la connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="017d2-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="017d2-125">Pendant la durée de vie de la connexion, le serveur ne revalide automatiquement pour vérifier la révocation du jeton.</span><span class="sxs-lookup"><span data-stu-id="017d2-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="017d2-126">Sur le serveur, l’authentification du jeton du porteur est configurée à l’aide de l'[intergiciel (middleware) du porteur JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="017d2-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="017d2-127">Dans le client JavaScript, le jeton peut être fourni à l’aide de l'option [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication).</span><span class="sxs-lookup"><span data-stu-id="017d2-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="017d2-128">Dans le client .NET, il y a une propriété [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) similaire qui peut être utilisée pour configurer le jeton :</span><span class="sxs-lookup"><span data-stu-id="017d2-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="017d2-129">La fonction jeton d’accès que vous fournissez est appelée avant **chaque** demande HTTP adressée par SignalR.</span><span class="sxs-lookup"><span data-stu-id="017d2-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="017d2-130">Si vous devez renouveler le jeton afin de maintenir la connexion active (car il peut expirer pendant la connexion), faites-le à partir de cette fonction et retournez le jeton mis à jour.</span><span class="sxs-lookup"><span data-stu-id="017d2-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="017d2-131">Dans l’API web standard, les jetons du porteur sont envoyés dans un en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="017d2-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="017d2-132">Toutefois, SignalR est impossible de définir ces en-têtes dans les navigateurs lors de l’utilisation de certains transports.</span><span class="sxs-lookup"><span data-stu-id="017d2-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="017d2-133">Lorsque vous utilisez le protocole WebSocket et les événements, le jeton est transmis comme paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="017d2-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="017d2-134">Pour prendre en charge cela sur le serveur, une configuration supplémentaire est requise :</span><span class="sxs-lookup"><span data-stu-id="017d2-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="017d2-135">Cookies et les jetons du porteur</span><span class="sxs-lookup"><span data-stu-id="017d2-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="017d2-136">Étant donné que les cookies sont spécifiques aux navigateurs, les envoyer à partir d’autres types de clients augmente la complexité par rapport à l’envoi de jetons du porteur.</span><span class="sxs-lookup"><span data-stu-id="017d2-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="017d2-137">Pour cette raison, l’authentification par cookie n’est pas recommandée, sauf si l’application a uniquement besoin d’authentifier les utilisateurs à partir du client de navigateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="017d2-138">Authentification du jeton du porteur est l’approche recommandée lors de l’utilisation de clients autres que le navigateur client.</span><span class="sxs-lookup"><span data-stu-id="017d2-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="017d2-139">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="017d2-139">Windows authentication</span></span>

<span data-ttu-id="017d2-140">Si [l’authentification Windows](xref:security/authentication/windowsauth) est configuré dans votre application, SignalR permet d'utiliser cette identité pour sécuriser les hubs.</span><span class="sxs-lookup"><span data-stu-id="017d2-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="017d2-141">Toutefois, pour pouvoir envoyer des messages à des utilisateurs individuels, vous devez ajouter un fournisseur d’ID d’utilisateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="017d2-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="017d2-142">C'est parce que le système d’authentification Windows ne fournit pas la demande « Name Identifier » que SignalR utilise pour déterminer le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="017d2-143">Ajoutez une nouvelle classe qui implémente `IUserIdProvider` et récupérez une des demandes à partir de l’utilisateur à utiliser comme identificateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="017d2-144">Par exemple, pour utiliser la demande « Name » (le nom d’utilisateur Windows sous la forme `[Domain]\[Username]`), créez la classe suivante :</span><span class="sxs-lookup"><span data-stu-id="017d2-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="017d2-145">Au lieu de `ClaimTypes.Name`, vous pouvez utiliser n’importe quelle valeur à partir du `User` (telles que l’identificateur SID de Windows, etc.).</span><span class="sxs-lookup"><span data-stu-id="017d2-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="017d2-146">La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système.</span><span class="sxs-lookup"><span data-stu-id="017d2-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="017d2-147">Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="017d2-148">Enregistrer ce composant dans votre `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="017d2-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="017d2-149">Dans le Client .NET, l’authentification Windows doit être activée en définissant la propriété [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :</span><span class="sxs-lookup"><span data-stu-id="017d2-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="017d2-150">L’authentification Windows est uniquement prise en charge par le navigateur client en utilisant Microsoft Internet Explorer ou Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="017d2-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="017d2-151">Utilisation des revendications pour personnaliser la gestion d’identité</span><span class="sxs-lookup"><span data-stu-id="017d2-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="017d2-152">Une application qui authentifie les utilisateurs peut dériver des ID d’utilisateur SignalR de revendications d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="017d2-153">Pour spécifier comment SignalR crée des ID utilisateur, vous devez implémenter `IUserIdProvider` et inscrire l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="017d2-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="017d2-154">L’exemple de code montre comment vous utiliseriez des revendications pour sélectionner l’adresse de messagerie de l’utilisateur en tant que la propriété d’identification.</span><span class="sxs-lookup"><span data-stu-id="017d2-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="017d2-155">La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système.</span><span class="sxs-lookup"><span data-stu-id="017d2-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="017d2-156">Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="017d2-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="017d2-157">L’inscription de compte ajoute une revendication avec le type `ClaimsTypes.Email` à la base de données ASP.NET identity.</span><span class="sxs-lookup"><span data-stu-id="017d2-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="017d2-158">Enregistrer ce composant dans votre `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="017d2-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="017d2-159">Autoriser les utilisateurs à accéder à des hubs et à des méthodes de hub</span><span class="sxs-lookup"><span data-stu-id="017d2-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="017d2-160">Par défaut, toutes les méthodes d'un hub peuvent être appelées par un utilisateur non authentifié.</span><span class="sxs-lookup"><span data-stu-id="017d2-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="017d2-161">Pour exiger l’authentification, appliquez l'attribut [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) sur le hub :</span><span class="sxs-lookup"><span data-stu-id="017d2-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="017d2-162">Vous pouvez utiliser les arguments de constructeur et les propriétés de l'attribut `[Authorize]` pour restreindre l’accès aux seuls utilisateurs correspondant aux [stratégies d’autorisation](xref:security/authorization/policies) spécifiques.</span><span class="sxs-lookup"><span data-stu-id="017d2-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="017d2-163">Par exemple, si vous avez une stratégie d’autorisation personnalisée appelée `MyAuthorizationPolicy` vous pouvez vous assurer que seuls les utilisateurs correspondant à cette stratégie peuvent accéder au hub en utilisant le code suivant :</span><span class="sxs-lookup"><span data-stu-id="017d2-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="017d2-164">Les méthodes de hub individuel peuvent avoir l'attribut `[Authorize]` également appliqué.</span><span class="sxs-lookup"><span data-stu-id="017d2-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="017d2-165">Si l’utilisateur actuel ne correspond pas à la stratégie appliquée à la méthode, une erreur est retournée à l’appelant :</span><span class="sxs-lookup"><span data-stu-id="017d2-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="017d2-166">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="017d2-166">Additional resources</span></span>

* [<span data-ttu-id="017d2-167">Authentification du jeton du porteur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="017d2-167">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
