---
title: Utiliser des fichiers statiques dans ASP.NET Core
author: rick-anderson
description: "Découvrez comment délivrer et sécuriser des fichiers statiques, et comment configurer le comportement d’un intergiciel (middleware) hébergeant des fichiers statiques dans une application web ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 7b156830ab59db3c08fbff6b2c4180d8765a113b
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>Utiliser des fichiers statiques dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Scott Addie](https://twitter.com/Scott_Addie)

Les fichiers statiques, comme les fichiers HTML, CSS, images et JavaScript, sont des ressources qu’une application ASP.NET Core délivre directement aux clients. Une configuration est nécessaire pour pouvoir délivrer ces fichiers.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Délivrer des fichiers statiques

Les fichiers statiques sont stockés dans le répertoire racine de votre projet web. Le répertoire par défaut est *\<racine_contenu>/wwwroot*, mais il peut être changé via la méthode [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_). Pour plus d’informations, consultez [Racine du contenu](xref:fundamentals/index#content-root) et [Racine web](xref:fundamentals/index#web-root).

L’hôte web de l’application doit être informé du répertoire racine du contenu.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

La méthode `WebHost.CreateDefaultBuilder` définit le répertoire actif comme racine du contenu :

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Définissez le répertoire actif comme racine du contenu en appelant [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) à l’intérieur de `Program.Main` :

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Les fichiers statiques sont accessibles via un chemin relatif à la racine web. Par exemple, le modèle de projet **Application web** contient plusieurs dossiers dans le dossier *wwwroot* :

* **wwwroot**
  * **css**
  * **images**
  * **js**

Le format d’URI pour accéder à un fichier dans le sous-dossier *images* est *http://\<adresse_serveur>/images/\<nom_fichier_image>*. Par exemple, *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si vous ciblez .NET Framework, ajoutez le package [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) à votre projet. Si vous ciblez .NET Core, le métapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) inclut ce package.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ajoutez le package [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) à votre projet.

---

Configurez le [middleware](xref:fundamentals/middleware/index) qui permet de délivrer des fichiers statiques.

### <a name="serve-files-inside-of-web-root"></a>Délivrer des fichiers dans la racine web

Appelez la méthode [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dans `Startup.Configure` :

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

La surcharge de la méthode `UseStaticFiles` sans paramètres marque les fichiers dans la racine web comme étant délivrables. Le balisage suivant référence *wwwroot/images/banner1.svg* :

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Délivrer des fichiers en dehors de la racine web

Considérez une hiérarchie de répertoires dans laquelle les fichiers statiques à délivrer se trouvent en dehors de la racine web :

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

Vous permettez à une requête d’accéder au fichier *banner1.svg* en configurant le middleware des fichiers statiques comme suit :

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

Dans le code précédent, la hiérarchie de répertoires *MyStaticFiles* est exposée publiquement via le segment d’URI *StaticFiles*. Une requête à *http://\<adresse_serveur>/StaticFiles/images/banner1.svg* délivre le fichier *banner1.svg*.

Le balisage suivant référence *MyStaticFiles/images/banner1.svg* :

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Définir des en-têtes de réponse HTTP

Un objet [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) peut être utilisé pour définir des en-têtes de réponse HTTP. En plus de configurer la possibilité de délivrer des fichiers statiques à partir de la racine web, le code suivant définit l’en-tête `Cache-Control` :

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

La méthode [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) se trouve dans le package [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).

Les fichiers peuvent être mis en cache publiquement pendant 10 minutes (600 secondes) :

![En-têtes de réponse montrant que l’en-tête Cache-Control a été ajouté](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorisations des fichiers statiques

Le middleware de fichiers statiques ne fournit pas de vérification des autorisations. Tous les fichiers qu’il délivre, notamment ceux sous *wwwroot*, sont accessibles publiquement. Pour délivrer des fichiers en fonction d’une autorisation :

* Stockez-les en dehors de *wwwroot* et de tout répertoire accessible au middleware de fichiers statiques **et**
* Délivrez-les via une méthode d’action à laquelle une autorisation est appliquée. Retournez un objet [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) :

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Activer l’exploration des répertoires

L’exploration des répertoires permet aux utilisateurs de votre application web de voir une liste des répertoires et des fichiers dans un répertoire spécifié. L’exploration des répertoires est désactivée par défaut pour des raisons de sécurité (consultez [Considérations](#considerations)). Activez l’exploration des répertoires en appelant la méthode [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) dans `Startup.Configure` :

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Ajoutez les services nécessaires en appelant la méthode [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) depuis `Startup.ConfigureServices` :

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Le code précédent permet l’exploration des répertoires du dossier *wwwroot/images* en utilisant l’URL *http://\<adresse_serveur>/MyImages*, avec des liens vers chaque fichier et dossier :

![exploration des répertoires](static-files/_static/dir-browse.png)

Consultez [Considérations](#considerations) sur les risques de sécurité lors de l’activation de l’exploration.

Notez les deux appels de `UseStaticFiles` dans l’exemple suivant. Le premier appel permet de délivrer des fichiers statiques dans le dossier *wwwroot*. Le deuxième appel active l’exploration des répertoires du dossier *wwwroot/images* en utilisant l’URL *http://\<adresse_serveur>/MyImages* :

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Délivrer un document par défaut

La définition d’une page d’accueil par défaut donne aux visiteurs un point de départ logique lors de la visite de votre site. Pour délivrer une page par défaut sans que l’utilisateur qualifie complètement l’URI, appelez la méthode [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) depuis `Startup.Configure` :

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles` doit être appelé avant `UseStaticFiles` pour délivrer le fichier par défaut. `UseDefaultFiles` est un module de réécriture d’URL qui ne délivre pas réellement le fichier. Activer le middleware de fichiers statiques via `UseStaticFiles` pour délivrer le fichier.

Avec `UseDefaultFiles`, les requêtes sur un dossier recherchent :

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

Le premier fichier trouvé dans la liste est délivré comme si la requête était l’URI qualifié complet. L’URL du navigateur continue de refléter l’URI demandé.

Le code suivant change le nom de fichier par défaut en *mydefault.html* :

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combine les fonctionnalités de `UseStaticFiles`, `UseDefaultFiles` et `UseDirectoryBrowser`.

Le code suivant active la possibilité de délivrer des fichiers statiques et le fichier par défaut. L’exploration des répertoires n’est pas activée.

```csharp
app.UseFileServer();
```

Le code suivant s’appuie sur la surcharge sans paramètre en activant l’exploration des répertoires :

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Considérez la hiérarchie de répertoires suivante :

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

Le code suivant active les fichiers statiques, les fichiers par défaut et l’exploration des répertoires de `MyStaticFiles` :

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser` doit être appelé quand la valeur de la propriété `EnableDirectoryBrowsing` est `true` :

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

En utilisant la hiérarchie de fichiers et le code précédent, les URL sont résolues comme suit :

| URI            |                             Réponse  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Si aucun fichier nommé default n’existe dans le répertoire *MyStaticFiles*, *http://\<adresse_serveur>/StaticFiles* retourne la liste des répertoires avec des liens cliquables :

![Liste des fichiers statiques](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles` et `UseDirectoryBrowser` utilisent l’URL *http://\<adresse_serveur>/StaticFiles* sans la barre oblique de fin pour déclencher une redirection côté client vers *http://\<adresse_serveur>/StaticFiles/*. Notez l’ajout de la barre oblique de fin. Les URL relatives dans les documents sont considérées comme non valides sans une barre oblique de fin.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

La classe [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) contient une propriété `Mappings` agissant comme un mappage des extensions de fichiers à des types de contenu MIME. Dans l’exemple suivant, plusieurs extensions de fichiers sont inscrites avec des types MIME connus. L’extension *.rtf* est remplacée et *.mp4* est supprimée.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Consultez [Types de contenu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Types de contenu non standard

Le middleware de fichiers statiques comprend près de 400 types de contenu de fichier. Si l’utilisateur demande un fichier d’un type inconnu, le middleware de fichiers statiques retourne une réponse HTTP 404 (Introuvable). Si l’exploration des répertoires est activée, un lien vers le fichier est affiché. L’URI retourne une erreur HTTP 404.

Le code suivant permet de délivrer des types inconnus et rend le fichier inconnu en tant qu’image :

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Avec le code précédent, une requête pour un fichier avec un type de contenu inconnu est retournée en tant qu’image.

> [!WARNING]
> L’activation de [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) présente un risque de sécurité. Il est désactivé par défaut et son utilisation est déconseillée. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) fournit une alternative plus sûre pour délivrer des fichiers avec des extensions non standard.

### <a name="considerations"></a>Éléments à prendre en considération

> [!WARNING]
> `UseDirectoryBrowser` et `UseStaticFiles` peuvent entraîner une fuite de secrets. La désactivation de l’exploration de répertoires est fortement recommandée en production. Examinez attentivement les répertoires qui sont activés via `UseStaticFiles` ou `UseDirectoryBrowser`. L’ensemble du répertoire et de ses sous-répertoires deviennent accessibles publiquement. Stockez les fichiers qui peuvent être délivrés au public dans un dossier dédié, comme *\<racine-contenu>/wwwroot*. Séparez ces fichiers des vues MVC, des Pages Razor (2.x uniquement), des fichiers de configuration, etc.

* Les URL pour le contenu exposé avec `UseDirectoryBrowser` et `UseStaticFiles` sont soumises aux restrictions de respect de la casse et de caractères du système de fichiers sous-jacent. Par exemple, Windows ne respecte pas la casse, mais Mac et Linux la respectent.

* Les applications ASP.NET Core hébergées dans IIS utilisent le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) pour transférer toutes les requêtes à l’application, notamment les demandes de fichiers statiques. Le gestionnaire de fichiers statiques d’IIS n’est pas utilisé. Il ne lui est pas permis de traiter les requêtes avant leur gestion par le module ASP.NET Core.

* Effectuez les étapes suivantes dans le Gestionnaire des services Internet (IIS) pour supprimer le gestionnaire de fichiers statiques d’IIS au niveau du serveur ou du site web :
    1. Accédez à la fonctionnalité **Modules**.
    1. Sélectionnez **StaticFileModule** dans la liste.
    1. Cliquez sur **Supprimer** dans l’encadré **Actions**.

> [!WARNING]
> Si le gestionnaire de fichiers statiques d’IIS est activé **et** que le module ASP.NET Core est configuré incorrectement, les fichiers statiques peuvent être délivrés. Cela se produit par exemple si le fichier *web.config* n’est pas déployé.

* Placez les fichiers de code (notamment *.cs* et *.cshtml*) en dehors de la racine web du projet d’application. Par conséquent, une séparation logique est créée entre le contenu côté client et le code basé sur le serveur de l’application. Ceci empêche la fuite de code côté serveur.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Présentation d’ASP.NET Core](xref:index)
