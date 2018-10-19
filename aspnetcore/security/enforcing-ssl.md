---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment exiger HTTPS/TLS dans une application web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325599"
---
# <a name="enforce-https-in-aspnet-core"></a>Appliquer HTTPS dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce document montre comment :

* Exiger s-HTTP pour toutes les demandes.
* Rediriger toutes les requêtes HTTP vers HTTPS.

Aucune API ne peut empêcher un client d’envoyer des données sensibles à la première demande.

> [!WARNING]
> Ne **pas** utiliser [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sur les API web qui reçoivent des informations sensibles. `RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS. Les clients d’API ne peuvent pas comprendre ou obéir aux règles de redirection HTTP vers HTTPS. Ces clients peuvent envoyer des informations sur HTTP. Les API web doivent soit :
>
> * Ne pas écouter sur HTTP.
> * Fermer la connexion avec le code d’état 400 (demande incorrecte) et ne pas servir la demande.

<a name="require"></a>

## <a name="require-https"></a>Exiger s-http

::: moniker range=">= aspnetcore-2.1"

Nous recommandons d’ASP.NET Core de production tous les appels d’applications web :

* Le Middleware de Redirection HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) pour rediriger toutes les requêtes HTTP vers HTTPS.
* [UseHsts](#hsts), protocole de sécurité Strict Transport HTTP (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Le code précédent en surbrillance :

* Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), sauf substitution par le `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Nous vous recommandons d’utiliser effectue une redirection temporaire plutôt que des redirections permanentes, comme la mise en cache de lien peut provoquer un comportement instable dans les environnements de développement. Nous vous recommandons d’utiliser [HSTS](#hsts) pour signaler aux clients qui sécurisent uniquement les ressources des demandes doivent être envoyés à l’application (uniquement en production).

> [!WARNING]
> Un port doit être disponible pour l’intergiciel (middleware) pour rediriger vers HTTPS. Si aucun port n’est disponible, la redirection vers HTTPS ne se produit. Le port HTTPS peut être spécifié en utilisant l’une des approches suivantes :
>
> * Définir `HttpsRedirectionOptions.HttpsPort`.
> * Définissez la variable d’environnement `ASPNETCORE_HTTPS_PORT`.
> * Dans le développement, définir une URL HTTPS dans *launchsettings.json*.
> * Configurer un point de terminaison URL HTTPS pour [Kestrel](xref:fundamentals/servers/kestrel) ou [HTTP.sys](xref:fundamentals/servers/httpsys).
>
> Quand Kestrel ou HTTP.sys est utilisée comme un serveur edge de public, Kestrel ou HTTP.sys doit être configuré pour écouter sur les deux :
>
> * Le port sécurisé où le client est redirigé (en règle générale, 443 dans 5001 dans le développement et de production).
> * Le port non sécurisé (en règle générale, 80 en production) et 5000 dans le développement.
>
> Le port non sécurisé doit être accessible par le client afin que l’application pour recevoir une demande non sécurisée et rediriger vers le port sécurisé.
>
> N’importe quel pare-feu entre le client et le serveur doit également les ports sont ouverts pour le trafic.
>
> Pour plus d’informations, consultez [configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) ou <xref:fundamentals/servers/httpsys>.

Les appels de code en surbrillance suivantes [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Appel `AddHttpsRedirection` est uniquement nécessaire de modifier les valeurs de `HttpsPort` ou `RedirectStatusCode`.

Le code précédent en surbrillance :

* Jeux [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), qui est la valeur par défaut. Utilisez les champs de la [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) classe les affectations à `RedirectStatusCode`.
* Définit le port HTTPS par 5001. La valeur par défaut est 443.

Les mécanismes suivants définir automatiquement le port :

* Le middleware peut découvrir les ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) lorsque les conditions suivantes s’appliquent :

  * Kestrel ou HTTP.sys est utilisé directement avec les points de terminaison HTTPS (s’applique également à l’exécution de l’application avec le débogueur du Code Visual Studio).
  * Uniquement **un seul port HTTPS** est utilisé par l’application.

* Visual Studio est utilisé :

  * IIS Express a HTTPS activé.
  * *launchSettings.json* définit le `sslPort` pour IIS Express.

> [!NOTE]
> Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible. Le port doit être configuré manuellement. Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.

Le port peut être configuré en définissant le [paramètre de configuration d’hôte Web https_port](xref:fundamentals/host/web-host#https-port):

**Clé**: https_port  
**Type** : *string*  
**Par défaut**: une valeur par défaut n’est pas définie.  
**Définition avec** : `UseSetting`  
**Variable d’environnement**: `<PREFIX_>HTTPS_PORT` (le préfixe est `ASPNETCORE_` lors de l’utilisation de l’hôte Web.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Le port peut être configuré indirectement en définissant l’URL avec le `ASPNETCORE_URLS` variable d’environnement. La variable d’environnement configure le serveur, et détecte ensuite l’intergiciel (middleware) indirectement les le port HTTPS par le biais de `IServerAddressesFeature`.

Si aucun port n’est définie :

* Les requêtes ne sont pas redirigées.
* L’intergiciel (middleware) consigne l’avertissement « Échoué pour déterminer le port https pour la redirection ».

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

Par [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de sécurité à accepter qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse. Quand un [navigateur qui prend en charge HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) reçoit cet en-tête :

* Le navigateur stocke la configuration pour le domaine qui empêche l’envoi de toutes les communications via HTTP. Le navigateur force toutes les communications via le protocole HTTPS.
* Le navigateur empêche l’utilisateur de l’utilisation de certificats non approuvés ou non valides. Le navigateur désactive les invites qui permettent à un utilisateur de confiance à ce certificat.

Étant donné que HSTS est appliquée par le client, il présente certaines limitations :

* Le client doit prendre en charge HSTS.
* HSTS nécessite au moins une demande HTTPS réussie pour établir la stratégie HSTS.
* L’application doit vérifier chaque requête HTTP et rediriger ou rejeter la demande HTTP.

ASP.NET Core 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension. Le code suivant appelle `UseHsts` lorsque l’application n’est pas dans [mode de développement](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` n’est pas recommandée dans le développement, car les paramètres HSTS sont hautement mis en cache par les navigateurs. Par défaut, `UseHsts` exclut l’adresse de bouclage local.

Pour les environnements de production mise en œuvre HTTPS pour la première fois, définissez la valeur HSTS initiale sur une valeur faible. Définissez la valeur des heures non plus d’une seule journée au cas où vous deviez restaurer l’infrastructure HTTPS vers HTTP. Une fois que vous êtes ainsi certain de la durabilité de la configuration de HTTPS, augmentez la valeur de max-age HSTS ; une valeur couramment utilisée est un an. 

L'exemple de code suivant :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Définit le paramètre de préchargement de l’en-tête Strict-Transport-Security. Préchargement ne fait pas partie de la [spécification de la RFC HSTS](https://tools.ietf.org/html/rfc6797), mais est pris en charge par les navigateurs web pour précharger des sites HSTS sur la nouvelle installation. Pour plus d’informations, consultez [https://hstspreload.org/](https://hstspreload.org/).
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

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Annulations de HTTPS/HSTS sur la création du projet

Dans certains scénarios de service back-end où la sécurité de la connexion est gérée en périphérie du réseau destinées au public, configuration de sécurité de la connexion au niveau de chaque nœud n’est pas nécessaire. Généré à partir des modèles dans Visual Studio ou à partir des applications Web le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande Activer [la redirection HTTPS](#require) et [HSTS](#hsts). Pour les déploiements ne nécessitant pas de ces scénarios, vous pouvez adhérer à de HTTPS/HSTS lorsque l’application est créée à partir du modèle.

Pour adhérer à de HTTPS/HSTS :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Désactivez la case à cocher **configurer pour le protocole HTTPS**.

![Diagramme des entités](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli) 

Utilisez l'option `--no-https`. Exemple :

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Comment configurer un certificat de développeur pour Docker

Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Informations supplémentaires

* [Prise en charge des navigateurs OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
