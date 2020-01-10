---
title: Conserver des revendications et des jetons supplémentaires à partir de fournisseurs externes dans ASP.NET Core
author: guardrex
description: Découvrez comment créer des revendications et des jetons supplémentaires à partir de fournisseurs externes.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 44b3e72085e6265319b53b548f7f7ddde2adbd14
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828579"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="4aace-103">Conserver des revendications et des jetons supplémentaires à partir de fournisseurs externes dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4aace-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="4aace-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4aace-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4aace-105">Une application ASP.NET Core peut établir des revendications et des jetons supplémentaires à partir de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="4aace-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="4aace-106">Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données utilisateur en revendications supplémentaires est le même.</span><span class="sxs-lookup"><span data-stu-id="4aace-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="4aace-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4aace-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4aace-108">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="4aace-108">Prerequisites</span></span>

<span data-ttu-id="4aace-109">Choisissez les fournisseurs d’authentification externes à prendre en charge dans l’application.</span><span class="sxs-lookup"><span data-stu-id="4aace-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="4aace-110">Pour chaque fournisseur, inscrivez l’application et obtenez un ID client et une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="4aace-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="4aace-111">Pour plus d'informations, consultez <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="4aace-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="4aace-112">L’exemple d’application utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="4aace-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="4aace-113">Définir l’ID client et la clé secrète client</span><span class="sxs-lookup"><span data-stu-id="4aace-113">Set the client ID and client secret</span></span>

<span data-ttu-id="4aace-114">Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et d’une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="4aace-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="4aace-115">Les valeurs ID client et clé secrète client sont créées pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="4aace-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="4aace-116">Chaque fournisseur externe utilisé par l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="4aace-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="4aace-117">Pour plus d’informations, consultez les rubriques fournisseur d’authentification externe qui s’appliquent à votre scénario :</span><span class="sxs-lookup"><span data-stu-id="4aace-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="4aace-118">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="4aace-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="4aace-119">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="4aace-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="4aace-120">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="4aace-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="4aace-121">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="4aace-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="4aace-122">Autres fournisseurs d’authentification</span><span class="sxs-lookup"><span data-stu-id="4aace-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="4aace-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="4aace-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="4aace-124">L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et une clé secrète client fournis par Google :</span><span class="sxs-lookup"><span data-stu-id="4aace-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="4aace-125">Établir l’étendue de l’authentification</span><span class="sxs-lookup"><span data-stu-id="4aace-125">Establish the authentication scope</span></span>

<span data-ttu-id="4aace-126">Spécifiez la liste des autorisations à récupérer du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="4aace-127">Les étendues d’authentification pour les fournisseurs externes communs apparaissent dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="4aace-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="4aace-128">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="4aace-128">Provider</span></span>  | <span data-ttu-id="4aace-129">Portée</span><span class="sxs-lookup"><span data-stu-id="4aace-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="4aace-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="4aace-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="4aace-131">Google</span><span class="sxs-lookup"><span data-stu-id="4aace-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="4aace-132">de Microsoft</span><span class="sxs-lookup"><span data-stu-id="4aace-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="4aace-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="4aace-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="4aace-134">Dans l’exemple d’application, la portée `userinfo.profile` de Google est automatiquement ajoutée par l’infrastructure quand <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> est appelé sur le <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4aace-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="4aace-135">Si l’application requiert des étendues supplémentaires, ajoutez-les aux options.</span><span class="sxs-lookup"><span data-stu-id="4aace-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="4aace-136">Dans l’exemple suivant, l’étendue Google `https://www.googleapis.com/auth/user.birthday.read` est ajoutée afin de récupérer l’anniversaire d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="4aace-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="4aace-137">Mapper des clés de données utilisateur et créer des revendications</span><span class="sxs-lookup"><span data-stu-id="4aace-137">Map user data keys and create claims</span></span>

<span data-ttu-id="4aace-138">Dans les options du fournisseur, spécifiez un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> ou <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> pour chaque clé/sous-clé dans les données utilisateur JSON du fournisseur externe pour que l’identité de l’application soit lue lors de la connexion.</span><span class="sxs-lookup"><span data-stu-id="4aace-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="4aace-139">Pour plus d’informations sur les types de revendication, consultez <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="4aace-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="4aace-140">L’exemple d’application crée des revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) à partir des clés `locale` et `picture` dans Google User Data :</span><span class="sxs-lookup"><span data-stu-id="4aace-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="4aace-141">Dans `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-141">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="4aace-142">Pendant le processus de connexion, les <xref:Microsoft.AspNetCore.Identity.UserManager%601> peuvent stocker des revendications `ApplicationUser` pour les données utilisateur disponibles à partir du <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="4aace-143">Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*) établit les revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) pour le `ApplicationUser`connecté, y compris une revendication pour <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="4aace-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="4aace-144">Par défaut, les revendications d’un utilisateur sont stockées dans le cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="4aace-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="4aace-145">Si le cookie d’authentification est trop volumineux, l’application risque d’échouer en raison des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4aace-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="4aace-146">Le navigateur détecte que l’en-tête du cookie est trop long.</span><span class="sxs-lookup"><span data-stu-id="4aace-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="4aace-147">La taille globale de la demande est trop importante.</span><span class="sxs-lookup"><span data-stu-id="4aace-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="4aace-148">Si une grande quantité de données utilisateur est requise pour le traitement des demandes de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="4aace-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="4aace-149">Limitez le nombre et la taille des revendications utilisateur pour le traitement des demandes uniquement pour ce que l’application requiert.</span><span class="sxs-lookup"><span data-stu-id="4aace-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="4aace-150">Utilisez un <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> personnalisé pour l' <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> de l’intergiciel d’authentification des cookies pour stocker l’identité entre les demandes.</span><span class="sxs-lookup"><span data-stu-id="4aace-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="4aace-151">Conservez de grandes quantités d’informations d’identité sur le serveur tout en envoyant une petite clé d’identificateur de session au client uniquement.</span><span class="sxs-lookup"><span data-stu-id="4aace-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="4aace-152">Enregistrer le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="4aace-152">Save the access token</span></span>

<span data-ttu-id="4aace-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockés dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie.</span><span class="sxs-lookup"><span data-stu-id="4aace-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="4aace-154">`SaveTokens` est défini sur `false` par défaut pour réduire la taille du cookie d’authentification final.</span><span class="sxs-lookup"><span data-stu-id="4aace-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="4aace-155">L’exemple d’application définit la valeur de `SaveTokens` sur `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="4aace-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="4aace-156">Lorsque `OnPostConfirmationAsync` s’exécute, stockez le jeton d’accès ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `AuthenticationProperties`du `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="4aace-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="4aace-157">L’exemple d’application enregistre le jeton d’accès dans `OnPostConfirmationAsync` (nouvelle inscription d’utilisateur) et `OnGetCallbackAsync` (utilisateur précédemment inscrit) dans *Account/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="4aace-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="4aace-158">Comment ajouter des jetons personnalisés supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4aace-158">How to add additional custom tokens</span></span>

<span data-ttu-id="4aace-159">Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute une <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec la <xref:System.DateTime> actuelle pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="4aace-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="4aace-160">Création et ajout de revendications</span><span class="sxs-lookup"><span data-stu-id="4aace-160">Creating and adding claims</span></span>

<span data-ttu-id="4aace-161">L’infrastructure fournit des actions courantes et des méthodes d’extension pour créer et ajouter des revendications à la collection.</span><span class="sxs-lookup"><span data-stu-id="4aace-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="4aace-162">Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> et <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="4aace-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="4aace-163">Les utilisateurs peuvent définir des actions personnalisées en dérivant de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> et en implémentant la méthode abstraite <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="4aace-164">Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="4aace-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="4aace-165">Suppression des revendications et des actions de revendication</span><span class="sxs-lookup"><span data-stu-id="4aace-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="4aace-166">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) supprime toutes les actions de revendication pour le <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné de la collection.</span><span class="sxs-lookup"><span data-stu-id="4aace-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="4aace-167">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) supprime une revendication du <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné à partir de l’identité.</span><span class="sxs-lookup"><span data-stu-id="4aace-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="4aace-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> est principalement utilisé avec [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) pour supprimer les revendications générées par le protocole.</span><span class="sxs-lookup"><span data-stu-id="4aace-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="4aace-169">Exemple de sortie d’application</span><span class="sxs-lookup"><span data-stu-id="4aace-169">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4aace-170">Une application ASP.NET Core peut établir des revendications et des jetons supplémentaires à partir de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="4aace-170">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="4aace-171">Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données utilisateur en revendications supplémentaires est le même.</span><span class="sxs-lookup"><span data-stu-id="4aace-171">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="4aace-172">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4aace-172">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4aace-173">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="4aace-173">Prerequisites</span></span>

<span data-ttu-id="4aace-174">Choisissez les fournisseurs d’authentification externes à prendre en charge dans l’application.</span><span class="sxs-lookup"><span data-stu-id="4aace-174">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="4aace-175">Pour chaque fournisseur, inscrivez l’application et obtenez un ID client et une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="4aace-175">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="4aace-176">Pour plus d'informations, consultez <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="4aace-176">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="4aace-177">L’exemple d’application utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="4aace-177">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="4aace-178">Définir l’ID client et la clé secrète client</span><span class="sxs-lookup"><span data-stu-id="4aace-178">Set the client ID and client secret</span></span>

<span data-ttu-id="4aace-179">Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et d’une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="4aace-179">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="4aace-180">Les valeurs ID client et clé secrète client sont créées pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="4aace-180">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="4aace-181">Chaque fournisseur externe utilisé par l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="4aace-181">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="4aace-182">Pour plus d’informations, consultez les rubriques fournisseur d’authentification externe qui s’appliquent à votre scénario :</span><span class="sxs-lookup"><span data-stu-id="4aace-182">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="4aace-183">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="4aace-183">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="4aace-184">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="4aace-184">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="4aace-185">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="4aace-185">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="4aace-186">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="4aace-186">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="4aace-187">Autres fournisseurs d’authentification</span><span class="sxs-lookup"><span data-stu-id="4aace-187">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="4aace-188">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="4aace-188">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="4aace-189">L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et une clé secrète client fournis par Google :</span><span class="sxs-lookup"><span data-stu-id="4aace-189">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="4aace-190">Établir l’étendue de l’authentification</span><span class="sxs-lookup"><span data-stu-id="4aace-190">Establish the authentication scope</span></span>

<span data-ttu-id="4aace-191">Spécifiez la liste des autorisations à récupérer du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-191">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="4aace-192">Les étendues d’authentification pour les fournisseurs externes communs apparaissent dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="4aace-192">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="4aace-193">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="4aace-193">Provider</span></span>  | <span data-ttu-id="4aace-194">Portée</span><span class="sxs-lookup"><span data-stu-id="4aace-194">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="4aace-195">Facebook</span><span class="sxs-lookup"><span data-stu-id="4aace-195">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="4aace-196">Google</span><span class="sxs-lookup"><span data-stu-id="4aace-196">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="4aace-197">de Microsoft</span><span class="sxs-lookup"><span data-stu-id="4aace-197">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="4aace-198">Twitter</span><span class="sxs-lookup"><span data-stu-id="4aace-198">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="4aace-199">Dans l’exemple d’application, la portée `userinfo.profile` de Google est automatiquement ajoutée par l’infrastructure quand <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> est appelé sur le <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4aace-199">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="4aace-200">Si l’application requiert des étendues supplémentaires, ajoutez-les aux options.</span><span class="sxs-lookup"><span data-stu-id="4aace-200">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="4aace-201">Dans l’exemple suivant, l’étendue Google `https://www.googleapis.com/auth/user.birthday.read` est ajoutée afin de récupérer l’anniversaire d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="4aace-201">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="4aace-202">Mapper des clés de données utilisateur et créer des revendications</span><span class="sxs-lookup"><span data-stu-id="4aace-202">Map user data keys and create claims</span></span>

<span data-ttu-id="4aace-203">Dans les options du fournisseur, spécifiez un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> ou <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> pour chaque clé/sous-clé dans les données utilisateur JSON du fournisseur externe pour que l’identité de l’application soit lue lors de la connexion.</span><span class="sxs-lookup"><span data-stu-id="4aace-203">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="4aace-204">Pour plus d’informations sur les types de revendication, consultez <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="4aace-204">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="4aace-205">L’exemple d’application crée des revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) à partir des clés `locale` et `picture` dans Google User Data :</span><span class="sxs-lookup"><span data-stu-id="4aace-205">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="4aace-206">Dans `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-206">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="4aace-207">Pendant le processus de connexion, les <xref:Microsoft.AspNetCore.Identity.UserManager%601> peuvent stocker des revendications `ApplicationUser` pour les données utilisateur disponibles à partir du <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-207">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="4aace-208">Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*) établit les revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) pour le `ApplicationUser`connecté, y compris une revendication pour <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="4aace-208">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="4aace-209">Par défaut, les revendications d’un utilisateur sont stockées dans le cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="4aace-209">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="4aace-210">Si le cookie d’authentification est trop volumineux, l’application risque d’échouer en raison des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4aace-210">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="4aace-211">Le navigateur détecte que l’en-tête du cookie est trop long.</span><span class="sxs-lookup"><span data-stu-id="4aace-211">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="4aace-212">La taille globale de la demande est trop importante.</span><span class="sxs-lookup"><span data-stu-id="4aace-212">The overall size of the request is too large.</span></span>

<span data-ttu-id="4aace-213">Si une grande quantité de données utilisateur est requise pour le traitement des demandes de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="4aace-213">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="4aace-214">Limitez le nombre et la taille des revendications utilisateur pour le traitement des demandes uniquement pour ce que l’application requiert.</span><span class="sxs-lookup"><span data-stu-id="4aace-214">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="4aace-215">Utilisez un <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> personnalisé pour l' <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> de l’intergiciel d’authentification des cookies pour stocker l’identité entre les demandes.</span><span class="sxs-lookup"><span data-stu-id="4aace-215">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="4aace-216">Conservez de grandes quantités d’informations d’identité sur le serveur tout en envoyant une petite clé d’identificateur de session au client uniquement.</span><span class="sxs-lookup"><span data-stu-id="4aace-216">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="4aace-217">Enregistrer le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="4aace-217">Save the access token</span></span>

<span data-ttu-id="4aace-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockés dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie.</span><span class="sxs-lookup"><span data-stu-id="4aace-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="4aace-219">`SaveTokens` est défini sur `false` par défaut pour réduire la taille du cookie d’authentification final.</span><span class="sxs-lookup"><span data-stu-id="4aace-219">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="4aace-220">L’exemple d’application définit la valeur de `SaveTokens` sur `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="4aace-220">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="4aace-221">Lorsque `OnPostConfirmationAsync` s’exécute, stockez le jeton d’accès ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `AuthenticationProperties`du `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="4aace-221">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="4aace-222">L’exemple d’application enregistre le jeton d’accès dans `OnPostConfirmationAsync` (nouvelle inscription d’utilisateur) et `OnGetCallbackAsync` (utilisateur précédemment inscrit) dans *Account/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="4aace-222">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="4aace-223">Comment ajouter des jetons personnalisés supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4aace-223">How to add additional custom tokens</span></span>

<span data-ttu-id="4aace-224">Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute une <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec la <xref:System.DateTime> actuelle pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="4aace-224">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="4aace-225">Création et ajout de revendications</span><span class="sxs-lookup"><span data-stu-id="4aace-225">Creating and adding claims</span></span>

<span data-ttu-id="4aace-226">L’infrastructure fournit des actions courantes et des méthodes d’extension pour créer et ajouter des revendications à la collection.</span><span class="sxs-lookup"><span data-stu-id="4aace-226">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="4aace-227">Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> et <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="4aace-227">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="4aace-228">Les utilisateurs peuvent définir des actions personnalisées en dérivant de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> et en implémentant la méthode abstraite <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="4aace-228">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="4aace-229">Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="4aace-229">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="4aace-230">Suppression des revendications et des actions de revendication</span><span class="sxs-lookup"><span data-stu-id="4aace-230">Removal of claim actions and claims</span></span>

<span data-ttu-id="4aace-231">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) supprime toutes les actions de revendication pour le <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné de la collection.</span><span class="sxs-lookup"><span data-stu-id="4aace-231">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="4aace-232">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) supprime une revendication du <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné à partir de l’identité.</span><span class="sxs-lookup"><span data-stu-id="4aace-232">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="4aace-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> est principalement utilisé avec [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) pour supprimer les revendications générées par le protocole.</span><span class="sxs-lookup"><span data-stu-id="4aace-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="4aace-234">Exemple de sortie d’application</span><span class="sxs-lookup"><span data-stu-id="4aace-234">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4aace-235">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4aace-235">Additional resources</span></span>

* <span data-ttu-id="4aace-236">L' [application SocialSample d’ingénierie dotnet/AspNetCore](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; l’exemple d’application liée se trouve sur la branche ingénieurs [de dotnet/AspNetCore GitHub référentiel](https://github.com/dotnet/AspNetCore) `master`.</span><span class="sxs-lookup"><span data-stu-id="4aace-236">[dotnet/AspNetCore engineering SocialSample app](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [dotnet/AspNetCore GitHub repo's](https://github.com/dotnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="4aace-237">La branche `master` contient du code sous développement actif pour la prochaine version de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4aace-237">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="4aace-238">Pour afficher une version de l’exemple d’application pour une version finale de ASP.NET Core, utilisez la liste déroulante **branche** pour sélectionner une branche de version (par exemple `release/{X.Y}`).</span><span class="sxs-lookup"><span data-stu-id="4aace-238">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
