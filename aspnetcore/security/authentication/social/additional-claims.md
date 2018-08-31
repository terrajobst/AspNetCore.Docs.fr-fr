---
title: Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core
author: guardrex
description: Découvrez comment établir des revendications supplémentaires et des jetons provenant de fournisseurs externes.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6386dd5f0bd901be01cf56081a6b9ca7f408f9f9
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336175"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="83a66-103">Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83a66-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="83a66-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="83a66-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="83a66-105">Une application ASP.NET Core peut établir des revendications supplémentaires et des jetons provenant de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="83a66-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="83a66-106">Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données de l’utilisateur vers des revendications supplémentaires est le même.</span><span class="sxs-lookup"><span data-stu-id="83a66-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="83a66-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="83a66-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisite"></a><span data-ttu-id="83a66-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="83a66-108">Prerequisite</span></span>

<span data-ttu-id="83a66-109">Décidez quels fournisseurs d’authentification externes pour prendre en charge dans l’application.</span><span class="sxs-lookup"><span data-stu-id="83a66-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="83a66-110">Pour chaque fournisseur, inscrire l’application et obtenir un ID client et la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="83a66-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="83a66-111">Pour plus d'informations, consultez <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="83a66-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="83a66-112">Le [exemple d’application](#sample-app-instructions) utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="83a66-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="authentication-provider-configuration"></a><span data-ttu-id="83a66-113">Configuration du fournisseur d’authentification</span><span class="sxs-lookup"><span data-stu-id="83a66-113">Authentication provider configuration</span></span>

### <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="83a66-114">Définir l’ID client et la clé secrète client</span><span class="sxs-lookup"><span data-stu-id="83a66-114">Set the client ID and client secret</span></span>

<span data-ttu-id="83a66-115">Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="83a66-115">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="83a66-116">ID client et les valeurs de clé secrète du client sont créés pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="83a66-116">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="83a66-117">Chaque fournisseur externe qui utilise l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="83a66-117">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="83a66-118">Pour plus d’informations, consultez les rubriques de fournisseur d’authentification externe qui s’appliquent à votre scénario :</span><span class="sxs-lookup"><span data-stu-id="83a66-118">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="83a66-119">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="83a66-119">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="83a66-120">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="83a66-120">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="83a66-121">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="83a66-121">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="83a66-122">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="83a66-122">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="83a66-123">Autres fournisseurs d’authentification</span><span class="sxs-lookup"><span data-stu-id="83a66-123">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="83a66-124">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="83a66-124">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="83a66-125">L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et la clé secrète client fournie par Google :</span><span class="sxs-lookup"><span data-stu-id="83a66-125">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a><span data-ttu-id="83a66-126">Établir l’étendue de l’authentification</span><span class="sxs-lookup"><span data-stu-id="83a66-126">Establish the authentication scope</span></span>

<span data-ttu-id="83a66-127">Spécifier la liste des autorisations nécessaires pour récupérer à partir du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="83a66-127">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="83a66-128">Étendues de l’authentification pour les fournisseurs externes courantes apparaissent dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="83a66-128">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="83a66-129">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="83a66-129">Provider</span></span>  | <span data-ttu-id="83a66-130">Portée</span><span class="sxs-lookup"><span data-stu-id="83a66-130">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="83a66-131">Facebook</span><span class="sxs-lookup"><span data-stu-id="83a66-131">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="83a66-132">Google</span><span class="sxs-lookup"><span data-stu-id="83a66-132">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="83a66-133">Microsoft</span><span class="sxs-lookup"><span data-stu-id="83a66-133">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="83a66-134">Twitter</span><span class="sxs-lookup"><span data-stu-id="83a66-134">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="83a66-135">L’exemple d’application ajoute Google `plus.login` étendue pour demander des autorisations de connexion Google + :</span><span class="sxs-lookup"><span data-stu-id="83a66-135">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="83a66-136">Mapper des clés de données utilisateur et de créer des revendications</span><span class="sxs-lookup"><span data-stu-id="83a66-136">Map user data keys and create claims</span></span>

<span data-ttu-id="83a66-137">Dans les options du fournisseur, vous devez spécifier un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> pour chaque clé dans les données d’utilisateur JSON du fournisseur externe pour l’identité de l’application à lire sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="83a66-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="83a66-138">Pour plus d’informations sur les types de revendications, consultez <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="83a66-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="83a66-139">L’exemple d’application crée un <xref:System.Security.Claims.ClaimTypes.Gender> revendication à partir de la `gender` clés dans les données utilisateur de Google :</span><span class="sxs-lookup"><span data-stu-id="83a66-139">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="83a66-140">Dans <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="83a66-140">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="83a66-141">Lors de la connexion des processus, le <xref:Microsoft.AspNetCore.Identity.UserManager`1> peut stocker un `ApplicationUser` de revendication pour les données utilisateur disponibles à partir de la <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="83a66-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="83a66-142">Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) établit une <xref:System.Security.Claims.ClaimTypes.Gender> de revendication pour l’élément signé dans `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="83a66-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a><span data-ttu-id="83a66-143">Enregistrer le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="83a66-143">Save the access token</span></span>

<span data-ttu-id="83a66-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockées dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie.</span><span class="sxs-lookup"><span data-stu-id="83a66-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="83a66-145">`SaveTokens` a la valeur `false` par défaut pour réduire la taille du cookie d’authentification finale.</span><span class="sxs-lookup"><span data-stu-id="83a66-145">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="83a66-146">L’exemple d’application définit la valeur de `SaveTokens` à `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="83a66-146">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="83a66-147">Lorsque `OnPostConfirmationAsync` s’exécute, stocker le jeton d’accès ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `ApplicationUser`de `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="83a66-147">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="83a66-148">L’exemple d’application enregistre dans le jeton d’accès :</span><span class="sxs-lookup"><span data-stu-id="83a66-148">The sample app saves the access token in:</span></span>

* <span data-ttu-id="83a66-149">`OnPostConfirmationAsync` &ndash; S’exécute pour la nouvelle inscription de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="83a66-149">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="83a66-150">`OnGetCallbackAsync` &ndash; S’exécute lorsqu’un utilisateur inscrit précédemment se connecte à l’application.</span><span class="sxs-lookup"><span data-stu-id="83a66-150">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="83a66-151">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="83a66-151">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="83a66-152">Comment ajouter des jetons personnalisés supplémentaires</span><span class="sxs-lookup"><span data-stu-id="83a66-152">How to add additional custom tokens</span></span>

<span data-ttu-id="83a66-153">Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec actuel <xref:System.DateTime> pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="83a66-153">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="83a66-154">Instructions de l’application exemple</span><span class="sxs-lookup"><span data-stu-id="83a66-154">Sample app instructions</span></span>

<span data-ttu-id="83a66-155">L’exemple d’application montre comment :</span><span class="sxs-lookup"><span data-stu-id="83a66-155">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="83a66-156">Obtenir le sexe de l’utilisateur à partir de Google et enregistre une revendication de sexe avec la valeur.</span><span class="sxs-lookup"><span data-stu-id="83a66-156">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="83a66-157">Store le jeton d’accès Google de l’utilisateur `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="83a66-157">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="83a66-158">Pour utiliser l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="83a66-158">To use the sample app:</span></span>

1. <span data-ttu-id="83a66-159">Inscrire l’application et obtenir un ID client valide et une clé secrète client pour l’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="83a66-159">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="83a66-160">Pour plus d'informations, consultez <xref:security/authentication/google-logins>.</span><span class="sxs-lookup"><span data-stu-id="83a66-160">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="83a66-161">Fournir l’ID client et le secret du client à l’application dans le <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="83a66-161">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="83a66-162">Exécutez l’application et demandez la page Mes revendications.</span><span class="sxs-lookup"><span data-stu-id="83a66-162">Run the app and request the My Claims page.</span></span> <span data-ttu-id="83a66-163">Lorsque l’utilisateur n’est pas connecté, l’application redirige vers Google.</span><span class="sxs-lookup"><span data-stu-id="83a66-163">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="83a66-164">Se connecter avec Google.</span><span class="sxs-lookup"><span data-stu-id="83a66-164">Sign in with Google.</span></span> <span data-ttu-id="83a66-165">Google redirige l’utilisateur vers l’application (`/Home/MyClaims`).</span><span class="sxs-lookup"><span data-stu-id="83a66-165">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="83a66-166">L’utilisateur est authentifié, et la page Mes revendications est chargée.</span><span class="sxs-lookup"><span data-stu-id="83a66-166">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="83a66-167">La revendication de sexe est présente sous **revendications utilisateur** avec la valeur obtenue à partir de Google.</span><span class="sxs-lookup"><span data-stu-id="83a66-167">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="83a66-168">Le jeton d’accès s’affiche dans le **propriétés d’authentification**.</span><span class="sxs-lookup"><span data-stu-id="83a66-168">The access token appears in the **Authentication Properties**.</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
