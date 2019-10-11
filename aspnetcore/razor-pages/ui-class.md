---
title: Interface utilisateur Razor réutilisable dans les bibliothèques de classes avec ASP.NET Core
author: Rick-Anderson
description: Explique comment créer l’interface utilisateur de Razor réutilisables à l’aide de vues partielles dans une bibliothèque de classes dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: dcd24f7dafd198f88cdf84d1ab67c84f45428a95
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179334"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Créer une interface utilisateur réutilisable à l’aide du projet de bibliothèque de classes Razor dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Les vues Razor, les pages, les contrôleurs, les modèles de page, les [composants Razor](xref:blazor/class-libraries), les [composants de vue](xref:mvc/views/view-components)et les modèles de données peuvent être intégrés dans une bibliothèque de classes Razor (RCL). La RCL peut être empaquetée et réutilisée. Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient. Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Créer une bibliothèque de classes contenant l’interface utilisateur Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Nommez la bibliothèque (par exemple, « RazorClassLib ») > **OK**. Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.
* Vérifiez **ASP.NET Core 3,0** ou une version ultérieure est sélectionné.
* Sélectionnez **bibliothèque de classes Razor** > **OK**.

Le modèle de bibliothèque de classes Razor (RCL) est par défaut le développement de composants Razor par défaut. Une option de modèle dans Visual Studio fournit la prise en charge des modèles pour les pages et les vues.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir de la ligne de commande, exécutez `dotnet new razorclasslib`. Exemple :

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Le modèle de bibliothèque de classes Razor (RCL) est par défaut le développement de composants Razor par défaut. Transmettez l’option `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) pour assurer la prise en charge des pages et des vues.

Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new). Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.

---

Ajoutez des fichiers Razor à la RCL.

Les modèles ASP.NET Core supposent que le contenu RCL se trouve dans le *zones* dossier. Consultez [RCL pages layout](#rcl-pages-layout) pour créer un RCL qui expose du contenu dans `~/Pages` plutôt que `~/Areas/Pages`.

## <a name="reference-rcl-content"></a>Référencer le contenu RCL

La RCL peut être référencée par :

* Un package NuGet. Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* Un fichier *{NomProjet}.csproj*. Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="override-views-partial-views-and-pages"></a>Substituer des vues, des vues partielles et des pages

Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire. Par exemple, ajoutez *application Web 1/Areas/MyFeature/pages/Page1. cshtml* à application Web 1, et Page1 dans le application Web 1 aura priorité sur Page1 dans le RCL.

Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.

Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Mettez à jour le balisage pour indiquer le nouvel emplacement. Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.

### <a name="rcl-pages-layout"></a>Disposition des Pages de RCL

Pour référence RCL de contenu comme s’il faisait partie de l’application web *Pages* dossier, créez le projet RCL avec la structure de fichier suivante :

* *RazorUIClassLib/Pages*
* *RazorUIClassLib/Pages/Shared*

Supposons que *RazorUIClassLib/Pages/Shared* contient deux fichiers partielles : *_Header.cshtml* et *_Footer.cshtml*. Le `<partial>` balises peut être ajoutés à *_Layout.cshtml* fichier :

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>Créer un RCL avec des ressources statiques

Un RCL peut nécessiter des ressources statiques auxiliaires qui peuvent être référencées par l’application consommatrice de RCL. ASP.NET Core permet de créer des RCLs qui incluent des ressources statiques disponibles pour une application consommatrice.

Pour inclure des ressources d’accompagnement dans le cadre d’un RCL, créez un dossier *wwwroot* dans la bibliothèque de classes et incluez tous les fichiers nécessaires dans ce dossier.

Lors de la compression d’un RCL, toutes les ressources associées dans le dossier *wwwroot* sont automatiquement incluses dans le package.

### <a name="exclude-static-assets"></a>Exclure des ressources statiques

Pour exclure des ressources statiques, ajoutez le chemin d’exclusion souhaité au groupe de propriétés `$(DefaultItemExcludes)` dans le fichier projet. Séparez les entrées par un point-virgule (`;`).

Dans l’exemple suivant, la feuille de style *lib. CSS* du dossier *wwwroot* n’est pas considérée comme une ressource statique et n’est pas incluse dans le RCL publié :

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a>Intégration de la machine à écrire

Pour inclure des fichiers de machine à écrire dans un RCL :

1. Placez les fichiers de machine à écrire ( *. TS*) en dehors du dossier *wwwroot* . Par exemple, placez les fichiers dans un dossier *client* .

1. Configurez la sortie de génération de machine à écrire pour le dossier *wwwroot* . Définissez la propriété `TypescriptOutDir` à l’intérieur d’une `PropertyGroup` dans le fichier projet :

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. Incluez la cible de la machine à écrire en tant que dépendance de la cible `ResolveCurrentProjectStaticWebAssets` en ajoutant la cible suivante à l’intérieur d’une `PropertyGroup` dans le fichier projet :

   ```xml
  <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
    CompileTypeScript;
    $(ResolveCurrentProjectStaticWebAssetsInputs)
  </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a>Consommer du contenu à partir d’un RCL référencé

Les fichiers inclus dans le dossier *wwwroot* du RCL sont exposés à l’application consommatrice sous le préfixe `_content/{LIBRARY NAME}/`. Par exemple, une bibliothèque nommée *Razor. class. lib* génère un chemin d’accès au contenu statique à `_content/Razor.Class.Lib/`.

L’application consommatrice fait référence aux ressources statiques fournies par la bibliothèque avec `<script>`, `<style>`, `<img>` et d’autres balises HTML. L’application consommatrice doit avoir la [prise en charge des fichiers statiques](xref:fundamentals/static-files) activée dans `Startup.Configure` :

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

Lors de l’exécution de l’application consommatrice à partir de la sortie de génération (`dotnet run`), les ressources Web statiques sont activées par défaut dans l’environnement de développement. Pour prendre en charge les ressources dans d’autres environnements lors de l’exécution à partir de la sortie de génération, appelez `UseStaticWebAssets` sur le générateur d’ordinateur hôte dans *Program.cs*:

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

L’appel de `UseStaticWebAssets` n’est pas nécessaire lors de l’exécution d’une application à partir de la sortie publiée (`dotnet publish`).

### <a name="multi-project-development-flow"></a>Déroulement du développement de projets multiples

Lorsque l’application consommatrice s’exécute :

* Les ressources du RCL restent dans leurs dossiers d’origine. Les ressources ne sont pas déplacées vers l’application consommatrice.
* Toute modification dans le dossier *wwwroot* de RCL est reflétée dans l’application consommatrice une fois que le RCL est reconstruit et sans régénérer l’application consommateur.

Lorsque le RCL est généré, un manifeste qui décrit les emplacements des ressources Web statiques est généré. L’application consommatrice lit le manifeste au moment de l’exécution pour consommer les ressources des packages et des projets référencés. Lorsqu’un nouvel élément multimédia est ajouté à un RCL, le RCL doit être régénéré pour mettre à jour son manifeste avant qu’une application consommatrice puisse accéder au nouvel élément multimédia.

### <a name="publish"></a>Publier

Lorsque l’application est publiée, les ressources d’accompagnement de tous les packages et projets référencés sont copiées dans le dossier *wwwroot* de l’application publiée sous `_content/{LIBRARY NAME}/`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Les vues Razor, les pages, les contrôleurs, les modèles de page, les [composants Razor](xref:blazor/class-libraries), les [composants de vue](xref:mvc/views/view-components)et les modèles de données peuvent être intégrés dans une bibliothèque de classes Razor (RCL). La RCL peut être empaquetée et réutilisée. Les applications peuvent inclure la RCL et remplacer les vues et les pages qu’elle contient. Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Créer une bibliothèque de classes contenant l’interface utilisateur Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Nommez la bibliothèque (par exemple, « RazorClassLib ») > **OK**. Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.
* Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.
* Sélectionnez **bibliothèque de classes Razor** > **OK**.

Un RCL a le fichier projet suivant :

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir de la ligne de commande, exécutez `dotnet new razorclasslib`. Exemple :

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Pour plus d’informations, consultez [dotnet new](/dotnet/core/tools/dotnet-new). Pour éviter une collision de nom de fichier avec la bibliothèque de vues générée, vérifiez que le nom de la bibliothèque ne se termine pas par `.Views`.

---

Ajoutez des fichiers Razor à la RCL.

Les modèles ASP.NET Core supposent que le contenu RCL se trouve dans le *zones* dossier. Consultez [RCL pages layout](#rcl-pages-layout) pour créer un RCL qui expose du contenu dans `~/Pages` plutôt que `~/Areas/Pages`.

## <a name="reference-rcl-content"></a>Référencer le contenu RCL

La RCL peut être référencée par :

* Un package NuGet. Consultez [Création de packages NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) et [Créer et publier un package NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* Un fichier *{NomProjet}.csproj*. Consultez [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Procédure pas à pas : Créer un projet RCL et l’utiliser à partir d’un projet Razor Pages

Vous pouvez télécharger et tester le [projet complet](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) au lieu de le créer de toutes pièces. L’exemple proposé sous forme de téléchargement contient du code et des liens supplémentaires qui facilitent le test du projet. Si vous souhaitez commenter le mode d’obtention des exemples (téléchargement ou création au moyen d’instructions détaillées), entrez vos commentaires dans [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).

### <a name="test-the-download-app"></a>Tester l’application téléchargée

Si vous n’avez pas téléchargé l’application complète et que vous souhaitez créer le projet pas à pas, passez à la [section suivante](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ouvrez le fichier *.sln* dans Visual Studio. Exécuter l’application.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir d’une invite de commandes dans le répertoire *cli*, générez la RCL et l’application web.

```dotnetcli
dotnet build
```

Passez au répertoire *WebApp1* et exécutez l’application :

```dotnetcli
dotnet run
```

---

Suivez les instructions contenues dans [Test WebApp1](#test-webapp1)

## <a name="create-an-rcl"></a>Créer un RCL

Dans cette section, un RCL est créé. Des fichiers Razor sont ajoutés à la RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Créez le projet RCL :

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Nommez l’application **RazorUIClassLib** > **OK**.
* Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.
* Sélectionnez **bibliothèque de classes Razor** > **OK**.
* Ajoutez un fichier de vue partielle Razor nommé *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir de la ligne de commande, exécutez ce qui suit :

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Les commandes précédentes :

* Crée le `RazorUIClassLib` RCL.
* Créent une page _Message Razor et l’ajoutent à la RCL. Le paramètre `-np` crée la page sans `PageModel`.
* Crée un [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) de fichiers et l’ajoute à la RCL.

Le *_ViewStart.cshtml* fichier est nécessaire pour utiliser la disposition du projet de Pages Razor (qui est ajoutée à la section suivante).

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Ajouter les fichiers Razor et des dossiers au projet

* Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* par le code suivant :

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Remplacez le balisage dans *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* par le code suivant :

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` est nécessaire pour utiliser la vue partielle (`<partial name="_Message" />`). Au lieu d’inclure la directive `@addTagHelper`, vous pouvez ajouter un fichier *_ViewImports.cshtml*. Exemple :

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  Pour plus d’informations sur *_ViewImports.cshtml*, consultez [importation de Directives partagées](xref:mvc/views/layout#importing-shared-directives)

* Générez la bibliothèque de classes pour vérifier l’absence d’erreurs de compilateur :

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

La sortie de build contient *RazorUIClassLib.dll* et *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* contient le contenu Razor compilé.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Utiliser la bibliothèque de l’interface utilisateur Razor à partir d’un projet Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Créez l’application web Razor Pages :

* Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution > **Ajouter** >  **nouveau projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Nommez l’application **WebApp1**.
* Vérifiez que **ASP.NET Core 2.1** ou ultérieur est sélectionné.
* Sélectionnez **application Web** > **OK**.

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **WebApp1**, puis sélectionnez **Définir comme projet de démarrage**.
* Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **application Web 1** et sélectionnez **dépendances de build** > **dépendances de projet**.
* Cochez **RazorUIClassLib** comme dépendance de **WebApp1**.
* Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **application Web 1** et sélectionnez **Ajouter** > **référence**.
* Dans la boîte de dialogue **Gestionnaire de références** , activez la case à cocher **RazorUIClassLib** > **OK**.

Exécuter l’application.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Créez un Razor Pages application Web et un fichier solution contenant l’application Razor Pages et le RCL :

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Générez et exécutez l’application web :

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a>Tester WebApp1

Accédez à `/MyFeature/Page1` pour vérifier que la bibliothèque de classes de l’interface utilisateur Razor est en cours d’utilisation.

## <a name="override-views-partial-views-and-pages"></a>Substituer des vues, des vues partielles et des pages

Quand une vue, une vue partielle ou une page Razor est présente dans l’application web et la RCL, le balisage Razor (fichier *.cshtml*) dans l’application web est prioritaire. Par exemple, ajoutez *application Web 1/Areas/MyFeature/pages/Page1. cshtml* à application Web 1, et Page1 dans le application Web 1 aura priorité sur Page1 dans le RCL.

Dans l’exemple proposé sous forme de téléchargement, renommez *WebApp1/Areas/MyFeature2* en *WebApp1/Areas/MyFeature* pour tester la priorité.

Copiez la vue partielle *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* dans *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Mettez à jour le balisage pour indiquer le nouvel emplacement. Générez et exécutez l’application pour vérifier que la version de la vue partielle de l’application est utilisée.

### <a name="rcl-pages-layout"></a>Disposition des Pages de RCL

Pour référence RCL de contenu comme s’il faisait partie de l’application web *Pages* dossier, créez le projet RCL avec la structure de fichier suivante :

* *RazorUIClassLib/Pages*
* *RazorUIClassLib/Pages/Shared*

Supposons que *RazorUIClassLib/Pages/Shared* contient deux fichiers partielles : *_Header.cshtml* et *_Footer.cshtml*. Le `<partial>` balises peut être ajoutés à *_Layout.cshtml* fichier :

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
