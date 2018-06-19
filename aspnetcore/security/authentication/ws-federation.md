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
ms.locfileid: "30898802"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="12dd8-103">Authentifier les utilisateurs avec WS-Federation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12dd8-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="12dd8-104">Ce didacticiel montre comment permettre aux utilisateurs de se connecter avec un fournisseur d’authentification WS-Federation, comme Active Directory Federation Services (ADFS) ou [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="12dd8-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="12dd8-105">Elle utilise l’exemple d’application ASP.NET Core 2.0 décrit dans [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="12dd8-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="12dd8-106">Pour les applications ASP.NET Core 2.0, la prise en charge de WS-Federation est fournie par [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="12dd8-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="12dd8-107">Ce composant est porté à partir de [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) et partage un grand nombre de mécanismes du composant.</span><span class="sxs-lookup"><span data-stu-id="12dd8-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="12dd8-108">Toutefois, les composants diffèrent de deux manières.</span><span class="sxs-lookup"><span data-stu-id="12dd8-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="12dd8-109">Par défaut, l’intergiciel (middleware) nouvelle :</span><span class="sxs-lookup"><span data-stu-id="12dd8-109">By default, the new middleware:</span></span>

* <span data-ttu-id="12dd8-110">N’autorise pas les connexions non sollicitées.</span><span class="sxs-lookup"><span data-stu-id="12dd8-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="12dd8-111">Cette fonctionnalité du protocole WS-Federation est vulnérable aux attaques XSRF.</span><span class="sxs-lookup"><span data-stu-id="12dd8-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="12dd8-112">Toutefois, il peut être activé avec le `AllowUnsolicitedLogins` option.</span><span class="sxs-lookup"><span data-stu-id="12dd8-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="12dd8-113">Ne vérifie pas chaque publication de formulaire pour les messages de connexion.</span><span class="sxs-lookup"><span data-stu-id="12dd8-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="12dd8-114">Demande seulement à la `CallbackPath` sont vérifiées pour l’authentification-ins. `CallbackPath` par défaut est `/signin-wsfed` mais peut être modifié.</span><span class="sxs-lookup"><span data-stu-id="12dd8-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="12dd8-115">Ce chemin d’accès peut être partagée avec d’autres fournisseurs d’authentification en activant la `SkipUnrecognizedRequests` option.</span><span class="sxs-lookup"><span data-stu-id="12dd8-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="12dd8-116">Inscription de l’application avec Active Directory</span><span class="sxs-lookup"><span data-stu-id="12dd8-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="12dd8-117">Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="12dd8-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="12dd8-118">Ouvrez le serveur **Assistant Ajouter une partie confiance** à partir de la console de gestion des services AD FS :</span><span class="sxs-lookup"><span data-stu-id="12dd8-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Ajoutez la partie de confiance confiance Assistant : accueil](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="12dd8-120">Choisissez cette option pour entrer manuellement les données :</span><span class="sxs-lookup"><span data-stu-id="12dd8-120">Choose to enter data manually:</span></span>

![Ajouter l’Assistant d’approbation de partie de confiance : Sélectionnez la Source de données](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="12dd8-122">Entrez un nom d’affichage pour la partie de confiance.</span><span class="sxs-lookup"><span data-stu-id="12dd8-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="12dd8-123">Le nom n’est pas important de l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12dd8-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="12dd8-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) ne dispose pas de prise en charge pour le chiffrement des jetons, afin de ne pas configurer un certificat de chiffrement du jeton :</span><span class="sxs-lookup"><span data-stu-id="12dd8-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Ajouter l’Assistant d’approbation de partie de confiance : Configurer des certificats](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="12dd8-126">Activer la prise en charge de protocole WS-Federation passif, à l’aide des URL de l’application.</span><span class="sxs-lookup"><span data-stu-id="12dd8-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="12dd8-127">Vérifiez que le port est correct pour l’application :</span><span class="sxs-lookup"><span data-stu-id="12dd8-127">Verify the port is correct for the app:</span></span>

![Ajouter l’Assistant d’approbation de partie de confiance : Configurer des URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="12dd8-129">Ce doit être une URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="12dd8-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="12dd8-130">IIS Express peut fournir un certificat auto-signé lors de l’hébergement de l’application pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="12dd8-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="12dd8-131">Kestrel requiert une configuration manuelle des certificats.</span><span class="sxs-lookup"><span data-stu-id="12dd8-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="12dd8-132">Consultez le [documentation de Kestrel](xref:fundamentals/servers/kestrel) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="12dd8-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="12dd8-133">Cliquez sur **suivant** jusqu'à la fin de l’Assistant et **fermer** à la fin.</span><span class="sxs-lookup"><span data-stu-id="12dd8-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="12dd8-134">Identité ASP.NET Core nécessite un **ID de nom** de revendication.</span><span class="sxs-lookup"><span data-stu-id="12dd8-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="12dd8-135">Ajoutez-en une à partir de la **modifier les règles de revendication** boîte de dialogue :</span><span class="sxs-lookup"><span data-stu-id="12dd8-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Modifier les règles de revendication](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="12dd8-137">Dans le **ajouter un Assistant de règle de revendication transformer**, laissez la valeur par défaut **envoyer les attributs LDAP en tant que revendications** modèle sélectionné, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="12dd8-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="12dd8-138">Ajouter une règle de mappage du **nom de compte SAM** attribut LDAP à le **ID de nom** revendication sortante :</span><span class="sxs-lookup"><span data-stu-id="12dd8-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Ajouter un Assistant de règle de revendication de transformation : Configurer la règle de revendication](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="12dd8-140">Cliquez sur **Terminer** > **OK** dans les **modifier les règles de revendication** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="12dd8-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="12dd8-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="12dd8-141">Azure Active Directory</span></span>

* <span data-ttu-id="12dd8-142">Accédez au panneau des inscriptions des applications du locataire AAD.</span><span class="sxs-lookup"><span data-stu-id="12dd8-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="12dd8-143">Cliquez sur **nouvelle inscription de l’application**:</span><span class="sxs-lookup"><span data-stu-id="12dd8-143">Click **New application registration**:</span></span>

![Azure Active Directory : Inscriptions d’application](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="12dd8-145">Entrez un nom pour l’inscription de l’application.</span><span class="sxs-lookup"><span data-stu-id="12dd8-145">Enter a name for the app registration.</span></span> <span data-ttu-id="12dd8-146">Cela n’est pas important de l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12dd8-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="12dd8-147">Entrez l’URL de l’application écoute sur que le **URL de connexion**:</span><span class="sxs-lookup"><span data-stu-id="12dd8-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory : Créer l’inscription d’une application](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="12dd8-149">Cliquez sur **points de terminaison** et notez la **Document de métadonnées de fédération** URL.</span><span class="sxs-lookup"><span data-stu-id="12dd8-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="12dd8-150">Il s’agit de l’intergiciel de WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="12dd8-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory : points de terminaison](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="12dd8-152">Accédez à la nouvelle inscription de l’application.</span><span class="sxs-lookup"><span data-stu-id="12dd8-152">Navigate to the new app registration.</span></span> <span data-ttu-id="12dd8-153">Cliquez sur **paramètres** > **propriétés** et notez la **URI ID d’application**.</span><span class="sxs-lookup"><span data-stu-id="12dd8-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="12dd8-154">Il s’agit de l’intergiciel de WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="12dd8-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory : Propriétés d’inscription de l’application](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="12dd8-156">Ajouter WS-Federation en tant qu’un fournisseur de connexion externe pour ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="12dd8-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="12dd8-157">Ajouter une dépendance sur [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) au projet.</span><span class="sxs-lookup"><span data-stu-id="12dd8-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="12dd8-158">Ajouter WS-Federation pour le `Configure` méthode dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="12dd8-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="12dd8-159">Se connecter avec WS-Federation</span><span class="sxs-lookup"><span data-stu-id="12dd8-159">Log in with WS-Federation</span></span>

<span data-ttu-id="12dd8-160">Accédez à l’application et cliquez sur le **connecter** lien dans l’en-tête de navigation.</span><span class="sxs-lookup"><span data-stu-id="12dd8-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="12dd8-161">Il existe une option pour vous connecter avec WsFederation : ![page de connexion](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="12dd8-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="12dd8-162">Avec AD FS en tant que le fournisseur, le bouton redirige vers une page de connexion AD FS : ![page de connexion AD FS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="12dd8-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="12dd8-163">Avec Azure Active Directory en tant que le fournisseur, le bouton redirige vers une page de connexion à AAD : ![AAD-page de connexion](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="12dd8-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="12dd8-164">Une réussite sign-in pour un nouvel utilisateur redirige vers la page d’inscription de l’application utilisateur : ![page d’inscription](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="12dd8-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="12dd8-165">Utiliser WS-Federation sans ASP.NET Core identité</span><span class="sxs-lookup"><span data-stu-id="12dd8-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="12dd8-166">L’intergiciel (middleware) WS-Federation peut être utilisé sans identité.</span><span class="sxs-lookup"><span data-stu-id="12dd8-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="12dd8-167">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="12dd8-167">For example:</span></span>

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
