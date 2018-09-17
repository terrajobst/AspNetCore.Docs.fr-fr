---
title: Métapackage Microsoft.AspNetCore.App pour ASP.NET Core 2.1 et ultérieur
author: Rick-Anderson
description: Le métapackage Microsoft.AspNetCore.App comprend tous les packages ASP.NET Core et Entity Framework Core pris en charge.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: d27c3aa53d6edd235006dc136f09558395e15b6e
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538451"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Métapackage Microsoft.AspNetCore.App pour ASP.NET Core 2.1

Cette fonctionnalité nécessite ASP.NET Core 2.1 et ultérieur ciblant .NET Core 2.1 et ultérieur.

Le [métapackage ](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) pour ASP.NET Core :

* N’inclut pas les dépendances tierces sauf pour [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) et [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Ces dépendances tierces sont considérées comme nécessaires pour assurer le fonctionnement des principales fonctionnalités des frameworks.
* Inclut tous les packages pris en charge par l’équipe ASP.NET Core, sauf ceux qui contiennent des dépendances tierces (autres que celles mentionnées au-dessus).
* Inclut tous les packages pris en charge par l’équipe Entity Framework Core, sauf ceux qui contiennent des dépendances tierces (autres que celles mentionnées au-dessus).

Toutes les fonctionnalités ASP.NET Core 2.1 et ultérieur et Entity Framework Core 2.1 et ultérieur sont incluses dans le package `Microsoft.AspNetCore.App`. Les modèles de projet par défaut ciblant ASP.NET Core 2.1 et ultérieur utilisent ce package. Nous recommandons que les applications ciblant ASP.NET Core 2.1 et ultérieur et Entity Framework Core 2.1 et ultérieur utilisent le package `Microsoft.AspNetCore.App`.

Le numéro de version du métapackage `Microsoft.AspNetCore.App` représente la version d’ASP.NET Core et la version d’Entity Framework Core.

L’utilisation du métapackage `Microsoft.AspNetCore.App` offre des restrictions de version qui protègent votre application :

* Si un package inclus a une dépendance transitive (non directe) sur un package dans `Microsoft.AspNetCore.App` et que ces numéros de version diffèrent, NuGet génère une erreur.
* Les autres packages ajoutés à votre application ne peuvent pas changer la version des packages inclus dans `Microsoft.AspNetCore.App`.
* La cohérence des versions garantit une expérience fiable. `Microsoft.AspNetCore.App` a été conçu pour empêcher des combinaisons de versions non testées de bits associés d’être utilisées ensemble dans la même application.

Les applications qui utilisent le métapackage `Microsoft.AspNetCore.App` tirent automatiquement parti du framework partagé ASP.NET Core. Quand vous utilisez le métapackage `Microsoft.AspNetCore.App`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application : le framework partagé ASP.NET Core contient ces ressources. Les ressources présentes dans le framework partagé sont précompilées afin d’améliorer la vitesse de démarrage des applications. Pour plus d’informations, consultez « framework partagé » dans [Empaquetage de la distribution de .NET Core](/dotnet/core/build/distribution-packaging).

Le fichier de projet suivant référence le métapackage `Microsoft.AspNetCore.App` pour ASP.NET Core et représente un modèle ASP.NET Core 2.1 standard :

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.4" />
  </ItemGroup>

</Project>
```

Le numéro de version sur la référence `Microsoft.AspNetCore.App` ne garantit **pas** que la version du framework partagé sera utilisée. Par exemple, supposons que la version `2.1.1` est spécifiée, mais que la version `2.1.3` est installée. Dans ce cas, l’application utilise `2.1.3`. Même si ce n’est pas recommandé, vous pouvez désactiver le comportement de restauration par progression (correctif et/ou mineur). Pour plus d’informations sur le comportement de restauration par progression de la version de package, consultez [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

## <a name="update-aspnet-core"></a>Mettre à jour ASP.NET Core

Le [métapackage](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` n’est pas un package traditionnel qui est mis à jour à partir de NuGet. Semblable à `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` représente un runtime partagé avec une sémantique de version spéciale gérée en dehors de NuGet. Pour plus d’informations, consultez [Packages, métapackages et frameworks](/dotnet/core/packages).

Pour mettre à jour ASP.NET Core :

* Sur les machines de développement et les serveurs de builds, téléchargez et installez le [SDK .NET Core](https://www.microsoft.com/net/download).
* Sur les serveurs de déploiement, téléchargez et installez le [Runtime .NET Core](https://www.microsoft.com/net/download).

 Les applications seront restaurées par progression jusqu’à la version installée la plus récente au moment de leur redémarrage. Vous n’avez pas besoin de mettre à jour le numéro de version `Microsoft.AspNetCore.App` dans le fichier projet. Pour plus d’informations, consultez [Restaurer par progression des applications dépendantes du framework](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Si votre application utilisait `Microsoft.AspNetCore.All`, consultez [Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
