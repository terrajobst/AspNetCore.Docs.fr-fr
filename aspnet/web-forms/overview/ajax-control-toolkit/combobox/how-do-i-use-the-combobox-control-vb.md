---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Comment utiliser le contrôle de zone de liste déroulante ? (VB) | Microsoft Docs
author: microsoft
description: Zone de liste déroulante est un contrôle ASP.NET AJAX qui combine la souplesse d’une zone de texte avec une liste d’options à partir de laquelle les utilisateurs peuvent choisir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ec00a58581f36f87ecdca2fbd96fcea645f75f48
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829284"
---
<a name="how-do-i-use-the-combobox-control-vb"></a>Comment utiliser le contrôle de zone de liste déroulante ? (VB)
====================
par [Microsoft](https://github.com/microsoft)

> Zone de liste déroulante est un contrôle ASP.NET AJAX qui combine la souplesse d’une zone de texte avec une liste d’options à partir de laquelle les utilisateurs peuvent choisir.


L’objectif de ce didacticiel est d’expliquer le contrôle ComboBox de boîte à outils de contrôle AJAX. La zone de liste déroulante fonctionne comme une combinaison entre un contrôle DropDownList de ASP.NET standard et un contrôle de zone de texte. Vous pouvez sélectionner dans une liste déjà existante d’éléments ou entrez un nouvel élément.

La zone de liste déroulante est similaire à l’extendeur de contrôle de la saisie semi-automatique, mais les contrôles sont utilisés dans des scénarios différents. L’extendeur AutoComplete interroge un service web pour obtenir des entrées correspondantes. Le contrôle de zone de liste déroulante, en revanche, est initialisé avec un ensemble d’éléments. À l’aide de la rend extendeur AutoComplete sens lorsque vous travaillez avec un grand nombre de données (des millions de pièces de voiture) tout en utilisant le contrôle de zone de liste déroulante de sens lorsque vous travaillez avec un petit ensemble de données (des dizaines de parties de la voiture).

## <a name="selecting-from-a-static-list-of-items"></a>Sélection d’une liste statique d’éléments

Laisser s démarrer avec un exemple simple d’utilisation du contrôle de zone de liste déroulante. Imaginez que vous souhaitez afficher une liste statique d’éléments dans une liste déroulante. Toutefois, vous souhaitez laissez ouverte la possibilité que la liste n’est pas terminée. Vous souhaitez autoriser un utilisateur à entrer une valeur personnalisée dans la liste.

Nous ll créer une page Web Forms ASP.NET et utilisez le contrôle de zone de liste déroulante de la page. Ajouter la nouvelle page ASP.NET à votre projet et basculez en mode Design.

Si vous souhaitez utiliser le contrôle de zone de liste déroulante dans la page vous devez ajouter un contrôle ScriptManager à la page. Faites glisser le contrôle ScriptManager sous l’onglet Extensions AJAX sur l’aire du concepteur. Vous devez ajouter le contrôle ScriptManager en haut de la page ; Vous pouvez l’ajouter immédiatement au-dessous de l’ouverture côté serveur &lt;formulaire&gt; balise.

Ensuite, faites glisser le contrôle de zone de liste déroulante sur la page. Vous trouverez le contrôle de zone de liste déroulante dans la boîte à outils avec les autres contrôles AJAX Control Toolkit et extendeurs de contrôle (voir figure 1).


[![Formulaire simple pour la création d’une carte de visite](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Figure 01**: sélection du contrôle de zone de liste déroulante à partir de la boîte à outils ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image2.png))


Nous ll utiliser le contrôle de zone de liste déroulante pour afficher une liste statique de choix. L’utilisateur peut sélectionner un niveau particulier de spiciness pour leurs produits alimentaires à partir d’une liste de trois choix : violence, support et à chaud (voir Figure 2).


[![Sélection d’une liste statique d’éléments](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Figure 02**: sélection d’une liste statique d’éléments ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image4.png))


Il existe deux façons que vous pouvez ajouter ces choix pour le contrôle de zone de liste déroulante. Tout d’abord, vous sélectionnez l’option de tâche de modifier les Options lorsque vous pointez votre souris sur le contrôle en mode Design et que vous ouvrez l’éditeur d’élément (voir Figure 3).


[![Modification d’éléments de la zone de liste déroulante](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Figure 03**: éléments de zone de liste déroulante modification ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image6.png))


La deuxième option consiste à ajouter la liste des éléments entre les balises &lt;asp : zone de liste déroulante&gt; balises en mode Source. La page dans le Listing 1 contient la zone de liste déroulante mis à jour comportant la liste d’éléments.

**Liste 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Lorsque vous ouvrez la page dans le Listing 1, vous pouvez sélectionner une des options existantes à partir de la zone de liste déroulante. En d’autres termes, la zone de liste déroulante fonctionne exactement comme un contrôle DropDownList.

Toutefois, vous avez également la possibilité d’entrer un nouveau choix (par exemple, Super corsée) qui n’est pas dans la liste existante. Par conséquent, la zone de liste déroulante fonctionne également comme un contrôle de zone de texte.

Indépendamment de si vous choisissez un préexistants élément ou que vous entrez un élément personnalisé, lorsque vous envoyez le formulaire, votre choix s’affiche dans le contrôle d’étiquette. Lorsque vous envoyez le formulaire, le niveau de btnSubmit\_cliquez sur Gestionnaire s’exécute et met à jour de l’étiquette (voir Figure 4).


[![Affichage de l’élément sélectionné](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Figure 04**: affichage de l’élément sélectionné ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image8.png))


La zone de liste déroulante prend en charge les mêmes propriétés que le contrôle DropDownList pour la récupération de l’élément sélectionné après l’envoi d’un formulaire :

- SelectedItem.Text - affiche la valeur de la propriété Text de l’élément sélectionné.
- SelectedItem.Value - affiche la valeur de la propriété Value de l’élément sélectionné ou affiche le texte tapé dans la zone de liste déroulante.
- SelectedValue - même en tant que SelectedItem.Value, à ceci près que cette propriété vous permet de spécifier l’élément sélectionné (initiale) par défaut.

Si vous tapez un choix personnalisé dans la zone de liste déroulante puis le choix personnalisé est affecté aux propriétés SelectedItem.Text et de SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>En sélectionnant la liste des éléments à partir de la base de données

Vous pouvez récupérer la liste des éléments qui affiche la zone de liste déroulante à partir d’une base de données. Par exemple, vous pouvez lier la zone de liste déroulante pour un contrôle SqlDataSource, un contrôle ObjectDataSource, un LinqDataSource ou un contrôle EntityDataSource.

Imaginez que vous souhaitez afficher une liste de films dans une zone de liste déroulante. Vous souhaitez récupérer la liste de films à partir de la table de base de données de films. Procédez comme suit :

1. Créez une page nommée Movies.aspx
2. Ajouter un contrôle ScriptManager à la page en faisant glisser le ScriptManager sous l’onglet Extensions AJAX dans la boîte à outils vers la page.
3. Ajouter un contrôle de zone de liste déroulante à la page en faisant glisser de la zone de liste déroulante sur la page.
4. En mode conception, pointez votre souris sur le contrôle de zone de liste déroulante et sélectionnez le **choisir la Source de données** option de tâche (voir Figure 5). L’Assistant de Configuration de Source de données est lancé.
5. Dans le **choisir une Source de données** étape, sélectionnez le &lt;nouvelle source de données&gt; option.
6. Dans le **choisir un Type de Source de données** étape, sélectionnez la base de données.
7. Dans le **choisir votre connexion de données** étape, sélectionnez votre base de données (par exemple, MoviesDB.mdf).
8. Dans le **enregistrer la chaîne de connexion au fichier de Configuration de l’Application** étape, sélectionnez l’option pour enregistrer votre chaîne de connexion.
9. Dans le **configurer l’instruction Select** étape, sélectionnez la table de base de données de films et sélectionnez toutes les colonnes.
10. Dans le **tester la requête** étape, cliquez sur le bouton Terminer.
11. Dans le **choisir la Source de données** étape, sélectionnez la colonne de titre pour le champ à afficher et de la colonne Id pour les données de champ (voir Figure).
12. Cliquez sur le bouton OK pour fermer l’Assistant.


[![Choix d’une source de données](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Figure 05**: choix d’une source de données ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image10.png))


[![Choisir les champs de texte et la valeur de données](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Figure 06**: choisir les champs de texte et la valeur de données ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image12.png))


Après avoir terminé les étapes ci-dessus, la zone de liste déroulante est liée à un contrôle SqlDataSource qui représente les films à partir de la table de base de données de films. La source de la page ressemble à la liste 2 (j’ai nettoyé un peu la mise en forme).

**Listing 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Notez que le contrôle de zone de liste déroulante a une propriété DataSourceID qui pointe vers le contrôle SqlDataSource. Lorsque vous ouvrez la page dans un navigateur, la liste de films à partir de la base de données s’affiche (voir la Figure 7). Vous pouvez un choix un film à partir de la liste ou entrez un nouveau film en tapant le film dans la zone de liste déroulante.


[![Affiche une liste de films](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Figure 07**: affichage d’une liste de films ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Définissant le DropDownStyle

Vous pouvez utiliser la propriété ComboBox DropDownStyle pour modifier le comportement de la zone de liste déroulante. Cette propriété accepte il les valeurs possibles :

- Liste déroulante - affiche de la zone de liste déroulante (valeur par défaut) une liste déroulante liste lorsque vous cliquez sur la flèche et vous pouvez entrer une valeur personnalisée.
- Simple - la zone de liste déroulante s’affiche automatiquement une liste déroulante et vous pouvez entrer une valeur personnalisée.
- DropDownList - la zone de liste déroulante fonctionne exactement comme un contrôle DropDownList.

Les différentes entre la liste déroulante et Simple sont lorsque la liste des éléments est affichée. Dans le cas Simple, la liste s’affiche immédiatement lorsque vous déplacez le focus vers la zone de liste déroulante. Dans le cas de liste déroulante, vous devez cliquer sur la flèche pour afficher la liste des éléments.

La valeur de DropDownList, le contrôle de zone de liste déroulante de fonctionner comme un contrôle DropDownList standard. Toutefois, il existe une importante différence ici. Les versions antérieures d’Internet Explorer affichent un contrôle DropDownList avec un z-index infini pour le contrôle s’affiche devant n’importe quel contrôle placé devant elle. Étant donné que le contrôle ComboBox effectue le rendu HTML &lt;div&gt; balise au lieu d’un élément HTML &lt;sélectionnez&gt; balise, le contrôle ComboBox correctement respecte positionnement z.

## <a name="setting-the-autocompletemode"></a>Définissant le AutoCompleteMode

La propriété ComboBox AutoCompleteMode vous permet de spécifier que se passe-t-il quand un utilisateur tape du texte dans la zone de liste déroulante. Cette propriété accepte les valeurs possibles suivantes :

- None : (valeur par défaut) la zone de liste déroulante ne fournit pas de comportement de saisie semi-automatique.
- Suggérer - la zone de liste déroulante affiche la liste et il met en surbrillance l’élément correspondant dans la liste (voir Figure 8).
- Append : le contrôle ComboBox n’affiche pas la liste, et il ajoute l’élément correspondant dans la liste sur ce que vous avez tapé (voir Figure 9).
- À la fois SuggestAppend - la zone de liste déroulante affiche la liste et ajoute l’élément correspondant dans la liste sur ce que vous avez tapé (voir Figure 10).


[![La zone de liste déroulante affiche une suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Figure 08**: la zone de liste déroulante affiche une suggestion ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image16.png))


[![Zone de liste déroulante ajoute du texte correspondant](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Figure 09**: zone de liste déroulante ajoute du texte correspondant ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image18.png))


[![La zone de liste déroulante suggère et ajoute](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Figure 10**: la zone de liste déroulante suggère et ajoute ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image20.png))


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à utiliser le contrôle de zone de liste déroulante pour afficher un ensemble fixe d’éléments. Nous lié le contrôle de zone de liste déroulante à la fois à un ensemble d’éléments de statique et à une table de base de données. Enfin, vous avez appris à modifier le comportement de la zone de liste déroulante en définissant ses propriétés DropDownStyle et AutoCompleteMode.

> [!div class="step-by-step"]
> [Précédent](how-do-i-use-the-combobox-control-cs.md)
