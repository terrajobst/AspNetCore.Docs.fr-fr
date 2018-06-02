---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Ajouter un nouvel élément à la base de données | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="add-a-new-item-to-the-database"></a>Ajouter un nouvel élément à la base de données
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez ajouter la possibilité aux utilisateurs de créer un nouveau livre. Dans app.js, ajoutez le code suivant pour le modèle d’affichage :

[!code-javascript[Main](part-9/samples/sample1.js)]

Dans Index.cshtml, remplacez le balisage suivant :

[!code-html[Main](part-9/samples/sample2.html)]

Par :

[!code-html[Main](part-9/samples/sample3.html)]

Cette balise crée un formulaire pour l’envoi de l’auteur d’un nouveau. Les valeurs de la liste déroulante auteur sont liés aux données à le `authors` observable dans le modèle d’affichage. Pour les autres entrées de formulaire, les valeurs sont liés aux données à le `newBook` propriété du modèle de la vue.

Le Gestionnaire d’envoi du formulaire est lié à la `addBook` (fonction) :

[!code-html[Main](part-9/samples/sample4.html)]

Le `addBook` fonction lit les valeurs actuelles des entrées de formulaire lié aux données pour créer un objet JSON. Puis il valide l’objet JSON à `/api/books`.

> [!div class="step-by-step"]
> [Précédent](part-8.md)
> [Suivant](part-10.md)
