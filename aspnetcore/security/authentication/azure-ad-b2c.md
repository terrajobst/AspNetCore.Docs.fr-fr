---
title: Authentification cloud avec Azure Active Directory B2C dans ASP.NET Core
author: camsoper
description: Découvrez comment configurer l’authentification Azure Active Directory B2C avec ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 02/27/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 54117bf0dd45305d060eef5fecfb98ed45f8ecdb
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815304"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="cde8c-103">Authentification cloud avec Azure Active Directory B2C dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cde8c-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="cde8c-104">Auteur : [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="cde8c-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="cde8c-105">[Azure B2C Active de répertoire](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) est une solution de gestion des identités de cloud pour les applications web et mobiles.</span><span class="sxs-lookup"><span data-stu-id="cde8c-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="cde8c-106">Le service fournit une authentification pour les applications hébergées dans le cloud et sur site.</span><span class="sxs-lookup"><span data-stu-id="cde8c-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="cde8c-107">Types d’authentification, les comptes individuels, les comptes de réseau social et fédérés comptes d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="cde8c-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="cde8c-108">En outre, Azure AD B2C peut fournir une authentification multifacteur avec une configuration minimale.</span><span class="sxs-lookup"><span data-stu-id="cde8c-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="cde8c-109">Azure Active Directory (Azure AD) et Azure AD B2C sont des offres de produits distinctes.</span><span class="sxs-lookup"><span data-stu-id="cde8c-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="cde8c-110">Un locataire Azure AD représente une organisation, alors qu’un locataire Azure AD B2C représente une collection d’identités qui doivent être utilisées avec les applications de confiance.</span><span class="sxs-lookup"><span data-stu-id="cde8c-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="cde8c-111">Pour plus d’informations, consultez [Azure AD B2C : Forum aux questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span><span class="sxs-lookup"><span data-stu-id="cde8c-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="cde8c-112">Dans ce didacticiel, découvrez comment :</span><span class="sxs-lookup"><span data-stu-id="cde8c-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cde8c-113">Créer un locataire Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="cde8c-114">Inscrire une application dans Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="cde8c-115">Utiliser Visual Studio pour créer une application web de ASP.NET Core configurée pour utiliser le locataire Azure AD B2C pour l’authentification</span><span class="sxs-lookup"><span data-stu-id="cde8c-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="cde8c-116">Configurer des stratégies de contrôle du comportement du locataire Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cde8c-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="cde8c-117">Prerequisites</span></span>

<span data-ttu-id="cde8c-118">Les éléments suivants sont requis pour cette procédure pas à pas :</span><span class="sxs-lookup"><span data-stu-id="cde8c-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="cde8c-119">Abonnement Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="cde8c-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [<span data-ttu-id="cde8c-120">Visual Studio 2019</span><span class="sxs-lookup"><span data-stu-id="cde8c-120">Visual Studio 2019</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="cde8c-121">Créer le client Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="cde8c-122">Créer un locataire Azure Active Directory B2C [comme décrit dans la documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span><span class="sxs-lookup"><span data-stu-id="cde8c-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="cde8c-123">Lorsque vous y êtes invité, associer le locataire à un abonnement Azure est facultative pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="cde8c-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="cde8c-124">Inscrire l’application dans Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="cde8c-125">Dans le locataire Azure AD B2C nouvellement créé, inscrivez votre application en utilisant [les étapes décrites dans la documentation](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) sous le **inscrire une application web** section.</span><span class="sxs-lookup"><span data-stu-id="cde8c-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) under the **Register a web app** section.</span></span> <span data-ttu-id="cde8c-126">Arrêter à la **créer un secret de client d’application web** section.</span><span class="sxs-lookup"><span data-stu-id="cde8c-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="cde8c-127">Une clé secrète client n’est pas nécessaire pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="cde8c-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="cde8c-128">Utilisez les valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="cde8c-128">Use the following values:</span></span>

| <span data-ttu-id="cde8c-129">Paramètre</span><span class="sxs-lookup"><span data-stu-id="cde8c-129">Setting</span></span>                       | <span data-ttu-id="cde8c-130">Value</span><span class="sxs-lookup"><span data-stu-id="cde8c-130">Value</span></span>                     | <span data-ttu-id="cde8c-131">Notes</span><span class="sxs-lookup"><span data-stu-id="cde8c-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cde8c-132">**Name**</span><span class="sxs-lookup"><span data-stu-id="cde8c-132">**Name**</span></span>                      | <span data-ttu-id="cde8c-133">*&lt;Nom de l’application&gt;*</span><span class="sxs-lookup"><span data-stu-id="cde8c-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="cde8c-134">Entrez un **nom** pour l’application qui décrit votre application aux consommateurs.</span><span class="sxs-lookup"><span data-stu-id="cde8c-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="cde8c-135">**Inclure l’application web / API web**</span><span class="sxs-lookup"><span data-stu-id="cde8c-135">**Include web app / web API**</span></span> | <span data-ttu-id="cde8c-136">Oui</span><span class="sxs-lookup"><span data-stu-id="cde8c-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="cde8c-137">**Autoriser un flux implicite**</span><span class="sxs-lookup"><span data-stu-id="cde8c-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="cde8c-138">Oui</span><span class="sxs-lookup"><span data-stu-id="cde8c-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="cde8c-139">**URL de réponse**</span><span class="sxs-lookup"><span data-stu-id="cde8c-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="cde8c-140">URL de réponse sont des points de terminaison auxquels Azure AD B2C retourne les jetons demandés par votre application.</span><span class="sxs-lookup"><span data-stu-id="cde8c-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="cde8c-141">Visual Studio fournit l’URL de réponse à utiliser.</span><span class="sxs-lookup"><span data-stu-id="cde8c-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="cde8c-142">Pour l’instant, entrez `https://localhost:44300/signin-oidc` pour remplir le formulaire.</span><span class="sxs-lookup"><span data-stu-id="cde8c-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="cde8c-143">**URI ID d’application**</span><span class="sxs-lookup"><span data-stu-id="cde8c-143">**App ID URI**</span></span>                | <span data-ttu-id="cde8c-144">Laisser vide</span><span class="sxs-lookup"><span data-stu-id="cde8c-144">Leave blank</span></span>               | <span data-ttu-id="cde8c-145">Non requis pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="cde8c-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="cde8c-146">**Inclure le client natif**</span><span class="sxs-lookup"><span data-stu-id="cde8c-146">**Include native client**</span></span>     | <span data-ttu-id="cde8c-147">Non</span><span class="sxs-lookup"><span data-stu-id="cde8c-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="cde8c-148">Si la configuration d’une URL de réponse non localhost, vous devez connaître le [contraintes sur ce qui est autorisé dans la liste des URL de réponse](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application).</span><span class="sxs-lookup"><span data-stu-id="cde8c-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application).</span></span> 

<span data-ttu-id="cde8c-149">Une fois que l’application est inscrite, la liste des applications dans le client s’affiche.</span><span class="sxs-lookup"><span data-stu-id="cde8c-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="cde8c-150">Sélectionnez l’application que vous venez d’inscrire.</span><span class="sxs-lookup"><span data-stu-id="cde8c-150">Select the app that was just registered.</span></span> <span data-ttu-id="cde8c-151">Sélectionnez le **copie** icône à droite de la **ID d’Application** champ pour le copier dans le Presse-papiers.</span><span class="sxs-lookup"><span data-stu-id="cde8c-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="cde8c-152">Rien n’est plus peuvent être configuré dans le locataire Azure AD B2C pour l’instant, mais laissant la fenêtre du navigateur ouverte.</span><span class="sxs-lookup"><span data-stu-id="cde8c-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="cde8c-153">Il existe davantage de configuration après la création de l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cde8c-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio"></a><span data-ttu-id="cde8c-154">Créer une application ASP.NET Core dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cde8c-154">Create an ASP.NET Core app in Visual Studio</span></span>

<span data-ttu-id="cde8c-155">Le modèle d’Application Web de Visual Studio peut être configuré pour utiliser le locataire Azure AD B2C pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="cde8c-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="cde8c-156">Dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="cde8c-156">In Visual Studio:</span></span>

1. <span data-ttu-id="cde8c-157">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cde8c-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="cde8c-158">Sélectionnez **Web Application** à partir de la liste des modèles.</span><span class="sxs-lookup"><span data-stu-id="cde8c-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="cde8c-159">Sélectionnez le **modifier l’authentification** bouton.</span><span class="sxs-lookup"><span data-stu-id="cde8c-159">Select the **Change Authentication** button.</span></span>
    
    ![Bouton de l’authentification de modification](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="cde8c-161">Dans le **modifier l’authentification** boîte de dialogue, sélectionnez **comptes d’utilisateur individuels**, puis sélectionnez **se connecter à un magasin d’utilisateurs existant dans le cloud** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="cde8c-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![Boîte de dialogue Modifier l’authentification](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="cde8c-163">Remplissez le formulaire avec les valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="cde8c-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="cde8c-164">Paramètre</span><span class="sxs-lookup"><span data-stu-id="cde8c-164">Setting</span></span>                       | <span data-ttu-id="cde8c-165">Value</span><span class="sxs-lookup"><span data-stu-id="cde8c-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="cde8c-166">**Nom de domaine**</span><span class="sxs-lookup"><span data-stu-id="cde8c-166">**Domain Name**</span></span>               | <span data-ttu-id="cde8c-167">*&lt;le nom de domaine de votre client B2C&gt;*</span><span class="sxs-lookup"><span data-stu-id="cde8c-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="cde8c-168">**ID d’application**</span><span class="sxs-lookup"><span data-stu-id="cde8c-168">**Application ID**</span></span>            | <span data-ttu-id="cde8c-169">*&lt;Collez l’ID d’Application à partir du Presse-papiers&gt;*</span><span class="sxs-lookup"><span data-stu-id="cde8c-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="cde8c-170">**Chemin d’accès de rappel**</span><span class="sxs-lookup"><span data-stu-id="cde8c-170">**Callback Path**</span></span>             | <span data-ttu-id="cde8c-171">*&lt;Utilisez la valeur par défaut&gt;*</span><span class="sxs-lookup"><span data-stu-id="cde8c-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="cde8c-172">**Stratégie d’inscription ou connexion**</span><span class="sxs-lookup"><span data-stu-id="cde8c-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="cde8c-173">**Stratégie de réinitialisation de mot de passe**</span><span class="sxs-lookup"><span data-stu-id="cde8c-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="cde8c-174">**Modifier la stratégie de profil**</span><span class="sxs-lookup"><span data-stu-id="cde8c-174">**Edit profile policy**</span></span>       | <span data-ttu-id="cde8c-175">*&lt;Laisser vide&gt;*</span><span class="sxs-lookup"><span data-stu-id="cde8c-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="cde8c-176">Sélectionnez le **copie** situé en regard **URI de réponse** pour copier l’URI de réponse dans le Presse-papiers.</span><span class="sxs-lookup"><span data-stu-id="cde8c-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="cde8c-177">Sélectionnez **OK** pour fermer la **modifier l’authentification** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="cde8c-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="cde8c-178">Sélectionnez **OK** pour créer l’application web.</span><span class="sxs-lookup"><span data-stu-id="cde8c-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="cde8c-179">Terminer l’inscription des applications B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-179">Finish the B2C app registration</span></span>

<span data-ttu-id="cde8c-180">Revenez à la fenêtre de navigateur avec les propriétés de l’application B2C toujours ouvertes.</span><span class="sxs-lookup"><span data-stu-id="cde8c-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="cde8c-181">Modifier la fichier temporaire **URL de réponse** spécifié précédemment pour la valeur copiée à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cde8c-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="cde8c-182">Sélectionnez **enregistrer** en haut de la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="cde8c-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="cde8c-183">Si vous n’avez pas de copier l’URL de réponse, utilisez l’adresse HTTPS à partir de l’onglet débogage dans les propriétés du projet web et ajouter la **CallbackPath** valeur *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="cde8c-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="cde8c-184">Configurer des stratégies</span><span class="sxs-lookup"><span data-stu-id="cde8c-184">Configure policies</span></span>

<span data-ttu-id="cde8c-185">Utilisez les étapes de la documentation d’Azure AD B2C pour [créer une stratégie d’inscription ou connexion](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), puis [créer une stratégie de réinitialisation de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions).</span><span class="sxs-lookup"><span data-stu-id="cde8c-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions).</span></span> <span data-ttu-id="cde8c-186">Utilisez les exemples de valeurs fournies dans la documentation pour **fournisseurs d’identité**, **attributs d’inscription**, et **des revendications d’Application**.</span><span class="sxs-lookup"><span data-stu-id="cde8c-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="cde8c-187">À l’aide de la **exécuter maintenant** bouton pour tester les stratégies, comme décrit dans la documentation est facultatif.</span><span class="sxs-lookup"><span data-stu-id="cde8c-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="cde8c-188">Vérifiez les noms de stratégie sont exactement comme décrit dans la documentation, car ces stratégies ont été utilisées dans le **modifier l’authentification** boîte de dialogue dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cde8c-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="cde8c-189">Les noms de stratégie peuvent être vérifiés dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="cde8c-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a><span data-ttu-id="cde8c-190">Configurez les options de OpenIdConnectOptions/JwtBearer/Cookie sous-jacent</span><span class="sxs-lookup"><span data-stu-id="cde8c-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="cde8c-191">Pour configurer les options sous-jacente directement, utilisez la constante de schéma approprié dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cde8c-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a><span data-ttu-id="cde8c-192">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="cde8c-192">Run the app</span></span>

<span data-ttu-id="cde8c-193">Dans Visual Studio, appuyez sur **F5** pour générer et exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="cde8c-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="cde8c-194">Une fois l’application web démarre, sélectionnez **Accept** à accepter l’utilisation de cookies (si vous y êtes invité), puis sélectionnez **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="cde8c-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![Connectez-vous à l’application](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="cde8c-196">Le navigateur le redirige vers le locataire Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="cde8c-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="cde8c-197">Connectez-vous avec un compte existant (si un a été créé les stratégies de test) ou sélectionnez **s’inscrire maintenant** pour créer un nouveau compte.</span><span class="sxs-lookup"><span data-stu-id="cde8c-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="cde8c-198">Le **votre mot de passe oublié ?** lien est utilisé pour réinitialiser un mot de passe oublié.</span><span class="sxs-lookup"><span data-stu-id="cde8c-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Connexion à Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="cde8c-200">Après vous être connecté avec succès, le navigateur le redirige vers l’application web.</span><span class="sxs-lookup"><span data-stu-id="cde8c-200">After successfully signing in, the browser redirects to the web app.</span></span>

![Succès](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="cde8c-202">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="cde8c-202">Next steps</span></span>

<span data-ttu-id="cde8c-203">Dans ce tutoriel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="cde8c-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cde8c-204">Créer un locataire Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="cde8c-205">Inscrire une application dans Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="cde8c-206">Utiliser Visual Studio pour créer une Application ASP.NET Core Web, configuré pour utiliser le locataire Azure AD B2C pour l’authentification</span><span class="sxs-lookup"><span data-stu-id="cde8c-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="cde8c-207">Configurer des stratégies de contrôle du comportement du locataire Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="cde8c-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="cde8c-208">Maintenant que l’application ASP.NET Core est configurée pour utiliser Azure AD B2C pour l’authentification, le [attribut Authorize](xref:security/authorization/simple) peut être utilisé pour sécuriser votre application.</span><span class="sxs-lookup"><span data-stu-id="cde8c-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="cde8c-209">Continuer à développer votre application en apprenant à :</span><span class="sxs-lookup"><span data-stu-id="cde8c-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="cde8c-210">[Personnaliser l’interface utilisateur de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="cde8c-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="cde8c-211">[Configurer les exigences de complexité de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span><span class="sxs-lookup"><span data-stu-id="cde8c-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="cde8c-212">[Activer l’authentification multifacteur](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span><span class="sxs-lookup"><span data-stu-id="cde8c-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="cde8c-213">Configurer les fournisseurs d’identité supplémentaires, telles que [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)et d’autres.</span><span class="sxs-lookup"><span data-stu-id="cde8c-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="cde8c-214">[Utiliser l’API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) pour récupérer des informations supplémentaires, telles que l’appartenance au groupe, à partir du locataire Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="cde8c-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="cde8c-215">[Sécuriser un ASP.NET Core API web à l’aide d’Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span><span class="sxs-lookup"><span data-stu-id="cde8c-215">[Secure an ASP.NET Core web API using Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span></span>
* <span data-ttu-id="cde8c-216">[Appeler une API web .NET à partir d’une application web de .NET à l’aide d’Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span><span class="sxs-lookup"><span data-stu-id="cde8c-216">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
