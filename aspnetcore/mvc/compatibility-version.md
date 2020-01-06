---
title: Version de compatibilité pour ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment la classe Startup dans ASP.NET Core configure des services et le pipeline de requête de l’application.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 9/25/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b29e2ee49aaf0f557f1acd0cf03e9e82d5ea0105
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75357729"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Version de compatibilité pour ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

La méthode <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> est une absence d’opération pour ASP.NET Core applications 3,0. Autrement dit, l’appel de `SetCompatibilityVersion` avec n’importe quelle valeur de <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion> n’a aucun impact sur l’application.

* La version mineure suivante de ASP.NET Core peut fournir une nouvelle valeur de `CompatibilityVersion`.
* les valeurs de `CompatibilityVersion` `Version_2_0` par `Version_2_2` sont marquées `[Obsolete(...)]`.
* Consultez [rupture des modifications d’API dans anti-contrefaçon, cors, diagnostics, MVC et routage](https://github.com/aspnet/Announcements/issues/387). Cette liste comprend les modifications avec rupture pour les commutateurs de compatibilité.

Pour voir comment `SetCompatibilityVersion` fonctionne avec ASP.NET Core applications 2. x, sélectionnez la [version ASP.NET Core 2,2 de cet article](https://docs.microsoft.com/aspnet/core/mvc/compatibility-version?view=aspnetcore-2.2).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

La méthode <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> permet à une application ASP.NET Core 2. x d’accepter ou de refuser les modifications de comportement potentiellement en rupture introduites dans ASP.NET Core MVC 2,1 ou 2,2. Ces changements de comportement potentiellement cassants concernent en général la façon dont le sous-système MVC se comporte et la façon dont **votre code** est appelé par le runtime. En acceptant, vous obtenez le comportement le plus récent et le comportement à long terme d’ASP.NET Core.

Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.2 :

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

Nous vous recommandons de tester votre application avec la version la plus récente (`CompatibilityVersion.Latest`). Nous pensons que la plupart des applications ne connaîtront pas de changements de comportement cassants avec la version la plus récente.

Les applications qui appellent des `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sont protégées contre les changements de comportement potentiellement inaltérables introduits dans les versions ASP.NET Core 2.1/2.2 MVC. Cette protection :

* Ne s’applique pas à tous les changements de 2.1 et ultérieur. Elle est destinée aux changements de comportement potentiellement cassants du runtime ASP.NET Core dans le sous-système MVC.
* Ne s’étend pas à ASP.NET Core 3,0.

La compatibilité par défaut pour les applications ASP.NET Core 2,1 et 2,2 qui n’appellent **pas** `SetCompatibilityVersion` est 2,0. Autrement dit, ne pas appeler `SetCompatibilityVersion` revient à appeler `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.2, sauf pour les comportements suivants :

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Pour les applications qui rencontrent des changements de comportement cassants, l’utilisation des commutateurs de compatibilité appropriés :

* Vous permet d’utiliser la dernière version et de refuser des changements de comportement cassants spécifiques.
* Vous laisse le temps de mettre à jour votre application pour qu’elle fonctionne avec les derniers changements.

La documentation <xref:Microsoft.AspNetCore.Mvc.MvcOptions> explique clairement ce qui a changé et pourquoi ces changements représentent une amélioration pour la plupart des utilisateurs.

Avec ASP.NET Core 3,0, les anciens comportements pris en charge par les commutateurs de compatibilité ont été supprimés. Nous pensons que ce sont des changements positifs, qui vont bénéficier à presque tous les utilisateurs. En introduisant ces modifications dans 2,1 et 2,2, la plupart des applications peuvent bénéficier de la mise à jour, tandis que d’autres ont du temps.
::: moniker-end
