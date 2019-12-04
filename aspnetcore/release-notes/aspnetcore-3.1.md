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
ms.openlocfilehash: 634c6937089a0a0fe1f862a83771aff65a1f8418
ms.sourcegitcommit: 5974e3e66dab3398ecf2324fbb82a9c5636f70de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74778841"
---
# <a name="whats-new-in-aspnet-core-31"></a>Nouveautés de ASP.NET Core 3,1

Cet article met en évidence les modifications les plus importantes apportées à ASP.NET Core 3,1 avec des liens vers la documentation appropriée.

## <a name="partial-class-support-for-razor-components"></a>Prise en charge des classes partielles pour les composants Razor

Les composants Razor sont désormais générés en tant que classes partielles. Le code d’un composant Razor peut être écrit à l’aide d’un fichier code-behind défini en tant que classe partielle, plutôt que de définir tout le code du composant dans un fichier unique. Pour plus d’informations, consultez [prise en charge des classes partielles](xref:blazor/components#partial-class-support).

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor tag Helper de composant et passer des paramètres à des composants de niveau supérieur

Dans Blazor avec ASP.NET Core 3,0, les composants étaient rendus dans des pages et des vues à l’aide d’un programme d’assistance HTML (`Html.RenderComponentAsync`). Dans ASP.NET Core 3,1, restituez un composant à partir d’une page ou d’une vue avec le nouveau tag Helper du composant :

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

Le programme d’assistance HTML reste pris en charge dans ASP.NET Core 3,1, mais le tag Helper Component est recommandé.

les applications Blazor Server peuvent désormais passer des paramètres aux composants de niveau supérieur lors du rendu initial. Auparavant, vous pouviez uniquement passer des paramètres à un composant de niveau supérieur avec [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static). Avec cette version, [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) et [RenderModel. ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) sont pris en charge. Toute valeur de paramètre spécifiée est sérialisée au format JSON et incluse dans la réponse initiale.

Par exemple, prérendez un composant `Counter` avec un volume d’incrément (`IncrementAmount`) :

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Pour plus d’informations, consultez [intégrer des composants dans des applications Razor pages et MVC](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).

## <a name="support-for-shared-queues-in-httpsys"></a>Prise en charge des files d’attente partagées dans HTTP. sys

[Http. sys](xref:fundamentals/servers/httpsys) prend en charge la création de files d’attente de demandes anonymes. Dans ASP.NET Core 3,1, nous avons ajouté la possibilité de créer une file d’attente de requêtes HTTP. sys nommée existante ou de l’attacher à celle-ci. La création ou l’attachement à une file d’attente de requêtes HTTP. sys existante active les scénarios où le protocole HTTP. Le processus de contrôleur sys qui possède la file d’attente est indépendant du processus de l’écouteur. Cette indépendance permet de conserver les connexions existantes et les demandes mises en file d’attente entre les redémarrages du processus de l’écouteur :

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a>Dernières modifications pour les cookies SameSite

Le comportement des cookies SameSite a changé pour refléter les futures modifications apportées au navigateur. Cela peut affecter les scénarios d’authentification tels que AzureAd, OpenIdConnect ou WsFederation. Pour plus d'informations, consultez <xref:security/samesite>.

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a>Empêcher les actions par défaut des événements dans les applications Blazor

Utilisez l’attribut `@on{EVENT}:preventDefault` directive pour empêcher l’action par défaut pour un événement. Dans l’exemple suivant, l’action par défaut d’affichage du caractère de la clé dans la zone de texte est interdite :

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

Pour plus d’informations, consultez [empêcher les actions par défaut](xref:blazor/components#prevent-default-actions).

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a>Arrêter la propagation des événements dans les applications Blazor

Utilisez l’attribut `@on{EVENT}:stopPropagation` directive pour arrêter la propagation des événements. Dans l’exemple suivant, la sélection de la case à cocher empêche les événements Click du `<div>` enfant de se propager vers le `<div>`parent :

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

Pour plus d’informations, consultez [arrêter la propagation des événements](xref:blazor/components#stop-event-propagation).

## <a name="detailed-errors-during-opno-locblazor-app-development"></a>Erreurs détaillées lors du développement d’applications Blazor

Quand une application Blazor ne fonctionne pas correctement pendant le développement, la réception d’informations d’erreur détaillées de l’application vous aide à résoudre les problèmes et à résoudre le problème. Lorsqu’une erreur se produit, Blazor applications affichent une barre dorée en bas de l’écran :

* Pendant le développement, la barre dorée vous dirige vers la console du navigateur, où vous pouvez voir l’exception.
* En production, la barre dorée avertit l’utilisateur qu’une erreur s’est produite et recommande l’actualisation du navigateur.

Pour plus d’informations, consultez [erreurs détaillées lors du développement](xref:blazor/handle-errors#detailed-errors-during-development).
