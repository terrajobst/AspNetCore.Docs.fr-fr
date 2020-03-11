---
title: Héberger et déployer des ASP.NET Core Blazor
author: guardrex
description: Découvrez comment héberger et déployer des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 238e7fc8f8d64c7847dc8847fb66e22442a3c8e0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667152"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>Héberger et déployer ASP.NET Core Blazor

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a>Publier l’application

Les applications sont publiées pour le déploiement dans la configuration Release.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Sélectionnez **Build** > **Publier {APPLICATION}** dans la barre de navigation.
1. Sélectionnez l’onglet *Cible de publication*. Pour publier localement, sélectionnez **Dossier**.
1. Acceptez l’emplacement par défaut dans le champ **Choisir un dossier** ou spécifiez un autre emplacement. Cliquez sur le bouton **Publier**.

# <a name="net-core-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Utilisez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) pour publier l’application avec une configuration Release :

```dotnetcli
dotnet publish -c Release
```

---

La publication de l’application déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [crée](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement. Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.

Une application Blazor webassembly est publiée dans le dossier */bin/Release/{Target Framework}/Publish/{assembly/dist* . Une application Blazor Server est publiée dans le dossier */bin/Release/{Target Framework}/Publish* .

Les ressources du dossier sont déployées sur le serveur web. Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.

## <a name="app-base-path"></a>Chemin de base de l’application

Le *chemin d’accès de base* de l’application est le chemin de l’URL racine de l’application. Tenez compte des ASP.NET Core application et Blazor sous-application suivantes :

* L’application ASP.NET Core est nommée `MyApp`:
  * L’application réside physiquement dans *d:/MyApp*.
  * Les demandes sont reçues au `https://www.contoso.com/{MYAPP RESOURCE}`.
* Une Blazor application nommée `CoolApp` est une sous-application de `MyApp`:
  * La sous-application réside physiquement dans *d:/MyApp/coolapp*.
  * Les demandes sont reçues au `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.

Si vous ne spécifiez pas de configuration supplémentaire pour `CoolApp`, la sous-application dans ce scénario n’a aucune connaissance de son emplacement sur le serveur. Par exemple, l’application ne peut pas construire des URL relatives correctes pour ses ressources sans savoir qu’elle réside au chemin d’URL relatif `/CoolApp/`.

Pour fournir la configuration du chemin d’accès de base de l’application Blazor de `https://www.contoso.com/CoolApp/`, l’attribut `href` de la balise `<base>` est défini sur le chemin d’accès racine relatif dans le fichier *pages/_Host. cshtml* (Blazor Server) ou le fichier *wwwroot/index.html* (Blazor webassembly) :

```html
<base href="/CoolApp/">
```

Blazor les applications serveur définissent également le chemin d’accès de base côté serveur en appelant <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> dans le pipeline de requête de l’application de `Startup.Configure`:

```csharp
app.UsePathBase("/CoolApp");
```

En fournissant le chemin d’URL relatif, un composant qui ne se trouve pas dans le répertoire racine peut construire des URL relatives au chemin d’accès racine de l’application. Les composants à différents niveaux de la structure de répertoires peuvent créer des liens vers d’autres ressources à des emplacements de l’application. Le chemin d’accès de base de l’application est également utilisé pour intercepter les liens hypertexte sélectionnés où la `href` cible du lien se trouve dans l’espace URI du chemin d’accès de base de l’application. Le routeur Blazor gère la navigation interne.

Dans de nombreux scénarios d’hébergement, le chemin d’URL relatif à l’application est la racine de l’application. Dans ce cas, le chemin d’accès de base de l’URL relative de l’application est une barre oblique (`<base href="/" />`), qui est la configuration par défaut pour une application Blazor. Dans d’autres scénarios d’hébergement, tels que les pages GitHub et les sous-applications IIS, le chemin d’accès de base de l’application doit être défini sur le chemin d’URL relatif du serveur de l’application.

Pour définir le chemin d’accès de base de l’application, mettez à jour la balise `<base>` dans les éléments `<head>` balise du fichier *pages/_Host. cshtml* (serveurBlazor) ou du fichier *wwwroot/index.html* (Blazor webassembly). Définissez la valeur de l’attribut `href` sur `/{RELATIVE URL PATH}/` (la barre oblique de fin est requise), où `{RELATIVE URL PATH}` est le chemin d’URL relatif complet de l’application.

Pour une application Blazor webassembly avec un chemin d’URL relatif non racine (par exemple, `<base href="/CoolApp/">`), l’application ne parvient pas à trouver ses ressources lorsqu’elle est *exécutée localement*. Pour surmonter ce problème pendant le développement local et les tests, vous pouvez fournir un argument de *base de chemin* qui correspond à la valeur `href` de la balise `<base>` au moment de l’exécution. N’incluez pas de barre oblique finale. Pour passer l’argument de base Path lors de l’exécution locale de l’application, exécutez la commande `dotnet run` à partir du répertoire de l’application avec l’option `--pathbase` :

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

Pour une application Blazor webassembly avec un chemin d’URL relatif de `/CoolApp/` (`<base href="/CoolApp/">`), la commande est la suivante :

```dotnetcli
dotnet run --pathbase=/CoolApp
```

L’application Blazor webassembly répond localement à `http://localhost:port/CoolApp`.

## <a name="deployment"></a>Déploiement

Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
