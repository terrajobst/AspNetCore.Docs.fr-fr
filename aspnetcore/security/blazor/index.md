---
title: Authentification et autorisation avec ASP.NET Core Blazor
author: guardrex
description: Pour en savoir plus sur les scénarios d’authentification et d’autorisation avec Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2019
uid: security/blazor/index
ms.openlocfilehash: 8714acbeb6e8a00992a601030811b24f53426b82
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310525"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>Authentification et autorisation avec ASP.NET Core Blazor

Par [Steve Sanderson](https://github.com/SteveSandersonMS)

ASP.NET Core prend en charge la configuration et la gestion de la sécurité dans les applications Blazor.

Les scénarios de sécurité diffèrent entre les applications Blazor côté serveur et côté client. Étant donné que les applications Blazor côté serveur s’exécutent sur le serveur, les vérifications d’autorisation sont en mesure de déterminer :

* Les options de l’interface utilisateur présentées à un utilisateur (par exemple, les entrées de menu disponibles pour un utilisateur).
* Les règles d’accès pour les zones de l’application et les composants.

Les applications Blazor côté client exécutées sur le client. L’autorisation est *uniquement* utilisée pour déterminer les options de l’interface utilisateur à afficher. Dans la mesure où les vérifications côté client peuvent être modifiées ou ignorées par un utilisateur, une application Blazor côté client ne peut pas appliquer les règles d’accès d’autorisation.

## <a name="authentication"></a>Authentication

Blazor utilise les mécanismes d’authentification ASP.NET Core existants pour établir l’identité de l’utilisateur. Le mécanisme exact dépend de la façon dont l’application Blazor est hébergée, côté serveur ou côté client.

### <a name="blazor-server-side-authentication"></a>Authentification Blazor côté serveur

Les applications Blazor côté serveur fonctionnent via une connexion en temps réel qui est créée à l’aide de SignalR. [L’authentification dans les applications basées sur SignalR](xref:signalr/authn-and-authz) est gérée lorsque la connexion est établie. L’authentification peut reposer sur un cookie ou un autre jeton du porteur.

Le modèle de projet Blazor côté serveur peut configurer l’authentification pour vous lorsque le projet est créé.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Suivez les instructions de Visual Studio dans l’article <xref:blazor/get-started> pour créer un projet Blazor côté serveur avec un mécanisme d’authentification.

Après avoir choisi le modèle **Application serveur Blazor** dans la boîte de dialogue **Créer une application web ASP.NET Core.** , sélectionnez **Modifier** sous **Authentification**.

Une boîte de dialogue s’ouvre pour offrir le même ensemble de mécanismes d’authentification que ceux disponibles pour les autres projets ASP.NET Core :

* **Aucune authentification**
* **Comptes d’utilisateur individuels** &ndash; Les comptes d'utilisateur peuvent être stockés :
  * Dans l’application à l’aide du système [d’identité](xref:security/authentication/identity) d’ASP.NET Core.
  * Avec [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **Comptes professionnels ou scolaires**
* **Authentification Windows**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Suivez les instructions de Visual Studio Code dans l’article <xref:blazor/get-started> pour créer un projet Blazor côté serveur avec un mécanisme d’authentification :

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Les valeurs autorisées d’authentification (`{AUTHENTICATION}`) sont présentées dans le tableau suivant.

| Mécanisme d’authentification                                                                 | Valeur`{AUTHENTICATION}` |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Aucune authentification                                                                        | `None`                   |
| Individuel<br>Les utilisateurs stockés dans l’application avec l’identité ASP.NET Core.                        | `Individual`             |
| Individuel<br>Les utilisateurs stockés dans [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Comptes professionnels ou scolaires<br>Authentification d’organisation pour un seul abonné.            | `SingleOrg`              |
| Comptes professionnels ou scolaires<br>Authentification d’organisation pour plusieurs abonnés.           | `MultiOrg`               |
| Authentification Windows                                                                   | `Windows`                |

La commande crée un dossier nommé avec la valeur fournie pour l’espace réservé `{APP NAME}` et utilise le nom de dossier en tant que nom de l’application. Pour plus d’informations, consultez la commande [dotnet new](/dotnet/core/tools/dotnet-new) dans le Guide .NET Core.

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="blazor-client-side-authentication"></a>Authentification Blazor côté client

Dans les applications Blazor côté client, les contrôles d’authentification peuvent être contournés, car tout le code côté client peut être modifié par les utilisateurs. Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.

L’implémentation d’un service `AuthenticationStateProvider` personnalisé pour les applications Blazor côté client est couverte dans les sections suivantes.

## <a name="authenticationstateprovider-service"></a>Service AuthenticationStateProvider

Les applications Blazor côté serveur incluent un service `AuthenticationStateProvider` intégré qui obtient des données d’état d’authentification auprès de `HttpContext.User` de ASP.NET Core. Il s’agit de la façon dont l’état d’authentification s’intègre avec les mécanismes d’authentification ASP.NET Core côté serveur existants.

`AuthenticationStateProvider` est le service sous-jacent utilisé par le composant `AuthorizeView` et le composant `CascadingAuthenticationState` pour obtenir l’état d’authentification.

Vous n’utilisez généralement pas `AuthenticationStateProvider` directement. Utilisez les approches [Composant AuthorizeView](#authorizeview-component) ou [Tâche<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) décrites plus loin dans cet article. Le principal inconvénient de l’utilisation directe de `AuthenticationStateProvider` est que le composant n’est pas automatiquement averti en cas de modifications de données d’état de l’authentification sous-jacente.

Le service `AuthenticationStateProvider` peut fournir les données <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur actuel, comme indiqué dans l’exemple suivant :

```cshtml
@page "/"
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

Si `user.Identity.IsAuthenticated` est `true` et parce que l’utilisateur est <xref:System.Security.Claims.ClaimsPrincipal>, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.

Pour plus d’informations sur l’injection de dépendances et les services, consultez <xref:blazor/dependency-injection> et <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implémenter un AuthenticationStateProvider personnalisé

Si vous créez une application Blazor côté client ou si les spécifications de votre application requièrent absolument un fournisseur personnalisé, implémentez un fournisseur et substituez `GetAuthenticationStateAsync` :

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

Le service `CustomAuthStateProvider` est inscrit dans `Startup.ConfigureServices` :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

Avec `CustomAuthStateProvider`, tous les utilisateurs sont authentifiés avec le nom d’utilisateur `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Exposer l’état d’authentification comme un paramètre en cascade

Si les données d’état d’authentification sont requises pour la logique procédurale, par exemple lors d’une action déclenchée par l’utilisateur, obtenez les données d’état d’authentification en définissant un paramètre en cascade de type `Task<AuthenticationState>` :

```cshtml
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

Si `user.Identity.IsAuthenticated` est `true`, les revendications peuvent être énumérées et l’appartenance aux rôles évaluée.

Configurez `Task<AuthenticationState>` le paramètre en cascade à `AuthorizeRouteView` l' `CascadingAuthenticationState` aide des composants et :

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

## <a name="authorization"></a>Authorization

Une fois qu’un utilisateur est authentifié, les règles *d’autorisation* sont appliquées pour contrôler ce que l’utilisateur peut faire.

L’accès est généralement accordé ou refusé selon les conditions suivantes :

* Un utilisateur est authentifié (connecté).
* Un utilisateur appartient à un *rôle*.
* Un utilisateur a une *revendication*.
* Une *stratégie* est satisfaite.

Tous ces concepts sont les mêmes que dans une application ASP.NET Core MVC ou Razor Pages. Pour plus d’informations sur la sécurité d’ASP.NET Core, consultez les articles sous [Sécurité et identité dans ASP.NET Core](xref:security/index).

## <a name="authorizeview-component"></a>Composant AuthorizeView

Le composant `AuthorizeView` affiche sélectivement l’interface utilisateur en fonction de l’autorisation que l’utilisateur a pour l’afficher. Cette approche est utile lorsque vous devez uniquement *afficher* les données de l’utilisateur et que vous n’avez pas besoin d’utiliser l’identité de l’utilisateur dans la logique procédurale.

Le composant expose une variable `context` de type `AuthenticationState`, que vous pouvez utiliser pour accéder aux informations relatives à l’utilisateur connecté :

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

Vous pouvez également fournir un contenu différent à afficher si l’utilisateur n’est pas authentifié :

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

Le contenu de `<Authorized>` et de `<NotAuthorized>` peut inclure des éléments arbitraires, comme d’autres composants interactifs.

Les conditions d’autorisation, comme les rôles ou les stratégies qui contrôlent les options d’interface utilisateur ou d’accès, sont traitées dans la section [Autorisation](#authorization).

Si les conditions d’autorisation ne sont pas spécifiées, `AuthorizeView` utilise une stratégie par défaut et traite :

* Les utilisateurs authentifiés (connectés) comme étant autorisés.
* Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.

### <a name="role-based-and-policy-based-authorization"></a>Autorisation en fonction du rôle et de la stratégie

Le composant `AuthorizeView` prend en charge l’autorisation *basée sur le rôle* et *basée sur les stratégies*.

Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Pour plus d'informations, consultez <xref:security/authorization/roles>.

Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

L’autorisation basée sur les revendications est un cas spécial d’autorisation basée sur les stratégies. Par exemple, vous pouvez définir une stratégie qui impose aux utilisateurs d’avoir une certaine revendication. Pour plus d'informations, consultez <xref:security/authorization/policies>.

Ces API peuvent être utilisées dans les applications Blazor côté serveur ou client.

Si ni `Roles` ni `Policy` n’est spécifié, `AuthorizeView` utilise la stratégie par défaut.

### <a name="content-displayed-during-asynchronous-authentication"></a>Contenu affiché lors de l’authentification asynchrone

Blazor permet de déterminer l’état d’authentification *de façon asynchrone*. Le scénario principal de cette approche est dans les applications Blazor côté client qui créent une requête d’authentification sur un point de terminaison externe.

Lorsque l’authentification est en cours, `AuthorizeView` n’affiche aucun contenu par défaut. Pour afficher le contenu pendant que l’authentification se produit, utilisez l’élément `<Authorizing>` :

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

Cette approche n’est pas normalement applicable aux applications Blazor côté serveur. Les applications Blazor côté serveur connaissent l’état de l’authentification dès que l’état est établi. Le contenu de `Authorizing` peut être fourni dans le compsosant `AuthorizeView` d’une application Blazor côté serveur, mais le contenu n’est jamais affiché.

## <a name="authorize-attribute"></a>Attribut [Authorize]

Tout comme une application peut utiliser `[Authorize]` avec un contrôleur MVC ou une page Razor, `[Authorize]` peut également servir avec les Razor Components :

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> Utilisez uniquement `[Authorize]` sur les composants `@page` atteints via le routeur Blazor. L’autorisation est effectuée uniquement en tant qu’aspect du routage et *pas* pour les composants enfants rendus dans une page. Pour autoriser l’affichage d’éléments spécifiques dans une page, utilisez `AuthorizeView` à la place.

Vous devrez peut-être ajouter `@using Microsoft.AspNetCore.Authorization` au composant ou au fichier *_Imports.razor* pour que le composant compile.

L’attribut `[Authorize]` prend également en charge l’autorisation en fonction du rôle ou des stratégies. Pour l’autorisation en fonction du rôle, utilisez le paramètre `Roles` :

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

Pour l’autorisation en fonction des stratégies, utilisez le paramètre `Policy` :

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Si ni `Roles` ni `Policy` n’est spécifié, `[Authorize]` utilise la stratégie par défaut, qui consiste par défaut à traiter :

* Les utilisateurs authentifiés (connectés) comme étant autorisés.
* Les utilisateurs non authentifiés (déconnectés) comme étant non autorisés.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Personnaliser le contenu non autorisé avec le composant Router

Le `Router` composant, conjointement avec le `AuthorizeRouteView` composant, permet à l’application de spécifier du contenu personnalisé dans les cas suivants :

* Le contenu est introuvable.
* L’utilisateur ne répond pas à une condition `[Authorize]` appliquée au composant. L’attribut `[Authorize]` est couvert dans la section [Attribut [Authorize]](#authorize-attribute).
* Une authentification asynchrone est en cours.

Dans le modèle de projet Blazor côté serveur par défaut, le fichier *App.razor* montre comment définir le contenu personnalisé :

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

Le contenu de `<NotFound>`, `<NotAuthorized>` et `<Authorizing>` peut inclure des éléments arbitraires, comme d’autres composants interactifs.

Si `<NotAuthorized>` n’est pas spécifié `<AuthorizeRouteView>` , le utilise le message de secours suivant :

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Notification sur les changements d’état de l’authentification

Si l’application détermine que les données d’état d’authentification sous-jacentes ont changé (par exemple, parce que l’utilisateur s’est déconnecté ou qu’un autre utilisateur a modifié ses rôles), un `AuthenticationStateProvider` personnalisé peut éventuellement appeler la méthode `NotifyAuthenticationStateChanged` sur la classe de base `AuthenticationStateProvider`. Cela prévient les consommateurs des données d’état d’authentification (par exemple, `AuthorizeView`) qu’elles doivent appliquer un nouveau rendu à l’aide des nouvelles données.

## <a name="procedural-logic"></a>Logique procédurale

Si l’application est nécessaire pour vérifier les règles d’autorisation dans le cadre de la logique procédurale, utilisez un paramètre en cascade de type `Task<AuthenticationState>` pour obtenir le <xref:System.Security.Claims.ClaimsPrincipal> de l’utilisateur. `Task<AuthenticationState>` peut être combiné avec d’autres services, comme `IAuthorizationService`, pour évaluer les stratégies.

```cshtml
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

## <a name="authorization-in-blazor-client-side-apps"></a>Autorisation dans les applications Blazor côté client

Dans les applications Blazor côté client, les contrôles d’autorisation peuvent être contournés, car tout le code côté client peut être modifié par les utilisateurs. Cela vaut également pour toutes les technologies d’application côté client, y compris les infrastructures d’application JavaScript SPA ou les applications natives pour n’importe quel système d’exploitation.

**Effectuez toujours les vérifications d’autorisation sur le serveur au sein des points de terminaison de l’API auxquels votre application côté client accède.**

## <a name="troubleshoot-errors"></a>Résoudre les erreurs

Erreurs courantes :

* **L’autorisation nécessite un paramètre en cascade de type Task\<AuthenticationState>. Envisagez d’utiliser CascadingAuthenticationState pour fournir cela.**

* **La valeur `null` est reçue pour `authenticationStateTask`**

Il est probable que le projet n’a pas été créé à l’aide d’un modèle Blazor côté serveur avec l’authentification activée. Encapsulez un `<CascadingAuthenticationState>` dans certaines parties de l’arborescence de l’interface utilisateur, par exemple dans *App.razor*, comme suit :

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState` fournit le paramètre en cascade `Task<AuthenticationState>`, qu’il reçoit à son tour du service d’injection de dépendances `AuthenticationStateProvider` sous-jacent.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/index>
* <xref:security/blazor/server-side>
* <xref:security/authentication/windowsauth>
