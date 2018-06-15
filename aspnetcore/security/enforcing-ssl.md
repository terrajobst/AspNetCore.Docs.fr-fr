---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans application web ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 48a25b7ba7affe84cfa6fe16096409239c510221
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652186"
---
# <a name="enforce-https-in-aspnet-core"></a>Appliquer HTTPS dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce document montre comment :

* Exiger HTTPS pour toutes les demandes.
* Rediriger toutes les demandes HTTP vers HTTPS.

> [!WARNING]
> Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles. `RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS. Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS. Ces clients peuvent envoyer des informations sur HTTP. Les API web doivent soit :
>
> * Ne pas écouter sur HTTP.
> * Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.

<a name="require"></a>
## <a name="require-https"></a>Exiger HTTPS

::: moniker range=">= aspnetcore-2.1"

Nous vous recommandons de toutes les applications de web ASP.NET Core appellent l’intergiciel (middleware) de HTTPS Redirection ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les demandes HTTP vers HTTPS.

Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Le code suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Le code en surbrillance précédent :

* Jeux de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à `Status307TemporaryRedirect`, qui est la valeur par défaut. Les applications de production doivent appeler [UseHsts](#hsts).
* Définit le port HTTPS 5001. La valeur par défaut est 443.

Les mécanismes suivants définir le port automatiquement :

* L’intergiciel (middleware) peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :
  - Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur de Visual Studio Code).
  - Uniquement **un port HTTPS** est utilisé par l’application.
* Visual Studio est utilisé :
  - IIS Express a activés HTTPS.
  - *launchSettings.json* définit le `sslPort` pour IIS Express.

> [!NOTE]
> Quand une application est exécutée derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible. Le port doit être configuré manuellement. Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.

Le port peut être configuré en définissant le :

* La variable d’environnement `ASPNETCORE_HTTPS_PORT`.
* `http_port` clé de configuration d’hôte (par exemple, via *hostsettings.json* ou un argument de ligne de commande).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). Consultez l’exemple précédent qui montre comment définir le port à 5001.

> [!NOTE]
> Le port peut être configuré indirectement par la définition de l’URL avec le `ASPNETCORE_URLS` variable d’environnement. La variable d’environnement configure le serveur, puis l’intergiciel (middleware) détecte indirectement le port HTTPS via `IServerAddressesFeature`.

Si aucun port n’est définie :

* Les demandes ne sont pas redirigées.
* L’intergiciel (middleware) consigne un avertissement.

> [!NOTE]
> Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser le Middleware de réécriture d’URL (`AddRedirectToHttps`). `AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection. Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).
>
> Lors de la redirection vers HTTPS sans nécessité de règles de redirection supplémentaires, nous vous recommandons d’utiliser intergiciel (middleware) de HTTPS Redirection (`UseHttpsRedirection`) décrits dans cette rubrique.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger HTTPS. `[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peut être appliqué globalement. Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: 

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Le code en surbrillance précédent requiert que toutes les demandes utilisent `HTTPS`; par conséquent, les requêtes HTTP sont ignorés. Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting). L’intergiciel (middleware) permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.

Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité, car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`. Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protocole de sécurité stricte de Transport HTTP (HSTS)

Par [OWASP avoir](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport sécurité (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de l’inclusion de sécurité qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial. Une fois qu’un navigateur pris en charge reçoit cet en-tête ce navigateur empêche les communications d’être envoyés via HTTP dans le domaine spécifié et l’envoie à la place toutes les communications via le protocole HTTPS. Elle évite également les invites sur les navigateurs de clics HTTPS.

ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension. Le code suivant appelle `UseHsts` lorsque l’application n’est pas [mode de développement](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` n’est pas recommandé dans le développement, car l’en-tête HSTS est hautement cachable par les navigateurs. Par défaut, UseHsts exclut l’adresse de bouclage locale.

L'exemple de code suivant :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Définit le paramètre de préchargement de l’en-tête de sécurité de Transport Strict. Le préchargement n’est pas dans le cadre de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur l’installation. Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).
* Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS de sous-domaines d’hôte. 
* Définit explicitement le paramètre de max-age de l’en-tête de sécurité de Transport Strict à 60 jours. Si la valeur n’est pas définie, la valeur par défaut est 30 jours. Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.
* Ajoute `example.com` à la liste des hôtes à exclure.

`UseHsts` exclut les ordinateurs hôtes de bouclage suivants :

* `localhost` : L’adresse de bouclage IPv4.
* `127.0.0.1` : L’adresse de bouclage IPv4.
* `[::1]` : L’adresse de bouclage IPv6.

L’exemple précédent montre comment ajouter des hôtes supplémentaires.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Annulations de HTTPS sur la création du projet

Les modèles d’application ASP.NET Core web 2.1 ou version ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) [la redirection HTTPS](#require) et [HSTS](#hsts). Pour les déploiements qui n’ont pas besoin de HTTPS, vous pouvez annuler le protocole HTTPS. Par exemple, certains services principaux où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.

Pour se désengager de HTTPS :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Désactivez la case à cocher **configurer pour le protocole HTTPS**.

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli) 

Utilisez l'option `--no-https`. Exemple :

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>La configuration d’un certificat de développeur pour Docker

Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
