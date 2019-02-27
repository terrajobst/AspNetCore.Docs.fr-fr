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
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="928ae-103">Authentifier les utilisateurs avec WS-Federation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="928ae-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="928ae-104">Ce didacticiel montre comment permettre aux utilisateurs de se connecter avec un fournisseur d’authentification WS-Federation telles que Active Directory Federation Services (ADFS) ou [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="928ae-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="928ae-105">Elle utilise l’exemple d’application ASP.NET Core 2.0 décrit dans [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="928ae-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="928ae-106">Pour les applications ASP.NET Core 2.0, WS-Federation est prise en charge par [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="928ae-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="928ae-107">Ce composant est porté à partir [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) et partage un grand nombre de passer du stade de ce composant.</span><span class="sxs-lookup"><span data-stu-id="928ae-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="928ae-108">Toutefois, les composants diffèrent de deux manières.</span><span class="sxs-lookup"><span data-stu-id="928ae-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="928ae-109">Par défaut, le middleware de nouvelle :</span><span class="sxs-lookup"><span data-stu-id="928ae-109">By default, the new middleware:</span></span>

* <span data-ttu-id="928ae-110">N’autorise pas les connexions non sollicitées.</span><span class="sxs-lookup"><span data-stu-id="928ae-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="928ae-111">Cette fonctionnalité du protocole WS-Federation est vulnérable aux attaques XSRF.</span><span class="sxs-lookup"><span data-stu-id="928ae-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="928ae-112">Toutefois, il peut être activé avec le `AllowUnsolicitedLogins` option.</span><span class="sxs-lookup"><span data-stu-id="928ae-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="928ae-113">Ne vérifie pas chaque publication de formulaire pour les messages de connexion.</span><span class="sxs-lookup"><span data-stu-id="928ae-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="928ae-114">Seules les requêtes pour le `CallbackPath` sont vérifiées pour l’authentification-ins. `CallbackPath` par défaut est `/signin-wsfed` mais peut être modifié via le hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="928ae-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="928ae-115">Ce chemin d’accès peut être partagé avec d’autres fournisseurs d’authentification en activant le [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span><span class="sxs-lookup"><span data-stu-id="928ae-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="928ae-116">Inscrire l’application auprès d’Active Directory</span><span class="sxs-lookup"><span data-stu-id="928ae-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="928ae-117">Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="928ae-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="928ae-118">Ouvrez le serveur **de partie de confiance Assistant Ajout confiance** à partir de la console de gestion AD FS :</span><span class="sxs-lookup"><span data-stu-id="928ae-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Ajouter la partie de confiance Assistant approbation de partie : Bienvenue](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="928ae-120">Choisir d’entrer manuellement les données :</span><span class="sxs-lookup"><span data-stu-id="928ae-120">Choose to enter data manually:</span></span>

![Ajouter la partie de confiance Assistant approbation de partie : Sélectionnez la Source de données](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="928ae-122">Entrez un nom d’affichage pour la partie de confiance.</span><span class="sxs-lookup"><span data-stu-id="928ae-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="928ae-123">Le nom n’est pas important de l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="928ae-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="928ae-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) ne dispose pas de prise en charge pour le chiffrement de jeton, afin de ne pas configurer un certificat de chiffrement de jeton :</span><span class="sxs-lookup"><span data-stu-id="928ae-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Ajouter la partie de confiance Assistant approbation de partie : Configurer le certificat](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="928ae-126">Activer la prise en charge de protocole passif WS-Federation, à l’aide de l’URL de l’application.</span><span class="sxs-lookup"><span data-stu-id="928ae-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="928ae-127">Vérifiez que le port est correct pour l’application :</span><span class="sxs-lookup"><span data-stu-id="928ae-127">Verify the port is correct for the app:</span></span>

![Ajouter la partie de confiance Assistant approbation de partie : Configurer l'URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="928ae-129">Ce doit être une URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="928ae-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="928ae-130">IIS Express peut fournir un certificat auto-signé lors de l’hébergement de l’application pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="928ae-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="928ae-131">Kestrel nécessite une configuration manuelle des certificats.</span><span class="sxs-lookup"><span data-stu-id="928ae-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="928ae-132">Consultez le [documentation de Kestrel](xref:fundamentals/servers/kestrel) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="928ae-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="928ae-133">Cliquez sur **suivant** via le reste de l’Assistant et **fermer** à la fin.</span><span class="sxs-lookup"><span data-stu-id="928ae-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="928ae-134">ASP.NET Core Identity nécessite un **nom ID** de revendication.</span><span class="sxs-lookup"><span data-stu-id="928ae-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="928ae-135">Ajouter un à partir de la **modifier les règles de revendication** boîte de dialogue :</span><span class="sxs-lookup"><span data-stu-id="928ae-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Modifier les règles de revendication](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="928ae-137">Dans le **ajouter un Assistant de règle de revendication de transformation**, laissez la valeur par défaut **envoyer les attributs LDAP en tant que revendications** modèle sélectionné, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="928ae-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="928ae-138">Ajouter un mappage de règle le **SAM-Account-Name** attribut LDAP à le **nom ID** revendication sortante :</span><span class="sxs-lookup"><span data-stu-id="928ae-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Ajouter un Assistant de règle de revendication de transformation : Configurer la règle de revendication](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="928ae-140">Cliquez sur **Terminer** > **OK** dans le **modifier les règles de revendication** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="928ae-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="928ae-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="928ae-141">Azure Active Directory</span></span>

* <span data-ttu-id="928ae-142">Accédez au panneau inscriptions des applications du locataire AAD.</span><span class="sxs-lookup"><span data-stu-id="928ae-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="928ae-143">Cliquez sur **nouvelle inscription d’application**:</span><span class="sxs-lookup"><span data-stu-id="928ae-143">Click **New application registration**:</span></span>

![Azure Active Directory : Inscriptions d’application](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="928ae-145">Entrez un nom pour l’inscription d’application.</span><span class="sxs-lookup"><span data-stu-id="928ae-145">Enter a name for the app registration.</span></span> <span data-ttu-id="928ae-146">Ce n’est pas important de l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="928ae-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="928ae-147">Entrez l’URL de l’application écoute sur comme le **Sign-on URL**:</span><span class="sxs-lookup"><span data-stu-id="928ae-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory : Créer l’inscription de l’application](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="928ae-149">Cliquez sur **points de terminaison** et notez le **Document de métadonnées de fédération** URL.</span><span class="sxs-lookup"><span data-stu-id="928ae-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="928ae-150">Il s’agit de l’intergiciel de WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="928ae-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory : points de terminaison](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="928ae-152">Accédez à la nouvelle inscription d’application.</span><span class="sxs-lookup"><span data-stu-id="928ae-152">Navigate to the new app registration.</span></span> <span data-ttu-id="928ae-153">Cliquez sur **paramètres** > **propriétés** et prenez note de la **URI ID d’application**.</span><span class="sxs-lookup"><span data-stu-id="928ae-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="928ae-154">Il s’agit de l’intergiciel de WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="928ae-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory : Propriétés d’inscription de l’application](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="928ae-156">Ajouter WS-Federation comme un fournisseur de connexion externe pour ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="928ae-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="928ae-157">Ajouter une dépendance sur [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) au projet.</span><span class="sxs-lookup"><span data-stu-id="928ae-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="928ae-158">Ajouter WS-Federation à `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="928ae-158">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="928ae-159">Se connecter avec WS-Federation</span><span class="sxs-lookup"><span data-stu-id="928ae-159">Log in with WS-Federation</span></span>

<span data-ttu-id="928ae-160">Accédez à l’application et cliquez sur le **connectez-vous** lien dans l’en-tête de navigation.</span><span class="sxs-lookup"><span data-stu-id="928ae-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="928ae-161">Il existe une option pour vous connecter avec WsFederation : ![Page de connexion](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="928ae-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="928ae-162">Avec ADFS comme fournisseur, le bouton redirige vers une page de connexion AD FS : ![Page de connexion AD FS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="928ae-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="928ae-163">Avec Azure Active Directory en tant que le fournisseur, le bouton redirige vers une page de connexion AAD : ![Page de connexion AAD](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="928ae-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="928ae-164">Une réussite de connexion à un nouvel utilisateur redirige vers la page d’application utilisateur d’inscription : ![Page d’inscription](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="928ae-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="928ae-165">Utiliser WS-Federation sans ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="928ae-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="928ae-166">L’intergiciel (middleware) WS-Federation peut être utilisé sans identité.</span><span class="sxs-lookup"><span data-stu-id="928ae-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="928ae-167">Exemple :</span><span class="sxs-lookup"><span data-stu-id="928ae-167">For example:</span></span>

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
