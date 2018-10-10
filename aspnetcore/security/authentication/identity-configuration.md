---
title: Configurer ASP.NET Core Identity
author: AdrienTorris
description: Comprendre les valeurs par défaut de ASP.NET Core Identity et découvrez comment configurer les propriétés d’identité pour utiliser des valeurs personnalisées.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 02441cd28c2a99eda7b50ed54f4437d4b52ca5d9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911935"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="52b22-103">Configurer ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="52b22-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="52b22-104">ASP.NET Core Identity utilise les valeurs par défaut pour les paramètres de stratégie de mot de passe, de verrouillage et de configuration du cookie.</span><span class="sxs-lookup"><span data-stu-id="52b22-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="52b22-105">Ces paramètres peuvent être remplacés dans le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="52b22-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="52b22-106">Options d’identité</span><span class="sxs-lookup"><span data-stu-id="52b22-106">Identity options</span></span>

<span data-ttu-id="52b22-107">Le [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe représente les options qui peuvent être utilisées pour configurer le système d’identité.</span><span class="sxs-lookup"><span data-stu-id="52b22-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="52b22-108">`IdentityOptions` doit avoir la valeur **après** appelant `AddIdentity` ou `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="52b22-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="52b22-109">Identité de revendications</span><span class="sxs-lookup"><span data-stu-id="52b22-109">Claims Identity</span></span>

<span data-ttu-id="52b22-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Spécifie le [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) avec les propriétés affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="52b22-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="52b22-111">Propriété</span><span class="sxs-lookup"><span data-stu-id="52b22-111">Property</span></span> | <span data-ttu-id="52b22-112">Description</span><span class="sxs-lookup"><span data-stu-id="52b22-112">Description</span></span> | <span data-ttu-id="52b22-113">Par défaut</span><span class="sxs-lookup"><span data-stu-id="52b22-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="52b22-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="52b22-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="52b22-115">Obtient ou définit le type de revendication utilisé pour une revendication de rôle.</span><span class="sxs-lookup"><span data-stu-id="52b22-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="52b22-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="52b22-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="52b22-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="52b22-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="52b22-118">Obtient ou définit le type de revendication utilisé pour la revendication d’horodatage de sécurité.</span><span class="sxs-lookup"><span data-stu-id="52b22-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="52b22-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="52b22-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="52b22-120">Obtient ou définit le type de revendication utilisé pour la revendication d’identificateur utilisateur.</span><span class="sxs-lookup"><span data-stu-id="52b22-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="52b22-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="52b22-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="52b22-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="52b22-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="52b22-123">Obtient ou définit le type de revendication utilisé pour la revendication de nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="52b22-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="52b22-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="52b22-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="52b22-125">Verrouillage</span><span class="sxs-lookup"><span data-stu-id="52b22-125">Lockout</span></span>

<span data-ttu-id="52b22-126">Verrouillage est défini dans le [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) méthode :</span><span class="sxs-lookup"><span data-stu-id="52b22-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="52b22-127">Le code précédent est basé sur le `Login` modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="52b22-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="52b22-128">Options de verrouillage sont définies `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="52b22-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="52b22-129">Le code précédent définit le [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="52b22-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="52b22-130">Une authentification réussie réinitialise le nombre de tentatives d’accès ayant échoué et réinitialise l’horloge.</span><span class="sxs-lookup"><span data-stu-id="52b22-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="52b22-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Spécifie le [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="52b22-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="52b22-132">Propriété</span><span class="sxs-lookup"><span data-stu-id="52b22-132">Property</span></span> | <span data-ttu-id="52b22-133">Description</span><span class="sxs-lookup"><span data-stu-id="52b22-133">Description</span></span> | <span data-ttu-id="52b22-134">Par défaut</span><span class="sxs-lookup"><span data-stu-id="52b22-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="52b22-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="52b22-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="52b22-136">Détermine si un utilisateur peut être verrouillé.</span><span class="sxs-lookup"><span data-stu-id="52b22-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="52b22-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="52b22-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="52b22-138">La durée pendant laquelle un utilisateur est verrouillé lorsqu’un verrouillage se produit.</span><span class="sxs-lookup"><span data-stu-id="52b22-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="52b22-139">5 minutes</span><span class="sxs-lookup"><span data-stu-id="52b22-139">5 minutes</span></span> |
| [<span data-ttu-id="52b22-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="52b22-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="52b22-141">Le nombre de tentatives d’accès ayant échoué jusqu'à ce qu’un utilisateur est verrouillé, si le verrouillage est activé.</span><span class="sxs-lookup"><span data-stu-id="52b22-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="52b22-142">5</span><span class="sxs-lookup"><span data-stu-id="52b22-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="52b22-143">Mot de passe</span><span class="sxs-lookup"><span data-stu-id="52b22-143">Password</span></span>

<span data-ttu-id="52b22-144">Par défaut, Identity requiert que les mots de passe contiennent une majuscule, une minuscule, un chiffre et un caractère non alphanumérique.</span><span class="sxs-lookup"><span data-stu-id="52b22-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="52b22-145">Les mots de passe doivent comporter au moins six caractères.</span><span class="sxs-lookup"><span data-stu-id="52b22-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="52b22-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) peuvent être définies dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="52b22-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="52b22-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Spécifie le [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="52b22-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="52b22-148">Propriété</span><span class="sxs-lookup"><span data-stu-id="52b22-148">Property</span></span> | <span data-ttu-id="52b22-149">Description</span><span class="sxs-lookup"><span data-stu-id="52b22-149">Description</span></span> | <span data-ttu-id="52b22-150">Par défaut</span><span class="sxs-lookup"><span data-stu-id="52b22-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="52b22-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="52b22-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="52b22-152">Nécessite un nombre compris entre 0-9 dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="52b22-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="52b22-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="52b22-154">La longueur minimale du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-154">The minimum length of the password.</span></span> | <span data-ttu-id="52b22-155">6</span><span class="sxs-lookup"><span data-stu-id="52b22-155">6</span></span> |
| [<span data-ttu-id="52b22-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="52b22-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="52b22-157">Nécessite un caractère minuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="52b22-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="52b22-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="52b22-159">Nécessite un caractère non alphanumérique dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="52b22-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="52b22-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="52b22-161">S’applique uniquement à ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="52b22-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="52b22-162">Requiert un nombre de caractères distincts dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="52b22-163">1</span><span class="sxs-lookup"><span data-stu-id="52b22-163">1</span></span> |
| [<span data-ttu-id="52b22-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="52b22-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="52b22-165">Nécessite un caractère majuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="52b22-166">Propriété</span><span class="sxs-lookup"><span data-stu-id="52b22-166">Property</span></span> | <span data-ttu-id="52b22-167">Description</span><span class="sxs-lookup"><span data-stu-id="52b22-167">Description</span></span> | <span data-ttu-id="52b22-168">Par défaut</span><span class="sxs-lookup"><span data-stu-id="52b22-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="52b22-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="52b22-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="52b22-170">Nécessite un nombre compris entre 0-9 dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="52b22-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="52b22-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="52b22-172">La longueur minimale du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-172">The minimum length of the password.</span></span> | <span data-ttu-id="52b22-173">6</span><span class="sxs-lookup"><span data-stu-id="52b22-173">6</span></span> |
| [<span data-ttu-id="52b22-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="52b22-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="52b22-175">Nécessite un caractère minuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="52b22-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="52b22-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="52b22-177">Nécessite un caractère non alphanumérique dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="52b22-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="52b22-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="52b22-179">Nécessite un caractère majuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="52b22-180">Connectez-vous</span><span class="sxs-lookup"><span data-stu-id="52b22-180">Sign-in</span></span>

<span data-ttu-id="52b22-181">Le code suivant définit `SignIn` paramètres (pour les valeurs par défaut) :</span><span class="sxs-lookup"><span data-stu-id="52b22-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="52b22-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Spécifie le [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="52b22-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="52b22-183">Propriété</span><span class="sxs-lookup"><span data-stu-id="52b22-183">Property</span></span> | <span data-ttu-id="52b22-184">Description</span><span class="sxs-lookup"><span data-stu-id="52b22-184">Description</span></span> | <span data-ttu-id="52b22-185">Par défaut</span><span class="sxs-lookup"><span data-stu-id="52b22-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="52b22-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="52b22-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="52b22-187">Nécessite un e-mail de confirmation pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="52b22-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="52b22-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="52b22-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="52b22-189">Nécessite un numéro de téléphone confirmé pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="52b22-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="52b22-190">jetons</span><span class="sxs-lookup"><span data-stu-id="52b22-190">Tokens</span></span>

<span data-ttu-id="52b22-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Spécifie le [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="52b22-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="52b22-192">Propriété</span><span class="sxs-lookup"><span data-stu-id="52b22-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="52b22-193">Description</span><span class="sxs-lookup"><span data-stu-id="52b22-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="52b22-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="52b22-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="52b22-195">Obtient ou définit le `AuthenticatorTokenProvider` permet de valider les connexions à deux facteurs avec un authentificateur.</span><span class="sxs-lookup"><span data-stu-id="52b22-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="52b22-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="52b22-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="52b22-197">Obtient ou définit le `ChangeEmailTokenProvider` permet de générer des jetons utilisés dans l’e-mail de confirmation de modification de courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="52b22-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="52b22-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="52b22-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="52b22-199">Obtient ou définit le `ChangePhoneNumberTokenProvider` permet de générer des jetons utilisés lors de la modification des numéros de téléphone.</span><span class="sxs-lookup"><span data-stu-id="52b22-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="52b22-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="52b22-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="52b22-201">Obtient ou définit le fournisseur de jetons utilisé pour générer des jetons utilisés dans l’e-mail de confirmation de compte.</span><span class="sxs-lookup"><span data-stu-id="52b22-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="52b22-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="52b22-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="52b22-203">Obtient ou définit le [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) permet de générer des jetons utilisés dans des messages électroniques de réinitialisation de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52b22-203">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="52b22-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="52b22-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="52b22-205">Utilisé pour construire un [fournisseur de jetons utilisateur](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) avec la clé utilisée en tant que le nom du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="52b22-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="52b22-206">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="52b22-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="52b22-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Spécifie le [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) avec les propriétés affichées dans la table.</span><span class="sxs-lookup"><span data-stu-id="52b22-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="52b22-208">Propriété</span><span class="sxs-lookup"><span data-stu-id="52b22-208">Property</span></span> | <span data-ttu-id="52b22-209">Description</span><span class="sxs-lookup"><span data-stu-id="52b22-209">Description</span></span> | <span data-ttu-id="52b22-210">Par défaut</span><span class="sxs-lookup"><span data-stu-id="52b22-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="52b22-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="52b22-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="52b22-212">Caractères autorisés dans le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="52b22-212">Allowed characters in the username.</span></span> | <span data-ttu-id="52b22-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="52b22-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="52b22-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="52b22-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="52b22-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="52b22-215">0123456789</span></span><br><span data-ttu-id="52b22-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="52b22-216">-.\_@+</span></span> |
| [<span data-ttu-id="52b22-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="52b22-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="52b22-218">Chaque utilisateur doit avoir un e-mail unique.</span><span class="sxs-lookup"><span data-stu-id="52b22-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="52b22-219">Paramètres de cookies</span><span class="sxs-lookup"><span data-stu-id="52b22-219">Cookie settings</span></span>

<span data-ttu-id="52b22-220">Configurer le cookie de l’application dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="52b22-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="52b22-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) doit être appelé **après** appelant `AddIdentity` ou `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="52b22-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="52b22-222">Pour plus d’informations, consultez [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="52b22-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>
