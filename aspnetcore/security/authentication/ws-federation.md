---
title: Authentifier les utilisateurs avec WS-Federation dans ASP.NET Core
author: chlowell
description: Ce didacticiel montre comment utiliser WS-Federation dans une application ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Authentifier les utilisateurs avec WS-Federation dans ASP.NET Core

Ce didacticiel montre comment permettre aux utilisateurs de se connecter avec un fournisseur d’authentification WS-Federation, comme Active Directory Federation Services (ADFS) ou [Azure Active Directory](/azure/active-directory/) (AAD). Elle utilise l’exemple d’application ASP.NET Core 2.0 décrit dans [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).

Pour les applications ASP.NET Core 2.0, la prise en charge de WS-Federation est fournie par [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Ce composant est porté à partir de [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) et partage un grand nombre de mécanismes du composant. Toutefois, les composants diffèrent de deux manières.

Par défaut, l’intergiciel (middleware) nouvelle :

* N’autorise pas les connexions non sollicitées. Cette fonctionnalité du protocole WS-Federation est vulnérable aux attaques XSRF. Toutefois, il peut être activé avec le `AllowUnsolicitedLogins` option.
* Ne vérifie pas chaque publication de formulaire pour les messages de connexion. Demande seulement à la `CallbackPath` sont vérifiées pour l’authentification-ins. `CallbackPath` par défaut est `/signin-wsfed` mais peut être modifié. Ce chemin d’accès peut être partagée avec d’autres fournisseurs d’authentification en activant la `SkipUnrecognizedRequests` option.

## <a name="register-the-app-with-active-directory"></a>Inscription de l’application avec Active Directory

### <a name="active-directory-federation-services"></a>Active Directory Federation Services

* Ouvrez le serveur **Assistant Ajouter une partie confiance** à partir de la console de gestion des services AD FS :

![Ajoutez la partie de confiance confiance Assistant : accueil](ws-federation/_static/AdfsAddTrust.png)

* Choisissez cette option pour entrer manuellement les données :

![Ajouter l’Assistant d’approbation de partie de confiance : Sélectionnez la Source de données](ws-federation/_static/AdfsSelectDataSource.png)

* Entrez un nom d’affichage pour la partie de confiance. Le nom n’est pas important de l’application ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) ne dispose pas de prise en charge pour le chiffrement des jetons, afin de ne pas configurer un certificat de chiffrement du jeton :

![Ajouter l’Assistant d’approbation de partie de confiance : Configurer des certificats](ws-federation/_static/AdfsConfigureCert.png)

* Activer la prise en charge de protocole WS-Federation passif, à l’aide des URL de l’application. Vérifiez que le port est correct pour l’application :

![Ajouter l’Assistant d’approbation de partie de confiance : Configurer des URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Ce doit être une URL HTTPS. IIS Express peut fournir un certificat auto-signé lors de l’hébergement de l’application pendant le développement. Kestrel requiert une configuration manuelle des certificats. Consultez le [documentation de Kestrel](xref:fundamentals/servers/kestrel) pour plus d’informations.

* Cliquez sur **suivant** jusqu'à la fin de l’Assistant et **fermer** à la fin.

* Identité ASP.NET Core nécessite un **ID de nom** de revendication. Ajoutez-en une à partir de la **modifier les règles de revendication** boîte de dialogue :

![Modifier les règles de revendication](ws-federation/_static/EditClaimRules.png)

* Dans le **ajouter un Assistant de règle de revendication transformer**, laissez la valeur par défaut **envoyer les attributs LDAP en tant que revendications** modèle sélectionné, puis cliquez sur **suivant**. Ajouter une règle de mappage du **nom de compte SAM** attribut LDAP à le **ID de nom** revendication sortante :

![Ajouter un Assistant de règle de revendication de transformation : Configurer la règle de revendication](ws-federation/_static/AddTransformClaimRule.png)

* Cliquez sur **Terminer** > **OK** dans les **modifier les règles de revendication** fenêtre.

### <a name="azure-active-directory"></a>Azure Active Directory

* Accédez au panneau des inscriptions des applications du locataire AAD. Cliquez sur **nouvelle inscription de l’application**:

![Azure Active Directory : Inscriptions d’application](ws-federation/_static/AadNewAppRegistration.png)

* Entrez un nom pour l’inscription de l’application. Cela n’est pas important de l’application ASP.NET Core.
* Entrez l’URL de l’application écoute sur que le **URL de connexion**:

![Azure Active Directory : Créer l’inscription d’une application](ws-federation/_static/AadCreateAppRegistration.png)

* Cliquez sur **points de terminaison** et notez la **Document de métadonnées de fédération** URL. Il s’agit de l’intergiciel de WS-Federation `MetadataAddress`:

![Azure Active Directory : points de terminaison](ws-federation/_static/AadFederationMetadataDocument.png)

* Accédez à la nouvelle inscription de l’application. Cliquez sur **paramètres** > **propriétés** et notez la **URI ID d’application**. Il s’agit de l’intergiciel de WS-Federation `Wtrealm`:

![Azure Active Directory : Propriétés d’inscription de l’application](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Ajouter WS-Federation en tant qu’un fournisseur de connexion externe pour ASP.NET Core Identity

* Ajouter une dépendance sur [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) au projet.
* Ajouter WS-Federation pour le `Configure` méthode dans *Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
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

Accédez à l’application et cliquez sur le **connecter** lien dans l’en-tête de navigation. Il existe une option pour vous connecter avec WsFederation : ![page de connexion](ws-federation/_static/WsFederationButton.png)

Avec AD FS en tant que le fournisseur, le bouton redirige vers une page de connexion AD FS : ![page de connexion AD FS](ws-federation/_static/AdfsLoginPage.png)

Avec Azure Active Directory en tant que le fournisseur, le bouton redirige vers une page de connexion à AAD : ![AAD-page de connexion](ws-federation/_static/AadSignIn.png)

Une réussite sign-in pour un nouvel utilisateur redirige vers la page d’inscription de l’application utilisateur : ![page d’inscription](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Utiliser WS-Federation sans ASP.NET Core identité

L’intergiciel (middleware) WS-Federation peut être utilisé sans identité. Par exemple :

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
