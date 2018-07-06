---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: La mise en cache des données avec ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: La mise en cache peut signifier la différence entre une lente et une application Web rapide. Ce didacticiel est le premier des quatre examine en détail à la mise en cache dans ASP.NET...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: aa2af934a45ebd7e23d5d2ccf5a80f4949f1ec4f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817536"
---
<a name="caching-data-with-the-objectdatasource-vb"></a>La mise en cache des données avec ObjectDataSource (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe) ou [télécharger le PDF](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> La mise en cache peut signifier la différence entre une lente et une application Web rapide. Ce didacticiel est le premier des quatre examine en détail à la mise en cache dans ASP.NET. Découvrez les concepts clés de la mise en cache et comment appliquer la mise en cache à la couche de présentation via le contrôle ObjectDataSource.


## <a name="introduction"></a>Introduction

En informatique, *la mise en cache* est le processus consistant à prendre des données ou des informations sont coûteuses d’obtenir et stocker une copie de celui-ci dans un emplacement qui est plus rapide d’accès. Pour les applications orientées données, requêtes volumineuses et complexes consomment généralement la majorité de la durée d’exécution de l’application s. Tel un s performances de l’application, puis, peuvent souvent être améliorées en stockant les résultats de requêtes de base de données coûteux dans la mémoire de s d’application.

ASP.NET 2.0 offre une variété d’options de mise en cache. Une page web entière ou un balisage s rendu de contrôle de l’utilisateur peut être mis en cache via *la mise en cache de sortie*. Les contrôles ObjectDataSource et SqlDataSource fournissent des fonctionnalités mise en cache, de même qu’en permettant des données doit être mis en cache au niveau du contrôle. Et ASP.NET s *cache de données* fournit une API de mise en cache riche qui permet aux développeurs de page par programmation des objets du cache. Dans ce didacticiel et les trois suivants, nous allons examiner à l’aide de l’ObjectDataSource s mise en cache de fonctionnalités, ainsi que le cache de données. Nous allons également découvrir comment mettre en cache les données de l’application au démarrage et comment actualiser les données mises en cache grâce à l’utilisation de dépendances de cache SQL. Ces didacticiels explorent pas la mise en cache de sortie. Pour obtenir un aperçu détaillé de la mise en cache de sortie, consultez [mise en cache de sortie dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

La mise en cache peut s’appliquer à n’importe quel endroit dans l’architecture, à partir de la couche d’accès aux données de via la couche de présentation. Dans ce didacticiel, nous allons examiner d’appliquer la mise en cache à la couche de présentation via le contrôle ObjectDataSource. Dans le didacticiel suivant, que nous allons examiner la mise en cache les données à la couche de logique métier.

## <a name="key-caching-concepts"></a>Concepts de la mise en cache de clé

La mise en cache peut améliorer considérablement un s d’application globale des performances et évolutivité en prenant des données sont coûteuses générer et stocker une copie de celui-ci dans un emplacement qui est accessible de manière plus efficace. Étant donné que le cache conserve simplement une copie des données réelles, sous-jacent, il peut devenir obsolète, ou *obsolètes*, si les données sous-jacentes changent. Pour remédier à ce problème, un développeur de pages peut indiquer des critères par lequel l’élément de cache sera *évincées* à partir du cache, à l’aide :

- **Critères temporels** un élément peut être ajouté au cache pour absolue ou glissante de durée. Par exemple, un développeur de page peut indiquer une durée égale à, par exemple, 60 secondes. Avec une durée absolue, l’élément mis en cache est évincé 60 secondes après que qu’il a été ajouté au cache, quel que soit la fréquence à laquelle il a accédé. Avec une période glissante, l’élément mis en cache est évincé 60 secondes après le dernier accès.
- **Critères de dépendance** une dépendance peut être associée à un élément lors de l’ajout au cache. Lorsque la dépendance de l’élément s change, il est supprimé du cache. La dépendance peut être un fichier, un autre élément de cache ou une combinaison des deux. ASP.NET 2.0 permet également de dépendances de cache SQL, qui permettent aux développeurs d’ajouter un élément au cache et l’avez supprimée lorsque la base de données sous-jacente change. Nous allons examiner les dépendances de cache SQL dans le prochain [à l’aide des dépendances de Cache SQL](using-sql-cache-dependencies-vb.md) didacticiel.

Quel que soit les critères d’éviction spécifiés, un élément dans le cache peut être *au nettoyage* avant les critères temporels ou basée sur une dépendance a été remplie. Si le cache a atteint sa capacité maximale, les éléments existants doivent être supprimés avant que d’autres peuvent être ajoutés. Par conséquent, en le par programmation intégrant avec les données mises en cache s vital que vous supposez toujours que les données mises en cache ne peuvent pas être présents. Nous allons examiner le modèle à utiliser lors de l’accès aux données à partir du cache par programmation dans notre didacticiel suivant, *la mise en cache des données dans l’Architecture*.

La mise en cache fournit un moyen économique pour tirant plus de performances à partir d’une application. En tant que [Steven Smith](http://aspadvice.com/blogs/ssmith/) énonce dans son article [mise en cache ASP.NET : Techniques et meilleures pratiques](https://msdn.microsoft.com/library/aa478965.aspx):

La mise en cache peut être un bon moyen pour une bonne suffisamment les performances sans nécessiter beaucoup de temps et d’analyse. Mémoire est bon marchée, si vous pouvez obtenir les performances que vous avez besoin en mettant en cache la sortie pendant 30 secondes au lieu de passer une semaine d’ou de l’optimisation de votre code ou la base de données, effectuez la solution de mise en cache (en supposant que 30 - seconde anciennes données est OK) et de passer. Finalement, une mauvaise conception accèdera probablement, bien sûr, vous pouvez donc essayer de concevoir correctement vos applications. Mais si vous souhaitez simplement obtenir bon suffisamment aujourd'hui des performances, la mise en cache peut être une excellente [approche], achat et vous pourrez faire pour refactoriser votre application à une date ultérieure, lorsque vous avez le temps de le faire.

Bien que la mise en cache peut fournir des améliorations de performances sensible, il n’est pas applicable dans toutes les situations, comme avec les applications qui utilisent des données fréquemment mise à jour en temps réel ou où même peu de temps-durée de vie des données périmées sont inacceptables. Mais pour la majorité des applications, la mise en cache doit être utilisé. Pour plus d’informations sur la mise en cache dans ASP.NET 2.0, reportez-vous à la [mise en cache pour des performances](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) section de la [didacticiels de démarrage rapide ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Étape 1 : Création de Pages Web mise en cache

Avant de commencer notre exploration des fonctionnalités de mise en cache de s ObjectDataSource, permettent de s tout d’abord prendre un moment pour créer les pages ASP.NET dans notre projet de site Web que nous avons besoin pour ce didacticiel et les trois suivants. Commencez par ajouter un nouveau dossier nommé `Caching`. Ensuite, ajoutez les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels liés à la mise en cache](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**Figure 1**: ajoutez les Pages ASP.NET pour les didacticiels liés à la mise en cache


Comme dans les autres dossiers, `Default.aspx` dans le `Caching` dossier répertorie les didacticiels dans sa section. N’oubliez pas que le `SectionLevelTutorialListing.ascx` contrôle utilisateur fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur la page s en mode Création.


[![Figure 2 : Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**Figure 2**: Figure 2 : ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image4.png))


Enfin, ajoutez ces pages en tant qu’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après l’utilisation des données binaires `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais les éléments pour les didacticiels de mise en cache.


![Le plan de Site inclut maintenant des entrées pour les didacticiels de mise en cache](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**Figure 3**: le plan de Site inclut maintenant des entrées pour les didacticiels de mise en cache


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Étape 2 : Afficher une liste de produits dans une Page Web

Ce didacticiel explore l’utilisation de l’ObjectDataSource contrôle s mise en cache des fonctionnalités intégrées. Avant de nous pouvons examiner ces fonctionnalités, cependant, nous devons tout d’abord une page à travailler à partir de. S permettent de créer une page web qui utilise un GridView pour afficher des informations produit récupérées par un ObjectDataSource à partir de la `ProductsBLL` classe.

Commencez par ouvrir le `ObjectDataSource.aspx` page dans le `Caching` dossier. Faites glisser un GridView à partir de la boîte à outils vers le concepteur, définissez son `ID` propriété `Products`et, à partir de sa balise active, choisir de lier à un nouveau contrôle ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource pour travailler avec la `ProductsBLL` classe.


[![Configurer pour utiliser la classe ProductsBLL ObjectDataSource](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**Figure 4**: configurer ObjectDataSource à utiliser le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image8.png))


Pour cette page, permettent de créer un GridView modifiable afin que nous pouvons examiner ce qui se passe lors de la modification de données mises en cache dans ObjectDataSource via l’interface de s GridView s. Laissez la liste déroulante dans l’onglet Sélection sa valeur par défaut, `GetProducts()`, mais modifier l’élément sélectionné dans l’onglet de mise à jour pour le `UpdateProduct` surcharge qui accepte `productName`, `unitPrice`, et `productID` comme paramètres d’entrée.


[![La valeur de la liste déroulante de mise à jour onglet s la surcharge appropriée UpdateProduct](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**Figure 5**: la valeur de la liste déroulante de s onglet de mise à jour le convenant `UpdateProduct` de surcharge ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image11.png))


Enfin, définissez les listes déroulantes dans les onglets INSERT et DELETE (aucun) et cliquez sur Terminer. À la fin de l’Assistant Configurer la Source de données, Visual Studio définit les opérations de mappage ObjectDataSource `OldValuesParameterFormatString` propriété `original_{0}`. Comme indiqué dans le [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) (didacticiel), cette propriété doit être supprimé de la syntaxe déclarative ou rétabli à sa valeur par défaut, `{0}`, dans l’ordre de notre mise à jour du workflow continuer sans erreur.

En outre, à la fin de l’Assistant Visual Studio ajoute un champ pour le contrôle GridView pour chacun des champs de données de produit. Supprimer tout sauf la `ProductName`, `CategoryName`, et `UnitPrice` BoundFields. Ensuite, mettez à jour le `HeaderText` propriétés de chacune de ces BoundFields à catégorie, les produits et les prix, respectivement. Dans la mesure où le `ProductName` champ est obligatoire, convertir le BoundField en TemplateField et ajoutez un contrôle RequiredFieldValidator à la `EditItemTemplate`. De même, convertir le `UnitPrice` BoundField en TemplateField et ajoutez un CompareValidator pour vous assurer que la valeur entrée par l’utilisateur est une devise valide valeur s supérieure ou égale à zéro. Outre ces modifications, n’hésitez pas à effectuer toute modification esthétique, comme l’alignement à droite le `UnitPrice` valeur ou la spécification de la mise en forme pour la `UnitPrice` texte dans ses interfaces en lecture seule et d’édition.

Rendre le contrôle GridView modifiables en cochant la case Activer la modification dans la balise active de s GridView. Vérifiez également les cases à cocher Activer la pagination et activer le tri.

> [!NOTE]
> Vous avez besoin d’une révision de la personnalisation de l’interface de modification s GridView ? Dans ce cas, vous référer à la [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) didacticiel.


[![Activer la prise en charge de GridView pour modification, le tri et pagination](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**Figure 6**: activer la prise en charge GridView pour modification, le tri et la pagination ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image14.png))


Après avoir apporté ces modifications de GridView, le balisage déclaratif s GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

Comme le montre la Figure 7, le contrôle GridView modifiable répertorie le nom, la catégorie et le prix de chacun des produits dans la base de données. Prenez un moment pour tester le tri de la fonctionnalité de page s les résultats, les, parcourir et modifier un enregistrement.


[![Chaque nom de produit, la catégorie et le prix sont répertorié dans un triable, Pageable modifiable GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**Figure 7**: chaque produit s nom, catégorie et le prix est répertorié dans un triable, Pageable GridView modifiable ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Étape 3 : Examen lorsque ObjectDataSource the est demandant des données

Le `Products` GridView récupère ses données à afficher en appelant le `Select` méthode de la `ProductsDataSource` ObjectDataSource. Cette ObjectDataSource crée une instance de la couche de logique métier s `ProductsBLL` classe et appelle son `GetProducts()` (méthode), qui à son tour appelle la couche d’accès aux données s `ProductsTableAdapter` s `GetProducts()` (méthode). La méthode de la couche DAL se connecte à la base de données Northwind et émet configuré `SELECT` requête. Ces données sont ensuite renvoyées à la couche DAL, qui empaquette dans un `NorthwindDataTable`. L’objet DataTable est retourné à la couche BLL, qui renvoie à ObjectDataSource, qui renvoie au GridView. Le contrôle GridView crée ensuite un `GridViewRow` pour chaque objet `DataRow` dans la table de données et chaque `GridViewRow` est finalement rendu dans le code HTML qui est retourné au client et affiché sur le navigateur du visiteur s.

Cette séquence d’événements se produit chaque fois que le contrôle GridView doit être lié à ses données sous-jacentes. Cela se produit lorsque la page est visitée en premier, lors du déplacement d’une page de données vers un autre, lors du tri de GridView, ou lorsque vous modifiez les données de s GridView via son intégré, modifier ou supprimer des interfaces. Si l’état d’affichage GridView s est désactivé, le contrôle GridView sera reliée sur chaque publication (postback) également. Le contrôle GridView peut également être explicitement reliée à ses données en appelant son `DataBind()` (méthode).

Pour apprécier entièrement la fréquence avec laquelle les données sont récupérées à partir de la base de données, permettent d’afficher un message indiquant que lorsque les données sont ré-récupérées de s. Ajouter un contrôle étiquette au-dessus de la GridView nommé `ODSEvents`. Effacez son `Text` propriété et définissez son `EnableViewState` propriété `False`. Sous l’étiquette, ajoutez un contrôle Web de bouton et définissez son `Text` propriété pour la publication (postback).


[![Ajouter une étiquette et un bouton à la Page au-dessus de la GridView](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**Figure 8**: ajouter une étiquette et un bouton à la Page ci-dessus GridView ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image20.png))


Pendant le flux de travail de l’accès du données, les opérations de mappage ObjectDataSource `Selecting` événement se déclenche avant la création de l’objet sous-jacent et sa méthode configuré appelé. Créez un gestionnaire d’événements pour cet événement et ajoutez le code suivant :


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

L’étiquette affiche chaque fois que ObjectDataSource effectue une demande à l’architecture pour les données, l’événement de sélection de texte déclenché.

Visitez cette page dans un navigateur. Lorsque la page est visitée en premier, l’événement de sélection de texte déclenché s’affiche. Cliquez sur le bouton de publication (postback) et notez que le texte disparaît (en supposant que les opérations de mappage GridView `EnableViewState` propriété est définie sur `True`, la valeur par défaut). Il s’agit, car, lors de la publication, le contrôle GridView est reconstruit à partir de son état d’affichage et par conséquent ne se tourner vers ObjectDataSource pour ses données. Tri, la pagination ou de modification de données, cependant, entraîne le contrôle GridView à relier à sa source de données, et par conséquent, l’événement de sélection de l’option déclenché s’affiche de nouveau texte.


[![Chaque fois que le contrôle GridView est de nouveau lié à sa Source de données, sélection déclenché d’événement s’affiche.](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**Figure 9**: chaque fois que GridView est de nouveau lié à sa Source de données, de déclenchement d’événement sélection s’affiche ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![Cliquant sur le bouton provoque la publication (postback) le contrôle GridView à être reconstruites à partir de son état d’affichage](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**Figure 10**: sur le bouton de publication (postback), le contrôle GridView à être reconstruites à partir de son état d’affichage ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image26.png))


Il peut sembler inutile récupérer les données de base de données chaque fois que les données sont paginées via ou triées. Après tout, depuis que nous re à l’aide de la pagination par défaut, ObjectDataSource a récupéré tous les enregistrements lors de l’affichage de la première page. Même si le contrôle GridView ne fournit pas de tri et de pagination de prise en charge, les données doivent être récupérées à partir de la base de données chaque fois que la page est visitée en premier par n’importe quel utilisateur (et sur chaque publication (postback), si l’état d’affichage est désactivé). Mais si le contrôle GridView ne s’affichent les mêmes données à tous les utilisateurs, ces demandes de base de données supplémentaires sont superflues. Pourquoi ne pas mettre en cache les résultats retournés par la `GetProducts()` (méthode) et lier le contrôle GridView à ceux mis en cache les résultats ?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Étape 4 : La mise en cache les données à l’aide de l’ObjectDataSource

En définissant simplement quelques propriétés, ObjectDataSource peut être configuré pour mettre en cache automatiquement ses données récupérées dans le cache de données ASP.NET. La liste suivante résume les propriétés liés au cache de l’ObjectDataSource :

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) doit être définie sur `True` pour activer la mise en cache. La valeur par défaut est `False`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) la quantité de temps, en secondes, pendant laquelle les données sont mises en cache. La valeur par défaut est 0. ObjectDataSource sera uniquement mettre en cache données si `EnableCaching` est `True` et `CacheDuration` a une valeur supérieure à zéro.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) peut être définie sur `Absolute` ou `Sliding`. Si `Absolute`, ObjectDataSource met en cache les données récupérées pour `CacheDuration` secondes ; si `Sliding`, les données expirent uniquement une fois qu’elle n’est pas terminée pour `CacheDuration` secondes. La valeur par défaut est `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) cette propriété permet d’associer les entrées de cache s ObjectDataSource avec une dépendance de cache existante. Les entrées de données s ObjectDataSource peuvent être prématurément supprimées du cache en faisant expirer associées `CacheKeyDependency`. Cette propriété est couramment utilisée pour associer une dépendance de cache SQL avec le cache de s ObjectDataSource, une rubrique, nous allons Explorer à l’avenir [à l’aide des dépendances de Cache SQL](using-sql-cache-dependencies-vb.md) didacticiel.

S permettent de configurer le `ProductsDataSource` ObjectDataSource pour mettre en cache ses données pendant 30 secondes sur une échelle absolue. Définir les opérations de mappage ObjectDataSource `EnableCaching` propriété `True` et son `CacheDuration` propriété à 30. Laissez le `CacheExpirationPolicy` sa valeur par défaut, la valeur de propriété `Absolute`.


[![Configurer l’ObjectDataSource pour mettre en Cache ses données pendant 30 secondes](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**Figure 11**: configurer ObjectDataSource pour mettre en Cache ses données pendant 30 secondes ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-vb/_static/image29.png))


Enregistrez vos modifications et revenir sur cette page dans un navigateur. La déclenchement d’événement de sélection de texte s’affiche lorsque vous visitez tout d’abord la page, comme initialement les données ne sont pas dans le cache. Mais les publications ultérieures déclenchées en cliquant sur le bouton de publication (postback), de tri, la pagination, ou en cliquant sur les boutons Modifier ou annuler *pas* actualiser l’événement de sélection de l’option déclenché texte. Il s’agit, car le `Selecting` événement est déclenché uniquement lors de l’ObjectDataSource obtient ses données à partir de son objet sous-jacent ; le `Selecting` ne se déclenche pas si les données sont extraites à partir du cache de données.

Après 30 secondes, les données seront supprimées du cache. Les données seront également être retirées du cache si les opérations de mappage ObjectDataSource `Insert`, `Update`, ou `Delete` méthodes sont appelées. Par conséquent, après 30 secondes ou le bouton de mise à jour a été activé, tri, la pagination, ou en cliquant sur les boutons Modifier ou Annuler entraîne l’ObjectDataSource obtenir ses données à partir de son objet sous-jacent, affichage de l’événement de sélection de l’option déclenché texte lors de la `Selecting` événement est déclenché. Les résultats retournés sont placés dans le cache de données.

> [!NOTE]
> Si vous consultez fréquemment la déclenchement d’événement de sélection de texte, même lorsque vous pensez ObjectDataSource à travailler avec des données mises en cache, il peut être dû à des contraintes de mémoire. Si vous ne comporte pas suffisamment de mémoire disponible, les données ajoutées au cache par l’ObjectDataSource peuvent récupérées. Si t ne ObjectDataSource semble être correctement la mise en cache les données ou les caches uniquement les données sporadique, fermez certaines applications pour libérer de la mémoire et réessayez.


Figure 12 illustre le s ObjectDataSource mise en cache de flux de travail. Lors du déclenche de l’événement de sélection de l’option texte s’affiche sur votre écran, c’est parce que les données n’était pas dans le cache et a dû être récupérée à partir de l’objet sous-jacent. Lorsque ce texte est manquant, toutefois, il s, car les données étaient disponibles à partir du cache. Lorsque les données sont retournées à partir du cache là s aucun appel à l’objet sous-jacent et, par conséquent, aucune requête de base de données exécutées.


![L’ObjectDataSource stocke et récupère ses données à partir du Cache de données](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**Figure 12**: ObjectDataSource stocke et récupère ses données à partir du Cache de données


Chaque application ASP.NET a son propre cache de données s partagées entre toutes les pages et les visiteurs de l’instance. Cela signifie que les données stockées dans le cache de données par ObjectDataSource sont de même partagées entre tous les utilisateurs qui visitent la page. Pour vérifier cela, ouvrez le `ObjectDataSource.aspx` page dans un navigateur. Lors de la première visite la page, la déclenchement d’événement de sélection de texte s’affiche (en supposant que les données ajoutées au cache par les tests précédents a, à ce stade, été supprimées). Ouvrez une deuxième instance du navigateur et la copie et collez l’URL de la première instance de navigateur à la seconde. Dans la deuxième instance de navigateur, la déclenchement d’événement de sélection de texte n’est pas affichée car il s en utilisant le même mis en cache les données en tant que la première.

Lors de l’insertion de ses données récupérées dans le cache, ObjectDataSource utilise une valeur de clé de cache inclut : le `CacheDuration` et `CacheExpirationPolicy` les valeurs de propriété, le type de l’objet métier sous-jacent utilisé par ObjectDataSource, qui est spécifiée via le [ `TypeName` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, dans cet exemple) ; la valeur de la `SelectMethod` propriété et le nom et les valeurs des paramètres dans le `SelectParameters` collection ; et les valeurs de ses `StartRowIndex`et `MaximumRows` propriétés, qui sont utilisées lors de l’implémentation [la pagination personnalisée](../paging-and-sorting/paging-and-sorting-report-data-vb.md).

Élaboration de la valeur de clé de cache comme une combinaison de ces propriétés permet de s’assurer une entrée de cache unique comme ces valeurs changent. Par exemple, dans les derniers didacticiels nous ve examiné à l’aide de la `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`, qui retourne tous les produits pour une catégorie spécifiée. Un utilisateur peut provenir des page et afficher les boissons, qui a un `CategoryID` de 1. Si l’ObjectDataSource mis en cache ses résultats sans tenir compte de le `SelectParameters` valeurs, lorsqu’un autre utilisateur a accédé à la page pour afficher les condiments pendant les produits de boissons dans le cache, ils d voient les produits de boissons mis en cache plutôt que les condiments. En modifiant la clé de cache par ces propriétés, qui incluent les valeurs de la `SelectParameters`, ObjectDataSource tient à jour une entrée de cache distinct pour beverages et condiments.

## <a name="stale-data-concerns"></a>Problèmes de données obsolètes

ObjectDataSource évince automatiquement ses éléments à partir du cache lorsque l’un de ses `Insert`, `Update`, ou `Delete` méthodes est appelée. Cela permet de protéger contre les données périmées en supprimant les entrées du cache lorsque les données sont modifiées via la page. Toutefois, il est possible pour un ObjectDataSource à l’aide de la mise en cache pour toujours afficher les données périmées. Dans le cas le plus simple, cela peut être dû les données en modifiant directement dans la base de données. Par exemple un administrateur de base de données venez d’exécuter un script qui modifie certains enregistrements dans la base de données.

Ce scénario pourrait également dérouler de façon plus subtile. Tandis que ObjectDataSource évince ses éléments à partir du cache lorsque l’une de ses méthodes de modification de données est appelée, les éléments mis en cache supprimées sont pour l’ObjectDataSource s combinaison de valeurs de propriété (`CacheDuration`, `TypeName`, `SelectMethod`, et ainsi de suite). Si vous avez deux ObjectDataSources qui utilisent différents `SelectMethods` ou `SelectParameters`, mais toujours peut mettre à jour les mêmes données, puis une ObjectDataSource peut mettre à jour une ligne et invalider ses propres entrées de cache, mais la ligne correspondante pour le deuxième ObjectDataSource sera toujours pris en charge à partir du cache. Je vous encourage à créer des pages pour exposer cette fonctionnalité. Créer une page qui affiche un GridView modifiable qui extrait ses données à partir de l’ObjectDataSource qui utilise la mise en cache et est configuré pour obtenir des données à partir de la `ProductsBLL` classe s `GetProducts()` (méthode). Ajoutez un autre modifiable GridView et ObjectDataSource à cette page (ou à une autre), mais pour cette deuxième ObjectDataSource sorte qu’il utilise le `GetProductsByCategoryID(categoryID)` (méthode). Depuis les deux ObjectDataSources `SelectMethod` propriétés diffèrent, ils ll chaque ont leurs propres valeurs mises en cache. Si vous modifiez un produit dans une grille, la prochaine fois que vous liez les données à la grille autres (par la pagination, de tri, etc.), il toujours fournir les données anciennes, mis en cache et ne reflète pas la modification a été effectuée à partir de la grille des autres.

En bref, utilisez uniquement temporelle des expirations dans le si vous êtes disposé à ont le potentiel des données périmées et utiliser les expirations dans le plus courtes pour les scénarios où l’actualisation des données est importante. Si des données périmées ne sont pas acceptables, renoncer à la mise en cache ou utiliser les dépendances de cache SQL (en supposant qu’il s’agit des données de base de données vers laquelle vous faites la mise en cache). Nous allons explorer les dépendances de cache SQL dans un futur didacticiel.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons examiné les fonctionnalités de mise en cache intégrées s ObjectDataSource. En définissant simplement quelques propriétés, nous pouvons demander à ObjectDataSource pour mettre en cache les résultats retournés à partir du spécifié `SelectMethod` dans le cache de données ASP.NET. Le `CacheDuration` et `CacheExpirationPolicy` propriétés indiquent la durée de l’élément est mis en cache et s’il s’agit d’une absolue ou expiration décalée. Le `CacheKeyDependency` propriété associe toutes les entrées de cache s ObjectDataSource avec une dépendance de cache existante. Cela peut être utilisé pour supprimer les entrées de s ObjectDataSource à partir du cache avant l’expiration temporelle est atteinte et est généralement utilisée avec les dépendances de cache SQL.

Étant donné que ObjectDataSource met simplement en cache ses valeurs au cache de données, nous pourrions répliquer la fonctionnalité intégrée de s ObjectDataSource par programmation. Il ne judicieux de le faire au niveau de la couche présentation, étant donné que ObjectDataSource offre cette fonctionnalité prête à l’emploi, mais nous pouvons implémenter des fonctionnalités de mise en cache dans une couche distincte de l’architecture. Pour ce faire, nous allons devoir répéter la même logique utilisée par l’ObjectDataSource. Nous allons découvrir comment travailler par programmation avec le cache de données à partir de l’architecture dans notre didacticiel suivant.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Mise en cache ASP.NET : Techniques et meilleures pratiques](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guide d’Architecture de mise en cache pour les Applications .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Mise en cache de sortie dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](using-sql-cache-dependencies-cs.md)
> [Suivant](caching-data-in-the-architecture-vb.md)
