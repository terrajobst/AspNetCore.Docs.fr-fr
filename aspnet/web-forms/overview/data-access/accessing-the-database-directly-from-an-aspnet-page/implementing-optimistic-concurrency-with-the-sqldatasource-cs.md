---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Implémentation de l’accès concurrentiel optimiste avec SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous passez en revue les éléments essentiels de contrôle d’accès concurrentiel optimiste et puis Explorez comment l’implémenter à l’aide du contrôle SqlDataSource.
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 2907e05dfa82d44d38217b90d85d5c2192613560
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811463"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implémentation de l’accès concurrentiel optimiste avec SqlDataSource (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) ou [télécharger le PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> Dans ce didacticiel, nous passez en revue les éléments essentiels de contrôle d’accès concurrentiel optimiste et puis Explorez comment l’implémenter à l’aide du contrôle SqlDataSource.


## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons examiné comment ajouter l’insertion, de la mise à jour et de suppression des fonctionnalités au contrôle SqlDataSource. En bref, pour fournir ces fonctionnalités nous avions besoin spécifier le correspondantes `INSERT`, `UPDATE`, ou `DELETE` instruction SQL dans le contrôle s `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` propriétés, ainsi que le texte approprié paramètres dans le `InsertParameters`, `UpdateParameters`, et `DeleteParameters` collections. Bien que ces propriétés et les collections peuvent être spécifiées manuellement, le bouton Avancé de configurer la Source de données Assistant s offre un générer `INSERT`, `UPDATE`, et `DELETE` case à cocher des instructions qui crée automatiquement ces instructions basé sur le `SELECT` instruction.

En même temps que la génération `INSERT`, `UPDATE`, et `DELETE` instructions case à cocher, la boîte de dialogue Options de génération SQL avancées inclut une option de l’accès concurrentiel optimiste utiliser (voir Figure 1). Lorsqu’elle est activée, le `WHERE` clauses dans le nom généré automatiquement `UPDATE` et `DELETE` instructions ont été modifiées pour effectuer uniquement la mise à jour ou de suppression si t ne données base de données sous-jacente été modifiée depuis l’utilisateur dernier chargée des données dans la grille.


![Vous pouvez ajouter la prise en charge de l’accès concurrentiel optimiste à partir de l’avancée boîte de dialogue Options de génération SQL](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Figure 1**: vous pouvez ajouter la prise en charge de l’accès concurrentiel optimiste à partir de l’avancée boîte de dialogue Options de génération SQL


Dans le [implémentation de l’accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) didacticiel, nous avons examiné les principes fondamentaux du contrôle d’accès concurrentiel optimiste et comment l’ajouter à ObjectDataSource. Dans ce didacticiel nous retoucher sur l’essentiel de contrôle d’accès concurrentiel optimiste et puis Explorez comment l’implémenter à l’aide de SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Un récapitulatif de l’accès concurrentiel optimiste

Pour les applications web qui permettent à plusieurs utilisateurs simultanés pour modifier ou supprimer les mêmes données, il existe un risque qu’un utilisateur peut remplacer accidentellement un autre modifie s. Dans le [implémentation de l’accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) didacticiel que je fournis à l’exemple suivant :

Imaginez que deux utilisateurs, Jisun et Sam, ont été les deux accédant à une page dans une application qui a autorisé les visiteurs à mettre à jour et supprimer des produits à travers un contrôle GridView. Cliquez sur le bouton Modifier pour Chai en même temps. Jisun modifie le nom du produit à Chai thé et clique sur le bouton de mise à jour. Le résultat net est une `UPDATE` instruction est envoyée à la base de données définit *tous les* des champs modifiables produit s (même si Jisun mise à jour uniquement un champ, `ProductName`). À ce stade, la base de données a les valeurs Chai thé, la catégorie boissons, liquides exotiques fournisseur, et ainsi de suite de ce produit particulier. Toutefois, le contrôle GridView à l’écran de s Sam affiche toujours le nom du produit dans la ligne GridView modifiable en tant que Chai. Quelques secondes après que Jisun s modifications ont été validées, Sam met à jour de la catégorie à Condiments et clique sur mise à jour. Il en résulte un `UPDATE` instruction envoyée à la base de données qui définit le nom du produit qui correspond à Chai, le `CategoryID` à l’ID de catégorie Condiments correspondant et ainsi de suite. Modifications de s Jisun au nom du produit ont été remplacées.

La figure 2 illustre cette interaction.


[![Lorsque deux utilisateurs mettre à jour simultanément un enregistrement il s pour un utilisateur s les modifications potentielles pour remplacer les autres opérations de mappage](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Figure 2**: lorsque deux utilisateurs simultanément mise à jour un enregistrement il s potentiel pour un utilisateur s modifie à remplacer les autres opérations de mappage ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Pour éviter ce type de dépliage, une forme de [contrôle d’accès concurrentiel](http://en.wikipedia.org/wiki/Concurrency_control) doit être implémentée. [L’accès concurrentiel optimiste](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) l’objectif de ce didacticiel fonctionne sur l’hypothèse que bien que les conflits d’accès concurrentiel peut être ainsi, la grande majorité du temps ces conflits a gagné t surviennent. Par conséquent, si un conflit se produit, contrôle d’accès concurrentiel optimiste informe simplement l’utilisateur enregistrer leur modifications impossible, car un autre utilisateur a modifié les mêmes données.

> [!NOTE]
> Pour les applications où il est supposé qu’il y aura plusieurs conflits d’accès concurrentiel ou si ces conflits sont inacceptables, puis d’accès concurrentiel pessimiste contrôle peut être utilisé à la place. Faire référence à la [implémentation de l’accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) didacticiel pour une discussion plus détaillée sur le contrôle d’accès concurrentiel pessimiste.


Contrôle d’accès concurrentiel optimiste fonctionne en veillant à ce que l’enregistrement en cours de mise à jour ou supprimé a les mêmes valeurs, comme c’était le cas au démarrage de la mise à jour ou la suppression des processus. Par exemple, lorsque vous cliquez sur le bouton Modifier dans un GridView modifiable, les valeurs de l’enregistrement s sont lire à partir de la base de données et affichées dans les zones de texte et d’autres contrôles Web. Ces valeurs d’origine sont enregistrés par le contrôle GridView. Versions ultérieures, une fois que l’utilisateur modifie son et clique sur le bouton de mise à jour, la `UPDATE` instruction utilisée doit prendre en compte les valeurs d’origine ainsi que les nouvelles valeurs et uniquement mettre à jour l’enregistrement de base de données sous-jacent si les valeurs d’origine que l’utilisateur a commencé à modifier sont identiques aux valeurs toujours dans la base de données. Figure 3 illustre cette séquence d’événements.


[![Pour la mise à jour ou la suppression réussisse, les valeurs d’origine doivent être égales pour les valeurs actuelles de la base de données](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Figure 3**: pour la mise à jour ou supprimer pour réussir, le d’origine valeurs doit être égale aux valeurs de base de données ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Il existe différentes approches d’implémentation de l’accès concurrentiel optimiste (consultez [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [logique de la mise à jour d’accès concurrentiel Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) pour un bref aperçu présentant un nombre d’options). La technique utilisée par SqlDataSource (ainsi que par le typés ADO.NET utilisé dans notre couche d’accès aux données) renforce la `WHERE` clause pour inclure une comparaison de toutes les valeurs d’origine. Ce qui suit `UPDATE` instruction, par exemple, des mises à jour le nom et le prix d’un produit uniquement si les valeurs actuelles de la base de données sont égales aux valeurs qui ont été récupérées à l’origine lors de la mise à jour l’enregistrement dans le contrôle GridView. Le `@ProductName` et `@UnitPrice` paramètres contiennent les nouvelles valeurs entrées par l’utilisateur, tandis que `@original_ProductName` et `@original_UnitPrice` contiennent les valeurs qui ont été chargées à l’origine dans le contrôle GridView lorsque l’utilisateur a cliqué sur le bouton Modifier :


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Comme nous le verrons dans ce didacticiel, il est aussi simple que la vérification d’une case à cocher de l’activation du contrôle d’accès concurrentiel optimiste avec SqlDataSource.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Étape 1 : Création d’un SqlDataSource prenant en charge l’accès concurrentiel optimiste

Commencez par ouvrir le `OptimisticConcurrency.aspx` page à partir de la `SqlDataSource` dossier. Faites glisser un contrôle SqlDataSource à partir de la boîte à outils vers le concepteur, les paramètres de son `ID` propriété `ProductsDataSourceWithOptimisticConcurrency`. Ensuite, cliquez sur le lien configurer la Source de données à partir de la balise active de contrôle s. Dans le premier écran dans l’Assistant, choisissez de travailler avec le `NORTHWINDConnectionString` et cliquez sur Suivant.


[![Choisissez de travailler avec le NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Figure 4**: choisissez de travailler avec les `NORTHWINDConnectionString` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


Pour cet exemple, nous ajouterons un GridView qui permet aux utilisateurs de modifier le `Products` table. Par conséquent, à partir de la configuration de l’écran de l’instruction Select, choisissez le `Products` table dans la liste déroulante et sélectionnez le `ProductID`, `ProductName`, `UnitPrice`, et `Discontinued` colonnes, comme illustré à la Figure 5.


[![À partir de la Table Products, retourner le ProductID, ProductName, UnitPrice et les colonnes supprimées](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Figure 5**: à partir de la `Products` Table, retournez le `ProductID`, `ProductName`, `UnitPrice`, et `Discontinued` colonnes ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Après avoir sélectionné les colonnes, cliquez sur le bouton Avancé pour afficher la boîte de dialogue Options de génération SQL avancées. Vérifiez la générer `INSERT`, `UPDATE`, et `DELETE` instructions et utilisez les cases à cocher de l’accès concurrentiel optimiste, puis cliquez sur OK (voir Figure 1 pour une capture d’écran). Terminez l’Assistant en cliquant sur Suivant, puis sur Terminer.

À l’issue de l’Assistant Configurer la Source de données, prenez un moment pour examiner les résultats `DeleteCommand` et `UpdateCommand` propriétés et le `DeleteParameters` et `UpdateParameters` collections. Pour ce faire, le plus simple consiste à cliquer sur l’onglet Source dans l’angle inférieur gauche pour afficher la syntaxe déclarative s de page. Vous y trouverez un `UpdateCommand` valeur :


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Avec sept paramètres dans le `UpdateParameters` collection :


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

De même, le `DeleteCommand` propriété et `DeleteParameters` collection doit ressembler à ce qui suit :


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

En plus de l’augmentation de la `WHERE` clauses de la `UpdateCommand` et `DeleteCommand` propriétés (et ajoutant les paramètres supplémentaires pour les collections de paramètres respectifs), en sélectionnant l’utilisation optimiste option d’accès concurrentiel s’ajuste à deux autres propriétés :

- Modifications du [ `ConflictDetection` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) à partir de `OverwriteChanges` (la valeur par défaut) pour `CompareAllValues`
- Modifications du [ `OldValuesParameterFormatString` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) de {0} (la valeur par défaut) vers l’original\_ {0} .

Lorsque les données de contrôle Web appelant les opérations de mappage SqlDataSource `Update()` ou `Delete()` (méthode), il passe les valeurs d’origine. Si les opérations de mappage SqlDataSource `ConflictDetection` propriété est définie sur `CompareAllValues`, ces valeurs d’origine sont ajoutés à la commande. Le `OldValuesParameterFormatString` propriété fournit le modèle d’affectation de noms utilisé pour ces paramètres de valeur d’origine. L’Assistant Configurer la Source de données utilise d’origine\_ {0} et noms de chaque paramètre d’origine dans le `UpdateCommand` et `DeleteCommand` propriétés et `UpdateParameters` et `DeleteParameters` collections en conséquence.

> [!NOTE]
> Étant donné que nous n’utilisez ne pas les opérations de mappage contrôle SqlDataSource insertion de fonctionnalités, n’hésitez pas à supprimer le `InsertCommand` propriété et sa `InsertParameters` collection.


## <a name="correctly-handlingnullvalues"></a>Gérer correctement`NULL`valeurs

Malheureusement, l’augmentée `UPDATE` et `DELETE` généré automatiquement les instructions par l’Assistant Configurer la Source de données lors de l’utilisation de l’accès concurrentiel optimiste faire *pas* fonctionnent avec les enregistrements qui contiennent `NULL` valeurs. Pour comprendre pourquoi, envisagez de notre s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

Le `UnitPrice` colonne dans le `Products` table peut avoir `NULL` valeurs. Si un enregistrement particulier a un `NULL` valeur `UnitPrice`, le `WHERE` partie de la clause `[UnitPrice] = @original_UnitPrice` sera *toujours* ont la valeur False, car `NULL = NULL` retourne toujours False. Par conséquent, les enregistrements qui contiennent `NULL` valeurs ne peut pas être modifiés ou supprimés, comme le `UPDATE` et `DELETE` instructions `WHERE` clauses a gagné t retournent toutes les lignes pour mettre à jour ou supprimer.

> [!NOTE]
> Ce bogue a été signalé initialement à Microsoft en juin 2004 dans [SqlDataSource génère des instructions SQL inexactes](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) et disponible doit être résolu dans la prochaine version d’ASP.NET.


Pour résoudre ce problème, nous devons mettre à jour manuellement la `WHERE` clauses à la fois dans le `UpdateCommand` et `DeleteCommand` propriétés pour **tous les** colonnes pouvant avoir `NULL` valeurs. En général, modifier `[ColumnName] = @original_ColumnName` à :


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Cette modification peut être effectuée directement via le balisage déclaratif, via les options UpdateQuery ou DeleteQuery à partir de la fenêtre Propriétés, ou via la mise à jour et supprimer des onglets dans la spécifiez une instruction SQL personnalisée ou une option de procédure stockée dans les données de configuration Assistant source. Là encore, cette modification doit être effectuée pour *chaque* colonne dans le `UpdateCommand` et `DeleteCommand` s `WHERE` clause pouvant contenir `NULL` valeurs.

Cette application à notre exemple donne le résultat suivant modifié `UpdateCommand` et `DeleteCommand` valeurs :


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Étape 2 : Ajout d’un GridView avec modifier et les Options de suppression

Avec SqlDataSource configuré pour prendre en charge l’accès concurrentiel optimiste, il reste qu’à ajouter un contrôle Web de données à la page qui utilise ce contrôle d’accès concurrentiel. Pour ce didacticiel, permettent d’ajouter un GridView qui fournit à la fois modifier et supprimer des fonctionnalités s. Pour ce faire, faites glisser un GridView à partir de la boîte à outils vers le concepteur et le jeu de son `ID` à `Products`. À partir de la balise active de s GridView, liez-le à le `ProductsDataSourceWithOptimisticConcurrency` contrôle SqlDataSource ajouté à l’étape 1. Enfin, vérifiez les options Activer la modification et activer la suppression à partir de la balise active.


[![Lier le contrôle GridView à SqlDataSource et activer la modification et suppression](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Figure 6**: lier le contrôle GridView à SqlDataSource et d’activer la modification et de suppression ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


Après avoir ajouté le contrôle GridView, configurer son apparence en supprimant le `ProductID` BoundField, modification de la `ProductName` BoundField s `HeaderText` propriété produit, et de la mise à jour le `UnitPrice` BoundField afin que son `HeaderText` propriété est Il vous suffit de prix. Dans l’idéal, nous d améliorer l’interface de modification pour inclure un contrôle RequiredFieldValidator pour le `ProductName` valeur et un CompareValidator pour les `UnitPrice` valeur (pour vous assurer qu’il s une valeur numérique au format correct). Reportez-vous à la [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) décrivant la plus approfondi de personnalisation de la s GridView modification d’interface.

> [!NOTE]
> Le contrôle GridView état d’affichage s doit être activée dans la mesure où les valeurs d’origine passés à partir de la GridView à SqlDataSource sont stockées dans l’affichage état.


Après avoir apporté ces modifications au GridView, le balisage déclaratif GridView et SqlDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Pour afficher le contrôle d’accès concurrentiel optimiste en action, ouvrez deux fenêtres de navigateur et charger le `OptimisticConcurrency.aspx` page dans les deux. Cliquez sur les boutons de modification pour le premier produit dans les deux navigateurs. Dans un navigateur, modifiez le nom de produit et cliquez sur la mise à jour. Le navigateur sera publication (postback) et le contrôle GridView renverra au mode édition préalable, montrant le nouveau nom de produit pour l’enregistrement de modification uniquement.

Dans la deuxième fenêtre de navigateur, modifier le prix (mais laisser le nom du produit en tant que sa valeur d’origine) et cliquez sur la mise à jour. Lors de la publication, la grille retourne au mode édition préalable, mais la modification du prix n’est pas enregistrée. Le second navigateur montre la même valeur que le premier le nouveau nom de produit avec l’ancien prix. Les modifications apportées dans la deuxième fenêtre de navigateur ont été perdues. En outre, les modifications ont été perdues au lieu de cela en mode silencieux, en ne raison d’aucune exception ou un message indiquant qu’une violation d’accès concurrentiel s’est produite.


[![Les modifications dans la deuxième fenêtre de navigateur ont été perdues en mode silencieux](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Figure 7**: les modifications dans le deuxième navigateur fenêtre ont été en mode silencieux perdus ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


La raison pourquoi les modifications de navigateur s deuxième n’ont pas étées était car le `UPDATE` instruction s `WHERE` clause éliminé tous les enregistrements et par conséquent n’a affecté aucune ligne. S permettent d’examiner le `UPDATE` instruction à nouveau :


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Lorsque la deuxième fenêtre de navigateur met à jour l’enregistrement, le nom de produit d’origine spécifié dans le `WHERE` clause n t correspondance inscrire avec le nom de produit existant (dans la mesure où il a été modifiée par le premier navigateur). Par conséquent, l’instruction `[ProductName] = @original_ProductName` retourne False et le `UPDATE` n’affecte pas d’enregistrement.

> [!NOTE]
> Fonctionnement de Delete de la même manière. Avec deux fenêtres du navigateur ouvertes, commencez par modification d’un produit donné avec un et puis d’enregistrer ses modifications. Après avoir enregistré les modifications dans le navigateur, cliquez sur le bouton Supprimer pour le même produit dans l’autre. Étant donné que le don de valeurs d’origine t correspondre dans le `DELETE` instruction s `WHERE` clause, la suppression échoue en mode silencieux.


L’utilisateur final s du point de vue dans la deuxième fenêtre de navigateur, après avoir cliqué sur le bouton de mise à jour la grille retourne au mode d’édition préalable, mais leurs modifications ont été perdues. Toutefois, cet emplacement s aucun retour visuel respecter leur t ne savais pas de modifications. Dans l’idéal, si un utilisateur s modifications sont perdues une violation d’accès concurrentiel, nous d notifier ces derniers et, peut-être, maintenir la grille en mode édition. Permettent de voir comment cela s.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Étape 3 : Déterminer quand une Violation d’accès concurrentiel s’est produite

Dans la mesure où une violation d’accès concurrentiel rejette les modifications a été, il serait intéressant avertir l’utilisateur lorsqu’une violation d’accès concurrentiel s’est produite. Pour avertir l’utilisateur, s permettent d’ajouter un contrôle Web Label en haut de la page nommée `ConcurrencyViolationMessage` dont `Text` propriété affiche le message suivant : vous avez tenté de mettre à jour ou supprimer un enregistrement qui a été mises à jour simultanément par un autre utilisateur. Passez en revue les modifications des autres utilisateurs puis rétablir votre mise à jour ou supprimer. Définir le contrôle d’étiquette s `CssClass` propriété à avertissement, qui est une classe CSS définie dans `Styles.css` qui affiche le texte dans une police rouge, italique, gras et grande. Enfin, définissez l’étiquette s `Visible` et `EnableViewState` propriétés à `false`. Cela permet de masquer l’étiquette à l’exception uniquement ces publications où nous définissons explicitement son `Visible` propriété `true`.


[![Ajouter un contrôle étiquette à la Page pour afficher l’avertissement](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Figure 8**: ajouter un contrôle étiquette à la Page pour afficher l’avertissement ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Lorsque vous effectuez une mise à jour ou suppression, les opérations de mappage GridView `RowUpdated` et `RowDeleted` gestionnaires d’événements se déclenchent après que son contrôle de source de données a effectué la mise à jour demandée ou la suppression. Nous pouvons déterminer combien de lignes ont été affectés par l’opération à partir de ces gestionnaires d’événements. Si aucune ligne ont été affectés, nous souhaitons afficher la `ConcurrencyViolationMessage` étiquette.

Créer un gestionnaire d’événements pour les deux le `RowUpdated` et `RowDeleted` événements et ajoutez le code suivant :


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

Dans les deux gestionnaires d’événements, nous vérifions le `e.AffectedRows` propriété et, si elle est égale à 0, définissez la `ConcurrencyViolationMessage` étiquette s `Visible` propriété `true`. Dans le `RowUpdated` Gestionnaire d’événements, nous indiquons également le contrôle GridView à rester en mode édition en définissant le `KeepInEditMode` true à la propriété. Ce faisant, nous avons besoin relier les données à la grille afin que les autres données de l’utilisateur se sont chargées dans l’interface de modification. Cela est accompli en appelant le s GridView `DataBind()` (méthode).

Comme le montre la Figure 9, ces deux gestionnaires d’événements, un message très sensible s’affiche chaque fois qu’une violation d’accès concurrentiel se produit.


[![Un Message s’affiche en cas d’une Violation d’accès concurrentiel](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Figure 9**: un Message s’affiche en cas d’une Violation d’accès concurrentiel ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Récapitulatif

Lors de la création d’une application web où de multiples utilisateurs peuvent modifier les mêmes données, il est important de tenir compte des options de contrôle d’accès concurrentiel. Par défaut, les données d’ASP.NET que contrôles Web et les contrôles de source de données n’utilisent pas de n’importe quel contrôle d’accès concurrentiel. Comme nous l’avons vu dans ce didacticiel, l’implémentation de contrôle d’accès concurrentiel optimiste avec SqlDataSource est relativement simple et rapide. SqlDataSource gère la plupart du gros du travail pour votre ajout augmentée `WHERE` clauses pour le nom généré automatiquement `UPDATE` et `DELETE` , mais les instructions sont quelques subtilités dans Gestion des `NULL` valeur des colonnes, comme indiqué dans le Gérer correctement `NULL` section de valeurs.

Ce didacticiel conclut notre examen de SqlDataSource. Nos didacticiels restants retournera à l’utilisation des données à l’aide de l’ObjectDataSource et l’architecture à plusieurs niveaux.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [Suivant](querying-data-with-the-sqldatasource-control-vb.md)
