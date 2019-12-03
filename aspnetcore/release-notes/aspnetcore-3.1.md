---
title: Nouveautés de ASP.NET Core 3,1
author: rick-anderson
description: Découvrez les nouvelles fonctionnalités de ASP.NET Core 3,1.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: 4737747f84a59780fe70f63195f7580bd812e4de
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734025"
---
# <a name="whats-new-in-aspnet-core-31"></a><span data-ttu-id="2690f-103">Nouveautés de ASP.NET Core 3,1</span><span class="sxs-lookup"><span data-stu-id="2690f-103">What's new in ASP.NET Core 3.1</span></span>

<span data-ttu-id="2690f-104">Cet article met en évidence les modifications les plus importantes apportées à ASP.NET Core 3,1 avec des liens vers la documentation appropriée.</span><span class="sxs-lookup"><span data-stu-id="2690f-104">This article highlights the most significant changes in ASP.NET Core 3.1 with links to relevant documentation.</span></span>

## <a name="partial-class-support-for-razor-components"></a><span data-ttu-id="2690f-105">Prise en charge des classes partielles pour les composants Razor</span><span class="sxs-lookup"><span data-stu-id="2690f-105">Partial class support for Razor components</span></span>

<span data-ttu-id="2690f-106">Les composants Razor sont désormais générés en tant que classes partielles.</span><span class="sxs-lookup"><span data-stu-id="2690f-106">Razor components are now generated as partial classes.</span></span> <span data-ttu-id="2690f-107">Le code d’un composant Razor peut être écrit à l’aide d’un fichier code-behind défini en tant que classe partielle, plutôt que de définir tout le code du composant dans un fichier unique.</span><span class="sxs-lookup"><span data-stu-id="2690f-107">Code for a Razor component can be written using a code-behind file defined as a partial class rather than defining all the code for the component in a single file.</span></span> <span data-ttu-id="2690f-108">Pour plus d’informations, consultez [prise en charge des classes partielles](xref:blazor/components#partial-class-support).</span><span class="sxs-lookup"><span data-stu-id="2690f-108">For more information, see [Partial class support](xref:blazor/components#partial-class-support).</span></span>

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor<span data-ttu-id="2690f-109"> tag Helper de composant et passer des paramètres à des composants de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="2690f-109"> Component Tag Helper and pass parameters to top-level components</span></span>

<span data-ttu-id="2690f-110">Dans Blazor avec ASP.NET Core 3,0, les composants étaient rendus dans des pages et des vues à l’aide d’un programme d’assistance HTML (`Html.RenderComponentAsync`).</span><span class="sxs-lookup"><span data-stu-id="2690f-110">In Blazor with ASP.NET Core 3.0, components were rendered into pages and views using an HTML Helper (`Html.RenderComponentAsync`).</span></span> <span data-ttu-id="2690f-111">Dans ASP.NET Core 3,1, restituez un composant à partir d’une page ou d’une vue avec le nouveau tag Helper du composant :</span><span class="sxs-lookup"><span data-stu-id="2690f-111">In ASP.NET Core 3.1, render a component from a page or view with the new Component Tag Helper:</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="2690f-112">Le programme d’assistance HTML reste pris en charge dans ASP.NET Core 3,1, mais le tag Helper Component est recommandé.</span><span class="sxs-lookup"><span data-stu-id="2690f-112">The HTML Helper remains supported in ASP.NET Core 3.1, but the Component Tag Helper is recommended.</span></span>

<span data-ttu-id="2690f-113">les applications Blazor Server peuvent désormais passer des paramètres aux composants de niveau supérieur lors du rendu initial.</span><span class="sxs-lookup"><span data-stu-id="2690f-113">Blazor Server apps can now pass parameters to top-level components during the initial render.</span></span> <span data-ttu-id="2690f-114">Auparavant, vous pouviez uniquement passer des paramètres à un composant de niveau supérieur avec [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span><span class="sxs-lookup"><span data-stu-id="2690f-114">Previously you could only pass parameters to a top-level component with [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span></span> <span data-ttu-id="2690f-115">Avec cette version, [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) et [RenderModel. ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="2690f-115">With this release, both [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) and [RenderModel.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) are supported.</span></span> <span data-ttu-id="2690f-116">Toute valeur de paramètre spécifiée est sérialisée au format JSON et incluse dans la réponse initiale.</span><span class="sxs-lookup"><span data-stu-id="2690f-116">Any specified parameter values are serialized as JSON and included in the initial response.</span></span>

<span data-ttu-id="2690f-117">Par exemple, prérendez un composant `Counter` avec un volume d’incrément (`IncrementAmount`) :</span><span class="sxs-lookup"><span data-stu-id="2690f-117">For example, prerender a `Counter` component with an increment amount (`IncrementAmount`):</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="2690f-118">Pour plus d’informations, consultez [intégrer des composants dans des applications Razor pages et MVC](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span><span class="sxs-lookup"><span data-stu-id="2690f-118">For more information, see [Integrate components into Razor Pages and MVC apps](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span></span>

## <a name="support-for-shared-queues-in-httpsys"></a><span data-ttu-id="2690f-119">Prise en charge des files d’attente partagées dans HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="2690f-119">Support for shared queues in HTTP.sys</span></span>

<span data-ttu-id="2690f-120">[Http. sys](xref:fundamentals/servers/httpsys) prend en charge la création de files d’attente de demandes anonymes.</span><span class="sxs-lookup"><span data-stu-id="2690f-120">[HTTP.sys](xref:fundamentals/servers/httpsys) supports creating anonymous request queues.</span></span> <span data-ttu-id="2690f-121">Dans ASP.NET Core 3,1, nous avons ajouté la possibilité de créer une file d’attente de requêtes HTTP. sys nommée existante ou de l’attacher à celle-ci.</span><span class="sxs-lookup"><span data-stu-id="2690f-121">In ASP.NET Core 3.1, we’ve added to ability to create or attach to an existing named HTTP.sys request queue.</span></span> <span data-ttu-id="2690f-122">La création ou l’attachement à une file d’attente de requêtes HTTP. sys existante active les scénarios où le protocole HTTP. Le processus de contrôleur sys qui possède la file d’attente est indépendant du processus de l’écouteur.</span><span class="sxs-lookup"><span data-stu-id="2690f-122">Creating or attaching to an existing named HTTP.sys request queue enables scenarios where the HTTP.Sys controller process that owns the queue is independent of the listener process.</span></span> <span data-ttu-id="2690f-123">Cette indépendance permet de conserver les connexions existantes et les demandes mises en file d’attente entre les redémarrages du processus de l’écouteur :</span><span class="sxs-lookup"><span data-stu-id="2690f-123">This independence makes it possible to preserve existing connections and enqueued requests between listener process restarts:</span></span>

[!code-csharp[](sample/Program.cs?name=snippet)]

<!-- TODO
## Breaking changes for SameSite cookies
-->

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a><span data-ttu-id="2690f-124">Empêcher les actions par défaut des événements dans les applications Blazor</span><span class="sxs-lookup"><span data-stu-id="2690f-124">Prevent default actions for events in Blazor apps</span></span>

<span data-ttu-id="2690f-125">Utilisez l’attribut `@on{EVENT}:preventDefault` directive pour empêcher l’action par défaut pour un événement.</span><span class="sxs-lookup"><span data-stu-id="2690f-125">Use the `@on{EVENT}:preventDefault` directive attribute to prevent the default action for an event.</span></span> <span data-ttu-id="2690f-126">Dans l’exemple suivant, l’action par défaut d’affichage du caractère de la clé dans la zone de texte est interdite :</span><span class="sxs-lookup"><span data-stu-id="2690f-126">In the following example, the default action of displaying the key's character in the text box is prevented:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

<span data-ttu-id="2690f-127">Pour plus d’informations, consultez [empêcher les actions par défaut](xref:blazor/components#prevent-default-actions).</span><span class="sxs-lookup"><span data-stu-id="2690f-127">For more information, see [Prevent default actions](xref:blazor/components#prevent-default-actions).</span></span>

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a><span data-ttu-id="2690f-128">Arrêter la propagation des événements dans les applications Blazor</span><span class="sxs-lookup"><span data-stu-id="2690f-128">Stop event propagation in Blazor apps</span></span>

<span data-ttu-id="2690f-129">Utilisez l’attribut `@on{EVENT}:stopPropagation` directive pour arrêter la propagation des événements.</span><span class="sxs-lookup"><span data-stu-id="2690f-129">Use the `@on{EVENT}:stopPropagation` directive attribute to stop event propagation.</span></span> <span data-ttu-id="2690f-130">Dans l’exemple suivant, la sélection de la case à cocher empêche les événements Click du `<div>` enfant de se propager vers le `<div>`parent :</span><span class="sxs-lookup"><span data-stu-id="2690f-130">In the following example, selecting the check box prevents click events from the child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

<span data-ttu-id="2690f-131">Pour plus d’informations, consultez [arrêter la propagation des événements](xref:blazor/components#stop-event-propagation).</span><span class="sxs-lookup"><span data-stu-id="2690f-131">For more information, see [Stop event propagation](xref:blazor/components#stop-event-propagation).</span></span>

## <a name="detailed-errors-during-opno-locblazor-app-development"></a><span data-ttu-id="2690f-132">Erreurs détaillées lors du développement d’applications Blazor</span><span class="sxs-lookup"><span data-stu-id="2690f-132">Detailed errors during Blazor app development</span></span>

<span data-ttu-id="2690f-133">Quand une application Blazor ne fonctionne pas correctement pendant le développement, la réception d’informations d’erreur détaillées de l’application vous aide à résoudre les problèmes et à résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="2690f-133">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="2690f-134">Lorsqu’une erreur se produit, Blazor applications affichent une barre dorée en bas de l’écran :</span><span class="sxs-lookup"><span data-stu-id="2690f-134">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="2690f-135">Pendant le développement, la barre dorée vous dirige vers la console du navigateur, où vous pouvez voir l’exception.</span><span class="sxs-lookup"><span data-stu-id="2690f-135">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="2690f-136">En production, la barre dorée avertit l’utilisateur qu’une erreur s’est produite et recommande l’actualisation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="2690f-136">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="2690f-137">Pour plus d’informations, consultez [erreurs détaillées lors du développement](xref:blazor/handle-errors#detailed-errors-during-development).</span><span class="sxs-lookup"><span data-stu-id="2690f-137">For more information, see [Detailed errors during development](xref:blazor/handle-errors#detailed-errors-during-development).</span></span>
