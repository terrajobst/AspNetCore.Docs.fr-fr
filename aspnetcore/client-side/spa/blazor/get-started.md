---
title: Prise en main Blazor
author: guardrex
description: Découvrez comment bien démarrer avec Blazor en créant et modifiant un projet Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 667c57d536450fa2f8ae1cabc7c5a76a16d38a55
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665585"
---
# <a name="get-started-with-blazor"></a>Prise en main Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Conditions préalables :

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Pour créer votre premier projet Blazor dans Visual Studio :

1. Installez la dernière version [extension des Services de langage Blazor](https://go.microsoft.com/fwlink/?linkid=870389) à partir de Visual Studio Marketplace. Cette étape permet de Blazor modèles disponibles pour Visual Studio.
1. Rendre les modèles Blazor disponible pour une utilisation avec l’interface CLI .NET Core en exécutant la commande suivante dans une invite de commandes :

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Sélectionnez **fichier** > **nouveau projet** > **Web** > **Application Web ASP.NET Core**.
1. Assurez-vous que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés en haut.
1. Choisissez le modèle **Blazor** et sélectionnez **OK**.
1. Appuyez sur **F5** pour exécuter l'application.

Félicitations ! Vous venez d’exécuter votre première application Blazor !

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

Conditions préalables :

* [Afficher un aperçu de .NET core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Ajouter les modèles de Blazor en exécutant la commande suivante dans une invite de commandes :

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Créer votre premier projet Blazor dans une invite de commandes :

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. Dans un navigateur, accédez à `https://localhost:5001`.

Félicitations ! Vous venez d’exécuter votre première application Blazor !

---

## <a name="blazor-project"></a>Projet de Blazor

Quand l’application est exécutée, plusieurs pages sont disponibles à partir des onglets dans la barre latérale :

* Accueil
* Counter
* Récupérer des données

Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page. Incrémenter un compteur dans une page Web normalement requiert l’écriture de JavaScript, mais Blazor fournit une meilleure approche à l’aide C#.

*Pages/Counter.cshtml* :

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Une demande de `/counter` dans le navigateur, comme spécifié par le `@page` directive en haut, entraîne le composant de compteur afficher son contenu. Composants restituent dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisé pour mettre à jour l’interface utilisateur de manière flexible et efficace.

Chaque fois que le **Click me** bouton est sélectionné :

* Le `onclick` événement est déclenché.
* La méthode `IncrementCount` est appelée.
* Le `currentCount` est incrémenté.
* Le composant est une nouvelle fois restitué.

Le runtime compare le nouveau contenu au contenu précédent et s’applique uniquement le contenu est modifié pour le modèle DOM (Document Object).

Ajouter un composant à un autre composant à l’aide d’une syntaxe de type HTML. Paramètres de composant sont spécifiés à l’aide d’attributs ou contenu enfant. Par exemple, un composant de compteur peut être ajouté à la page d’accueil de l’application en ajoutant un `<Counter />` élément pour le composant d’Index.

Dans *pages/index.cshtml*, remplacez le composant enquête invite avec un composant de compteur :

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Exécuter l’application. La page d’accueil a son propre compteur.

Pour ajouter un paramètre au composant de compteur, mettre à jour du composant `@functions` bloc :

* Ajouter une propriété pour `IncrementAmount` décorée avec le `[Parameter]` attribut.
* Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.

*Pages/Counter.cshtml* :

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.

*Pages/Index.cshtml* :

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Exécuter l’application. La page d’accueil a son propre compteur incrémente par dix chaque fois que le **Click me** bouton est sélectionné.

## <a name="next-steps"></a>Étapes suivantes

<xref:tutorials/first-razor-components-app>
