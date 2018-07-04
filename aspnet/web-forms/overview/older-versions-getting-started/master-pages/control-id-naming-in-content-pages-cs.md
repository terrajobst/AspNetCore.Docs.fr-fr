---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Contrôler l’ID d’affectation de noms dans les Pages de contenu (c#) | Microsoft Docs
author: rick-anderson
description: Explique comment les contrôles ContentPlaceHolder servent de conteneur d’attribution de noms et par conséquent facilitez l’utilisation par programmation un contrôle difficile (via FindConrol)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ad9224e4cb580e9ef671f39bbd3da39df692a56
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377296"
---
<a name="control-id-naming-in-content-pages-c"></a>ID de contrôle d’affectation de noms dans les Pages de contenu (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Explique comment les contrôles ContentPlaceHolder servent de conteneur d’attribution de noms et par conséquent facilitez l’utilisation par programmation un contrôle difficile (via FindConrol). Examine ce problème et les solutions de contournement. Explique également comment accéder par programme à la valeur de ClientID résultante.


## <a name="introduction"></a>Introduction

Tous les contrôles de serveur ASP.NET incluent une `ID` propriété qui identifie le contrôle et est le moyen par lequel le contrôle est accessible par programmation dans la classe code-behind. De même, les éléments dans un document HTML peuvent inclure un `id` l’attribut qui identifie de façon unique l’élément ; ces `id` valeurs sont souvent utilisées dans un script côté client à référencer par programme un élément HTML particulier. Pour cette raison, vous pouvez supposer que lorsqu’un contrôle serveur ASP.NET est rendu en HTML, son `ID` valeur est utilisée comme le `id` valeur de l’élément HTML restitué. Cela n’est pas nécessairement le cas, car dans certaines circonstances, un seul contrôle avec une seule `ID` valeur peut apparaître plusieurs fois dans le balisage rendu. Considérez un contrôle GridView qui inclut un TemplateField avec un contrôle Web Label avec une `ID` valeur de ProductName. Lorsque le contrôle GridView est lié à sa source de données lors de l’exécution, cette étiquette est utilisée une seule fois pour chaque ligne GridView. Rendus besoins d’étiquette unique `id` valeur.

Pour gérer de tels scénarios, ASP.NET permet à certains contrôles être désigné en tant que conteneurs d’attribution de noms. Un conteneur d’attribution de noms sert un nouveau `ID` espace de noms. Tous les contrôles de serveur qui s’affichent dans le conteneur d’attribution de noms ont leur rendu `id` valeur préfixée avec la `ID` du contrôle conteneur d’attribution de noms. Par exemple, le `GridView` et `GridViewRow` classes sont des conteneurs d’attribution de noms. Par conséquent, un contrôle d’étiquette défini dans un GridView TemplateField avec `ID` ProductName reçoit un rendu `id` valeur `GridViewID_GridViewRowID_ProductName`. Étant donné que *GridViewRowID* est unique pour chaque ligne de GridView, résultant `id` valeurs sont uniques.

> [!NOTE]
> Le [ `INamingContainer` interface](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) est utilisé pour indiquer qu’un contrôle de serveur ASP.NET particulier doit fonctionner comme un conteneur d’attribution de noms. Le `INamingContainer` interface ne forment pas toutes les méthodes du contrôle serveur doit implémenter ; au lieu de cela, il est utilisé en tant que marqueur. Pour générer le balisage rendu, si un contrôle implémente cette interface puis le moteur ASP.NET ajoute automatiquement le préfixe son `ID` valeur à ses descendants rendue `id` des valeurs d’attribut. Ce processus est décrit plus en détail à l’étape 2.


Conteneurs d’attribution de noms non seulement modifier le rendu `id` valeur d’attribut, mais également affecter la façon dont le contrôle peut être référencé par programmation à partir de la classe de code-behind de la page ASP.NET. Le `FindControl("controlID")` méthode est couramment utilisée pour référencer par programme un contrôle Web. Toutefois, `FindControl` ne pénétrer pas aux noms de conteneurs. Par conséquent, vous ne pouvez pas utiliser directement le `Page.FindControl` méthode à référencer des contrôles au sein d’un GridView ou un autre conteneur d’attribution de noms.

Comme vous pouvez le constatez peut-être, les pages maîtres et ContentPlaceHolders sont implémentés en tant que conteneurs d’attribution de noms. Dans ce didacticiel, nous allons examiner comment master pages affectent HTML élément `id` valeurs et les façons de référencer par programmation des contrôles Web au sein d’une page de contenu à l’aide `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Étape 1 : Ajout d’une nouvelle Page ASP.NET

Pour illustrer les concepts abordés dans ce didacticiel, nous allons ajouter une nouvelle page ASP.NET à notre site Web. Créer une nouvelle page de contenu nommée `IDIssues.aspx` dans le dossier racine, en le liant à le `Site.master` page maître.


![Ajouter le contenu IDIssues.aspx de Page dans le dossier racine](control-id-naming-in-content-pages-cs/_static/image1.png)

**Figure 01**: ajouter la Page de contenu `IDIssues.aspx` dans le dossier racine


Visual Studio crée automatiquement un contrôle de contenu pour chacun des quatre ContentPlaceHolders de la page maître. Comme indiqué dans le [ *ContentPlaceHolders multiples et contenu par défaut* ](multiple-contentplaceholders-and-default-content-cs.md) didacticiel, si un contrôle de contenu n’est pas présent contenu de ContentPlaceHolder de la page maître par défaut est émis à la place. Étant donné que le `QuickLoginUI` et `LeftColumnContent` ContentPlaceHolders contiennent un balisage par défaut convenable pour cette page, poursuivre et supprimer leur correspondant des contrôles de contenu à partir de `IDIssues.aspx`. À ce stade, balisage déclaratif de la page de contenu doit ressembler à ce qui suit :


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

Dans le [ *spécifiant le titre, les balises Meta et les autres en-têtes HTML dans la Page maître* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) didacticiel, nous avons créé une classe de page de base personnalisée (`BasePage`) qui configure automatiquement titre de la page s’il s’agit pas explicitement défini. Pour le `IDIssues.aspx` page pour utiliser cette fonctionnalité, la classe code-behind de la page doit dériver de la `BasePage` classe (au lieu de `System.Web.UI.Page`). Modifiez la définition de la classe de code-behind afin qu’il ressemble à ceci :


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Enfin, mettez à jour le `Web.sitemap` fichier à inclure une entrée pour cette leçon de nouveau. Ajouter un `<siteMapNode>` élément et définissez son `title` et `url` des attributs pour « Problèmes d’affectation de noms de contrôle ID » et `~/IDIssues.aspx`, respectivement. Après avoir établi la cet ajout votre `Web.sitemap` les balises du fichier doivent ressembler à ce qui suit :


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Comme le montre la Figure 2, la nouvelle entrée de mappage de site dans `Web.sitemap` est immédiatement répercutée dans la section de leçons dans la colonne de gauche.


![La Section leçons inclut désormais un lien vers &quot;contrôler le nommage des problèmes des ID&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Figure 02**: la Section leçons inclut désormais un lien vers « ID de contrôle d’affectation de noms problèmes »


## <a name="step-2-examining-the-renderedidchanges"></a>Étape 2 : Examiner le rendu`ID`modifications

Pour mieux comprendre les modifications ASP.NET moteur permet le rendu `id` contrôle les valeurs du serveur, nous allons ajouter quelques contrôles Web pour le `IDIssues.aspx` page, puis afficher le balisage rendu envoyé au navigateur. En particulier, tapez le texte « Veuillez entrer votre âge : » suivie d’un contrôle TextBox Web. Plus bas dans la page Ajouter un contrôle Web Button et un contrôle Web Label. Définir la zone de texte `ID` et `Columns` propriétés à `Age` et 3, respectivement. Le bouton Définir `Text` et `ID` propriétés à « Submit » et `SubmitButton`. Désactivez out l’étiquette `Text` propriété et définissez son `ID` à `Results`.

À ce stade balisage déclaratif de votre contrôle de contenu doit ressembler à ce qui suit :


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Figure 3 montre la page lorsqu’ils sont affichés via le Concepteur de Visual Studio.


[![La Page inclut trois contrôles Web : une zone de texte, bouton et une étiquette](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Figure 03**: la Page inclut trois contrôles Web : une zone de texte, un bouton et une étiquette ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image5.png))


Visitez la page via un navigateur et affichez la source HTML. En tant que le balisage ci-dessous, le `id` les valeurs des éléments HTML pour les contrôles de zone de texte, bouton et étiquette Web sont une combinaison de la `ID` les valeurs des contrôles Web et la `ID` valeurs des conteneurs d’attribution de noms dans la page.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Comme indiqué précédemment dans ce didacticiel, la page maître et ses ContentPlaceHolders servent de conteneurs d’attribution de noms. Par conséquent, les deux contribuent le rendu `ID` valeurs de leurs contrôles imbriqués. Prendre la zone de texte `id` d’attribut, par exemple : `ctl00_MainContent_Age`. N’oubliez pas que le contrôle de zone de texte `ID` valeur a été `Age`. Cela est préfixé avec son contrôle ContentPlaceHolder `ID` valeur, `MainContent`. En outre, cette valeur est préfixée avec la page maître `ID` valeur, `ctl00`. L’effet net est une `id` valeur d’attribut consistant en la `ID` les valeurs de la page maître, le contrôle ContentPlaceHolder et la zone de texte.

La figure 4 illustre ce comportement. Pour déterminer le rendu `id` de la `Age` zone de texte, commencez par le `ID` valeur du contrôle zone de texte, `Age`. Progressez ensuite, dans la hiérarchie des contrôles. À chaque conteneur d’attribution de noms (ces nœuds avec une couleur pêche), préfixe actuel rendu `id` avec le conteneur de dénomination `id`.


![Les attributs d’id de rendu sont basées sur les valeurs d’ID des conteneurs d’attribution de noms](control-id-naming-in-content-pages-cs/_static/image6.png)

**Figure 04**: le rendu `id` attributs sont basés sur le `ID` valeurs des conteneurs d’attribution de noms


> [!NOTE]
> Comme expliqué, la `ctl00` partie rendue `id` attribut constitue le `ID` valeur de la page maître, mais vous vous demandez peut-être comment ce `ID` venue à la valeur. Nous n’a pas le spécifié de n’importe où dans notre page maître ou de contenu. La plupart des contrôles serveur dans une page ASP.NET sont ajoutés explicitement par un balisage déclaratif de la page. Le `MainContent` contrôle ContentPlaceHolder a été explicitement spécifié dans le balisage de `Site.master`; le `Age` zone de texte a été défini `IDIssues.aspx`du balisage. Nous pouvons spécifier le `ID` valeurs pour ces types de contrôles à partir de la fenêtre Propriétés ou de la syntaxe déclarative. Autres contrôles, tels que la page maître elle-même, ne sont pas définis dans le balisage déclaratif. Par conséquent, leur `ID` valeurs doivent être générés automatiquement pour nous. Les jeux de moteur ASP.NET le `ID` valeurs lors de l’exécution de ces contrôles dont les ID n’ont pas été définis explicitement. Il utilise le modèle d’affectation de noms `ctlXX`, où *XX* est une valeur entière séquentiellement.


Étant donné que la page maître elle-même sert comme un conteneur d’attribution de noms, les contrôles Web définis dans la page maître également ont été modifiée rendu `id` des valeurs d’attribut. Par exemple, le `DisplayDate` étiquette que nous avons ajouté à la page maître dans le [ *création d’une disposition de l’échelle du Site avec des Pages maîtres* ](creating-a-site-wide-layout-using-master-pages-cs.md) didacticiel a ce balisage de rendu qui suit :


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Notez que le `id` attribut inclut les deux de la page maître `ID` valeur (`ctl00`) et le `ID` valeur du contrôle Web Label (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Étape 3 : Référencement par programmation des contrôles Web via`FindControl`

Tous les contrôles serveur ASP.NET inclut un `FindControl("controlID")` méthode qui consiste à rechercher les descendants du contrôle pour un contrôle nommé *controlID*. Si un tel contrôle est trouvé, il est retourné ; Si aucun contrôle correspondant n’est trouvé, `FindControl` retourne `null`.

`FindControl` est utile dans les scénarios où vous devez accéder à un contrôle, mais vous n’avez pas une référence directe à celle-ci. Lorsque vous travaillez avec les données des contrôles Web tels que GridView, par exemple, les contrôles dans les champs du contrôle GridView sont définies une seule fois dans la syntaxe déclarative, mais lors de l’exécution, une instance du contrôle est créée pour chaque ligne GridView. Par conséquent, les contrôles générés lors de l’exécution existent, mais nous n’avons pas une référence directe disponible à partir de la classe code-behind. Par conséquent, nous devons utiliser `FindControl` travailler par programmation avec un contrôle spécifique dans les champs du contrôle GridView. (Pour plus d’informations sur l’utilisation de `FindControl` pour accéder aux contrôles dans les modèles d’un contrôle Web de données, consultez [mise en forme en fonction lors de données personnalisées](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Ce même scénario se produit lorsque vous ajoutez dynamiquement des contrôles Web à un formulaire Web, un sujet abordé dans [création d’Interfaces utilisateur dynamiques données entrée](https://msdn.microsoft.com/library/aa479330.aspx).

Pour illustrer l’utilisation de la `FindControl` méthode pour rechercher des contrôles dans une page de contenu, créez un gestionnaire d’événements pour le `SubmitButton`de `Click` événements. Dans le Gestionnaire d’événements, ajoutez le code suivant, qui fait référence à par programmation la `Age` zone de texte et `Results` à l’aide de l’étiquette la `FindControl` (méthode), puis affiche un message dans `Results` basée sur l’entrée de l’utilisateur.

> [!NOTE]
> Bien sûr, nous n’avez pas besoin d’utiliser `FindControl` pour référencer les contrôles Label et TextBox pour cet exemple. Nous pourrions référencez-les directement via leurs `ID` valeurs de propriété. Utiliser `FindControl` ici pour illustrer ce qui se passe lorsque vous utilisez `FindControl` à partir d’une page de contenu.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Bien que la syntaxe utilisée pour appeler le `FindControl` méthode diffère légèrement dans les deux premières lignes de `SubmitButton_Click`, ils sont sémantiquement équivalents. Souvenez-vous que tous les contrôles de serveur ASP.NET incluent une `FindControl` (méthode). Cela inclut le `Page` (classe), à partir de quels ASP.NET de toutes les classes de code-behind doivent dériver. Par conséquent, l’appel `FindControl("controlID")` équivaut à appeler `Page.FindControl("controlID")`, en supposant que vous n’avez pas remplacé le `FindControl` méthode dans votre classe code-behind ou dans une classe de base personnalisée.

Après avoir entré ce code, visitez le `IDIssues.aspx` page via un navigateur, entrez votre âge, puis cliquez sur le bouton « Submit ». Lorsque vous cliquez sur le bouton « Submit » un `NullReferenceException` est déclenché (voir Figure 5).


[![Une exception NullReferenceException est levée.](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Figure 05**: un `NullReferenceException` est déclenché ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image9.png))


Si vous définissez un point d’arrêt dans le `SubmitButton_Click` Gestionnaire d’événements vous verrez que les deux appels à `FindControl` retourner un `null` valeur. Le `NullReferenceException` est déclenché quand nous tentons d’accéder à la `Age` la zone de texte `Text` propriété.

Le problème est que `Control.FindControl` recherche uniquement *contrôle*de descendants sont *dans le même conteneur d’attribution de noms*. Étant donné que la page maître constitue un nouveau conteneur d’attribution de noms, un appel à `Page.FindControl("controlID")` jamais présente dans l’objet de page maître `ctl00`. (Vous référer à la Figure 4 pour afficher la hiérarchie des contrôles, qui affiche le `Page` objet en tant que le parent de l’objet de page maître `ctl00`.) Par conséquent, le `Results` étiquette et `Age` zone de texte sont introuvables et `ResultsLabel` et `AgeTextBox` reçoivent les valeurs de `null`.

Il existe deux solutions à ce défi : nous pouvons descendre, un conteneur d’attribution de noms à la fois, pour le contrôle approprié ; ou nous pouvons créer notre propre `FindControl` méthode qui touche aux conteneurs d’attribution de noms. Nous allons examiner chacune de ces options.

### <a name="drilling-into-the-appropriate-naming-container"></a>Extraction vers le conteneur de dénomination appropriée

Pour utiliser `FindControl` pour référencer le `Results` étiquette ou `Age` zone de texte, nous devons appeler `FindControl` à partir d’un contrôle de l’ancêtre dans le même conteneur d’attribution de noms. Comme montré de la Figure 4, le `MainContent` contrôle ContentPlaceHolder est l’ancêtre uniquement de `Results` ou `Age` qui est dans le même conteneur d’attribution de noms. En d’autres termes, l’appel la `FindControl` méthode à partir de la `MainContent` contrôle, comme indiqué dans l’extrait de code ci-dessous, correctement retourne une référence à la `Results` ou `Age` contrôles.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Toutefois, nous ne pouvons pas travailler avec le `MainContent` ContentPlaceHolder à partir de la classe code-behind de notre page de contenu à l’aide de la syntaxe ci-dessus car ContentPlaceHolder est défini dans la page maître. Au lieu de cela, nous devons utiliser `FindControl` pour obtenir une référence à `MainContent`. Remplacez le code dans le `SubmitButton_Click` Gestionnaire d’événements avec les modifications suivantes :


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Si vous visitez la page via un navigateur, entrez votre âge et cliquez sur le bouton « Submit », un `NullReferenceException` est déclenché. Si vous définissez un point d’arrêt dans le `SubmitButton_Click` Gestionnaire d’événements vous verrez que cette exception se produit lorsque vous tentez d’appeler le `MainContent` l’objet `FindControl` (méthode). Le `MainContent` objet est `null` , car le `FindControl` méthode ne peut pas localiser un objet nommé « MainContent ». La raison sous-jacente est la même qu’avec la `Results` étiquette et `Age` contrôles TextBox : `FindControl` démarre sa recherche à partir du haut de la hiérarchie des contrôles et de ne pas pénétrer des conteneurs d’attribution de noms, mais la `MainContent` ContentPlaceHolder est dans la page maître, qui est un conteneur d’attribution de noms.

Avant que nous pouvons utiliser `FindControl` pour obtenir une référence à `MainContent`, nous devons tout d’abord une référence au contrôle de page maître. Une fois que nous avons une référence à la page maître, nous pouvons obtenir une référence à la `MainContent` ContentPlaceHolder via `FindControl` et, à partir de là, fait référence à la `Results` étiquette et `Age` zone de texte (là encore, via `FindControl`). Mais comment obtenir une référence à la page maître ? En inspectant le `id` attributs dans le balisage rendu, il est évident que la page maître `ID` valeur est `ctl00`. Par conséquent, nous pourrions utiliser `Page.FindControl("ctl00")` pour obtenir une référence à la page maître, puis utiliser cet objet pour obtenir une référence à `MainContent`, et ainsi de suite. L’extrait de code suivant illustre cette logique :


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Bien que ce code fonctionnera certainement, il suppose que générés automatiquement de la page maître `ID` sera toujours `ctl00`. Il n’est jamais une bonne idée de faire des hypothèses sur les valeurs générées automatiquement.

Heureusement, une référence à la page maître est accessible via la `Page` la classe `Master` propriété. Par conséquent, au lieu de devoir utiliser `FindControl("ctl00")` pour obtenir une référence de la page maître pour accéder à la `MainContent` ContentPlaceHolder, nous pouvons à la place utiliser `Page.Master.FindControl("MainContent")`. Mise à jour le `SubmitButton_Click` Gestionnaire d’événements par le code suivant :


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Cette fois-ci, visitez la page via un navigateur, entrer votre âge et en cliquant sur le bouton « Submit » affiche le message dans le `Results` de l’étiquette, comme prévu.


[![Âge de l’utilisateur est affiché dans l’étiquette](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Figure 06**: âge de l’utilisateur est affiché dans l’étiquette ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Rechercher par le biais d’affectation de noms conteneurs de manière récursive

La raison pour laquelle l’exemple de code précédent référencé le `MainContent` contrôle ContentPlaceHolder à partir de la page maître, puis le `Results` étiquette et `Age` des contrôles de zone de texte `MainContent`, est, car le `Control.FindControl` méthode recherche uniquement dans *contrôle*d’attribution de noms conteneur. Avoir `FindControl` restent dans le conteneur de dénomination de sens dans la plupart des scénarios, car les deux contrôles dans deux conteneurs d’attribution de noms différents peuvent avoir les mêmes `ID` valeurs. Prenons le cas d’un GridView qui définit un contrôle Web Label nommé `ProductName` dans une de ses TemplateField. Lorsque les données sont liées au GridView lors de l’exécution, un `ProductName` étiquette est créée pour chaque ligne GridView. Si `FindControl` recherches à l’aide d’affectation de noms tous les conteneurs et nous avons appelé `Page.FindControl("ProductName")`, quelle instance de l’étiquette doit le `FindControl` retourner ? Le `ProductName` étiquette dans la première ligne GridView ? Celui de la dernière ligne ?

Par conséquent, avoir `Control.FindControl` rechercher simplement *contrôle*d’attribution de noms conteneur est pertinent dans la plupart des cas. Mais il existe des autres cas, tel que celui que nous devrons affronter, où nous avons un unique `ID` sur tous les conteneurs d’attribution et souhaitez éviter d’avoir à référencer méticuleusement chaque conteneur d’attribution de noms dans la hiérarchie des contrôles à un contrôle d’accès. Avoir un `FindControl` variant et qui les recherches de manière récursive tous les conteneurs d’attribution de noms rend est clair, trop. Malheureusement, le .NET Framework n’inclut pas une telle méthode.

La bonne nouvelle est que nous pouvons créer notre propre `FindControl` méthode ce récursivement recherche tous les conteneurs d’attribution de noms. En fait, à l’aide de *méthodes d’extension* nous pouvons ajouter un `FindControlRecursive` méthode à la `Control` classe pour accompagner ses `FindControl` (méthode).

> [!NOTE]
> Méthodes d’extension sont une fonctionnalité nouvelle de c# 3.0 et Visual Basic 9, qui sont des langages fournis avec le .NET Framework version 3.5 et Visual Studio 2008. En bref, les méthodes d’extension permettent à un développeur pour créer une nouvelle méthode pour un type de classe existant via une syntaxe spéciale. Pour plus d’informations sur cette fonctionnalité utile, reportez-vous à mon article, [extension des fonctionnalités de Type Base avec les méthodes d’Extension](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Pour créer la méthode d’extension, ajoutez un nouveau fichier à la `App_Code` dossier nommé `PageExtensionMethods.cs`. Ajouter une méthode d’extension nommée `FindControlRecursive` qui prend comme entrée un `string` paramètre nommé `controlID`. Méthodes d’extension fonctionner correctement, il est essentiel que la classe elle-même et ses méthodes d’extension marqué `static`. En outre, toutes les méthodes d’extension doivent accepter comme leur premier paramètre un objet du type auquel s’applique la méthode d’extension et ce paramètre d’entrée doivent être précédées du mot clé `this`.

Ajoutez le code suivant à la `PageExtensionMethods.cs` fichier de classe pour définir cette classe et la `FindControlRecursive` méthode d’extension :


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Avec ce code en place, revenez à la `IDIssues.aspx` classe code-behind de la page et commentez actuel `FindControl` les appels de méthode. Remplacez-les par des appels à `Page.FindControlRecursive("controlID")`. Trouvée concernant les méthodes d’extension est qu’ils apparaissent directement dans les listes déroulantes IntelliSense. Comme le montre la Figure 7, lorsque vous tapez la Page, puis appuyez sur la période, le `FindControlRecursive` méthode est incluse dans la liste déroulante, ainsi que l’autre IntelliSense `Control` méthodes de la classe.


[![Méthodes d’extension sont inclus dans l’IntelliSense listes déroulantes](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Figure 07**: méthodes d’Extension sont inclus dans l’IntelliSense listes déroulantes ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image15.png))


Entrez le code suivant dans le `SubmitButton_Click` Gestionnaire d’événements, puis de le tester en visitant la page, en entrant votre âge et en cliquant sur le bouton « Submit ». Comme indiqué dans la Figure 6, le résultat obtenu sera le message, « Vous êtes ans d’âge ! »


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Étant donné que les méthodes d’extension débutent avec c# 3.0 et Visual Basic 9, si vous utilisez Visual Studio 2005 vous ne pouvez pas utiliser les méthodes d’extension. Au lieu de cela, vous devez implémenter la `FindControlRecursive` méthode dans une classe d’assistance. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) a ce type d’exemple dans son billet de blog, [principale des Pages ASP.NET et `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Étape 4 : À l’aide de la bonne`id`valeur dans un Script côté Client de l’attribut

Comme indiqué dans Présentation de ce didacticiel, de rendu d’un contrôle Web `id` attribut est souvent utilisé dans un script côté client à référencer par programme un élément HTML particulier. Par exemple, le code JavaScript suivant fait référence à un élément HTML par son `id` , puis affiche sa valeur dans une boîte de message modale :


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Rappelez-vous que, dans ASP.NET, les pages qui n’incluent pas d’affectation de noms d’un conteneur, l’élément HTML restitué `id` attribut est identique au contrôle Web `ID` valeur de propriété. Pour cette raison, il est tentant de coder en dur dans `id` des valeurs d’attribut dans le code JavaScript. Autrement dit, si vous savez que vous souhaitez accéder à la `Age` contrôle TextBox Web via un script côté client, le faire via un appel à `document.getElementById("Age")`.

Le problème avec cette approche est que lors de l’utilisation des pages maîtres (ou autres contrôles de conteneur d’attribution de noms), le code HTML restitué `id` n’est pas synonyme avec le contrôle Web `ID` propriété. Votre première inclination peut-être de visiter la page via un navigateur et afficher la source pour déterminer la véritable `id` attribut. Une fois que vous connaissez le rendu `id` valeur, vous pouvez le coller dans l’appel à `getElementById` pour accéder à l’élément HTML que vous avez besoin pour travailler avec le script côté client. Cette approche est moins qu’idéale, car la hiérarchie des contrôles de certaines modifications à la page ou modifications apportées à la `ID` modifient les propriétés des contrôles d’affectation de noms résultant `id` attribut, ainsi interrompre votre code JavaScript.

La bonne nouvelle est que le `id` valeur d’attribut qui est rendu est accessible dans le code côté serveur par le biais du contrôle Web [ `ClientID` propriété](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Vous devez utiliser cette propriété pour déterminer le `id` utilisé dans un script côté client de valeur d’attribut. Par exemple, pour ajouter une fonction JavaScript à la page qui, lorsqu’elle est appelée, affiche la valeur de la `Age` zone de texte dans une boîte de dialogue modale, ajoutez le code suivant à la `Page_Load` Gestionnaire d’événements :


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Le code ci-dessus injecte la valeur de la `Age` propriété ClientID de la zone de texte dans l’appel de JavaScript à `getElementById`. Si vous visitez cette page via un navigateur et affichez la source HTML, vous trouverez le code JavaScript suivant :


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Notez comment le bon `id` valeur de l’attribut `ctl00_MainContent_Age`, apparaît dans l’appel à `getElementById`. Étant donné que cette valeur est calculée lors de l’exécution, il fonctionne indépendamment des modifications ultérieures apportées à la hiérarchie de contrôle de page.

> [!NOTE]
> Cet exemple de JavaScript montre simplement comment ajouter une fonction JavaScript qui fait référence à l’élément HTML restitué par un contrôle serveur correctement. Pour utiliser cette fonction, vous devez créer le code JavaScript pour appeler la fonction lors du chargement du document ou lorsqu’une action utilisateur spécifique se passe réellement. Pour plus d’informations sur ces et autres sujets connexes, lire [utilisation de Script côté Client](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Récapitulatif

Certains contrôles serveur ASP.NET servent de conteneurs d’attribution de noms, ce qui affecte le rendu `id` attribut des valeurs de leurs contrôles descendants, ainsi que l’étendue des contrôles canvassed par le `FindControl` (méthode). En ce qui concerne les pages maîtres, la page maître lui-même et ses contrôles ContentPlaceHolder sont aux noms de conteneurs. Par conséquent, nous devons placer les deux sens un peu plus de travail pour faire référence à la page de contenu à l’aide des contrôles par programmation `FindControl`. Dans ce didacticiel, nous avons examiné deux techniques : l’extraction dans le contrôle ContentPlaceHolder et continuent d’appeler ses `FindControl` méthode ; et le déploiement de notre propre `FindControl` implémentation de manière récursive qui effectue une recherche dans tous les conteneurs d’attribution de noms.

Outre les problèmes côté serveur introduisent des conteneurs d’attribution de noms en ce qui concerne le référencement des contrôles Web, il existe également des problèmes côté client. En l’absence de nommage des conteneurs, le contrôle Web `ID` valeur de propriété et le rendu `id` valeur d’attribut sont identiques. Mais avec l’ajout de conteneur d’attribution, le rendu `id` attribut comprend à la fois le `ID` valeurs du contrôle Web et les conteneurs d’attribution de noms dans l’ascendance de sa hiérarchie de contrôle. Ces problèmes d’affectation de noms sont sans doute pas tant que vous utilisez le contrôle Web `ClientID` propriété afin de déterminer le rendu `id` valeur dans votre script côté client de l’attribut.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Pages maître ASP.NET et `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Création d’Interfaces utilisateur Dynamic Data](https://msdn.microsoft.com/library/aa479330.aspx)
- [Extension des fonctionnalités de Type de Base avec les méthodes d’Extension](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Comment : Référencer le contenu de la Page maître ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Master Pages : Conseils, astuces et pièges](http://www.odetocode.com/articles/450.aspx)
- [Utilisation de Script côté Client](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs livres de sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier livre s’intitule [ *Sams Teach vous-même ASP.NET 3.5 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être atteint à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Zack Jones et Suchi Barnerjee. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](urls-in-master-pages-cs.md)
> [Suivant](interacting-with-the-master-page-from-the-content-page-cs.md)
