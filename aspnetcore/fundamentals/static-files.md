---
title: Travailler avec des fichiers statiques dans ASP.NET Core
author: rick-anderson
description: "Découvrez comment traiter et sécuriser les fichiers statiques et configurer les comportements d’intergiciel (middleware) d'hébergement de fichiers statiques dans une application web ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>Travailler avec des fichiers statiques dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Scott Addie](https://twitter.com/Scott_Addie)

Les fichiers statiques, tels que les fichiers HTML, CSS, images et JavaScript, sont des ressources qu'une application ASP.NET Core sert directement aux clients. Une configuration est requise pour permettre de servir ces fichiers.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Les fichiers statiques

Les fichiers statiques sont stockés dans le répertoire racine de votre projet web. Le répertoire par défaut est  *\<content_root>/wwwroot*, mais il peut être modifié via la méthode [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_). Consultez [racine du contenu](xref:fundamentals/index#content-root) et [racine Web](xref:fundamentals/index#web-root) pour plus d’informations.

L'hôte web de l’application doit être informé du répertoire racine du contenu.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le `WebHost.CreateDefaultBuilder` méthode définit la racine du contenu dans le répertoire actif :

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Définissez comme racine contenue dans le répertoire en cours en appelant [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) à l’intérieur de `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Les fichiers statiques sont accessibles via un chemin d’accès relatif à la racine web. Par exemple, le modèle de projet **Application Web** contient plusieurs dossiers dans le dossier *wwwroot* :

* **wwwroot**
  * **css**
  * **images**
  * **js**

Le format d’URI pour accéder à un fichier dans le sous-dossier *images* est *http://\<server_address>/images/\<nom_fichier_image>*. Par exemple, *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si vous ciblez le .NET Framework, ajoutez le package [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pour votre projet. Si vous ciblez .NET Core, le [metapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) inclut ce package.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si vous ciblez le .NET Framework, ajoutez le package [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) à votre projet.

---

Configurez [l’intergiciel (middleware)](xref:fundamentals/middleware) qui permet le traitement de fichiers statiques.

### <a name="serve-files-inside-of-web-root"></a>Les fichiers au sein de la racine web

Appelez le méthode [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dans `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

La surcharge de méthode sans paramètre `UseStaticFiles` marque les fichiers dans la racine web comme pouvant être traités. Le balisage suivant référence *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Les fichiers en dehors de la racine web

Considérez une hiérarchie de répertoires dans lesquels les fichiers statiques pris en charge résident en dehors de la racine web :

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

Une demande peut accéder au fichier *banner1.svg* en configurant l’intergiciel (middleware) de fichiers statiques comme suit :

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

Dans le code précédent, la hiérarchie de répertoires *MyStaticFiles* est exposée publiquement via le segment d’URI *StaticFiles*. Une demande de *http://\<server_address>/StaticFiles/images/banner1.svg* sert le fichier *banner1.svg*.

Les références suivantes de balisage *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Définir les en-têtes de réponse HTTP

Un objet [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) peut être utilisé pour définir les en-têtes de réponse HTTP. Outre la configuration des services de fichiers statiques à partir de la racine web, le code suivant définit l'en-tête `Cache-Control` :

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

La méthode [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) existe dans le package [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).

Les fichiers sont autorisés a être mis en cache publiquement pendant 10 minutes (600 secondes) :

![En-têtes de réponse indiquant l’en-tête Cache-Control a été ajouté.](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorisation de fichier statique

L’intergiciel (middleware) de fichier statique ne fournit pas les vérifications d’autorisation. Tous les fichiers pris en charge par, notamment ceux de *wwwroot*, sont accessibles publiquement. Pour traiter les fichiers en fonction de l’autorisation :

* Stockez-les en dehors de *wwwroot* et n’importe quel répertoire accessible à l’intergiciel (middleware) de fichiers statiques **et**
* Traitez-les via une méthode d’action à laquelle l’autorisation est appliquée. Retourner un objet [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) :

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Activer l’exploration des répertoires

L'exploration des répertoires permet aux utilisateurs de votre application web d'afficher une liste de répertoires et de fichiers de contenu dans un répertoire spécifié. L'exploration des répertoires est désactivée par défaut pour des raisons de sécurité (voir [considérations](#considerations)). Activez l'exploration de répertoire en appelant la méthode [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) dans `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Ajouter des services requis en appelant la méthode [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) à partir de `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Le code précédent permet l’exploration des répertoires du dossier *wwwroot/images* à l’aide de l’URL *http://\<server_address>/ MyImages*, avec des liens vers chaque fichier et dossier :

![exploration des répertoires](static-files/_static/dir-browse.png)

Consultez [considérations](#considerations) sur les risques de sécurité lors de l’exploration.

Notez les deux appels à `UseStaticFiles` dans l’exemple suivant. Le premier appel permet de servir les fichiers statiques dans le dossier *wwwroot*. Le deuxième appel permet l’exploration des répertoires du dossier *wwwroot/images* à l’aide de l’URL *http://\<server_address>/MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Prendre en charge un document par défaut

Définition d’une page d’accueil par défaut fournit des visiteurs un point de départ logique lors de la visite de votre site. Pour prendre en charge une page par défaut sans que l’utilisateur qualifié de l’URI, appelez le [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) méthode à partir de `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles` doit être appelé avant `UseStaticFiles` pour traiter le fichier par défaut. `UseDefaultFiles`est un module de réécriture d’URL qui ne sert pas réellement le fichier. Activer l’intergiciel (middleware) de fichiers statiques via `UseStaticFiles` pour traiter le fichier.

Avec `UseDefaultFiles`, les requêtes vers un dossier recherchent :

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

Le premier fichier trouvé dans la liste est traitée comme si la demande était l’URI complète. L’URL de navigateur continue de refléter l’URI demandée.

Le code suivant modifie le nom de fichier par défaut à *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combine les fonctionnalités de `UseStaticFiles`, `UseDefaultFiles`, et `UseDirectoryBrowser`.

Le code suivant active le traitement des fichiers statiques et du fichier par défaut

```csharp
app.UseFileServer();
```

Le code suivant s’appuie sur la surcharge sans paramètre en activant l’exploration des répertoires :

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Tenez compte de la hiérarchie de répertoires suivants :

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

Le code suivant active les fichiers statiques, les fichiers et par défaut, l'exploration des répertoires de `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`doit être appelé lorsque la valeur de propriété `EnableDirectoryBrowsing` est `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

En utilisant la hiérarchie des fichiers et le code qui précède, l'URL est résolue comme suit :

| URI            |                             Réponse  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Si aucun fichier de la valeur par défaut nommée existe dans le répertoire *MyStaticFiles*, *http://\<server_address>/StaticFiles* renvoie la liste des répertoires avec des liens cliquables :

![Liste de fichiers statiques](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`et `UseDirectoryBrowser` utilisent l’URL *http://\<server_address>/StaticFiles* sans la barre oblique pour déclencher une redirection côté client vers *http://\<server_address>/StaticFiles/*. Notez l’ajout de la barre oblique. Les URL relatives dans les documents sont considérées comme non valides sans barre oblique de fin.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

La classe [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) contient une propriété `Mappings` agissant comme un mappage des extensions de fichier pour les types de contenu MIME. Dans l’exemple suivant, plusieurs extensions de fichiers sont inscrites pour les types MIME connus. L'extension *.rtf* est remplacée, et *.mp4* est supprimé.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Consultez [les types de contenu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Types de contenu non standard

L’intergiciel (middleware) de fichiers statiques comprend presque 400 types de contenu connus. Si l’utilisateur demande un fichier d’un type de fichier inconnu, l’intergiciel (middleware) de fichier statique retourne une réponse HTTP 404 (introuvable). Si l’exploration des répertoires est activée, un lien vers le fichier s’affiche. L’URI retourne une erreur HTTP 404.

Le code suivant permet de servir des types inconnus et rend le fichier inconnu en tant qu’image :

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Avec le code précédent, une demande pour un fichier avec un type de contenu inconnu est retournée en tant qu’image.

> [!WARNING]
> L’activation de [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) est un risque de sécurité. Il est désactivé par défaut, et son utilisation est déconseillée. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) fournit une alternative plus sûre pour servir des fichiers avec les extensions non standard.

### <a name="considerations"></a>Éléments à prendre en considération

> [!WARNING]
> `UseDirectoryBrowser`et `UseStaticFiles` peut entraîner une fuite de secrets. Désactiver l’exploration de répertoire en production est fortement recommandé. Lisez attentivement les répertoires qui sont activés via `UseStaticFiles` ou `UseDirectoryBrowser`. L’ensemble du répertoire et ses sous-répertoires deviennent accessibles publiquement. Stocker les fichiers destinés à être servis au public dans un dossier dédié, tel que  *\<content_root>/wwwroot*. Séparez ces fichiers des vues MVC, des Pages Razor (2.x uniquement), des fichiers de configuration, etc.

* Les URL pour le contenu exposé avec `UseDirectoryBrowser` et `UseStaticFiles` sont soumises aux restrictions de caractères du système de fichiers sous-jacent et de respect de la casse. Par exemple, Windows respecte la casse&mdash;Mac et Linux ne le font pas.

* Les applications ASP.NET Core hébergées dans IIS utilisent le [Module ASP.NET Core (ANCM)](xref:fundamentals/servers/aspnet-core-module) pour transférer toutes les demandes à l’application, y compris les demandes de fichiers statiques. Le Gestionnaire de fichiers statiques d’IIS n’est pas utilisé. Il n'a pas de possibilité de traiter les demandes avant qu’ils soient gérés par le ANCM.

* Effectuez les étapes suivantes dans le Gestionnaire IIS pour supprimer le gestionnaire de fichiers statiques IIS au niveau du serveur ou du site Web :
    1. Accédez à la fonctionnalité **Modules**.
    1. Sélectionnez **StaticFileModule** dans la liste.
    1. Cliquez sur **supprimer** dans la barre latérale des **Actions**.

> [!WARNING]
> Si le gestionnaire de fichiers statiques d’IIS est activé **et** que le module ASP.NET Core est incorrectement configuré, les fichiers statiques sont pris en charge. Cela se produit par exemple si le fichier *web.config* n’est pas déployé.

* Placez les fichiers de code (y compris *.cs* et *.cshtml*) en dehors de la racine web du projet d’application. Par conséquent, une séparation logique est créée entre le contenu côté client et le code basé sur le serveur de l’application. Cela empêche le code côté serveur d'avoir des fuites.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Intergiciel (middleware)](xref:fundamentals/middleware)

* [Présentation d’ASP.NET Core](xref:index)
