---
uid: web-api/overview/security/basic-authentication
title: L’authentification de base dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Décrit l’utilisation de l’authentification de base dans l’API Web ASP.NET.
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 035baec7c56c0bf6eaacd26ea5192faf2ed6e932
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829585"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="f5adf-103">Authentification de base dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f5adf-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f5adf-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f5adf-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f5adf-105">L’authentification de base est définie dans [RFC 2617, l’authentification HTTP : authentification de base et Digest accès](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="f5adf-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="f5adf-106">Inconvénients</span><span class="sxs-lookup"><span data-stu-id="f5adf-106">Disadvantages</span></span>

- <span data-ttu-id="f5adf-107">Informations d’identification de l’utilisateur sont envoyées dans la demande.</span><span class="sxs-lookup"><span data-stu-id="f5adf-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="f5adf-108">Informations d’identification sont envoyées en texte brut.</span><span class="sxs-lookup"><span data-stu-id="f5adf-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="f5adf-109">Informations d’identification sont envoyées avec chaque requête.</span><span class="sxs-lookup"><span data-stu-id="f5adf-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="f5adf-110">Aucun moyen de se déconnecter, sauf en mettant fin à la session de navigateur.</span><span class="sxs-lookup"><span data-stu-id="f5adf-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="f5adf-111">Vulnérable aux intersites falsification de requête (CSRF) ; nécessite l’anti-CSRF de mesures.</span><span class="sxs-lookup"><span data-stu-id="f5adf-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="f5adf-112">Avantages</span><span class="sxs-lookup"><span data-stu-id="f5adf-112">Advantages</span></span>

- <span data-ttu-id="f5adf-113">Norme Internet.</span><span class="sxs-lookup"><span data-stu-id="f5adf-113">Internet standard.</span></span>
- <span data-ttu-id="f5adf-114">Prise en charge par tous les principaux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="f5adf-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="f5adf-115">Protocole relativement simple.</span><span class="sxs-lookup"><span data-stu-id="f5adf-115">Relatively simple protocol.</span></span>

<span data-ttu-id="f5adf-116">L’authentification de base fonctionne comme suit :</span><span class="sxs-lookup"><span data-stu-id="f5adf-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="f5adf-117">Si une demande nécessite une authentification, le serveur renvoie 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="f5adf-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="f5adf-118">La réponse inclut un en-tête WWW-Authenticate, indiquant que le serveur prend en charge l’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="f5adf-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="f5adf-119">Le client envoie une autre requête, avec les informations d’identification de client dans l’en-tête d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="f5adf-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="f5adf-120">Les informations d’identification sont mis en forme en tant que la chaîne « nom : mot de passe », codé en base64.</span><span class="sxs-lookup"><span data-stu-id="f5adf-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="f5adf-121">Les informations d’identification ne sont pas chiffrées.</span><span class="sxs-lookup"><span data-stu-id="f5adf-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="f5adf-122">L’authentification de base est effectuée dans le contexte d’un « domaine ».</span><span class="sxs-lookup"><span data-stu-id="f5adf-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="f5adf-123">Le serveur inclut le nom du domaine dans l’en-tête WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="f5adf-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="f5adf-124">Les informations d’identification sont valides dans ce domaine.</span><span class="sxs-lookup"><span data-stu-id="f5adf-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="f5adf-125">L’étendue exacte d’un domaine est définie par le serveur.</span><span class="sxs-lookup"><span data-stu-id="f5adf-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="f5adf-126">Par exemple, vous pouvez définir plusieurs domaines dans l’ordre aux ressources de la partition.</span><span class="sxs-lookup"><span data-stu-id="f5adf-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="f5adf-127">Étant donné que les informations d’identification sont envoyées sans être chiffrées, l’authentification de base n’est sécurisée via le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f5adf-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="f5adf-128">Consultez [utilisation de SSL dans l’API Web](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f5adf-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="f5adf-129">L’authentification de base est également vulnérable aux attaques de falsification de requête Intersites.</span><span class="sxs-lookup"><span data-stu-id="f5adf-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="f5adf-130">Une fois que l’utilisateur entre des informations d’identification, le navigateur envoie automatiquement les sur les demandes suivantes au même domaine, pendant la durée de la session.</span><span class="sxs-lookup"><span data-stu-id="f5adf-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="f5adf-131">Cela inclut les demandes AJAX.</span><span class="sxs-lookup"><span data-stu-id="f5adf-131">This includes AJAX requests.</span></span> <span data-ttu-id="f5adf-132">Consultez [empêcher les falsifications de requête intersites (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="f5adf-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="f5adf-133">Authentification de base avec IIS</span><span class="sxs-lookup"><span data-stu-id="f5adf-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="f5adf-134">IIS prend en charge l’authentification de base, mais il existe un inconvénient : l’utilisateur est authentifié par rapport à leurs informations d’identification Windows.</span><span class="sxs-lookup"><span data-stu-id="f5adf-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="f5adf-135">Cela signifie que l’utilisateur doit avoir un compte sur le domaine du serveur.</span><span class="sxs-lookup"><span data-stu-id="f5adf-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="f5adf-136">Pour un site web orienté public, vous souhaitez généralement s’authentifier auprès d’un fournisseur d’appartenances ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f5adf-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="f5adf-137">Pour activer l’authentification de base à l’aide d’IIS, la valeur est le mode d’authentification « Windows » dans le fichier Web.config de votre projet ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="f5adf-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="f5adf-138">Dans ce mode, IIS utilise les informations d’identification Windows pour authentifier.</span><span class="sxs-lookup"><span data-stu-id="f5adf-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="f5adf-139">En outre, vous devez activer l’authentification de base dans IIS.</span><span class="sxs-lookup"><span data-stu-id="f5adf-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="f5adf-140">Dans le Gestionnaire des services Internet, accédez à l’affichage des fonctionnalités d’authentification et sélectionnez Activer l’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="f5adf-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="f5adf-141">Dans votre projet d’API Web, ajoutez le `[Authorize]` attribut pour toutes les actions de contrôleur qui s’authentifient.</span><span class="sxs-lookup"><span data-stu-id="f5adf-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="f5adf-142">Un client s’authentifie en définissant l’en-tête d’autorisation dans la demande.</span><span class="sxs-lookup"><span data-stu-id="f5adf-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="f5adf-143">Les clients de navigateur effectuent cette étape automatiquement.</span><span class="sxs-lookup"><span data-stu-id="f5adf-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="f5adf-144">Les clients nonbrowser devez définir l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="f5adf-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="f5adf-145">Authentification de base avec appartenance personnalisée</span><span class="sxs-lookup"><span data-stu-id="f5adf-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="f5adf-146">Comme mentionné, l’authentification de base intégrée à IIS utilise les informations d’identification Windows.</span><span class="sxs-lookup"><span data-stu-id="f5adf-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="f5adf-147">Cela signifie que vous avez besoin créer des comptes pour vos utilisateurs sur le serveur d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="f5adf-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="f5adf-148">Mais pour une application internet, des comptes d’utilisateur sont généralement stockées dans une base de données externe.</span><span class="sxs-lookup"><span data-stu-id="f5adf-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="f5adf-149">Le code suivant comment un module HTTP qui effectue l’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="f5adf-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="f5adf-150">Vous pouvez facilement incorporer un fournisseur d’appartenances ASP.NET en remplaçant le `CheckPassword` (méthode), qui est une méthode factice dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="f5adf-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="f5adf-151">Dans Web API 2, vous devez envisager d’écrire un [filtre d’authentification](authentication-filters.md) ou [intergiciel (middleware) OWIN](../../../aspnet/overview/owin-and-katana/index.md), au lieu d’un module HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5adf-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="f5adf-152">Pour activer le module HTTP, ajoutez le code suivant à votre fichier web.config dans le **system.webServer** section :</span><span class="sxs-lookup"><span data-stu-id="f5adf-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="f5adf-153">Remplacez « Nom_votre_assembly » par le nom de l’assembly (sans l’extension « dll »).</span><span class="sxs-lookup"><span data-stu-id="f5adf-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="f5adf-154">Vous devez désactiver les autres schémas d’authentification, tels que de l’authentification de formulaires ou Windows.</span><span class="sxs-lookup"><span data-stu-id="f5adf-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
