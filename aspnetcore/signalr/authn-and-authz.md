---
title: Authentification et autorisation dans ASP.NET Core Signalr
author: bradygaster
description: Découvrez comment utiliser l’authentification et les autorisations dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 07/15/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e7e7a9fd537ba89b64c15594652a290357a00038
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412535"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Authentification et autorisation dans ASP.NET Core Signalr

Par [Andrew Stanton-infirmière](https://twitter.com/anurse)

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Authentifier les utilisateurs qui se connectent à un hub SignalR

Signalr peut être utilisé avec [l’authentification ASP.net Core](xref:security/authentication/identity) pour associer un utilisateur à chaque connexion. Dans un hub, les données d’authentification sont accessibles à partir de la propriété [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user). L’authentification permet au hub d'appeler des méthodes sur toutes les connexions associées à un utilisateur (voir [gérer les utilisateurs et groupes dans SignalR](xref:signalr/groups) pour plus d’informations). Plusieurs connexions peuvent être associées à un seul utilisateur.

Voici un exemple `Startup.Configure` qui utilise signalr et l’authentification ASP.net Core:

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
> L’ordre dans lequel vous inscrivez Signalr et ASP.NET Core middleware d’authentification est important. Appelez `UseAuthentication` toujours avant `UseSignalR` afin que signalr ait un utilisateur sur le `HttpContext`.

### <a name="cookie-authentication"></a>Authentification par cookie

Dans une application basée sur navigateur, l’authentification par cookie permet à vos informations d’identification utilisateur existantes de circuler automatiquement vers les connexions SignalR. Lorsque vous utilisez le navigateur client, aucune configuration supplémentaire n'est nécessaire. Si l’utilisateur est connecté à votre application, la connexion Signalr hérite automatiquement de cette authentification.

Les cookies sont un moyen spécifique au navigateur d’envoyer des jetons d’accès, mais les clients sans navigateur peuvent les envoyer. Lorsque vous utilisez le [Client .NET](xref:signalr/dotnet-client), la propriété `Cookies` peut être configurée dans l'appel à `.WithUrl` afin de fournir un cookie. Toutefois, utiliser l’authentification par cookie à partir du Client .NET nécessite que l’application fournisse une API pour échanger des données d’authentification pour un cookie.

### <a name="bearer-token-authentication"></a>Authentification du jeton du porteur

Le client peut fournir un jeton d’accès au lieu d’utiliser un cookie. Le serveur valide le jeton et l’utilise pour identifier l’utilisateur. Cette validation est effectuée uniquement lorsque la connexion est établie. Pendant la durée de vie de la connexion, le serveur n’est pas automatiquement revalidé pour vérifier la révocation des jetons.

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
> La fonction de jeton d’accès que vous fournissez est appelée avant **chaque** requête HTTP effectuée par signalr. Si vous devez renouveler le jeton afin de maintenir la connexion active (car il peut expirer pendant la connexion), faites-le à partir de cette fonction et retournez le jeton mis à jour.

Dans les API Web standard, les jetons du porteur sont envoyés dans un en-tête HTTP. Toutefois, Signalr ne peut pas définir ces en-têtes dans les navigateurs lors de l’utilisation de certains transports. Lorsque vous utilisez des WebSockets et des événements envoyés par le serveur, le jeton est transmis sous la forme d’un paramètre de chaîne de requête. Pour prendre en charge cela sur le serveur, une configuration supplémentaire est requise:

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>Cookies et jetons de porteur 

Étant donné que les cookies sont spécifiques aux navigateurs, leur envoi à partir d’autres types de clients augmente la complexité par rapport à l’envoi des jetons de porteur. C’est la raison pour laquelle l’authentification par cookie n’est pas recommandée, sauf si l’application doit uniquement authentifier les utilisateurs à partir du navigateur client. L’authentification par jeton du porteur est l’approche recommandée lors de l’utilisation de clients autres que le navigateur client.

### <a name="windows-authentication"></a>Authentification Windows

Si [l’authentification Windows](xref:security/authentication/windowsauth) est configuré dans votre application, SignalR permet d'utiliser cette identité pour sécuriser les hubs. Toutefois, pour pouvoir envoyer des messages à des utilisateurs individuels, vous devez ajouter un fournisseur d’ID d’utilisateur personnalisé. C'est parce que le système d’authentification Windows ne fournit pas la demande « Name Identifier » que SignalR utilise pour déterminer le nom d’utilisateur.

Ajoutez une nouvelle classe qui implémente `IUserIdProvider` et récupérez une des demandes à partir de l’utilisateur à utiliser comme identificateur. Par exemple, pour utiliser la demande « Name » (le nom d’utilisateur Windows sous la forme `[Domain]\[Username]`), créez la classe suivante :

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

Au lieu de `ClaimTypes.Name`, vous pouvez utiliser n’importe quelle valeur à partir du `User` (telles que l’identificateur SID de Windows, etc.).

> [!NOTE]
> La valeur que vous choisissez doit être unique parmi tous les utilisateurs de votre système. Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.

Inscrivez ce composant dans votre `Startup.ConfigureServices` méthode.

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

### <a name="use-claims-to-customize-identity-handling"></a>Utiliser des revendications pour personnaliser la gestion des identités

Une application qui authentifie les utilisateurs peut dériver des ID d’utilisateur Signalr de revendications d’utilisateur. Pour spécifier le mode de création des ID d’utilisateur `IUserIdProvider` par signalr, implémentez et inscrivez l’implémentation.

L’exemple de code montre comment utiliser les revendications pour sélectionner l’adresse e-mail de l’utilisateur comme propriété d’identification. 

> [!NOTE]
> La valeur que vous choisissez doit être unique parmi tous les utilisateurs de votre système. Sinon, un message destiné à un utilisateur pourrait aboutir à un autre utilisateur.

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

L’inscription du compte ajoute une revendication de `ClaimsTypes.Email` type à la base de données d’identité ASP.net.

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

Inscrivez ce composant dans votre `Startup.ConfigureServices`.

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Autoriser les utilisateurs à accéder à des hubs et à des méthodes de hub

Par défaut, toutes les méthodes d'un hub peuvent être appelées par un utilisateur non authentifié. Pour exiger l’authentification, appliquez l'attribut [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) sur le hub :

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Vous pouvez utiliser les arguments de constructeur et les propriétés de l'attribut `[Authorize]` pour restreindre l’accès aux seuls utilisateurs correspondant aux [stratégies d’autorisation](xref:security/authorization/policies) spécifiques. Par exemple, si vous avez une stratégie d’autorisation personnalisée appelée `MyAuthorizationPolicy` vous pouvez vous assurer que seuls les utilisateurs correspondant à cette stratégie peuvent accéder au hub en utilisant le code suivant :

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

Les méthodes de hub individuel peuvent avoir l'attribut `[Authorize]` également appliqué. Si l’utilisateur actuel ne correspond pas à la stratégie appliquée à la méthode, une erreur est retournée à l’appelant:

```csharp
[Authorize]
public class ChatHub : Hub
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

::: moniker range=">= aspnetcore-3.0"

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a>Utiliser des gestionnaires d’autorisations pour personnaliser l’autorisation de méthode de concentrateur

Signalr fournit une ressource personnalisée aux gestionnaires d’autorisations lorsqu’une méthode de concentrateur requiert une autorisation. La ressource est une instance de `HubInvocationContext`. Le `HubInvocationContext` comprend le `HubCallerContext`, le nom de la méthode de concentrateur appelée et les arguments de la méthode de concentrateur.

Prenons l’exemple d’une salle de conversation permettant à plusieurs entreprises de se connecter via Azure Active Directory. Toute personne disposant d’un compte Microsoft peut se connecter à chat, mais seuls les membres de l’organisation propriétaire doivent être en mesure d’interdire les utilisateurs ou d’afficher les historiques de conversation des utilisateurs. En outre, nous pouvons souhaiter restreindre certaines fonctionnalités de certains utilisateurs. L’utilisation des fonctionnalités mises à jour dans ASP.NET Core 3,0 est tout à fait possible. Notez comment le `DomainRestrictedRequirement` sert de personnalisé. `IAuthorizationRequirement` Maintenant que le `HubInvocationContext` paramètre de ressource est passé, la logique interne peut inspecter le contexte dans lequel le Hub est appelé et prendre des décisions pour permettre à l’utilisateur d’exécuter des méthodes de concentrateur individuelles.

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}

public class DomainRestrictedRequirement : 
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>, 
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement, 
        HubInvocationContext resource)
    {
        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name) && 
            context.User.Identity.Name.EndsWith("@microsoft.com"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName,
        string currentUsername)
    {
        return !(currentUsername.Equals("asdf42@microsoft.com") && 
            hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase));
    }
}
```

Dans `Startup.ConfigureServices`, ajoutez la nouvelle stratégie, en fournissant la `DomainRestrictedRequirement` spécification personnalisée comme paramètre pour créer la `DomainRestricted` stratégie.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services
        .AddAuthorization(options =>
        {
            options.AddPolicy("DomainRestricted", policy =>
            {
                policy.Requirements.Add(new DomainRestrictedRequirement());
            });
        });
}
```

Dans l’exemple précédent, la `DomainRestrictedRequirement` classe est à la `IAuthorizationRequirement` fois un et `AuthorizationHandler` son propre pour cette spécification. Il est acceptable de répartir ces deux composants dans des classes distinctes pour séparer les préoccupations. L’une des avantages de l’approche de l’exemple est qu’il n’est `AuthorizationHandler` pas nécessaire d’injecter le pendant le démarrage, car l’exigence et le gestionnaire sont identiques.

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* [Authentification du jeton du porteur dans ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Autorisation basée sur les ressources](xref:security/authorization/resourcebased)
