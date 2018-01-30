---
title: "Configurer l’identité de ASP.NET Core"
author: AdrienTorris
description: "Comprendre les valeurs par défaut de ASP.NET Core Identity et configurer les propriétés d’identité différents pour utiliser des valeurs personnalisées."
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a><span data-ttu-id="1f9d7-103">Configurer l’identité</span><span class="sxs-lookup"><span data-stu-id="1f9d7-103">Configure Identity</span></span>

<span data-ttu-id="1f9d7-104">ASP.NET Core Identity a des comportements courants dans les applications telles que la stratégie de mot de passe, l’heure de verrouillage et les paramètres de cookies que vous pouvez remplacer facilement dans votre application `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="1f9d7-105">Stratégie de mots de passe</span><span class="sxs-lookup"><span data-stu-id="1f9d7-105">Passwords policy</span></span>

<span data-ttu-id="1f9d7-106">Par défaut, identité requiert que les mots de passe contient un caractère majuscule, minuscule, un chiffre et un caractère non alphanumérique.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="1f9d7-107">Il existe également quelques autres restrictions.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-107">There are also some other restrictions.</span></span> <span data-ttu-id="1f9d7-108">Pour simplifier les restrictions de mot de passe, vous devez modifier le `ConfigureServices` méthode de la `Startup` classe de votre application.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1f9d7-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1f9d7-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1f9d7-110">Base d’ASP.NET 2.0 a ajouté le `RequiredUniqueChars` propriété.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="1f9d7-111">Dans le cas contraire, les options sont les mêmes à partir de ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1f9d7-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1f9d7-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="1f9d7-113">`IdentityOptions.Password`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="1f9d7-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="1f9d7-114">Propriété</span><span class="sxs-lookup"><span data-stu-id="1f9d7-114">Property</span></span>                | <span data-ttu-id="1f9d7-115">Description</span><span class="sxs-lookup"><span data-stu-id="1f9d7-115">Description</span></span>                       | <span data-ttu-id="1f9d7-116">Par défaut</span><span class="sxs-lookup"><span data-stu-id="1f9d7-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="1f9d7-117">Nécessite un nombre compris entre 0 et 9 dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="1f9d7-118">true</span><span class="sxs-lookup"><span data-stu-id="1f9d7-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="1f9d7-119">La longueur minimale du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-119">The minimum length of the password.</span></span> | <span data-ttu-id="1f9d7-120">6</span><span class="sxs-lookup"><span data-stu-id="1f9d7-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="1f9d7-121">Nécessite un caractère non alphanumérique dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="1f9d7-122">true</span><span class="sxs-lookup"><span data-stu-id="1f9d7-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="1f9d7-123">Nécessite une lettre majuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="1f9d7-124">true</span><span class="sxs-lookup"><span data-stu-id="1f9d7-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="1f9d7-125">Nécessite un caractère minuscule dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="1f9d7-126">true</span><span class="sxs-lookup"><span data-stu-id="1f9d7-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="1f9d7-127">Requiert un nombre de caractères distincts dans le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="1f9d7-128">1</span><span class="sxs-lookup"><span data-stu-id="1f9d7-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="1f9d7-129">Verrouillage de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="1f9d7-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="1f9d7-130">`IdentityOptions.Lockout`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="1f9d7-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="1f9d7-131">Propriété</span><span class="sxs-lookup"><span data-stu-id="1f9d7-131">Property</span></span>                | <span data-ttu-id="1f9d7-132">Description</span><span class="sxs-lookup"><span data-stu-id="1f9d7-132">Description</span></span>                       | <span data-ttu-id="1f9d7-133">Par défaut</span><span class="sxs-lookup"><span data-stu-id="1f9d7-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="1f9d7-134">La durée pendant laquelle un utilisateur est verrouillé lorsqu’un verrouillage se produit.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="1f9d7-135">5 minutes</span><span class="sxs-lookup"><span data-stu-id="1f9d7-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="1f9d7-136">Le nombre de tentatives d’accès ayant échoué jusqu'à ce qu’un utilisateur est verrouillé, si le verrouillage est activé.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="1f9d7-137">5</span><span class="sxs-lookup"><span data-stu-id="1f9d7-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="1f9d7-138">Détermine si un utilisateur peut être verrouillé.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="1f9d7-139">true</span><span class="sxs-lookup"><span data-stu-id="1f9d7-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="1f9d7-140">Paramètres de connexion</span><span class="sxs-lookup"><span data-stu-id="1f9d7-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="1f9d7-141">`IdentityOptions.SignIn`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="1f9d7-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="1f9d7-142">Propriété</span><span class="sxs-lookup"><span data-stu-id="1f9d7-142">Property</span></span>                | <span data-ttu-id="1f9d7-143">Description</span><span class="sxs-lookup"><span data-stu-id="1f9d7-143">Description</span></span>                       | <span data-ttu-id="1f9d7-144">Par défaut</span><span class="sxs-lookup"><span data-stu-id="1f9d7-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="1f9d7-145">Nécessite un message électronique de confirmation pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="1f9d7-146">False</span><span class="sxs-lookup"><span data-stu-id="1f9d7-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="1f9d7-147">Nécessite un numéro de téléphone confirmé pour vous connecter.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="1f9d7-148">False</span><span class="sxs-lookup"><span data-stu-id="1f9d7-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="1f9d7-149">Paramètres de validation d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="1f9d7-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="1f9d7-150">`IdentityOptions.User`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="1f9d7-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="1f9d7-151">Propriété</span><span class="sxs-lookup"><span data-stu-id="1f9d7-151">Property</span></span>                | <span data-ttu-id="1f9d7-152">Description</span><span class="sxs-lookup"><span data-stu-id="1f9d7-152">Description</span></span>                       | <span data-ttu-id="1f9d7-153">Par défaut</span><span class="sxs-lookup"><span data-stu-id="1f9d7-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="1f9d7-154">Nécessite que chaque utilisateur à un message électronique unique.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="1f9d7-155">False</span><span class="sxs-lookup"><span data-stu-id="1f9d7-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="1f9d7-156">Caractères autorisés dans le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-156">Allowed characters in the username.</span></span> | <span data-ttu-id="1f9d7-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="1f9d7-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="1f9d7-158">Paramètres de cookies pour l’application</span><span class="sxs-lookup"><span data-stu-id="1f9d7-158">Application's cookie settings</span></span>

<span data-ttu-id="1f9d7-159">Comme la stratégie de mots de passe, tous les paramètres du cookie de l’application peuvent être modifiés dans la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1f9d7-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1f9d7-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1f9d7-161">Sous `ConfigureServices` dans le `Startup` (classe), vous pouvez configurer le cookie de l’application.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1f9d7-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1f9d7-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="1f9d7-163">`CookieAuthenticationOptions`a les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="1f9d7-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="1f9d7-164">Propriété</span><span class="sxs-lookup"><span data-stu-id="1f9d7-164">Property</span></span>                | <span data-ttu-id="1f9d7-165">Description</span><span class="sxs-lookup"><span data-stu-id="1f9d7-165">Description</span></span>                       | <span data-ttu-id="1f9d7-166">Par défaut</span><span class="sxs-lookup"><span data-stu-id="1f9d7-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="1f9d7-167">Le nom du cookie.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-167">The name of the cookie.</span></span>  | <span data-ttu-id="1f9d7-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="1f9d7-169">Lorsque la valeur est true, le cookie n’est pas accessible à partir de scripts côté client.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="1f9d7-170">true</span><span class="sxs-lookup"><span data-stu-id="1f9d7-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="1f9d7-171">Détermine combien de temps le ticket d’authentification stocké dans le cookie reste valide à partir du point de que sa création.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="1f9d7-172">14 jours</span><span class="sxs-lookup"><span data-stu-id="1f9d7-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="1f9d7-173">Lorsqu’un utilisateur n’est pas autorisé, il seront redirigé à ce chemin d’accès à la connexion.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="1f9d7-174">/ / Connexion au compte</span><span class="sxs-lookup"><span data-stu-id="1f9d7-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="1f9d7-175">Lorsqu’un utilisateur est déconnecté, il seront redirigé vers ce chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="1f9d7-176">/ Compte/déconnexion</span><span class="sxs-lookup"><span data-stu-id="1f9d7-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="1f9d7-177">Lorsqu’un utilisateur échoue une vérification d’autorisation, il seront redirigé vers ce chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="1f9d7-178">Lorsque la valeur est true, un nouveau cookie est émis avec une nouvelle heure d’expiration lorsque le cookie actuel est plus de milieu de la fenêtre d’expiration.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="1f9d7-179">/ Compte/accès refusé</span><span class="sxs-lookup"><span data-stu-id="1f9d7-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="1f9d7-180">Détermine le nom du paramètre de chaîne de requête qui est ajouté par l’intergiciel (middleware) quand un code d’état non autorisé 401 est remplacée par une redirection 302 sur le chemin d’accès de connexion.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="1f9d7-181">true</span><span class="sxs-lookup"><span data-stu-id="1f9d7-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="1f9d7-182">Cela concerne uniquement les ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="1f9d7-183">Le nom logique pour un schéma d’authentification particulier.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="1f9d7-184">Cet indicateur est uniquement pertinent pour ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="1f9d7-185">Lorsque la valeur est true, l’authentification par cookie doit s’exécuter sur chaque demande et tenter de valider et de reconstruire toute entité sérialisée de que sa création.</span><span class="sxs-lookup"><span data-stu-id="1f9d7-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
