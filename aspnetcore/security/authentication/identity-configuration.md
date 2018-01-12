---
title: "Configurer l’identité de ASP.NET Core"
author: AdrienTorris
description: "Comprendre les valeurs par défaut de ASP.NET Core Identity et configurer les propriétés d’identité différents pour utiliser des valeurs personnalisées."
keywords: "Authentification ASP.NET Core, identité, sécurité"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="b91c4-104">Configurer l’identité</span><span class="sxs-lookup"><span data-stu-id="b91c4-104">Configure Identity</span></span>

<span data-ttu-id="b91c4-105">ASP.NET Core Identity a des comportements courants dans les applications telles que la stratégie de mot de passe, l’heure de verrouillage et les paramètres de cookies que vous pouvez remplacer facilement dans votre application `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="b91c4-106">Stratégie de mots de passe</span><span class="sxs-lookup"><span data-stu-id="b91c4-106">Passwords policy</span></span>

<span data-ttu-id="b91c4-107">Par défaut, identité requiert que les mots de passe contient un caractère majuscule, minuscule, un chiffre et un caractère non alphanumérique.</span><span class="sxs-lookup"><span data-stu-id="b91c4-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="b91c4-108">Il existe également quelques autres restrictions.</span><span class="sxs-lookup"><span data-stu-id="b91c4-108">There are also some other restrictions.</span></span> <span data-ttu-id="b91c4-109">Pour simplifier les restrictions de mot de passe, vous devez modifier le `ConfigureServices` méthode de la `Startup` classe de votre application.</span><span class="sxs-lookup"><span data-stu-id="b91c4-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b91c4-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b91c4-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b91c4-111">Base d’ASP.NET 2.0 a ajouté le `RequiredUniqueChars` propriété.</span><span class="sxs-lookup"><span data-stu-id="b91c4-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="b91c4-112">Dans le cas contraire, les options sont les mêmes à partir de ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="b91c4-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b91c4-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b91c4-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="b91c4-114">`IdentityOptions.Password`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="b91c4-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="b91c4-115">Propriété</span><span class="sxs-lookup"><span data-stu-id="b91c4-115">Property</span></span>                | <span data-ttu-id="b91c4-116">Description</span><span class="sxs-lookup"><span data-stu-id="b91c4-116">Description</span></span>                       | <span data-ttu-id="b91c4-117">Par défaut</span><span class="sxs-lookup"><span data-stu-id="b91c4-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="b91c4-118">Nécessite un nombre compris entre 0 et 9 dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="b91c4-119">true</span><span class="sxs-lookup"><span data-stu-id="b91c4-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="b91c4-120">La longueur minimale du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-120">The minimum length of the password.</span></span> | <span data-ttu-id="b91c4-121">6</span><span class="sxs-lookup"><span data-stu-id="b91c4-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="b91c4-122">Nécessite un caractère non alphanumérique dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="b91c4-123">true</span><span class="sxs-lookup"><span data-stu-id="b91c4-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="b91c4-124">Nécessite une lettre majuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="b91c4-125">true</span><span class="sxs-lookup"><span data-stu-id="b91c4-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="b91c4-126">Nécessite un caractère minuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="b91c4-127">true</span><span class="sxs-lookup"><span data-stu-id="b91c4-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="b91c4-128">Requiert un nombre de caractères distincts dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="b91c4-129">1</span><span class="sxs-lookup"><span data-stu-id="b91c4-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="b91c4-130">Verrouillage de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="b91c4-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="b91c4-131">`IdentityOptions.Lockout`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="b91c4-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="b91c4-132">Propriété</span><span class="sxs-lookup"><span data-stu-id="b91c4-132">Property</span></span>                | <span data-ttu-id="b91c4-133">Description</span><span class="sxs-lookup"><span data-stu-id="b91c4-133">Description</span></span>                       | <span data-ttu-id="b91c4-134">Par défaut</span><span class="sxs-lookup"><span data-stu-id="b91c4-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="b91c4-135">La durée pendant laquelle un utilisateur est verrouillé lorsqu’un verrouillage se produit.</span><span class="sxs-lookup"><span data-stu-id="b91c4-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="b91c4-136">5 minutes</span><span class="sxs-lookup"><span data-stu-id="b91c4-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="b91c4-137">Le nombre de tentatives d’accès ayant échoué jusqu'à ce qu’un utilisateur est verrouillé, si le verrouillage est activé.</span><span class="sxs-lookup"><span data-stu-id="b91c4-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="b91c4-138">5</span><span class="sxs-lookup"><span data-stu-id="b91c4-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="b91c4-139">Détermine si un utilisateur peut être verrouillé.</span><span class="sxs-lookup"><span data-stu-id="b91c4-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="b91c4-140">true</span><span class="sxs-lookup"><span data-stu-id="b91c4-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="b91c4-141">Paramètres de connexion</span><span class="sxs-lookup"><span data-stu-id="b91c4-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="b91c4-142">`IdentityOptions.SignIn`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="b91c4-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="b91c4-143">Propriété</span><span class="sxs-lookup"><span data-stu-id="b91c4-143">Property</span></span>                | <span data-ttu-id="b91c4-144">Description</span><span class="sxs-lookup"><span data-stu-id="b91c4-144">Description</span></span>                       | <span data-ttu-id="b91c4-145">Par défaut</span><span class="sxs-lookup"><span data-stu-id="b91c4-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="b91c4-146">Nécessite un message électronique de confirmation pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="b91c4-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="b91c4-147">False</span><span class="sxs-lookup"><span data-stu-id="b91c4-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="b91c4-148">Nécessite un numéro de téléphone confirmé pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="b91c4-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="b91c4-149">False</span><span class="sxs-lookup"><span data-stu-id="b91c4-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="b91c4-150">Paramètres de validation d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="b91c4-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="b91c4-151">`IdentityOptions.User`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="b91c4-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="b91c4-152">Propriété</span><span class="sxs-lookup"><span data-stu-id="b91c4-152">Property</span></span>                | <span data-ttu-id="b91c4-153">Description</span><span class="sxs-lookup"><span data-stu-id="b91c4-153">Description</span></span>                       | <span data-ttu-id="b91c4-154">Par défaut</span><span class="sxs-lookup"><span data-stu-id="b91c4-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="b91c4-155">Nécessite que chaque utilisateur à un message électronique unique.</span><span class="sxs-lookup"><span data-stu-id="b91c4-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="b91c4-156">False</span><span class="sxs-lookup"><span data-stu-id="b91c4-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="b91c4-157">Caractères autorisés dans le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b91c4-157">Allowed characters in the username.</span></span> | <span data-ttu-id="b91c4-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="b91c4-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="b91c4-159">Paramètres de cookies pour l’application</span><span class="sxs-lookup"><span data-stu-id="b91c4-159">Application's cookie settings</span></span>

<span data-ttu-id="b91c4-160">Comme la stratégie de mots de passe, tous les paramètres du cookie de l’application peuvent être modifiés dans la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="b91c4-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b91c4-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b91c4-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b91c4-162">Sous `ConfigureServices` dans le `Startup` (classe), vous pouvez configurer le cookie de l’application.</span><span class="sxs-lookup"><span data-stu-id="b91c4-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b91c4-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b91c4-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="b91c4-164">`CookieAuthenticationOptions`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="b91c4-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="b91c4-165">Propriété</span><span class="sxs-lookup"><span data-stu-id="b91c4-165">Property</span></span>                | <span data-ttu-id="b91c4-166">Description</span><span class="sxs-lookup"><span data-stu-id="b91c4-166">Description</span></span>                       | <span data-ttu-id="b91c4-167">Par défaut</span><span class="sxs-lookup"><span data-stu-id="b91c4-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="b91c4-168">Le nom du cookie.</span><span class="sxs-lookup"><span data-stu-id="b91c4-168">The name of the cookie.</span></span>  | <span data-ttu-id="b91c4-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="b91c4-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="b91c4-170">Lorsque la valeur est true, le cookie n’est pas accessible à partir de scripts côté client.</span><span class="sxs-lookup"><span data-stu-id="b91c4-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="b91c4-171">true</span><span class="sxs-lookup"><span data-stu-id="b91c4-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="b91c4-172">Détermine combien de temps le ticket d’authentification stocké dans le cookie reste valide à partir du point de que sa création.</span><span class="sxs-lookup"><span data-stu-id="b91c4-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="b91c4-173">14 jours</span><span class="sxs-lookup"><span data-stu-id="b91c4-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="b91c4-174">Lorsqu’un utilisateur n’est pas autorisé, il seront redirigé à ce chemin d’accès à la connexion.</span><span class="sxs-lookup"><span data-stu-id="b91c4-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="b91c4-175">/ / Connexion au compte</span><span class="sxs-lookup"><span data-stu-id="b91c4-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="b91c4-176">Lorsqu’un utilisateur est déconnecté, il seront redirigé vers ce chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="b91c4-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="b91c4-177">/ Compte/déconnexion</span><span class="sxs-lookup"><span data-stu-id="b91c4-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="b91c4-178">Lorsqu’un utilisateur échoue une vérification d’autorisation, il seront redirigé vers ce chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="b91c4-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="b91c4-179">Lorsque la valeur est true, un nouveau cookie est émis avec une nouvelle heure d’expiration lorsque le cookie actuel est plus de milieu de la fenêtre d’expiration.</span><span class="sxs-lookup"><span data-stu-id="b91c4-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="b91c4-180">/ Compte/accès refusé</span><span class="sxs-lookup"><span data-stu-id="b91c4-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="b91c4-181">Détermine le nom du paramètre de chaîne de requête qui est ajouté par l’intergiciel (middleware) quand un code d’état non autorisé 401 est remplacée par une redirection 302 sur le chemin d’accès de connexion.</span><span class="sxs-lookup"><span data-stu-id="b91c4-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="b91c4-182">true</span><span class="sxs-lookup"><span data-stu-id="b91c4-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="b91c4-183">Cela concerne uniquement les ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b91c4-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="b91c4-184">Le nom logique pour un schéma d’authentification particulier.</span><span class="sxs-lookup"><span data-stu-id="b91c4-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="b91c4-185">Cet indicateur est uniquement pertinent pour ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b91c4-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="b91c4-186">Lorsque la valeur est true, l’authentification par cookie doit s’exécuter sur chaque demande et tenter de valider et de reconstruire toute entité sérialisée de que sa création.</span><span class="sxs-lookup"><span data-stu-id="b91c4-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |