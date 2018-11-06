---
title: Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0
author: Rick-Anderson
description: Le métapackage Microsoft.AspNetCore.All n’est pas recommandé pour ASP.NET Core 2.1 et versions ultérieures.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148848"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0

> [!NOTE]
> Nous recommandons que les applications qui ciblent ASP.NET Core 2.1 et ultérieur utilisent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) au lieu de ce package. Consultez [Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App](#migrate) dans cet article.

Cette fonctionnalité nécessite ASP.NET Core 2.x ciblant .NET Core 2.x.

Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :

* Tous les packages pris en charge par l’équipe ASP.NET Core.
* Tous les packages pris en charge par Entity Framework Core.
* Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.

Toutes les fonctionnalités d’ASP.NET Core 2.x et d’Entity Framework Core 2.x sont incluses dans le package `Microsoft.AspNetCore.All`. Les modèles de projet par défaut ciblant ASP.NET Core 2.0 utilisent ce package.

Le numéro de version du métapackage `Microsoft.AspNetCore.All` représente la version d’ASP.NET Core et la version d’Entity Framework Core.

Les applications qui utilisent le métapackage `Microsoft.AspNetCore.All` tirent automatiquement parti du [magasin de packages de runtime de .NET Core](/dotnet/core/deploying/runtime-store). Ce magasin Runtime contient toutes les ressources runtime nécessaires à l’exécution des applications ASP.NET Core 2.x. Quand vous utilisez le métapackage `Microsoft.AspNetCore.All`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application. En effet, le magasin Runtime de .NET Core contient ces ressources. Les ressources présentes dans le magasin Runtime sont précompilées afin d’améliorer la vitesse de démarrage des applications.

Vous pouvez utiliser le processus de suppression de package pour supprimer les packages que vous n’utilisez pas. Les packages supprimés sont exclus de la sortie d’application publiée.

Le fichier *.csproj* suivant fait référence au métapackage `Microsoft.AspNetCore.All` pour ASP.NET Core :

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Gestion des versions implicites

Dans ASP.NET Core 2.1 ou ultérieur, vous pouvez spécifier la référence de package `Microsoft.AspNetCore.All` sans version. Lorsque la version n’est pas spécifiée, une version implicite est spécifiée par le SDK (`Microsoft.NET.Sdk.Web`). Nous vous recommandons de garder la version implicite spécifiée par le kit SDK et de ne pas définir de façon explicite le numéro de version sur la référence de package. Si vous avez des questions au sujet de cette approche, envoyez vos commentaires GitHub dans la [Discussion concernant la version implicite de Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).

La version implicite est définie sur `major.minor.0` pour les applications portables. Le mécanisme de restauration par progression des frameworks partagés exécute l’application sur la dernière version compatible parmi les frameworks partagés installés. Pour garantir l’utilisation de la même version en développement, test et production, vérifiez que la même version du framework partagé est installée dans tous les environnements. Pour les applications autonomes, le numéro de version implicite est défini sur le `major.minor.patch` du framework partagé inclus dans le SDK installé.

La spécification d’un numéro de version sur la référence de package `Microsoft.AspNetCore.All` ne garantit **pas** que la version du framework partagé sera choisie. Par exemple, supposons que la version « 2.1.1 » est spécifiée, mais que la version « 2.1.3 » est installée. Dans ce cas, l’application utilise « 2.1.3 ». Même si ce n’est pas recommandé, vous pouvez désactiver la restauration par progression (correctif et/ou mineur). Pour plus d’informations sur la restauration par progression de l’hôte dotnet et sur la configuration de son comportement, consultez [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Le SDK du projet doit être défini sur `Microsoft.NET.Sdk.Web` dans le fichier projet pour utiliser la version implicite de `Microsoft.AspNetCore.All`. Lorsque le SDK `Microsoft.NET.Sdk` est spécifié (`<Project Sdk="Microsoft.NET.Sdk">` en haut du fichier projet), l’avertissement suivant est généré :

*Avertissement NU1604 : La dépendance du projet Microsoft.AspNetCore.All ne contient pas de limite inférieure inclusive. Incluez une limite inférieure dans la version de dépendance pour assurer des résultats de restauration cohérents.*

C’est un problème connu avec le kit SDK .NET Core 2.1 ; il sera corrigé dans le kit SDK .NET Core 2.2.

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App

Les packages suivants sont inclus dans `Microsoft.AspNetCore.All` mais pas le package `Microsoft.AspNetCore.App`.

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

Pour passer de `Microsoft.AspNetCore.All` à `Microsoft.AspNetCore.App`, si votre application utilise les API des packages ci-dessus, ou les packages apportés par ces packages, ajoutez des références à ces packages dans votre projet.

Toutes les dépendances des packages précédents qui ne sont pas des dépendances de `Microsoft.AspNetCore.App` ne sont pas incluses de manière implicite. Exemple :

* `StackExchange.Redis` comme dépendance de `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` comme dépendance de `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`

## <a name="update-aspnet-core-21"></a>Mettre à jour ASP.NET Core 2.1

Nous vous recommandons de migrer vers le métapackage `Microsoft.AspNetCore.App` pour 2.1 et versions ultérieures. Pour continuer à utiliser le métapackage `Microsoft.AspNetCore.All` et vérifier que la dernière version corrective est déployée :

* Sur les machines de développement et les serveurs de builds, installez le [kit SDK .NET Core](https://www.microsoft.com/net/download) le plus récent.
* Sur les serveurs de déploiement, installez le [Runtime .NET Core](https://www.microsoft.com/net/download) le plus récent.
 Votre application est restaurée par progression jusqu’à la version installée la plus récente au moment de son redémarrage.
