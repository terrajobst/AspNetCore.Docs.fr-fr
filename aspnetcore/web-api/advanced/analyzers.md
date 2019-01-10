---
title: Utiliser les analyseurs d’API web
author: pranavkm
description: Découvrez plus d’informations sur les analyseurs d’API web dans Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 7558552586d3056c43d8bfd9ef74cbcb3396726f
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425092"
---
# <a name="use-web-api-analyzers"></a>Utiliser les analyseurs d’API web

ASP.NET Core 2.2 et les versions ultérieures introduisent le package NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) contenant des analyseurs pour les API web. Les analyseurs travaillent avec les contrôleurs annotés avec <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, tout en s’appuyant sur les [conventions des API](xref:web-api/advanced/conventions).

## <a name="package-installation"></a>Installation de package

Vous pouvez ajouter `Microsoft.AspNetCore.Mvc.Api.Analyzers` en adoptant une des approches suivantes :

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de la fenêtre **Console du Gestionnaire de package** :
  * Accédez à **Affichage** > **Autres fenêtres** > **Console du Gestionnaire de package**.
  * Accédez au répertoire où se trouve le fichier *ApiConventions.csproj*.
  * Exécutez la commande suivante :

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* À partir de la boîte de dialogue **Gérer les packages NuGet** :
  * Cliquez avec le bouton droit sur le projet dans **Explorateur de solutions** > **Gérer les packages NuGet**.
  * Définissez **Source du package** sur « nuget.org ».
  * Entrez « Microsoft.AspNetCore.Mvc.Api.Analyzers » dans la zone de recherche.
  * Sélectionnez le package « Microsoft.AspNetCore.Mvc.Api.Analyzers » sous l’onglet **Parcourir**, puis cliquez sur **Installer**.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages...**.
* Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.
* Entrez « Microsoft.AspNetCore.Mvc.Api.Analyzers » dans la zone de recherche.
* Sélectionnez le package « Microsoft.AspNetCore.Mvc.Api.Analyzers » dans le volet de résultats, puis cliquez sur **Ajouter un package**.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Exécutez la commande suivante à partir du **Terminal intégré** :

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Exécutez la commande suivante :

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a>Analyseurs pour les conventions d’API

Les documents sur les API ouvertes contiennent les codes d’état et les types de réponse qu’une action peut retourner. Dans ASP.NET Core MVC, des attributs comme <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> et <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> sont utilisés pour documenter une action. <xref:tutorials/web-api-help-pages-using-swagger> constitue une documentation plus détaillée de votre API.

Un des analyseurs du package inspecte les contrôleurs annotés avec <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> et identifie les actions qui ne documentent pas entièrement leurs réponses. Prenons l'exemple suivant :

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

L’action précédente documente le type de retour avec réussite HTTP 200, mais ne documente pas le code d’état d’échec HTTP 404. L’analyseur signale la documentation manquante pour le code d’état HTTP 404 sous la forme d’un avertissement. Une option pour résoudre le problème est fournie.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* [Annotation avec attribut ApiController](xref:web-api/index#annotation-with-apicontroller-attribute)
