---
title: Attaques empêcher Cross-Site Request Forgery (XSRF/CSRF) dans ASP.NET Core
author: steve-smith
description: Découvrez comment éviter les attaques contre les applications web où un site Web malveillant peut influencer l’interaction entre un navigateur client et l’application.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: a32e0e2dbd7fab95562a562cb88767d4c1e8049d
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458485"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="0399f-103">Attaques empêcher Cross-Site Request Forgery (XSRF/CSRF) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0399f-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="0399f-104">Par [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0399f-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0399f-105">Falsification de requête intersites (également appelé XSRF ou CSRF) est une attaque contre les applications web hébergées par laquelle une application web malveillant peut influencer l’interaction entre un navigateur client et une application web qui fait confiance à ce navigateur.</span><span class="sxs-lookup"><span data-stu-id="0399f-105">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="0399f-106">Ces attaques sont possibles, car les navigateurs web envoient certains types de jetons d’authentification automatiquement avec chaque demande à un site Web.</span><span class="sxs-lookup"><span data-stu-id="0399f-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="0399f-107">Cette forme de code malveillant est également appelé un *clic attaque* ou *par vol de session* , car l’attaque tire parti de l’utilisateur d’authentifié précédemment session.</span><span class="sxs-lookup"><span data-stu-id="0399f-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="0399f-108">Un exemple d’une attaque CSRF :</span><span class="sxs-lookup"><span data-stu-id="0399f-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="0399f-109">Un utilisateur se connecte à `www.good-banking-site.com` à l’aide de l’authentification par formulaire.</span><span class="sxs-lookup"><span data-stu-id="0399f-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="0399f-110">Le serveur authentifie l’utilisateur et émet une réponse qui inclut un cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="0399f-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="0399f-111">Le site est vulnérable aux attaques, car il fait confiance à toute demande qu’il reçoit avec un cookie d’authentification valide.</span><span class="sxs-lookup"><span data-stu-id="0399f-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="0399f-112">L’utilisateur visite un site malveillant, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="0399f-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="0399f-113">Le site malveillant, `www.bad-crook-site.com`, contient un formulaire HTML similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="0399f-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="0399f-114">Notez que le formulaire `action` publications sur le site vulnérable, et non vers le site malveillant.</span><span class="sxs-lookup"><span data-stu-id="0399f-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="0399f-115">Il s’agit de la partie « cross-site » de falsification de requête Intersites.</span><span class="sxs-lookup"><span data-stu-id="0399f-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="0399f-116">L’utilisateur sélectionne le bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="0399f-116">The user selects the submit button.</span></span> <span data-ttu-id="0399f-117">Le navigateur effectue la demande et inclut automatiquement le cookie d’authentification pour le domaine demandé, `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="0399f-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="0399f-118">La requête s’exécute sur le `www.good-banking-site.com` serveur avec un contexte de l’authentification de l’utilisateur et peut exécuter toute action pour laquelle un utilisateur authentifié est autorisé à effectuer.</span><span class="sxs-lookup"><span data-stu-id="0399f-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="0399f-119">Outre le scénario où l’utilisateur sélectionne le bouton Envoyer le formulaire, le site malveillant pourrait :</span><span class="sxs-lookup"><span data-stu-id="0399f-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="0399f-120">Exécuter un script qui envoie automatiquement le formulaire.</span><span class="sxs-lookup"><span data-stu-id="0399f-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="0399f-121">Envoyer l’envoi du formulaire sous la forme d’une requête AJAX.</span><span class="sxs-lookup"><span data-stu-id="0399f-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="0399f-122">Masquer le formulaire à l’aide de CSS.</span><span class="sxs-lookup"><span data-stu-id="0399f-122">Hide the form using CSS.</span></span>

<span data-ttu-id="0399f-123">Ces scénarios alternatifs ne nécessitent aucune action ou une entrée de l’utilisateur autre qu’initialement visitant le site malveillant.</span><span class="sxs-lookup"><span data-stu-id="0399f-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="0399f-124">À l’aide de HTTPS n’empêche pas une attaque CSRF.</span><span class="sxs-lookup"><span data-stu-id="0399f-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="0399f-125">Le site malveillant peut envoyer un `https://www.good-banking-site.com/` demander tout aussi facilement qu’il peut envoyer une requête non sécurisée.</span><span class="sxs-lookup"><span data-stu-id="0399f-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="0399f-126">Certaines attaques ciblent les points de terminaison qui répondent aux demandes GET, auquel cas une balise d’image peut être utilisée pour effectuer l’action.</span><span class="sxs-lookup"><span data-stu-id="0399f-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="0399f-127">Ce type d’attaque est courant sur les sites de forum qui permettent des images mais bloquent JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0399f-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="0399f-128">Les applications qui modifient l’état sur les demandes GET, où les variables ou les ressources sont modifiés, sont vulnérables aux attaques malveillantes.</span><span class="sxs-lookup"><span data-stu-id="0399f-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="0399f-129">**Les requêtes GET qui changent d’état sont non sécurisés. Une bonne pratique consiste à ne jamais changer l’état sur une requête GET.**</span><span class="sxs-lookup"><span data-stu-id="0399f-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="0399f-130">Les attaques CSRF sont possibles pour les applications web qui utilisent des cookies pour l’authentification, car :</span><span class="sxs-lookup"><span data-stu-id="0399f-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="0399f-131">Navigateurs stockent des cookies émis par une application web.</span><span class="sxs-lookup"><span data-stu-id="0399f-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="0399f-132">Les cookies stockés incluent les cookies de session pour les utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="0399f-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="0399f-133">Les navigateurs envoient que tous les cookies associés à un domaine à l’application web chaque demande, quel que soit la façon dont la demande d’application a été générée dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="0399f-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="0399f-134">Toutefois, les attaques CSRF ne sont pas limitées à exploiter les cookies.</span><span class="sxs-lookup"><span data-stu-id="0399f-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="0399f-135">Par exemple, l’authentification de base et Digest sont également vulnérables.</span><span class="sxs-lookup"><span data-stu-id="0399f-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="0399f-136">Une fois un utilisateur se connecte avec l’authentification de base ou Digest, le navigateur envoie automatiquement les informations d’identification jusqu'à ce que la session&dagger; se termine.</span><span class="sxs-lookup"><span data-stu-id="0399f-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="0399f-137">&dagger;Dans ce contexte, *session* fait référence à la session côté client au cours de laquelle l’utilisateur est authentifié.</span><span class="sxs-lookup"><span data-stu-id="0399f-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="0399f-138">Il est sans rapport avec les sessions côté serveur ou [Middleware de Session ASP.NET Core](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="0399f-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="0399f-139">Les utilisateurs peuvent se prémunir contre les vulnérabilités CSRF en prenant les précautions :</span><span class="sxs-lookup"><span data-stu-id="0399f-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="0399f-140">Signature de l’applications web une fois que leur utilisation.</span><span class="sxs-lookup"><span data-stu-id="0399f-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="0399f-141">Cookies du navigateur effacer périodiquement.</span><span class="sxs-lookup"><span data-stu-id="0399f-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="0399f-142">Toutefois, des vulnérabilités CSRF sont fondamentalement un problème avec l’application web, pas l’utilisateur final.</span><span class="sxs-lookup"><span data-stu-id="0399f-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="0399f-143">Principes de base de l’authentification</span><span class="sxs-lookup"><span data-stu-id="0399f-143">Authentication fundamentals</span></span>

<span data-ttu-id="0399f-144">L’authentification basée sur le cookie est un formulaire populaires d’authentification.</span><span class="sxs-lookup"><span data-stu-id="0399f-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="0399f-145">Systèmes d’authentification par jeton gagnent en popularité, en particulier pour les Applications à Page unique (SPA).</span><span class="sxs-lookup"><span data-stu-id="0399f-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="0399f-146">Authentification basée sur les cookies</span><span class="sxs-lookup"><span data-stu-id="0399f-146">Cookie-based authentication</span></span>

<span data-ttu-id="0399f-147">Lorsqu’un utilisateur s’authentifie à l’aide de leur nom d’utilisateur et le mot de passe, ils sont émis un jeton contenant un ticket d’authentification qui peut être utilisé pour l’authentification et l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="0399f-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="0399f-148">Le jeton est stocké en tant que permet un cookie qui accompagne chaque demande du client.</span><span class="sxs-lookup"><span data-stu-id="0399f-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="0399f-149">Génération et la validation de ce cookie sont effectuée par le Middleware Cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="0399f-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="0399f-150">Le [intergiciel (middleware)](xref:fundamentals/middleware/index) sérialise un principal d’utilisateur dans un cookie chiffré.</span><span class="sxs-lookup"><span data-stu-id="0399f-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="0399f-151">Sur les demandes suivantes, le middleware valide le cookie recrée le principal et attribue au principal de la [utilisateur](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) propriété du [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="0399f-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="0399f-152">Authentification par jeton</span><span class="sxs-lookup"><span data-stu-id="0399f-152">Token-based authentication</span></span>

<span data-ttu-id="0399f-153">Lorsqu’un utilisateur est authentifié, ils sont émis un jeton (pas un jeton anti-contrefaçon).</span><span class="sxs-lookup"><span data-stu-id="0399f-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="0399f-154">Le jeton contient les informations de l’utilisateur sous la forme de [revendications](/dotnet/framework/security/claims-based-identity-model) ou un jeton de référence qui pointe l’application à l’état utilisateur mis à jour dans l’application.</span><span class="sxs-lookup"><span data-stu-id="0399f-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="0399f-155">Lorsqu’un utilisateur tente d’accéder à une ressource nécessitant une authentification, le jeton est envoyé à l’application avec un en-tête d’autorisation supplémentaires sous forme de jeton du porteur.</span><span class="sxs-lookup"><span data-stu-id="0399f-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="0399f-156">Cela rend l’application sans état.</span><span class="sxs-lookup"><span data-stu-id="0399f-156">This makes the app stateless.</span></span> <span data-ttu-id="0399f-157">Dans chaque demande ultérieure, le jeton est passé dans la demande pour la validation côté serveur.</span><span class="sxs-lookup"><span data-stu-id="0399f-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="0399f-158">Ce jeton n’est pas *chiffrées*; il a *encodé*.</span><span class="sxs-lookup"><span data-stu-id="0399f-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="0399f-159">Sur le serveur, le jeton est décodé pour accéder à ses informations.</span><span class="sxs-lookup"><span data-stu-id="0399f-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="0399f-160">Pour envoyer le jeton sur les demandes suivantes, stocker le jeton dans le stockage local du navigateur.</span><span class="sxs-lookup"><span data-stu-id="0399f-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="0399f-161">Ne soyez pas inquiet à une vulnérabilité CSRF si le jeton est stocké dans le stockage local du navigateur.</span><span class="sxs-lookup"><span data-stu-id="0399f-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="0399f-162">CSRF est un problème lorsque le jeton est stocké dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="0399f-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="0399f-163">Plusieurs applications hébergées sur un domaine</span><span class="sxs-lookup"><span data-stu-id="0399f-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="0399f-164">Environnements d’hébergement partagés sont vulnérables au piratage de session connexion CSRF et autres attaques.</span><span class="sxs-lookup"><span data-stu-id="0399f-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="0399f-165">Bien que `example1.contoso.net` et `example2.contoso.net` sont des hôtes différents, il existe une relation de confiance implicite entre les hôtes sous le `*.contoso.net` domaine.</span><span class="sxs-lookup"><span data-stu-id="0399f-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="0399f-166">Cette relation de confiance implicite permet des hôtes potentiellement non fiables affecter des cookies entre eux (les stratégies de même origine qui régissent les demandes AJAX ne s’appliquent obligatoirement aux cookies HTTP).</span><span class="sxs-lookup"><span data-stu-id="0399f-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="0399f-167">Pour empêcher les attaques qui exploitent les cookies de confiance entre les applications hébergées sur le même domaine, ne pas partager des domaines.</span><span class="sxs-lookup"><span data-stu-id="0399f-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="0399f-168">Lorsque chaque application est hébergée sur son propre domaine, il n’existe aucune relation d’approbation implicite de cookie à exploiter.</span><span class="sxs-lookup"><span data-stu-id="0399f-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="0399f-169">Configuration anti-contrefaçon ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0399f-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="0399f-170">ASP.NET Core implémente à l’aide d’anti-contrefaçon [Protection des données ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="0399f-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="0399f-171">La pile de protection des données doit être configurée pour fonctionner dans une batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="0399f-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="0399f-172">Consultez [configuration de la protection des données](xref:security/data-protection/configuration/overview) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="0399f-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="0399f-173">Dans ASP.NET Core 2.0 ou version ultérieure, le [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injecte des jetons anti-contrefaçon dans les éléments de formulaire HTML.</span><span class="sxs-lookup"><span data-stu-id="0399f-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="0399f-174">Le balisage suivant dans un fichier Razor génère automatiquement des jetons anti-contrefaçon :</span><span class="sxs-lookup"><span data-stu-id="0399f-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="0399f-175">De même, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) génère des jetons anti-contrefaçon par défaut si la méthode du formulaire n’est pas GET.</span><span class="sxs-lookup"><span data-stu-id="0399f-175">Similarly, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="0399f-176">La génération automatique des jetons anti-contrefaçon pour les éléments de formulaire HTML se produit lorsque le `<form>` balise contient le `method="post"` attribut et une des opérations suivantes sont remplie :</span><span class="sxs-lookup"><span data-stu-id="0399f-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

* <span data-ttu-id="0399f-177">L’attribut action est vide (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="0399f-177">The action attribute is empty (`action=""`).</span></span>
* <span data-ttu-id="0399f-178">L’attribut d’action n’est pas fourni (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="0399f-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="0399f-179">Vous pouvez désactiver la génération automatique des jetons anti-contrefaçon pour les éléments de formulaire HTML :</span><span class="sxs-lookup"><span data-stu-id="0399f-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="0399f-180">Désactiver explicitement des jetons anti-contrefaçon avec le `asp-antiforgery` attribut :</span><span class="sxs-lookup"><span data-stu-id="0399f-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="0399f-181">L’élément du formulaire est choisi par de Tag Helpers à l’aide du programme d’assistance de balise [! annulations symbole](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="0399f-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="0399f-182">Supprimer le `FormTagHelper` à partir de la vue.</span><span class="sxs-lookup"><span data-stu-id="0399f-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="0399f-183">Le `FormTagHelper` peut être supprimé à partir d’une vue en ajoutant la directive suivante à la vue Razor :</span><span class="sxs-lookup"><span data-stu-id="0399f-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="0399f-184">[Pages Razor](xref:razor-pages/index) sont protégés automatiquement contre XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="0399f-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="0399f-185">Pour plus d’informations, consultez [XSRF/CSRF et Pages Razor](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="0399f-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="0399f-186">L’approche la plus courante pour se défendre contre les attaques CSRF consiste à utiliser le *modèle de jeton synchronisateur* (STP).</span><span class="sxs-lookup"><span data-stu-id="0399f-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="0399f-187">STP est utilisé lorsque l’utilisateur demande une page avec les données de formulaire :</span><span class="sxs-lookup"><span data-stu-id="0399f-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="0399f-188">Le serveur envoie un jeton associé avec l’identité de l’utilisateur actuel au client.</span><span class="sxs-lookup"><span data-stu-id="0399f-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="0399f-189">Le client envoie le jeton au serveur pour la vérification.</span><span class="sxs-lookup"><span data-stu-id="0399f-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="0399f-190">Si le serveur reçoit un jeton qui ne correspond pas à identité de l’utilisateur authentifié, la demande est rejetée.</span><span class="sxs-lookup"><span data-stu-id="0399f-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="0399f-191">Le jeton est unique et imprévisible.</span><span class="sxs-lookup"><span data-stu-id="0399f-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="0399f-192">Le jeton peut également être utilisé pour garantir un séquençage correct d’une série de demandes (par exemple, en garantissant la séquence de demande de : la page 1 &ndash; page 2 &ndash; page 3).</span><span class="sxs-lookup"><span data-stu-id="0399f-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="0399f-193">Tous les formulaires dans les modèles ASP.NET Core MVC et les Pages Razor génèrent des jetons anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="0399f-194">Les deux exemples de vue suivants génèrent des jetons anti-contrefaçon :</span><span class="sxs-lookup"><span data-stu-id="0399f-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="0399f-195">Ajouter explicitement un jeton anti-contrefaçon pour un `<form>` élément sans recourir à des Tag Helpers avec le programme d’assistance HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="0399f-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="0399f-196">Dans chacun des cas précédents, ASP.NET Core ajoute un champ de formulaire masqué similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="0399f-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="0399f-197">ASP.NET Core inclut trois [filtres](xref:mvc/controllers/filters) pour travailler avec des jetons anti-contrefaçon :</span><span class="sxs-lookup"><span data-stu-id="0399f-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="0399f-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="0399f-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="0399f-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="0399f-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="0399f-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="0399f-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="0399f-201">Options anti-contrefaçon</span><span class="sxs-lookup"><span data-stu-id="0399f-201">Antiforgery options</span></span>

<span data-ttu-id="0399f-202">Personnaliser [options anti-contrefaçon](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0399f-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

<span data-ttu-id="0399f-203">&dagger;Définir l’anti-contrefaçon `Cookie` propriétés à l’aide des propriétés de la [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) classe.</span><span class="sxs-lookup"><span data-stu-id="0399f-203">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="0399f-204">Option</span><span class="sxs-lookup"><span data-stu-id="0399f-204">Option</span></span> | <span data-ttu-id="0399f-205">Description</span><span class="sxs-lookup"><span data-stu-id="0399f-205">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="0399f-206">Cookie</span><span class="sxs-lookup"><span data-stu-id="0399f-206">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="0399f-207">Détermine les paramètres utilisés pour créer les cookies anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-207">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="0399f-208">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="0399f-208">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="0399f-209">Le nom du champ de formulaire masqué utilisé par le système anti-contrefaçon pour restituer des jetons anti-contrefaçon dans les vues.</span><span class="sxs-lookup"><span data-stu-id="0399f-209">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="0399f-210">HeaderName</span><span class="sxs-lookup"><span data-stu-id="0399f-210">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="0399f-211">Le nom de l’en-tête utilisé par le système anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-211">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="0399f-212">Si `null`, le système considère uniquement les données de formulaire.</span><span class="sxs-lookup"><span data-stu-id="0399f-212">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="0399f-213">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="0399f-213">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="0399f-214">Spécifie s’il faut supprimer la génération de la `X-Frame-Options` en-tête.</span><span class="sxs-lookup"><span data-stu-id="0399f-214">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="0399f-215">Par défaut, l’en-tête est généré avec la valeur « SAMEORIGIN ».</span><span class="sxs-lookup"><span data-stu-id="0399f-215">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="0399f-216">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="0399f-216">Defaults to `false`.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="0399f-217">Option</span><span class="sxs-lookup"><span data-stu-id="0399f-217">Option</span></span> | <span data-ttu-id="0399f-218">Description</span><span class="sxs-lookup"><span data-stu-id="0399f-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="0399f-219">Cookie</span><span class="sxs-lookup"><span data-stu-id="0399f-219">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="0399f-220">Détermine les paramètres utilisés pour créer les cookies anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-220">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="0399f-221">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="0399f-221">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="0399f-222">Le domaine du cookie.</span><span class="sxs-lookup"><span data-stu-id="0399f-222">The domain of the cookie.</span></span> <span data-ttu-id="0399f-223">La valeur par défaut est `null`.</span><span class="sxs-lookup"><span data-stu-id="0399f-223">Defaults to `null`.</span></span> <span data-ttu-id="0399f-224">Cette propriété est obsolète et sera supprimée dans une future version.</span><span class="sxs-lookup"><span data-stu-id="0399f-224">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="0399f-225">L’alternative recommandée est Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="0399f-225">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="0399f-226">CookieName</span><span class="sxs-lookup"><span data-stu-id="0399f-226">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="0399f-227">Le nom du cookie.</span><span class="sxs-lookup"><span data-stu-id="0399f-227">The name of the cookie.</span></span> <span data-ttu-id="0399f-228">Si ne pas définie, le système génère un nom unique commençant par le [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ( ». AspNetCore.Antiforgery. »).</span><span class="sxs-lookup"><span data-stu-id="0399f-228">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="0399f-229">Cette propriété est obsolète et sera supprimée dans une future version.</span><span class="sxs-lookup"><span data-stu-id="0399f-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="0399f-230">L’alternative recommandée est Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="0399f-230">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="0399f-231">CookiePath</span><span class="sxs-lookup"><span data-stu-id="0399f-231">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="0399f-232">Le chemin d’accès défini sur le cookie.</span><span class="sxs-lookup"><span data-stu-id="0399f-232">The path set on the cookie.</span></span> <span data-ttu-id="0399f-233">Cette propriété est obsolète et sera supprimée dans une future version.</span><span class="sxs-lookup"><span data-stu-id="0399f-233">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="0399f-234">L’alternative recommandée est Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="0399f-234">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="0399f-235">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="0399f-235">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="0399f-236">Le nom du champ de formulaire masqué utilisé par le système anti-contrefaçon pour restituer des jetons anti-contrefaçon dans les vues.</span><span class="sxs-lookup"><span data-stu-id="0399f-236">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="0399f-237">HeaderName</span><span class="sxs-lookup"><span data-stu-id="0399f-237">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="0399f-238">Le nom de l’en-tête utilisé par le système anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-238">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="0399f-239">Si `null`, le système considère uniquement les données de formulaire.</span><span class="sxs-lookup"><span data-stu-id="0399f-239">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="0399f-240">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="0399f-240">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="0399f-241">Spécifie si HTTPS est requis par le système anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-241">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="0399f-242">Si `true`, les requêtes non-HTTPS échouent.</span><span class="sxs-lookup"><span data-stu-id="0399f-242">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="0399f-243">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="0399f-243">Defaults to `false`.</span></span> <span data-ttu-id="0399f-244">Cette propriété est obsolète et sera supprimée dans une future version.</span><span class="sxs-lookup"><span data-stu-id="0399f-244">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="0399f-245">L’alternative recommandée consiste à définir les Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="0399f-245">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="0399f-246">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="0399f-246">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="0399f-247">Spécifie s’il faut supprimer la génération de la `X-Frame-Options` en-tête.</span><span class="sxs-lookup"><span data-stu-id="0399f-247">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="0399f-248">Par défaut, l’en-tête est généré avec la valeur « SAMEORIGIN ».</span><span class="sxs-lookup"><span data-stu-id="0399f-248">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="0399f-249">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="0399f-249">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="0399f-250">Pour plus d’informations, consultez [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="0399f-250">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="0399f-251">Configurer les fonctionnalités anti-contrefaçon avec IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="0399f-251">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="0399f-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fournit l’API pour configurer les fonctionnalités anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="0399f-253">`IAntiforgery` peut être demandé dans le `Configure` méthode de la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="0399f-253">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="0399f-254">L’exemple suivant utilise l’intergiciel (middleware) à partir de la page d’accueil de l’application pour générer un jeton anti-contrefaçon et l’envoyer dans la réponse en tant que cookie (à l’aide de la convention de nommage Angular par défaut est décrite plus loin dans cette rubrique) :</span><span class="sxs-lookup"><span data-stu-id="0399f-254">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="0399f-255">Exiger une validation anti-contrefaçon</span><span class="sxs-lookup"><span data-stu-id="0399f-255">Require antiforgery validation</span></span>

<span data-ttu-id="0399f-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) est un filtre d’action qui peut être appliqué à une action individuelle, un contrôleur, ou dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="0399f-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="0399f-257">Les demandes effectuées à des actions qui ont ce filtre est appliqué sont bloquées, sauf si la demande inclut un jeton anti-contrefaçon valide.</span><span class="sxs-lookup"><span data-stu-id="0399f-257">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="0399f-258">Le `ValidateAntiForgeryToken` attribut requiert un jeton pour les demandes aux méthodes d’action il décore, y compris les requêtes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0399f-258">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="0399f-259">Si le `ValidateAntiForgeryToken` attribut est appliqué sur les contrôleurs de l’application, il peut être remplacé par le `IgnoreAntiforgeryToken` attribut.</span><span class="sxs-lookup"><span data-stu-id="0399f-259">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="0399f-260">ASP.NET Core ne prend en charge l’ajout automatique des jetons anti-contrefaçon aux demandes GET.</span><span class="sxs-lookup"><span data-stu-id="0399f-260">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="0399f-261">Valider automatiquement les jetons anti-contrefaçon pour les méthodes HTTP non sécurisés</span><span class="sxs-lookup"><span data-stu-id="0399f-261">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="0399f-262">Les applications ASP.NET Core ne pas générer des jetons anti-contrefaçon pour les méthodes HTTP sécurisés (GET, HEAD, OPTIONS et TRACE).</span><span class="sxs-lookup"><span data-stu-id="0399f-262">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="0399f-263">Au lieu d’appliquer largement le `ValidateAntiForgeryToken` attribut, puis en remplaçant avec `IgnoreAntiforgeryToken` attributs, la [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribut peut être utilisé.</span><span class="sxs-lookup"><span data-stu-id="0399f-263">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="0399f-264">Cet attribut fonctionne de manière identique à la `ValidateAntiForgeryToken` d’attribut, à ceci près qu’il ne nécessite des jetons pour les demandes effectuées à l’aide de méthodes HTTP suivantes :</span><span class="sxs-lookup"><span data-stu-id="0399f-264">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="0399f-265">GET</span><span class="sxs-lookup"><span data-stu-id="0399f-265">GET</span></span>
* <span data-ttu-id="0399f-266">HEAD</span><span class="sxs-lookup"><span data-stu-id="0399f-266">HEAD</span></span>
* <span data-ttu-id="0399f-267">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="0399f-267">OPTIONS</span></span>
* <span data-ttu-id="0399f-268">TRACE</span><span class="sxs-lookup"><span data-stu-id="0399f-268">TRACE</span></span>

<span data-ttu-id="0399f-269">Nous recommandons l’utilisation de `AutoValidateAntiforgeryToken` largement pour les scénarios non-API.</span><span class="sxs-lookup"><span data-stu-id="0399f-269">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="0399f-270">Cela garantit que les actions POST sont protégées par défaut.</span><span class="sxs-lookup"><span data-stu-id="0399f-270">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="0399f-271">L’alternative consiste à ignorer les jetons anti-contrefaçon par défaut, sauf si `ValidateAntiForgeryToken` est appliqué aux méthodes d’action individuelles.</span><span class="sxs-lookup"><span data-stu-id="0399f-271">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="0399f-272">Il n’est plus probable que dans ce scénario pour une méthode d’action POST rester pas protégé par erreur, en laissant l’application vulnérable aux attaques par falsification de requête Intersites.</span><span class="sxs-lookup"><span data-stu-id="0399f-272">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="0399f-273">Toutes les publications doivent envoyer le jeton anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="0399f-273">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="0399f-274">API n’ont pas un mécanisme automatique pour l’envoi de la partie non cookie du jeton.</span><span class="sxs-lookup"><span data-stu-id="0399f-274">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="0399f-275">L’implémentation dépend probablement de l’implémentation du code client.</span><span class="sxs-lookup"><span data-stu-id="0399f-275">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="0399f-276">Certains exemples sont présentés ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="0399f-276">Some examples are shown below:</span></span>

<span data-ttu-id="0399f-277">Exemple de niveau classe :</span><span class="sxs-lookup"><span data-stu-id="0399f-277">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="0399f-278">Exemple global :</span><span class="sxs-lookup"><span data-stu-id="0399f-278">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="0399f-279">Remplacement global ou attributs anti-contrefaçon contrôleur</span><span class="sxs-lookup"><span data-stu-id="0399f-279">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="0399f-280">Le [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtre est utilisé pour éliminer le besoin d’un jeton anti-contrefaçon pour une action donnée (ou un contrôleur).</span><span class="sxs-lookup"><span data-stu-id="0399f-280">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="0399f-281">Lorsqu’il est appliqué, ce filtre remplace `ValidateAntiForgeryToken` et `AutoValidateAntiforgeryToken` filtres spécifiés à un niveau supérieur (globalement ou sur un contrôleur).</span><span class="sxs-lookup"><span data-stu-id="0399f-281">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="0399f-282">Une fois l’authentification des jetons d’actualisation</span><span class="sxs-lookup"><span data-stu-id="0399f-282">Refresh tokens after authentication</span></span>

<span data-ttu-id="0399f-283">Les jetons doivent être actualisées une fois que l’utilisateur est authentifié en redirigeant l’utilisateur à une vue ou une page Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="0399f-283">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="0399f-284">JavaScript, AJAX et les applications SPA</span><span class="sxs-lookup"><span data-stu-id="0399f-284">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="0399f-285">Dans les applications HTML traditionnelles, les jetons anti-contrefaçon sont passés au serveur à l’aide des champs de formulaire masqué.</span><span class="sxs-lookup"><span data-stu-id="0399f-285">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="0399f-286">Dans les applications modernes basées sur JavaScript et les applications SPA, nombre de demandes envoyées par programmation.</span><span class="sxs-lookup"><span data-stu-id="0399f-286">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="0399f-287">Ces demandes AJAX peuvent utiliser d’autres techniques (telles que les en-têtes de requête ou des cookies) pour envoyer le jeton.</span><span class="sxs-lookup"><span data-stu-id="0399f-287">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="0399f-288">Si les cookies sont utilisés pour stocker les jetons d’authentification et pour authentifier les demandes d’API sur le serveur, CSRF est un problème potentiel.</span><span class="sxs-lookup"><span data-stu-id="0399f-288">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="0399f-289">Si le stockage local est utilisé pour stocker le jeton, une vulnérabilité CSRF peut être atténuée, car les valeurs à partir du stockage local ne sont pas automatiquement envoyés au serveur avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="0399f-289">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="0399f-290">Par conséquent, l’utilisation du stockage local pour stocker le jeton anti-contrefaçon sur le client et envoie le jeton, comme un en-tête de demande est une approche recommandée.</span><span class="sxs-lookup"><span data-stu-id="0399f-290">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="0399f-291">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0399f-291">JavaScript</span></span>

<span data-ttu-id="0399f-292">À l’aide de JavaScript avec des vues, le jeton peut être créé à l’aide d’un service à partir de la vue.</span><span class="sxs-lookup"><span data-stu-id="0399f-292">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="0399f-293">Injecter le [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service dans la vue et appelez [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="0399f-293">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="0399f-294">Cette approche évite de devoir traiter directement avec la définition des cookies à partir du serveur ou de les lire à partir du client.</span><span class="sxs-lookup"><span data-stu-id="0399f-294">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="0399f-295">L’exemple précédent utilise JavaScript pour lire la valeur de champ masqué pour l’en-tête AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="0399f-295">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="0399f-296">JavaScript peut également jetons d’accès dans des cookies et utiliser le contenu du cookie pour créer un en-tête avec la valeur du jeton.</span><span class="sxs-lookup"><span data-stu-id="0399f-296">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="0399f-297">En supposant que le script demande à envoyer le jeton dans un en-tête appelé `X-CSRF-TOKEN`, configurer le service anti-contrefaçon pour rechercher le `X-CSRF-TOKEN` en-tête :</span><span class="sxs-lookup"><span data-stu-id="0399f-297">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="0399f-298">L’exemple suivant utilise JavaScript pour effectuer une requête AJAX avec l’en-tête approprié :</span><span class="sxs-lookup"><span data-stu-id="0399f-298">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="0399f-299">AngularJS</span><span class="sxs-lookup"><span data-stu-id="0399f-299">AngularJS</span></span>

<span data-ttu-id="0399f-300">AngularJS utilise une convention à l’adresse CSRF.</span><span class="sxs-lookup"><span data-stu-id="0399f-300">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="0399f-301">Si le serveur envoie un cookie portant le nom `XSRF-TOKEN`, le AngularJS `$http` service ajoute la valeur du cookie à un en-tête lorsqu’il envoie une demande au serveur.</span><span class="sxs-lookup"><span data-stu-id="0399f-301">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="0399f-302">Ce processus est automatique.</span><span class="sxs-lookup"><span data-stu-id="0399f-302">This process is automatic.</span></span> <span data-ttu-id="0399f-303">L’en-tête n’a pas besoin de définir explicitement dans le client.</span><span class="sxs-lookup"><span data-stu-id="0399f-303">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="0399f-304">Le nom d’en-tête est `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="0399f-304">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="0399f-305">Le serveur doit détecter cet en-tête et valider son contenu.</span><span class="sxs-lookup"><span data-stu-id="0399f-305">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="0399f-306">Pour les API ASP.NET Core travailler avec cette convention dans le démarrage de votre application :</span><span class="sxs-lookup"><span data-stu-id="0399f-306">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="0399f-307">Configurer votre application pour fournir un jeton dans un cookie appelé `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="0399f-307">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="0399f-308">Configurer le service anti-contrefaçon pour rechercher un en-tête nommé `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="0399f-308">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="0399f-309">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0399f-309">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="0399f-310">Étendre anti-contrefaçon</span><span class="sxs-lookup"><span data-stu-id="0399f-310">Extend antiforgery</span></span>

<span data-ttu-id="0399f-311">Le [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type permet aux développeurs d’étendre le comportement du système anti-CSRF par les données supplémentaires de l’aller-retour dans chaque jeton.</span><span class="sxs-lookup"><span data-stu-id="0399f-311">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="0399f-312">Le [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) méthode est appelée chaque fois qu’un jeton de champ est généré, et la valeur de retour est incorporée dans le jeton généré.</span><span class="sxs-lookup"><span data-stu-id="0399f-312">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="0399f-313">Un implémenteur peut retourner un horodatage, une valeur à usage unique ou toute autre valeur, puis appelez [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) pour valider ces données lorsque le jeton est validé.</span><span class="sxs-lookup"><span data-stu-id="0399f-313">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="0399f-314">Nom d’utilisateur du client est déjà incorporée dans les jetons générés, il est donc inutile d’inclure ces informations.</span><span class="sxs-lookup"><span data-stu-id="0399f-314">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="0399f-315">Si un jeton contient des données supplémentaires, mais aucune `IAntiForgeryAdditionalDataProvider` est configuré, les données supplémentaires n’est pas validées.</span><span class="sxs-lookup"><span data-stu-id="0399f-315">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0399f-316">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0399f-316">Additional resources</span></span>

* <span data-ttu-id="0399f-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) sur [ouvrir Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="0399f-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
