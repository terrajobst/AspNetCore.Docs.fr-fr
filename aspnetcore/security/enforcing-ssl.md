---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans application web ASP.NET Core.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: d8bf11d7d2df8d8b197f001570a8fab1f3262814
ms.sourcegitcommit: 4e34ce61e1e7f1317102b16012ce0742abf2cca6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514802"
---
# <a name="enforce-https-in-aspnet-core"></a>Appliquer HTTPS dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce document montre comment :

* Exiger s-HTTP pour toutes les demandes.
* Rediriger toutes les requêtes HTTP vers HTTPS.

> [!WARNING]
> Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles. `RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS. Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS. Ces clients peuvent envoyer des informations sur HTTP. Les API web doivent soit :
>
> * Ne pas écouter sur HTTP.
> * Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.

<a name="require"></a>
## <a name="require-https"></a>Exiger s-http

::: moniker range=">= aspnetcore-2.1"

Nous vous recommandons de toutes les applications web de ASP.NET Core appellent intergiciel de la Redirection de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les requêtes HTTP vers HTTPS.

Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Le code précédent en surbrillance :

* Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`). Les applications de production doivent appeler [UseHsts](#hsts).
* Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).

Le code suivant appelle [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Le code précédent en surbrillance :

* Jeux [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à `Status307TemporaryRedirect`, qui est la valeur par défaut. Les applications de production doivent appeler [UseHsts](#hsts).
* Définit le port HTTPS par 5001. La valeur par défaut est 443.

Les mécanismes suivants définir automatiquement le port :

* Le middleware peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :
  - Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur du Code Visual Studio).
  - Uniquement **un seul port HTTPS** est utilisé par l’application.
* Visual Studio est utilisé :
  - IIS Express a HTTPS activé.
  - *launchSettings.json* définit le `sslPort` pour IIS Express.

> [!NOTE]
> Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible. Le port doit être configuré manuellement. Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.

Le port peut être configuré en définissant le [paramètre de configuration d’hôte Web https_port](xref:fundamentals/host/web-host#https-port):

**Clé**: https_port **Type**: *chaîne*
**par défaut**: une valeur par défaut n’est pas définie.
**À l’aide de**: `UseSetting` 
 **variable d’environnement**: `<PREFIX_>HTTPS_PORT` (le préfixe est `ASPNETCORE_` lors de l’utilisation de l’hôte Web.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Le port peut être configuré indirectement en définissant l’URL avec le `ASPNETCORE_URLS` variable d’environnement. La variable d’environnement configure le serveur, et détecte ensuite l’intergiciel (middleware) indirectement les le port HTTPS par le biais de `IServerAddressesFeature`.

Si aucun port n’est définie :

* Les requêtes ne sont pas redirigées.
* L’intergiciel (middleware) consigne un avertissement.

> [!NOTE]
> Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`). `AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection. Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).
>
> Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Le [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisée pour exiger s-HTTP. `[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peuvent être appliquées globalement. Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: 

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Le code précédent en surbrillance nécessite d’utilisent toutes les demandes `HTTPS`; par conséquent, les requêtes HTTP sont ignorés. Le code en surbrillance suivant redirige toutes les requêtes HTTP vers HTTPS :

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting). Le middleware permet également à l’application pour définir le code d’état ou le code d’état et le port lors de l’exécution de la redirection.

Exiger le protocole HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité, car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutés à votre application. Il faudra penser à appliquer l'attribut `[RequireHttps]`. Vous ne pouvez pas garantir que l'attribut `[RequireHttps]` est appliqué lors de l’ajout de nouveaux contrôleurs et des Pages Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protocole de sécurité Strict Transport HTTP (HSTS)

Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial. Lorsqu’un navigateur qui prend en charge HSTS reçoit cet en-tête, il stocke la configuration pour le domaine qui empêche l’envoi de toutes les communications via HTTP et au lieu de cela force toutes les communications via le protocole HTTPS. Il empêche également l’utilisateur à l’aide de certificats non approuvés ou non valides, désactiver les invites de navigateur qui permettent un utilisateur de confiance à ce certificat.

ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension. Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` n’est pas recommandée dans le développement, car l’en-tête HSTS est hautement mis en cache par les navigateurs. Par défaut, `UseHsts` exclut l’adresse de bouclage local.

Pour les environnements de production mise en œuvre HTTPS pour la première fois, définissez la valeur HSTS initiale sur une valeur faible. Définissez la valeur des heures non plus d’une seule journée au cas où vous deviez restaurer l’infrastructure HTTPS vers HTTP. Une fois que vous êtes ainsi certain de la durabilité de la configuration de HTTPS, augmentez la valeur de max-age HSTS ; une valeur couramment utilisée est un an. 

L'exemple de code suivant :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security. Le préchargement n’est pas dans le cadre de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation. Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).
* Permet de [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), auquel s’applique la stratégie HSTS à héberger des sous-domaines. 
* Définit explicitement le paramètre d’âge maximal de l’en-tête Strict-Transport-Security à 60 jours. Si ce n’est pas définie, la valeur par défaut est 30 jours. Consultez le [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) pour plus d’informations.
* Ajoute `example.com` à la liste des hôtes à exclure.

`UseHsts` exclut les hôtes de bouclage suivants :

* `localhost` : L’adresse de bouclage IPv4.
* `127.0.0.1` : L’adresse de bouclage IPv4.
* `[::1]` : L’adresse de bouclage IPv6.

L’exemple précédent montre comment ajouter des hôtes supplémentaires.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Annulations de HTTPS sur la création du projet

Activent les modèles d’application ASP.NET Core web 2.1 ou version ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) [la redirection HTTPS](#require) et [HSTS](#hsts). Pour les déploiements qui ne nécessitent pas HTTPS, vous pouvez ne pas adhérer du protocole HTTPS. Par exemple, certains services back-end où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.

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

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Comment configurer un certificat de développeur pour Docker

Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
