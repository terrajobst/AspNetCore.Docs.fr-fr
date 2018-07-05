---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Création d’une Action (c#) | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC. En savoir plus sur la configuration requise pour une méthode devant subir une action.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: d6f981201b93e3650b18f112dd9330f1470aa573
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372496"
---
<a name="creating-an-action-c"></a>Création d’une Action (c#)
====================
par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC. En savoir plus sur la configuration requise pour une méthode devant subir une action.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur. Vous en savoir plus sur la configuration requise d’une méthode d’action. Vous allez également apprendre à une méthode empêcher l’exposition en tant qu’action.

## <a name="adding-an-action-to-a-controller"></a>Ajout d’une Action à un contrôleur

Vous ajoutez une nouvelle action à un contrôleur d’en ajoutant une nouvelle méthode au contrôleur. Par exemple, le contrôleur dans le Listing 1 contient une action nommée Index() et une action nommée SayHello(). Les deux méthodes sont exposées en tant qu’actions.

**Liste 1 - Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Pour pouvoir être exposés à l’univers en tant qu’action, une méthode doit remplir certaines conditions :

- La méthode doit être publique.
- La méthode ne peut pas être une méthode statique.
- La méthode ne peut pas être une méthode d’extension.
- La méthode ne peut pas être un constructeur, un accesseur Get ou un accesseur Set.
- La méthode ne peut pas avoir de types génériques ouverts.
- La méthode n’est pas une méthode de la classe de base du contrôleur.
- La méthode ne peut pas contenir **ref** ou **out** paramètres.

Notez qu’il n’y a aucune restriction sur le type de retour d’une action de contrôleur. Une action de contrôleur peut retourner une chaîne, une valeur DateTime, une instance de la classe Random, ou void. L’infrastructure ASP.NET MVC convertit tout type de retour n’est pas un résultat d’action dans une chaîne et afficher la chaîne dans le navigateur.

Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée comme une action de contrôleur. Soyez prudent ici. Une action de contrôleur peut être appelée par toute personne connectée à Internet. Ne créez pas, par exemple, une action de contrôleur DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Empêche l’appelé une méthode publique

Si vous devez créer une méthode publique dans une classe de contrôleur et que vous ne souhaitez pas exposer la méthode comme une action de contrôleur puis vous pouvez empêcher la méthode invoquée à l’aide de l’attribut [NonAction]. Par exemple, le contrôleur dans le Listing 2 contient une méthode publique nommée CompanySecrets() est décorée avec l’attribut [NonAction].

**Listing 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Si vous tentez d’appeler l’action du contrôleur CompanySecrets() en tapant /Work/CompanySecrets dans la barre d’adresses de votre navigateur vous allez obtenir le message d’erreur dans la Figure 1.


[![Appel d’une méthode NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figure 01**: appel d’une méthode NonAction ([cliquez pour afficher l’image en taille réelle](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Précédent](creating-a-controller-cs.md)
> [Suivant](asp-net-mvc-routing-overview-vb.md)
