---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validation avec une couche de Service (c#) | Microsoft Docs
author: StephenWalther
description: Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct. Dans ce didacticiel, Stephen Walther explique comment vous...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 69ff78949589017d12a791231e38b400b49f2917
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827448"
---
<a name="validating-with-a-service-layer-c"></a>Validation avec une couche de Service (c#)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct. Dans ce didacticiel, Stephen Walther explique comment vous pouvez conserver une sharp séparation des préoccupations en isolant votre couche de service à partir de votre couche de contrôleur.


L’objectif de ce didacticiel est de décrire une méthode pour la validation dans une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à déplacer votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct.

## <a name="separating-concerns"></a>Séparation des préoccupations

Lorsque vous générez une application ASP.NET MVC, vous ne devez pas placer votre logique de base de données à l’intérieur de vos actions de contrôleur. Mélange de votre logique de base de données et de contrôleur rend votre application plus difficile à gérer au fil du temps. Il est recommandé que vous placez toute votre logique de base de données dans une couche de référentiels distincts.

Par exemple, le Listing 1 contient un référentiel simple nommé le ProductRepository. Le référentiel de produit contient tout le code d’accès de données pour l’application. La liste inclut également l’interface IProductRepository qui implémente le référentiel de produit.

**Liste 1--Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Le contrôleur dans le Listing 2 utilise la couche de référentiels dans ses Index() et Create() actions. Notez que ce contrôleur ne contient pas de logique de base de données. Création d’une couche de référentiel vous permet de maintenir une séparation claire des préoccupations. Les contrôleurs sont chargés pour la logique de contrôle de flux application et le référentiel est responsable de la logique d’accès aux données.

**Listing 2 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Création d’une couche de Service

Par conséquent, logique de contrôle de flux application appartienne à un contrôleur et appartienne de logique d’accès aux données dans un référentiel. Dans ce cas, où placer votre logique de validation ? Une option consiste à placer votre logique de validation dans un *couche de service*.

Une couche de service est une couche supplémentaire dans une application ASP.NET MVC qui sert d’intermédiaire pour la communication entre un contrôleur et une couche de référentiels. La couche de service contient la logique métier. En particulier, il contient la logique de validation.

Par exemple, la couche de service de produit dans le Listing 3 a une méthode CreateProduct(). La méthode CreateProduct() appelle la méthode ValidateProduct() pour valider un nouveau produit avant de passer le produit dans le référentiel de produit.

**Liste 3 - Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Le contrôleur de produit a été mis à jour sur la liste de 4 à utiliser la couche de service au lieu de la couche de référentiels. La couche de contrôleur communique avec la couche de service. La couche de service communique avec la couche de référentiels. Chaque couche a une responsabilité distincte.

**Liste 4 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Notez que le service de produit est créé dans le constructeur de contrôleur du produit. Lorsque le service de produit est créé, le dictionnaire d’états de modèle est passé au service. Le service de produit utilise l’état du modèle pour transmettre des messages d’erreur de validation sur le contrôleur.

## <a name="decoupling-the-service-layer"></a>Découplage de la couche de Service

Nous avons a échoué isoler le contrôleur et les couches de service dans un aspect. Le contrôleur et les couches de service communiquent via le modèle d’état. En d’autres termes, la couche de service a une dépendance sur une fonctionnalité particulière de l’infrastructure ASP.NET MVC.

Nous souhaitons isoler la couche de service à partir de notre couche contrôleur autant que possible. En théorie, nous devrions être en mesure d’utiliser la couche de service avec n’importe quel type d’application et non seulement une application ASP.NET MVC. Par exemple, à l’avenir, nous pourrions créer un WPF frontal pour notre application. Nous devons trouver un moyen de supprimer la dépendance sur ASP.NET MVC état du modèle à partir de la couche de notre service.

Dans la liste 5, la couche de service a été mise à jour afin qu’il n’utilise plus l’état du modèle. Au lieu de cela, il utilise n’importe quelle classe qui implémente l’interface IValidationDictionary.

**Liste 5 - Models\ProductService.cs (découplé)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

L’interface IValidationDictionary est définie dans la liste 6. Cette interface simple possède une méthode unique et une seule propriété.

**Liste 6 - Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

La classe dans la liste 7, nommé la classe ModelStateWrapper, implémente l’interface IValidationDictionary. Vous pouvez instancier la classe ModelStateWrapper en passant un dictionnaire d’états de modèle au constructeur.

**Liste 7 - Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Enfin, le contrôleur mis à jour dans la liste 8 utilise le ModelStateWrapper lors de la création de la couche de service dans son constructeur.

**Liste 8 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

À l’aide de la IValidationDictionary interface et la classe ModelStateWrapper nous permet d’isoler totalement notre couche de service à partir de la couche de notre contrôleur. La couche de service n’est plus dépendante de l’état du modèle. Vous pouvez passer n’importe quelle classe qui implémente l’interface IValidationDictionary à la couche de service. Par exemple, une application WPF peut implémenter l’interface IValidationDictionary avec une classe de collection simple.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a été pour présenter une approche de la validation dans une application ASP.NET MVC. Dans ce didacticiel, vous avez appris à déplacer toute votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct. Vous avez également appris comment isoler votre couche de service à partir de votre couche de contrôleur en créant une classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Précédent](validating-with-the-idataerrorinfo-interface-cs.md)
> [Suivant](validation-with-the-data-annotation-validators-cs.md)
