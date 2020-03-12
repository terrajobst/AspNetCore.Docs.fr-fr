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
# <a name="enforce-a-content-security-policy-for-aspnet-core-opno-locblazor"></a>Appliquer une stratégie de sécurité de contenu pour ASP.NET Core Blazor

Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

L’exécution de scripts [entre sites (XSS)](xref:security/cross-site-scripting) est une faille de sécurité dans laquelle un attaquant place un ou plusieurs scripts malveillants côté client dans le contenu rendu d’une application. Une stratégie de sécurité de contenu (CSP) vous aide à vous protéger contre les attaques XSS en informant le navigateur de la validité :

* Sources pour le contenu chargé, y compris les scripts, les feuilles de style et les images.
* Actions effectuées par une page, en spécifiant des cibles d’URL autorisées de formulaires.
* Plug-ins qui peuvent être chargés.

Pour appliquer un CSP à une application, le développeur spécifie plusieurs *directives* de sécurité de contenu CSP dans un ou plusieurs en-têtes de `Content-Security-Policy` ou balises `<meta>`.

Les stratégies sont évaluées par le navigateur pendant le chargement d’une page. Le navigateur inspecte les sources de la page et détermine si elles répondent aux exigences des directives de sécurité du contenu. Lorsque les directives de stratégie ne sont pas respectées pour une ressource, le navigateur ne charge pas la ressource. Par exemple, considérez une stratégie qui n’autorise pas les scripts tiers. Quand une page contient une balise `<script>` avec une origine tierce dans l’attribut `src`, le navigateur empêche le chargement du script.

CSP est pris en charge dans la plupart des navigateurs mobiles et de bureau modernes, notamment chrome, Edge, Firefox, Opera et Safari. CSP est recommandé pour les applications Blazor.

## <a name="policy-directives"></a>Directives de stratégie

Au minimum, spécifiez les directives et les sources suivantes pour les applications Blazor. Ajoutez des directives et des sources supplémentaires selon vos besoins. Les directives suivantes sont utilisées dans la section [appliquer la stratégie](#apply-the-policy) de cet article, où des exemples de stratégies de sécurité pour Blazor webassembly et Blazor Server sont fournis :

* l' [URI de base](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; restreint les URL pour la balise de `<base>` d’une page. Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.
* [bloc-All-Mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash; empêche le chargement de contenu HTTP et HTTPS mixte.
* [default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash; indique une solution de secours pour les directives source qui ne sont pas explicitement spécifiées par la stratégie. Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.
* [img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; indique des sources valides pour les images.
  * Spécifiez `data:` pour autoriser le chargement d’images à partir d' `data:` URL.
  * Spécifiez `https:` pour autoriser le chargement d’images à partir de points de terminaison HTTPs.
* [objet-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; indique des sources valides pour les balises `<object>`, `<embed>`et `<applet>`. Spécifiez `none` pour empêcher toutes les sources d’URL.
* [script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; indique des sources valides pour les scripts.
  * Spécifiez la source de l’hôte `https://stackpath.bootstrapcdn.com/` pour les scripts de démarrage.
  * Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.
  * Dans une application Blazor webassembly :
    * Spécifiez les hachages suivants pour permettre le chargement des scripts incorporés de l' Blazor webassembly requis :
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * Spécifiez `unsafe-eval` pour utiliser `eval()` et les méthodes de création de code à partir de chaînes.
  * Dans une application Blazor Server, spécifiez le hachage `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` pour le script inline qui effectue une détection de secours pour les feuilles de style.
* [style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; indique des sources valides pour les feuilles de style.
  * Spécifiez le `https://stackpath.bootstrapcdn.com/` source hôte pour les feuilles de style de démarrage.
  * Spécifiez `self` pour indiquer que l’origine de l’application, y compris le schéma et le numéro de port, est une source valide.
  * Spécifiez `unsafe-inline` pour autoriser l’utilisation de styles intralignes. La déclaration inline est requise pour l’interface utilisateur dans les applications Blazor Server pour la reconnexion du client et du serveur après la demande initiale. Dans une version ultérieure, le style intraligne peut être supprimé afin que `unsafe-inline` ne soit plus nécessaire.
* [Upgrade-unsecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash; indique que les URL de contenu provenant de sources non sécurisées (http) doivent être acquises en toute sécurité via HTTPS.

Les directives précédentes sont prises en charge par tous les navigateurs, à l’exception de Microsoft Internet Explorer.

Pour obtenir des hachages SHA pour d’autres scripts inline :

* Appliquez le CSP présenté dans la section [appliquer la stratégie](#apply-the-policy) .
* Accédez à la console outils de développement du navigateur tout en exécutant l’application localement. Le navigateur calcule et affiche les hachages des scripts bloqués lorsqu’un en-tête CSP ou une balise de `meta` est présent.
* Copiez les hachages fournis par le navigateur dans les sources de `script-src`. Utilisez des guillemets simples autour de chaque hachage.

Pour obtenir une matrice de prise en charge des navigateurs de niveau 2 de stratégie de sécurité de contenu, consultez la page [comment utiliser le niveau de stratégie de sécurité de contenu 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).

## <a name="apply-the-policy"></a>Application de la stratégie

Utilisez une balise de `<meta>` pour appliquer la stratégie :

* Définissez la valeur de l’attribut `http-equiv` sur `Content-Security-Policy`.
* Placez les directives dans la valeur de l’attribut `content`. Séparez les directives par un point-virgule (`;`).
* Placez toujours la balise `meta` dans le contenu `<head>`.

Les sections suivantes présentent des exemples de stratégies pour Blazor webassembly et Blazor Server. Ces exemples sont associés à des versions de cet article pour chaque version de Blazor. Pour utiliser une version appropriée pour votre version, sélectionnez la version du document avec le sélecteur de **version** déroulante sur cette page Web.

### <a name="opno-locblazor-webassembly"></a>Blazor webassembly

Dans le `<head>` contenu de la page hôte *wwwroot/index.html* , appliquez les directives décrites dans la section [directives de stratégie](#policy-directives) :

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

### <a name="opno-locblazor-server"></a>Serveur de Blazor

Dans le contenu `<head>` de la page hôte *pages/_Host. cshtml* , appliquez les directives décrites dans la section [directives de stratégie](#policy-directives) :

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

## <a name="meta-tag-limitations"></a>Limitations des balises meta

Une stratégie de balise de `<meta>` ne prend pas en charge les directives suivantes :

* [ancêtres de cadre](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [rapport-à](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [rapport-URI](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [immédiatement](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

Pour prendre en charge les directives précédentes, utilisez un en-tête nommé `Content-Security-Policy`. La chaîne de directive est la valeur de l’en-tête.

## <a name="test-a-policy-and-receive-violation-reports"></a>Tester une stratégie et recevoir des rapports de violation

Le test permet de vérifier que les scripts tiers ne sont pas bloqués par inadvertance lors de la création d’une stratégie initiale.

Pour tester une stratégie pendant un certain temps sans appliquer les directives de stratégie, définissez le nom de l’attribut `http-equiv` ou de l’en-tête de la balise `<meta>` d’une stratégie basée sur un en-tête sur `Content-Security-Policy-Report-Only`. Les rapports d’échec sont envoyés en tant que documents JSON à une URL spécifiée. Pour plus d’informations, consultez la [page documentation Web MDN : contenu-sécurité-stratégie-rapport uniquement](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).

Pour la création de rapports sur les violations pendant qu’une stratégie est active, consultez les articles suivants :

* [rapport-à](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [rapport-URI](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

Bien que `report-uri` ne soit plus recommandé pour une utilisation, les deux directives doivent être utilisées jusqu’à ce que `report-to` soit pris en charge par tous les principaux navigateurs. N’utilisez pas uniquement `report-uri`, car la prise en charge de `report-uri` est sujette à être supprimée à *tout moment* à partir des navigateurs. Supprimer la prise en charge de `report-uri` dans vos stratégies lorsque `report-to` est entièrement pris en charge. Pour suivre l’adoption de `report-to`, consultez Comment [utiliser : rapport](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).

Testez et mettez à jour la stratégie d’une application chaque version.

## <a name="troubleshoot"></a>Dépanner

* Les erreurs s’affichent dans la console outils de développement du navigateur. Les navigateurs fournissent des informations sur les éléments suivants :
  * Éléments qui ne sont pas conformes à la stratégie.
  * Comment modifier la stratégie pour autoriser un élément bloqué.
* Une stratégie est entièrement efficace lorsque le navigateur du client prend en charge toutes les directives incluses. Pour obtenir une matrice de prise en charge actuelle du navigateur, consultez Comment [utiliser : Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).

## <a name="additional-resources"></a>Ressources supplémentaires

* [MDN Web docs : Content-Security-Policy](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [Niveau de stratégie de sécurité du contenu 2](https://www.w3.org/TR/CSP2/)
* [Évaluateur Google CSP](https://csp-evaluator.withgoogle.com/)
