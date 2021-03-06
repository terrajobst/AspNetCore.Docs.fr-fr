---
title: Développer des applications ASP.NET Core à l’aide de OpenAPI
author: ryanbrandenburg
description: Montre comment utiliser l’outil « Microsoft. dotnet-openapi » pour ajouter des références aux fichiers OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 079e36511b63c186ffa7726bdb1e3c3bcbda9d34
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655266"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a>Développer des applications ASP.NET Core à l’aide des outils OpenAPI

Par Ryan Brandenburg

[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) est un [outil Global .net Core](/dotnet/core/tools/global-tools) pour la gestion des références [openapi](https://github.com/OAI/OpenAPI-Specification) dans un projet.

## <a name="installation"></a>Installation

Pour installer `Microsoft.dotnet-openapi`, exécutez la commande suivante :

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a>Ajouter

L’ajout d’une référence OpenAPI à l’aide de l’une des commandes de cette page ajoute un élément `<OpenApiReference />` semblable au suivant au fichier *. csproj* :

```xml
<OpenApiReference Include="openapi.json" />
```

La référence précédente est requise pour que l’application appelle le code client généré.

<!-- TODO: Restore after https://github.com/dotnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a>Ajouter un fichier

#### <a name="options"></a>Options

| Option Short| Option longue| Description | Exemple |
|-------|------|-------|---------|
| -p|--updateProject | Projet sur lequel effectuer l’opération. |Ajouter un fichier à dotnet openapi *--updateProject .\Ref.csproj* .\OpenAPI.JSON |
| -c|--Code-Generator| Générateur de code à appliquer à la référence. Les options sont `NSwagCSharp` et `NSwagTypeScript`. Si `--code-generator` n’est pas spécifié, les outils sont définis par défaut sur `NSwagCSharp`.|dotnet openapi ajouter un fichier .\OpenApi.json--Code-Generator
| -H|--help|Afficher les informations d’aide|Ajouter un fichier Dotnet openapi--Help|

#### <a name="arguments"></a>Arguments

|  Argument  | Description | Exemple |
|-------------|-------------|---------|
| fichier source | Source à partir de laquelle créer une référence. Doit être un fichier OpenAPI. |dotnet openapi ajouter un fichier *.\OpenAPI.JSON* |

### <a name="add-url"></a>Ajouter une URL

#### <a name="options"></a>Options

| Option Short| Option longue| Description | Exemple |
|-------|------|-------------|---------|
| -p|--updateProject | Projet sur lequel effectuer l’opération. |Ajouter une URL dotnet openapi *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -o|--fichier de sortie | Où placer la copie locale du fichier OpenAPI. |dotnet openapi ajouter une URL `https://contoso.com/openapi.json` *--output-file MyClient. JSON* |
| -c|--Code-Generator| Générateur de code à appliquer à la référence. Les options sont `NSwagCSharp` et `NSwagTypeScript`. |dotnet openapi ajouter un fichier .\OpenApi.json--Code-Generator
| -H|--help|Afficher les informations d’aide|Ajouter une URL dotnet openapi--Help|

#### <a name="arguments"></a>Arguments

|  Argument  | Description | Exemple |
|-------------|-------------|---------|
| URL source | Source à partir de laquelle créer une référence. Doit être une URL. |`https://contoso.com/openapi.json` d’ajout d’URL dotnet openapi |

## <a name="remove"></a>Supprimer

Supprime la référence OpenAPI correspondant au nom de fichier donné du fichier *. csproj* . Lorsque la référence OpenAPI est supprimée, les clients ne sont pas générés. Les fichiers local *. JSON* et *. YAML* sont supprimés.

### <a name="options"></a>Options

| Option Short| Option longue| Description| Exemple |
|-------|------|------------|---------|
| -p|--updateProject | Projet sur lequel effectuer l’opération. |dotnet openapi Remove *--updateProject .\Ref.csproj* .\OpenAPI.JSON |
| -H|--help|Afficher les informations d’aide|openapi de la suppression dotnet--Help|

### <a name="arguments"></a>Arguments

|  Argument  | Description| Exemple |
| ------------|------------|---------|
| fichier source | Source à laquelle supprimer la référence. |dotnet openapi supprimer *.\OpenAPI.JSON* |

## <a name="refresh"></a>Actualiser

Actualise la version locale d’un fichier qui a été téléchargé à l’aide du contenu le plus récent à partir de l’URL de téléchargement.

### <a name="options"></a>Options

| Option Short| Option longue| Description | Exemple |
|-------|------|-------------|---------|
| -p|--updateProject | Projet sur lequel effectuer l’opération. | actualisation de dotnet openapi *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -H|--help|Afficher les informations d’aide|actualisation de dotnet openapi--aide|

### <a name="arguments"></a>Arguments

|  Argument  | Description | Exemple |
| ------------|-------------|---------|
| URL source | URL à partir de laquelle actualiser la référence. | `https://contoso.com/openapi.json` d’actualisation dotnet openapi |
