---
title: Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core
author: rick-anderson
description: Découvrez comment créer des profils de publication dans Visual Studio et les utiliser pour gérer les déploiements d’applications ASP.NET Core sur différentes cibles.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3d24cd2cd4697e8e7cf7e4bdf4d076a09b6a6a23
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284706"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core

De [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) et [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="<= aspnetcore-1.1"

Pour obtenir la version 1.1 de cette rubrique, téléchargez les [profils de publication Visual Studio pour un déploiement d’application ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).

::: moniker-end

Ce document est axé sur l’utilisation de Visual Studio 2017 ou d’une version ultérieure pour créer et utiliser des profils de publication. Les profils de publication créés avec Visual Studio peuvent être exécutés à partir de MSBuild et Visual Studio. Consultez [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour obtenir des instructions de publication sur Azure.

Le fichier projet suivant a été créé avec la commande `dotnet new mvc` :

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

L’attribut `Sdk` de l’élément `<Project>` effectue les tâches suivantes :

* Importation du fichier de propriétés à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* au début.
* Importation du fichier targets à partir de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* à la fin.

L’emplacement par défaut de `MSBuildSDKsPath` (avec Visual Studio 2017 Enterprise) est le dossier *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

Le SDK `Microsoft.NET.Sdk.Web` dépend des éléments suivants :

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Ce qui entraîne l’importation des propriétés et cibles suivantes :

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Les cibles de publication importent l’ensemble approprié de cibles en fonction de la méthode de publication utilisée.

Quand MSBuild ou Visual Studio charge un projet, les actions principales suivantes se produisent :

* Génération du projet
* Calcul des fichiers à publier
* Publication des fichiers sur la destination

## <a name="compute-project-items"></a>Calcul des éléments du projet

Quand le projet est chargé, les éléments du projet (fichiers) sont calculés. L’attribut `item type` détermine comment le fichier est traité. Par défaut, les fichiers *.cs* sont inclus dans la liste d’éléments `Compile`. Les fichiers dans la liste d’éléments `Compile` sont compilés.

La liste d’éléments `Content` contient des fichiers qui sont publiés en plus des sorties de génération. Par défaut, les fichiers qui correspondent au modèle `wwwroot/**` sont inclus dans l’élément `Content`. Le [modèle d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot/\*\*` correspond à tous les fichiers dans le dossier *wwwroot* **et** les sous-dossiers. Pour ajouter explicitement un fichier à la liste de publication, ajoutez-le directement au fichier *.csproj* comme indiqué dans [ Inclure des fichiers](#include-files).

Quand vous sélectionnez le bouton **Publier** dans Visual Studio ou quand vous publiez à partir de la ligne de commande :

* Les éléments/propriétés sont calculés (il s’agit des fichiers nécessaires à la génération).
* **Visual Studio uniquement** : Les packages NuGet sont restaurés. (La restauration doit être explicite par l’utilisateur sur l’interface CLI.)
* Le projet est généré.
* Les éléments de publication sont calculés (il s’agit des fichiers nécessaires à la publication).
* Le projet est publié (les fichiers calculés sont copiés sur la destination de publication).

Quand un projet ASP.NET Core référence `Microsoft.NET.Sdk.Web` dans le fichier projet, un fichier *app_offline.htm* est placé à la racine du répertoire des applications web. Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement. Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Publication de base à partir d’une ligne de commande

La publication à partir d’une ligne de commande fonctionne sur toutes les plateformes .NET Core prises en charge et ne nécessite pas Visual Studio. Dans les exemples ci-dessous, la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) est exécutée à partir du répertoire du projet (qui contient le fichier *.csproj*). Si le dossier actuel n’est pas le dossier du projet, transmettez explicitement le chemin du fichier projet. Par exemple :

```console
dotnet publish C:\Webs\Web1
```

Exécutez les commandes suivantes pour créer et publier une application web :

```console
dotnet new mvc
dotnet publish
```

La commande [dotnet publish](/dotnet/core/tools/dotnet-publish) produit un résultat similaire au suivant :

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Le dossier de publication par défaut est `bin\$(Configuration)\netcoreapp<version>\publish`. La valeur par défaut de `$(Configuration)` est *Debug*. Dans l’exemple précédent, `<TargetFramework>` a pour valeur `netcoreapp2.0`.

`dotnet publish -h` affiche des informations d’aide relatives à la publication.

La commande suivante spécifie une build `Release` et le répertoire de publication :

```console
dotnet publish -c Release -o C:\MyWebs\test
```

La commande [dotnet publish](/dotnet/core/tools/dotnet-publish) appelle MSBuild, qui appelle la cible `Publish`. Les paramètres passés à `dotnet publish` sont passés à MSBuild. Le paramètre `-c` est mappé à la propriété MSBuild `Configuration`. Le paramètre `-o` est mappé à `OutputPath`.

Les propriétés MSBuild peuvent être passées à l’aide de l’un des formats suivants :

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

La commande suivante publie une build `Release` sur un partage réseau :

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Le partage réseau est spécifié avec des barres obliques (*//r8/*) et fonctionne sur toutes les plateformes .NET Core prises en charge.

Vérifiez que l’application publiée pour le déploiement n’est pas en cours d’exécution. Les fichiers dans le dossier *publish* sont verrouillés quand l’application est en cours d’exécution. Le déploiement ne peut pas se produire car les fichiers verrouillés ne peuvent pas être copiés.

## <a name="publish-profiles"></a>Profils de publication

Cette section utilise Visual Studio 2017 ou une version ultérieure pour créer un profil de publication. Une fois le profil créé, la publication à partir de Visual Studio ou de la ligne de commande est disponible.

Les profils de publication peuvent simplifier le processus de publication, et n’importe quel nombre de profils peut exister. Créez un profil de publication dans Visual Studio en choisissant l’une des méthodes suivantes :

* Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez **Publier**.
* Sélectionner **Publier {NOM DU PROJET}** dans le menu **Générer**.

L’onglet **Publier** de la page de capacités d’application s’affiche. Si le projet n’a pas de profil de publication, la page suivante s’affiche :

![Onglet Publier de la page de capacités d’application](visual-studio-publish-profiles/_static/az.png)

Quand **Dossier** est sélectionné, spécifiez un chemin de dossier pour stocker les ressources publiées. Le dossier par défaut est *bin\Release\PublishOutput*. Cliquez sur le bouton **Créer un profil** pour terminer.

Une fois créé un profil de publication, l’onglet **Publier** change. Le profil créé apparaît dans une liste déroulante. Cliquez sur **Créer un profil** pour créer un autre profil.

![Onglet Publier de la page de capacités d’application montrant FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

L’Assistant Publication prend en charge les cibles de publication suivantes :

* Azure App Service
* Machines virtuelles Azure
* IIS, FTP, etc. (pour n’importe quel serveur web)
* Dossier
* Profil d’importation

Consultez [Quelles options de publication choisir ?](/visualstudio/ide/not-in-toc/web-publish-options) pour plus d’informations.

Quand vous créez un profil de publication avec Visual Studio, un fichier MSBuild *Properties/PublishProfiles/{NOM DU PROFIL}.pubxml* est créé. Le fichier *.pubxml* est un fichier MSBuild qui contient des paramètres de configuration de publication. Vous pouvez changer ce fichier pour personnaliser le processus de génération et de publication. Ce fichier est lu par le processus de publication. `<LastUsedBuildConfiguration>` est spéciale, car il s’agit d’une propriété globale qui ne doit être dans aucun fichier importé dans la build. Pour plus d’informations, consultez [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (Comment définir la propriété de configuration).

Dans le cas d’une publication sur une cible Azure, le fichier *.pubxml* contient l’identificateur de votre abonnement Azure. Avec ce type de cible, nous vous déconseillons d’ajouter ce fichier au contrôle de code source. Dans le cas d’une publication sur une cible non-Azure, nous vous recommandons d’archiver le fichier *.pubxml*.

Les informations sensibles (telles que le mot de passe de publication) sont chiffrées par utilisateur/machine. Elles sont stockées dans le fichier *Properties/PublishProfiles/{NOM DU PROFIL}.pubxml.user*. Ce fichier pouvant stocker des informations sensibles, il ne doit pas être archivé dans le contrôle de code source.

Pour obtenir une vue d’ensemble expliquant comment publier une application web sur ASP.NET Core, consultez [Héberger et déployer](xref:host-and-deploy/index). Les tâches et cibles MSBuild nécessaires pour publier une application ASP.NET Core sont disponibles au format open source à l’adresse https://github.com/aspnet/websdk.

`dotnet publish` peut utiliser les profils de publication de dossier, MSDeploy et [Kudu](https://github.com/projectkudu/kudu/wiki) :

Dossier (fonctionne sur plusieurs plateformes) :

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (ne fonctionne que sur Windows car MSDeploy n’est pas multiplateforme) :

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Package MSDeploy (ne fonctionne que sur Windows car MSDeploy n’est pas multiplateforme) :

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Dans les exemples précédents, **ne** transmettez pas `deployonbuild` à `dotnet publish`.

Pour plus d’informations, consultez [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` prend en charge les API Kudu pour publier sur Azure à partir de n’importe quelle plateforme. La publication Visual Studio prend en charge les API Kudu, mais avec la prise en charge par WebSDK pour la publication multiplateforme sur Azure.

Ajoutez un profil de publication au dossier *Properties/PublishProfiles* avec le contenu suivant :

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

Exécutez la commande suivante pour compresser le contenu de la publication et le publier sur Azure à l’aide des API Kudu :

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Définissez les propriétés MSBuild suivantes lors de l’utilisation d’un profil de publication :

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

Pendant la publication avec un profil nommé *FolderProfile*, l’une des commandes ci-dessous peut être exécutée :

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Quand elle est appelée, la commande [dotnet build](/dotnet/core/tools/dotnet-build) appelle `msbuild` pour exécuter les processus de génération et de publication. L’appel de `dotnet build` ou `msbuild` est équivalent au moment du passage d’un profil de dossier. Quand vous appelez MSBuild directement sur Windows, la version du .NET Framework de MSBuild est utilisée. MSDeploy est actuellement limité aux ordinateurs Windows pour la publication. L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild, et MSBuild utilise MSDeploy sur les profils autres que les dossiers de profil. L’appel de `dotnet build` sur un profil autre qu’un profil de dossier entraîne l’appel de MSBuild (à l’aide de MSDeploy) et échoue (même en cas d’exécution sur une plateforme Windows). Pour publier avec un profil autre qu’un profil de dossier, appelez MSBuild directement.

Le profil de publication de dossier suivant a été créé avec Visual Studio et publie sur un partage réseau :

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

Notez que `<LastUsedBuildConfiguration>` a la valeur `Release`. Lors de la publication à partir de Visual Studio, la propriété de configuration `<LastUsedBuildConfiguration>` prend la valeur en vigueur lors du démarrage du processus de publication. La propriété de configuration `<LastUsedBuildConfiguration>` est spéciale et ne doit pas être substituée dans un fichier MSBuild importé. Cette propriété peut être substituée à partir de la ligne de commande.

À l’aide de CLI .NET Core :

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

À l’aide de MSBuild :

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publier sur un point de terminaison MSDeploy à partir de la ligne de commande

L’exemple suivant utilise une application web ASP.NET Core créée par Visual Studio et nommée *AzureWebApp*. Un profil de publication Azure Apps est ajouté avec Visual Studio. Pour plus d’informations sur la création d’un profil, consultez la section [profils de publication](#publish-profiles).

Pour déployer l’application à l’aide d’un profil de publication, exécutez la `msbuild` commande à partir d’une **invite de commandes développeur** Visual Studio. L’invite de commandes est disponible dans le dossier *Visual Studio* du menu **Démarrer** sur la barre des tâches Windows. Pour en faciliter accès, vous pouvez ajouter à l’invite de commandes au menu **Outils** dans Visual Studio. Pour plus d’informations, consultez [Invite de commandes développeur pour Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

MSBuild utilise la syntaxe de commande suivante :

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {PATH} &ndash; Chemin d’accès vers le fichier de projet de l’application.
* {PROFILE} &ndash; Nom du profil de publication.
* {USERNAME} &ndash; Nom d’utilisateur MSDeploy. {USERNAME} figure dans le profil de publication.
* {PASSWORD} &ndash; Mot de passe MSDeploy. Obtenez le {PASSWORD} à partir du fichier *{PROFILE}. PublishSettings*. Téléchargez le fichier *.PublishSettings* à partir d’un des emplacements suivants :
  * Explorateur de solutions : Sélectionnez **Vue** > **Cloud Explorer**. Connectez-vous avec votre abonnement Azure. Ouvrez **App Services**. Faites un clic droit sur l’application. Sélectionnez **Télecharger un profil de publication**.
  * Portail Azure : cliquez sur **Obtenir le profil de publication** dans le panneau **Vue d’ensemble** de l’application web.

L’exemple suivant utilise un profil de publication nommé *AzureWebApp - Web Deploy*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Un profil de publication peut également être utilisé avec la commande [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) de l’interface CLI .NET Core depuis une invite de commandes Windows :

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> La commande [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) est multiplateforme et peut compiler des applications ASP.NET Core sur macOS et Linux. Toutefois, MSBuild sur macOS et Linux n’est pas capable de déployer une application dans Azure ou un autre point de terminaison MSDeploy. MSDeploy est disponible uniquement sur Windows.

## <a name="exclude-files"></a>Exclure des fichiers

Lors de la publication d’applications web ASP.NET Core, les artefacts de build et le contenu du dossier *wwwroot* sont inclus. `msbuild` prend en charge les [modèles d’utilisation des caractères génériques](https://gruntjs.com/configuring-tasks#globbing-patterns). Par exemple, l’élément `<Content>` suivant exclut tous les fichiers texte (*.txt*) du dossier *wwwroot/content* et tous ses sous-dossiers.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Vous pouvez ajouter le balisage précédent à un profil de publication ou au fichier *.csproj*. En cas d’ajout au fichier *.csproj*, la règle est ajoutée à tous les profils de publication dans le projet.

L’élément `<MsDeploySkipRules>` suivant exclut tous les fichiers du dossier *wwwroot/content* :

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` ne supprime pas les cibles *skip* du site de déploiement. Les dossiers et fichiers ciblés par `<Content>` sont supprimés du site de déploiement. Par exemple, supposez qu’une application web déployée avait les fichiers suivants :

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Si les éléments `<MsDeploySkipRules>` suivants sont ajoutés, ces fichiers ne sont pas supprimés sur le site de déploiement.

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

Les éléments `<MsDeploySkipRules>` précédents empêchent le déploiement des fichiers *skipped*. Ces fichiers ne sont pas supprimés une fois déployés.

L’élément `<Content>` suivant supprime les fichiers ciblés sur le site de déploiement :

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

L’utilisation du déploiement à partir de la ligne de commande avec l’élément `<Content>` précédent génère la sortie suivante :

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

Le balisage suivant inclut un dossier *images* situé en dehors du répertoire de projet dans le dossier *wwwroot/images* du site de publication :

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Vous pouvez ajouter le balisage au fichier *.csproj* ou au profil de publication. Si vous l’ajoutez au fichier *csproj*, il est inclus dans chaque profil de publication du projet.

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

Les cibles intégrées `BeforePublish` et `AfterPublish` exécutent une cible avant ou après la cible de publication. Ajoutez les éléments suivants au profil de publication pour journaliser les messages de console avant et après la publication :

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publier sur un serveur à l’aide d’un certificat non approuvé

Ajoutez la propriété `<AllowUntrustedCertificate>` avec la valeur `True` au profil de publication :

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Le service Kudu

Pour afficher les fichiers dans un déploiement d’application web Azure App Service, utilisez le [service Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Ajoutez le jeton `scm` au nom de l’application web. Par exemple :

| URL                                    | Résultat       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Application web      |
| `http://mysite.scm.azurewebsites.net/` | Service Kudu |

Sélectionnez l’élément de menu [Console de débogage](https://github.com/projectkudu/kudu/wiki/Kudu-console) pour afficher, modifier, supprimer ou ajouter des fichiers.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifie le déploiement des applications web et des sites web sur les serveurs IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues) : soumettez vos problèmes et demandez des fonctionnalités pour le déploiement.
* [Publier une application web ASP.NET sur une machine virtuelle Azure à partir de Visual Studio](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
