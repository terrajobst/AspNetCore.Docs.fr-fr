---
title: Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core
author: rick-anderson
description: Apprendre à créer des profils de publication dans Visual Studio et les utiliser pour la gestion des déploiements d’applications ASP.NET Core pour différentes cibles.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core

De [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce document se concentre sur l’utilisation de Visual Studio 2017 pour créer et utiliser des profils de publication. Les profils de publication créées avec Visual Studio peuvent être exécutés à partir de MSBuild et Visual Studio 2017. Consultez [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour obtenir des instructions de publication sur Azure.

Le fichier projet suivant a été créé avec la commande `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

Le `<Project>` l’élément `Sdk` attribut effectue les tâches suivantes :

* Importe le fichier de propriétés de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* au début.
* Importe le fichier des cibles à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* à la fin.

L’emplacement par défaut de `MSBuildSDKsPath` (avec Visual Studio 2017 Enterprise) est le dossier *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

Le `Microsoft.NET.Sdk.Web` dépend de kit de développement logiciel :

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Ce qui entraîne l'import des propriétés et des cibles suivantes :

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Les cibles de publication importent le bon jeu de cibles en fonction de la méthode de publication utilisée.

Lorsque MSBuild ou Visual Studio charge un projet, les actions de niveau supérieur suivantes se produisent :

* Génération du projet
* Traitement des fichiers à publier
* Publication des fichiers sur la destination

## <a name="compute-project-items"></a>Traitement des éléments du projet

Quand le projet est chargé, les éléments du projet (fichiers) sont traités. L’attribut `item type` détermine comment le fichier est traité. Par défaut, les fichiers *.cs* sont inclus dans la liste d’éléments `Compile`. Les fichiers dans la liste d’éléments `Compile` sont compilés.

Le `Content` liste d’éléments contient des fichiers qui sont publiés en plus les sorties de génération. Par défaut, les fichiers correspondant au modèle `wwwroot/**` sont inclus dans le `Content` élément. Le `wwwroot/\*\*` [motif de la globalisation](https://gruntjs.com/configuring-tasks#globbing-patterns) correspond à tous les fichiers dans le *wwwroot* dossier **et** sous-dossiers. Pour ajouter explicitement un fichier à la liste de publication, ajoutez le fichier directement dans le *.csproj* de fichiers comme dans [des fichiers Include](#include-files).

Lorsque vous sélectionnez le bouton **publier** dans Visual Studio, ou lors de la publication à partir de la ligne de commande :

* Les éléments/propriétés sont traités (il s’agit des fichiers nécessaires à la génération).
* **Visual Studio seulement**: les packages NuGet sont restaurés. (La restauration doit être explicite par l’utilisateur sur l’interface en ligne de commande.)
* Le projet est généré.
* Les éléments de publication sont traités (il s’agit des fichiers nécessaires à la publication).
* Le projet est publié (les fichiers calculées sont copiés vers la destination de publication).

Lorsque un projet ASP.NET Core référence `Microsoft.NET.Sdk.Web` dans le fichier projet, un fichier *app_offline.htm* est placé à la racine du répertoire de l'application web. Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement. Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Publication en ligne de commande de base

Publication en ligne de commande fonctionne sur toutes les plateformes prises en charge de .NET Core et ne nécessite pas Visual Studio. Dans les exemples ci-dessous, la [dotnet publier](/dotnet/core/tools/dotnet-publish) commande est exécutée à partir du répertoire de projet (qui contient le *.csproj* fichier). S’il n’est pas dans le dossier du projet, passez explicitement le chemin du fichier projet. Par exemple :

```console
dotnet publish C:\Webs\Web1
```

Exécutez les commandes suivantes pour créer et publier une application web :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

Le [dotnet publier](/dotnet/core/tools/dotnet-publish) commande produit un résultat semblable au suivant :

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Le dossier de publication par défaut est `bin\$(Configuration)\netcoreapp<version>\publish`. La valeur par défaut pour `$(Configuration)` est *déboguer*. Dans l’exemple précédent, le `<TargetFramework>` est `netcoreapp2.0`.

`dotnet publish -h` affiche des informations d’aide relatives à la publication.

La commande suivante spécifie une build `Release` et le répertoire de publication :

```console
dotnet publish -c Release -o C:\MyWebs\test
```

Le [dotnet publier](/dotnet/core/tools/dotnet-publish) commande appels MSBuild, ce qui permet d’appeler le `Publish` cible. Les paramètres passés à `dotnet publish` sont passés à MSBuild. Le paramètre `-c` est mappé à la propriété MSBuild `Configuration`. Le paramètre `-o` est mappé à `OutputPath`.

Les propriétés MSBuild peuvent être passées à l’aide d’un des formats suivants :

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

La commande suivante publie une build `Release` sur un partage réseau :

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Le partage réseau est spécifié avec des barres obliques (*//r8/*) et fonctionne sur toutes les plateformes .NET Core prises en charge.

Vérifiez que l’application publiée pour le déploiement n’est pas en cours d’exécution. Les fichiers dans le dossier *publish* sont verrouillés quand l’application est en cours d’exécution. Le déploiement ne peut pas se produire car les fichiers verrouillés ne peuvent pas être copiés.

## <a name="publish-profiles"></a>Profils de publication

Cette section utilise Visual Studio 2017 pour créer un profil de publication. Une fois créé, la publication à partir de Visual Studio ou de la ligne de commande est disponible.

Publier les profils peuvent simplifier le processus de publication, et n’importe quel nombre de profils peut exister. Créer un profil de publication dans Visual Studio en choisissant l’un des chemins suivants :

* Cliquez sur le projet dans l’Explorateur de solutions et sélectionnez **publier**.
* Sélectionnez **publier &lt;project_name&gt;**  à partir de la **générer** menu.

Le **publier** onglet de la page de capacités de l’application s’affiche. Si le projet ne dispose pas d’un profil de publication, la page suivante s’affiche :

![L’onglet de la publication de la page de capacités de l’application](visual-studio-publish-profiles/_static/az.png)

Lorsque **dossier** est sélectionnée, spécifiez un chemin d’accès de dossier pour stocker les ressources publiées. Le dossier par défaut est *bin\Release\PublishOutput*. Cliquez sur le **créer un profil** bouton Terminer.

Une fois qu’un profil de publication est créé, le **publier** onglet modifications. Le profil nouvellement créé s’affiche dans une liste déroulante. Cliquez sur **créer nouveau profil** pour créer un autre profil.

![L’onglet de la publication de la page application capacités FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

L’Assistant Publication prend en charge les cibles de publication suivantes :

* Azure App Service
* Machines virtuelles Azure
* IIS, FTP, etc. (pour n’importe quel serveur web)
* Dossier
* Importer un profil

Pour plus d’informations, consultez [les options de publication sont adaptées à mes besoins](/visualstudio/ide/not-in-toc/web-publish-options).

Lors de la création d’un profil de publication avec Visual Studio, un *propriétés/PublishProfiles/&lt;profile_name&gt;.pubxml* fichier MSBuild est créé. Le *.pubxml* est un fichier de MSBuild et contient les paramètres de configuration de publication. Ce fichier peut être modifié pour personnaliser la génération et le processus de publication. Ce fichier est lu par le processus de publication. `<LastUsedBuildConfiguration>` est spécial, car elle est une propriété globale et ne doivent pas être dans n’importe quel fichier qui est importé dans la build. Consultez [MSBuild : comment définir la propriété de configuration](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) pour plus d’informations.

Lors de la publication vers une cible Azure, le *.pubxml* fichier contient l’identificateur de votre abonnement Azure. Avec ce type de cible, il est déconseillé d’ajouter ce fichier au contrôle de code source. Lors de la publication vers une cible non-Azure, il est recommandé d’archiver le *.pubxml* fichier.

Les informations sensibles (telles que le mot de passe de publication) sont chiffrées sur un par niveau de l’utilisateur/ordinateur. Il est stocké dans le *propriétés/PublishProfiles/&lt;profile_name&gt;. pubxml.user* fichier. Étant donné que ce fichier peut stocker des informations sensibles, il ne doit pas être archivé dans le contrôle de code source.

Pour une vue d’ensemble de la publication d’une application web sur ASP.NET Core, consultez [hôte et déployer](xref:host-and-deploy/index). Les tâches et cibles MSBuild nécessaires pour publier une application ASP.NET Core sont open source à https://github.com/aspnet/websdk.

`dotnet publish` permet de dossier, MSDeploy, et [Kudu](https://github.com/projectkudu/kudu/wiki) des profils de publication :

Dossier (fonctionne sur plusieurs plateformes) :

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (actuellement ce fonctionne uniquement dans Windows depuis MSDeploy n’est pas inter-plateformes) :

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Package MSDeploy (actuellement ce fonctionne uniquement dans Windows depuis MSDeploy n’est pas inter-plateformes) :

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Dans les exemples précédents, **ne** passer `deployonbuild` à `dotnet publish`.

Pour plus d’informations, consultez [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` prend en charge les API Kudu publier sur Azure à partir de n’importe quelle plateforme. Prend en charge les API Kudu, mais il est pris en charge par WebSDK pour inter-plateformes publier sur Azure de publication de Visual Studio.

Ajouter un profil de publication pour le *propriétés/PublishProfiles* dossier avec le contenu suivant :

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Exécutez la commande suivante pour compresser le contenu de la publication et le publier sur Azure à l’aide de l’API Kudu :

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Définissez les propriétés MSBuild suivantes lors de l’utilisation d’un profil de publication :

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Lors de la publication avec un profil nommé *FolderProfile*, une des commandes ci-dessous peuvent être exécutée :

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Lors de l’appel [dotnet build](/dotnet/core/tools/dotnet-build), elle appelle `msbuild` pour exécuter la build et de processus de publication. Appelant `dotnet build` ou `msbuild` équivaut lors du passage d’un profil de dossier. Lorsque vous appelez MSBuild directement sur Windows, la version de .NET Framework de MSBuild est utilisée. MSDeploy est actuellement limité aux ordinateurs Windows pour la publication. L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild, et MSBuild utilise MSDeploy sur les profils autres que les dossiers de profil. L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild (à l’aide de MSDeploy) et échoue (même en cas d’exécution sur une plateforme Windows). Pour publier avec un profil autre qu’un profil de dossier, appelez MSBuild directement.

Le profil de publication de type dossier suivant a été créé avec Visual Studio et publie sur un partage réseau :

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Notez que `<LastUsedBuildConfiguration>` a la valeur `Release`. Lors de la publication à partir de Visual Studio, la propriété de configuration `<LastUsedBuildConfiguration>` prend la valeur en vigueur lors du démarrage du processus de publication. Le `<LastUsedBuildConfiguration>` propriété de configuration est spéciale et ne doit pas être substituée dans un fichier MSBuild importé. Cette propriété peut être substituée à partir de la ligne de commande.

À l’aide du Kit de développement .NET Core CLI :

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

À l’aide de MSBuild :

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publier sur un point de terminaison MSDeploy à partir de la ligne de commande

La publication peut être accomplie à l’aide du .NET Core CLI ou MSBuild. `dotnet publish` s’exécute dans le contexte de .NET Core. Le `msbuild` commande nécessite .NET Framework, qui restreint aux environnements Windows.

Pour publier avec MSDeploy, le plus simple est d’abord de créer un profil de publication dans Visual Studio 2017, puis d’utiliser le profil à partir de la ligne de commande.

Dans l’exemple suivant, une application de web ASP.NET Core est créée (à l’aide de `dotnet new mvc`), et un profil de publication Azure est ajouté avec Visual Studio.

Exécutez `msbuild` d’un **invite de commandes développeur pour VS 2017**. L’invite de commandes développeur possède la bon *msbuild.exe* dans son chemin d’accès avec un ensemble de variables MSBuild.

MSBuild utilise la syntaxe suivante :

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Récupérez le `Password` auprès du fichier  *\<nom de publication >. PublishSettings*. Téléchargez le *.PublishSettings* à partir d’un fichier de :

* L’Explorateur de solutions : Avec le bouton droit sur l’application Web et sélectionnez **télécharger le profil de publication**.
* Portail Azure : cliquez sur **obtenir le profil de publication** sur l’application Web **vue d’ensemble** Panneau de configuration.

`Username` figure dans le profil de publication.

L’exemple suivant utilise le *Web11112 - Web Deploy* profil de publication :

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Exclure des fichiers

Lors de la publication d’applications web ASP.NET Core, les artefacts de build et le contenu du dossier *wwwroot* sont inclus. `msbuild` prend en charge les [modèles d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns). Par exemple, `<Content>` élément exclut tout le texte (*.txt*) fichiers à partir de la *wwwroot/contenu* dossier et tous ses sous-dossiers.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Le balisage précédent peut être ajouté à un profil de publication ou le *.csproj* fichier. En cas d’ajout au fichier *.csproj*, la règle est ajoutée à tous les profils de publication dans le projet.

Les éléments suivants `<MsDeploySkipRules>` élément exclut tous les fichiers à partir de la *wwwroot/contenu* dossier :

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` ne supprime pas les cibles *skip* du site de déploiement. Les fichiers et dossiers `<Content>` ciblés sont supprimés du site de déploiement. Par exemple,  supposez qu'une application web déployée avait les fichiers suivants :

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Si les éléments suivants `<MsDeploySkipRules>` les éléments sont ajoutés, ces fichiers ne pourraient être supprimées sur le site de déploiement.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

L’exemple précédent `<MsDeploySkipRules>` empêchent les éléments le *ignorée* fichiers d’être déployé. Elle ne supprime pas ces fichiers une fois qu’elles sont déployées.

Les éléments suivants `<Content>` élément supprime les fichiers ciblés sur le site de déploiement :

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

À l’aide du déploiement de ligne de commande avec l’exemple précédent `<Content>` élément génère la sortie suivante :

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>Fichiers Include

Le balisage suivant inclut un dossier *images* en dehors du répertoire de projet pour le dossier *wwwroot/images* du site de publication :

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Vous pouvez ajouter le balisage au fichier *.csproj* ou au profil de publication. S’il est ajouté au fichier *.csproj*, il est inclus dans chaque profil de publication dans le projet.

Le balisage mis en surbrillance ci-dessous montre comment :

* Copier un fichier situé en dehors du projet dans le dossier *wwwroot*
* Exclure le dossier *wwwroot\Content*
* Exclure *Views\Home\About2.cshtml*

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Pour obtenir d’autres exemples de déploiement, consultez le fichier [Lisez-moi webSDK](https://github.com/aspnet/websdk).

## <a name="run-a-target-before-or-after-publishing"></a>Exécuter une cible avant ou après la publication

La fonction intégrée `BeforePublish` et `AfterPublish` cibles d’exécutent d’une cible avant ou après la cible de publication. Pour le profil de publication pour enregistrer des messages de console avant et après la publication, ajoutez les éléments suivants :

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publier sur un serveur à l’aide d’un certificat non approuvé

Ajouter le `<AllowUntrustedCertificate>` propriété avec la valeur `True` pour le profil de publication :

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Le service Kudu

Pour afficher les fichiers dans un déploiement d’application Azure App Service web, utilisez le [service de Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Ajouter le `scm` le jeton dans le nom de l’application web. Par exemple :

| URL                                    | Résultat       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Application web      |
| `http://mysite.scm.azurewebsites.net/` | Service de kudu |

Sélectionnez le [Console de débogage](https://github.com/projectkudu/kudu/wiki/Kudu-console) élément de menu à afficher, modifier, supprimer ou ajouter des fichiers.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifie le déploiement des applications web et des sites web sur des serveurs IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Problèmes de fichiers et de demander des fonctionnalités pour le déploiement.
