---
title: Interface utilisateur Razor réutilisable dans les bibliothèques de classes avec ASP.NET Core
author: Rick-Anderson
description: Explique comment créer une interface utilisateur Razor réutilisable dans une bibliothèque de classes.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 44091ab8ab5d69a5975e191fd00fca1d1d957238
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Créez une interface utilisateur réutilisable à l’aide du projet Razor Class Library dans ASP.NET Core.

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Les vues, pages, contrôleurs, modèles de page et modèles de données Razor peuvent être intégrés à une bibliothèque de classes Razor (RCL, Razor Class Library). La RCL peut être empaquetée et réutilisée. Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient. Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.

Cette fonctionnalité nécessite [!INCLUDE[](~/includes/2.1-SDK.md)].

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Créer une bibliothèque de classes contenant l’interface utilisateur Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.
* Sélectionnez **Razor Class Library** > **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir de la ligne de commande, exécutez `dotnet new razorclasslib`. Exemple :

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new).

------
Ajoutez des fichiers Razor à la RCL.

Nous vous recommandons de placer le contenu RCL dans le dossier *Areas*. 


## <a name="referencing-razor-class-library-content"></a>Référencement de contenu RCL

La RCL peut être référencée par :

* Un package NuGet. Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* Un fichier *{NomProjet}.csproj*. Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

### <a name="partial-files-access-in-the-rcl"></a>Accès aux fichiers partiels dans la RCL

Pour le contenu situé hors de la RCL, le runtime ASP.NET Core ne recherche pas de fichiers partiels dans la RCL.

Par exemple, dans l’exemple de téléchargement, la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* **ne peut pas** être référencée dans *WebApp1\Pages\About.cshtml*. Toutefois, les pages dans la RCL *RazorUIClassLib/* **peuvent** accéder à *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Procédure pas à pas : Créer un projet RCL et l’utiliser à partir d’un projet de pages Razor

Vous pouvez télécharger et tester le [projet complet](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) au lieu de le créer de toutes pièces. L’exemple proposé sous forme de téléchargement contient du code et des liens supplémentaires qui facilitent le test du projet. Si vous souhaitez commenter le mode d’obtention des exemples (téléchargement ou création au moyen d’instructions détaillées), entrez vos commentaires dans [ce problème GitHub](https://github.com/aspnet/Docs/issues/6098).

### <a name="test-the-download-app"></a>Tester l’application téléchargée

Si vous n’avez pas téléchargé l’application complète et que vous souhaitez créer le projet pas à pas, passez à la [section suivante](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ouvrez le fichier *.sln* dans Visual Studio. Exécutez l’application.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir d’une invite de commandes dans le répertoire *cli*, générez la RCL et l’application web.

``` CLI
dotnet build
```

Passez au répertoire *WebApp1* et exécutez l’application :

``` CLI
dotnet run
```
------

Suivez les instructions contenues dans [Test WebApp1](#test)

## <a name="create-a-razor-class-library"></a>Créer une RCL

Dans cette section, vous allez créer une RCL. Des fichiers Razor sont ajoutés à la RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Créez le projet RCL :

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Nommez l’application **RazorUIClassLib**.
* Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.
* Sélectionnez **Razor Class Library** > **OK**.

Créez l’application web Razor Pages :

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur la solution > **Ajouter** >  **Nouveau projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Nommez l’application **WebApp1**.
* Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.
* Sélectionnez **Application web** > **OK**.

### <a name="add-razor-files-and-folders-to-the-project"></a>Ajouter des fichiers et des dossiers Razor au projet.

* Ajoutez un fichier de vue partielle Razor nommé *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.
* Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Copiez le fichier *_ViewStart.cshtml* du projet WebApp1 dans *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.

  Le fichier [viewstart](xref:mvc/views/layout#running-code-before-each-view) est nécessaire pour utiliser la disposition du projet Razor Pages.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir de la ligne de commande, exécutez ce qui suit :

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Les commandes précédentes :

* Créent la RCL `RazorUIClassLib`.
* Créent une page _Message Razor et l’ajoutent à la RCL. Le paramètre `-np` crée la page sans `PageModel`.
* Créent un fichier [viewstart](xref:mvc/views/layout#running-code-before-each-view) et l’ajoutent à la RCL.

Le fichier viewstart est nécessaire pour utiliser la disposition du projet de pages Razor (lequel est ajouté dans la section suivante).

Mettez à jour les pages Razor :

* Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* par le code suivant :

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` est nécessaire pour utiliser la vue partielle (`<partial name="_Message" />`). Au lieu d’inclure la directive `@addTagHelper`, vous pouvez ajouter un fichier *_ViewImports.cshtml*. Exemple :

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Pour plus d’informations sur viewimports, consultez [Importation de directives partagées](xref:mvc/views/layout#importing-shared-directives).

* Générez la bibliothèque de classes pour vérifier l’absence d’erreurs de compilateur :

``` CLI
dotnet build RazorUIClassLib
```

La sortie de build contient *RazorUIClassLib.dll* et *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* contient le contenu Razor compilé.

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Utiliser la bibliothèque de l’interface utilisateur Razor à partir d’un projet Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Définir comme projet de démarrage**.
* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Dépendances de build** > **Dépendances du projet**.
* Cochez **RazorUIClassLib** comme dépendance de **WebApp1**.
* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Ajouter** > **Référence**.
* Dans la boîte de dialogue **Gestionnaire de références**, cochez **RazorUIClassLib** > **OK**.

Exécutez l’application.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Créez une application web Razor Pages et un fichier de solution contenant l’application Razor Pages et la RCL :

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Générez et exécutez l’application web :

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a>Tester WebApp1

Vérifiez que la bibliothèque de classes de l’interface utilisateur Razor est utilisée.

* Accédez à `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Substituer des vues, des vues partielles et des pages

Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire. Par exemple, si vous ajoutez *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* à WebApp1, Page1 dans WebApp1 l’emporte sur Page1 dans la RCL.

Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.

Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Mettez à jour le balisage pour indiquer le nouvel emplacement. Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.
