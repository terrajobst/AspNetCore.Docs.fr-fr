---
title: Scénarios de sécurité supplémentaires de ASP.NET Core Blazor webassembly
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: ccb512392341e3eea33f4ab45740b7900f7b63f9
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989454"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>Scénarios de sécurité supplémentaires ASP.NET Core éblouissant webassembly

Par [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a>Gérer les erreurs de demande de jeton

Lorsqu’une application à page unique (SPA) authentifie un utilisateur à l’aide d’Open ID Connect (OIDC), l’état d’authentification est conservé localement au sein du SPA et dans le fournisseur d’identité (IP) sous la forme d’un cookie de session défini à la suite de l’utilisateur qui fournit son informations d’identification.

Les jetons que l’adresse IP émet pour l’utilisateur sont généralement valides pendant de courtes périodes, environ une heure normalement, de sorte que l’application cliente doit régulièrement extraire les nouveaux jetons. Dans le cas contraire, l’utilisateur est déconnecté après l’expiration des jetons accordés. Dans la plupart des cas, les clients OIDC sont en mesure de configurer de nouveaux jetons sans que l’utilisateur soit obligé de s’authentifier à nouveau grâce à l’état d’authentification ou à la « session » qui est conservée au sein de l’adresse IP.

Dans certains cas, le client ne peut pas obtenir un jeton sans intervention de l’utilisateur, par exemple, lorsque, pour une raison quelconque, l’utilisateur se déconnecte explicitement de l’adresse IP. Ce scénario se produit si un utilisateur visite `https://login.microsoftonline.com` et se déconnecte. Dans ces scénarios, l’application ne sait pas immédiatement que l’utilisateur s’est déconnecté. Tout jeton que le client contient peut ne plus être valide. En outre, le client n’est pas en mesure d’approvisionner un nouveau jeton sans interaction de l’utilisateur après l’expiration du jeton actuel.

Ces scénarios ne sont pas spécifiques à l’authentification basée sur les jetons. Elles font partie de la nature des SPAs. Un SPA utilisant des cookies ne peut pas non plus appeler une API serveur si le cookie d’authentification est supprimé.

Quand une application effectue des appels d’API vers des ressources protégées, vous devez tenir compte des éléments suivants :

* Pour approvisionner un nouveau jeton d’accès pour appeler l’API, l’utilisateur peut être amené à s’authentifier à nouveau.
* Même si le client possède un jeton qui semble être valide, l’appel au serveur peut échouer parce que le jeton a été révoqué par l’utilisateur.

Lorsque l’application demande un jeton, deux résultats sont possibles :

* La demande a échoué et l’application a un jeton valide.
* La demande échoue et l’application doit authentifier à nouveau l’utilisateur pour obtenir un nouveau jeton.

En cas d’échec d’une demande de jeton, vous devez décider si vous souhaitez enregistrer un état actuel avant d’effectuer une redirection. Il existe plusieurs approches avec des niveaux de complexité de plus en plus complexes :

* Stocke l’état actuel de la page dans le stockage de session. Pendant l' `OnInitializeAsync`, vérifiez si l’État peut être restauré avant de continuer.
* Ajoutez un paramètre de chaîne de requête et utilisez-le comme méthode pour signaler à l’application qu’elle doit réalimenter l’état enregistré précédemment.
* Ajoutez un paramètre de chaîne de requête avec un identificateur unique pour stocker les données dans le stockage de session sans risquer des collisions avec d’autres éléments.

L’exemple suivant montre comment :

* Conserver l’état avant la redirection vers la page de connexion.
* Récupérez l’état précédent après l’authentification à l’aide du paramètre de chaîne de requête.

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a>Enregistrer l’état de l’application avant une opération d’authentification

Au cours d’une opération d’authentification, il existe des cas où vous souhaitez enregistrer l’état de l’application avant que le navigateur soit redirigé vers l’adresse IP. Cela peut être le cas lorsque vous utilisez un conteneur d’État et que vous souhaitez restaurer l’État une fois l’authentification réussie. Vous pouvez utiliser un objet d’état d’authentification personnalisé pour conserver l’état spécifique à l’application ou une référence à celui-ci, et restaurer cet État une fois l’opération d’authentification terminée.

composant `Authentication` (*pages/Authentication. Razor*) :

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="request-additional-access-tokens"></a>Demander des jetons d’accès supplémentaires

La plupart des applications ont uniquement besoin d’un jeton d’accès pour interagir avec les ressources protégées qu’elles utilisent. Dans certains scénarios, une application peut nécessiter plusieurs jetons pour interagir avec au moins deux ressources. La méthode `IAccessTokenProvider.RequestToken` fournit une surcharge qui permet à une application de configurer un jeton avec un ensemble donné d’étendues, comme illustré dans l’exemple suivant :

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a>Personnaliser les itinéraires de l’application

Par défaut, la bibliothèque de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` utilise les itinéraires indiqués dans le tableau suivant pour représenter des États d’authentification différents.

| Routage                            | Objectif |
| -------------------------------- | ------- |
| `authentication/login`           | Déclenche une opération de connexion. |
| `authentication/login-callback`  | Gère le résultat de toute opération de connexion. |
| `authentication/login-failed`    | Affiche des messages d’erreur lorsque l’opération de connexion échoue pour une raison quelconque. |
| `authentication/logout`          | Déclenche une opération de déconnexion. |
| `authentication/logout-callback` | Gère le résultat d’une opération de déconnexion. |
| `authentication/logout-failed`   | Affiche des messages d’erreur lorsque l’opération de déconnexion échoue pour une raison quelconque. |
| `authentication/logged-out`      | Indique que l’utilisateur a réussi à se déconnecter. |
| `authentication/profile`         | Déclenche une opération de modification du profil utilisateur. |
| `authentication/register`        | Déclenche une opération pour inscrire un nouvel utilisateur. |

Les itinéraires indiqués dans le tableau précédent sont configurables via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`. Quand vous définissez des options pour fournir des itinéraires personnalisés, vérifiez que l’application a un itinéraire qui gère chaque chemin d’accès.

Dans l’exemple suivant, tous les chemins d’accès ont le préfixe `/security`.

composant `Authentication` (*pages/Authentication. Razor*) :

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main` (*Program.cs*) :

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

Si la spécification exige des chemins d’accès complètement différents, définissez les itinéraires comme décrit précédemment et affichez le `RemoteAuthenticatorView` avec un paramètre d’action explicite :

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

Si vous le souhaitez, vous avez la possibilité de scinder l’interface utilisateur en différentes pages.

## <a name="customize-the-authentication-user-interface"></a>Personnaliser l’interface utilisateur de l’authentification

`RemoteAuthenticatorView` comprend un ensemble par défaut d’éléments d’interface utilisateur pour chaque État d’authentification. Chaque État peut être personnalisé en passant une `RenderFragment`personnalisée. Pour personnaliser le texte affiché pendant le processus de connexion initial, vous pouvez modifier les `RemoteAuthenticatorView` comme suit.

composant `Authentication` (*pages/Authentication. Razor*) :

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

Le `RemoteAuthenticatorView` possède un fragment qui peut être utilisé par itinéraire d’authentification, comme indiqué dans le tableau suivant.

| Routage                            | Fragment                |
| -------------------------------- | ----------------------- |
| `authentication/login`           | `<LoggingIn>`           |
| `authentication/login-callback`  | `<CompletingLoggingIn>` |
| `authentication/login-failed`    | `<LogInFailed>`         |
| `authentication/logout`          | `<LogOut>`              |
| `authentication/logout-callback` | `<CompletingLogOut>`    |
| `authentication/logout-failed`   | `<LogOutFailed>`        |
| `authentication/logged-out`      | `<LogOutSucceeded>`     |
| `authentication/profile`         | `<UserProfile>`         |
| `authentication/register`        | `<Registering>`         |
