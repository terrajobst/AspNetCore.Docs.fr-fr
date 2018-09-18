---
title: Configurer ASP.NET Core Identity
author: AdrienTorris
description: Comprendre les valeurs par défaut de ASP.NET Core Identity et découvrez comment configurer les propriétés d’identité pour utiliser des valeurs personnalisées.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 0faab001b981c79f6afa16b2a8cf80c1ef141b11
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011298"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="67631-103">Configurer ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="67631-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="67631-104">ASP.NET Core Identity utilise les valeurs par défaut pour les paramètres de stratégie de mot de passe, de verrouillage et de configuration du cookie.</span><span class="sxs-lookup"><span data-stu-id="67631-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="67631-105">Ces paramètres peuvent être remplacés dans le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="67631-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="67631-106">Options d’identité</span><span class="sxs-lookup"><span data-stu-id="67631-106">Identity options</span></span>

<span data-ttu-id="67631-107">Le [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe représente les options qui peuvent être utilisées pour configurer le système d’identité.</span><span class="sxs-lookup"><span data-stu-id="67631-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="67631-108">`IdentityOptions` doit avoir la valeur **après** appelant `AddIdentity` ou `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="67631-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="67631-109">Identité de revendications</span><span class="sxs-lookup"><span data-stu-id="67631-109">Claims Identity</span></span>

<span data-ttu-id="67631-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Spécifie le [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) avec les propriétés affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="67631-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="67631-111">Propriété</span><span class="sxs-lookup"><span data-stu-id="67631-111">Property</span></span> | <span data-ttu-id="67631-112">Description</span><span class="sxs-lookup"><span data-stu-id="67631-112">Description</span></span> | <span data-ttu-id="67631-113">Par défaut</span><span class="sxs-lookup"><span data-stu-id="67631-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="67631-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="67631-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="67631-115">Obtient ou définit le type de revendication utilisé pour une revendication de rôle.</span><span class="sxs-lookup"><span data-stu-id="67631-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="67631-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="67631-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="67631-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="67631-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="67631-118">Obtient ou définit le type de revendication utilisé pour la revendication d’horodatage de sécurité.</span><span class="sxs-lookup"><span data-stu-id="67631-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="67631-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="67631-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="67631-120">Obtient ou définit le type de revendication utilisé pour la revendication d’identificateur utilisateur.</span><span class="sxs-lookup"><span data-stu-id="67631-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="67631-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="67631-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="67631-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="67631-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="67631-123">Obtient ou définit le type de revendication utilisé pour la revendication de nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="67631-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="67631-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="67631-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="67631-125">Verrouillage</span><span class="sxs-lookup"><span data-stu-id="67631-125">Lockout</span></span>

<span data-ttu-id="67631-126">Verrouillage est défini dans le [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) méthode :</span><span class="sxs-lookup"><span data-stu-id="67631-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="67631-127">Le code précédent est basé sur le `Login` modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="67631-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="67631-128">Options de verrouillage sont définies `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="67631-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="67631-129">Le code précédent définit le [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="67631-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="67631-130">Une authentification réussie réinitialise le nombre de tentatives d’accès ayant échoué et réinitialise l’horloge.</span><span class="sxs-lookup"><span data-stu-id="67631-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="67631-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Spécifie le [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="67631-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="67631-132">Propriété</span><span class="sxs-lookup"><span data-stu-id="67631-132">Property</span></span> | <span data-ttu-id="67631-133">Description</span><span class="sxs-lookup"><span data-stu-id="67631-133">Description</span></span> | <span data-ttu-id="67631-134">Par défaut</span><span class="sxs-lookup"><span data-stu-id="67631-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="67631-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="67631-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="67631-136">Détermine si un utilisateur peut être verrouillé.</span><span class="sxs-lookup"><span data-stu-id="67631-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="67631-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="67631-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="67631-138">La durée pendant laquelle un utilisateur est verrouillé lorsqu’un verrouillage se produit.</span><span class="sxs-lookup"><span data-stu-id="67631-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="67631-139">5 minutes</span><span class="sxs-lookup"><span data-stu-id="67631-139">5 minutes</span></span> |
| [<span data-ttu-id="67631-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="67631-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="67631-141">Le nombre de tentatives d’accès ayant échoué jusqu'à ce qu’un utilisateur est verrouillé, si le verrouillage est activé.</span><span class="sxs-lookup"><span data-stu-id="67631-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="67631-142">5</span><span class="sxs-lookup"><span data-stu-id="67631-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="67631-143">Mot de passe</span><span class="sxs-lookup"><span data-stu-id="67631-143">Password</span></span>

<span data-ttu-id="67631-144">Par défaut, Identity requiert que les mots de passe contiennent une majuscule, une minuscule, un chiffre et un caractère non alphanumérique.</span><span class="sxs-lookup"><span data-stu-id="67631-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="67631-145">Les mots de passe doivent comporter au moins six caractères.</span><span class="sxs-lookup"><span data-stu-id="67631-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="67631-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) peuvent être définies dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="67631-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="67631-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Spécifie le [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="67631-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="67631-148">Propriété</span><span class="sxs-lookup"><span data-stu-id="67631-148">Property</span></span> | <span data-ttu-id="67631-149">Description</span><span class="sxs-lookup"><span data-stu-id="67631-149">Description</span></span> | <span data-ttu-id="67631-150">Par défaut</span><span class="sxs-lookup"><span data-stu-id="67631-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="67631-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="67631-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="67631-152">Nécessite un nombre compris entre 0-9 dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="67631-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="67631-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="67631-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="67631-154">La longueur minimale du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="67631-154">The minimum length of the password.</span></span> | <span data-ttu-id="67631-155">6</span><span class="sxs-lookup"><span data-stu-id="67631-155">6</span></span> |

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="67631-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | S’applique uniquement à ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="67631-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="67631-157">Requiert un nombre de caractères distincts dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="67631-157">Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="67631-158">| 1 |</span><span class="sxs-lookup"><span data-stu-id="67631-158">| 1 |</span></span>

::: moniker-end

<span data-ttu-id="67631-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Nécessite un caractère minuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="67631-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requires a lowercase character in the password.</span></span><span data-ttu-id="67631-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Nécessite un caractère non alphanumérique dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="67631-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requires a non-alphanumeric character in the password.</span></span><span data-ttu-id="67631-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Nécessite un caractère majuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="67631-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requires an uppercase character in the password.</span></span> | `true` |

### <a name="sign-in"></a><span data-ttu-id="67631-162">Connectez-vous</span><span class="sxs-lookup"><span data-stu-id="67631-162">Sign-in</span></span>

<span data-ttu-id="67631-163">Le code suivant définit `SignIn` paramètres (pour les valeurs par défaut) :</span><span class="sxs-lookup"><span data-stu-id="67631-163">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="67631-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Spécifie le [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="67631-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="67631-165">Propriété</span><span class="sxs-lookup"><span data-stu-id="67631-165">Property</span></span> | <span data-ttu-id="67631-166">Description</span><span class="sxs-lookup"><span data-stu-id="67631-166">Description</span></span> | <span data-ttu-id="67631-167">Par défaut</span><span class="sxs-lookup"><span data-stu-id="67631-167">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="67631-168">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="67631-168">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="67631-169">Nécessite un e-mail de confirmation pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="67631-169">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="67631-170">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="67631-170">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="67631-171">Nécessite un numéro de téléphone confirmé pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="67631-171">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="67631-172">jetons</span><span class="sxs-lookup"><span data-stu-id="67631-172">Tokens</span></span>

<span data-ttu-id="67631-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Spécifie le [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="67631-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="67631-174">Propriété</span><span class="sxs-lookup"><span data-stu-id="67631-174">Property</span></span>                                                         |                                                                                      <span data-ttu-id="67631-175">Description</span><span class="sxs-lookup"><span data-stu-id="67631-175">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="67631-176">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="67631-176">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="67631-177">Obtient ou définit le `AuthenticatorTokenProvider` permet de valider les connexions à deux facteurs avec un authentificateur.</span><span class="sxs-lookup"><span data-stu-id="67631-177">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="67631-178">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="67631-178">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="67631-179">Obtient ou définit le `ChangeEmailTokenProvider` permet de générer des jetons utilisés dans l’e-mail de confirmation de modification de courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="67631-179">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="67631-180">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="67631-180">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="67631-181">Obtient ou définit le `ChangePhoneNumberTokenProvider` permet de générer des jetons utilisés lors de la modification des numéros de téléphone.</span><span class="sxs-lookup"><span data-stu-id="67631-181">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="67631-182">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="67631-182">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="67631-183">Obtient ou définit le fournisseur de jetons utilisé pour générer des jetons utilisés dans l’e-mail de confirmation de compte.</span><span class="sxs-lookup"><span data-stu-id="67631-183">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="67631-184">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="67631-184">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="67631-185">Obtient ou définit le [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) permet de générer des jetons utilisés dans des messages électroniques de réinitialisation de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="67631-185">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="67631-186">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="67631-186">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="67631-187">Utilisé pour construire un [fournisseur de jetons utilisateur](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) avec la clé utilisée en tant que le nom du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="67631-187">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="67631-188">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="67631-188">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="67631-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Spécifie le [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="67631-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="67631-190">Propriété</span><span class="sxs-lookup"><span data-stu-id="67631-190">Property</span></span> | <span data-ttu-id="67631-191">Description</span><span class="sxs-lookup"><span data-stu-id="67631-191">Description</span></span> | <span data-ttu-id="67631-192">Par défaut</span><span class="sxs-lookup"><span data-stu-id="67631-192">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="67631-193">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="67631-193">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="67631-194">Caractères autorisés dans le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="67631-194">Allowed characters in the username.</span></span> | <span data-ttu-id="67631-195">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="67631-195">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="67631-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="67631-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="67631-197">0123456789</span><span class="sxs-lookup"><span data-stu-id="67631-197">0123456789</span></span><br><span data-ttu-id="67631-198">-._@+</span><span class="sxs-lookup"><span data-stu-id="67631-198">-._@+</span></span> |
| [<span data-ttu-id="67631-199">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="67631-199">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="67631-200">Chaque utilisateur doit avoir un e-mail unique.</span><span class="sxs-lookup"><span data-stu-id="67631-200">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="67631-201">Paramètres de cookies</span><span class="sxs-lookup"><span data-stu-id="67631-201">Cookie settings</span></span>

<span data-ttu-id="67631-202">Configurer le cookie de l’application dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="67631-202">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="67631-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) doit être appelé **après** appelant `AddIdentity` ou `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="67631-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="67631-204">Pour plus d’informations, consultez [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="67631-204">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>
