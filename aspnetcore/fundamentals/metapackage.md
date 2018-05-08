---
title: Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.x et ultérieur
author: Rick-Anderson
description: Le métapackage Microsoft.AspNetCore.All comprend tous les packages ASP.NET Core et Entity Framework Core, ainsi que leurs dépendances.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Métapackage Microsoft.AspNetCore.All pour ASP.NET Core 2.x

Cette fonctionnalité nécessite ASP.NET Core 2.x ciblant .NET Core 2.x.

Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :

* Tous les packages pris en charge par l’équipe ASP.NET Core.
* Tous les packages pris en charge par Entity Framework Core. 
* Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core. 

Toutes les fonctionnalités d’ASP.NET Core 2.x et d’Entity Framework Core 2.x sont incluses dans le package `Microsoft.AspNetCore.All`. Les modèles de projet par défaut ciblant ASP.NET Core 2.0 utilisent ce package.

Le numéro de version du métapackage `Microsoft.AspNetCore.All` représente la version d’ASP.NET Core et la version d’Entity Framework Core (alignées sur la version de .NET Core).

Les applications qui utilisent le métapackage `Microsoft.AspNetCore.All` tirent automatiquement parti du [magasin de packages de runtime de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Ce magasin Runtime contient toutes les ressources runtime nécessaires à l’exécution des applications ASP.NET Core 2.x. Quand vous utilisez le métapackage `Microsoft.AspNetCore.All`, **aucune** des ressources des packages NuGet ASP.NET Core référencés n’est déployée avec l’application. En effet, le magasin Runtime de .NET Core contient ces ressources. Les ressources présentes dans le magasin Runtime sont précompilées afin d’améliorer la vitesse de démarrage des applications.

Vous pouvez utiliser le processus de suppression de package pour supprimer les packages que vous n’utilisez pas. Les packages supprimés sont exclus de la sortie d’application publiée.

Le fichier *.csproj* suivant fait référence au métapackage `Microsoft.AspNetCore.All` pour ASP.NET Core :

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
