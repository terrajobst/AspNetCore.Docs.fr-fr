---
title: Configurer ASP.NET Core identité
author: AdrienTorris
description: Comprenez les valeurs par défaut de l’identité ASP.NET Core et apprenez à configurer les propriétés d’identité pour utiliser des valeurs personnalisées.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661405"
---
# <a name="configure-aspnet-core-identity"></a>Configurer ASP.NET Core identité

ASP.NET Core identité utilise des valeurs par défaut pour les paramètres tels que la stratégie de mot de passe, le verrouillage et la configuration des cookies. Ces paramètres peuvent être remplacés dans la classe `Startup`.

## <a name="identity-options"></a>Options d’identité

La classe [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) représente les options qui peuvent être utilisées pour configurer le système d’identité. `IdentityOptions` doit être défini **après** l’appel de `AddIdentity` ou `AddDefaultIdentity`.

### <a name="claims-identity"></a>Identité des revendications

[IdentityOptions. ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) spécifie le [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) avec les propriétés indiquées dans le tableau suivant.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Obtient ou définit le type de revendication utilisé pour une revendication de rôle. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication de cachet de sécurité. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication de l’identificateur d’utilisateur. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication de nom d’utilisateur. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Verrouillage

Le verrouillage est défini dans la méthode [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) :

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Le code précédent est basé sur le modèle d’identité `Login`. 

Les options de verrouillage sont définies dans `StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Le code précédent définit le [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) avec les valeurs par défaut.

Une authentification réussie réinitialise le nombre de tentatives d’accès ayant échoué et réinitialise l’horloge.

[IdentityOptions. lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) spécifie le [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les propriétés indiquées dans le tableau.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Détermine si un nouvel utilisateur peut être verrouillé. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Durée pendant laquelle un utilisateur est verrouillé lorsqu’un verrouillage se produit. | 5 minutes |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Nombre d’échecs de tentative d’accès jusqu’à ce qu’un utilisateur soit verrouillé, si le verrouillage est activé. | 5\. |

### <a name="password"></a>Password

Par défaut, Identity requiert que les mots de passe contiennent une majuscule, une minuscule, un chiffre et un caractère non alphanumérique. Les mots de passe doivent comporter au moins six caractères. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) peut être défini dans `Startup.ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions. Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) spécifie le [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) avec les propriétés indiquées dans le tableau.

::: moniker range=">= aspnetcore-2.0"

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Requiert un nombre compris entre 0-9 dans le mot de passe. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Longueur minimale du mot de passe. | 6\. |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requiert un caractère minuscule dans le mot de passe. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requiert un caractère non alphanumérique dans le mot de passe. | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | S’applique uniquement à ASP.NET Core 2,0 ou version ultérieure.<br><br> Requiert le nombre de caractères distincts dans le mot de passe. | 1 |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requiert un caractère majuscule dans le mot de passe. | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Requiert un nombre compris entre 0-9 dans le mot de passe. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Longueur minimale du mot de passe. | 6\. |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requiert un caractère minuscule dans le mot de passe. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requiert un caractère non alphanumérique dans le mot de passe. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requiert un caractère majuscule dans le mot de passe. | `true` |

::: moniker-end

### <a name="sign-in"></a>Connexion

Le code suivant définit `SignIn` paramètres (aux valeurs par défaut) :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions. Signy](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) spécifie le [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) avec les propriétés indiquées dans le tableau.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Requiert un e-mail confirmé pour la connexion. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Nécessite un numéro de téléphone confirmé pour la connexion. | `false` |

### <a name="tokens"></a>Jetons

[IdentityOptions. Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) spécifie le [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) avec les propriétés affichées dans le tableau.

|                                                        Propriété                                                         |                                                                                      Description                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Obtient ou définit le `AuthenticatorTokenProvider` utilisé pour valider les connexions à deux facteurs avec un authentificateur.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Obtient ou définit le `ChangeEmailTokenProvider` utilisé pour générer les jetons utilisés dans les e-mails de confirmation de modification de courrier électronique.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Obtient ou définit le `ChangePhoneNumberTokenProvider` utilisé pour générer les jetons utilisés lors de la modification des numéros de téléphone.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Obtient ou définit le fournisseur de jetons utilisé pour générer les jetons utilisés dans les e-mails de confirmation de compte.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Obtient ou définit le [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) utilisé pour générer les jetons utilisés dans les courriers électroniques de réinitialisation de mot de passe. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Utilisé pour construire un [fournisseur de jetons utilisateur](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) avec la clé utilisée comme nom du fournisseur.                 |

### <a name="user"></a>Utilisateur

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions. User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) spécifie l' [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) avec les propriétés affichées dans le tableau.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caractères autorisés dans le nom d’utilisateur. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Nécessite que chaque utilisateur dispose d’un e-mail unique. | `false` |

### <a name="cookie-settings"></a>Paramètres de cookie

Configurez le cookie de l’application dans `Startup.ConfigureServices`. [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) doit être appelé **après** l’appel de `AddIdentity` ou `AddDefaultIdentity`.

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

## <a name="password-hasher-options"></a>Options de hachage de mot de passe

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> obtient et définit les options de hachage de mot de passe.

| Option | Description |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | Mode de compatibilité utilisé lors du hachage de nouveaux mots de passe. La valeur par défaut est <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Le premier octet d’un mot de passe haché, appelé *marqueur de format*, spécifie la version de l’algorithme de hachage utilisé pour hacher le mot de passe. Lors de la vérification d’un mot de passe par rapport à un hachage, la méthode <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> sélectionne l’algorithme approprié en fonction du premier octet. Un client est en mesure de s’authentifier, quelle que soit la version de l’algorithme utilisée pour hacher le mot de passe. La définition du mode de compatibilité affecte le hachage de *nouveaux mots de passe*. |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | Nombre d’itérations utilisées pour hacher les mots de passe à l’aide de PBKDF2. Cette valeur est utilisée uniquement lorsque la <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> est définie sur <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. La valeur doit être un entier positif et la valeur par défaut est `10000`. |

Dans l’exemple suivant, le <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> est défini sur `12000` dans `Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
