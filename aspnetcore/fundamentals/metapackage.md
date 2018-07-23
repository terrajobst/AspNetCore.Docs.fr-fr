---
title: Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0
author: Rick-Anderson
description: Le métapackage Microsoft.AspNetCore.All comprend tous les packages ASP.NET Core et Entity Framework Core, ainsi que leurs dépendances.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123825"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.0

> [!NOTE]
> Nous recommandons que les applications qui ciblent ASP.NET Core 2.1 et ultérieur utilisent [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) au lieu de ce package. Consultez [Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App](#migrate) dans cet article.

Cette fonctionnalité nécessite ASP.NET Core 2.x ciblant .NET Core 2.x.

Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :

* Tous les packages pris en charge par l’équipe ASP.NET Core.
* Tous les packages pris en charge par Entity Framework Core.
* Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.

Toutes les fonctionnalités d’ASP.NET Core 2.x et d’Entity Framework Core 2.x sont incluses dans le package `Microsoft.AspNetCore.All`. Les modèles de projet par défaut ciblant ASP.NET Core 2.0 utilisent ce package.

Le numéro de version du métapackage `Microsoft.AspNetCore.All` représente la version d’ASP.NET Core et la version d’Entity Framework Core.

Les applications qui utilisent le métapackage `Microsoft.AspNetCore.All` tirent automatiquement parti du [magasin de packages de runtime de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Ce magasin Runtime contient toutes les ressources runtime nécessaires à l’exécution des applications ASP.NET Core 2.x. Quand vous utilisez le métapackage `Microsoft.AspNetCore.All`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application. En effet, le magasin Runtime de .NET Core contient ces ressources. Les ressources présentes dans le magasin Runtime sont précompilées afin d’améliorer la vitesse de démarrage des applications.

Vous pouvez utiliser le processus de suppression de package pour supprimer les packages que vous n’utilisez pas. Les packages supprimés sont exclus de la sortie d’application publiée.

Le fichier *.csproj* suivant fait référence au métapackage `Microsoft.AspNetCore.All` pour ASP.NET Core :

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

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
