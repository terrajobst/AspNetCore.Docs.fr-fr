---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Pages maîtres | Microsoft Docs
author: microsoft
description: Un des composants clés pour un site Web a réussi est un aspect cohérent. Dans ASP.NET 1.x, les développeurs permettant de contrôles utilisateur pour répliquer courantes page elem....
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f40eb338a1b6b8eebb6578dd7938e96a05b1617f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828911"
---
<a name="master-pages"></a>Pages maîtres
====================
par [Microsoft](https://github.com/microsoft)

> Un des composants clés pour un site Web a réussi est un aspect cohérent. Dans ASP.NET 1.x, les développeurs des contrôles utilisateur utilisé pour répliquer les éléments de page communs sur une application Web. Alors que c’est certainement une solution possible, à l’aide de contrôles utilisateur également quelques inconvénients. Par exemple, une modification de la position d’un contrôle utilisateur nécessite une modification de plusieurs pages sur un site. Contrôles utilisateur ne sont pas également restituées en mode Création après avoir inséré sur une page.


Un des composants clés pour un site Web a réussi est un aspect cohérent. Dans ASP.NET 1.x, les développeurs des contrôles utilisateur utilisé pour répliquer les éléments de page communs sur une application Web. Alors que c’est certainement une solution possible, à l’aide de contrôles utilisateur également quelques inconvénients. Par exemple, une modification de la position d’un contrôle utilisateur nécessite une modification de plusieurs pages sur un site. Contrôles utilisateur ne sont pas également restituées en mode Création après avoir inséré sur une page.

ASP.NET 2.0 introduit Master pages comme un moyen de conserver une apparence cohérente et comme vous le verrez bientôt, Master pages représentent une amélioration significative sur la méthode de contrôle utilisateur.

## <a name="why-master-pages"></a>Pages maîtres pourquoi ?

Vous vous demandez peut-être pourquoi les pages maîtres étaient nécessaires dans ASP.NET 2.0. Après tout, les développeurs de site Web sont déjà à l’aide de contrôles utilisateur dans ASP.NET 1.x pour partager des zones de contenu entre les pages. Il existe en fait plusieurs raisons pourquoi les contrôles utilisateur sont une solution moins optimal pour la création d’une disposition courante.

Contrôles utilisateur ne définissent en fait mise en page. Au lieu de cela, elles définissent la disposition et les fonctionnalités d’une partie d’une page. La distinction entre ces deux est importante, car elle rend la facilité de gestion d’une solution de contrôle utilisateur beaucoup plus difficile. Par exemple, lorsque vous souhaitez modifier la position d’un contrôle utilisateur dans votre page, vous devez modifier la page sur laquelle le contrôle utilisateur s’affiche. Vous avez correctement si vous avez uniquement quelques pages, mais dans les sites de grande taille, il peut devenir un cauchemar en termes de gestion site !

Un autre inconvénient de l’utilisation de contrôles utilisateur pour définir une disposition commune est inclu dans l’architecture d’ASP.NET lui-même. Si aucun membre public d’un contrôle utilisateur est modifié, il vous oblige à recompiler toutes les pages qui utilisent le contrôle utilisateur. À son tour, ASP.NET sera ensuite accéder à vos pages lors de leur re-JIT. Une fois encore, vous obtenez une architecture non évolutive et un problème de gestion de site pour les sites de grande taille.

Les deux de ces problèmes (et bien plus encore) sont bien traités par les pages maîtres dans ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Fonctionnement des Pages maître

Une page maître est analogue à un modèle pour les autres pages. Les éléments de page qui doivent être partagés entre les autres pages (par exemple, les menus, les bordures, etc.) sont ajoutés à la page maître. Lorsque de nouvelles pages sont ajoutés au site, vous pouvez les associer avec une page maître. Une page qui est associée à une page maître est appelée un **page de contenu**. Par défaut, une page de contenu prend l’apparence de la page maître. Toutefois, lorsque vous créez une page maître, vous pouvez définir les parties de la page de la page de contenu peut remplacer avec son propre contenu. Ces parties sont définies à l’aide d’un nouveau contrôle introduit dans ASP.NET 2.0 ; le **ContentPlaceHolder** contrôle.

Une page maître peut contenir n’importe quel nombre de contrôles ContentPlaceHolder (ou aucun). Sur la page de contenu, le contenu des contrôles ContentPlaceHolder apparaît à l’intérieur de **contenu** contrôles, un autre nouveau contrôle dans ASP.NET 2.0. Par défaut, les contrôles de contenu des pages de contenu sont vides, afin que vous pouvez fournir votre propre contenu. Si vous souhaitez utiliser le contenu à partir de la page maître à l’intérieur les contrôles de contenu, vous pouvez le faire par conséquent, comme vous le verrez plus loin dans ce module. Le contrôle de contenu est mappé au contrôle ContentPlaceHolder via l’attribut ContentPlaceHolderID du contrôle de contenu. Le code ci-dessous maps un contrôle de contenu à un contrôle ContentPlaceHolder appelé mainBody sur une page maître.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Vous entendrez souvent personnes décrivent les pages maîtres comme étant une classe de base pour les autres pages. Ceci ne constitue pas réellement. La relation entre les pages maîtres et les pages de contenu n’est pas un de l’héritage.


**Figure 1** montre une page maître et une page de contenu associée tels qu’ils apparaissent dans Visual Studio 2005. Vous pouvez voir le contrôle ContentPlaceHolder dans la page maître et le correspondantes contrôle dans la page de contenu de contenu. Notez que le contenu de pages maîtres qui se trouve en dehors de ContentPlaceHolder est visible mais est grisé dans la page de contenu. Uniquement le contenu à l’intérieur de ContentPlaceHolder peut être supplanté par la page de contenu. Tout autre contenu provient de la page maître est immuable.


![Une page maître et sa page de contenu associé](master-pages/_static/image1.jpg)

**Figure 1**: une page maître et sa page de contenu associé


## <a name="creating-a-master-page"></a>Création d’une Page maître

Pour créer une nouvelle page maître :

1. Ouvrez Visual Studio 2005 et créer un nouveau site Web.
2. Cliquez sur fichier, nouveau, de fichiers.
3. Choisissez fichier Master à partir de la boîte de dialogue Ajouter un nouvel élément, comme indiqué dans **figure 2**.
4. Cliquez sur Ajouter.


![Création d’une Page maître](master-pages/_static/image2.jpg)

**Figure 2**: création d’une Page maître


Notez que l’extension de fichier pour une page maître est <em>.master</em>. Il s’agit d’une des manières qui diffère d’une page maître à partir d’une page ordinaire. La principale différence est que dans procédural à la place d’un @Page directive, la page maître contient un @Master directive. Basculer en mode Source pour le masque de page que vous venez de créer et d’examiner le code.

Une page maître aura un contrôle ContentPlaceHolder par défaut. Dans la plupart des cas, il est plus judicieux d’abord créer les éléments de page communs et puis insérer des contrôles ContentPlaceHolder là où le contenu personnalisé est souhaité. Dans ce cas, les développeurs souhaitez supprimer le contrôle ContentPlaceHolder par défaut et insérer de nouveaux au cours du développement de la page. Contrôles ContentPlaceHolder ne sont pas redimensionnables en dépit du fait qu’ils affichent les poignées de redimensionnement. Les tailles de contrôle ContentPlaceHolder automatiquement en fonction du contenu qu’il contient avec une exception ; Si vous placez un contrôle ContentPlaceHolder à l’intérieur d’un élément de bloc comme une cellule de tableau, il se redimensionne en fonction de la taille de l’élément.

## <a name="lab-1-working-with-master-pages"></a>Atelier pratique 1 Utilisation de Pages maîtres

Dans cet atelier, vous créez une page maître et définissent trois contrôles ContentPlaceHolder. Puis vous créez une nouvelle page de contenu et remplacez le contenu à partir d’au moins un des contrôles ContentPlaceHolder.

1. Créer une page maître et insérer des contrôles ContentPlaceHolder. 

    1. Créer une nouvelle page maître, comme décrit ci-dessus.
    2. Supprimer le contrôle ContentPlaceHolder par défaut.
    3. Sélectionnez le contrôle ContentPlaceHolder en cliquant sur la bordure ombrée supérieure du contrôle et supprimez-le en appuyant sur la touche SUPPR de votre clavier.
    4. Insérer une nouvelle table en utilisant le *en-tête et côté* modèle comme illustré à la figure 3. Modifier la largeur et la hauteur à 90 %, afin que la table entière est visible dans le concepteur.


![](master-pages/_static/image3.jpg)

**Figure 3**


1. Placez le curseur dans chaque cellule de la table et définissez le *valign* propriété *haut*.
2. À partir de la boîte à outils, insérez un contrôle ContentPlaceHolder dans la cellule supérieure de la table (la cellule d’en-tête.)
3. Lorsque vous insérez ce contrôle ContentPlaceHolder, vous remarquerez que la hauteur de ligne peut prendre jusqu'à presque toute la page, comme indiqué dans la figure 4. Ne pas se préoccuper de qui a ce stade.


![L’espace vide est dans la même cellule comme ContentPlaceHolder](master-pages/_static/image1.gif)

**Figure 4**: l’espace vide est dans la même cellule comme ContentPlaceHolder


1. Placez un contrôle ContentPlaceHolder dans les deux autres cellules. Une fois que les autres contrôles ContentPlaceHolder ont été insérées, la taille des cellules de tableau doit être comme vous pouvez l’imaginer. La page doit maintenant ressembler à la page illustrée **figure 5**.


![Le maître avec tous les contrôles ContentPlaceHolder. Notez que la hauteur de cellule pour la cellule d’en-tête est maintenant il doit être](master-pages/_static/image2.gif)

**Figure 5**: Master The avec tous les contrôles ContentPlaceHolder. Notez que la hauteur de cellule pour la cellule d’en-tête est maintenant il doit être


1. Entrez du texte de votre choix dans chacun des trois contrôles ContentPlaceHolder.
2. Enregistrez la page maître en tant que exercise1.master.
3. Créer un nouveau formulaire Web et l’associer à la page maître exercise1.master.
4. Sélectionnez fichier, nouveau, le fichier dans Visual Studio 2005.
5. Sélectionnez **Web Form** dans la boîte de dialogue Ajouter un nouvel élément.
6. Assurez-vous que la case à cocher Sélectionner la page maître est vérifiée comme illustré à la figure 6.


![Ajout d’une nouvelle Page de contenu](master-pages/_static/image3.gif)

**Figure 6**: ajout d’une nouvelle Page de contenu


1. Cliquez sur Ajouter.
2. Sélectionnez la exercise1.master dans l’instruction Select une boîte de dialogue de page maître comme indiqué dans la figure 7.
3. Cliquez sur OK pour ajouter la nouvelle page de contenu.

La nouvelle page de contenu s’affiche dans Visual Studio avec un contrôle de contenu pour chaque contrôle ContentPlaceHolder sur la page maître. Par défaut, les contrôles de contenu sont vides, afin que vous puissiez ajouter votre propre contenu. Si vous auriez un jour comme pour qu’ils puissent utiliser le contenu à partir du contrôle ContentPlaceHolder sur la page maître, cliquez simplement sur le symbole de balise active (la petite flèche noire dans le coin supérieur droit du contrôle) et choisissez *maîtres contenu par défaut* à partir de la balise active comme indiqué dans **figure 8**. Lorsque vous procédez ainsi, l’élément de menu change pour *créer un contenu personnalisé*. En cliquant sur à ce stade supprime le contenu à partir de la page maître, ce qui vous permet de définir le contenu personnalisé pour ce contrôle de contenu particulier.


![Définition d’un contrôle de contenu pour le contenu de Pages maître par défaut](master-pages/_static/image4.gif)

**Figure 7**: définition d’un contrôle de contenu sur le contenu de Pages maître par défaut


## <a name="connecting-master-page-and-content-pages"></a>Connexion de Page maître et les Pages de contenu

L’association entre une page maître et une page de contenu peut être configurée dans un des quatre façons différentes :

- Le <strong>MasterPageFile</strong> attribut de la @Page (directive)
- Définition de la **Page.MasterPageFile** propriété dans le code.
- Le **&lt;pages&gt;** élément dans le fichier de configuration d’applications (web.config dans le dossier racine de l’application)
- Le **&lt;pages&gt;** élément dans un fichier de configuration (web.config dans un sous-dossier) de sous-dossiers

## <a name="masterpagefile-attribute"></a>MasterPageFile (attribut)

L’attribut MasterPageFile rend plus facile à appliquer une page maître à une page ASP.NET. Il est également la méthode utilisée pour appliquer la page maître, quand vous activez le **sélectionner la Page maître** case à cocher comme vous l’avez fait dans l’exercice 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Définition Page.MasterPageFile dans le Code

En définissant la propriété MasterPageFile dans le code, vous pouvez appliquer une page maître spécifique à votre contenu lors de l’exécution. Cela est utile dans les cas où vous devrez peut-être appliquer une page maître spécifique basée sur un rôle d’utilisateurs ou d’autres critères. La propriété MasterPageFile doit être définie dans la méthode PreInit. S’il est défini après la méthode PreInit, une exception InvalidOperationException est levée. La page sur laquelle cette propriété est définie doit avoir également un contenu contrôle en tant que le contrôle de niveau supérieur de la page. Sinon une HttpException sera levée lorsque la propriété MasterPageFile est définie.

## <a name="using-the-ltpagesgt-element"></a>À l’aide de la &lt;pages&gt; élément

Vous pouvez configurer une page maître pour vos pages en définissant l’attribut masterPageFile dans le &lt;pages&gt; élément du fichier web.config. Lorsque vous utilisez cette méthode, n’oubliez pas que les fichiers web.config plus bas dans la structure de l’application peuvent remplacer ce paramètre. N’importe quel attribut MasterPageFile défini dans un @Page directive sera également remplacer ce paramètre. À l’aide de la &lt;pages&gt; élément permet de facilement créer un <em>master</em> page maître qui peut être substituée si nécessaire dans des dossiers ou fichiers.

## <a name="properties-in-master-pages"></a>Propriétés dans les Pages maîtres

Une page maître peut exposer des propriétés faisant de simplement ces propriétés publiques dans la page maître. Par exemple, le code suivant définit une propriété appelée SomeProperty :

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Pour accéder à la propriété SomeProperty à partir de la page de contenu, vous devez utiliser le masque de propriété comme suit :

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Pages maîtres d’imbrication

Pages maîtres sont la solution parfaite pour garantir une apparence commune sur une grande application Web. Toutefois, il n’est pas rare d’avoir certaines parties d’un partage de grand site une interface commune, tandis que les autres parties partagent une interface différente. Pour répondre à ce besoin, plusieurs pages maîtres sont la solution parfaite. Toutefois, ce toujours ne traite pas le fait qu’une grande application peut avoir certains composants (par exemple, un menu, par exemple) qui sont partagées entre toutes les pages et autres composants qui sont uniquement partagées entre certaines sections du site. Pour ce type de situation, les pages maîtres imbriquées suffit bien. Comme vous l’avez vu, une page maître normale se compose d’une page maître et une page de contenu. Dans une situation de page maître imbriquée, il existe deux pages maîtres ; un masque de parent et un masque d’enfant. La page maître enfant est également une page de contenu et son maître est la page maître parente.

Voici le code pour une page maître standard :

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

Dans un scénario maître imbriqué, ce serait le maître de parent. Une autre page maître utiliseriez cette page en tant que sa page maître, et que le code ressemblerait à ceci :

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Notez que dans ce scénario, le maître de l’enfant est également une page de contenu pour le maître de parent. Tout contenu de la forme de base enfants s’affiche à l’intérieur d’un contrôle de contenu qui obtient son contenu à partir d’un contrôle ContentPlaceHolder du parent.

> [!NOTE]
> Prise en charge de concepteur n’est pas disponible pour les pages maîtres imbriquées. Lorsque vous développez à l’aide de maîtres imbriquées, vous devez utiliser la vue de source.


Cette vidéo montre une procédure pas à pas de l’utilisation des pages maîtres imbriquées.


![](master-pages/_static/image1.png)


[Ouvre vidéo plein écran](master-pages/_static/nested1.wmv)


![Sélection d’une Page maître](master-pages/_static/image4.jpg)

**Figure 8**: sélection d’une Page maître
