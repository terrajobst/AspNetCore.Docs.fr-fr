---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Gestion des Exceptions de niveau BLL et DAL dans une Page ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous verrons comment afficher un message d’erreur convivial et éducative doit se faire une exception pendant une insertion, mise à jour ou opération de suppression de...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 86b8bb00e83f311d311a51a747086356833a8c93
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829532"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Gestion des Exceptions de niveau BLL et DAL dans une Page ASP.NET (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) ou [télécharger le PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> Dans ce didacticiel, nous verrons comment afficher un message d’erreur convivial et éducative doit se produire une exception lors d’une insertion, mise à jour ou opération de suppression d’un contrôle Web de données ASP.NET.


## <a name="introduction"></a>Introduction

Utilisation des données à partir d’une application de web ASP.NET à l’aide d’une architecture à plusieurs niveaux d’application implique les trois étapes générales suivantes :

1. Déterminer quelle méthode de la couche de logique métier doit être appelé et le paramètre des valeurs pour la passer. Les valeurs de paramètre peuvent être codée en dur, affecté par programmation ou entrées entré par l’utilisateur.
2. Appelez la méthode.
3. Traiter les résultats. Lorsque vous appelez une méthode de la couche de logique métier qui retourne des données, cela peut impliquer la liaison des données à un contrôle Web de données. Pour les méthodes BLL qui modifient les données, cela peut inclure l’exécution de certaines actions selon une valeur de retour ou normalement gère toute exception qui a été créée à l’étape 2.

Comme nous l’avons vu dans la [didacticiel précédent](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), ObjectDataSource et les données des contrôles Web fournissent des points d’extensibilité pour les étapes 1 et 3. Le contrôle GridView, par exemple, déclenche son `RowUpdating` événement avant de pouvoir affecter les valeurs de champ à son ObjectDataSource `UpdateParameters` collection ; son `RowUpdated` événement est déclenché après l’ObjectDataSource ait terminé l’opération.

Nous avons déjà examiné les événements qui se déclenchent au cours de l’étape 1 et l’ont vu comment ils peuvent être utilisés pour personnaliser les paramètres d’entrée ou d’annuler l’opération. Dans ce didacticiel, nous allons porter notre attention vers les événements qui se déclenchent une fois l’opération terminée. Avec ces gestionnaires d’événements post-niveau, entre autres choses, nous pouvons déterminer si une exception s’est produite lors de l’opération et gérer de façon appropriée, afficher un message d’erreur convivial et éducative sur l’écran plutôt que par défaut ASP.NET standard page d’exception.

Pour illustrer l’utilisation de ces événements post-niveau, nous allons créer une page qui répertorie les produits dans un GridView modifiable. Lorsque la mise à jour un produit, si une exception est levée notre ASP.NET page affiche un bref message au-dessus de la GridView expliquant qu’un problème s’est produite. C’est parti !

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Étape 1 : Création d’un GridView modifiable des produits

Dans le didacticiel précédent, nous avons créé un GridView modifiable avec seulement deux champs, `ProductName` et `UnitPrice`. Cela requis de la création d’une surcharge supplémentaire pour le `ProductsBLL` la classe `UpdateProduct` (méthode), qui a accepté uniquement les trois paramètres d’entrée (nom du produit, le prix unitaire et ID) par opposition un paramètre pour chaque champ de produit. Pour ce didacticiel, nous allons exercerez à cette technique à nouveau, création d’un GridView modifiable qui affiche le nom du produit, quantité par unité, le prix unitaire et des unités en stock, mais autorise uniquement le nom, le prix unitaire et des unités en stock à modifier.

Pour prendre en charge ce scénario, nous aurons besoin une autre surcharge de la `UpdateProduct` méthode, un constructeur qui accepte quatre paramètres : le nom du produit, les prix unitaire, les unités en stock et de code. Ajoutez la méthode suivante à la `ProductsBLL` classe :


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Avec cette méthode est terminée, nous sommes prêts à créer la page ASP.NET qui permet la modification de ces quatre champs de produit spécifique de. Ouvrez le `ErrorHandling.aspx` page dans le `EditInsertDelete` dossier et ajoutez un GridView à la page via le concepteur. Lier le contrôle GridView à une nouvelle ObjectDataSource, mappage le `Select()` méthode à la `ProductsBLL` la classe `GetProducts()` (méthode) et le `Update()` méthode à la `UpdateProduct` surcharge venez de créer.


[![Utilisez la surcharge de méthode UpdateProduct qui accepte quatre paramètres d’entrée](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Figure 1**: utilisez le `UpdateProduct` méthode de surcharge qu’accepte quatre paramètres d’entrée ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


Cela créera un ObjectDataSource avec une `UpdateParameters` collection avec quatre paramètres et un GridView avec un champ pour chacun des champs de produit. Balisage déclaratif de l’ObjectDataSource affecte le `OldValuesParameterFormatString` la valeur de propriété `original_{0}`, ce qui provoquera une exception dans la mesure où notre classe BLL n’espérez pas un paramètre d’entrée nommé `original_productID` devant être transmis dans. N’oubliez pas de supprimer ce paramètre complètement à partir de la syntaxe déclarative (ou affectez-lui la valeur par défaut, `{0}`).

Ensuite, alléger le contrôle GridView à inclure uniquement les `ProductName`, `QuantityPerUnit`, `UnitPrice`, et `UnitsInStock` BoundFields. Également n’hésitez pas à appliquer au niveau du champ mise en forme vous le jugez nécessaire (par exemple en modifiant la `HeaderText` propriétés).

Dans le didacticiel précédent que nous avons vu comment mettre en forme le `UnitPrice` BoundField sous forme de devise à la fois en mode lecture seule et en mode édition. Nous allons suivre le même ici. Rappelez-vous que ce paramètre obligatoire la BoundField `DataFormatString` propriété `{0:c}`, ses `HtmlEncode` propriété `false`et son `ApplyFormatInEditMode` à `true`, comme illustré dans la Figure 2.


[![Configurer le UnitPrice BoundField à afficher sous forme de devise](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Figure 2**: configurer le `UnitPrice` BoundField à afficher sous forme de devise ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


Mise en forme le `UnitPrice` comme une devise dans l’interface de modification nécessite la création d’un gestionnaire d’événements pour le contrôle GridView `RowUpdating` événement qui analyse la chaîne au format monétaire dans un `decimal` valeur. N’oubliez pas que le `RowUpdating` Gestionnaire d’événements à partir du dernier didacticiel également vérifiés pour s’assurer que l’utilisateur fourni un `UnitPrice` valeur. Toutefois, pour ce didacticiel, nous allons autoriser l’utilisateur à omettre le prix.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Notre GridView inclut un `QuantityPerUnit` BoundField, mais cette BoundField doit être uniquement à des fins d’affichage et ne doit pas être modifiées par l’utilisateur. Pour ce faire, il suffit de définir 'BoundFields `ReadOnly` propriété `true`.


[![Rendre le QuantityPerUnit BoundField en lecture seule](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Figure 3**: rendre le `QuantityPerUnit` BoundField en lecture seule ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


Enfin, vérifiez la case à cocher Activer la modification à partir de la balise active le contrôle GridView. Après avoir effectué ces étapes le `ErrorHandling.aspx` le Concepteur de la page doit ressembler à la Figure 4.


[![Supprimer tous sauf le nécessaires BoundFields et vérifiez l’activer la case à cocher de modification](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Figure 4**: supprimer tout sauf le nécessaire BoundFields et cocher la case Activer la modification ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


À ce stade nous disposons d’une liste de tous les produits `ProductName`, `QuantityPerUnit`, `UnitPrice`, et `UnitsInStock` champs ; Toutefois, uniquement le `ProductName`, `UnitPrice`, et `UnitsInStock` champs peuvent être modifiés.


[![Les utilisateurs peuvent désormais facilement modifier les noms des produits, les prix et les unités en Stock champs](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Figure 5**: les noms les utilisateurs peuvent désormais facilement modifier les produits, les prix et les unités en Stock champs ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Étape 2 : Gérer correctement les Exceptions au niveau de la couche DAL

Bien que notre GridView modifiable fonctionne parfaitement lorsque les utilisateurs entrent des valeurs autorisées pour le nom du produit modifié, les prix et les unités en stock, entrer des valeurs illégales génère une exception. Par exemple, si vous omettez le `ProductName` valeur entraîne une [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) levée depuis le `ProductName` propriété dans le `ProdcutsRow` classe a son `AllowDBNull` propriété définie sur `false`; si le base de données est en panne, un `SqlException` seront levées par le TableAdapter lorsque vous tentez de vous connecter à la base de données. Sans effectuer aucune action, ces exceptions se propagent de la couche d’accès aux données sur la couche de logique métier, puis sur la page ASP.NET et enfin à l’exécution d’ASP.NET.

En fonction de la configuration de votre application web et ou non que vous visitez l’application à partir de `localhost`, une exception non gérée peut entraîner une page d’erreur de serveur générique, un rapport d’erreur détaillé ou une page web conviviale. Consultez [Web Application Error Handling in ASP.NET](http://www.15seconds.com/issue/030102.htm) et [customErrors, élément](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) pour plus d’informations sur la façon dont le runtime ASP.NET répond à une exception non interceptée.

La figure 6 illustre l’écran rencontrée lorsque vous tentez de mettre à jour d’un produit sans spécifier le `ProductName` valeur. Ceci est la valeur par défaut de rapport d’erreurs détaillées affiché lorsque transitant par `localhost`.


[![En omettant les détails de l’Exception affichage nom du produit](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Figure 6**: l’omission nom sera affichage Détails du produit de l’Exception ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


Si ces détails de l’exception sont utiles lorsque vous testez une application, présentant un utilisateur final de cet écran face à une exception est loin d’être idéal. Un utilisateur final probable ne sait pas qu’un `NoNullAllowedException` est ou pourquoi elle a été générée. Une meilleure approche consiste à présenter à l’utilisateur avec un message plus convivial expliquant que pose tente de mettre à jour le produit.

Si une exception se produit lors de l’exécution de l’opération, les événements post-niveau dans ObjectDataSource et le contrôle Web de données fournissent un moyen de détecter et d’annuler l’exception de se propager à l’exécution d’ASP.NET. Dans notre exemple, nous allons créer un gestionnaire d’événements pour le contrôle GridView `RowUpdated` événement qui détermine si une exception a été déclenchée et, dans ce cas, affiche les détails d’exception dans un contrôle Web Label.

Commencez par ajouter une étiquette à la page ASP.NET, en définissant son `ID` propriété `ExceptionDetails` et éliminant son `Text` propriété. Pour dessiner les yeux de l’utilisateur à ce message, définissez son `CssClass` propriété `Warning`, qui est une classe CSS que nous avons ajouté à la `Styles.css` fichier dans le didacticiel précédent. Rappelez-vous que cette classe CSS provoque le texte du libellé à afficher dans une police rouge, italique, gras, très grande taille.


[![Ajouter un contrôle Web d’étiquette à la Page](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Figure 7**: ajouter un contrôle Web d’étiquette à la Page ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


Dans la mesure où nous voulons cet étiquette Web contrôle soit visible uniquement immédiatement après une exception s’est produite, définissez son `Visible` propriété sur false dans le `Page_Load` Gestionnaire d’événements :


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Avec ce code, sur la première visite de page et les publications ultérieures le `ExceptionDetails` contrôle aura son `Visible` propriété définie sur `false`. Face à une exception au niveau de la couche DAL ou de la couche BLL, qui nous pouvons détecter dans le GridView `RowUpdated` Gestionnaire d’événements, nous allons définir le `ExceptionDetails` du contrôle `Visible` true à la propriété. Étant donné que les gestionnaires d’événements de contrôle Web se produisent après le `Page_Load` Gestionnaire d’événements dans le cycle de vie de page, l’étiquette s’affiche. Toutefois, sur la publication suivante, le `Page_Load` Gestionnaire d’événements est rétablis la `Visible` propriété retour au `false`, masquer à nouveau à partir de la vue.

> [!NOTE]
> Ou bien, nous pouvons supprimer la nécessité pour le paramètre le `ExceptionDetails` du contrôle `Visible` propriété dans `Page_Load` , affectez son `Visible` propriété `false` dans la syntaxe déclarative et la désactivation de son état d’affichage (définissant son `EnableViewState` propriété `false`). Nous allons utiliser cette approche alternative dans un futur didacticiel.


Avec le contrôle d’étiquette ajouté, l’étape suivante consiste à créer le Gestionnaire d’événements pour le contrôle GridView `RowUpdated` événement. Sélectionnez le contrôle GridView dans le concepteur, accédez à la fenêtre Propriétés, cliquez sur l’icône d’éclair, répertoriant les événements du contrôle GridView. Il doit déjà être il une entrée pour le contrôle GridView `RowUpdating` événement, comme nous avons créé un gestionnaire d’événements pour cet événement précédemment dans ce didacticiel. Créer un gestionnaire d’événements pour le `RowUpdated` également des événements.


![Créer un gestionnaire d’événements pour RowUpdated (événement le contrôle GridView)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Figure 8**: créer un gestionnaire d’événements pour le contrôle GridView `RowUpdated` événement


> [!NOTE]
> Vous pouvez également créer le Gestionnaire d’événements via les listes de liste déroulante en haut du fichier de classe code-behind. Sélectionnez le contrôle GridView dans la liste déroulante sur la gauche et le `RowUpdated` événement de celui sur la droite.


Création de ce gestionnaire d’événements ajoute le code suivant à la classe de code-behind de la page ASP.NET :


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Second paramètre de ce gestionnaire d’événements est un objet de type [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), qui a trois propriétés dignes d’intérêt pour la gestion des exceptions :

- `Exception` une référence à l’exception levée ; Si aucune exception n’a été levée, cette propriété a la valeur de `null`
- `ExceptionHandled` valeur booléenne qui indique si l’exception a été gérée dans le `RowUpdated` Gestionnaire d’événements ; si `false` (la valeur par défaut), l’exception est à nouveau levée percolation jusqu'à l’exécution d’ASP.NET
- `KeepInEditMode` Si la valeur `true` la ligne GridView modifiée reste en mode édition ; si `false` (la valeur par défaut), la ligne GridView revient à son mode de lecture seule

Notre code, puis, vérifiez si `Exception` n’est pas `null`, ce qui signifie qu’une exception a été levée pendant l’opération. Si c’est le cas, nous souhaitons :

- Affichez un message convivial dans le `ExceptionDetails` étiquette
- Indiquer que l’exception a été gérée.
- Conserver la ligne GridView en mode édition

Ce code suivant effectue ces objectifs :


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Ce gestionnaire d’événements commence par vérifier a posteriori si `e.Exception` est `null`. Le cas contraire, le `ExceptionDetails` l’étiquette `Visible` propriété est définie sur `true` et son `Text` propriété à « Problème est survenu un mise à jour du produit. » Les détails de l’exception réelle qui a été levée résident dans le `e.Exception` l’objet `InnerException` propriété. Cette exception interne est vérifiée et, s’il s’agit d’un type particulier, un message supplémentaire, utile est ajouté à la `ExceptionDetails` l’étiquette `Text` propriété. Enfin, le `ExceptionHandled` et `KeepInEditMode` propriétés sont toutes deux définies sur `true`.

La figure 9 illustre une capture d’écran de cette page lors de l’omission du nom du produit ; Figure 10 montre les résultats lorsque vous entrez une instruction non conforme `UnitPrice` valeur (-50).


[![Le ProductName BoundField doit contenir une valeur](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Figure 9**: le `ProductName` BoundField doit contenir une valeur ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![Valeurs UnitPrice négatives sont interdites](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Figure 10**: négatif `UnitPrice` les valeurs sont pas autorisés ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


En définissant le `e.ExceptionHandled` propriété `true`, le `RowUpdated` Gestionnaire d’événements a indiqué qu’il a traité l’exception. Par conséquent, l’exception ne se propage dans le runtime ASP.NET.

> [!NOTE]
> Les figures 9 et 10 montrent une méthode élégante de gérer les exceptions déclenchées en raison de l’entrée utilisateur non valide. Dans l’idéal, cependant, ce type d’entrée non valide ne peut pas atteindre la couche de logique métier en premier lieu, comme la page ASP.NET doit garantir que les entrées de l’utilisateur sont valides avant d’appeler le `ProductsBLL` la classe `UpdateProduct` (méthode). Dans notre didacticiel suivant, que nous verrons comment ajouter des contrôles de validation aux interfaces d’éditions et insertion pour vous assurer que les données soumises à la couche de logique métier est conforme aux règles d’entreprise. Les contrôles de validation n'empêchent pas seulement l’appel de la `UpdateProduct` méthode jusqu'à ce que les données fournies par l’utilisateur sont valides, mais également fournissent une expérience utilisateur plus informative pour identifier les problèmes de saisie de données.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Étape 3 : Normalement de gestion des Exceptions de niveau BLL

Lors de l’insertion, la mise à jour ou supprimer des données, la couche Data Access peut lever une exception en cas d’une erreur liée aux données. La base de données est peut-être hors connexion, une colonne de table de base de données requis aviez ne peut-être pas une valeur spécifiée ou une contrainte de niveau table a été violée. En plus des exceptions strictement liées aux données, la couche de logique métier pouvez utiliser des exceptions pour indiquer les cas de violation de règles d’entreprise. Dans le [création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-vb.md) (didacticiel), par exemple, nous avons ajouté une vérification de la règle métier à l’original `UpdateProduct` de surcharge. Plus précisément, si l’utilisateur a été marquer un produit comme abandonnés, nous nécessaire que le produit doit ne pas être le seul fourni par son fournisseur. Si cette condition a été enfreinte, un `ApplicationException` a été levée.

Pour le `UpdateProduct` surcharge créée dans ce didacticiel, nous allons ajouter une règle d’entreprise qui interdit le `UnitPrice` champ d’être définie à une nouvelle valeur est plus de deux fois la version d’origine `UnitPrice` valeur. Pour ce faire, vous devez ajuster le `UpdateProduct` surcharge afin qu’il effectue cette vérification et lève un `ApplicationException` si la règle est violée. La méthode de mise à jour suivante :


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Avec cette modification, toute mise à jour de prix est plus de deux fois le prix existant entraîne un `ApplicationException` levée. Tout comme l’exception levée à partir de la couche DAL, ce BLL-déclenché `ApplicationException` peuvent être détectés et gérés dans le GridView `RowUpdated` Gestionnaire d’événements. En fait, le `RowUpdated` code du Gestionnaire d’événements, comme indiqué, correctement détecte cette exception et affiche le `ApplicationException`de `Message` valeur de propriété. La figure 11 illustre une capture lorsqu’un utilisateur tente de mettre à jour le prix de Chai vers 50,00 $, qui est supérieure à deux fois son prix actuel de 19,95 $.


[![Les règles d’entreprise interdisent les augmentations de prix que plus de doubler les prix d’un produit](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Figure 11**: les augmentations de prix interdire que plus de doubler les prix d’un produit des règles d’entreprise ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> Dans l’idéal seraient refactorisés nos règles de logique métier hors du `UpdateProduct` surcharges de méthode et dans une méthode courante. Cela est considérée comme un exercice pour le lecteur.


## <a name="summary"></a>Récapitulatif

Pendant l’insertion, la mise à jour et la suppression des opérations, le contrôle Web de données et de l’ObjectDataSource impliqués déclenchent des événements préalables- et post-niveau ce serre-livres l’opération proprement dite. Comme nous l’avons vu dans ce didacticiel et le précédent, lorsque vous travaillez avec un GridView modifiable le GridView `RowUpdating` se déclenche l’événement, suivie de l’ObjectDataSource `Updating` événement, le moment où la commande de mise à jour est effectuée à l’ObjectDataSource objet sous-jacent. Une fois l’opération terminée, l’ObjectDataSource `Updated` se déclenche l’événement, suivie de la GridView `RowUpdated` événement.

Nous pouvons créer des gestionnaires d’événements pour les événements de niveau préalable afin de personnaliser les paramètres d’entrée ou pour les événements post-niveau afin d’inspecter et de répondre aux résultats de l’opération. Gestionnaires d’événements post-niveau sont plus couramment utilisés pour détecter si une exception s’est produite pendant l’opération. Face à une exception, ces gestionnaires d’événements post-niveau peuvent éventuellement gérer l’exception sur leurs propres. Dans ce didacticiel, nous avons vu comment gérer cette exception en affichant un message d’erreur convivial.

Dans le didacticiel suivant, nous allons voir comment réduire la probabilité d’exceptions résultant de problèmes de mise en forme des données (par exemple, entrez une valeur négative `UnitPrice`). Plus précisément, nous allons examiner comment ajouter des contrôles de validation aux interfaces d’éditions et insertion.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Liz Shulok. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Suivant](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
