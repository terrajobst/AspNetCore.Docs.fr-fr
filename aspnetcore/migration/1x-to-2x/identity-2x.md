---
title: Migrer d’authentification et identité vers ASP.NET Core 2.0
author: scottaddie
description: Cet article décrit les étapes les plus courantes pour migration ASP.NET Core 1.x l’authentification et identité vers ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: c83356e12fa5ae581b369265b9d857b08445ed51
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313746"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Migrer d’authentification et identité vers ASP.NET Core 2.0

Par [Scott Addie](https://github.com/scottaddie) et [arts martiaux Hao](https://github.com/HaoK)

ASP.NET Core 2.0 offre un nouveau modèle pour l’authentification et [identité](xref:security/authentication/identity) qui simplifie la configuration à l’aide des services. Les applications ASP.NET Core 1.x qui utilisent l’authentification ou Identity peuvent être mis à jour pour utiliser le nouveau modèle, comme indiqué ci-dessous.

## <a name="update-namespaces"></a>Mettre à jour les espaces de noms

Dans la version 1.x, les classes telles `IdentityRole` et `IdentityUser` ont été trouvés dans le `Microsoft.AspNetCore.Identity.EntityFrameworkCore` espace de noms.

Dans la version 2.0, le <xref:Microsoft.AspNetCore.Identity> espace de noms est devenue la nouvelle page d’accueil pour plusieurs de ces classes. Avec le code d’identité par défaut, les classes concernées incluent `ApplicationUser` et `Startup`. Ajuster votre `using` instructions pour résoudre les références concernées.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Intergiciel (middleware) d’authentification et services

Dans les projets 1.x, l’authentification est configurée via l’intergiciel. Une méthode de l’intergiciel (middleware) est appelée pour chaque schéma d’authentification que vous souhaitez prendre en charge.

L’exemple de 1.x suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:

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

Dans les 2.0 projets, l’authentification est configurée par le biais de services. Chaque schéma d’authentification est enregistré dans le `ConfigureServices` méthode de *Startup.cs*. Le `UseIdentity` méthode est remplacée par `UseAuthentication`.

L’exemple suivant 2.0 configure l’authentification Facebook avec l’identité dans *Startup.cs*:

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

Le `UseAuthentication` méthode ajoute un composant d’intergiciel d’authentification unique, qui est responsable de l’authentification automatique et la gestion des demandes d’authentification à distance. Il remplace tous les composants d’intergiciel (middleware) individuels par un composant d’intergiciel (middleware) unique et commun.

Vous trouverez ci-dessous des instructions de migration 2.0 pour chaque schéma d’authentification principale.

### <a name="cookie-based-authentication"></a>Authentification basée sur les cookies

Sélectionnez une des deux options ci-dessous et apportez les modifications nécessaires dans *Startup.cs*:

1. Utilisation des cookies avec l’identité
    - Remplacez `UseIdentity` avec `UseAuthentication` dans le `Configure` méthode :

        ```csharp
        app.UseAuthentication();
        ```

    - Appeler le `AddIdentity` méthode dans le `ConfigureServices` méthode pour ajouter les services d’authentification de cookie.
    - Si vous le souhaitez, appelez le `ConfigureApplicationCookie` ou `ConfigureExternalCookie` méthode dans le `ConfigureServices` méthode pour modifier les paramètres de cookies d’identité.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Utilisation des cookies sans Identity
    - Remplacez le `UseCookieAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Appeler le `AddAuthentication` et `AddCookie` méthodes dans le `ConfigureServices` méthode :

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

### <a name="jwt-bearer-authentication"></a>Authentification de porteur JWT

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez le `UseJwtBearerAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appeler le `AddJwtBearer` méthode dans le `ConfigureServices` méthode :

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Cet extrait de code n’utilise pas l’identité, le schéma par défaut doivent donc être défini en passant `JwtBearerDefaults.AuthenticationScheme` à la `AddAuthentication` (méthode).

### <a name="openid-connect-oidc-authentication"></a>Authentification OpenID Connect (OIDC)

Apportez les modifications suivantes dans *Startup.cs*:

- Remplacez le `UseOpenIdConnectAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appeler le `AddOpenIdConnect` méthode dans le `ConfigureServices` méthode :

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

- Remplacez le `PostLogoutRedirectUri` propriété dans le `OpenIdConnectOptions` action avec `SignedOutRedirectUri`:

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a>Authentification Facebook

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez le `UseFacebookAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appeler le `AddFacebook` méthode dans le `ConfigureServices` méthode :

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
- Remplacez le `UseGoogleAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appeler le `AddGoogle` méthode dans le `ConfigureServices` méthode :

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Authentification de Microsoft Account

Apportez les modifications suivantes dans *Startup.cs*:
- Remplacez le `UseMicrosoftAccountAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appeler le `AddMicrosoftAccount` méthode dans le `ConfigureServices` méthode :

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
- Remplacez le `UseTwitterAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Appeler le `AddTwitter` méthode dans le `ConfigureServices` méthode :

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Définition des schémas d’authentification par défaut

Dans la version 1.x, le `AutomaticAuthenticate` et `AutomaticChallenge` propriétés de la [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe de base ont été conçue pour être définie sur un schéma d’authentification unique. Il n’a aucune bonne façon d’appliquer cette recommandation.

Dans la version 2.0, ces deux propriétés ont été supprimées en tant que propriétés sur chaque `AuthenticationOptions` instance. Ils peuvent être configurés dans le `AddAuthentication` appel de méthode dans le `ConfigureServices` méthode de *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Dans l’extrait de code précédent, le schéma par défaut est défini sur `CookieAuthenticationDefaults.AuthenticationScheme` (« Cookies »).

Vous pouvez également utiliser une version surchargée de la `AddAuthentication` pour définir plusieurs propriétés. Dans l’exemple suivant de la méthode surchargée, le schéma par défaut a la valeur `CookieAuthenticationDefaults.AuthenticationScheme`. Le schéma d’authentification peut également être spécifié dans votre personne `[Authorize]` attributs ou des stratégies d’autorisation.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Définir un schéma par défaut dans 2.0 si une des conditions suivantes est remplie :
- Vous souhaitez que l’utilisateur de se connecter automatiquement
- Vous utilisez le `[Authorize]` stratégies d’attribut ou d’autorisation sans spécifier de schémas

Une exception à cette règle est la `AddIdentity` (méthode). Cette méthode ajoute des cookies pour vous et définit la valeur par défaut s’authentifier et défiez les schémas au cookie application `IdentityConstants.ApplicationScheme`. En outre, il définit le schéma de connexion par défaut sur le cookie externe `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Utiliser des extensions d’authentification HttpContext

Le `IAuthenticationManager` interface est le point d’entrée principal dans le système d’authentification 1.x. Il a été remplacé par un nouvel ensemble de `HttpContext` méthodes d’extension dans le `Microsoft.AspNetCore.Authentication` espace de noms.

Par exemple, les projets 1.x référence un `Authentication` propriété :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Dans les 2.0 projets, vous devez importer le `Microsoft.AspNetCore.Authentication` espace de noms et supprimer le `Authentication` références de propriété :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>L’authentification Windows (HTTP.sys / IISIntegration)

Il existe deux variantes de l’authentification Windows :

* L’hôte autorise uniquement les utilisateurs authentifiés. Cette variation n’est pas affectée par les modifications de 2.0.
* L’hôte permet à la fois anonymes et les utilisateurs authentifiés. Cette variation est affectée par les modifications de 2.0. Par exemple, l’application doit autoriser les utilisateurs anonymes à le [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys) de couche, mais autoriser les utilisateurs au niveau du contrôleur. Dans ce scénario, définissez le schéma par défaut dans le `Startup.ConfigureServices` (méthode).

  Pour [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), le schéma par défaut la valeur `IISDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  Pour [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), le schéma par défaut la valeur `HttpSysDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  Impossible de définir le schéma par défaut empêche la demande authorize (test) de fonctionner avec l’exception suivante :

  > `System.InvalidOperationException`: Aucun authenticationScheme a été spécifié et il n’a aucun DefaultChallengeScheme trouvé.

Pour plus d'informations, consultez <xref:security/authentication/windowsauth>.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Instances de IdentityCookieOptions

Un effet secondaire des 2.0 modifications est le paramètre permet à l’aide de ces options au lieu d’instances d’options de cookie. La possibilité de personnaliser les noms de schéma identité cookie est supprimée.

Par exemple, des projets 1.x utilisent [l’injection de constructeur](xref:mvc/controllers/dependency-injection#constructor-injection) à passer un `IdentityCookieOptions` paramètre dans *AccountController.cs* et *ManageController.cs*. Le schéma d’authentification externe cookie est accessible à partir de l’instance fournie :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

L’injection de constructeur mentionnés ci-dessus devienne inutile dans les 2.0 projets et le `_externalCookieScheme` champ peut être supprimé :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

les projets 1.x utilisés le `_externalCookieScheme` champ comme suit :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Dans les 2.0 projets, remplacez le code précédent avec les éléments suivants. Le `IdentityConstants.ExternalScheme` constante peut être utilisée directement.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Résoudre récemment ajouté `SignOutAsync` appeler en important l’espace de noms suivant :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Ajouter des propriétés de navigation IdentityUser POCO

Les propriétés de navigation d’Entity Framework (EF) Core de la base de `IdentityUser` POCO (Plain Old CLR Object) ont été supprimés. Si votre projet 1.x utilisé ces propriétés, les ajouter manuellement au projet 2.0 :

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

Pour empêcher les clés étrangères en double lors de l’exécution des Migrations d’EF Core, ajoutez le code suivant à votre `IdentityDbContext` classe `OnModelCreating` (méthode) (une fois que le `base.OnModelCreating();` appeler) :

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

## <a name="replace-getexternalauthenticationschemes"></a>Remplacez GetExternalAuthenticationSchemes

La méthode synchrone `GetExternalAuthenticationSchemes` a été supprimé en faveur d’une version asynchrone. les projets 1.x ont le code suivant *Controllers/ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Cette méthode s’affiche dans *Views/Account/Login.cshtml* trop :

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

Dans les 2.0 projets, utilisez le <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> (méthode). La modification de *ManageController.cs* ressemble au code suivant :

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

Dans *Login.cshtml*, le `AuthenticationScheme` propriété accédée dans le `foreach` boucle devient `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Modification de propriété ManageLoginsViewModel

Un `ManageLoginsViewModel` objet est utilisé dans le `ManageLogins` action de *ManageController.cs*. Dans les projets 1.x, l’objet `OtherLogins` propriété type de retour est `IList<AuthenticationDescription>`. Ce type de retour nécessite une importation de `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Dans les 2.0 projets, le type de retour devient `IList<AuthenticationScheme>`. Ce nouveau type de retour nécessite en remplaçant le `Microsoft.AspNetCore.Http.Authentication` importer avec un `Microsoft.AspNetCore.Authentication` importer.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations, consultez le [Discussion pour Auth 2.0](https://github.com/aspnet/Security/issues/1338) problème sur GitHub.
