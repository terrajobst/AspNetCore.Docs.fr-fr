---
uid: visual-studio/overview/2013/using-browser-link
title: À l’aide du lien du navigateur dans Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822193"
---
<a name="using-browser-link-in-visual-studio-2013"></a>À l’aide du lien du navigateur dans Visual Studio 2013
====================
par [Mike Wasson](https://github.com/MikeWasson)

Lien du navigateur est une nouvelle fonctionnalité dans Visual Studio 2013 qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs web. Vous pouvez utiliser le lien du navigateur pour actualiser votre application web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests dans plusieurs navigateurs.

- [Actualisation du navigateur](#browser-refresh)
- [Affichage du tableau de bord de lien de navigateur](#dashboard)
- [Activation du lien de navigateur pour les fichiers HTML statiques](#static-html)
- [La désactivation de lien du navigateur](#disabling)
- [Comment cela fonctionne-t-il ?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Actualisation du navigateur

Avec l’actualisation du navigateur, vous pouvez actualiser plusieurs navigateurs qui sont connectés à Visual Studio via le lien du navigateur.

Pour utiliser l’actualisation du navigateur, d’abord créer une application ASP.NET, à l’aide d’un des modèles de projet. Déboguer l’application en appuyant sur F5 ou en cliquant sur l’icône de flèche dans la barre d’outils :

![](using-browser-link/_static/image1.png)

Vous pouvez également utiliser la liste déroulante pour sélectionner un navigateur spécifique pour le débogage.

![](using-browser-link/_static/image2.png)

Pour déboguer avec plusieurs navigateurs, sélectionnez **naviguer avec**. Dans le **naviguer avec** boîte de dialogue, maintenez enfoncée la touche CTRL enfoncée pour sélectionner plusieurs navigateurs. Cliquez sur **Parcourir** pour déboguer avec les navigateurs sélectionnés. Lien du navigateur fonctionne également si vous lancez un navigateur en dehors de Visual Studio et accédez à l’URL de l’application.

![](using-browser-link/_static/image3.png)

Les contrôles de lien du navigateur se trouvent dans la liste déroulante avec l’icône de flèche circulaire. L’icône de flèche est la **Actualiser** bouton.

![](using-browser-link/_static/image4.png)

Pour voir quels sont les navigateurs sont connectés, pointez la souris sur le **Actualiser** bouton pendant le débogage. Les navigateurs connectés sont affichés dans une fenêtre d’info-bulle.

![](using-browser-link/_static/image5.png)

Pour actualiser les navigateurs connectés, cliquez sur le **Actualiser** bouton ou appuyez sur CTRL + ALT + ENTRÉE. Par exemple, la capture d’écran suivante montre un projet ASP.NET, que j’ai créé à l’aide du modèle de projet MVC 5. Vous pouvez voir l’application en cours d’exécution dans les deux navigateurs en haut. En bas, le projet est ouvert dans Visual Studio.

![](using-browser-link/_static/image6.png)

Dans Visual Studio, j’ai modifié le &lt;h1&gt; titre pour la page d’accueil :

![](using-browser-link/_static/image7.png)

Lorsque j’ai cliqué sur le **Actualiser** bouton, la modification est apparue dans les deux fenêtres du navigateur :

![](using-browser-link/_static/image8.png)

**Notes**

- Pour activer le lien du navigateur, définissez `debug=true` dans le [ &lt;compilation&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) élément dans le fichier Web.config du projet.
- L’application doit être en cours d’exécution sur l’hôte local.
- L’application doit cibler .NET 4.0 ou version ultérieure.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Affichage du tableau de bord de lien de navigateur

Le tableau de bord lien du navigateur affiche des informations sur les connexions de lien du navigateur. Pour afficher le tableau de bord, sélectionnez le menu déroulant de lien du navigateur (la petite flèche à côté du **Actualiser** bouton). Puis cliquez sur **tableau de bord de lien de navigateur**.

![](using-browser-link/_static/image9.png)

Le tableau de bord répertorie les navigateurs connectés et l’URL à laquelle chaque navigateur a navigué.

![](using-browser-link/_static/image10.png)

Le **conditions préalables** section montre toutes les étapes nécessaires pour activer le lien du navigateur pour ce projet. Par exemple, la capture d’écran suivante montre un projet où « debug » a la valeur false dans le fichier Web.config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Activation du lien de navigateur pour les fichiers HTML statiques

Pour activer le lien du navigateur pour les fichiers HTML statiques, ajoutez le code suivant à votre fichier Web.config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Pour des raisons de performances, supprimez ce paramètre lorsque vous publiez votre projet.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>La désactivation de lien du navigateur

Lien du navigateur est activé par défaut. Il existe plusieurs façons de le désactiver :

- Dans le menu déroulant du lien du navigateur, décochez la case **activer le lien du navigateur**. 

    ![](using-browser-link/_static/image12.png)
- Dans le fichier Web.config, ajoutez une clé nommée « vs : EnableBrowserLink » avec la valeur « false » dans la section appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Dans le fichier Web.config, définissez le débogage sur false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Comment cela fonctionne-t-il ?

Lien du navigateur utilise [SignalR](../../../signalr/index.md) pour créer un canal de communication entre Visual Studio et le navigateur. Quand le lien du navigateur est activé, Visual Studio fait office de serveur SignalR auquel plusieurs clients (navigateurs) peuvent se connecter.  Lien du navigateur enregistre également un module HTTP avec ASP.NET. Ce module injecte spéciale &lt;script&gt; références dans chaque demande de page à partir du serveur. Vous pouvez voir les références de script en sélectionnant « Afficher la source » dans le navigateur.

![](using-browser-link/_static/image13.png)

Vos fichiers sources ne sont pas modifiées. Le module HTTP injecte dynamiquement les références de script.

Le code côté navigateur étant toutes les fonctions JavaScript, il fonctionne sur tous les navigateurs qui [SignalR prend en charge](../../../signalr/overview/getting-started/supported-platforms.md), sans nécessiter de n’importe quel plug-in de navigateur.
