---
title: Authentification et autorisation dans gRPC pour ASP.NET Core
author: jamesnk
description: Découvrez comment utiliser l’authentification et l’autorisation dans gRPC pour ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/26/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 34f7f8a5a22159329b3d6c4524943434c460c7fb
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602424"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="3b59f-103">Authentification et autorisation dans gRPC pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b59f-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="3b59f-104">Par [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="3b59f-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="3b59f-105">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3b59f-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="3b59f-106">Authentifier les utilisateurs appelant un service gRPC</span><span class="sxs-lookup"><span data-stu-id="3b59f-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="3b59f-107">gRPC peut être utilisé avec [l’authentification ASP.net Core](xref:security/authentication/identity) pour associer un utilisateur à chaque appel.</span><span class="sxs-lookup"><span data-stu-id="3b59f-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="3b59f-108">Voici un exemple `Startup.Configure` qui utilise l’authentification gRPC et ASP.net Core:</span><span class="sxs-lookup"><span data-stu-id="3b59f-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        routes.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> <span data-ttu-id="3b59f-109">L’ordre dans lequel vous inscrivez l’intergiciel (middleware) d’authentification ASP.NET Core est important.</span><span class="sxs-lookup"><span data-stu-id="3b59f-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="3b59f-110">Appelez `UseAuthentication` toujours et `UseAuthorization` après `UseRouting` et avant `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="3b59f-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="3b59f-111">Une fois l’authentification configurée, l’utilisateur est accessible dans une méthode de service gRPC via `ServerCallContext`le.</span><span class="sxs-lookup"><span data-stu-id="3b59f-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="3b59f-112">Authentification du jeton du porteur</span><span class="sxs-lookup"><span data-stu-id="3b59f-112">Bearer token authentication</span></span>

<span data-ttu-id="3b59f-113">Le client peut fournir un jeton d’accès pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="3b59f-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="3b59f-114">Le serveur valide le jeton et l’utilise pour identifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3b59f-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="3b59f-115">Sur le serveur, l’authentification du jeton du porteur est configurée à l’aide de l'[intergiciel (middleware) du porteur JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="3b59f-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="3b59f-116">Dans le client .NET gRPC, le jeton peut être envoyé avec des appels comme un en-tête:</span><span class="sxs-lookup"><span data-stu-id="3b59f-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

```csharp
public bool DoAuthenticatedCall(
    Ticketer.TicketerClient client, string token)
{
    var headers = new Metadata();
    headers.Add("Authorization", $"Bearer {token}");

    var request = new BuyTicketsRequest { Count = 1 };
    var response = await client.BuyTicketsAsync(request, headers);

    return response.Success;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="3b59f-117">Authentification par certificat client</span><span class="sxs-lookup"><span data-stu-id="3b59f-117">Client certificate authentication</span></span>

<span data-ttu-id="3b59f-118">Un client peut également fournir un certificat client pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="3b59f-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="3b59f-119">L' [authentification par certificat](https://tools.ietf.org/html/rfc5246#section-7.4.4) se produit au niveau du TLS, à long terme avant qu’il ne soit ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="3b59f-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="3b59f-120">Lorsque la demande entre ASP.NET Core, le [package d’authentification du certificat client](xref:security/authentication/certauth) vous permet de résoudre le certificat `ClaimsPrincipal`en.</span><span class="sxs-lookup"><span data-stu-id="3b59f-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="3b59f-121">L’hôte doit être configuré pour accepter les certificats clients.</span><span class="sxs-lookup"><span data-stu-id="3b59f-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="3b59f-122">Pour plus d’informations sur l’acceptation des certificats clients dans Kestrel, IIS et Azure, consultez [configurer votre hôte pour exiger des certificats](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .</span><span class="sxs-lookup"><span data-stu-id="3b59f-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="3b59f-123">Dans le client .net gRPC, le certificat client est ajouté à `HttpClientHandler` qui est ensuite utilisé pour créer le client gRPC:</span><span class="sxs-lookup"><span data-stu-id="3b59f-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC client
    var httpClient = new HttpClient(handler);
    httpClient.BaseAddress = new Uri(baseAddress);

    return GrpcClient.Create<Ticketer.TicketerClient>(httpClient);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="3b59f-124">Autres mécanismes d’authentification</span><span class="sxs-lookup"><span data-stu-id="3b59f-124">Other authentication mechanisms</span></span>

<span data-ttu-id="3b59f-125">Un grand nombre ASP.NET Core mécanismes d’authentification pris en charge fonctionnent avec gRPC:</span><span class="sxs-lookup"><span data-stu-id="3b59f-125">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="3b59f-126">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b59f-126">Azure Active Directory</span></span>
* <span data-ttu-id="3b59f-127">Certificat client</span><span class="sxs-lookup"><span data-stu-id="3b59f-127">Client Certificate</span></span>
* <span data-ttu-id="3b59f-128">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="3b59f-128">IdentityServer</span></span>
* <span data-ttu-id="3b59f-129">Jeton JWT</span><span class="sxs-lookup"><span data-stu-id="3b59f-129">JWT Token</span></span>
* <span data-ttu-id="3b59f-130">OAuth 2,0</span><span class="sxs-lookup"><span data-stu-id="3b59f-130">OAuth 2.0</span></span>
* <span data-ttu-id="3b59f-131">OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="3b59f-131">OpenID Connect</span></span>
* <span data-ttu-id="3b59f-132">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="3b59f-132">WS-Federation</span></span>

<span data-ttu-id="3b59f-133">Pour plus d’informations sur la configuration de l’authentification sur le serveur, consultez [ASP.net Core l’authentification](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="3b59f-133">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="3b59f-134">La configuration du client gRPC pour utiliser l’authentification dépend du mécanisme d’authentification que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="3b59f-134">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="3b59f-135">Les exemples précédents du jeton de porteur et du certificat client illustrent deux façons de configurer le client gRPC pour envoyer des métadonnées d’authentification avec des appels gRPC:</span><span class="sxs-lookup"><span data-stu-id="3b59f-135">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="3b59f-136">Les clients gRPC fortement typés utilisent `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="3b59f-136">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="3b59f-137">L’authentification peut être configurée sur [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)ou en ajoutant des instances personnalisées [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) à `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3b59f-137">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="3b59f-138">Chaque appel gRPC a un argument `CallOptions` facultatif.</span><span class="sxs-lookup"><span data-stu-id="3b59f-138">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="3b59f-139">Les en-têtes personnalisés peuvent être envoyés à l’aide de la collection d’en-têtes de l’option.</span><span class="sxs-lookup"><span data-stu-id="3b59f-139">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="3b59f-140">L’authentification Windows (NTLM/Kerberos/Negotiate) ne peut pas être utilisée avec gRPC.</span><span class="sxs-lookup"><span data-stu-id="3b59f-140">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="3b59f-141">gRPC requiert HTTP/2 et HTTP/2 ne prend pas en charge l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="3b59f-141">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="3b59f-142">Autoriser les utilisateurs à accéder aux services et aux méthodes de service</span><span class="sxs-lookup"><span data-stu-id="3b59f-142">Authorize users to access services and service methods</span></span>

<span data-ttu-id="3b59f-143">Par défaut, toutes les méthodes d’un service peuvent être appelées par des utilisateurs non authentifiés.</span><span class="sxs-lookup"><span data-stu-id="3b59f-143">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="3b59f-144">Pour exiger une authentification, appliquez l’attribut [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) au service:</span><span class="sxs-lookup"><span data-stu-id="3b59f-144">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="3b59f-145">Vous pouvez utiliser les arguments de constructeur et les propriétés de l'attribut `[Authorize]` pour restreindre l’accès aux seuls utilisateurs correspondant aux [stratégies d’autorisation](xref:security/authorization/policies) spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3b59f-145">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="3b59f-146">Par exemple, si vous avez une stratégie d’autorisation personnalisée `MyAuthorizationPolicy`nommée, assurez-vous que seuls les utilisateurs qui correspondent à cette stratégie peuvent accéder au service à l’aide du code suivant:</span><span class="sxs-lookup"><span data-stu-id="3b59f-146">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="3b59f-147">L’attribut peut également être appliqué `[Authorize]` à chaque méthode de service.</span><span class="sxs-lookup"><span data-stu-id="3b59f-147">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="3b59f-148">Si l’utilisateur actuel ne correspond pas aux stratégies appliquées à la **fois** à la méthode et à la classe, une erreur est retournée à l’appelant:</span><span class="sxs-lookup"><span data-stu-id="3b59f-148">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
    public override Task<AvailableTicketsResponse> GetAvailableTickets(
        Empty request, ServerCallContext context)
    {
        // ... buy tickets for the current user ...
    }

    [Authorize("Administrators")]
    public override Task<BuyTicketsResponse> RefundTickets(
        BuyTicketsRequest request, ServerCallContext context)
    {
        // ... refund tickets (something only Administrators can do) ..
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="3b59f-149">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3b59f-149">Additional resources</span></span>

* [<span data-ttu-id="3b59f-150">Authentification du jeton du porteur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b59f-150">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="3b59f-151">Configurer l’authentification par certificat client dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b59f-151">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
