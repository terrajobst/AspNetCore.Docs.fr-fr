---
title: Prise en main de ASP.NET Core Blazor
author: guardrex
description: Commencez avec Blazor en créant une application Blazor avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: abecb640930c1e5770c0fad45a1e9a6df31a20f4
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306454"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Prise en main d’ASP.NET Core Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Pour vous familiariser avec éblouissant, suivez les instructions de votre choix d’outils :

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Pour créer des applications de serveur éblouissantes, installez [Visual Studio 2019 version 16,4 ou ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .

   Pour créer des applications de serveur éblouissant et de webassembly éblouissant, installez Visual Studio 2019 16,6 Preview 2 ou version ultérieure avec la charge de travail **développement Web et ASP.net** .

   Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *Webassembly éblouissant* et le *serveur éblouissant*, consultez <xref:blazor/hosting-models>.

1. Créez un projet.

1. Sélectionnez l' **application éblouissant**. Sélectionnez **Suivant**.

1. Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut. Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet. Sélectionnez **Create** (Créer).

1. Pour une expérience webassembly éblouissant (Visual Studio 16,6 Preview 2 ou version ultérieure), choisissez le modèle **application éblouissant Webassembly** . Pour une expérience de serveur éblouissant (Visual Studio 16,4 ou version ultérieure), choisissez le modèle **application de serveur éblouissant** . Sélectionnez **Create** (Créer).

1. Appuyez sur <kbd>Ctrl</kbd>+<kbd>F5</kbd> pour exécuter l’application.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Si vous le souhaitez, vous pouvez installer le modèle d’aperçu de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) en exécutant la commande suivante :

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > La [version de kit SDK .net Core 3.1.201 ou ultérieure](https://dotnet.microsoft.com/download/dotnet-core/3.1) est **requise** pour utiliser le modèle webassembly 3,2 Preview 3 éblouissant. Confirmez la version de kit SDK .NET Core installée en exécutant `dotnet --version` dans un interpréteur de commandes.

1. Installez [Visual Studio Code](https://code.visualstudio.com/).

1. Installez la dernière [ C# extension pour Visual Studio code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) et l’extension de [débogueur JavaScript (nocturne)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) avec `debug.javascript.usePreview` définie sur `true`.

1. Pour une expérience de serveur Blazor, exécutez la commande suivante dans une interface de commande :

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   Pour une expérience de webassembly Blazor, exécutez la commande suivante dans une interface de commande :

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.

1. Ouvrez le dossier *WebApplication1* dans Visual Studio code.

1. L’IDE demande que vous ajoutiez des ressources pour générer et déboguer le projet. Sélectionnez **Oui**.

1. Exécutez l’application à l’aide du débogueur Visual Studio Code.

1. Dans un navigateur, accédez à `https://localhost:5001`.

# <a name="visual-studio-for-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Le serveur éblouissant est pris en charge dans Visual Studio pour Mac. Le webassembly éblouissant n’est pas pris en charge pour le moment. Pour générer des applications webassembly éblouissantes sur macOS, suivez les instructions de l’onglet **CLI .net Core** .

1. Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).

1. Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.

1. Dans la barre latérale, sélectionnez **.net Core** > **application**.

1. Sélectionnez le modèle **application de serveur éblouissant** . Sélectionnez **Create** (Créer).

   Pour plus d’informations sur le modèle d’hébergement de serveur éblouissant, consultez <xref:blazor/hosting-models>.

1. Définissez la version **cible** de **.NET Framework sur .net Core 3,1** , puis sélectionnez **suivant**.

1. Dans le champ **nom du projet** , nommez l’application `WebApplication1`. Sélectionnez **Create** (Créer).

1. Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*. Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.

Si une invite s’affiche pour faire confiance au certificat de développement, approuvez le certificat et continuez.

# <a name="net-core-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

1. Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Si vous le souhaitez, vous pouvez installer le modèle d’aperçu de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) en exécutant la commande suivante :

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > La [version de kit SDK .net Core 3.1.201 ou ultérieure](https://dotnet.microsoft.com/download/dotnet-core/3.1) est **requise** pour utiliser le modèle webassembly 3,2 Preview 3 éblouissant. Confirmez la version de kit SDK .NET Core installée en exécutant `dotnet --version` dans un interpréteur de commandes.

1. Pour une expérience de serveur Blazor, exécutez les commandes suivantes dans une interface de commande :

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Pour une expérience de webassembly Blazor, exécutez les commandes suivantes dans une interface de commande :

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.

1. Dans un navigateur, accédez à `https://localhost:5001`.

---

Plusieurs pages sont disponibles à partir des onglets de la barre latérale :

* Accueil
* Compteur
* Extraire les données

Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page. L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais C#avec Blazor vous pouvez utiliser.

*Pages/Counter.razor* :

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Une demande d' `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`. Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.

Chaque fois que le bouton **Click Me** est sélectionné :

* L’événement `onclick` est déclenché.
* La méthode `IncrementCount` est appelée.
* Le `currentCount` est incrémenté.
* Le composant est de nouveau restitué.

Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).

Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML. Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.

*Pages/Index.razor* :

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Exécutez l'application. La page d’accueil possède son propre compteur fourni par le composant `Counter`.

Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant. Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :

* Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.
* Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.

*Pages/Counter.razor* :

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Spécifiez le `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.

*Pages/Index.razor* :

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Exécutez l'application. Le composant `Index` possède son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné. Le composant `Counter` (*Counter. Razor*) sur `/counter` continue d’être incrémenté d’une unité.

## <a name="next-steps"></a>Étapes suivantes

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/templates>
* <xref:signalr/introduction>
