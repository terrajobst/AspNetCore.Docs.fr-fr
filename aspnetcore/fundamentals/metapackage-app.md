---
title: Métapackage Microsoft.AspNetCore.App pour ASP.NET Core 2.1 et ultérieur
author: Rick-Anderson
description: Le métapackage Microsoft.AspNetCore.App comprend tous les packages ASP.NET Core et Entity Framework Core pris en charge.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage-app
ms.openlocfilehash: 7c7f69a6176d3f7982a67106cb823ff42200b50e
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/18/2018
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

Les applications qui utilisent le métapackage `Microsoft.AspNetCore.App` tirent automatiquement parti du framework partagé ASP.NET Core. Quand vous utilisez le métapackage `Microsoft.AspNetCore.App`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application &mdash; le framework partagé ASP.NET Core contient ces ressources. Les ressources présentes dans le framework partagé sont précompilées afin d’améliorer la vitesse de démarrage des applications. Pour plus d’informations, consultez « framework partagé » dans [Empaquetage de la distribution de .NET Core](/dotnet/core/build/distribution-packaging).

Le fichier projet *.csproj* suivant référence le métapackage `Microsoft.AspNetCore.App` pour ASP.NET Core :

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

La balise précédente représente un modèle ASP.NET Core 2.1 et ultérieur typique. Il ne spécifie pas de numéro de version pour la référence du package `Microsoft.AspNetCore.App`. Lorsque la version n’est pas spécifiée, une version [implicite](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) est spécifiée par le kit SDK, autrement dit, `Microsoft.NET.Sdk.Web`. Nous vous recommandons de garder la version implicite spécifiée par le kit SDK et de ne pas définir de façon explicite le numéro de version sur la référence de package. Vous pouvez laisser un commentaire GitHub dans [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).

La version implicite est définie sur `major.minor.0` pour les applications portables. Le mécanisme de restauration par progression des frameworks partagés exécute l’application sur la dernière version compatible parmi les frameworks partagés installés. Pour garantir l’utilisation de la même version en développement, test et production, vérifiez que la même version du framework partagé est installée dans tous les environnements. Pour les applications autonomes, le numéro de version implicite est défini sur le `major.minor.patch` du framework partagé inclus dans le kit SDK installé.

La spécification d’un numéro de version sur la référence `Microsoft.AspNetCore.App` ne garantit **pas** que la version du framework partagé sera choisie. Par exemple, supposons que la version « 2.1.1 » est spécifiée, mais que la version « 2.1.3 » est installée. Dans ce cas, l’application utilise « 2.1.3 ». Même si ce n’est pas recommandé, vous pouvez désactiver la restauration par progression (correctif et/ou mineur). Pour plus d’informations sur la restauration par progression de l’hôte dotnet et sur la configuration de son comportement, consultez [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Le [métapackage](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` n’est pas un package traditionnel qui est mis à jour à partir de NuGet. Semblable à `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` représente un runtime partagé avec une sémantique de version spéciale gérée en dehors de NuGet. Pour plus d’informations, consultez [Packages, métapackages et frameworks](/dotnet/core/packages).

Si votre application utilisait `Microsoft.AspNetCore.All`, consultez [Migration de Microsoft.AspNetCore.All vers Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
