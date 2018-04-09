---
title: Lien du navigateur dans ASP.NET Core
author: ncarandini
description: Explique comment le lien du navigateur est une fonctionnalité de Visual Studio qui lie l’environnement de développement avec un ou plusieurs navigateurs web.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: a75a896dd7ebc488e3e9344ec705c24201924375
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="browser-link-in-aspnet-core"></a>Lien du navigateur dans ASP.NET Core

Par [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), et [Tom Dykstra](https://github.com/tdykstra)

Le lien du navigateur est une fonctionnalité de Visual Studio qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs web. Vous pouvez utiliser le lien du navigateur pour actualiser votre application web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests dans plusieurs navigateurs.

## <a name="browser-link-setup"></a>Installation du lien du navigateur

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le modèles de projet d’ASP.NET Core 2.x **Application Web**, **Vide** et **API Web** utilisent le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) qui contient une référence de package pour [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) Grâce au métapackage `Microsoft.AspNetCore.All`, aucune action supplémentaire n'est nécessaire pour rendre le lien du navigateur disponible.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Le modèle de projet d’ASP.NET Core 1.x **Application Web** a une référence de package pour le package [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Pour les modèles de projet **Vide** ou **API Web**, vous devez ajouter une référence de package à `Microsoft.VisualStudio.Web.BrowserLink`.

Dans la mesure où il s'agit d'une fonctionnalité Visual Studio, la meilleure façon d'ajouter le package à un modèle de projet **Vide** ou **API Web** consiste à ouvrir la **Console du Gestionnaire de package** (**Affichage**  >   **Autres fenêtres**  >  **Console du Gestionnaire de package**) et à exécuter la commande suivante :

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Vous pouvez également utiliser le **Gestionnaire de package NuGet**.  Cliquez sur le nom du projet dans **l’Explorateur de solutions** et choisissez **Gérer les packages NuGet** :

![Gestionnaire de Package NuGet ouvert](using-browserlink/_static/open-nuget-package-manager.png)

Recherchez et installez le package :

![Ajouter un package avec le Gestionnaire de Package NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>Configuration

Dans la méthode `Configure` du fichier *Startup.cs* :

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

Pour plus d’informations, consultez [travailler avec plusieurs environnements](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Comment utiliser le lien du navigateur

Quand vous avez un projet ASP.NET Core ouvert, Visual Studio affiche le contrôle de barre d’outils Lien du navigateur à côté du contrôle de barre d’outils **Cible de débogage** :

![Menu de liste déroulante de lien de navigateur](using-browserlink/_static/browserLink-dropdown-menu.png)

À partir du contrôle de barre d’outils Lien du navigateur, vous pouvez :

* Actualiser l’application web dans plusieurs navigateurs à la fois.
* Ouvrir le **tableau de bord Lien du navigateur**.
* Activer ou désactiver le **lien du navigateur**. Remarque : Le lien du navigateur est désactivé par défaut dans Visual Studio 2017 (15.3).
* Activer ou désactiver [la synchronisation automatique CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Certains plug-ins de Visual Studio, notamment *Web Extension Pack 2015* et *Web Extension Pack 2017*, proposent des fonctionnalités étendues pour le lien du navigateur, mais certaines des fonctionnalités supplémentaires ne fonctionnent pas avec ASP. Projets NET Core.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Actualiser à la fois l’application web dans plusieurs navigateurs

Pour choisir un navigateur web unique à lancer au démarrage du projet, utilisez le menu déroulant du contrôle de barre d'outils **Cible de débogage** :

![Menu déroulant de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Pour ouvrir plusieurs navigateurs à la fois, choisissez **Naviguer avec...** dans la même liste déroulante. Maintenez la touche Ctrl enfoncée pour sélectionner les navigateurs de votre choix, puis cliquez sur **Parcourir** :

![Ouvrir à la fois des nombreux navigateurs](using-browserlink/_static/open-many-browsers-at-once.png)

Voici une capture d’écran montrant Visual Studio avec la vue Index ouverte et deux navigateurs ouverts :

![Synchroniser avec l’exemple de deux navigateurs](using-browserlink/_static/sync-with-two-browsers-example.png)

Placez le curseur sur la barre d’outils du lien du navigateur pour voir les navigateurs connectés au projet :

![Placez le curseur pointe](using-browserlink/_static/hoover-tip.png)

Changez l’affichage de l’index. Tous les navigateurs connectés sont mis à jour quand vous cliquez sur le bouton d’actualisation du lien du navigateur :

![navigateurs-sync-changes](using-browserlink/_static/browsers-sync-to-changes.png)

Le lien du navigateur fonctionne également avec les navigateurs lancés en dehors de Visual Studio et accessibles au moyen de l’URL de l’application.

### <a name="the-browser-link-dashboard"></a>Le tableau de bord de lien de navigateur

Dans la liste déroulante de lien du navigateur vers le bas du menu pour gérer les connexions avec les navigateurs ouverts, ouvrez le tableau de bord de lien de navigateur :

![Ouvrir-browserslink-tableau de bord](using-browserlink/_static/open-browserlink-dashboard.png)

Si aucun navigateur n’est connecté, vous pouvez démarrer une session autre qu'une session de débogage en sélectionnant le lien *Afficher dans le navigateur* :

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Dans le cas contraire, les navigateurs connectés sont affichés avec le chemin d’accès à la page qui affiche chaque navigateur :

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Si vous le souhaitez, vous pouvez cliquer sur un nom de navigateur répertorié pour actualiser seulement ce navigateur.

### <a name="enable-or-disable-browser-link"></a>Activer ou désactiver le lien du navigateur

Quand vous réactivez le lien du navigateur après l’avoir désactivé, vous devez actualiser les navigateurs pour les reconnecter.

### <a name="enable-or-disable-css-auto-sync"></a>Activer ou désactiver la synchronisation automatique CSS

Lorsque la synchronisation automatique CSS est activée, les navigateurs connectés sont automatiquement actualisées lorsque vous modifiez des fichiers CSS.

## <a name="how-does-it-work"></a>Comment cela fonctionne-t-il ?

Lien du navigateur utilise SignalR pour créer un canal de communication entre Visual Studio et le navigateur. Lorsque le lien du navigateur est activée, Visual Studio fait Office de serveur SignalR plusieurs clients (navigateurs) pouvant se connecter à. Lien du navigateur enregistre également un composant d’intergiciel (middleware) dans le pipeline de demande ASP.NET. Ce composant injecte spéciaux `<script>` références dans chaque demande de page à partir du serveur. Vous pouvez consulter les références de script en sélectionnant **afficher la source** dans le navigateur et le défilement à la fin de la `<body>` le contenu de la balise :

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Vos fichiers sources ne sont pas modifiés. Le composant d’intergiciel (middleware) injecte les références de script dynamiquement. 

Le code côté navigateur étant uniquement du code JavaScript, il fonctionne sur tous les navigateurs prenant en charge SignalR sans nécessiter un plug-in de navigateur.
