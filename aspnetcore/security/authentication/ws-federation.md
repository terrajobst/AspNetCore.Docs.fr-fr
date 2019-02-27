---
title: Authentifier les utilisateurs avec WS-Federation dans ASP.NET Core
author: chlowell
description: Ce didacticiel montre comment utiliser WS-Federation dans une application ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 7967410686da0e59742b721c0154e143bf19ba01
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833538"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Authentifier les utilisateurs avec WS-Federation dans ASP.NET Core

Ce didacticiel montre comment permettre aux utilisateurs de se connecter avec un fournisseur d’authentification WS-Federation telles que Active Directory Federation Services (ADFS) ou [Azure Active Directory](/azure/active-directory/) (AAD). Elle utilise l’exemple d’application ASP.NET Core 2.0 décrit dans [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).

Pour les applications ASP.NET Core 2.0, WS-Federation est prise en charge par [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Ce composant est porté à partir [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) et partage un grand nombre de passer du stade de ce composant. Toutefois, les composants diffèrent de deux manières.

Par défaut, le middleware de nouvelle :

* N’autorise pas les connexions non sollicitées. Cette fonctionnalité du protocole WS-Federation est vulnérable aux attaques XSRF. Toutefois, il peut être activé avec le `AllowUnsolicitedLogins` option.
* Ne vérifie pas chaque publication de formulaire pour les messages de connexion. Seules les requêtes pour le `CallbackPath` sont vérifiées pour l’authentification-ins. `CallbackPath` par défaut est `/signin-wsfed` mais peut être modifié via le hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) classe. Ce chemin d’accès peut être partagé avec d’autres fournisseurs d’authentification en activant le [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.

## <a name="register-the-app-with-active-directory"></a>Inscrire l’application auprès d’Active Directory

### <a name="active-directory-federation-services"></a>Active Directory Federation Services

* Ouvrez le serveur **de partie de confiance Assistant Ajout confiance** à partir de la console de gestion AD FS :

![Ajouter la partie de confiance Assistant approbation de partie : Bienvenue](ws-federation/_static/AdfsAddTrust.png)

* Choisir d’entrer manuellement les données :

![Ajouter la partie de confiance Assistant approbation de partie : Sélectionnez la Source de données](ws-federation/_static/AdfsSelectDataSource.png)

* Entrez un nom d’affichage pour la partie de confiance. Le nom n’est pas important de l’application ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) ne dispose pas de prise en charge pour le chiffrement de jeton, afin de ne pas configurer un certificat de chiffrement de jeton :

![Ajouter la partie de confiance Assistant approbation de partie : Configurer le certificat](ws-federation/_static/AdfsConfigureCert.png)

* Activer la prise en charge de protocole passif WS-Federation, à l’aide de l’URL de l’application. Vérifiez que le port est correct pour l’application :

![Ajouter la partie de confiance Assistant approbation de partie : Configurer l'URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Ce doit être une URL HTTPS. IIS Express peut fournir un certificat auto-signé lors de l’hébergement de l’application pendant le développement. Kestrel nécessite une configuration manuelle des certificats. Consultez le [documentation de Kestrel](xref:fundamentals/servers/kestrel) pour plus d’informations.

* Cliquez sur **suivant** via le reste de l’Assistant et **fermer** à la fin.

* ASP.NET Core Identity nécessite un **nom ID** de revendication. Ajouter un à partir de la **modifier les règles de revendication** boîte de dialogue :

![Modifier les règles de revendication](ws-federation/_static/EditClaimRules.png)

* Dans le **ajouter un Assistant de règle de revendication de transformation**, laissez la valeur par défaut **envoyer les attributs LDAP en tant que revendications** modèle sélectionné, puis cliquez sur **suivant**. Ajouter un mappage de règle le **SAM-Account-Name** attribut LDAP à le **nom ID** revendication sortante :

![Ajouter un Assistant de règle de revendication de transformation : Configurer la règle de revendication](ws-federation/_static/AddTransformClaimRule.png)

* Cliquez sur **Terminer** > **OK** dans le **modifier les règles de revendication** fenêtre.

### <a name="azure-active-directory"></a>Azure Active Directory

* Accédez au panneau inscriptions des applications du locataire AAD. Cliquez sur **nouvelle inscription d’application**:

![Azure Active Directory : Inscriptions d’application](ws-federation/_static/AadNewAppRegistration.png)

* Entrez un nom pour l’inscription d’application. Ce n’est pas important de l’application ASP.NET Core.
* Entrez l’URL de l’application écoute sur comme le **Sign-on URL**:

![Azure Active Directory : Créer l’inscription de l’application](ws-federation/_static/AadCreateAppRegistration.png)

* Cliquez sur **points de terminaison** et notez le **Document de métadonnées de fédération** URL. Il s’agit de l’intergiciel de WS-Federation `MetadataAddress`:

![Azure Active Directory : points de terminaison](ws-federation/_static/AadFederationMetadataDocument.png)

* Accédez à la nouvelle inscription d’application. Cliquez sur **paramètres** > **propriétés** et prenez note de la **URI ID d’application**. Il s’agit de l’intergiciel de WS-Federation `Wtrealm`:

![Azure Active Directory : Propriétés d’inscription de l’application](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Ajouter WS-Federation comme un fournisseur de connexion externe pour ASP.NET Core Identity

* Ajouter une dépendance sur [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) au projet.
* Ajouter WS-Federation à `Startup.ConfigureServices`:

    ```csharp
    services.AddIdentity<IdentityUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Se connecter avec WS-Federation

Accédez à l’application et cliquez sur le **connectez-vous** lien dans l’en-tête de navigation. Il existe une option pour vous connecter avec WsFederation : ![Page de connexion](ws-federation/_static/WsFederationButton.png)

Avec ADFS comme fournisseur, le bouton redirige vers une page de connexion AD FS : ![Page de connexion AD FS](ws-federation/_static/AdfsLoginPage.png)

Avec Azure Active Directory en tant que le fournisseur, le bouton redirige vers une page de connexion AAD : ![Page de connexion AAD](ws-federation/_static/AadSignIn.png)

Une réussite de connexion à un nouvel utilisateur redirige vers la page d’application utilisateur d’inscription : ![Page d’inscription](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Utiliser WS-Federation sans ASP.NET Core Identity

L’intergiciel (middleware) WS-Federation peut être utilisé sans identité. Exemple :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
