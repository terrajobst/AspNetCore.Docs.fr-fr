---
title: ASP.NET Core de débogage Blazor webassembly
author: guardrex
description: Découvrez comment déboguer des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 5dbc900ab68682068a7f9e3ffdaabef89a0c7798
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306466"
---
# <a name="debug-aspnet-core-opno-locblazor-webassembly"></a>ASP.NET Core de débogage Blazor webassembly

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor les applications webassembly peuvent être déboguées à l’aide des outils de développement de navigateur dans les navigateurs basés sur le chrome (Edge/chrome).  Vous pouvez également déboguer votre application à l’aide de Visual Studio ou Visual Studio Code.

Les scénarios disponibles sont les suivants :

* Définir et supprimer des points d’arrêt.
* Exécutez l’application avec prise en charge du débogage dans Visual Studio et Visual Studio Code (prise en charge de<kbd>F5</kbd> ).
* Une étape (<kbd>F10</kbd>) à l’aide du code.
* Reprendre l’exécution du code avec <kbd>F8</kbd> dans un navigateur ou <kbd>F5</kbd> dans Visual Studio ou Visual Studio code.
* Dans l' *affichage des variables locales,* observez les valeurs des variables locales.
* Consultez la pile des appels, y compris les chaînes d’appel qui passent de JavaScript à .NET et de .NET à JavaScript.

Pour le moment, vous *ne pouvez pas*:

* Inspectez les tableaux.
* Pointez sur inspecter les membres.
* Déboguez l’étape dans ou hors du code managé.
* Bénéficiez d’une prise en charge complète pour l’inspection des types valeur.
* Arrêt sur les exceptions non gérées.
* Atteindre les points d’arrêt pendant le démarrage de l’application.
* Déboguez une application avec un service Worker.

Nous continuerons à améliorer l’expérience de débogage dans les versions à venir.

## <a name="prerequisites"></a>Conditions préalables requises

Le débogage requiert l’un des navigateurs suivants :

* Microsoft Edge (version 80 ou ultérieure)
* Google Chrome (version 70 ou ultérieure)

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a>Activer le débogage pour Visual Studio et Visual Studio Code

Le débogage est activé automatiquement pour les nouveaux projets créés à l’aide du modèle de projet ASP.NET Core 3,2 Preview 3 ou version ultérieure Blazor webassembly.

Pour activer le débogage pour une application Blazor webassembly existante, mettez à jour le fichier *launchSettings. JSON* dans le projet de démarrage pour inclure la propriété `inspectUri` suivante dans chaque profil de lancement :

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

Une fois mis à jour, le fichier *launchSettings. JSON* doit ressembler à l’exemple suivant :

[!code-json[](debug/launchSettings.json?highlight=14,22)]

Propriété `inspectUri` :

* Permet à l’IDE de détecter que l’application est une application Blazor webassembly.
* Indique à l’infrastructure de débogage de script de se connecter au navigateur via le proxy de débogage de Blazor.

## <a name="visual-studio"></a>Visual Studio

Pour déboguer une application Blazor webassembly dans Visual Studio :

1. Vérifiez que vous avez [installé la dernière version d’évaluation de Visual Studio 2019 16,6](https://visualstudio.com/preview) (Preview 2 ou version ultérieure).
1. Créez une nouvelle ASP.NET Core hébergée Blazor application webassembly.
1. Appuyez sur <kbd>F5</kbd> pour exécuter l’application dans le débogueur.
1. Définissez un point d’arrêt dans *Counter. Razor* dans la méthode `IncrementCount`.
1. Accédez à l’onglet **compteur** et sélectionnez le bouton pour atteindre le point d’arrêt :

   ![Compteur de débogage](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. Examinez la valeur du champ `currentCount` dans la fenêtre variables locales :

   ![Afficher les variables locales](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. Appuyez sur <kbd>F5</kbd> pour poursuivre l’exécution.

Lors du débogage de votre application Blazor webassembly, vous pouvez également déboguer votre code serveur :

1. Définissez un point d’arrêt dans la page *fetchData. Razor* dans `OnInitializedAsync`.
1. Définissez un point d’arrêt dans la `WeatherForecastController` dans la méthode d’action `Get`.
1. Accédez à l’onglet **extraire les données** pour atteindre le premier point d’arrêt dans le composant `FetchData` juste avant qu’il envoie une requête HTTP au serveur :

   ![Déboguer extraire des données](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. Appuyez sur <kbd>F5</kbd> pour poursuivre l’exécution, puis appuyez sur le point d’arrêt sur le serveur dans la `WeatherForecastController`:

   ![Serveur de débogage](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. Appuyez de nouveau sur <kbd>F5</kbd> pour permettre à l’exécution de se poursuivre et consultez le tableau prévisions météo rendu.

## <a name="visual-studio-code"></a>Visual Studio Code

Pour déboguer une application Blazor webassembly dans Visual Studio Code :
 
1. Installez l' [ C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) et l’extension de [débogueur JavaScript (nocturne)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) avec `debug.javascript.usePreview` défini sur `true`.

   ![Extensions](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![Débogueur JS preview](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. Ouvrez une application Blazor webassembly existante avec le débogage activé.

   * Si vous recevez la notification suivante indiquant qu’une configuration supplémentaire est requise pour activer le débogage, vérifiez que les extensions appropriées sont installées et que le débogage de l’aperçu JavaScript est activé, puis rechargez la fenêtre :

     ![Installation supplémentaire obligatoire](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * Une notification propose d’ajouter les ressources requises à l’application pour la génération et le débogage. Sélectionnez **Oui**:

     ![Ajouter les ressources requises](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. Le démarrage de l’application dans le débogueur est un processus en deux étapes :

   1 \. **Tout d’abord**, démarrez l’application à l’aide de la configuration de lancement de **.net Core (Blazor Standalone)** .

   2 \. **Une fois l’application démarrée**, démarrez le navigateur à l’aide de l' **assembly de débogage .net Core Blazor assembly dans** la configuration de lancement chrome (nécessite chrome). Pour utiliser Edge au lieu de chrome, modifiez la `type` de la configuration Launch dans *. vscode/Launch. JSON* de `pwa-chrome` à `pwa-edge`.

1. Définissez un point d’arrêt dans la méthode `IncrementCount` du composant `Counter`, puis sélectionnez le bouton pour atteindre le point d’arrêt :

   ![Déboguer le compteur dans VS Code](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a>Déboguer dans le navigateur

1. Exécutez une version Debug de l’application dans l’environnement de développement.

1. Appuyez sur <kbd>maj</kbd>+<kbd>ALT</kbd>+<kbd>D</kbd>.

1. Le navigateur doit être exécuté avec le débogage distant activé. Si le débogage distant est désactivé, une page d’erreur **Impossible de trouver un onglet de navigateur pouvant être débogué** est générée. La page d’erreur contient des instructions pour l’exécution du navigateur avec le port de débogage ouvert afin que le Blazor le proxy de débogage puisse se connecter à l’application. *Fermez toutes les instances de navigateur* et redémarrez le navigateur comme indiqué.

Une fois que le navigateur est en cours d’exécution avec le débogage distant activé, le raccourci clavier de débogage ouvre un nouvel onglet du débogueur. Après un moment, l’onglet **sources** affiche une liste des assemblys .net dans l’application. Développez chaque assembly et recherchez les fichiers sources *. cs*/ *. Razor* disponibles pour le débogage. Définissez des points d’arrêt, revenez à l’onglet de l’application, et les points d’arrêt sont atteints lorsque le code s’exécute. Une fois le point d’arrêt atteint, une seule étape (<kbd>F10</kbd>) passe par l’exécution du code ou de la reprise (<kbd>F8</kbd>).

Blazor fournit un proxy de débogage qui implémente le [protocole chrome devtools](https://chromedevtools.github.io/devtools-protocol/) et qui augmente le protocole avec. Informations spécifiques à .net. Quand l’utilisateur clique sur le raccourci de débogage, Blazor pointe le chrome DevTools au niveau du proxy. Le proxy se connecte à la fenêtre du navigateur que vous cherchez à déboguer (par conséquent, il est nécessaire d’activer le débogage distant).

## <a name="browser-source-maps"></a>Mappages des sources du navigateur

Les mappages de source de navigateur permettent au navigateur de mapper les fichiers compilés à leurs fichiers sources d’origine et sont couramment utilisés pour le débogage côté client. Toutefois, Blazor n’est pas C# actuellement mappé directement à JavaScript/WASM. Au lieu de cela, Blazor effectue une interprétation IL dans le navigateur, de sorte que les mappages de source ne sont pas pertinents.

## <a name="troubleshoot"></a>Dépanner

Si vous rencontrez des erreurs, le Conseil suivant peut vous aider :

Dans l’onglet **débogueur** , ouvrez les outils de développement de votre navigateur. Dans la console, exécutez `localStorage.clear()` pour supprimer les points d’arrêt.
