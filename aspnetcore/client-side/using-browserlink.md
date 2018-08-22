---
title: Lien du navigateur dans ASP.NET Core
author: ncarandini
description: Explique comment le lien du navigateur est une fonctionnalité de Visual Studio qui lie l’environnement de développement avec un ou plusieurs navigateurs web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 452ba5149563c186750466f471c7b950f0017614
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835672"
---
# <a name="browser-link-in-aspnet-core"></a>Lien du navigateur dans ASP.NET Core

Par [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), et [Tom Dykstra](https://github.com/tdykstra)

Le lien du navigateur est une fonctionnalité de Visual Studio qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs web. Vous pouvez utiliser le lien du navigateur pour actualiser votre application web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests dans plusieurs navigateurs.

## <a name="browser-link-setup"></a>Installation du lien du navigateur

::: moniker range=">= aspnetcore-2.1"

Lors de la conversion d’un projet ASP.NET Core 2.0 pour ASP.NET Core 2.1 et la transition vers le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), installer le [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package pour Fonctionnalité BrowserLink. Utilisent les modèles de projet ASP.NET Core 2.1 le `Microsoft.AspNetCore.App` métapackage par défaut.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **Web Application**, **vide**, et **API Web** utilisation de modèles de projet la [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) , qui contient une référence de package pour [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Par conséquent, à l’aide de la `Microsoft.AspNetCore.All` métapackage ne nécessite aucune action supplémentaire de proposer le lien du navigateur à utiliser.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Le modèle de projet d’ASP.NET Core 1.x **Application Web** a une référence de package pour le package [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Pour les modèles de projet **Vide** ou **API Web**, vous devez ajouter une référence de package à `Microsoft.VisualStudio.Web.BrowserLink`.

S’agissant d’une fonctionnalité de Visual Studio, le moyen le plus simple pour ajouter le package à un **vide** ou **API Web** modèle de projet consiste à ouvrir le **Console du Gestionnaire de Package** (**Vue** > **Windows autres** > **Console du Gestionnaire de Package**) et exécutez la commande suivante :

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Vous pouvez également utiliser le **Gestionnaire de package NuGet**.  Cliquez sur le nom du projet dans **l’Explorateur de solutions** et choisissez **Gérer les packages NuGet** :

![Gestionnaire de Package NuGet ouvert](using-browserlink/_static/open-nuget-package-manager.png)

Recherchez et installez le package :

![Ajoutez le package avec le Gestionnaire de Package NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Configuration

Dans la méthode `Startup.Configure` :

```csharp
app.UseBrowserLink();
```

Le code se trouve généralement à l’intérieur d’un bloc `if` qui permet uniquement d'utiliser le lien du navigateur dans l’environnement de développement, comme indiqué ici :

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Comment utiliser le lien du navigateur

Quand vous avez un projet ASP.NET Core ouvert, Visual Studio affiche le contrôle de barre d’outils Lien du navigateur à côté du contrôle de barre d’outils **Cible de débogage** :

![Menu de liste déroulante de lien de navigateur](using-browserlink/_static/browserLink-dropdown-menu.png)

À partir du contrôle de barre d’outils Lien du navigateur, vous pouvez :

* Actualiser l’application web dans plusieurs navigateurs à la fois.
* Ouvrir le **tableau de bord Lien du navigateur**.
* Activer ou désactiver le **lien du navigateur**. Remarque : Le lien du navigateur est désactivé par défaut dans Visual Studio 2017 (15.3).
* Activer ou désactiver [la synchronisation automatique CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Certains plug-ins Visual Studio, notamment *Web Extension Pack 2015* et *Web Extension Pack 2017*, proposent des fonctionnalités étendues pour le lien du navigateur, mais certaines de ces fonctionnalités ne fonctionnent pas avec ASP. Projets NET Core.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Actualiser l’application web dans plusieurs navigateurs à la fois

Pour choisir un navigateur web unique à lancer au démarrage du projet, utilisez le menu déroulant du contrôle de barre d'outils **Cible de débogage** :

![Menu déroulant de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Pour ouvrir plusieurs navigateurs à la fois, choisissez **Naviguer avec...** dans la même liste déroulante. Maintenez la touche Ctrl enfoncée pour sélectionner les navigateurs de votre choix, puis cliquez sur **Parcourir** :

![Ouvrir à la fois des nombreux navigateurs](using-browserlink/_static/open-many-browsers-at-once.png)

Voici une capture d’écran montrant Visual Studio avec la vue Index ouverte et deux navigateurs ouverts :

![Synchroniser avec l’exemple de deux navigateurs](using-browserlink/_static/sync-with-two-browsers-example.png)

Placez le curseur sur la barre d’outils du lien du navigateur pour voir les navigateurs connectés au projet :

![Info-bulle de pointage](using-browserlink/_static/hoover-tip.png)

Changez l’affichage de l’index. Tous les navigateurs connectés sont mis à jour quand vous cliquez sur le bouton d’actualisation du lien du navigateur :

![navigateurs-sync-changes](using-browserlink/_static/browsers-sync-to-changes.png)

Le lien du navigateur fonctionne également avec les navigateurs lancés en dehors de Visual Studio et accessibles au moyen de l’URL de l’application.

### <a name="the-browser-link-dashboard"></a>Le tableau de bord de lien de navigateur

Dans la liste de lien du navigateur déroulante menu pour gérer la connexion avec les navigateurs ouverts, ouvrez le tableau de bord de lien de navigateur :

![Open-browserslink-tableau de bord](using-browserlink/_static/open-browserlink-dashboard.png)

Si aucun navigateur n’est connecté, vous pouvez démarrer une session autre qu'une session de débogage en sélectionnant le lien *Afficher dans le navigateur* :

![browserlink-tableau de bord-no-connexions](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Sinon, les navigateurs connectés sont affichées avec le chemin d’accès à la page qui affiche chaque navigateur :

![browserlink tableau de bord deux connexions](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Si vous le souhaitez, vous pouvez cliquer sur un nom de navigateur répertorié pour actualiser seulement ce navigateur.

### <a name="enable-or-disable-browser-link"></a>Activer ou désactiver le lien du navigateur

Quand vous réactivez le lien du navigateur après l’avoir désactivé, vous devez actualiser les navigateurs pour les reconnecter.

### <a name="enable-or-disable-css-auto-sync"></a>Activer ou désactiver la synchronisation automatique CSS

Lorsque la synchronisation automatique CSS est activée, les navigateurs connectés sont automatiquement actualisées lorsque vous apportez des modifications aux fichiers CSS.

## <a name="how-it-works"></a>Son fonctionnement

Browser Link utilise SignalR pour créer un canal de communication entre Visual Studio et le navigateur. Quand le lien du navigateur est activé, Visual Studio fait office de serveur SignalR auquel plusieurs clients (navigateurs) peuvent se connecter.  Lien du navigateur enregistre également un composant d’intergiciel (middleware) dans le pipeline de demande ASP.NET Core. Ce composant injecte des références `<script>` dans chaque demande de page à partir du serveur. Pour voir les références de script, sélectionnez **Afficher la source** dans le navigateur et faites défiler jusqu’à la fin du contenu de la balise `<body>` :

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Vos fichiers sources ne sont pas modifiés. Le composant d’intergiciel (middleware) injecte dynamiquement les références de script.

Le code côté navigateur étant uniquement du code JavaScript, il fonctionne sur tous les navigateurs prenant en charge SignalR sans nécessiter un plug-in de navigateur.
