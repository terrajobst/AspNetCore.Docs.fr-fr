---
title: Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core
author: guardrex
description: Découvrez comment établir des revendications supplémentaires et des jetons provenant de fournisseurs externes.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874844"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="319a8-103">Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="319a8-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="319a8-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="319a8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="319a8-105">Une application ASP.NET Core peut établir des revendications supplémentaires et des jetons provenant de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="319a8-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="319a8-106">Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données de l’utilisateur vers des revendications supplémentaires est le même.</span><span class="sxs-lookup"><span data-stu-id="319a8-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="319a8-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="319a8-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="319a8-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="319a8-108">Prerequisites</span></span>

<span data-ttu-id="319a8-109">Décidez quels fournisseurs d’authentification externes pour prendre en charge dans l’application.</span><span class="sxs-lookup"><span data-stu-id="319a8-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="319a8-110">Pour chaque fournisseur, inscrire l’application et obtenir un ID client et la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="319a8-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="319a8-111">Pour plus d'informations, consultez <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="319a8-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="319a8-112">L’exemple d’application utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="319a8-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="319a8-113">Définir l’ID client et la clé secrète client</span><span class="sxs-lookup"><span data-stu-id="319a8-113">Set the client ID and client secret</span></span>

<span data-ttu-id="319a8-114">Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="319a8-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="319a8-115">ID client et les valeurs de clé secrète du client sont créés pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="319a8-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="319a8-116">Chaque fournisseur externe qui utilise l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="319a8-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="319a8-117">Pour plus d’informations, consultez les rubriques de fournisseur d’authentification externe qui s’appliquent à votre scénario :</span><span class="sxs-lookup"><span data-stu-id="319a8-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="319a8-118">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="319a8-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="319a8-119">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="319a8-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="319a8-120">Authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="319a8-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="319a8-121">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="319a8-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="319a8-122">Autres fournisseurs d’authentification</span><span class="sxs-lookup"><span data-stu-id="319a8-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="319a8-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="319a8-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="319a8-124">L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et la clé secrète client fournie par Google :</span><span class="sxs-lookup"><span data-stu-id="319a8-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="319a8-125">Établir l’étendue de l’authentification</span><span class="sxs-lookup"><span data-stu-id="319a8-125">Establish the authentication scope</span></span>

<span data-ttu-id="319a8-126">Spécifier la liste des autorisations nécessaires pour récupérer à partir du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="319a8-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="319a8-127">Étendues de l’authentification pour les fournisseurs externes courantes apparaissent dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="319a8-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="319a8-128">Fournisseur</span><span class="sxs-lookup"><span data-stu-id="319a8-128">Provider</span></span>  | <span data-ttu-id="319a8-129">Portée</span><span class="sxs-lookup"><span data-stu-id="319a8-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="319a8-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="319a8-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="319a8-131">Google</span><span class="sxs-lookup"><span data-stu-id="319a8-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="319a8-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="319a8-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="319a8-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="319a8-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="319a8-134">Dans de l’exemple d’application, Google `userinfo.profile` étendue est ajoutée automatiquement par le framework lorsque <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> est appelée sur le <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="319a8-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="319a8-135">Si l’application requiert des étendues supplémentaires, ajoutez-les aux options.</span><span class="sxs-lookup"><span data-stu-id="319a8-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="319a8-136">Dans l’exemple suivant, Google `https://www.googleapis.com/auth/user.birthday.read` étendue est ajoutée afin de récupérer la date d’anniversaire d’un utilisateur :</span><span class="sxs-lookup"><span data-stu-id="319a8-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="319a8-137">Mapper des clés de données utilisateur et de créer des revendications</span><span class="sxs-lookup"><span data-stu-id="319a8-137">Map user data keys and create claims</span></span>

<span data-ttu-id="319a8-138">Dans les options du fournisseur, vous devez spécifier un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> ou <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> pour chaque sous-clé/de clé dans les données d’utilisateur JSON du fournisseur externe pour l’identité de l’application à lire sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="319a8-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="319a8-139">Pour plus d’informations sur les types de revendications, consultez <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="319a8-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="319a8-140">L’exemple d’application crée des paramètres régionaux (`urn:google:locale`) et image (`urn:google:picture`) des revendications à partir du `locale` et `picture` clés dans les données utilisateur de Google :</span><span class="sxs-lookup"><span data-stu-id="319a8-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="319a8-141">Dans <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="319a8-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="319a8-142">Lors de la connexion des processus, le <xref:Microsoft.AspNetCore.Identity.UserManager%601> peut stocker un `ApplicationUser` revendications pour les données utilisateur disponibles à partir de la <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="319a8-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="319a8-143">Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) établit les paramètres régionaux (`urn:google:locale`) et image (`urn:google:picture`) revendications pour le signé dans `ApplicationUser`, y compris une revendication pour <xref:System.Security.Claims.ClaimTypes.GivenName> :</span><span class="sxs-lookup"><span data-stu-id="319a8-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="319a8-144">Par défaut, les revendications d’un utilisateur sont stockées dans le cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="319a8-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="319a8-145">Si le cookie d’authentification est trop volumineux, il peut entraîner l’application en échec, car :</span><span class="sxs-lookup"><span data-stu-id="319a8-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="319a8-146">Le navigateur détecte que l’en-tête de cookie est trop long.</span><span class="sxs-lookup"><span data-stu-id="319a8-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="319a8-147">La taille globale de la demande est trop grande.</span><span class="sxs-lookup"><span data-stu-id="319a8-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="319a8-148">Si une grande quantité de données utilisateur est nécessaire pour traiter les demandes d’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="319a8-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="319a8-149">Limiter le nombre et la taille de revendications d’utilisateur pour le traitement de requête à ce que l’application, il suffit.</span><span class="sxs-lookup"><span data-stu-id="319a8-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="319a8-150">Utiliser un personnalisé <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> du Middleware de Cookie d’authentification <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> pour stocker l’identité entre les requêtes.</span><span class="sxs-lookup"><span data-stu-id="319a8-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="319a8-151">Conserver les grandes quantités d’informations d’identité sur le serveur lors de l’envoi uniquement une clé d’identificateur de session small au client.</span><span class="sxs-lookup"><span data-stu-id="319a8-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="319a8-152">Enregistrer le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="319a8-152">Save the access token</span></span>

<span data-ttu-id="319a8-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockées dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie.</span><span class="sxs-lookup"><span data-stu-id="319a8-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="319a8-154">`SaveTokens` a la valeur `false` par défaut pour réduire la taille du cookie d’authentification finale.</span><span class="sxs-lookup"><span data-stu-id="319a8-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="319a8-155">L’exemple d’application définit la valeur de `SaveTokens` à `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="319a8-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="319a8-156">Lorsque `OnPostConfirmationAsync` s’exécute, stocker le jeton d’accès ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `ApplicationUser`de `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="319a8-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="319a8-157">L’exemple d’application enregistre le jeton d’accès dans `OnPostConfirmationAsync` (nouvelle inscription d’utilisateur) et `OnGetCallbackAsync` (utilisateur inscrit précédemment) dans *Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="319a8-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="319a8-158">Comment ajouter des jetons personnalisés supplémentaires</span><span class="sxs-lookup"><span data-stu-id="319a8-158">How to add additional custom tokens</span></span>

<span data-ttu-id="319a8-159">Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec actuel <xref:System.DateTime> pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="319a8-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="319a8-160">Création et ajout de revendications</span><span class="sxs-lookup"><span data-stu-id="319a8-160">Creating and adding claims</span></span>

<span data-ttu-id="319a8-161">Le framework fournit des actions courantes et les méthodes d’extension pour la création et ajout de revendications à la collection.</span><span class="sxs-lookup"><span data-stu-id="319a8-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="319a8-162">Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> et <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="319a8-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="319a8-163">Les utilisateurs peuvent définir des actions personnalisées en dérivant de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> et l’implémentation abstraite <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> (méthode).</span><span class="sxs-lookup"><span data-stu-id="319a8-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="319a8-164">Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="319a8-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="319a8-165">Suppression des actions de revendication et revendications</span><span class="sxs-lookup"><span data-stu-id="319a8-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="319a8-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) supprime toutes les revendications des actions pour la donnée <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> à partir de la collection.</span><span class="sxs-lookup"><span data-stu-id="319a8-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="319a8-167">[ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) supprime une revendication de la donnée <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> à partir de l’identité.</span><span class="sxs-lookup"><span data-stu-id="319a8-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="319a8-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> est principalement utilisé avec [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) pour supprimer les revendications généré par le protocole.</span><span class="sxs-lookup"><span data-stu-id="319a8-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="319a8-169">Résultat de l’application exemple</span><span class="sxs-lookup"><span data-stu-id="319a8-169">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="319a8-170">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="319a8-170">Additional resources</span></span>

* <span data-ttu-id="319a8-171">[ASPNET/AspNetCore ingénierie SocialSample application](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; l’application exemple lié se trouve sur le [du référentiel de GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` branche ingénierie.</span><span class="sxs-lookup"><span data-stu-id="319a8-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="319a8-172">Le `master` branche contient du code en cours de développement pour la prochaine version d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="319a8-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="319a8-173">Pour afficher une version de l’exemple d’application pour une version finale d’ASP.NET Core, utilisez le **branche** liste déroulante liste pour sélectionner une branche de version (par exemple `release/2.2`).</span><span class="sxs-lookup"><span data-stu-id="319a8-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
