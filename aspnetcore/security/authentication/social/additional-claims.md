---
title: Conserver des revendications et des jetons supplémentaires à partir de fournisseurs externes dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer des revendications et des jetons supplémentaires à partir de fournisseurs externes.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 9dfe5745125e34ed813d078529471a0ba2a53ab0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666830"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="fb5ca-103">Conserver des revendications et des jetons supplémentaires à partir de fournisseurs externes dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb5ca-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fb5ca-104">Une application ASP.NET Core peut établir des revendications et des jetons supplémentaires à partir de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-104">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="fb5ca-105">Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données utilisateur en revendications supplémentaires est le même.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-105">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="fb5ca-106">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb5ca-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb5ca-107">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="fb5ca-107">Prerequisites</span></span>

<span data-ttu-id="fb5ca-108">Choisissez les fournisseurs d’authentification externes à prendre en charge dans l’application.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-108">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="fb5ca-109">Pour chaque fournisseur, inscrivez l’application et obtenez un ID client et une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-109">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="fb5ca-110">Pour plus d’informations, consultez <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-110">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="fb5ca-111">L’exemple d’application utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="fb5ca-111">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="fb5ca-112">Définir l’ID client et la clé secrète client</span><span class="sxs-lookup"><span data-stu-id="fb5ca-112">Set the client ID and client secret</span></span>

<span data-ttu-id="fb5ca-113">Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et d’une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-113">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="fb5ca-114">Les valeurs ID client et clé secrète client sont créées pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-114">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="fb5ca-115">Chaque fournisseur externe utilisé par l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-115">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="fb5ca-116">Pour plus d’informations, consultez les rubriques fournisseur d’authentification externe qui s’appliquent à votre scénario :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-116">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="fb5ca-117">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="fb5ca-117">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="fb5ca-118">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="fb5ca-118">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="fb5ca-119">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="fb5ca-119">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="fb5ca-120">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="fb5ca-120">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="fb5ca-121">Autres fournisseurs d’authentification</span><span class="sxs-lookup"><span data-stu-id="fb5ca-121">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="fb5ca-122">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="fb5ca-122">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="fb5ca-123">L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et une clé secrète client fournis par Google :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-123">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="fb5ca-124">Établir l’étendue de l’authentification</span><span class="sxs-lookup"><span data-stu-id="fb5ca-124">Establish the authentication scope</span></span>

<span data-ttu-id="fb5ca-125">Spécifiez la liste des autorisations à récupérer du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-125">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="fb5ca-126">Les étendues d’authentification pour les fournisseurs externes communs apparaissent dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-126">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="fb5ca-127">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="fb5ca-127">Provider</span></span>  | <span data-ttu-id="fb5ca-128">Étendue</span><span class="sxs-lookup"><span data-stu-id="fb5ca-128">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="fb5ca-129">Facebook</span><span class="sxs-lookup"><span data-stu-id="fb5ca-129">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="fb5ca-130">Google</span><span class="sxs-lookup"><span data-stu-id="fb5ca-130">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="fb5ca-131">Microsoft</span><span class="sxs-lookup"><span data-stu-id="fb5ca-131">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="fb5ca-132">Twitter</span><span class="sxs-lookup"><span data-stu-id="fb5ca-132">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="fb5ca-133">Dans l’exemple d’application, la portée `userinfo.profile` de Google est automatiquement ajoutée par l’infrastructure quand <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> est appelé sur le <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-133">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="fb5ca-134">Si l’application requiert des étendues supplémentaires, ajoutez-les aux options.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-134">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="fb5ca-135">Dans l’exemple suivant, l’étendue Google `https://www.googleapis.com/auth/user.birthday.read` est ajoutée afin de récupérer l’anniversaire d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-135">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="fb5ca-136">Mapper des clés de données utilisateur et créer des revendications</span><span class="sxs-lookup"><span data-stu-id="fb5ca-136">Map user data keys and create claims</span></span>

<span data-ttu-id="fb5ca-137">Dans les options du fournisseur, spécifiez un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> ou <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> pour chaque clé/sous-clé dans les données utilisateur JSON du fournisseur externe pour que l’identité de l’application soit lue lors de la connexion.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="fb5ca-138">Pour plus d’informations sur les types de revendication, consultez <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="fb5ca-139">L’exemple d’application crée des revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) à partir des clés `locale` et `picture` dans Google User Data :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-139">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="fb5ca-140">Dans `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-140">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="fb5ca-141">Pendant le processus de connexion, les <xref:Microsoft.AspNetCore.Identity.UserManager%601> peuvent stocker des revendications `ApplicationUser` pour les données utilisateur disponibles à partir du <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="fb5ca-142">Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*) établit les revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) pour le `ApplicationUser`connecté, y compris une revendication pour <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="fb5ca-143">Par défaut, les revendications d’un utilisateur sont stockées dans le cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-143">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="fb5ca-144">Si le cookie d’authentification est trop volumineux, l’application risque d’échouer en raison des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-144">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="fb5ca-145">Le navigateur détecte que l’en-tête du cookie est trop long.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-145">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="fb5ca-146">La taille globale de la demande est trop importante.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-146">The overall size of the request is too large.</span></span>

<span data-ttu-id="fb5ca-147">Si une grande quantité de données utilisateur est requise pour le traitement des demandes de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-147">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="fb5ca-148">Limitez le nombre et la taille des revendications utilisateur pour le traitement des demandes uniquement pour ce que l’application requiert.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-148">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="fb5ca-149">Utilisez un <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> personnalisé pour l' <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> de l’intergiciel d’authentification des cookies pour stocker l’identité entre les demandes.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-149">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="fb5ca-150">Conservez de grandes quantités d’informations d’identité sur le serveur tout en envoyant une petite clé d’identificateur de session au client uniquement.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-150">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="fb5ca-151">Enregistrer le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="fb5ca-151">Save the access token</span></span>

<span data-ttu-id="fb5ca-152"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockés dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-152"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="fb5ca-153">`SaveTokens` est défini sur `false` par défaut pour réduire la taille du cookie d’authentification final.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-153">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="fb5ca-154">L’exemple d’application définit la valeur de `SaveTokens` sur `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-154">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="fb5ca-155">Lorsque `OnPostConfirmationAsync` s’exécute, stockez le jeton d’accès ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `AuthenticationProperties`du `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-155">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="fb5ca-156">L’exemple d’application enregistre le jeton d’accès dans `OnPostConfirmationAsync` (nouvelle inscription d’utilisateur) et `OnGetCallbackAsync` (utilisateur précédemment inscrit) dans *Account/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-156">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="fb5ca-157">Comment ajouter des jetons personnalisés supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fb5ca-157">How to add additional custom tokens</span></span>

<span data-ttu-id="fb5ca-158">Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute une <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec la <xref:System.DateTime> actuelle pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-158">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="fb5ca-159">Création et ajout de revendications</span><span class="sxs-lookup"><span data-stu-id="fb5ca-159">Creating and adding claims</span></span>

<span data-ttu-id="fb5ca-160">L’infrastructure fournit des actions courantes et des méthodes d’extension pour créer et ajouter des revendications à la collection.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-160">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="fb5ca-161">Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> et <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-161">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="fb5ca-162">Les utilisateurs peuvent définir des actions personnalisées en dérivant de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> et en implémentant la méthode abstraite <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-162">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="fb5ca-163">Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-163">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="fb5ca-164">Suppression des revendications et des actions de revendication</span><span class="sxs-lookup"><span data-stu-id="fb5ca-164">Removal of claim actions and claims</span></span>

<span data-ttu-id="fb5ca-165">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) supprime toutes les actions de revendication pour le <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné de la collection.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-165">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="fb5ca-166">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) supprime une revendication du <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné à partir de l’identité.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-166">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="fb5ca-167"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> est principalement utilisé avec [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) pour supprimer les revendications générées par le protocole.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-167"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="fb5ca-168">Exemple de sortie d’application</span><span class="sxs-lookup"><span data-stu-id="fb5ca-168">Sample app output</span></span>

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

<span data-ttu-id="fb5ca-169">Une application ASP.NET Core peut établir des revendications et des jetons supplémentaires à partir de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-169">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="fb5ca-170">Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données utilisateur en revendications supplémentaires est le même.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-170">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="fb5ca-171">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb5ca-171">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb5ca-172">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="fb5ca-172">Prerequisites</span></span>

<span data-ttu-id="fb5ca-173">Choisissez les fournisseurs d’authentification externes à prendre en charge dans l’application.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-173">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="fb5ca-174">Pour chaque fournisseur, inscrivez l’application et obtenez un ID client et une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-174">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="fb5ca-175">Pour plus d’informations, consultez <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-175">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="fb5ca-176">L’exemple d’application utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="fb5ca-176">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="fb5ca-177">Définir l’ID client et la clé secrète client</span><span class="sxs-lookup"><span data-stu-id="fb5ca-177">Set the client ID and client secret</span></span>

<span data-ttu-id="fb5ca-178">Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et d’une clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-178">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="fb5ca-179">Les valeurs ID client et clé secrète client sont créées pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-179">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="fb5ca-180">Chaque fournisseur externe utilisé par l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-180">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="fb5ca-181">Pour plus d’informations, consultez les rubriques fournisseur d’authentification externe qui s’appliquent à votre scénario :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-181">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="fb5ca-182">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="fb5ca-182">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="fb5ca-183">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="fb5ca-183">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="fb5ca-184">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="fb5ca-184">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="fb5ca-185">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="fb5ca-185">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="fb5ca-186">Autres fournisseurs d’authentification</span><span class="sxs-lookup"><span data-stu-id="fb5ca-186">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="fb5ca-187">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="fb5ca-187">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="fb5ca-188">L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et une clé secrète client fournis par Google :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-188">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="fb5ca-189">Établir l’étendue de l’authentification</span><span class="sxs-lookup"><span data-stu-id="fb5ca-189">Establish the authentication scope</span></span>

<span data-ttu-id="fb5ca-190">Spécifiez la liste des autorisations à récupérer du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-190">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="fb5ca-191">Les étendues d’authentification pour les fournisseurs externes communs apparaissent dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-191">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="fb5ca-192">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="fb5ca-192">Provider</span></span>  | <span data-ttu-id="fb5ca-193">Étendue</span><span class="sxs-lookup"><span data-stu-id="fb5ca-193">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="fb5ca-194">Facebook</span><span class="sxs-lookup"><span data-stu-id="fb5ca-194">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="fb5ca-195">Google</span><span class="sxs-lookup"><span data-stu-id="fb5ca-195">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="fb5ca-196">Microsoft</span><span class="sxs-lookup"><span data-stu-id="fb5ca-196">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="fb5ca-197">Twitter</span><span class="sxs-lookup"><span data-stu-id="fb5ca-197">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="fb5ca-198">Dans l’exemple d’application, la portée `userinfo.profile` de Google est automatiquement ajoutée par l’infrastructure quand <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> est appelé sur le <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-198">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="fb5ca-199">Si l’application requiert des étendues supplémentaires, ajoutez-les aux options.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-199">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="fb5ca-200">Dans l’exemple suivant, l’étendue Google `https://www.googleapis.com/auth/user.birthday.read` est ajoutée afin de récupérer l’anniversaire d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-200">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="fb5ca-201">Mapper des clés de données utilisateur et créer des revendications</span><span class="sxs-lookup"><span data-stu-id="fb5ca-201">Map user data keys and create claims</span></span>

<span data-ttu-id="fb5ca-202">Dans les options du fournisseur, spécifiez un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> ou <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> pour chaque clé/sous-clé dans les données utilisateur JSON du fournisseur externe pour que l’identité de l’application soit lue lors de la connexion.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-202">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="fb5ca-203">Pour plus d’informations sur les types de revendication, consultez <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-203">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="fb5ca-204">L’exemple d’application crée des revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) à partir des clés `locale` et `picture` dans Google User Data :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-204">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="fb5ca-205">Dans `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-205">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="fb5ca-206">Pendant le processus de connexion, les <xref:Microsoft.AspNetCore.Identity.UserManager%601> peuvent stocker des revendications `ApplicationUser` pour les données utilisateur disponibles à partir du <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-206">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="fb5ca-207">Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*) établit les revendications de paramètres régionaux (`urn:google:locale`) et d’image (`urn:google:picture`) pour le `ApplicationUser`connecté, y compris une revendication pour <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-207">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="fb5ca-208">Par défaut, les revendications d’un utilisateur sont stockées dans le cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-208">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="fb5ca-209">Si le cookie d’authentification est trop volumineux, l’application risque d’échouer en raison des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-209">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="fb5ca-210">Le navigateur détecte que l’en-tête du cookie est trop long.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-210">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="fb5ca-211">La taille globale de la demande est trop importante.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-211">The overall size of the request is too large.</span></span>

<span data-ttu-id="fb5ca-212">Si une grande quantité de données utilisateur est requise pour le traitement des demandes de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="fb5ca-212">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="fb5ca-213">Limitez le nombre et la taille des revendications utilisateur pour le traitement des demandes uniquement pour ce que l’application requiert.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-213">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="fb5ca-214">Utilisez un <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> personnalisé pour l' <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> de l’intergiciel d’authentification des cookies pour stocker l’identité entre les demandes.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-214">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="fb5ca-215">Conservez de grandes quantités d’informations d’identité sur le serveur tout en envoyant une petite clé d’identificateur de session au client uniquement.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-215">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="fb5ca-216">Enregistrer le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="fb5ca-216">Save the access token</span></span>

<span data-ttu-id="fb5ca-217"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockés dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-217"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="fb5ca-218">`SaveTokens` est défini sur `false` par défaut pour réduire la taille du cookie d’authentification final.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-218">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="fb5ca-219">L’exemple d’application définit la valeur de `SaveTokens` sur `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-219">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="fb5ca-220">Lorsque `OnPostConfirmationAsync` s’exécute, stockez le jeton d’accès ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `AuthenticationProperties`du `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-220">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="fb5ca-221">L’exemple d’application enregistre le jeton d’accès dans `OnPostConfirmationAsync` (nouvelle inscription d’utilisateur) et `OnGetCallbackAsync` (utilisateur précédemment inscrit) dans *Account/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-221">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="fb5ca-222">Comment ajouter des jetons personnalisés supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fb5ca-222">How to add additional custom tokens</span></span>

<span data-ttu-id="fb5ca-223">Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute une <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec la <xref:System.DateTime> actuelle pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="fb5ca-223">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="fb5ca-224">Création et ajout de revendications</span><span class="sxs-lookup"><span data-stu-id="fb5ca-224">Creating and adding claims</span></span>

<span data-ttu-id="fb5ca-225">L’infrastructure fournit des actions courantes et des méthodes d’extension pour créer et ajouter des revendications à la collection.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-225">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="fb5ca-226">Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> et <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-226">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="fb5ca-227">Les utilisateurs peuvent définir des actions personnalisées en dérivant de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> et en implémentant la méthode abstraite <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-227">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="fb5ca-228">Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-228">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="fb5ca-229">Suppression des revendications et des actions de revendication</span><span class="sxs-lookup"><span data-stu-id="fb5ca-229">Removal of claim actions and claims</span></span>

<span data-ttu-id="fb5ca-230">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) supprime toutes les actions de revendication pour le <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné de la collection.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-230">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="fb5ca-231">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) supprime une revendication du <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> donné à partir de l’identité.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-231">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="fb5ca-232"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> est principalement utilisé avec [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) pour supprimer les revendications générées par le protocole.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-232"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="fb5ca-233">Exemple de sortie d’application</span><span class="sxs-lookup"><span data-stu-id="fb5ca-233">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="fb5ca-234">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fb5ca-234">Additional resources</span></span>

* <span data-ttu-id="fb5ca-235">L' [application SocialSample d’ingénierie dotnet/AspNetCore](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; l’exemple d’application liée se trouve sur la branche ingénieurs [de dotnet/AspNetCore GitHub référentiel](https://github.com/dotnet/AspNetCore) `master`.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-235">[dotnet/AspNetCore engineering SocialSample app](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [dotnet/AspNetCore GitHub repo's](https://github.com/dotnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="fb5ca-236">La branche `master` contient du code sous développement actif pour la prochaine version de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb5ca-236">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="fb5ca-237">Pour afficher une version de l’exemple d’application pour une version finale de ASP.NET Core, utilisez la liste déroulante **branche** pour sélectionner une branche de version (par exemple `release/{X.Y}`).</span><span class="sxs-lookup"><span data-stu-id="fb5ca-237">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
