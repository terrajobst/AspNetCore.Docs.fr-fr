---
title: Prise en main de ASP.NET Core Blazor
author: guardrex
description: Commencez avec Blazor en créant une application Blazor avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 09400a076849bdec35beb284a488d01feb8a84c2
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160000"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a>Prise en main de ASP.NET Core Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Prise en main de Blazor:

1. Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Installez éventuellement le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :
   * Installez le [Kit de développement logiciel (SDK) .net Core 3,1 ou ultérieur (version préliminaire)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Exécutez la commande suivante dans une interface de commande. [Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Le package de modèles a une version préliminaire alors que Blazor Webassembly est en préversion.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. Suivez les instructions de votre choix d’outils :

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Installez [Visual Studio 16,4 ou une version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .

   2 \. Créez un nouveau projet.

   3 \. Sélectionnez **Blazor application**. Sélectionnez **Suivant**.

   4 \. Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut. Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet. Sélectionnez **Créer**.

   5 \. Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** . Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** . Sélectionnez **Créer**. Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.

   6 \. Appuyez sur **Ctrl**+**F5** pour exécuter l’application.

   > [!NOTE]
   > Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension. L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. Installez [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.

   4 \. Ouvrez le dossier *WebApplication1* dans Visual Studio code.

   5 \. Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet. Sélectionnez **Oui**.

   6 \. Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code. Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.

   7 \. Dans un navigateur, accédez à `https://localhost:5001`.

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

   1 \. Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).

   2 \. Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.

   3 \. Dans la barre latérale, sélectionnez **.net Core** > **application**.

   4 \. Sélectionnez le modèle **application serveurBlazor** . Seul le modèle Blazor Server est disponible dans Visual Studio pour Mac pour l’instant. Pour une expérience Blazor webassembly, suivez les instructions de l’onglet **CLI .net Core** . Après avoir sélectionné le modèle Blazor Server, sélectionnez **suivant**. Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. Définissez la version **cible** de **.NET Framework sur .net Core 3,1** , puis sélectionnez **suivant**.

   6 \. Dans le champ **nom du projet** , nommez l’application `WebApplication1`. Sélectionnez **Créer**.

   7 \. Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*. Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.

   Si une invite s’affiche pour faire confiance au certificat de développement, approuvez le certificat et continuez.

   # <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

   Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.

   Dans un navigateur, accédez à `https://localhost:5001`.

   ---

Plusieurs pages sont disponibles à partir des onglets de la barre latérale :

* Accueil
* Counter
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

Exécutez l’application. La page d’accueil possède son propre compteur fourni par le composant `Counter`.

Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant. Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :

* Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.
* Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.

*Pages/Counter.razor* :

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Spécifiez le `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.

*Pages/Index.razor* :

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Exécutez l’application. Le composant `Index` possède son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné. Le composant `Counter` (*Counter. Razor*) sur `/counter` continue d’être incrémenté d’une unité.

## <a name="next-steps"></a>Étapes suivantes :

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/templates>
* <xref:signalr/introduction>
