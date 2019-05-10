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
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Authentification et autorisation dans ASP.NET Core SignalR

Par [Andrew Stanton-Nurse](https://twitter.com/anurse)

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Authentifier les utilisateurs qui se connectent à un hub SignalR

SignalR peut être utilisé avec [authentification ASP.NET Core](xref:security/authentication/identity) pour associer un utilisateur à chaque connexion. Dans un hub, les données d’authentification sont accessibles à partir de la propriété [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user). L’authentification permet au hub d'appeler des méthodes sur toutes les connexions associées à un utilisateur (voir [gérer les utilisateurs et groupes dans SignalR](xref:signalr/groups) pour plus d’informations). Plusieurs connexions peuvent être associées à un seul utilisateur.

Voici un exemple de `Startup.Configure` qui utilise l’authentification de SignalR et ASP.NET Core :

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
> L’ordre dans lequel vous inscrivez le middleware d’authentification SignalR et ASP.NET Core est important. Appelez toujours `UseAuthentication` avant `UseSignalR` afin que SignalR a un utilisateur le `HttpContext`.

### <a name="cookie-authentication"></a>Authentification des cookies

Dans une application basée sur navigateur, l’authentification par cookie permet à vos informations d’identification utilisateur existantes de circuler automatiquement vers les connexions SignalR. Lorsque vous utilisez le navigateur client, aucune configuration supplémentaire n'est nécessaire. Si l’utilisateur est connecté à votre application, la connexion SignalR hérite automatiquement de cette authentification.

Les cookies sont un moyen de spécifiques au navigateur d’envoyer des jetons d’accès, mais les clients non-Web peuvent les envoyer. Lorsque vous utilisez le [Client .NET](xref:signalr/dotnet-client), la propriété `Cookies` peut être configurée dans l'appel à `.WithUrl` afin de fournir un cookie. Toutefois, utiliser l’authentification par cookie à partir du Client .NET nécessite que l’application fournisse une API pour échanger des données d’authentification pour un cookie.

### <a name="bearer-token-authentication"></a>Authentification du jeton du porteur

Le client peut fournir un jeton d’accès au lieu d’utiliser un cookie. Le serveur valide le jeton et l’utilise pour identifier l’utilisateur. Cette validation est effectuée uniquement quand la connexion est établie. Pendant la durée de vie de la connexion, le serveur ne revalide automatiquement pour vérifier la révocation du jeton.

Sur le serveur, l’authentification du jeton du porteur est configurée à l’aide de l'[intergiciel (middleware) du porteur JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

Dans le client JavaScript, le jeton peut être fourni à l’aide de l'option [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication).

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

Dans le client .NET, il y a une propriété [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) similaire qui peut être utilisée pour configurer le jeton :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> La fonction jeton d’accès que vous fournissez est appelée avant **chaque** demande HTTP adressée par SignalR. Si vous devez renouveler le jeton afin de maintenir la connexion active (car il peut expirer pendant la connexion), faites-le à partir de cette fonction et retournez le jeton mis à jour.

Dans l’API web standard, les jetons du porteur sont envoyés dans un en-tête HTTP. Toutefois, SignalR est impossible de définir ces en-têtes dans les navigateurs lors de l’utilisation de certains transports. Lorsque vous utilisez le protocole WebSocket et les événements, le jeton est transmis comme paramètre de chaîne de requête. Pour prendre en charge cela sur le serveur, une configuration supplémentaire est requise :

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>Cookies et les jetons du porteur 

Étant donné que les cookies sont spécifiques aux navigateurs, les envoyer à partir d’autres types de clients augmente la complexité par rapport à l’envoi de jetons du porteur. Pour cette raison, l’authentification par cookie n’est pas recommandée, sauf si l’application a uniquement besoin d’authentifier les utilisateurs à partir du client de navigateur. Authentification du jeton du porteur est l’approche recommandée lors de l’utilisation de clients autres que le navigateur client.

### <a name="windows-authentication"></a>Authentification Windows

Si [l’authentification Windows](xref:security/authentication/windowsauth) est configuré dans votre application, SignalR permet d'utiliser cette identité pour sécuriser les hubs. Toutefois, pour pouvoir envoyer des messages à des utilisateurs individuels, vous devez ajouter un fournisseur d’ID d’utilisateur personnalisé. C'est parce que le système d’authentification Windows ne fournit pas la demande « Name Identifier » que SignalR utilise pour déterminer le nom d’utilisateur.

Ajoutez une nouvelle classe qui implémente `IUserIdProvider` et récupérez une des demandes à partir de l’utilisateur à utiliser comme identificateur. Par exemple, pour utiliser la demande « Name » (le nom d’utilisateur Windows sous la forme `[Domain]\[Username]`), créez la classe suivante :

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

Au lieu de `ClaimTypes.Name`, vous pouvez utiliser n’importe quelle valeur à partir du `User` (telles que l’identificateur SID de Windows, etc.).

> [!NOTE]
> La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système. Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.

Enregistrer ce composant dans votre `Startup.ConfigureServices` (méthode).

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

Dans le Client .NET, l’authentification Windows doit être activée en définissant la propriété [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

L’authentification Windows est uniquement prise en charge par le navigateur client en utilisant Microsoft Internet Explorer ou Microsoft Edge.

### <a name="use-claims-to-customize-identity-handling"></a>Utilisation des revendications pour personnaliser la gestion d’identité

Une application qui authentifie les utilisateurs peut dériver des ID d’utilisateur SignalR de revendications d’utilisateur. Pour spécifier comment SignalR crée des ID utilisateur, vous devez implémenter `IUserIdProvider` et inscrire l’implémentation.

L’exemple de code montre comment vous utiliseriez des revendications pour sélectionner l’adresse de messagerie de l’utilisateur en tant que la propriété d’identification. 

> [!NOTE]
> La valeur que vous choisissez doit être unique parmi tous les utilisateurs dans votre système. Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

L’inscription de compte ajoute une revendication avec le type `ClaimsTypes.Email` à la base de données ASP.NET identity.

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

Enregistrer ce composant dans votre `Startup.ConfigureServices`.

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Autoriser les utilisateurs à accéder à des hubs et à des méthodes de hub

Par défaut, toutes les méthodes d'un hub peuvent être appelées par un utilisateur non authentifié. Pour exiger l’authentification, appliquez l'attribut [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) sur le hub :

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Vous pouvez utiliser les arguments de constructeur et les propriétés de l'attribut `[Authorize]` pour restreindre l’accès aux seuls utilisateurs correspondant aux [stratégies d’autorisation](xref:security/authorization/policies) spécifiques. Par exemple, si vous avez une stratégie d’autorisation personnalisée appelée `MyAuthorizationPolicy` vous pouvez vous assurer que seuls les utilisateurs correspondant à cette stratégie peuvent accéder au hub en utilisant le code suivant :

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

Les méthodes de hub individuel peuvent avoir l'attribut `[Authorize]` également appliqué. Si l’utilisateur actuel ne correspond pas à la stratégie appliquée à la méthode, une erreur est retournée à l’appelant :

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

## <a name="additional-resources"></a>Ressources supplémentaires

* [Authentification du jeton du porteur dans ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
