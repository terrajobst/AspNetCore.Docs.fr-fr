---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Ajout de contrôles de Validation pour le contrôle DataList d’édition Interface (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir combien il est facile pour ajouter des contrôles de validation pour le contrôle de DataList EditItemTemplate afin de fournir une édition plus infaillible utilisateur type int....
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b06020128daa01c58b27639ff1db23febc0cba9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397588"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Ajout de contrôles de Validation à l’Interface de modification du contrôle DataList (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) ou [télécharger le PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> Dans ce didacticiel, nous allons voir combien il est facile pour ajouter des contrôles de validation pour le contrôle de DataList EditItemTemplate afin de fournir une interface utilisateur de modification plus infaillible.


## <a name="introduction"></a>Introduction

Dans le contrôle DataList modification didacticiels jusqu'à présent, les contrôles DataList modification des interfaces n’ont pas inclus toute validation d’entrée utilisateur proactive même si l’utilisateur non valide entrée comme un nom de produit manquante ou résultats prix négatif dans une exception. Dans le [didacticiel précédent](handling-bll-and-dal-level-exceptions-vb.md) nous avons examiné comment ajouter des exceptions de code pour le contrôle DataList s `UpdateCommand` Gestionnaire d’événements afin d’intercepter et afficher correctement les informations sur toutes les exceptions qui ont été générées. Dans l’idéal, toutefois, l’interface de modification inclut les contrôles de validation pour empêcher un utilisateur d’entrer ces données non valides en premier lieu.

Dans ce didacticiel, nous allons voir combien il est facile d’ajouter des contrôles de validation pour le contrôle DataList s `EditItemTemplate` afin de fournir une interface utilisateur de modification plus infaillible. Plus précisément, ce didacticiel prend l’exemple créé dans le didacticiel précédent et complète de l’interface de modification pour inclure la validation appropriée.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Étape 1 : Réplication de l’exemple à partir de[gestion des Exceptions de niveau BLL et DAL](handling-bll-and-dal-level-exceptions-vb.md)

Dans le [BLL - gestion et les Exceptions au niveau de la couche DAL](handling-bll-and-dal-level-exceptions-vb.md) didacticiel, nous avons créé une page qui liste les noms et les prix des produits dans un contrôle DataList deux colonnes, modifiable. Notre objectif pour ce didacticiel consiste à augmenter l’interface de modification s DataList pour inclure les contrôles de validation. En particulier, notre logique de validation sera :

- Exiger que le nom du produit s
- Vérifiez que la valeur entrée pour le prix est un format monétaire valide
- Vérifiez que la valeur entrée pour le prix est supérieur ou égal à zéro, depuis un négatif `UnitPrice` valeur n’est pas conforme

Avant que nous pouvons examiner l’exemple précédent pour inclure la validation en décuplant le volume, nous devons tout d’abord répliquer l’exemple à partir de la `ErrorHandling.aspx` page dans le `EditDeleteDataList` dossier à la page pour ce didacticiel, `UIValidation.aspx`. Pour cela nous devons recopiez les deux le `ErrorHandling.aspx` page balisage déclaratif s et son code source. Un premier temps copier le balisage déclaratif en effectuant les étapes suivantes :

1. Ouvrez le `ErrorHandling.aspx` page dans Visual Studio
2. Atteindre le balisage déclaratif de page s (cliquez sur le bouton de la Source en bas de la page)
3. Copiez le texte dans le `<asp:Content>` et `</asp:Content>` les balises (lignes 3 à 32), comme illustré dans la Figure 1.


[![Copiez le texte au sein de la &lt;asp : Content&gt; contrôle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figure 2**: copiez le texte au sein de la `<asp:Content>` contrôle ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Ouvrez le `UIValidation.aspx` page
2. Atteindre le balisage déclaratif s de page
3. Collez le texte dans le `<asp:Content>` contrôle.

Pour copier le code source, ouvrez le `ErrorHandling.aspx.vb` page puis copiez simplement le texte *dans* la `EditDeleteDataList_ErrorHandling` classe. Copiez les trois gestionnaires d’événements (`Products_EditCommand`, `Products_CancelCommand`, et `Products_UpdateCommand`) avec le `DisplayExceptionDetails` (méthode), mais ne **pas** copier la déclaration de classe ou `using` instructions. Coller le texte copié *dans* le `EditDeleteDataList_UIValidation` classe dans `UIValidation.aspx.vb`.

Après avoir déplacé le contenu et le code à partir de `ErrorHandling.aspx` à `UIValidation.aspx`, prenez un moment pour tester les pages dans un navigateur. Vous devez voir la même sortie et rencontrez les mêmes fonctionnalités dans chacune de ces deux pages (voir Figure 2).


[![La Page UIValidation.aspx reproduit la fonctionnalité dans ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figure 2**: le `UIValidation.aspx` Page reproduit la fonctionnalité dans `ErrorHandling.aspx` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Étape 2 : Ajout des contrôles Validation à EditItemTemplate s DataList

Lors de la construction des formulaires de saisie de données, il est important que les utilisateurs entrent tous les champs obligatoires et que leurs données d’entrée fournies sont autorisées, correctement mise en forme des valeurs. Pour garantir que les entrées de s d’un utilisateur sont valides, ASP.NET fournit cinq contrôles de validation intégrées qui sont conçus pour valider la valeur d’un contrôle Web d’entrée unique :

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantit qu’une valeur a été fournie.
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valide une valeur par rapport à une autre valeur de contrôle Web ou une valeur constante ou garantit que le format de valeur s est légal pour un type de données spécifié
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantit qu’une valeur est dans une plage de valeurs
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valide une valeur par rapport à un [expression régulière](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valide une valeur par rapport à une méthode personnalisée, définie par l’utilisateur

Pour plus d’informations sur ces cinq contrôles revenir à la [Ajout de contrôles de Validation pour l’édition et insertion des Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) didacticiel ou retirer le [section contrôles de Validation](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) de la [Didacticiels de démarrage rapide ASP.NET](https://quickstarts.asp.net).

Dans notre didacticiel, nous devez utiliser un contrôle RequiredFieldValidator pour vous assurer qu’une valeur pour le nom du produit a été fournie et un CompareValidator pour vous assurer que le prix entré a une valeur supérieure ou égale à 0 et qu’il est présenté dans un format monétaire valide.

> [!NOTE]
> Alors que ASP.NET 1.x avait ces mêmes contrôles de validation de cinq, ASP.NET 2.0 a ajouté un certain nombre d’améliorations, la principale deux en cours d’un script côté client prend en charge les navigateurs Microsoft Internet Explorer et la possibilité de contrôles de validation de partition sur une page dans groupes de validation. Pour plus d’informations sur les nouvelles fonctionnalités de contrôle de validation de 2.0, reportez-vous à [dissection les contrôles de Validation dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Permettent de commencer par ajouter les contrôles de validation nécessaire à la DataList s s `EditItemTemplate`. Cette tâche peut être effectuée via le concepteur en cliquant sur le lien Modifier les modèles à partir de la balise active de DataList s, ou via la syntaxe déclarative. Permettent d’étape s via le processus à l’aide de l’option Modifier les modèles à partir de la vue de conception. Après avoir choisi de modifier le contrôle DataList s `EditItemTemplate`, ajoutez un contrôle RequiredFieldValidator en le faisant glisser à partir de la boîte à outils dans l’interface de modification de modèle, en le plaçant après le `ProductName` zone de texte.


[![Ajoutez un contrôle RequiredFieldValidator à EditItemTemplate après la zone de texte ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figure 3**: ajouter un contrôle RequiredFieldValidator à la `EditItemTemplate After` le `ProductName` zone de texte ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Tous les contrôles de validation fonctionnent en validant l’entrée d’un contrôle Web ASP.NET unique. Par conséquent, nous devons indiquer que le contrôle RequiredFieldValidator que nous venons d’ajouter doit valider par rapport à la `ProductName` zone de texte ; cela en définissant le contrôle de validation s [ `ControlToValidate` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) à la `ID` de le contrôle Web approprié (`ProductName`, dans cette instance). Ensuite, définissez le [ `ErrorMessage` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) à, vous devez fournir le nom du produit s et le [ `Text` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) à \*. Le `Text` valeur de propriété, s’il est fourni, est le texte affiché par le contrôle de validation si la validation échoue. Le `ErrorMessage` valeur de propriété, qui est requis, est utilisé par le contrôle ValidationSummary ; si le `Text` valeur de propriété est omise, la `ErrorMessage` valeur de propriété est affichée par le contrôle de validation d’entrée non valide.

Après avoir défini ces trois propriétés de RequiredFieldValidator, votre écran doit ressembler à la Figure 4.


[![Définir le RequiredFieldValidator s ControlToValidate, message d’erreur et les propriétés de texte](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figure 4**: définir le s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, et `Text` propriétés ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


Avec le contrôle RequiredFieldValidator ajouté à la `EditItemTemplate`, que ne reste à ajouter la validation nécessaire pour le prix du produit s zone de texte. Dans la mesure où le `UnitPrice` est facultatif lorsque vous modifiez un enregistrement, nous n’avez pas besoin pour ajouter un contrôle RequiredFieldValidator. Nous avons, toutefois, doivent-ils ajouter un CompareValidator pour vous assurer que le `UnitPrice`, s’il est fourni, est correctement mis en forme comme une devise et est supérieur ou égal à 0.

Ajouter le contrôle CompareValidator dans le `EditItemTemplate` et définissez son `ControlToValidate` propriété `UnitPrice`, ses `ErrorMessage` propriété sur le prix doit être supérieur ou égal à zéro et ne peut pas inclure le symbole monétaire et son `Text` propriété \*. Pour indiquer que le `UnitPrice` valeur doit être supérieure ou égale à 0, définissez le s CompareValidator [ `Operator` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) à `GreaterThanEqual`, ses [ `ValueToCompare` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) à 0, et son [ `Type` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) à `Currency`.

Après avoir ajouté ces deux contrôles de validation, le contrôle DataList s `EditItemTemplate` syntaxe déclarative s doit ressembler à ce qui suit :


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Après avoir apporté ces modifications, ouvrez la page dans un navigateur. Si vous tentez d’omettre le nom ou entrez une valeur de prix non valide lors de la modification d’un produit, un astérisque apparaît en regard de la zone de texte. Comme le montre la Figure 5, une valeur de prix qui inclut le symbole monétaire tels que 19,95 $ est considéré comme non valide. Les opérations de mappage CompareValidator `Currency` `Type` permettant de séparateurs de chiffres (par exemple, des virgules ou des points, selon les paramètres de culture) et un signe de début plus ou moins, contrairement à *pas* autoriser un symbole monétaire. Ce comportement peut perplex utilisateurs comme l’interface de modification affiche actuellement le `UnitPrice` en utilisant le format de devise.


[![Un astérisque apparaît en regard des zones de texte avec une entrée non valide](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figure 5**: un astérisque apparaît à côté de zones de texte avec une entrée non valide ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


Tandis que le fonctionnement de la validation en tant que-est, l’utilisateur doit supprimer manuellement le symbole monétaire lorsque vous modifiez un enregistrement qui n’est pas acceptable. En outre, si il ne sont pas valides entrées dans la modification de l’interface ni la mise à jour, ni annuler les boutons, quand vous cliquez sur, appellera une publication (postback). Dans l’idéal, le bouton Annuler retournerait la DataList à son état préalable Édition, quel que soit la validité des entrées utilisateur s. En outre, nous devons nous assurer que les données de page s sont valides avant la mise à jour les informations de produit dans le contrôle DataList s `UpdateCommand` Gestionnaire d’événements, en tant que les logique côté client peut être contournée par les utilisateurs dont les navigateurs aucune n en charge JavaScript ou pour les contrôles de validation sa prise en charge est désactivée.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Supprimer le symbole monétaire à partir de la zone de texte EditItemTemplate s UnitPrice

Lorsque vous utilisez le s CompareValidator `Currency``Type`, l’entrée en cours de validation ne doit pas inclure tous les symboles monétaires. La présence de ces symboles entraîne CompareValidator marquer l’entrée comme étant non valide. Toutefois, notre interface de modification inclut actuellement un symbole de devise dans la `UnitPrice` zone de texte, ce qui signifie que l’utilisateur doit supprimer explicitement le symbole de devise avant d’enregistrer leurs modifications. Pour résoudre ce problème, nous avons trois options :

1. Configurer le `EditItemTemplate` afin que le `UnitPrice` valeur de la zone de texte n’est pas mis en forme comme une devise.
2. Autoriser l’utilisateur à entrer un symbole monétaire en supprimant les CompareValidator et en la remplaçant par une RegularExpressionValidator qui vérifie une valeur monétaire mise en forme correctement. Le défi ici est que l’expression régulière pour valider une valeur de devise n’est pas aussi simple que CompareValidator et nécessiterait une écriture de code si nous souhaitons intégrer des paramètres de culture.
3. Supprimer le contrôle de validation complètement et s’appuient sur la logique de validation personnalisée côté serveur dans le s GridView `RowUpdating` Gestionnaire d’événements.

Laissez s passer avec l’option 1 pour ce didacticiel. Actuellement le `UnitPrice` est mis en forme en tant que valeur monétaire en raison de l’expression de liaison de données pour la zone de texte dans le `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Modifier le `Eval` instruction `Eval("UnitPrice", "{0:n2}")`, qui met en forme le résultat sous la forme un nombre avec deux chiffres de précision. Cela est possible directement par le biais de la syntaxe déclarative ou en cliquant sur le lien Modifier les DataBindings à partir de la `UnitPrice` zone de texte dans le contrôle DataList s `EditItemTemplate`.

Avec cette modification, le prix de mise en forme dans l’interface de modification inclut des virgules comme séparateur de groupe et un point comme séparateur décimal, mais laisse de côté le symbole monétaire.

> [!NOTE]
> Lorsque vous supprimez le format monétaire à partir de l’interface modifiable, je trouve utile pour placer le symbole monétaire sous forme de texte en dehors de la zone de texte. Ceci sert un indicateur à l’utilisateur qu’ils n’avez pas besoin de fournir le symbole monétaire.


## <a name="fixing-the-cancel-button"></a>Corriger le bouton Annuler

Par défaut, les contrôles de validation Web émettent le code JavaScript pour effectuer la validation côté client. Un clic sur un bouton, LinkButton ou ImageButton, les contrôles de validation sur la page sont vérifiées sur la côté client avant que la publication (postback) se produit. S’il existe des données non valides, la publication (postback) est annulée. Pour certains boutons, cependant, la validité des données peut être immatérielle ; Dans ce cas, la publication annulée en raison de données non valides est d’avoir une plaie.

Le bouton Annuler est un exemple. Imaginez qu’un utilisateur entre des données non valides, tels que l’omission du nom de produit s, puis décide she n t souhaitez enregistrer le produit après tout et actionne le bouton Annuler. Actuellement, le bouton Annuler déclenche les contrôles de validation sur la page, qui signalent que le nom du produit est manquant et empêcher la publication (postback). Notre utilisateur doit taper du texte dans le `ProductName` zone de texte juste pour annuler le processus de modification.

Heureusement, le bouton, LinkButton et ImageButton ont un [ `CausesValidation` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) qui peut indiquer qu’ou non en cliquant sur le bouton doit lancer la logique de validation (valeur par défaut est `True`). Définir le bouton Annuler s `CausesValidation` propriété `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Garantir les entrées sont valides dans le Gestionnaire d’événements UpdateCommand

En raison du script côté client émis par les contrôles de validation, si un utilisateur entre une entrée non valide les contrôles de validation annuler toute publication initiée par un bouton, LinkButton, ou ImageButton contrôles dont la propriété `CausesValidation` propriétés sont `True` (le valeur par défaut). Toutefois, si un utilisateur visite avec un navigateur archaïque ou dont prise en charge de JavaScript a été désactivée, les vérifications de validation côté client n’exécute pas.

Tous les contrôles de validation ASP.NET Répétez leur logique de validation dès la publication (postback) et signaler la validité globale des entrées de page s via le [ `Page.IsValid` propriété](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Toutefois, le flux de page n’est pas interrompu ou arrêté dans une façon selon la valeur de `Page.IsValid`. En tant que développeurs, il est de notre responsabilité pour vous assurer que le `Page.IsValid` propriété a la valeur `True` avant de continuer avec le code qui suppose que valide les données d’entrée.

Si un utilisateur aurait désactivé JavaScript, visite notre page, modifie un produit, entre une valeur du prix de trop coûteux et clique sur le bouton de mise à jour, la validation côté client sera contournée et résulte une publication (postback). Lors de la publication, la page s ASP.NET `UpdateCommand` s’exécute le Gestionnaire d’événements et une exception est levée lors de la tentative d’analyse trop coûteux pour une `Decimal`. Étant donné que nous avons la gestion des exceptions, cette exception est gérée correctement, mais nous pourrions empêcher des données non valides à partir de glissement via en premier lieu en poursuivant uniquement avec le `UpdateCommand` Gestionnaire d’événements si `Page.IsValid` a la valeur `True`.

Ajoutez le code suivant au début de la `UpdateCommand` Gestionnaire d’événements, immédiatement avant le `Try` bloc :


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Avec cet ajout, le produit va tenter de mettre à jour uniquement si les données envoyées sont valides. La plupart des utilisateurs qui ont remporté le t être en mesure de données non valides en raison des scripts côté client de contrôles validation de publication, mais les utilisateurs dont les navigateurs n prise en charge t JavaScript ou qui ont JavaScript prend en charge est désactivé, peuvent contourner les vérifications côté client et envoyer des données non valides.

> [!NOTE]
> Le lecteur astucieux Rappelez-vous que lors de la mise à jour des données avec le contrôle GridView, nous n’a besoin à vérifier explicitement la `Page.IsValid` propriété dans notre classe code-behind de page s. Il s’agit, car le contrôle GridView consulte la `Page.IsValid` propriété nous et uniquement les continue avec la mise à jour uniquement si elle retourne une valeur de `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Étape 3 : Synthèse des problèmes de saisie de données

Outre les contrôles de validation de cinq, ASP.NET propose le [contrôle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), qui affiche le `ErrorMessage` s de ces contrôles de validation qui a détecté des données non valides. Ces données peuvent figurer sous forme de texte sur la page web ou via un messagebox modale, côté client. Permettent de s’améliorer ce didacticiel pour inclure un messagebox côté client résumant les problèmes de validation.

Pour ce faire, faites glisser un contrôle ValidationSummary à partir de la boîte à outils vers le concepteur. L’emplacement de t ValidationSummary contrôle n’a pas grande importance depuis que nous allons configurer pour afficher uniquement le résumé en tant qu’un élément messagebox. Après avoir ajouté le contrôle, définissez son [ `ShowSummary` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) à `False` et son [ `ShowMessageBox` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) à `True`. Ainsi, les erreurs de validation sont résumées dans un messagebox côté client (voir Figure 6).


[![Les erreurs de Validation sont résumés dans Messagebox côté Client](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figure 6**: les erreurs de Validation sont résumés dans Messagebox côté Client ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment réduire la probabilité d’exceptions à l’aide de contrôles de validation afin de façon proactive que nos entrées d’utilisateurs sont valides avant d’essayer de les utiliser dans le flux de travail de mise à jour. ASP.NET fournit cinq contrôles Web de validation sont conçus pour inspecter un site Web particulier contrôlent s d’entrée et un rapport attestant de la validité de s d’entrée. Dans ce didacticiel nous avons utilisé deux de ces cinq contrôles RequiredFieldValidator et CompareValidator pour vous assurer que le nom de produit s a été fourni et que le prix a un format monétaire avec une valeur supérieure ou égale à zéro.

Ajout de contrôles de validation pour le s DataList modification d’interface est aussi simple que les faisant glisser sur le `EditItemTemplate` à partir de la boîte à outils et en définissant un certain nombre de propriétés. Par défaut, les contrôles de validation émettent automatiquement un script de validation côté client ; elles fournissent également une validation côté serveur lors de la publication, en stockant le résultat cumulé dans le `Page.IsValid` propriété. Pour contourner la validation côté client, un clic sur un bouton, LinkButton ou ImageButton, le bouton Définir s `CausesValidation` propriété `False`. En outre, avant d’effectuer des tâches avec les données envoyées sur la publication (postback), vérifiez que le `Page.IsValid` retourne de la propriété `True`.

Tous le contrôle DataList modification didacticiels nous ve analysée jusqu'à présent ont des interfaces de modifications très simples, une zone de texte pour le nom du produit s et l’autre pour le prix. L’interface de modification, toutefois, peut contenir un mélange de contrôles Web différents, comme DropDownList, les calendriers, les cases d’option, les cases à cocher et ainsi de suite. Dans notre prochain didacticiel, nous examinerons la création d’une interface qui utilise un certain nombre de contrôles Web.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Dennis Patterson, Ken Pespisa et Liz Shulok. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](handling-bll-and-dal-level-exceptions-vb.md)
> [Suivant](customizing-the-datalist-s-editing-interface-vb.md)
