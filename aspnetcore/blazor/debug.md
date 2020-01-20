---
title: ASP.NET Core de débogage Blazor
author: guardrex
description: Découvrez comment déboguer des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159987"
---
# <a name="debug-aspnet-core-opno-locblazor"></a>ASP.NET Core de débogage Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Il existe une prise en charge *précoce* du débogage Blazor webassembly à l’aide des outils de développement de navigateur dans les navigateurs basés sur le chrome (chrome/Edge). Le travail est en cours pour :

* Activez complètement le débogage dans Visual Studio.
* Activez le débogage dans Visual Studio Code.

Les fonctionnalités du débogueur sont limitées. Les scénarios disponibles sont les suivants :

* Définir et supprimer des points d’arrêt.
* Une seule étape (`F10`) par le biais de l’exécution du code ou de la reprise (`F8`).
* Dans l' *affichage des variables locales,* observez les valeurs des variables locales de type `int`, `string`et `bool`.
* Consultez la pile des appels, y compris les chaînes d’appel qui passent de JavaScript à .NET et de .NET à JavaScript.

Vous *ne pouvez pas*:

* Observez les valeurs des variables locales qui ne sont pas un `int`, `string`ou `bool`.
* Observez les valeurs de tous les champs ou propriétés de classe.
* Pointez sur les variables pour afficher leurs valeurs.
* Évaluer les expressions dans la console.
* Effectuer un pas à pas détaillé dans les appels asynchrones.
* Effectuez la plupart des autres scénarios de débogage ordinaires.

Le développement d’autres scénarios de débogage est l’un des objectifs de l’équipe d’ingénierie.

## <a name="prerequisites"></a>Prerequisites

Le débogage requiert l’un des navigateurs suivants :

* Google Chrome (version 70 ou ultérieure)
* Version préliminaire de Microsoft Edge ([canal de développement Edge](https://www.microsoftedgeinsider.com))

## <a name="procedure"></a>Procédure

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!WARNING]
> La prise en charge du débogage dans Visual Studio est une étape précoce du développement. Le débogage **F5** n’est pas pris en charge actuellement.

1. Exécutez une application Blazor webassembly dans `Debug` configuration sans débogage (**Ctrl**+**F5** au lieu de **F5**).
1. Ouvrez les propriétés de débogage de l’application (dernière entrée dans le menu **Déboguer** ) et copiez l’URL de l' **application**http. Accédez à l’adresse HTTP (et non à l’adresse HTTPs) de l’application à l’aide d’un navigateur basé sur le chrome (Edge Beta ou chrome).
1. Placez le focus clavier sur l’application dans la fenêtre du navigateur, et non dans le panneau Outils de développement. Il est préférable de garder le panneau Outils de développement fermé pour cette procédure. Une fois le débogage démarré, vous pouvez rouvrir le panneau Outils de développement.
1. Sélectionnez le raccourci clavier spécifique à Blazorsuivant :

   * `Shift+Alt+D` sur Windows
   * `Shift+Cmd+D` sur macOS

   Si vous recevez l' **onglet impossible de trouver le navigateur pouvant être débogué**, consultez [activer le débogage distant](#enable-remote-debugging).
   
   Après l’activation du débogage distant :
   
   1 \. Une nouvelle fenêtre de navigateur s'ouvre. Fermez la fenêtre précédente.

   2 \. Placez le focus clavier sur l’application dans la fenêtre du navigateur.

   3 \. Sélectionnez le raccourci clavier spécifique au Blazordans la nouvelle fenêtre de navigateur : `Shift+Alt+D` sur Windows ou `Shift+Cmd+D` sur macOS.

   4 \. L’onglet **devtools** s’ouvre dans le navigateur. **Resélectionnez l’onglet de l’application dans la fenêtre du navigateur.**

   Pour attacher l’application à Visual Studio, consultez la section [attacher au processus dans Visual Studio](#attach-to-process-in-visual-studio) .

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

1. Exécutez une application Blazor webassembly dans la configuration de `Debug` en passant l’option `--configuration Debug` à la commande [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.
1. Accédez à l’application à l’URL HTTP indiquée dans la fenêtre de l’interpréteur de commandes.
1. Placez le focus clavier sur l’application, et non sur le panneau Outils de développement. Il est préférable de garder le panneau Outils de développement fermé pour cette procédure. Une fois le débogage démarré, vous pouvez rouvrir le panneau Outils de développement.
1. Sélectionnez le raccourci clavier spécifique à Blazorsuivant :

   * `Shift+Alt+D` sur Windows
   * `Shift+Cmd+D` sur macOS

   Si vous recevez l' **onglet impossible de trouver le navigateur pouvant être débogué**, consultez [activer le débogage distant](#enable-remote-debugging).
   
   Après l’activation du débogage distant :
   
   1 \. Une nouvelle fenêtre de navigateur s'ouvre. Fermez la fenêtre précédente.

   2 \. Placez le focus clavier sur l’application dans la fenêtre du navigateur, et non dans le panneau Outils de développement.

   3 \. Sélectionnez le raccourci clavier spécifique au Blazordans la nouvelle fenêtre de navigateur : `Shift+Alt+D` sur Windows ou `Shift+Cmd+D` sur macOS.

---

## <a name="enable-remote-debugging"></a>Activer le débogage à distance

Si le débogage distant est désactivé, une page d’erreur **Impossible de trouver un onglet de navigateur pouvant être débogué** est générée par chrome. La page d’erreur contient des instructions pour l’exécution de chrome avec le port de débogage ouvert, afin que le Blazor le proxy de débogage puissent se connecter à l’application. *Fermez toutes les instances chrome* et redémarrez Chrome comme indiqué.

## <a name="debug-the-app"></a>Déboguer l’application

Une fois que Chrome s’exécute avec le débogage distant activé, le raccourci clavier de débogage ouvre un nouvel onglet du débogueur. Après un moment, l’onglet **sources** affiche une liste des assemblys .net dans l’application. Développez chaque assembly et recherchez les fichiers sources *. cs*/ *. Razor* disponibles pour le débogage. Définissez des points d’arrêt, revenez à l’onglet de l’application, et les points d’arrêt sont atteints lorsque le code s’exécute. Après l’accès à un point d’arrêt, vous devez effectuer une seule étape (`F10`) par le biais de l’exécution du code ou de la reprise (`F8`) normalement.

Blazor fournit un proxy de débogage qui implémente le [protocole chrome devtools](https://chromedevtools.github.io/devtools-protocol/) et qui augmente le protocole avec. Informations spécifiques à .net. Quand l’utilisateur clique sur le raccourci de débogage, Blazor pointe le chrome DevTools au niveau du proxy. Le proxy se connecte à la fenêtre du navigateur que vous cherchez à déboguer (par conséquent, il est nécessaire d’activer le débogage distant).

## <a name="attach-to-process-in-visual-studio"></a>Attacher au processus dans Visual Studio

L’attachement au processus de l’application dans Visual Studio est un scénario de débogage *temporaire* pour Blazor webassembly pendant le débogage **F5** en cours de développement.

Pour attacher le processus de l’application en cours d’exécution à Visual Studio :

1. Dans Visual Studio, sélectionnez **Déboguer** > **attacher au processus**.
1. Pour le **type de connexion**, sélectionnez **chrome devtools protocole WebSocket (aucune authentification)** .
1. Pour la **cible de connexion**, collez l’adresse http (et non l’adresse https) de l’application.
1. Sélectionnez **Actualiser** pour actualiser les entrées sous **processus disponibles**.
1. Sélectionnez le processus du navigateur à déboguer, puis sélectionnez **attacher**.
1. Dans la boîte de dialogue **Sélectionner le type de code** , sélectionnez le type de code pour le navigateur spécifique auquel vous voulez attacher (Edge ou chrome), puis sélectionnez **OK**.

## <a name="browser-source-maps"></a>Mappages des sources du navigateur

Les mappages de source de navigateur permettent au navigateur de mapper les fichiers compilés à leurs fichiers sources d’origine et sont couramment utilisés pour le débogage côté client. Toutefois, Blazor n’est pas C# actuellement mappé directement à JavaScript/WASM. Au lieu de cela, Blazor effectue une interprétation IL dans le navigateur, de sorte que les mappages de source ne sont pas pertinents.

## <a name="troubleshoot"></a>Dépannage

Si vous rencontrez des erreurs, le Conseil suivant peut vous aider :

Dans l’onglet **débogueur** , ouvrez les outils de développement de votre navigateur. Dans la console, exécutez `localStorage.clear()` pour supprimer les points d’arrêt.
