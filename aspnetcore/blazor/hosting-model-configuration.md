---
title: Configuration du modèle d’hébergement ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur la configuration du modèle d’hébergement Blazor, notamment sur l’intégration des composants Razor dans les applications Razor Pages et MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215059"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Configuration du modèle d’hébergement ASP.NET Core éblouissant

Par [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Cet article traite de l’hébergement de la configuration du modèle.

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a>Serveur Blazor

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>Intégrer des composants Razor dans des applications Razor Pages et MVC

#### <a name="use-components-in-pages-and-views"></a>Utiliser des composants dans les pages et les vues

Une Razor Pages ou une application MVC existante peut intégrer des composants Razor dans des pages et des vues :

1. Dans le fichier de disposition de l’application ( *_Layout. cshtml*) :

   * Ajoutez la balise `<base>` suivante à l’élément `<head>` :

     ```html
     <base href="~/" />
     ```

     La valeur `href` (le *chemin d’accès de base*de l’application) dans l’exemple précédent suppose que l’application se trouve dans le chemin d’URL racine (`/`). Si l’application est une sous-application, suivez les instructions de la section *chemin d’accès* à la base de l’application de l’article <xref:host-and-deploy/blazor/index#app-base-path>.

     Le fichier *_Layout. cshtml* se trouve dans le dossier *pages/Shared* d’une application Razor pages ou d’un dossier *Views/Shared* dans une application MVC.

   * Ajoutez une balise `<script>` pour le script *éblouissant. Server. js* juste avant la balise de fermeture `</body>` :

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     L’infrastructure ajoute le script *éblouissant. Server. js* à l’application. Il n’est pas nécessaire d’ajouter manuellement le script à l’application.

1. Ajoutez un fichier *_Imports. Razor* au dossier racine du projet avec le contenu suivant (modifiez le dernier espace de noms, `MyAppNamespace`, en lui attribuant l’espace de noms de l’application) :

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. Dans `Startup.ConfigureServices`, inscrivez le service du serveur éblouissant :

   ```csharp
   services.AddServerSideBlazor();
   ```

1. Dans `Startup.Configure`, ajoutez le point de terminaison du Hub éblouissant à `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Intégrer des composants dans n’importe quelle page ou vue. Pour plus d’informations, consultez la section *intégrer des composants dans Razor pages et applications MVC* de l’article <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-a-razor-pages-app"></a>Utiliser des composants routables dans une application Razor Pages

Pour prendre en charge les composants Razor routables dans les applications Razor Pages :

1. Suivez les instructions de la section [utiliser des composants dans les pages et les vues](#use-components-in-pages-and-views) .

1. Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Ajoutez un fichier *_Host. cshtml* au dossier *pages* avec le contenu suivant :

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.

1. Ajoutez un itinéraire de priorité basse pour la page *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. Ajoutez des composants routables à l’application. Par exemple :

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms représentant le dossier au fichier *pages/_ViewImports. cshtml* . Pour plus d’informations, consultez <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-an-mvc-app"></a>Utiliser des composants routables dans une application MVC

Pour prendre en charge les composants Razor routables dans les applications MVC :

1. Suivez les instructions de la section [utiliser des composants dans les pages et les vues](#use-components-in-pages-and-views) .

1. Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Ajoutez un fichier *_Host. cshtml* au dossier *views/de démarrage* avec le contenu suivant :

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.

1. Ajoutez une action au contrôleur d’hébergement :

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Ajoutez un itinéraire de faible priorité pour l’action de contrôleur qui retourne la vue *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Créez un dossier *pages* et ajoutez des composants routables à l’application. Par exemple :

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms représentant le dossier au fichier *views/_ViewImports. cshtml* . Pour plus d’informations, consultez <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="reflect-the-connection-state-in-the-ui"></a>Refléter l’état de la connexion dans l’interface utilisateur

Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter. En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.

Pour personnaliser l’interface utilisateur, définissez un élément avec un `id` de `components-reconnect-modal` dans la `<body>` de la page Razor *_Host. cshtml* :

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

Le tableau suivant décrit les classes CSS appliquées à l’élément `components-reconnect-modal`.

| Classe CSS                       | Indique&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | Connexion perdue. Le client tente de se reconnecter. Affichez le modal. |
| `components-reconnect-hide`     | Une connexion active est rétablie sur le serveur. Masquez le modal. |
| `components-reconnect-failed`   | Échec de la reconnexion, probablement en raison d’une défaillance du réseau. Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Reconnexion refusée. Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur est perdu. Pour recharger l’application, appelez `location.reload()`. Cet état de connexion peut se produire dans les cas suivants :<ul><li>Un blocage dans le circuit côté serveur se produit.</li><li>Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur. Les instances des composants avec lesquels l’utilisateur interagit sont supprimées.</li><li>Le serveur est redémarré ou le processus de travail de l’application est recyclé.</li></ul> |

### <a name="render-mode"></a>Mode d’affichage

Les applications serveur éblouissantes sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie. Elle est configurée dans la page Razor *_Host. cshtml* :

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode` configure si le composant :

* Est prérendu dans la page.
* Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.

| `RenderMode`        | Description |
| ------------------- | ----------- |
| `ServerPrerendered` | Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. |
| `Server`            | Restitue un marqueur pour une application Blazor Server. La sortie du composant n’est pas incluse. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. |
| `Static`            | Génère le rendu du composant en HTML statique. |

Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Restituer des composants interactifs avec état à partir de pages et de vues Razor

Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.

Lors du rendu de la page ou de la vue :

* Le composant est prérendu avec la page ou la vue.
* L’état initial du composant utilisé pour le prérendu est perdu.
* Un nouvel état de composant est créé lorsque la connexion SignalR est établie.

La page Razor suivante affiche un composant `Counter` :

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Rendre des composants non interactifs à partir de pages et de vues Razor

Dans la page Razor suivante, le composant `Counter` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Configurer le client SignalR pour les applications Blazor Server

Parfois, vous devez configurer le client SignalR utilisé par les applications Blazor Server. Par exemple, vous pouvez configurer la journalisation sur le client SignalR pour diagnostiquer un problème de connexion.

Pour configurer le client SignalR dans le fichier *pages/_Host. cshtml* :

* Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script `blazor.server.js`.
* Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur de SignalR.

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
