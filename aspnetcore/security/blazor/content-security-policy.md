---
title: Appliquer une stratégie de sécurité de contenu pour ASP.NET Core Blazor
author: guardrex
description: Découvrez comment utiliser une stratégie de sécurité de contenu (CSP, Content Security Policy) avec ASP.NET Core applications Blazor pour vous protéger contre les attaques de script entre sites (XSS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/content-security-policy
ms.openlocfilehash: 1cfebf7b3d3bbb98a671b6f2db7c6518cda74b65
ms.sourcegitcommit: 51c86c003ab5436598dbc42f26ea4a83a795fd6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78964549"
---
# <a name="enforce-a-content-security-policy-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="56a05-103">Appliquer une stratégie de sécurité de contenu pour ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="56a05-103">Enforce a Content Security Policy for ASP.NET Core Blazor</span></span>

<span data-ttu-id="56a05-104">Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="56a05-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="56a05-105">L’exécution de scripts [entre sites (XSS)](xref:security/cross-site-scripting) est une faille de sécurité dans laquelle un attaquant place un ou plusieurs scripts malveillants côté client dans le contenu rendu d’une application.</span><span class="sxs-lookup"><span data-stu-id="56a05-105">[Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) is a security vulnerability where an attacker places one or more malicious client-side scripts into an app's rendered content.</span></span> <span data-ttu-id="56a05-106">Une stratégie de sécurité de contenu (CSP) vous aide à vous protéger contre les attaques XSS en informant le navigateur de la validité :</span><span class="sxs-lookup"><span data-stu-id="56a05-106">A Content Security Policy (CSP) helps protect against XSS attacks by informing the browser of valid:</span></span>

* <span data-ttu-id="56a05-107">Sources pour le contenu chargé, y compris les scripts, les feuilles de style et les images.</span><span class="sxs-lookup"><span data-stu-id="56a05-107">Sources for loaded content, including scripts, stylesheets, and images.</span></span>
* <span data-ttu-id="56a05-108">Actions effectuées par une page, en spécifiant des cibles d’URL autorisées de formulaires.</span><span class="sxs-lookup"><span data-stu-id="56a05-108">Actions taken by a page, specifying permitted URL targets of forms.</span></span>
* <span data-ttu-id="56a05-109">Plug-ins qui peuvent être chargés.</span><span class="sxs-lookup"><span data-stu-id="56a05-109">Plugins that can be loaded.</span></span>

<span data-ttu-id="56a05-110">Pour appliquer un CSP à une application, le développeur spécifie plusieurs *directives* de sécurité de contenu CSP dans un ou plusieurs en-têtes de `Content-Security-Policy` ou balises `<meta>`.</span><span class="sxs-lookup"><span data-stu-id="56a05-110">To apply a CSP to an app, the developer specifies several CSP content security *directives* in one or more `Content-Security-Policy` headers or `<meta>` tags.</span></span>

<span data-ttu-id="56a05-111">Les stratégies sont évaluées par le navigateur pendant le chargement d’une page.</span><span class="sxs-lookup"><span data-stu-id="56a05-111">Policies are evaluated by the browser while a page is loading.</span></span> <span data-ttu-id="56a05-112">Le navigateur inspecte les sources de la page et détermine si elles répondent aux exigences des directives de sécurité du contenu.</span><span class="sxs-lookup"><span data-stu-id="56a05-112">The browser inspects the page's sources and determines if they meet the requirements of the content security directives.</span></span> <span data-ttu-id="56a05-113">Lorsque les directives de stratégie ne sont pas respectées pour une ressource, le navigateur ne charge pas la ressource.</span><span class="sxs-lookup"><span data-stu-id="56a05-113">When policy directives aren't met for a resource, the browser doesn't load the resource.</span></span> <span data-ttu-id="56a05-114">Par exemple, considérez une stratégie qui n’autorise pas les scripts tiers.</span><span class="sxs-lookup"><span data-stu-id="56a05-114">For example, consider a policy that doesn't allow third-party scripts.</span></span> <span data-ttu-id="56a05-115">Quand une page contient une balise `<script>` avec une origine tierce dans l’attribut `src`, le navigateur empêche le chargement du script.</span><span class="sxs-lookup"><span data-stu-id="56a05-115">When a page contains a `<script>` tag with a third-party origin in the `src` attribute, the browser prevents the script from loading.</span></span>

<span data-ttu-id="56a05-116">CSP est pris en charge dans la plupart des navigateurs mobiles et de bureau modernes, notamment chrome, Edge, Firefox, Opera et Safari.</span><span class="sxs-lookup"><span data-stu-id="56a05-116">CSP is supported in most modern desktop and mobile browsers, including Chrome, Edge, Firefox, Opera, and Safari.</span></span> <span data-ttu-id="56a05-117">CSP est recommandé pour les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="56a05-117">CSP is recommended for Blazor apps.</span></span>

## <a name="policy-directives"></a><span data-ttu-id="56a05-118">Directives de stratégie</span><span class="sxs-lookup"><span data-stu-id="56a05-118">Policy directives</span></span>

<span data-ttu-id="56a05-119">Au minimum, spécifiez les directives et les sources suivantes pour les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="56a05-119">Minimally, specify the following directives and sources for Blazor apps.</span></span> <span data-ttu-id="56a05-120">Ajoutez des directives et des sources supplémentaires selon vos besoins.</span><span class="sxs-lookup"><span data-stu-id="56a05-120">Add additional directives and sources as needed.</span></span> <span data-ttu-id="56a05-121">Les directives suivantes sont utilisées dans la section [appliquer la stratégie](#apply-the-policy) de cet article, où des exemples de stratégies de sécurité pour Blazor webassembly et Blazor Server sont fournis :</span><span class="sxs-lookup"><span data-stu-id="56a05-121">The following directives are used in the [Apply the policy](#apply-the-policy) section of this article, where example security policies for Blazor WebAssembly and Blazor Server are provided:</span></span>

* <span data-ttu-id="56a05-122">l' [URI de base](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; restreint les URL pour la balise de `<base>` d’une page.</span><span class="sxs-lookup"><span data-stu-id="56a05-122">[base-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; Restricts the URLs for a page's `<base>` tag.</span></span> <span data-ttu-id="56a05-123">Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.</span><span class="sxs-lookup"><span data-stu-id="56a05-123">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="56a05-124">[bloc-All-Mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash; empêche le chargement de contenu HTTP et HTTPS mixte.</span><span class="sxs-lookup"><span data-stu-id="56a05-124">[block-all-mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash; Prevents loading mixed HTTP and HTTPS content.</span></span>
* <span data-ttu-id="56a05-125">[default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash; indique une solution de secours pour les directives source qui ne sont pas explicitement spécifiées par la stratégie.</span><span class="sxs-lookup"><span data-stu-id="56a05-125">[default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash; Indicates a fallback for source directives that aren't explicitly specified by the policy.</span></span> <span data-ttu-id="56a05-126">Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.</span><span class="sxs-lookup"><span data-stu-id="56a05-126">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="56a05-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; indique des sources valides pour les images.</span><span class="sxs-lookup"><span data-stu-id="56a05-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; Indicates valid sources for images.</span></span>
  * <span data-ttu-id="56a05-128">Spécifiez `data:` pour autoriser le chargement d’images à partir d' `data:` URL.</span><span class="sxs-lookup"><span data-stu-id="56a05-128">Specify `data:` to permit loading images from `data:` URLs.</span></span>
  * <span data-ttu-id="56a05-129">Spécifiez `https:` pour autoriser le chargement d’images à partir de points de terminaison HTTPs.</span><span class="sxs-lookup"><span data-stu-id="56a05-129">Specify `https:` to permit loading images from HTTPS endpoints.</span></span>
* <span data-ttu-id="56a05-130">[objet-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; indique des sources valides pour les balises `<object>`, `<embed>`et `<applet>`.</span><span class="sxs-lookup"><span data-stu-id="56a05-130">[object-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; Indicates valid sources for the `<object>`, `<embed>`, and `<applet>` tags.</span></span> <span data-ttu-id="56a05-131">Spécifiez `none` pour empêcher toutes les sources d’URL.</span><span class="sxs-lookup"><span data-stu-id="56a05-131">Specify `none` to prevent all URL sources.</span></span>
* <span data-ttu-id="56a05-132">[script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; indique des sources valides pour les scripts.</span><span class="sxs-lookup"><span data-stu-id="56a05-132">[script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; Indicates valid sources for scripts.</span></span>
  * <span data-ttu-id="56a05-133">Spécifiez la source de l’hôte `https://stackpath.bootstrapcdn.com/` pour les scripts de démarrage.</span><span class="sxs-lookup"><span data-stu-id="56a05-133">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap scripts.</span></span>
  * <span data-ttu-id="56a05-134">Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.</span><span class="sxs-lookup"><span data-stu-id="56a05-134">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="56a05-135">Dans une application Blazor webassembly :</span><span class="sxs-lookup"><span data-stu-id="56a05-135">In a Blazor WebAssembly app:</span></span>
    * <span data-ttu-id="56a05-136">Spécifiez les hachages suivants pour permettre le chargement des scripts incorporés de l' Blazor webassembly requis :</span><span class="sxs-lookup"><span data-stu-id="56a05-136">Specify the following hashes to permit the required Blazor WebAssembly inline scripts to load:</span></span>
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * <span data-ttu-id="56a05-137">Spécifiez `unsafe-eval` pour utiliser `eval()` et les méthodes de création de code à partir de chaînes.</span><span class="sxs-lookup"><span data-stu-id="56a05-137">Specify `unsafe-eval` to use `eval()` and methods for creating code from strings.</span></span>
  * <span data-ttu-id="56a05-138">Dans une application Blazor Server, spécifiez le hachage `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` pour le script inline qui effectue une détection de secours pour les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="56a05-138">In a Blazor Server app, specify the `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` hash for the inline script that performs fallback detection for stylesheets.</span></span>
* <span data-ttu-id="56a05-139">[style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; indique des sources valides pour les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="56a05-139">[style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; Indicates valid sources for stylesheets.</span></span>
  * <span data-ttu-id="56a05-140">Spécifiez le `https://stackpath.bootstrapcdn.com/` source hôte pour les feuilles de style de démarrage.</span><span class="sxs-lookup"><span data-stu-id="56a05-140">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap stylesheets.</span></span>
  * <span data-ttu-id="56a05-141">Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.</span><span class="sxs-lookup"><span data-stu-id="56a05-141">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="56a05-142">Spécifiez `unsafe-inline` pour autoriser l’utilisation de styles intralignes.</span><span class="sxs-lookup"><span data-stu-id="56a05-142">Specify `unsafe-inline` to allow the use of inline styles.</span></span> <span data-ttu-id="56a05-143">La déclaration inline est requise pour l’interface utilisateur dans les applications Blazor Server pour la reconnexion du client et du serveur après la demande initiale.</span><span class="sxs-lookup"><span data-stu-id="56a05-143">The inline declaration is required for the UI in Blazor Server apps for reconnecting the client and server after the initial request.</span></span> <span data-ttu-id="56a05-144">Dans une version ultérieure, le style intraligne peut être supprimé afin que `unsafe-inline` ne soit plus nécessaire.</span><span class="sxs-lookup"><span data-stu-id="56a05-144">In a future release, inline styling might be removed so that `unsafe-inline` is no longer required.</span></span>
* <span data-ttu-id="56a05-145">[Upgrade-unsecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash; indique que les URL de contenu provenant de sources non sécurisées (http) doivent être acquises en toute sécurité via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56a05-145">[upgrade-insecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash; Indicates that content URLs from insecure (HTTP) sources should be acquired securely over HTTPS.</span></span>

<span data-ttu-id="56a05-146">Les directives précédentes sont prises en charge par tous les navigateurs, à l’exception de Microsoft Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="56a05-146">The preceding directives are supported by all browsers except Microsoft Internet Explorer.</span></span>

<span data-ttu-id="56a05-147">Pour obtenir des hachages SHA pour d’autres scripts inline :</span><span class="sxs-lookup"><span data-stu-id="56a05-147">To obtain SHA hashes for additional inline scripts:</span></span>

* <span data-ttu-id="56a05-148">Appliquez le CSP présenté dans la section [appliquer la stratégie](#apply-the-policy) .</span><span class="sxs-lookup"><span data-stu-id="56a05-148">Apply the CSP shown in the [Apply the policy](#apply-the-policy) section.</span></span>
* <span data-ttu-id="56a05-149">Accédez à la console outils de développement du navigateur tout en exécutant l’application localement.</span><span class="sxs-lookup"><span data-stu-id="56a05-149">Access the browser's developer tools console while running the app locally.</span></span> <span data-ttu-id="56a05-150">Le navigateur calcule et affiche les hachages des scripts bloqués lorsqu’un en-tête CSP ou une balise de `meta` est présent.</span><span class="sxs-lookup"><span data-stu-id="56a05-150">The browser calculates and displays hashes for blocked scripts when a CSP header or `meta` tag is present.</span></span>
* <span data-ttu-id="56a05-151">Copiez les hachages fournis par le navigateur dans les sources de `script-src`.</span><span class="sxs-lookup"><span data-stu-id="56a05-151">Copy the hashes provided by the browser to the `script-src` sources.</span></span> <span data-ttu-id="56a05-152">Utilisez des guillemets simples autour de chaque hachage.</span><span class="sxs-lookup"><span data-stu-id="56a05-152">Use single quotes around each hash.</span></span>

<span data-ttu-id="56a05-153">Pour obtenir une matrice de prise en charge des navigateurs de niveau 2 de stratégie de sécurité de contenu, consultez la page [comment utiliser le niveau de stratégie de sécurité de contenu 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).</span><span class="sxs-lookup"><span data-stu-id="56a05-153">For a Content Security Policy Level 2 browser support matrix, see [Can I use: Content Security Policy Level 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).</span></span>

## <a name="apply-the-policy"></a><span data-ttu-id="56a05-154">Application de la stratégie</span><span class="sxs-lookup"><span data-stu-id="56a05-154">Apply the policy</span></span>

<span data-ttu-id="56a05-155">Utilisez une balise de `<meta>` pour appliquer la stratégie :</span><span class="sxs-lookup"><span data-stu-id="56a05-155">Use a `<meta>` tag to apply the policy:</span></span>

* <span data-ttu-id="56a05-156">Définissez la valeur de l’attribut `http-equiv` sur `Content-Security-Policy`.</span><span class="sxs-lookup"><span data-stu-id="56a05-156">Set the value of the `http-equiv` attribute to `Content-Security-Policy`.</span></span>
* <span data-ttu-id="56a05-157">Placez les directives dans la valeur de l’attribut `content`.</span><span class="sxs-lookup"><span data-stu-id="56a05-157">Place the directives in the `content` attribute value.</span></span> <span data-ttu-id="56a05-158">Séparez les directives par un point-virgule (`;`).</span><span class="sxs-lookup"><span data-stu-id="56a05-158">Separate directives with a semicolon (`;`).</span></span>
* <span data-ttu-id="56a05-159">Placez toujours la balise `meta` dans le contenu `<head>`.</span><span class="sxs-lookup"><span data-stu-id="56a05-159">Always place the `meta` tag in the `<head>` content.</span></span>

<span data-ttu-id="56a05-160">Les sections suivantes présentent des exemples de stratégies pour Blazor webassembly et Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="56a05-160">The following sections show example policies for Blazor WebAssembly and Blazor Server.</span></span> <span data-ttu-id="56a05-161">Ces exemples sont associés à des versions de cet article pour chaque version de Blazor.</span><span class="sxs-lookup"><span data-stu-id="56a05-161">These examples are versioned with this article for each release of Blazor.</span></span> <span data-ttu-id="56a05-162">Pour utiliser une version appropriée pour votre version, sélectionnez la version du document avec le sélecteur de **version** déroulante sur cette page Web.</span><span class="sxs-lookup"><span data-stu-id="56a05-162">To use a version appropriate for your release, select the document version with the **Version** drop down selector on this webpage.</span></span>

### <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="56a05-163"> webassembly</span><span class="sxs-lookup"><span data-stu-id="56a05-163"> WebAssembly</span></span>

<span data-ttu-id="56a05-164">Dans le `<head>` contenu de la page hôte *wwwroot/index.html* , appliquez les directives décrites dans la section [directives de stratégie](#policy-directives) :</span><span class="sxs-lookup"><span data-stu-id="56a05-164">In the `<head>` content of the *wwwroot/index.html* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

```html
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=' 
                          'sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=' 
                          'sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=' 
                          'unsafe-eval';
               style-src https://stackpath.bootstrapcdn.com/
                         'self'
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

### <a name="opno-locblazor-server"></a><span data-ttu-id="56a05-165">Serveur de Blazor</span><span class="sxs-lookup"><span data-stu-id="56a05-165">Blazor Server</span></span>

<span data-ttu-id="56a05-166">Dans le contenu `<head>` de la page hôte *pages/_Host. cshtml* , appliquez les directives décrites dans la section [directives de stratégie](#policy-directives) :</span><span class="sxs-lookup"><span data-stu-id="56a05-166">In the `<head>` content of the *Pages/_Host.cshtml* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

```cshtml
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=';
               style-src https://stackpath.bootstrapcdn.com/
                         'self' 
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

## <a name="meta-tag-limitations"></a><span data-ttu-id="56a05-167">Limitations des balises meta</span><span class="sxs-lookup"><span data-stu-id="56a05-167">Meta tag limitations</span></span>

<span data-ttu-id="56a05-168">Une stratégie de balise de `<meta>` ne prend pas en charge les directives suivantes :</span><span class="sxs-lookup"><span data-stu-id="56a05-168">A `<meta>` tag policy doesn't support the following directives:</span></span>

* [<span data-ttu-id="56a05-169">ancêtres de cadre</span><span class="sxs-lookup"><span data-stu-id="56a05-169">frame-ancestors</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [<span data-ttu-id="56a05-170">rapport-à</span><span class="sxs-lookup"><span data-stu-id="56a05-170">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="56a05-171">rapport-URI</span><span class="sxs-lookup"><span data-stu-id="56a05-171">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [<span data-ttu-id="56a05-172">immédiatement</span><span class="sxs-lookup"><span data-stu-id="56a05-172">sandbox</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

<span data-ttu-id="56a05-173">Pour prendre en charge les directives précédentes, utilisez un en-tête nommé `Content-Security-Policy`.</span><span class="sxs-lookup"><span data-stu-id="56a05-173">To support the preceding directives, use a header named `Content-Security-Policy`.</span></span> <span data-ttu-id="56a05-174">La chaîne de directive est la valeur de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="56a05-174">The directive string is the header's value.</span></span>

## <a name="test-a-policy-and-receive-violation-reports"></a><span data-ttu-id="56a05-175">Tester une stratégie et recevoir des rapports de violation</span><span class="sxs-lookup"><span data-stu-id="56a05-175">Test a policy and receive violation reports</span></span>

<span data-ttu-id="56a05-176">Le test permet de vérifier que les scripts tiers ne sont pas bloqués par inadvertance lors de la création d’une stratégie initiale.</span><span class="sxs-lookup"><span data-stu-id="56a05-176">Testing helps confirm that third-party scripts aren't inadvertently blocked when building an initial policy.</span></span>

<span data-ttu-id="56a05-177">Pour tester une stratégie pendant un certain temps sans appliquer les directives de stratégie, définissez le nom de l’attribut `http-equiv` ou de l’en-tête de la balise `<meta>` d’une stratégie basée sur un en-tête sur `Content-Security-Policy-Report-Only`.</span><span class="sxs-lookup"><span data-stu-id="56a05-177">To test a policy over a period of time without enforcing the policy directives, set the `<meta>` tag's `http-equiv` attribute or header name of a header-based policy to `Content-Security-Policy-Report-Only`.</span></span> <span data-ttu-id="56a05-178">Les rapports d’échec sont envoyés en tant que documents JSON à une URL spécifiée.</span><span class="sxs-lookup"><span data-stu-id="56a05-178">Failure reports are sent as JSON documents to a specified URL.</span></span> <span data-ttu-id="56a05-179">Pour plus d’informations, consultez la [page documentation Web MDN : contenu-sécurité-stratégie-rapport uniquement](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).</span><span class="sxs-lookup"><span data-stu-id="56a05-179">For more information, see [MDN web docs: Content-Security-Policy-Report-Only](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).</span></span>

<span data-ttu-id="56a05-180">Pour la création de rapports sur les violations pendant qu’une stratégie est active, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="56a05-180">For reporting on violations while a policy is active, see the following articles:</span></span>

* [<span data-ttu-id="56a05-181">rapport-à</span><span class="sxs-lookup"><span data-stu-id="56a05-181">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="56a05-182">rapport-URI</span><span class="sxs-lookup"><span data-stu-id="56a05-182">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

<span data-ttu-id="56a05-183">Bien que `report-uri` ne soit plus recommandé pour une utilisation, les deux directives doivent être utilisées jusqu’à ce que `report-to` soit pris en charge par tous les principaux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="56a05-183">Although `report-uri` is no longer recommended for use, both directives should be used until `report-to` is supported by all of the major browsers.</span></span> <span data-ttu-id="56a05-184">N’utilisez pas uniquement `report-uri`, car la prise en charge de `report-uri` est sujette à être supprimée à *tout moment* à partir des navigateurs.</span><span class="sxs-lookup"><span data-stu-id="56a05-184">Don't exclusively use `report-uri` because support for `report-uri` is subject to being dropped *at any time* from browsers.</span></span> <span data-ttu-id="56a05-185">Supprimer la prise en charge de `report-uri` dans vos stratégies lorsque `report-to` est entièrement pris en charge.</span><span class="sxs-lookup"><span data-stu-id="56a05-185">Remove support for `report-uri` in your policies when `report-to` is fully supported.</span></span> <span data-ttu-id="56a05-186">Pour suivre l’adoption de `report-to`, consultez Comment [utiliser : rapport](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).</span><span class="sxs-lookup"><span data-stu-id="56a05-186">To track adoption of `report-to`, see [Can I use: report-to](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).</span></span>

<span data-ttu-id="56a05-187">Testez et mettez à jour la stratégie d’une application chaque version.</span><span class="sxs-lookup"><span data-stu-id="56a05-187">Test and update an app's policy every release.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="56a05-188">Dépanner</span><span class="sxs-lookup"><span data-stu-id="56a05-188">Troubleshoot</span></span>

* <span data-ttu-id="56a05-189">Les erreurs s’affichent dans la console outils de développement du navigateur.</span><span class="sxs-lookup"><span data-stu-id="56a05-189">Errors appear in the browser's developer tools console.</span></span> <span data-ttu-id="56a05-190">Les navigateurs fournissent des informations sur les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="56a05-190">Browsers provide information about:</span></span>
  * <span data-ttu-id="56a05-191">Éléments qui ne sont pas conformes à la stratégie.</span><span class="sxs-lookup"><span data-stu-id="56a05-191">Elements that don't comply with the policy.</span></span>
  * <span data-ttu-id="56a05-192">Comment modifier la stratégie pour autoriser un élément bloqué.</span><span class="sxs-lookup"><span data-stu-id="56a05-192">How to modify the policy to allow for a blocked item.</span></span>
* <span data-ttu-id="56a05-193">Une stratégie est entièrement efficace lorsque le navigateur du client prend en charge toutes les directives incluses.</span><span class="sxs-lookup"><span data-stu-id="56a05-193">A policy is only completely effective when the client's browser supports all of the included directives.</span></span> <span data-ttu-id="56a05-194">Pour obtenir une matrice de prise en charge actuelle du navigateur, consultez Comment [utiliser : Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).</span><span class="sxs-lookup"><span data-stu-id="56a05-194">For a current browser support matrix, see [Can I use: Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56a05-195">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="56a05-195">Additional resources</span></span>

* [<span data-ttu-id="56a05-196">MDN Web docs : Content-Security-Policy</span><span class="sxs-lookup"><span data-stu-id="56a05-196">MDN web docs: Content-Security-Policy</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [<span data-ttu-id="56a05-197">Niveau de stratégie de sécurité du contenu 2</span><span class="sxs-lookup"><span data-stu-id="56a05-197">Content Security Policy Level 2</span></span>](https://www.w3.org/TR/CSP2/)
* [<span data-ttu-id="56a05-198">Évaluateur Google CSP</span><span class="sxs-lookup"><span data-stu-id="56a05-198">Google CSP Evaluator</span></span>](https://csp-evaluator.withgoogle.com/)
