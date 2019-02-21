---
title: Version de compatibilité pour ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment la classe Startup dans ASP.NET Core configure des services et le pipeline de requête de l’application.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: 7c4189db435088e0803b35add82fa0eb9372e664
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410143"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="86617-103">Version de compatibilité pour ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="86617-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="86617-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86617-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="86617-105">La méthode <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> permet à une application d’accepter ou de refuser les changements de comportement potentiellement cassants introduits dans ASP.NET Core MVC 2.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="86617-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="86617-106">Ces changements de comportement potentiellement cassants concernent en général la façon dont le sous-système MVC se comporte et la façon dont **votre code** est appelé par le runtime.</span><span class="sxs-lookup"><span data-stu-id="86617-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="86617-107">En acceptant, vous obtenez le comportement le plus récent et le comportement à long terme d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="86617-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="86617-108">Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.2 :</span><span class="sxs-lookup"><span data-stu-id="86617-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="86617-109">Nous vous recommandons de tester votre application avec la version la plus récente (`CompatibilityVersion.Version_2_2`).</span><span class="sxs-lookup"><span data-stu-id="86617-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="86617-110">Nous pensons que la plupart des applications ne connaîtront pas de changements de comportement cassants avec la version la plus récente.</span><span class="sxs-lookup"><span data-stu-id="86617-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="86617-111">Les applications qui appellent `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sont protégées contre les changements de comportement potentiellement cassants qui ont été introduits dans ASP.NET Core 2.1 MVC et dans les versions 2.x ultérieures.</span><span class="sxs-lookup"><span data-stu-id="86617-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="86617-112">Cette protection :</span><span class="sxs-lookup"><span data-stu-id="86617-112">This protection:</span></span>

* <span data-ttu-id="86617-113">Ne s’applique pas à tous les changements de 2.1 et ultérieur. Elle est destinée aux changements de comportement potentiellement cassants du runtime ASP.NET Core dans le sous-système MVC.</span><span class="sxs-lookup"><span data-stu-id="86617-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="86617-114">Ne s’étend pas à la version majeure suivante.</span><span class="sxs-lookup"><span data-stu-id="86617-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="86617-115">La compatibilité par défaut pour les applications ASP.NET Core 2.1 et les versions 2.x ultérieures qui n’appellent **pas** `SetCompatibilityVersion` est la compatibilité 2.0.</span><span class="sxs-lookup"><span data-stu-id="86617-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="86617-116">Autrement dit, ne pas appeler `SetCompatibilityVersion` revient à appeler `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="86617-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="86617-117">Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.2, sauf pour les comportements suivants :</span><span class="sxs-lookup"><span data-stu-id="86617-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="86617-118">Pour les applications qui rencontrent des changements de comportement cassants, l’utilisation des commutateurs de compatibilité appropriés :</span><span class="sxs-lookup"><span data-stu-id="86617-118">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="86617-119">Vous permet d’utiliser la dernière version et de refuser des changements de comportement cassants spécifiques.</span><span class="sxs-lookup"><span data-stu-id="86617-119">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="86617-120">Vous laisse le temps de mettre à jour votre application pour qu’elle fonctionne avec les derniers changements.</span><span class="sxs-lookup"><span data-stu-id="86617-120">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="86617-121">Les commentaires dans la source de la classe [MvcOptions](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Mvc/Mvc.Core/src/MvcOptions.cs) expliquent clairement ce qui a changé et pourquoi les changements représentent une amélioration pour la plupart des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="86617-121">The [MvcOptions](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Mvc/Mvc.Core/src/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="86617-122">À une date ultérieure, il y aura une [version d’ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="86617-122">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="86617-123">Les comportements anciens pris en charge par les commutateurs de compatibilité seront supprimés dans la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="86617-123">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="86617-124">Nous pensons que ce sont des changements positifs, qui vont bénéficier à presque tous les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="86617-124">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="86617-125">Comme nous introduisons ces changements maintenant, la plupart des applications peuvent en profiter tout de suite. Pour les autres applications, les développeurs ont du temps pour les mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="86617-125">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
