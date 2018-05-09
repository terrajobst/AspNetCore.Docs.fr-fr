---
title: Appliquer HTTPS dans une base de ASP.NET
author: rick-anderson
description: Montre comment exiger HTTPS/TLS dans un cœur d’ASP.NET application web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: b324dbcd6d28c1a8505f96da333874728e2e6a18
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a>Appliquer HTTPS dans une base de ASP.NET

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce document montre comment :

- Exiger HTTPS pour toutes les demandes.
- Rediriger toutes les demandes HTTP vers HTTPS.

> [!WARNING]
> Faire **pas** utiliser `RequireHttpsAttribute` sur les API Web qui reçoivent des informations sensibles. `RequireHttpsAttribute` utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS. Les clients d’API ne peuvent pas comprendre ou obéissent aux règles de redirections HTTP vers HTTPS. Ces clients peuvent envoyer des informations sur HTTP. API Web doit soit :
>
>* Pas écouter sur HTTP.
>* Fermer la connexion avec le code d’état 400 (demande incorrecte) et pas desservir la demande.

<a name="require"></a>
## <a name="require-https"></a>Exiger HTTPS

::: moniker range=">= aspnetcore-2.1"
Nous vous recommandons de tous les principaux ASP.NET web applications appel `UseHttpsRedirection` pour rediriger toutes les demandes HTTP vers HTTPS. Si `UseHsts` est appelée dans l’application, il doit être appelé avant `UseHttpsRedirection`.

Le code suivant appelle `UseHttpsRedirection` dans la `Startup` classe :

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]


L'exemple de code suivant :

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

* Jeux de `RedirectStatusCode`.
* Définit le port HTTPS 5001.

::: moniker-end


::: moniker range="< aspnetcore-2.1"

Le [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) est utilisée pour exiger HTTPS. `[RequireHttpsAttribute]` peut décorer contrôleurs ou méthodes, ou peut être appliqué globalement. Pour appliquer l’attribut global, ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`: 

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Le code en surbrillance précédent requiert que toutes les demandes utilisent `HTTPS`; par conséquent, les requêtes HTTP sont ignorés. Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).

Exiger le protocol HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité. car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutées à votre application il faudra penser à appliquer le `[RequireHttps]` attribut. Vous ne pouvez pas garantir la `[RequireHttps]` attribut est appliqué lors de l’ajout de nouveaux contrôleurs et les Pages Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protocole de sécurité stricte de Transport HTTP (HSTS)

Par [OWASP avoir](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport sécurité (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) est une amélioration de l’inclusion de sécurité qui est spécifiée par une application web via l’utilisation d’un en-tête de réponse spécial. Une fois qu’un navigateur pris en charge reçoit cet en-tête ce navigateur empêche les communications d’être envoyés via HTTP dans le domaine spécifié et l’envoie à la place toutes les communications via le protocole HTTPS. Elle évite également les invites sur les navigateurs de clics HTTPS.

ASP.NET Core preview1 2.1 ou ultérieure implémente HSTS avec la `UseHsts` méthode d’extension. Le code suivant appelle `UseHsts` lorsque l’application n’est pas [mode de développement](xref:fundamentals/environments):

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` n’est pas recommandé dans le développement, car l’en-tête HSTS est hautement cachable par les navigateurs. Par défaut, UseHsts exclut l’adresse de bouclage locale.

L'exemple de code suivant :

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

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

Les ASP.NET Core 2.1 et les modèles d’application web ultérieure (à partir de Visual Studio ou la ligne de commande dotnet) permettent [la redirection HTTPS](#require) et [HSTS](#hsts). Pour les déploiements qui n’ont pas besoin de HTTPS, vous pouvez annuler le protocole HTTPS. Par exemple, certains services principaux où HTTPS est traitée en externe à la périphérie, à l’aide de HTTPS sur chaque nœud n’est pas nécessaire.

Pour s’en désengager de HTTPS :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Désactivez le **configurer pour le protocole HTTPS** case à cocher.

![Diagramme des entités](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli) 

Utilisez l'option `--no-https`. Exemple :

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a>La configuration d’un certificat de développeur pour Docker

Consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
