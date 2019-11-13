---
title: Lien du navigateur dans ASP.NET Core
author: ncarandini
description: Explique comment le lien du navigateur est une fonctionnalité de Visual Studio qui lie l’environnement de développement à un ou plusieurs navigateurs Web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962783"
---
# <a name="browser-link-in-aspnet-core"></a>Lien du navigateur dans ASP.NET Core

Par [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson)et [Tom Dykstra](https://github.com/tdykstra)

Le lien du navigateur est une fonctionnalité de Visual Studio qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs Web. Vous pouvez utiliser le lien du navigateur pour actualiser votre application Web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests entre navigateurs.

## <a name="browser-link-setup"></a>Configuration des liens du navigateur

::: moniker range=">= aspnetcore-2.1"

Lors de la conversion d’un projet ASP.NET Core 2,0 en ASP.NET Core 2,1 et de la transition vers le [AspNetCore Microsoft. app](xref:fundamentals/metapackage-app), installez le package [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pour la fonctionnalité BrowserLink. Les modèles de projet ASP.NET Core 2,1 utilisent le sous-package `Microsoft.AspNetCore.App` par défaut.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Les modèles de projet **application web**ASP.net Core 2,0, **vide**et **API Web** utilisent le sous- [package Microsoft. AspNetCore. All](xref:fundamentals/metapackage), qui contient une référence de package pour [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Par conséquent, l’utilisation du `Microsoft.AspNetCore.All` repackage ne nécessite aucune action supplémentaire pour rendre le lien de navigateur disponible.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Le modèle de projet d' **application Web** ASP.net Core 1. x a une référence de package pour le package [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) . Les projets de modèle d' **API Web** ou **vides** requièrent que vous ajoutiez une référence de package à `Microsoft.VisualStudio.Web.BrowserLink`.

Dans la mesure où il s’agit d’une fonctionnalité de Visual Studio, le moyen le plus simple d’ajouter le package à un projet de modèle d' **API Web** ou **vide** consiste à ouvrir la **console du gestionnaire de package** (**Afficher** > autre console du **Gestionnaire de package** **Windows** >) et à exécuter la commande suivante :

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Vous pouvez également utiliser le **Gestionnaire de package NuGet**. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur le nom du projet et choisissez **gérer les packages NuGet**:

![Ouvrir le gestionnaire de package NuGet](using-browserlink/_static/open-nuget-package-manager.png)

Recherchez et installez le package :

![Ajouter un package avec le gestionnaire de package NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Configuration

Dans la méthode `Startup.Configure` :

```csharp
app.UseBrowserLink();
```

En général, le code se trouve à l’intérieur d’un bloc de `if` qui active uniquement le lien du navigateur dans l’environnement de développement, comme illustré ici :

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Procédure d’utilisation d’un lien de navigateur

Quand un projet de ASP.NET Core est ouvert, Visual Studio affiche le contrôle de barre d’outils lien de navigateur en regard du contrôle de barre d’outils **cible de débogage** :

![Menu déroulant lien du navigateur](using-browserlink/_static/browserLink-dropdown-menu.png)

Dans le contrôle de barre d’outils lien de navigateur, vous pouvez :

* Actualisez l’application Web dans plusieurs navigateurs à la fois.
* Ouvrez le **tableau de bord du lien de navigateur**.
* Activez ou désactivez le **lien du navigateur**. Remarque : le lien du navigateur est désactivé par défaut dans Visual Studio 2017 (15,3).
* Activez ou désactivez la [synchronisation automatique CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Certains plug-ins Visual Studio, notamment le *Pack d’extension web 2015* et le *Pack d’extension Web 2017*, offrent des fonctionnalités étendues pour le lien du navigateur, mais certaines fonctionnalités supplémentaires ne fonctionnent pas avec les projets de ASP.net core.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Actualiser l’application Web dans plusieurs navigateurs à la fois

Pour choisir un seul navigateur Web à lancer au démarrage du projet, utilisez le menu déroulant du contrôle de barre d’outils de la **cible de débogage** :

![Menu déroulant F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Pour ouvrir plusieurs navigateurs à la fois, choisissez **Parcourir avec...** dans la même liste déroulante. Maintenez la touche CTRL enfoncée pour sélectionner les navigateurs de votre choix, puis cliquez sur **Parcourir**:

![Ouvrir plusieurs navigateurs à la fois](using-browserlink/_static/open-many-browsers-at-once.png)

Voici une capture d’écran montrant Visual Studio avec la vue d’index ouverte et deux navigateurs ouverts :

![Exemple de synchronisation avec deux navigateurs](using-browserlink/_static/sync-with-two-browsers-example.png)

Pointez sur le contrôle de barre d’outils lien du navigateur pour afficher les navigateurs qui sont connectés au projet :

![Pointe pointage](using-browserlink/_static/hoover-tip.png)

Modifiez la vue index et tous les navigateurs connectés sont mis à jour lorsque vous cliquez sur le bouton actualiser le lien du navigateur :

![navigateurs-synchronisation-à-modification](using-browserlink/_static/browsers-sync-to-changes.png)

Le lien du navigateur fonctionne également avec les navigateurs que vous lancez en dehors de Visual Studio et accédez à l’URL de l’application.

### <a name="the-browser-link-dashboard"></a>Tableau de bord du lien de navigateur

Ouvrez le tableau de bord du lien de navigateur dans le menu déroulant lien du navigateur pour gérer la connexion avec les navigateurs ouverts :

![ouvrir-browserslink-tableau de bord](using-browserlink/_static/open-browserlink-dashboard.png)

Si aucun navigateur n’est connecté, vous pouvez démarrer une session de non-débogage en sélectionnant le lien *afficher dans le navigateur* :

![browserlink-tableau de bord-sans-connexions](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Dans le cas contraire, les navigateurs connectés affichent le chemin d’accès à la page affichée par chaque navigateur :

![browserlink-tableau de bord-deux-connexions](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Si vous le souhaitez, vous pouvez cliquer sur le nom d’un navigateur pour actualiser ce navigateur.

### <a name="enable-or-disable-browser-link"></a>Activer ou désactiver le lien du navigateur

Lorsque vous réactivez le lien de navigateur après l’avoir désactivé, vous devez actualiser les navigateurs pour les reconnecter.

### <a name="enable-or-disable-css-auto-sync"></a>Activer ou désactiver la synchronisation automatique CSS

Lorsque la synchronisation automatique CSS est activée, les navigateurs connectés sont actualisés automatiquement lorsque vous apportez des modifications aux fichiers CSS.

## <a name="how-it-works"></a>Fonctionnement

Le lien du navigateur utilise SignalR pour créer un canal de communication entre Visual Studio et le navigateur. Lorsque le lien du navigateur est activé, Visual Studio agit comme un serveur de SignalR auquel plusieurs clients (navigateurs) peuvent se connecter. Le lien du navigateur inscrit également un composant d’intergiciel dans le pipeline de demande ASP.NET Core. Ce composant injecte des références `<script>` spéciales dans chaque demande de page à partir du serveur. Vous pouvez voir les références de script en sélectionnant **afficher la source** dans le navigateur et en faisant défiler jusqu’à la fin du contenu de la balise `<body>` :

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Vos fichiers sources ne sont pas modifiés. Le composant intergiciel (middleware) injecte les références de script de manière dynamique.

Étant donné que le code côté navigateur est tout JavaScript, il fonctionne sur tous les navigateurs que SignalR prend en charge sans nécessiter de plug-in de navigateur.
