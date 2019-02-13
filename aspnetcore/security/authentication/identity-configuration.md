---
title: Configurer ASP.NET Core Identity
author: AdrienTorris
description: Comprendre les valeurs par défaut de ASP.NET Core Identity et découvrez comment configurer les propriétés d’identité pour utiliser des valeurs personnalisées.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 3213f669cbfccdcda7cc7c0142b8101e696678e6
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159511"
---
# <a name="configure-aspnet-core-identity"></a>Configurer ASP.NET Core Identity

ASP.NET Core Identity utilise les valeurs par défaut pour les paramètres de stratégie de mot de passe, de verrouillage et de configuration du cookie. Ces paramètres peuvent être remplacés dans le `Startup` classe.

## <a name="identity-options"></a>Options d’identité

Le [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe représente les options qui peuvent être utilisées pour configurer le système d’identité. `IdentityOptions` doit avoir la valeur **après** appelant `AddIdentity` ou `AddDefaultIdentity`.

### <a name="claims-identity"></a>Identité de revendications

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Spécifie le [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) avec les propriétés affichées dans le tableau suivant.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Obtient ou définit le type de revendication utilisé pour une revendication de rôle. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication d’horodatage de sécurité. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication d’identificateur utilisateur. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication de nom d’utilisateur. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Verrouillage

Verrouillage est défini dans le [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) méthode :

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Le code précédent est basé sur le `Login` modèle d’identité. 

Options de verrouillage sont définies `StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Le code précédent définit le [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les valeurs par défaut.

Une authentification réussie réinitialise le nombre de tentatives d’accès ayant échoué et réinitialise l’horloge.

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Spécifie le [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Détermine si un utilisateur peut être verrouillé. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | La durée pendant laquelle un utilisateur est verrouillé lorsqu’un verrouillage se produit. | 5 minutes |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Le nombre de tentatives d’accès ayant échoué jusqu'à ce qu’un utilisateur est verrouillé, si le verrouillage est activé. | 5 |

### <a name="password"></a>Mot de passe

Par défaut, Identity requiert que les mots de passe contiennent une majuscule, une minuscule, un chiffre et un caractère non alphanumérique. Les mots de passe doivent comporter au moins six caractères. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) peuvent être définies dans `Startup.ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Spécifie le [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) avec les propriétés affichées dans la table.

::: moniker range=">= aspnetcore-2.0"

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Nécessite un nombre compris entre 0-9 dans le mot de passe. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La longueur minimale du mot de passe. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Nécessite un caractère minuscule dans le mot de passe. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Nécessite un caractère non alphanumérique dans le mot de passe. | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | S’applique uniquement à ASP.NET Core 2.0 ou version ultérieure.<br><br> Requiert un nombre de caractères distincts dans le mot de passe. | 1 |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Nécessite un caractère majuscule dans le mot de passe. | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Nécessite un nombre compris entre 0-9 dans le mot de passe. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La longueur minimale du mot de passe. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Nécessite un caractère minuscule dans le mot de passe. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Nécessite un caractère non alphanumérique dans le mot de passe. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Nécessite un caractère majuscule dans le mot de passe. | `true` |

::: moniker-end

### <a name="sign-in"></a>Connectez-vous

Le code suivant définit `SignIn` paramètres (pour les valeurs par défaut) :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Spécifie le [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Nécessite un e-mail de confirmation pour vous connecter. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Nécessite un numéro de téléphone confirmé pour vous connecter. | `false` |

### <a name="tokens"></a>jetons

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Spécifie le [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) avec les propriétés affichées dans la table.


|                                                        Propriété                                                         |                                                                                      Description                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Obtient ou définit le `AuthenticatorTokenProvider` permet de valider les connexions à deux facteurs avec un authentificateur.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Obtient ou définit le `ChangeEmailTokenProvider` permet de générer des jetons utilisés dans l’e-mail de confirmation de modification de courrier électronique.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Obtient ou définit le `ChangePhoneNumberTokenProvider` permet de générer des jetons utilisés lors de la modification des numéros de téléphone.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Obtient ou définit le fournisseur de jetons utilisé pour générer des jetons utilisés dans l’e-mail de confirmation de compte.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Obtient ou définit le [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) permet de générer des jetons utilisés dans des messages électroniques de réinitialisation de mot de passe. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Utilisé pour construire un [fournisseur de jetons utilisateur](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) avec la clé utilisée en tant que le nom du fournisseur.                 |

### <a name="user"></a>Utilisateur

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Spécifie le [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caractères autorisés dans le nom d’utilisateur. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Chaque utilisateur doit avoir un e-mail unique. | `false` |

### <a name="cookie-settings"></a>Paramètres de cookies

Configurer le cookie de l’application dans `Startup.ConfigureServices`. [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) doit être appelé **après** appelant `AddIdentity` ou `AddDefaultIdentity`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

Pour plus d’informations, consultez [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).

## <a name="password-hasher-options"></a>Options de hacheur de mot de passe

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> Obtient et définit des options pour le hachage de mot de passe.

| Option | Description |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | Le mode de compatibilité utilisé lors du hachage de nouveaux mots de passe. La valeur par défaut est <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Le premier octet d’un mot de passe haché, appelé un *marqueur de format*, spécifie la version de l’algorithme de hachage utilisé pour hacher le mot de passe. Lors de la vérification d’un mot de passe par rapport à un hachage, le <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> méthode sélectionne l’algorithme approprié basé sur le premier octet. Un client est en mesure d’authentifier, dont la version de l’algorithme utilisée pour hacher le mot de passe. Définition du mode de compatibilité affecte le hachage de *nouveaux mots de passe*. |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | Le nombre d’itérations utilisées lors du hachage des mots de passe à l’aide de PBKDF2. Cette valeur est utilisée uniquement lorsque la <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> est défini sur <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. La valeur doit être un entier positif et la valeur par défaut est `10000`. |

Dans l’exemple suivant, le <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> a la valeur `12000` dans `Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
