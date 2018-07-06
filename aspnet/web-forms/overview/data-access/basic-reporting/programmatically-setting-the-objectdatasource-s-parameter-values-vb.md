---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Définition par programmation des valeurs de paramètre de l’ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner l’ajout d’une méthode à notre DAL et la couche BLL qui accepte un seul paramètre d’entrée et retourne des données. L’exemple définit ce paramètre...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: d779de4f5bd0d03f413237689e5a64330fcb491d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825780"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Définition par programmation des valeurs de paramètre de l’ObjectDataSource (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) ou [télécharger le PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> Dans ce didacticiel, nous allons examiner l’ajout d’une méthode à notre DAL et la couche BLL qui accepte un seul paramètre d’entrée et retourne des données. L’exemple définit ce paramètre par programmation.


## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans la [didacticiel précédent](declarative-parameters-vb.md), plusieurs options sont disponibles pour le passage de façon déclarative les valeurs de paramètre aux méthodes de l’ObjectDataSource. Si la valeur du paramètre est codé en dur, provient d’un contrôle Web dans la page, ou dans toute autre source qui est lisible par une source de données n’est `Parameter` de l’objet, par exemple, que valeur peut être liée au paramètre d’entrée sans écrire une ligne de code.

Il peut arriver, cependant, lorsque la valeur du paramètre provient de certains source pas déjà pris en compte par un de la source de données intégrés `Parameter` objets. Si notre site pris en charge les comptes d’utilisateur, nous voulons définir le paramètre selon l’actuellement connecté ID d’utilisateur. du visiteur Ou bien, nous sommes amenés à personnaliser la valeur du paramètre avant de les envoyer le long de sous-jacent de l’ObjectDataSource méthode d’objet.

Chaque fois que l’ObjectDataSource `Select` méthode est appelée ObjectDataSource déclenche tout d’abord sa [événement Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Méthode de l’objet sous-jacent de l’ObjectDataSource est ensuite appelée. Une fois que la fin de l’opération de l’ObjectDataSource [sélectionnés événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) se déclenche (Figure 1 illustre cette séquence d’événements). Les valeurs de paramètre passés dans la méthode de l’objet sous-jacent de l’ObjectDataSource peuvent être définies ou personnalisées dans un gestionnaire d’événements pour le `Selecting` événement.


[![L’ObjectDataSource sélectionnés et en sélectionnant le feu événements avant et d’après son objet sous-jacent la méthode est appelée.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figure 1**: The ObjectDataSource `Selected` et `Selecting` événements incendie avant et d’après son objet sous-jacent la méthode est appelée ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


Dans ce didacticiel, nous examinerons l’ajout d’une méthode à notre DAL et la couche BLL qui accepte un seul paramètre d’entrée `Month`, de type `Integer` et retourne un `EmployeesDataTable` objet rempli avec les employés qui ont leur embauche anniversaire spécifié `Month`. Notre exemple définit ce paramètre par programmation selon le mois en cours, en affichant une liste de « Anniversaires ce mois-ci. »

C’est parti !

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Étape 1 : Ajout d’une méthode pour`EmployeesTableAdapter`

Pour notre premier exemple, nous devons ajouter un moyen de récupérer les employés dont `HireDate` s’est produite dans un mois spécifié. Pour offrir cette fonctionnalité conformément à notre architecture que nous devons tout d’abord créer une méthode dans `EmployeesTableAdapter` qui mappe à l’instruction SQL appropriée. Pour ce faire, commencez par ouvrir le DataSet typé de Northwind. Avec le bouton droit sur le `EmployeesTableAdapter` de l’étiquette et choisissez Ajouter une requête.


[![Ajouter une nouvelle requête à la EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figure 2**: ajouter une nouvelle requête pour le `EmployeesTableAdapter` ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Choisir d’ajouter une instruction SQL qui retourne des lignes. Quand vous atteignez la spécifier un `SELECT` instruction écran la valeur par défaut `SELECT` instruction pour la `EmployeesTableAdapter` sera déjà chargé. Ajoutez simplement dans le `WHERE` clause : `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) est une fonction T-SQL qui retourne une partie de date particulière d’un `datetime` type ; dans ce cas, nous utilisons `DATEPART` pour retourner le mois de la `HireDate` colonne.


[![Retour uniquement les lignes où la HireDate colonne est inférieure ou égale à la @HiredBeforeDate paramètre](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figure 3**: retourner uniquement les lignes où la `HireDate` colonne est inférieure ou égale à la `@HiredBeforeDate` paramètre ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Enfin, changez le `FillBy` et `GetDataBy` pour les noms de méthode `FillByHiredDateMonth` et `GetEmployeesByHiredDateMonth`, respectivement.


[![Choisissez des noms de méthode plus appropriées que FillBy et GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figure 4**: choisissez plus approprié méthode noms que `FillBy` et `GetDataBy` ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Cliquez sur Terminer pour terminer l’Assistant et revenir à l’aire de conception du jeu de données. Le `EmployeesTableAdapter` doit inclure désormais un nouvel ensemble de méthodes pour accéder à des employés embauchés dans un mois spécifié.


[![Les nouvelles méthodes s’affichent dans l’aire de conception du jeu de données](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figure 5**: les nouvelles méthodes s’affichent dans l’aire de conception du jeu de données ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Étape 2 : Ajout de la`GetEmployeesByHiredDateMonth(month)`à la couche de logique métier (méthode)

Étant donné que notre application architecture utilise distinct de couche pour la logique métier et les données d’accès logique, nous devons ajouter une méthode à notre BLL que les appels à la couche DAL pour récupérer les employés engagés avant une date spécifiée. Ouvrez le `EmployeesBLL.vb` fichier, puis ajoutez la méthode suivante :


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Comme avec nos autres méthodes dans cette classe, `GetEmployeesByHiredDateMonth(month)` appelle simplement vers le bas dans la couche DAL et retourne les résultats.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Étape 3 : Affichage des employés dont embauche anniversaire est ce mois-ci

Notre dernière étape pour cet exemple consiste à afficher les employés dont embauche anniversaire est ce mois-ci. Commencez par ajouter un contrôle GridView à la `ProgrammaticParams.aspx` page dans le `BasicReporting` dossier et ajoutez un nouveau ObjectDataSource comme source de données. Configurer l’ObjectDataSource à utiliser le `EmployeesBLL` classe avec le `SelectMethod` défini sur `GetEmployeesByHiredDateMonth(month)`.


[![Utilisez la classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figure 6**: utilisez le `EmployeesBLL` classe ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Sélectionnez les GetEmployeesByHiredDateMonth(month) à partir de la méthode](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figure 7**: Select From le `GetEmployeesByHiredDateMonth(month)` (méthode) ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Le dernier écran nous demande de fournir le `month` source de la valeur du paramètre. Étant donné que nous allons définir cette valeur par programmation, laissez le paramètre source la valeur par défaut aucune option et cliquez sur Terminer.


[![Conservez le paramètre Source None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figure 8**: laissez le paramètre Source définie sur None ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Cela créera un `Parameter` objet dans l’ObjectDataSource `SelectParameters` collection qui n’a pas une valeur spécifiée.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Pour définir cette valeur par programmation, nous devons créer un gestionnaire d’événements pour l’ObjectDataSource `Selecting` événement. Pour ce faire, accédez à la vue conception, puis double-cliquez sur ObjectDataSource. Vous pouvez également sélectionnez ObjectDataSource, accédez à la fenêtre Propriétés, cliquez sur l’icône d’éclair. Ensuite, double-cliquez sur dans la zone de texte à côté du `Selecting` événement ou tapez le nom de gestionnaire d’événements que vous souhaitez utiliser. Une troisième option, vous pouvez créer le Gestionnaire d’événements en sélectionnant ObjectDataSource et son `Selecting` événement dans les deux listes de liste déroulante en haut de la classe code-behind de la page.


![Cliquez sur l’icône d’éclair dans la fenêtre Propriétés pour répertorier les événements d’un contrôle Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figure 9**: cliquez sur l’icône d’éclair dans la fenêtre Propriétés pour répertorier les événements d’un contrôle Web


Tous les trois approches ajouter un nouveau gestionnaire d’événements pour l’ObjectDataSource `Selecting` événement à la classe code-behind de la page. Dans ce gestionnaire d’événements, nous pouvons lire et écrire dans les valeurs de paramètre à l’aide de `e.InputParameters(parameterName)`, où *`parameterName`* est la valeur de la `Name` d’attribut dans le `<asp:Parameter>` balise (le `InputParameters` collection peut également être indexée ordinale, comme dans `e.InputParameters(index)`). Pour définir le `month` paramètre pour le mois en cours, ajoutez le code suivant à la `Selecting` Gestionnaire d’événements :


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Lorsque vous visitez cette page via un navigateur, nous pouvons voir que seul un employé a été embauché ce mois-ci (mars) Laura Callahan, qui travaille chez l’entreprise depuis 1994.


[![Les employés dont les anniversaires ce mois-ci sont affichés.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figure 10**: ceux employés dont anniversaires ce mois sont affichés ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Récapitulatif

Valeurs des paramètres de l’ObjectDataSource peuvent en général être définies de façon déclarative, sans nécessiter une ligne de code, il est facile de définir les valeurs de paramètre par programmation. Il nous suffit est de créer un gestionnaire d’événements pour l’ObjectDataSource `Selecting` événement qui se déclenche avant que la méthode de l’objet sous-jacent est appelée et définir manuellement les valeurs pour un ou plusieurs paramètres via le `InputParameters` collection.

Ce didacticiel conclut la section rapports de base. Le [didacticiel suivant](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) lance la section de filtrage et les scénarios maître / détails, dans laquelle nous examinons les techniques permettant d’attribuer le visiteur pour filtrer les données et Explorer à partir d’un rapport principal dans un rapport détaillé.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](declarative-parameters-vb.md)
