---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Effectuer une Validation Simple (c#) | Microsoft Docs
author: StephenWalther
description: Découvrez comment effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, Stephen Walther présente l’état du modèle et l’application d’assistance de validation HTML...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 14d7857c64268df3b998e05797f749f03509dd4b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830188"
---
<a name="performing-simple-validation-c"></a>Effectuer une Validation Simple (c#)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Découvrez comment effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, Stephen Walther présente vous à l’état du modèle et les programmes d’assistance HTML de validation.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez effectuer la validation au sein d’une application ASP.NET MVC. Par exemple, vous allez apprendre à empêcher un utilisateur d’envoyer un formulaire qui ne contient-elle pas une valeur pour un champ obligatoire. Vous allez apprendre à utiliser l’état du modèle et les programmes d’assistance HTML validation.

## <a name="understanding-model-state"></a>État de modèle de présentation

Vous utilisez - état du modèle, ou plus précisément, le dictionnaire d’états de modèle - pour représenter les erreurs de validation. Par exemple, l’action Create() dans le Listing 1 valide les propriétés d’une classe de produit avant d’ajouter la classe de produit à une base de données.


Je ne suis pas recommandant que vous ajoutez votre logique de validation ou de la base de données à un contrôleur. Un contrôleur doit contenir uniquement les logique liée au contrôle de flux d’application. Nous effectuons un raccourci pour simplifier les choses.


**Liste 1 - Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

Dans la liste 1, les propriétés Name, Description et UnitsInStock de la classe de produit sont validées. Si une de ces propriétés échoue un test de validation une erreur est ajoutée au dictionnaire d’états de modèle (représenté par la propriété ModelState de la classe de contrôleur).

S’il existe des erreurs dans l’état du modèle la propriété ModelState.IsValid retourne false. Dans ce cas, le formulaire HTML pour la création d’un nouveau produit s’affiche de nouveau. Sinon, s’il n’existe aucune erreur de validation, le nouveau produit est ajouté à la base de données.

## <a name="using-the-validation-helpers"></a>À l’aide des Helpers de Validation

L’infrastructure ASP.NET MVC inclut les deux programmes d’assistance de validation : l’application d’assistance Html.ValidationMessage() et du programme d’assistance Html.ValidationSummary(). Ces deux programmes d’assistance dans une vue vous permet d’afficher des messages d’erreur de validation.

Les programmes d’assistance Html.ValidationMessage() et Html.ValidationSummary() sont utilisés dans les vues Create et Edit qui sont générés automatiquement par la génération de modèles automatique ASP.NET MVC. Suivez ces étapes pour générer la vue de créer :

1. L’action Create() dans le contrôleur de produit de clic droit et sélectionnez l’option de menu **ajouter une vue** (voir Figure 1).
2. Dans le **ajouter une vue** boîte de dialogue, cochez la case intitulée **créer une vue fortement typée** (voir Figure 2).
3. À partir de la **afficher la classe de données** liste déroulante, sélectionnez la classe Product.
4. À partir de la **afficher le contenu** liste déroulante, sélectionnez Créer.
5. Cliquez sur le bouton **Ajouter**.


Assurez-vous que vous générez votre application avant d’ajouter une vue. Sinon, la liste des classes n’apparaître pas dans le **afficher la classe de données** liste déroulante.


[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**Figure 01**: ajout d’une vue ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image2.png))


[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**Figure 02**: création d’une vue fortement typée ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image4.png))


Après avoir effectué ces étapes, vous obtenez la vue de créer dans le Listing 2.

**Listing 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

Dans la liste 2, l’application d’assistance Html.ValidationSummary() est appelée immédiatement au-dessus du formulaire HTML. Ce programme d’assistance est utilisé pour afficher la liste des messages d’erreur de validation. Le programme d’assistance Html.ValidationSummary() restitue les erreurs dans une liste à puces.

Le programme d’assistance Html.ValidationMessage() est appelée en regard de chacun des champs de formulaire HTML. Ce programme d’assistance est utilisé pour afficher un message d’erreur juste à côté d’un champ de formulaire. Dans le cas de Listing 2, l’application d’assistance Html.ValidationMessage() affiche un astérisque lorsqu’il existe une erreur.

La page dans la Figure 3 illustre les messages d’erreur affichés par les programmes d’assistance de validation lorsque le formulaire est envoyé avec les champs manquants et des valeurs non valides.


[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**Figure 03**: Create The view, soumise avec des problèmes ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image6.png))


Notez que l’apparence du contenu HTML d’entrée de champs sont également modifiés quand il existe une erreur de validation. Les convertisseurs d’assistance de Html.TextBox() un *classe = « erreur de validation d’entrée »* attribut lorsqu’il existe une erreur de validation associé à la propriété restituée par l’application d’assistance Html.TextBox().

Il existe trois classes de feuille style en cascade utilisées pour contrôler l’apparence des erreurs de validation :

- entrée--erreur de validation est appliquée à la &lt;d’entrée&gt; balise rendue par Html.TextBox() helper.
- champ--erreur de validation est appliquée à la &lt;span&gt; balise rendue par l’application d’assistance Html.ValidationMessage().
- Résumé-erreurs de validation - appliquée à la &lt;ul&gt; balise rendue par l’application d’assistance Html.ValidationSumamry().

Vous pouvez modifier ces classes de feuille de style en cascade et par conséquent de modifier l’apparence des erreurs de validation, en modifiant le fichier Site.css situé dans le dossier de contenu.

> [!NOTE] 
> 
> La classe HtmlHelper inclut les propriétés statiques en lecture seule pour récupérer les noms de la validation connexes CSS classes. Ces propriétés statiques sont nommées ValidationInputCssClassName, ValidationFieldCssClassName et ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding Validation et la Validation de Postbinding

Si vous envoyez le formulaire HTML pour la création d’un produit et que vous entrez une valeur non valide pour le champ price et aucune valeur pour le champ UnitsInStock, vous obtiendrez les messages de validation affichés dans la Figure 4. D'où proviennent ces messages d’erreur de validation ?


[![La boîte de dialogue Nouveau projet](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**Figure 04**: erreurs de Validation Prebinding ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-cs/_static/image8.png))


Il existe en fait deux types de messages d’erreur de validation - ceux générés avant que les champs de formulaire HTML sont liées à une classe et ceux générés une fois que les champs de formulaire sont liés à la classe. En d’autres termes, il existe prebinding des erreurs de validation et postbinding des erreurs de validation.

L’action Create() exposée par le contrôleur de produit dans le Listing 1 accepte une instance de la classe de produit. La signature de la méthode Create ressemble à ceci :

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

Les valeurs des champs de formulaire HTML dans le formulaire de création sont liés à la classe productToCreate par ce que l'on appelle un binder de modèle. Le binder de modèle par défaut ajoute automatiquement un message d’erreur à l’état du modèle lorsqu’il ne peut pas lier un champ de formulaire à une propriété de formulaire.

Le binder de modèle par défaut ne peut pas lier la chaîne « apple » à la propriété de prix de la classe de produit. Impossible d’assigner une chaîne à une propriété décimale. Par conséquent, le binder de modèle ajoute une erreur à l’état du modèle.

Le binder de modèle par défaut ne peut pas également affecter une valeur null à une propriété qui n’accepte pas les valeurs NULL. En particulier, le binder de modèle ne peut pas assigner une valeur null à la propriété UnitsInStock. Une fois encore, le binder de modèle renonce et ajoute un message d’erreur à l’état du modèle.

Si vous souhaitez personnaliser l’apparence de ces messages d’erreur de prebinding vous devez créer des chaînes de ressources pour ces messages.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a été pour décrire les mécanismes de base de validation dans l’infrastructure ASP.NET MVC. Vous avez appris comment utiliser l’état du modèle et les programmes d’assistance HTML validation. Nous avons abordé également la distinction entre prebinding et postbinding de validation. Dans d’autres didacticiels, nous allons aborder les différentes stratégies pour déplacer votre code de validation en dehors de vos contrôleurs et dans vos classes de modèle.

> [!div class="step-by-step"]
> [Précédent](displaying-a-table-of-database-data-cs.md)
> [Suivant](validating-with-the-idataerrorinfo-interface-cs.md)
