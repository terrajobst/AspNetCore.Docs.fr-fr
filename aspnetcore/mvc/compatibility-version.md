---
title: Version de compatibilité pour ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment la classe Startup dans ASP.NET Core configure des services et le pipeline de requête de l’application.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b360da105799a1dccb1902e167e50e78864b76a9
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647926"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Version de compatibilité pour ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

La méthode <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> permet à une application d’accepter ou de refuser les changements de comportement potentiellement cassants introduits dans ASP.NET Core MVC 2.1 ou version ultérieure. Ces changements de comportement potentiellement cassants concernent en général la façon dont le sous-système MVC se comporte et la façon dont **votre code** est appelé par le runtime. En acceptant, vous obtenez le comportement le plus récent et le comportement à long terme d’ASP.NET Core.

Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.2 :

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

Nous vous recommandons de tester votre application avec la version la plus récente (`CompatibilityVersion.Version_2_2`). Nous pensons que la plupart des applications ne connaîtront pas de changements de comportement cassants avec la version la plus récente.

Les applications qui appellent `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sont protégées contre les changements de comportement potentiellement cassants qui ont été introduits dans ASP.NET Core 2.1 MVC et dans les versions 2.x ultérieures. Cette protection :

* Ne s’applique pas à tous les changements de 2.1 et ultérieur. Elle est destinée aux changements de comportement potentiellement cassants du runtime ASP.NET Core dans le sous-système MVC.
* Ne s’étend pas à la version majeure suivante.

La compatibilité par défaut pour les applications ASP.NET Core 2.1 et les versions 2.x ultérieures qui n’appellent **pas** `SetCompatibilityVersion` est la compatibilité 2.0. Autrement dit, ne pas appeler `SetCompatibilityVersion` revient à appeler `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.2, sauf pour les comportements suivants :

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Pour les applications qui rencontrent des changements de comportement cassants, l’utilisation des commutateurs de compatibilité appropriés :

* Vous permet d’utiliser la dernière version et de refuser des changements de comportement cassants spécifiques.
* Vous laisse le temps de mettre à jour votre application pour qu’elle fonctionne avec les derniers changements.

La documentation <xref:Microsoft.AspNetCore.Mvc.MvcOptions> explique clairement ce qui a changé et pourquoi ces changements représentent une amélioration pour la plupart des utilisateurs.

À une date ultérieure, il y aura une [version d’ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). Les comportements anciens pris en charge par les commutateurs de compatibilité seront supprimés dans la version 3.0. Nous pensons que ce sont des changements positifs, qui vont bénéficier à presque tous les utilisateurs. Comme nous introduisons ces changements maintenant, la plupart des applications peuvent en profiter tout de suite. Pour les autres applications, les développeurs ont du temps pour les mettre à jour.
