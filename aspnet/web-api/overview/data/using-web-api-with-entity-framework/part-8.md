---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Afficher les détails de l’élément | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 268c44f842cc2beb32a0a3e4c74b83b7ca9fd787
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375177"
---
<a name="display-item-details"></a>Affichage des détails d’élément
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez ajouter la possibilité d’afficher les détails de chaque ouvrage. Dans app.js, ajoutez le code suivant au modèle de vue :

[!code-javascript[Main](part-8/samples/sample1.js)]

Dans Views/Home/Index.cshtml, ajoutez un élément de liaison de données pour le lien Détails :

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Cette opération lie au Gestionnaire de clic le &lt;un&gt; élément à la `getBookDetail` fonction sur le modèle de vue.

Dans le même fichier, remplacez la majoration suivante :

[!code-html[Main](part-8/samples/sample3.html)]

par le code :

[!code-html[Main](part-8/samples/sample4.html)]

Ce balisage crée une table qui est lié aux données aux propriétés de la `detail` observable dans le modèle de vue.

Le «&lt;!--ko--&gt; &quot; syntaxe vous permet d’inclure une liaison de Knockout en dehors d’un élément DOM. Dans ce cas, le `if` liaison provoque cette section de balisage à afficher uniquement lorsque `details` n’est pas null.

[!code-html[Main](part-8/samples/sample5.html)]

Maintenant, si vous exécutez l’application et cliquez sur une de le &quot;détail&quot; liens, l’application affiche les détails du livre.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Précédent](part-7.md)
> [Suivant](part-9.md)
