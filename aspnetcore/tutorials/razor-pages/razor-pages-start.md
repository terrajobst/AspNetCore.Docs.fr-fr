---
title: Bien démarrer avec des pages Razor dans ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861626"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Tutoriel : Bien démarrer avec Razor Pages dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.

L’application gère une base de données de titres de films. Vous apprenez à :

> [!div class="checklist"]
> * Créer une application web Razor Pages.
> * Ajouter et structurer un modèle.
> * Utiliser une base de données.
> * Ajouter une fonctionnalité de recherche et de validation.

À la fin, vous obtenez une application qui peut gérer et afficher des éléments de titres de films.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Créez une application web ASP.NET Core. Nommez le projet **RazorPagesMovie**. Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.
 ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, puis sélectionnez **Application web**.

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Le projet de démarrage suivant est créé :

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

* Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.

  Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application. La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`. La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local. Localhost traite uniquement les requêtes web de l’ordinateur local. Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web. Dans l’image précédente, le numéro de port est 5001. Quand vous exécutez l’application, vous voyez un autre numéro de port.

  Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code. De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Accédez à un répertoire (`cd`) destiné à contenir le projet.
* Exécutez la commande suivante :

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « RazorPagesMovie ». Faut-il les ajouter ?**  Sélectionnez **Oui**.

  * `dotnet new webapp -o RazorPagesMovie` : crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.
  * `code -r RazorPagesMovie` : charge le fichier projet *RazorPagesMovie.csproj* dans Visual Studio Code.

### <a name="launch-the-app"></a>Lancer l’application

* Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.

  Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`. La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`. La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local. Localhost traite uniquement les requêtes web de l’ordinateur local.

  Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code. De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

À partir d’un terminal, exécutez les commandes suivantes :

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor. Ouvrez un navigateur sur http://localhost:5000 pour voir l’application.

## <a name="open-the-project"></a>Ouvrir le projet

Appuyez sur Ctrl+C pour arrêter l’application.

Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Lancer l’application

Sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application. Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.

<!-- End of VS tabs -->

---

* Sélectionnez **Accepter** pour accepter le suivi. Cette application n’effectue pas le suivi d’informations personnelles. Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  L’illustration suivante montre l’application une fois le suivi accepté :

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>Dossiers et fichiers projet

Le tableau suivant répertorie les fichiers et dossiers du projet. À ce stade du tutoriel, le plus important est de comprendre le fichier *Startup.cs*. Vous n’avez pas besoin de passer en revue chaque lien ci-dessous. Les liens sont fournis en guise de référence quand vous avez besoin d’informations supplémentaires sur un fichier ou un dossier du projet.

| Fichier ou dossier              | Objectif |
| ----------------- | ------------ |
| *wwwroot* | Contient des fichiers statiques. Consultez [Fichiers statiques](xref:fundamentals/static-files). |
| *Pages* | Dossier pour [Pages Razor](xref:razor-pages/index). |
| *appsettings.json* | [Configuration](xref:fundamentals/configuration/index) |
| *Program.cs* | [Héberge](xref:fundamentals/host/index) l’application ASP.NET Core.|
| *Startup.cs* | Configure les services et le pipeline de requête. Voir [Démarrage](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Le dossier Pages

Le fichier *_Layout.cshtml* contient des éléments HTML communs (scripts et feuilles de style) et définit la disposition de l’application. Par exemple, quand vous cliquez sur **RazorPagesMovie**, **Home** ou **Privacy**, les mêmes éléments apparaissent. Les éléments communs incluent le menu de navigation en haut et l’en-tête en bas de la fenêtre. Pour plus d’informations, consultez [Disposition](xref:mvc/views/layout).

Le fichier *_ViewImports.cshtml* contient des directives Razor qui sont importées dans chaque page Razor. Pour plus d’informations, consultez [Importation de directives partagées](xref:mvc/views/layout#importing-shared-directives).

Le fichier *_ViewStart.cshtml* définit la propriété `Layout` des pages Razor pour qu’elle utilise le fichier *_Layout.cshtml*. Pour plus d’informations, consultez [Disposition](xref:mvc/views/layout).

Le fichier *_ValidationScriptsPartial.cshtml* fournit une référence aux scripts de validation [jQuery](https://jquery.com/). Quand les pages `Create` et `Edit` sont ajoutées plus loin dans ce tutoriel, le fichier *_ValidationScriptsPartial.cshtml* est utilisé.

Les pages `Index`, `Error` et `Privacy` sont fournies pour :

* `Index` : démarrer une application.
* `Error` : afficher des informations d’erreur.
* `Privacy` : spécifier des informations sur la politique de confidentialité du site.

Dans ce tutoriel, les pages précédentes ne sont pas utilisées.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>Utilisez la touche F7 pour basculer entre une page Razor Pages et le modèle de page

F7 permet de basculer entre une page Razor Pages (fichier *\*.cshtml*) et le fichier C# (*\*.cshtml.cs*).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

Par convention, la page Razor Pages (fichier *\*.cshtml*) et le `PageModel` associé ont le même nom de fichier racine.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Par convention, la page Razor Pages (fichier *\*.cshtml*) et le `PageModel` associé ont le même nom de fichier racine.

---

> [!div class="step-by-step"]
> [Suivant : Ajout d’un modèle](xref:tutorials/razor-pages/model)