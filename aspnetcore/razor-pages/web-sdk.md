---
title: Kit de développement logiciel (SDK) Web ASP.NET Core
author: Rick-Anderson
description: Vue d’ensemble de Microsoft. NET. Sdk. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869764"
---
# <a name="aspnet-core-web-sdk"></a>Kit de développement logiciel (SDK) Web ASP.NET Core

### <a name="overview"></a>Vue d'ensemble de

`Microsoft.NET.Sdk.Web` est un [Kit de développement logiciel (SDK) de projet MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) pour la génération d’applications ASP.net core. Il est possible de créer une application ASP.NET Core sans ce kit de développement logiciel (SDK), mais le kit de développement logiciel (SDK) Web est le suivant :

* Adapté à la fourniture d’une expérience de première classe.
* Cible recommandée pour la plupart des utilisateurs.

Utilisez le kit de développement logiciel (SDK) Web dans un projet :

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

Fonctionnalités activées à l’aide du kit de développement logiciel (SDK) Web :

* Les projets ciblant .NET Core 3,0 ou une version ultérieure référencent implicitement :

  * [Framework partagé ASP.net Core](xref:fundamentals/metapackage-app).
  * [Analyseurs](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) conçus pour générer des applications ASP.net core.
* Le kit de développement logiciel (SDK) Web importe les cibles MSBuild qui permettent l’utilisation de profils de publication et la publication à l’aide de WebDeploy.

### <a name="properties"></a>Propriétés

| Les | Description |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | Désactive la référence implicite au `Microsoft.AspNetCore.App` Framework partagé. |
| `DisableImplicitAspNetCoreAnalyzers` | Désactive la référence implicite aux analyseurs de ASP.NET Core. |
| `DisableImplicitComponentsAnalyzers` | Désactive la référence implicite aux analyseurs de composants Razor lors de la création d’applications Blazor (serveur). |
