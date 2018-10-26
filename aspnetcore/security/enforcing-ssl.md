---
title: Appliquer HTTPS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment exiger HTTPS/TLS dans une application web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: security/enforcing-ssl
ms.openlocfilehash: a5359fe49e71ab59b47a8a5a39e7b806ad308235
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090973"
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

## <a name="require-https"></a>Exiger s-http

::: moniker range=">= aspnetcore-2.1"

Nous recommandons que la production ASP.NET Core appel d’applications web :

* Intergiciel (middleware) la Redirection du protocole HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) pour rediriger les requêtes HTTP vers HTTPS.
* Intergiciel (middleware) HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) pour envoyer des en-têtes de protocole HTTP Strict Transport Security (HSTS) aux clients.

> [!NOTE]
> Les applications déployées dans une configuration de proxy inverse autoriser le proxy à gérer la sécurité de connexion (HTTPS). Si le proxy gère également la redirection HTTPS, il est inutile d’utiliser l’intergiciel (middleware) la Redirection de HTTPS. Si le serveur proxy gère également l’écriture des en-têtes HSTS (par exemple, [HSTS natifs prend en charge dans IIS 10.0 (1709) ou version ultérieure](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS intergiciel (middleware) n’est pas requis par l’application. Pour plus d’informations, consultez [de refus de HTTPS/HSTS sur la création du projet](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Le code précédent en surbrillance :

* Utilise la valeur par défaut [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Utilise la valeur par défaut [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), sauf substitution par le `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Nous vous recommandons d’utiliser des redirections temporaires au lieu des redirections permanentes. Lien de mise en cache peut provoquer un comportement instable dans les environnements de développement. Si vous préférez envoyer un code d’état de redirection permanente lorsque l’application est dans un environnement non liées au développement, consultez le [configurer des redirections permanentes en production](#configure-permanent-redirects-in-production) section. Nous vous recommandons d’utiliser [HSTS](#http-strict-transport-security-protocol-hsts) pour signaler aux clients qui sécurisent uniquement les ressources des demandes doivent être envoyés à l’application (uniquement en production).

### <a name="port-configuration"></a>Configuration du port

Un port doit être disponible pour l’intergiciel (middleware) pour rediriger une requête non sécurisée vers HTTPS. Si aucun port n’est disponible :

* Redirection vers HTTPS ne se produit.
* L’intergiciel (middleware) consigne l’avertissement « Échoué pour déterminer le port https pour la redirection ».

Spécifiez le port HTTPS en utilisant l’une des approches suivantes :

* Définissez [HttpsRedirectionOptions.HttpsPort](#options).
* Définir le `ASPNETCORE_HTTPS_PORT` variable d’environnement ou [paramètre de configuration d’hôte Web https_port](xref:fundamentals/host/web-host#https-port):

  **Clé**: `https_port`  
  **Type** : *string*  
  **Par défaut**: une valeur par défaut n’est pas définie.  
  **Définition avec** : `UseSetting`  
  **Variable d’environnement**: `<PREFIX_>HTTPS_PORT` (le préfixe est `ASPNETCORE_` lorsque vous utilisez le [hôte Web](xref:fundamentals/host/web-host).)

  Lorsque vous configurez un <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> dans `Program`:

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* Indiquer un port avec le schéma sécurisé à l’aide de la `ASPNETCORE_URLS` variable d’environnement. La variable d’environnement configure le serveur. L’intergiciel (middleware) détecte indirectement le port HTTPS par le biais de <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>. (Est **pas** Professionnel dans les déploiements de proxy inverse.)
* Dans le développement, définir une URL HTTPS dans *launchsettings.json*. Activer le protocole HTTPS lorsque IIS Express est utilisé.
* Configurer un point de terminaison URL HTTPS pour un déploiement de périphérie destinées au public de [Kestrel](xref:fundamentals/servers/kestrel) ou [HTTP.sys](xref:fundamentals/servers/httpsys). Uniquement **un seul port HTTPS** est utilisé par l’application. L’intergiciel (middleware) détecte le port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.

> [!NOTE]
> Quand une application s’exécute derrière un proxy inverse (par exemple, IIS, IIS Express), `IServerAddressesFeature` n’est pas disponible. Le port doit être configuré manuellement. Lorsque le port n’est pas défini, les demandes ne sont pas redirigées.

Quand Kestrel ou HTTP.sys est utilisée comme un serveur edge de public, Kestrel ou HTTP.sys doit être configuré pour écouter sur les deux :

* Le port sécurisé où le client est redirigé (en règle générale, 443 dans 5001 dans le développement et de production).
* Le port non sécurisé (en règle générale, 80 en production) et 5000 dans le développement.

Le port non sécurisé doit être accessible par le client afin que l’application pour recevoir une requête non sécurisée et de rediriger le client vers le port sécurisé.

Pour plus d’informations, consultez [configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) ou <xref:fundamentals/servers/httpsys>.

### <a name="deployment-scenarios"></a>Scénarios de déploiement

N’importe quel pare-feu entre le client et le serveur doit également communication ports sont ouverts pour le trafic.

Si les demandes sont transmises dans une configuration de proxy inverse, utilisez [intergiciel des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) avant d’appeler intergiciel (middleware) la Redirection de HTTPS. Transféré les mises à jour de l’intergiciel des en-têtes le `Request.Scheme`, en utilisant le `X-Forwarded-Proto` en-tête. Les autorisations de l’intergiciel (middleware) rediriger URI et autres stratégies de sécurité fonctionne correctement. Lors de l’intergiciel des en-têtes transférés n’est pas utilisé, l’application back-end ne peut pas recevoir le schéma correct et finir dans une boucle de redirection. Un message d’erreur utilisateur final commun est que trop de redirections ont eu lieu.

Lorsque vous déployez sur Azure App Service, suivez les instructions de [didacticiel : lier un certificat SSL personnalisé existant à Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Options

Les appels de code en surbrillance suivantes [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) pour configurer les options de l’intergiciel (middleware) :

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Appel `AddHttpsRedirection` est uniquement nécessaire de modifier les valeurs de `HttpsPort` ou `RedirectStatusCode`.

Le code précédent en surbrillance :

* Jeux [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) à <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, qui est la valeur par défaut. Utilisez les champs de la <xref:Microsoft.AspNetCore.Http.StatusCodes> classe les affectations à `RedirectStatusCode`.
* Définit le port HTTPS par 5001. La valeur par défaut est 443.

#### <a name="configure-permanent-redirects-in-production"></a>Configurer des redirections permanentes en production

Valeur par défaut est de l’intergiciel (middleware) d’envoyer un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) avec toutes les redirections. Si vous préférez envoyer un code d’état de redirection permanente lorsque l’application est dans un environnement non liées au développement, encapsulez la configuration des options intergiciel (middleware) dans un contrôle conditionnel pour un environnement de développement non.

Lorsque vous configurez un `IWebHostBuilder` dans *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a>Intergiciel (middleware) de HTTPS Redirection autre approche

Une alternative à l’utilisation de HTTPS Redirection Middleware (`UseHttpsRedirection`) consiste à utiliser l’intergiciel de réécriture d’URL (`AddRedirectToHttps`). `AddRedirectToHttps` peut également définir le code d’état et le port lors de l’exécution de la redirection. Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).

Lors de la redirection vers HTTPS sans la nécessité pour les règles de redirection supplémentaire, nous vous recommandons d’utiliser HTTPS Redirection Middleware (`UseHttpsRedirection`) décrits dans cette rubrique.

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

Pour les environnements de production mise en œuvre HTTPS pour la première fois, définissez initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) sur une valeur faible à l’aide d’une de la <xref:System.TimeSpan> méthodes. Définissez la valeur des heures non plus d’une seule journée au cas où vous deviez restaurer l’infrastructure HTTPS vers HTTP. Une fois que vous êtes ainsi certain de la durabilité de la configuration de HTTPS, augmentez la valeur de max-age HSTS ; une valeur couramment utilisée est un an.

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

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Annulations de HTTPS/HSTS sur la création du projet

Dans certains scénarios de service back-end où la sécurité de la connexion est gérée en périphérie du réseau destinées au public, configuration de sécurité de la connexion au niveau de chaque nœud n’est pas nécessaire. Généré à partir des modèles dans Visual Studio ou à partir des applications Web le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande Activer [la redirection HTTPS](#require-https) et [HSTS](#http-strict-transport-security-protocol-hsts). Pour les déploiements ne nécessitant pas de ces scénarios, vous pouvez adhérer à de HTTPS/HSTS lorsque l’application est créée à partir du modèle.

Pour adhérer à de HTTPS/HSTS :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Désactivez la case à cocher **configurer pour le protocole HTTPS**.

![Nouvelle Application Web ASP.NET Core boîte de dialogue affichant la configurer pour la case à cocher HTTPS non sélectionné.](enforcing-ssl/_static/out.png)

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

* <xref:host-and-deploy/proxy-load-balancer>
* [Héberger ASP.NET Core sur Linux avec Apache : configuration de SSL](xref:host-and-deploy/linux-apache#ssl-configuration)
* [Héberger ASP.NET Core sur Linux avec Nginx : configuration de SSL](xref:host-and-deploy/linux-nginx#configure-ssl)
* [Procédure pour configurer SSL sur IIS](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [Prise en charge des navigateurs OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
