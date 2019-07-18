---
title: Authentification et autorisation dans gRPC pour ASP.NET Core
author: jamesnk
description: Découvrez comment utiliser l’authentification et l’autorisation dans gRPC pour ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 06/07/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 49024295e4db7976924397bb24567d92d6298562
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308772"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a>Authentification et autorisation dans gRPC pour ASP.NET Core

Par [James Newton-King](https://twitter.com/jamesnk)

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-calling-a-grpc-service"></a>Authentifier les utilisateurs appelant un service gRPC

gRPC peut être utilisé avec [l’authentification ASP.net Core](xref:security/authentication/identity) pour associer un utilisateur à chaque appel.

Voici un exemple `Startup.Configure` qui utilise l’authentification gRPC et ASP.net Core:

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
> L’ordre dans lequel vous inscrivez l’intergiciel (middleware) d’authentification ASP.NET Core est important. Appelez `UseAuthentication` toujours et `UseAuthorization` après `UseRouting` et avant `UseEndpoints`.

Une fois l’authentification configurée, l’utilisateur est accessible dans une méthode de service gRPC via `ServerCallContext`le.

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a>Authentification du jeton du porteur

Le client peut fournir un jeton d’accès pour l’authentification. Le serveur valide le jeton et l’utilise pour identifier l’utilisateur.

Sur le serveur, l’authentification du jeton du porteur est configurée à l’aide de l'[intergiciel (middleware) du porteur JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

Dans le client .NET gRPC, le jeton peut être envoyé avec des appels comme un en-tête:

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

### <a name="client-certificate-authentication"></a>Authentification par certificat client

Un client peut également fournir un certificat client pour l’authentification. L' [authentification par certificat](https://tools.ietf.org/html/rfc5246#section-7.4.4) se produit au niveau du TLS, à long terme avant qu’il ne soit ASP.net core. Lorsque la demande entre ASP.NET Core, le [package d’authentification du certificat client](xref:security/authentication/certauth) vous permet de résoudre le certificat `ClaimsPrincipal`en.

> [!NOTE]
> L’hôte doit être configuré pour accepter les certificats clients. Pour plus d’informations sur l’acceptation des certificats clients dans Kestrel, IIS et Azure, consultez [configurer votre hôte pour exiger des certificats](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .

Dans le client .net gRPC, le certificat client est ajouté à `HttpClientHandler` qui est ensuite utilisé pour créer le client gRPC:

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

### <a name="other-authentication-mechanisms"></a>Autres mécanismes d’authentification

En plus de l’authentification par jeton de porteur et certificat client, tous les mécanismes d’authentification ASP.NET Core pris en charge, tels que OAuth, OpenID et Negotiate, doivent fonctionner avec gRPC. Pour plus d’informations sur la configuration de l’authentification côté serveur, consultez [ASP.net Core l’authentification](xref:security/authentication/identity) .

La configuration côté client dépend du mécanisme d’authentification que vous utilisez. Les exemples précédents d’authentification du jeton du porteur et du certificat client illustrent deux façons de configurer le client gRPC pour envoyer des métadonnées d’authentification avec des appels gRPC:

* Les clients gRPC fortement typés utilisent `HttpClient` en interne. L’authentification peut être configurée sur [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)ou en ajoutant des instances personnalisées [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) à `HttpClient`.
* Chaque appel gRPC a un argument `CallOptions` facultatif. Les en-têtes personnalisés peuvent être envoyés à l’aide de la collection d’en-têtes de l’option.

## <a name="authorize-users-to-access-services-and-service-methods"></a>Autoriser les utilisateurs à accéder aux services et aux méthodes de service

Par défaut, toutes les méthodes d’un service peuvent être appelées par des utilisateurs non authentifiés. Pour exiger une authentification, appliquez l’attribut [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) au service:

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

Vous pouvez utiliser les arguments de constructeur et les propriétés de l'attribut `[Authorize]` pour restreindre l’accès aux seuls utilisateurs correspondant aux [stratégies d’autorisation](xref:security/authorization/policies) spécifiques. Par exemple, si vous avez une stratégie d’autorisation personnalisée `MyAuthorizationPolicy`nommée, assurez-vous que seuls les utilisateurs qui correspondent à cette stratégie peuvent accéder au service à l’aide du code suivant:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

L’attribut peut également être appliqué `[Authorize]` à chaque méthode de service. Si l’utilisateur actuel ne correspond pas aux stratégies appliquées à la **fois** à la méthode et à la classe, une erreur est retournée à l’appelant:

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

## <a name="additional-resources"></a>Ressources supplémentaires

* [Authentification du jeton du porteur dans ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Configurer l’authentification par certificat client dans ASP.NET Core](xref:security/authentication/certauth)
