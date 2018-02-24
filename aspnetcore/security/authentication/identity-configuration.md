---
title: "Configurer l’identité de ASP.NET Core"
author: AdrienTorris
description: "Comprendre les valeurs par défaut de ASP.NET Core Identity et savoir comment configurer les propriétés d’identité pour utiliser des valeurs personnalisées."
manager: wpickett
ms.author: scaddie
ms.date: 02/21/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: c6f67240c4bfa5ddc1c3aad6c6270ed07349bc72
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="configure-identity"></a>Configurer l’identité

Identité de ASP.NET Core utilise la configuration par défaut pour les paramètres de stratégie de mot de passe, l’heure de verrouillage et paramètres de cookies. Ces paramètres peuvent être remplacés dans l’application `Startup` classe.

## <a name="identity-options"></a>Options d’identité

Le [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe représente les options qui peuvent être utilisées pour configurer le système d’identité.

### <a name="claims-identity"></a>Identité des revendications

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Spécifie le [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Obtient ou définit le type de revendication utilisé pour une revendication de rôle. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication d’horodatage de sécurité. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication d’identificateur utilisateur. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Obtient ou définit le type de revendication utilisé pour la revendication de nom d’utilisateur. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Verrouillage

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Spécifie le [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Détermine si un utilisateur peut être verrouillé. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | La durée pendant laquelle un utilisateur est verrouillé lorsqu’un verrouillage se produit. | 5 minutes |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Le nombre de tentatives d’accès ayant échoué jusqu'à ce qu’un utilisateur est verrouillé, si le verrouillage est activé. | 5 |

### <a name="password"></a>Mot de passe

Par défaut, Identity requiert que les mots de passe contiennent une majuscule, une minuscule, un chiffre et un caractère non alphanumérique. Les mots de passe doivent être au moins six caractères. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) peuvent être modifiées dans `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Base d’ASP.NET 2.0 a ajouté la [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) propriété. Dans le cas contraire, les options sont les mêmes que ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Spécifie le [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Nécessite un nombre compris entre 0 et 9 dans le mot de passe. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La longueur minimale du mot de passe. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | S’applique uniquement au cœur de ASP.NET 2.0 ou version ultérieure.<br><br> Requiert un nombre de caractères distincts dans le mot de passe. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Nécessite un caractère en minuscules dans le mot de passe. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Nécessite un caractère non alphanumérique dans le mot de passe. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Nécessite un caractère majuscule dans le mot de passe. | `true` |

### <a name="sign-in"></a>ouverture de session

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Spécifie le [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Nécessite un message électronique de confirmation pour vous connecter. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Nécessite un numéro de téléphone confirmé pour vous connecter. | `false` |

### <a name="tokens"></a>jetons

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Spécifie le [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) avec les propriétés affichées dans la table.

| Propriété | Description |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | Obtient ou définit le `AuthenticatorTokenProvider` permet de valider les connexions à deux facteurs avec un authentificateur. |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | Obtient ou définit le `ChangeEmailTokenProvider` permet de générer des jetons utilisés dans les messages électroniques de confirmation par courrier électronique modification. |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | Obtient ou définit le `ChangePhoneNumberTokenProvider` permet de générer des jetons utilisés lors de la modification des numéros de téléphone. |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | Obtient ou définit le fournisseur de jetons utilisé pour générer des jetons utilisés dans les messages électroniques de confirmation de compte. |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | Obtient ou définit le [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) permet de générer des jetons utilisés dans les messages électroniques de réinitialisation de mot de passe. |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | Utilisé pour construire un [fournisseur de jetons utilisateur](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) avec la clé utilisée en tant que le nom du fournisseur. |

### <a name="user"></a>Utilisateur

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Spécifie le [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) avec les propriétés affichées dans la table.

| Propriété | Description | Par défaut |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caractères autorisés dans le nom d’utilisateur. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Nécessite que chaque utilisateur à un message électronique unique. | `false` |

## <a name="cookie-settings"></a>Paramètres de cookies

Configurer le cookie de l’application dans `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) a les propriétés suivantes :

| Propriété | Description |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | Informe le gestionnaire qu’il doit modifier sortante *403 Interdit* code d’état dans un *redirection 302* sur le chemin d’accès donné.<br><br>La valeur par défaut est `/Account/AccessDenied`. |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Le nom logique pour un schéma d’authentification particulier. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Lorsque la valeur est true, l’authentification par cookie doit s’exécuter sur chaque demande et tenter de valider et de reconstruire toute entité sérialisée de que sa création. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Si la valeur est true, l’intergiciel (middleware) d’authentification gère les défis automatique. Si la valeur est false, l’intergiciel (middleware) d’authentification modifie uniquement les réponses lorsque explicitement indiqué par le `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | Obtient ou définit l’émetteur doit être utilisée pour toutes les revendications qui sont créées (héritée de [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | Le domaine auquel associer le cookie. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | Obtient ou définit la durée de vie d’un cookie. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | Indique si un cookie est accessible par un script côté client.<br><br>La valeur par défaut est `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Le nom du cookie.<br><br>La valeur par défaut est `.AspNetCore.Cookies`. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Le chemin d’accès du cookie. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | Le `SameSite` attribut du cookie.<br><br>La valeur par défaut est [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | Le [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) configuration.<br><br>La valeur par défaut est [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy). |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Le nom de domaine dans lequel le cookie est pris en charge. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Indicateur qui indique si le cookie doit être accessible uniquement aux serveurs.<br><br>La valeur par défaut est `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Permet d’isoler les applications qui s’exécutent sur le même nom d’hôte. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Un indicateur qui indique si le cookie créé doit être limité à HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou le même protocole que la demande (`CookieSecurePolicy.SameAsRequest`).<br><br>La valeur par défaut est `CookieSecurePolicy.SameAsRequest`. |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | Le composant utilisé pour obtenir les cookies de la demande ou de les définir dans la réponse. | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | Si la valeur, le fournisseur utilisé par le [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) pour la protection des données. |
| [Description](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | S’applique uniquement aux ASP.NET Core 1.x.<br><br> Informations supplémentaires sur le type d’authentification qui est rendue disponible à l’application. |
| [Événements](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | Le gestionnaire appelle des méthodes sur le fournisseur qui donnent le contrôle de l’application à certains points où le traitement se produit. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | Si la valeur, le service de type à obtenir le `Events` instance au lieu de la propriété (héritée de [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | Contrôle la quantité de temps le ticket d’authentification stocké dans le reste de cookie valide à partir du point de que sa création.<br><br>La valeur par défaut est de 14 jours. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | Lorsqu’un utilisateur n’est pas autorisé, ils sont redirigés vers ce chemin d’accès à la connexion.<br><br>La valeur par défaut est `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | Lorsqu’un utilisateur est déconnecté, ils sont redirigés vers ce chemin d’accès.<br><br>La valeur par défaut est `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | Détermine le nom du paramètre de chaîne de requête qui est ajouté par l’intergiciel (middleware) lorsqu’un *401 non autorisé* code d’état est passé à une *redirection 302* sur le chemin d’accès de connexion.<br><br>La valeur par défaut est `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | Conteneur facultatif dans lequel stocker l’identité des demandes. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | Lorsque la valeur est true, un nouveau cookie est émis avec une nouvelle heure d’expiration lorsque le cookie actuel est plus de milieu de la fenêtre d’expiration.<br><br>La valeur par défaut est `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | Le `TicketDataFormat` est utilisé pour protéger et déprotéger l’identité et autres propriétés qui sont stockées dans la valeur du cookie. |
