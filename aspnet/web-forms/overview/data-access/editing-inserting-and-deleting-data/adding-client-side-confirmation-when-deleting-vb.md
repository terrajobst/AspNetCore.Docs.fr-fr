---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Ajout d’une Confirmation du côté Client lors de la suppression (VB) | Microsoft Docs
author: rick-anderson
description: Dans les interfaces, nous avons créé jusqu'à présent, un utilisateur peut supprimer accidentellement les données en cliquant sur le bouton Supprimer quand ils signifiait cliquer sur le bouton Modifier. Dans ce t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: deae088d1daa63e2936aedf80eded18588b1ec60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833793"
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>Ajout d’une Confirmation du côté Client lors de la suppression (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) ou [télécharger le PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> Dans les interfaces, nous avons créé jusqu'à présent, un utilisateur peut supprimer accidentellement les données en cliquant sur le bouton Supprimer quand ils signifiait cliquer sur le bouton Modifier. Dans ce didacticiel, nous allons ajouter une boîte de dialogue de confirmation côté client qui s’affiche lorsque l’utilisateur clique sur le bouton Supprimer.


## <a name="introduction"></a>Introduction

Sur les didacticiels de plusieurs cours nous ve vu comment utiliser notre architecture d’application et les données des contrôles Web ObjectDataSource conjointement pour fournir l’insertion, de modification et de suppression de fonctionnalités. La suppression des interfaces nous ve examiné jusqu'à présent ont été composé d’une suppression bouton qui, lorsque vous cliquez sur, provoque une publication (postback) et appelle les opérations de mappage ObjectDataSource `Delete()` (méthode). Le `Delete()` méthode appelle ensuite la méthode configurée à partir de la couche de logique métier, qui se propage l’appel à la couche Data Access, émission réelle `DELETE` instruction à la base de données.

Cette interface utilisateur permet aux visiteurs supprimer des enregistrements via les contrôles GridView, DetailsView ou FormView, il ne dispose pas d’une forme quelconque de confirmation lorsque l’utilisateur clique sur le bouton Supprimer. Si un utilisateur clique sur accidentellement le bouton Supprimer quand ils signifiait cliquer sur Modifier, l’enregistrement qu'ils destinés à mettre à jour est supprimé à la place. Pour résoudre ce problème, dans ce didacticiel, nous allons ajouter une boîte de dialogue de confirmation côté client qui s’affiche lorsque l’utilisateur clique sur le bouton Supprimer.

Le code JavaScript `confirm(string)` fonction affiche son paramètre d’entrée de chaîne en tant que le texte à l’intérieur d’une boîte de dialogue modale qui est équipé à deux boutons - OK et Annuler (voir Figure 1). Le `confirm(string)` fonction retourne une valeur booléenne en fonction de l’utilisateur clique sur le bouton (`true`, si l’utilisateur clique sur OK, et `false` s’il clique sur Annuler).


![La méthode de confirm(string) JavaScript affiche Modal, côté Client Messagebox](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Figure 1**: le code JavaScript `confirm(string)` méthode affiche un Messagebox modale, côté Client


Au cours de l’envoi d’un formulaire, si la valeur `false` est retournée à partir d’un gestionnaire d’événements côté client, alors l’envoi du formulaire est annulée. À l’aide de cette fonctionnalité, nous pouvons avoir du côté client de bouton s Delete `onclick` Gestionnaire d’événements retournent la valeur d’un appel à `confirm("Are you sure you want to delete this product?")`. Si l’utilisateur clique sur Annuler, `confirm(string)` renvoie false, ce qui provoque l’envoi du formulaire d’annulation. Avec aucune publication (postback), le produit dont bouton Delete a gagné t être supprimé. Si, toutefois, l’utilisateur clique sur OK dans la boîte de dialogue de confirmation, la publication (postback) va continuer sans interruption, et le produit sera supprimé. Consultez [JavaScript à l’aide de s `confirm()` méthode à l’envoi d’un formulaire contrôle](http://www.webreference.com/programming/javascript/confirm/) pour plus d’informations sur cette technique.

Ajouter le script côté client nécessaire est légèrement différente si vous utilisez des modèles que quand vous utilisez un CommandField. Par conséquent, dans ce didacticiel nous allons examiner un FormView et GridView exemple.

> [!NOTE]
> À l’aide de techniques de confirmation côté client, telles que celles abordées dans ce didacticiel, part du principe que vos utilisateurs visitent avec les navigateurs qui prennent en charge JavaScript et qu’ils disposent de JavaScript est activé. Si une de ces hypothèses ne sont pas remplie pour un utilisateur particulier, en cliquant sur le bouton Supprimer sera immédiatement provoquer une publication (postback) (ne pas et affiche un messagebox confirmer).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Étape 1 : Création d’un FormView qui prend en charge la suppression

Commencez par ajouter un FormView à la `ConfirmationOnDelete.aspx` page dans le `EditInsertDelete` dossier, liant à un nouveau ObjectDataSource qui extrait dans les informations de produit via le `ProductsBLL` classe s `GetProducts()` (méthode). Configurer également ObjectDataSource afin que le `ProductsBLL` classe s `DeleteProduct(productID)` méthode est mappée à la s ObjectDataSource `Delete()` méthode ; Vérifiez que les onglets INSERT et UPDATE, listes déroulantes sont définies à (None). Enfin, vérifiez la case à cocher Activer la pagination dans la balise active de s FormView.

Après ces étapes, le balisage déclaratif de nouveau ObjectDataSource s se présente comme suit :


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Comme dans nos exemples de cours qui n’utilisaient pas d’accès concurrentiel optimiste, prenez un moment pour effacer le s ObjectDataSource `OldValuesParameterFormatString` propriété.

Dans la mesure où il a été lié à un contrôle ObjectDataSource qui prend uniquement en charge la suppression, les opérations de mappage FormView `ItemTemplate` offre uniquement le bouton Supprimer, sans les boutons Nouveau et mise à jour. Toutefois, le balisage déclaratif de s FormView, inclut un superflu `EditItemTemplate` et `InsertItemTemplate`, ce qui peut être supprimé. Prenez un moment pour personnaliser le `ItemTemplate` sorte qu’il affiche uniquement un sous-ensemble du produit des champs de données. Je ve configuré mien pour afficher le nom de produit s dans un `<h3>` titre au-dessus de ses noms de fournisseur et de catégorie (ainsi que le bouton Supprimer).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Avec ces modifications, nous avons une page web entièrement fonctionnelle qui permet à un utilisateur de basculer entre les produits celui à la fois, avec la possibilité de supprimer un produit en cliquant simplement sur le bouton Supprimer. Figure 2 illustre une capture d’écran de notre progression jusqu'à présent lorsqu’ils sont affichés via un navigateur.


[![Le contrôle FormView affiche des informations sur un produit unique](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Figure 2**: le FormView affiche des informations sur un produit unique ([cliquez pour afficher l’image en taille réelle](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Étape 2 : Appel de la fonction confirm(string) à partir de l’événement de supprimer des boutons côté Client onclick

Avec le contrôle FormView créé, l’étape finale consiste à configurer le bouton supprimer ces qu’au moment où il s cliqué par le visiteur, le code JavaScript `confirm(string)` fonction est appelée. Ajout d’un script côté client à un bouton, LinkButton ou ImageButton s côté client `onclick` événement peut être accompli à l’aide de la `OnClientClick property`, qui est une nouveauté pour ASP.NET 2.0. Étant donné que nous souhaitons que la valeur de la `confirm(string)` retournée de la fonction, définissez simplement cette propriété sur : `return confirm('Are you certain that you want to delete this product?');`

Après cette modification de la syntaxe déclarative de supprimer le LinkButton s doit ressembler à :


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

S résume-t-elle est ! Figure 3 illustre une capture d’écran de cette confirmation en action. Cliquer sur le bouton Supprimer pour afficher la boîte de dialogue Confirmer. Si l’utilisateur clique sur Annuler, la publication (postback) est annulée et le produit n’est pas supprimé. Si, toutefois, l’utilisateur clique sur OK, continue de la publication (postback) et les opérations de mappage ObjectDataSource `Delete()` méthode est appelée, sanctionné dans l’enregistrement de base de données en cours de suppression.

> [!NOTE]
> La chaîne passée dans le `confirm(string)` fonction JavaScript est délimitée par des apostrophes (au lieu des guillemets). Dans JavaScript, les chaînes peuvent être délimités à l’aide de type caractère. Nous utilisons des apostrophes ici afin que les séparateurs pour la chaîne passée dans `confirm(string)` n’introduisent pas une ambiguïté avec séparateurs utilisés pour le `OnClientClick` valeur de propriété.


[![Une Confirmation est maintenant affichée lorsque en cliquant sur le bouton Supprimer](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Figure 3**: une Confirmation est maintenant affichée lorsque en cliquant sur le bouton Supprimer ([cliquez pour afficher l’image en taille réelle](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Étape 3 : Configuration de la propriété OnClientClick pour le bouton Supprimer dans un CommandField

Lorsque vous travaillez avec un bouton, LinkButton ou ImageButton directement dans un modèle, une boîte de dialogue de confirmation peut être associée avec lui en configurant simplement son `OnClientClick` propriété pour retourner les résultats de l’interface JavaScript `confirm(string)` (fonction). Toutefois, le CommandField, qui ajoute un champ de boutons de suppression à un GridView ou à DetailsView - n’a pas un `OnClientClick` propriété qui peut être définie de manière déclarative. Au lieu de cela, nous devons référencer par programme le bouton Supprimer dans le s GridView ou DetailsView approprié `DataBound` Gestionnaire d’événements, puis définissez son `OnClientClick` propriété il.

> [!NOTE]
> Lorsque vous définissez le bouton Supprimer s `OnClientClick` propriété dans le texte approprié `DataBound` Gestionnaire d’événements, nous avons accès aux données ont été liées à l’enregistrement actif. Cela signifie que nous pouvons étendre le message de confirmation pour inclure des détails sur l’enregistrement particulier, tel que « Êtes-vous êtes-vous sûr de que vouloir supprimer le produit Chai ? » Cette personnalisation est également possible dans les modèles à l’aide de la syntaxe de liaison de données.


Paramètre d’application pratique la `OnClientClick` propriété pour le bouton Supprimer dans un CommandField, let s ajouter un GridView à la page. Configurez ce GridView pour utiliser le même contrôle ObjectDataSource qui utilise le contrôle FormView. Également limiter les opérations de mappage GridView BoundFields pour inclure uniquement le nom du produit s, la catégorie et le fournisseur. Enfin, cochez la case à cocher Activer la suppression de la balise active de s GridView. Cela ajoutera un CommandField au GridView s `Columns` collection avec son `ShowDeleteButton` propriété définie sur `true`.

Après avoir apporté ces modifications, votre balisage déclaratif de GridView s doit ressembler à ce qui suit :


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

Le CommandField contient une seule instance de supprimer le LinkButton qui sont accessibles par programme à partir de la s GridView `RowDataBound` Gestionnaire d’événements. Une fois référencé, nous pouvons définir son `OnClientClick` propriété en conséquence. Créer un gestionnaire d’événements pour le `RowDataBound` événement utilisant le code suivant :


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Ce gestionnaire d’événements fonctionne avec les lignes de données (ceux qui ont le bouton Supprimer) et commence par référencer par programme le bouton Supprimer. En général, utilisez le modèle suivant :


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* est le type de bouton utilisé par le CommandField - Button, LinkButton ou ImageButton. Par défaut, le CommandField utilise LinkButton, mais cela peut être personnalisé via le s CommandField `ButtonType property`. Le *commandFieldIndex* est l’index ordinal de la CommandField dans le s GridView `Columns` collection, tandis que le *controlIndex* est l’index du bouton Supprimer dans le s CommandField `Controls` collection. Le *controlIndex* valeur dépend de la position du bouton s par rapport à d’autres boutons dans la CommandField. Par exemple, si le seul bouton affiché dans le CommandField est le bouton Supprimer, utilisez un index de 0. Si, toutefois, s’il y a un bouton Modifier qui précède le bouton Supprimer, utilisez un index de 2. La raison pour laquelle un index de 2 est utilisé est car les deux contrôles sont ajoutés à la CommandField avant le bouton Supprimer : le bouton Modifier et un LiteralControl s utilisé pour ajouter l’espace entre les boutons Modifier et supprimer.

Dans notre exemple particulier, le CommandField utilise LinkButton et que, qui est le champ le plus à gauche, a un *commandFieldIndex* de 0. Étant donné qu’aucun autre bouton, mais le bouton Supprimer dans le CommandField, nous utilisons un *controlIndex* de 0.

Après avoir référencé le bouton Supprimer dans le CommandField, nous extrayons ensuite des informations sur le produit lié à la ligne actuelle de GridView. Enfin, nous définissons le bouton Supprimer s `OnClientClick` propriété pour le code JavaScript approprié, qui inclut le nom du produit s. Étant donné que la chaîne JavaScript passé dans le `confirm(string)` fonction est délimitée par des apostrophes, nous devons échapper les apostrophes qui apparaissent dans le nom du produit s. En particulier, les apostrophes dans le nom du produit s sont échappés avec «`\'`».

Avec ces modifications terminées, cliquez sur un bouton Supprimer affiche dans le GridView zone d’une boîte de dialogue de confirmation personnalisé (voir Figure 4). Comme avec le messagebox confirmation à partir de FormView, si l’utilisateur clique sur Annuler la publication (postback) est annulée, ce qui empêche la suppression ne se produise.

> [!NOTE]
> Cette technique peut également être utilisée pour accéder par programme le bouton Supprimer dans le CommandField dans un contrôle DetailsView. Pour le contrôle DetailsView, toutefois, vous d créer un gestionnaire d’événements pour le `DataBound` événement, dans la mesure où le contrôle DetailsView n’a pas un `RowDataBound` événement.


[![En cliquant sur le bouton de suppression de s GridView affiche une boîte de dialogue de Confirmation personnalisé](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Figure 4**: en cliquant sur le bouton Supprimer de s GridView affiche une boîte de dialogue de Confirmation personnalisé ([cliquez pour afficher l’image en taille réelle](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>Utilisation de TemplateField

Un des inconvénients de la CommandField est que ses boutons doit être accessible via l’indexation et que l’objet qui en résulte doit être converti vers le type de bouton approprié (bouton, LinkButton ou ImageButton). À l’aide des « nombres magiques » et codé en dur les types peuvent causer des problèmes qui ne peuvent pas être détectées avant l’exécution. Par exemple, si vous ou un autre développeur, ajoute de nouveaux boutons à la CommandField à un moment donné dans le futur (par exemple, un bouton Modifier) ou des modifications le `ButtonType` propriété, le code existant est toujours compilé sans erreur, mais la visite de la page peut provoquer une exception ou un comportement inattendu, selon la façon dont votre code a été écrit et quelles modifications ont été apportées.

Une autre approche consiste à convertir le s GridView et DetailsView CommandFields TemplateField. Cela générera un TemplateField avec un `ItemTemplate` qui a un LinkButton (ou bouton ou ImageButton) pour chaque bouton dans le CommandField. Ces boutons `OnClientClick` propriétés peuvent être attribuées de façon déclarative, comme nous l’avez vu avec le contrôle FormView ou accessibles par programmation dans approprié `DataBound` Gestionnaire d’événements au format suivant :


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Où *controlID* est la valeur du bouton s `ID` propriété. Bien que ce modèle nécessite toujours un type codé en dur pour la conversion, il supprime la nécessité pour l’indexation, ce qui permet la mise en page Modifier sans ce qui entraîne une erreur d’exécution.

## <a name="summary"></a>Récapitulatif

Le code JavaScript `confirm(string)` fonction est une technique couramment utilisée pour le workflow de soumission de formulaire contrôle. Lors de l’exécution, la fonction affiche une boîte de dialogue modale, côté client qui inclut les deux boutons OK et Annuler. Si l’utilisateur clique sur OK, le `confirm(string)` fonction renvoie `true`; cliquez sur Annuler pour revenir `false`. Cette fonctionnalité, couplée avec un comportement de navigateur s pour annuler l’envoi d’un formulaire si un gestionnaire d’événements pendant le processus de soumission retourne `false`, peut être utilisée pour afficher un messagebox confirmation lors de la suppression d’un enregistrement.

Le `confirm(string)` fonction peut être associée à une bouton Web contrôle s côté client `onclick` Gestionnaire d’événements via le contrôle s `OnClientClick` propriété. Lorsque vous travaillez avec un bouton Supprimer dans un modèle : dans un des modèles FormView s ou dans un TemplateField dans le contrôle DetailsView ou GridView - cette propriété peut être définie soit de manière déclarative ou par programmation, comme nous l’avons vu dans ce didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](implementing-optimistic-concurrency-vb.md)
> [Suivant](limiting-data-modification-functionality-based-on-the-user-vb.md)
