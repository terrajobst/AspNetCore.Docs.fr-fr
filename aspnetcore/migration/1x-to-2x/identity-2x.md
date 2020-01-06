---
title: Migrer l’authentification et l’identité vers ASP.NET Core 2,0
author: scottaddie
description: Cet article décrit les étapes les plus courantes pour la migration de l’authentification et de l’identité ASP.NET Core 1. x vers ASP.NET Core 2,0.
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: f3817fa1808c331f7e167618e3bb00d68ad08571
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355178"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Migrer l’authentification et l’identité vers ASP.NET Core 2,0

Par [Scott Addie](https://github.com/scottaddie) et [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2,0 dispose d’un nouveau modèle d’authentification et d' [identité](xref:security/authentication/identity) qui simplifie la configuration à l’aide de services. ASP.NET Core les applications 1. x qui utilisent l’authentification ou l’identité peuvent être mises à jour pour utiliser le nouveau modèle comme indiqué ci-dessous.

## <a name="update-namespaces"></a>Mettre à jour les espaces de noms

Dans 1. x, les classes telles que `IdentityRole` et `IdentityUser` ont été trouvées dans l’espace de noms `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Dans 2,0, l’espace de noms <xref:Microsoft.AspNetCore.Identity> est devenu la nouvelle page d’hébergement pour plusieurs de ces classes. Avec le code d’identité par défaut, les classes affectées incluent `ApplicationUser` et `Startup`. Ajustez vos instructions `using` pour résoudre les références affectées.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Intergiciel et services d’authentification

Dans les projets 1. x, l’authentification est configurée à l’aide d’un intergiciel (middleware). Une méthode d’intergiciel est appelée pour chaque schéma d’authentification que vous souhaitez prendre en charge.

L’exemple 1. x suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

Dans les projets 2,0, l’authentification est configurée via les services. Chaque schéma d’authentification est enregistré dans la méthode `ConfigureServices` de *Startup.cs*. La méthode `UseIdentity` est remplacée par `UseAuthentication`.

L’exemple 2,0 suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

La méthode `UseAuthentication` ajoute un seul composant d’intergiciel d’authentification, qui est responsable de l’authentification automatique et de la gestion des demandes d’authentification à distance. Il remplace tous les composants de l’intergiciel (middleware) individuels par un seul composant de middleware commun.

Vous trouverez ci-dessous des instructions de migration de 2,0 pour chaque schéma d’authentification principal.

### <a name="cookie-based-authentication"></a>Authentification basée sur les cookies

Sélectionnez l’une des deux options ci-dessous et apportez les modifications nécessaires dans *Startup.cs*:

1. Utiliser des cookies avec une identité
    - Remplacez `UseIdentity` par `UseAuthentication` dans la méthode `Configure` :

        ```csharp
        app.UseAuthentication();
        ```

    - Appelez la méthode `AddIdentity` dans la méthode `ConfigureServices` pour ajouter les services d’authentification de cookie.
    - Vous pouvez également appeler la méthode `ConfigureApplicationCookie` ou `ConfigureExternalCookie` dans la méthode `ConfigureServices` pour affiner les paramètres de cookie d’identité.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Utiliser des cookies sans identité
    - Remplacez l’appel de méthode `UseCookieAuthentication` dans la méthode `Configure` par `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Appelez les méthodes `AddAuthentication` et `AddCookie` dans la méthode `ConfigureServices` :

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a>Authentification du porteur JWT

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez l’appel de méthode `UseJwtBearerAuthentication` dans la méthode `Configure` par `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appelez la méthode `AddJwtBearer` dans la méthode `ConfigureServices` :

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Cet extrait de code n’utilise pas d’identité. par conséquent, le schéma par défaut doit être défini en passant `JwtBearerDefaults.AuthenticationScheme` à la méthode `AddAuthentication`.

### <a name="openid-connect-oidc-authentication"></a>Authentification OpenID Connect (OIDC)

Apportez les modifications suivantes dans *Startup.cs*:

- Remplacez l’appel de méthode `UseOpenIdConnectAuthentication` dans la méthode `Configure` par `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appelez la méthode `AddOpenIdConnect` dans la méthode `ConfigureServices` :

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

- Remplacez la propriété `PostLogoutRedirectUri` dans l’action `OpenIdConnectOptions` par `SignedOutRedirectUri`:

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a>Authentification Facebook

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez l’appel de méthode `UseFacebookAuthentication` dans la méthode `Configure` par `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appelez la méthode `AddFacebook` dans la méthode `ConfigureServices` :

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Authentification Google

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez l’appel de méthode `UseGoogleAuthentication` dans la méthode `Configure` par `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appelez la méthode `AddGoogle` dans la méthode `ConfigureServices` :

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Authentification Microsoft

Pour plus d’informations sur l’authentification compte Microsoft, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/14455).

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez l’appel de méthode `UseMicrosoftAccountAuthentication` dans la méthode `Configure` par `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appelez la méthode `AddMicrosoftAccount` dans la méthode `ConfigureServices` :

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Authentification Twitter

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez l’appel de méthode `UseTwitterAuthentication` dans la méthode `Configure` par `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appelez la méthode `AddTwitter` dans la méthode `ConfigureServices` :

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Définition des schémas d’authentification par défaut

Dans 1. x, les propriétés `AutomaticAuthenticate` et `AutomaticChallenge` de la classe de base [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) étaient destinées à être définies sur un seul schéma d’authentification. Il n’existait pas de bonne méthode pour l’appliquer.

Dans 2,0, ces deux propriétés ont été supprimées en tant que propriétés sur l’instance de `AuthenticationOptions` individuelle. Ils peuvent être configurés dans l’appel de méthode `AddAuthentication` au sein de la méthode `ConfigureServices` de *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Dans l’extrait de code précédent, le schéma par défaut est défini sur `CookieAuthenticationDefaults.AuthenticationScheme` (« cookies »).

Vous pouvez également utiliser une version surchargée de la méthode `AddAuthentication` pour définir plusieurs propriétés. Dans l’exemple de méthode surchargée suivant, le schéma par défaut est défini sur `CookieAuthenticationDefaults.AuthenticationScheme`. Le schéma d’authentification peut également être spécifié au sein de vos propres `[Authorize]` attributs ou stratégies d’autorisation.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Définissez un schéma par défaut dans 2,0 si l’une des conditions suivantes est remplie :
- Vous souhaitez que l’utilisateur soit automatiquement connecté
- Vous utilisez l’attribut `[Authorize]` ou les stratégies d’autorisation sans spécifier de schémas

La méthode `AddIdentity` est une exception à cette règle. Cette méthode ajoute des cookies pour vous et définit les schémas d’authentification et de stimulation par défaut sur le cookie d’application `IdentityConstants.ApplicationScheme`. En outre, il définit le schéma de connexion par défaut sur le cookie externe `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Utiliser les extensions d’authentification HttpContext

L’interface `IAuthenticationManager` est le point d’entrée principal dans le système d’authentification 1. x. Il a été remplacé par un nouvel ensemble de méthodes d’extension `HttpContext` dans l’espace de noms `Microsoft.AspNetCore.Authentication`.

Par exemple, les projets 1. x font référence à une propriété de `Authentication` :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Dans les projets 2,0, importez l’espace de noms `Microsoft.AspNetCore.Authentication` et supprimez les références de propriété `Authentication` :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Authentification Windows (HTTP. sys/IISIntegration)

Il existe deux variantes de l’authentification Windows :

* L’hôte autorise uniquement les utilisateurs authentifiés. Cette variation n’est pas affectée par les modifications apportées à 2,0.
* L’hôte autorise les utilisateurs anonymes et authentifiés. Cette variation est affectée par les modifications apportées à 2,0. Par exemple, l’application doit autoriser les utilisateurs anonymes au niveau de la couche [IIS](xref:host-and-deploy/iis/index) ou [http. sys](xref:fundamentals/servers/httpsys) , mais autoriser les utilisateurs au niveau du contrôleur. Dans ce scénario, définissez le schéma par défaut dans la méthode `Startup.ConfigureServices`.

  Pour [Microsoft. AspNetCore. Server. IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), définissez le schéma par défaut sur `IISDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  Pour [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), définissez le schéma par défaut sur `HttpSysDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  Si vous ne définissez pas le schéma par défaut, la demande Authorize (Challenge) ne pourra pas fonctionner avec l’exception suivante :

  > `System.InvalidOperationException`: aucun authenticationScheme n’a été spécifié et aucun DefaultChallengeScheme n’a été trouvé.

Pour plus d'informations, consultez <xref:security/authentication/windowsauth>.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Instances IdentityCookieOptions

Un effet secondaire des modifications 2,0 est le passage à l’utilisation des options nommées à la place des instances d’options de cookie. La possibilité de personnaliser les noms de schéma de cookie d’identité est supprimée.

Par exemple, les projets 1. x utilisent l' [injection de constructeur](xref:mvc/controllers/dependency-injection#constructor-injection) pour passer un paramètre `IdentityCookieOptions` dans *AccountController.cs* et *ManageController.cs*. Le schéma d’authentification de cookie externe est accessible à partir de l’instance fournie :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

L’injection de constructeur ci-dessus devient inutile dans les projets 2,0 et le champ `_externalCookieScheme` peut être supprimé :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

les projets 1. x utilisaient le champ `_externalCookieScheme` comme suit :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Dans les projets 2,0, remplacez le code précédent par ce qui suit. La constante `IdentityConstants.ExternalScheme` peut être utilisée directement.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Résolvez l’appel de `SignOutAsync` récemment ajouté en important l’espace de noms suivant :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Ajouter des propriétés de navigation IdentityUser POCO

Les propriétés de navigation principales du Entity Framework (EF) de l' `IdentityUser` de base POCO (Plain Old CLR Object) ont été supprimées. Si votre projet 1. x a utilisé ces propriétés, rajoutez-les manuellement au projet 2,0 :

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

Pour éviter les doublons de clés étrangères lors de l’exécution de EF Core migrations, ajoutez le code suivant à la méthode `OnModelCreating` de `IdentityDbContext` de la classe (après l’appel `base.OnModelCreating();`) :

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a>Remplacer GetExternalAuthenticationSchemes

La méthode synchrone `GetExternalAuthenticationSchemes` a été supprimée en faveur d’une version asynchrone. les projets 1. x ont le code suivant dans *Controllers/ManageController. cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Cette méthode apparaît également dans *views/Account/login. cshtml* :

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

Dans les projets 2,0, utilisez la méthode <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*>. La modification dans *ManageController.cs* ressemble au code suivant :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

Dans *login. cshtml*, la propriété `AuthenticationScheme` accessible dans la boucle `foreach` passe à `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Modification de la propriété ManageLoginsViewModel

Un objet `ManageLoginsViewModel` est utilisé dans l’action `ManageLogins` de *ManageController.cs*. Dans les projets 1. x, le type de retour de la propriété `OtherLogins` de l’objet est `IList<AuthenticationDescription>`. Ce type de retour nécessite une importation de `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Dans les projets 2,0, le type de retour devient `IList<AuthenticationScheme>`. Ce nouveau type de retour nécessite le remplacement de l’importation `Microsoft.AspNetCore.Http.Authentication` par une importation `Microsoft.AspNetCore.Authentication`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations, consultez la discussion sur le problème [Auth 2,0](https://github.com/aspnet/Security/issues/1338) sur GitHub.
