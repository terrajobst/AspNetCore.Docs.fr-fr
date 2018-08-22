---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Création d’une disposition cohérente dans ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: tfitzmac
description: Pour le rendre plus efficace de créer des pages web pour votre site, vous pouvez créer des blocs réutilisables de contenu (par exemple, les en-têtes et pieds de page) pour votre site Web et vous c...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 82d55bb8eee6c8121a5e4d3d8121bf2b075e3df8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830156"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Création d’une disposition cohérente dans les Sites ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment vous pouvez utiliser des pages de disposition dans un site Web ASP.NET Web Pages (Razor) pour créer des blocs réutilisables de contenu (par exemple, les en-têtes et pieds de page) et pour créer une apparence cohérente pour toutes les pages du site.
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer des blocs réutilisables de contenu tels que les en-têtes et pieds de page.
> - Comment créer une apparence cohérente pour toutes les pages de votre site à l’aide d’une mise en page.
> - Comment passer des données au moment de l’exécution à une page de disposition.
> 
> Voici les fonctionnalités d’ASP.NET introduites dans l’article :
> 
> - Blocs de contenu, qui sont des fichiers qui contiennent le contenu au format HTML à insérer dans plusieurs pages.
> - Pages de disposition, qui sont des pages qui contiennent du contenu au format HTML qui peut être partagé par les pages sur le site Web.
> - Le `RenderPage`, `RenderBody`, et `RenderSection` méthodes, qui indiquent à ASP.NET où insérer des éléments de page.
> - Le `PageData` dictionnaire qui vous permet de partager des données entre les blocs de contenu et les pages de disposition.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Sur les Pages de disposition

De nombreux sites Web dont le contenu s’affiche sur chaque page, comme un en-tête et pied de page ou une zone qui indique aux utilisateurs qu’ils êtes connectés. ASP.NET vous permet de créer un fichier distinct avec un bloc de contenu qui permettre contenir du texte, balisage et code, tout comme une page web normale. Vous pouvez ensuite insérer le bloc de contenu dans d’autres pages sur le site où vous souhaitez que les informations s’affichent. De cette façon, que vous n’êtes pas obligé de copier et coller le même contenu dans chaque page. Création de contenu courants comme ceci rend également plus facile à mettre à jour votre site. Si vous devez modifier le contenu, vous pouvez simplement mettre à jour un seul fichier, et les modifications sont alors répercutées partout où le contenu a été inséré.

Le diagramme suivant illustre la façon dont contenu bloque le travail. Lorsqu’un navigateur demande une page à partir du serveur web, ASP.NET insère des blocs de contenu au point où la `RenderPage` méthode est appelée dans la page principale. La page terminé (fusionnée) est ensuite envoyée au navigateur.

![Diagramme conceptuel montrant comment la méthode RenderPage insère une page référencée dans la page actuelle.](3-creating-a-consistent-look/_static/image1.jpg)

Dans cette procédure, vous allez créer une page qui fait référence à deux blocs de contenu (un en-tête et un pied de page) qui sont trouvent dans des fichiers distincts. Vous pouvez utiliser ces mêmes blocs de contenu dans n’importe quelle page de votre site. Lorsque vous avez terminé, vous obtiendrez une page comme celle-ci :

![Capture d’écran montrant une page dans le navigateur qui résulte de l’exécution d’une page qui inclut les appels à la méthode RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. Dans le dossier racine de votre site Web, créez un fichier nommé *Index.cshtml*.
2. Remplacez le balisage existant par le code suivant :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Dans le dossier racine, créez un dossier nommé *partagé*.

    > [!NOTE]
    > Il est courant pour stocker les fichiers qui sont partagées entre les pages web dans un dossier nommé *partagé*.
4. Dans le *partagé* dossier, créez un fichier nommé  *\_Header.cshtml*.
5. Remplacer le contenu existant avec les éléments suivants :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Notez que le nom de fichier est  *\_Header.cshtml*, un trait de soulignement (\_) comme préfixe. ASP.NET ne sont pas envoyer une page au navigateur si son nom commence par un trait de soulignement. Cela empêche les personnes à partir de la demande (par inadvertance ou autre) ces pages directement. Il est judicieux d’utiliser un trait de soulignement pour les pages de nom comportant des blocs de contenu, car vous ne veux pas vraiment les utilisateurs soient en mesure de demander ces pages &#8212; elles existent strictement pour être inséré dans les autres pages.
6. Dans le *partagé* dossier, créez un fichier nommé  *\_Footer.cshtml* et remplacez son contenu par le code suivant :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Dans le *Index.cshtml* page, ajoutez les deux appels à la `RenderPage` méthode, comme indiqué ici :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Cet exemple montre comment insérer un bloc de contenu dans une page web. Vous appelez le `RenderPage` (méthode) et lui transmettre le nom du fichier dont le contenu à insérer à ce stade. Ici, vous insérez le contenu de la  *\_Header.cshtml* et  *\_Footer.cshtml* de fichiers dans le *Index.cshtml* fichier.
8. Exécutez le *Index.cshtml* page dans un navigateur. (Dans WebMatrix, dans le **fichiers** espace de travail, cliquez sur le fichier, puis sélectionnez **lancer dans le navigateur**.)
9. Dans le navigateur, affichez la source de la page. (Par exemple, dans Internet Explorer, avec le bouton droit de la page, puis **afficher la Source**.)

    Cela vous permet de voir le balisage de page web qui est envoyé au navigateur, qui combine le balisage de page d’index avec les blocs de contenu. L’exemple suivant montre la page source qui est restituée pour *Index.cshtml*. Les appels à `RenderPage` insérés dans *Index.cshtml* ont été remplacés par le contenu réel des fichiers d’en-tête et pied de page.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Création d’une apparence cohérente à l’aide des Pages de disposition

Jusqu'à présent, vous avez vu qu’il est facile d’inclure le même contenu sur plusieurs pages. Une approche plus structurée pour créer une apparence cohérente pour un site consiste à utiliser les pages de disposition. Une page de disposition définit la structure d’une page web, mais ne contient pas de contenu réel. Une fois que vous avez créé une page de disposition, vous pouvez créer des pages web qui contiennent le contenu et les lier à la page de disposition. Lorsque ces pages sont affichés, ils amèneront mis en forme en fonction de la page de disposition. (Dans ce sens, une page de disposition agit comme une sorte de modèle pour le contenu qui est défini dans d’autres pages.)

La page de disposition s’apparente à n’importe quelle page HTML, sauf qu’il contient un appel à la `RenderBody` (méthode). La position de la `RenderBody` méthode dans la page de disposition détermine où les informations à partir de la page de contenu seront incluses.

Le diagramme suivant montre les pages de contenu et les pages de disposition sont associées au moment de l’exécution pour produire la page web terminé. Le navigateur demande une page de contenu. La page de contenu a un code qui spécifie la page de disposition à utiliser pour la structure de la page. Dans la page de disposition, le contenu est inséré au point où la `RenderBody` méthode est appelée. Contenu de blocs peuvent également être insérées dans la page de disposition en appelant le `RenderPage` (méthode), la façon que vous l’avez fait dans la section précédente. Lorsque la page web est terminée, il est envoyé au navigateur.

![Capture d’écran montrant une page dans le navigateur qui résulte de l’exécution d’une page qui inclut les appels à la méthode RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

La procédure suivante montre comment créer une disposition des pages de contenu et un lien hypertexte à celui-ci.

1. Dans le *partagé* dossier de votre site Web, créez un fichier nommé  *\_Layout1.cshtml*.
2. Remplacer le contenu existant avec les éléments suivants :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Vous utilisez la `RenderPage` méthode dans une page de disposition pour insérer des blocs de contenu. Une page de disposition peut contenir qu’un seul appel à la `RenderBody` (méthode).
3. Dans le *partagé* dossier, créez un fichier nommé  *\_Header2.cshtml* et remplacer le contenu existant par le code suivant :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Dans le dossier racine, créez un nouveau dossier et nommez-le *Styles*.
5. Dans le *Styles* dossier, créez un fichier nommé *Site.css* et ajoutez les définitions de style suivantes :

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Ces définitions de style sont ici uniquement pour montrer comment les feuilles de style peuvent être utilisées avec les pages de disposition. Si vous le souhaitez, vous pouvez définir vos propres styles pour ces éléments.
6. Dans le dossier racine, créez un fichier nommé *Content1.cshtml* et remplacer le contenu existant par le code suivant :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Il s’agit d’une page qui utilise une page de disposition. Le bloc de code en haut de la page indique quelle page de disposition à utiliser pour formater ce contenu.
7. Exécutez *Content1.cshtml* dans un navigateur. La page rendue utilise le format et la feuille de style définis dans  *\_Layout1.cshtml* et le texte (contenu) définies dans *Content1.cshtml*.

    ![[image]](3-creating-a-consistent-look/_static/image4.jpg)

    Vous pouvez répéter l’étape 6 pour créer des pages de contenu supplémentaires qui peuvent alors partager la même page de disposition.

    > [!NOTE]
    > Vous pouvez configurer votre site afin que vous puissiez utiliser automatiquement de la même page de disposition pour toutes les pages de contenu dans un dossier. Pour plus d’informations, consultez [personnalisation du comportement de l’échelle du Site pour ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Conception de Pages de disposition qui ont plusieurs Sections de contenu

Une page de contenu peut avoir plusieurs sections, ce qui est utile si vous souhaitez utiliser des dispositions qui ont contenu remplaçable dans plusieurs zones. Dans la page de contenu, vous attribuez un nom unique chaque section. (La section par défaut est laissée sans nom.) Dans la page de disposition, vous allez ajouter un `RenderBody` méthode pour spécifier où doit apparaître la section sans nom (valeur par défaut). Vous ajoutez ensuite distinct `RenderSection` méthodes afin de restituer des sections nommées individuellement.

Le diagramme suivant illustre la façon dont ASP.NET gère un contenu qui est divisé en plusieurs sections. Chaque section nommée est contenue dans un bloc de section dans la page de contenu. (Ils sont nommés `Header` et `List` dans l’exemple.) Le framework insère section contenue dans la page de disposition au point où la `RenderSection` méthode est appelée. La section sans nom (valeur par défaut) est insérée au point où la `RenderBody` méthode est appelée, comme vous l’avez vu précédemment.

![Diagramme conceptuel montrant comment la méthode RenderSection insère des sections de références dans la page actuelle.](3-creating-a-consistent-look/_static/image5.jpg)

Cette procédure montre comment créer une page de contenu qui a plusieurs sections de contenu et le rendu à l’aide d’une page de disposition qui prend en charge plusieurs sections de contenu.

1. Dans le *partagé* dossier, créez un fichier nommé  *\_Layout2.cshtml*.
2. Remplacer le contenu existant avec les éléments suivants :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Vous utilisez la `RenderSection` méthode pour restituer des sections de l’en-tête et la liste.
3. Dans le dossier racine, créez un fichier nommé *Content2.cshtml* et remplacer le contenu existant par le code suivant :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Cette page de contenu contient un bloc de code en haut de la page. Chaque section nommée est contenue dans un bloc de section. Le reste de la page contient la section de contenu par défaut (sans nom).
4. Exécutez *Content2.cshtml* dans un navigateur.

    ![Capture d’écran montrant une page dans le navigateur qui résulte de l’exécution d’une page qui inclut les appels à la méthode RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Rendre des Sections de contenu facultatif

En règle générale, les sections que vous créez dans une page de contenu doivent correspondre les sections qui sont définies dans la page de disposition. Vous pouvez obtenir des erreurs si un des éléments suivants se produit :

- La page de contenu contient une section qui ne dispose d’aucune section correspondante dans la page de disposition.
- La page de disposition contient une section pour lequel il n’existe aucun contenu.
- La page de disposition inclut des appels de méthode qui tentent de restituer la même section plusieurs fois.

Toutefois, vous pouvez remplacer ce comportement pour une section nommée en déclarant la section facultative dans la page de disposition. Cela vous permet de définir plusieurs pages de contenu qui peuvent partager une page de disposition, mais qui peuvent ou non avoir un contenu d’une section spécifique.

1. Ouvrez *Content2.cshtml* et supprimer la section suivante :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Enregistrez la page et exécutez-le dans un navigateur. Un message d’erreur s’affiche, étant donné que le contenu d’une section définie dans la page de disposition, à savoir la section d’en-tête ne fournit pas la page de contenu.

    ![Capture d’écran montrant l’erreur qui se produit si vous exécutez une page qui appelle la méthode de RenderSection, mais la section correspondante n’est pas fournie.](3-creating-a-consistent-look/_static/image7.jpg)
3. Dans le *partagé* dossier, ouvrez le  *\_Layout2.cshtml* page et remplacez cette ligne :

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    avec le code suivant :

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Comme alternative, vous pouvez remplacer la ligne de code précédente par le bloc de code suivant, qui produit les mêmes résultats :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Exécutez le *Content2.cshtml* page dans un navigateur à nouveau. (Si cette page est encore ouvert dans le navigateur, vous pouvez simplement l’actualiser.) Cette fois la page s’affiche sans erreur, bien qu’elle n’ait aucun en-tête.

## <a name="passing-data-to-layout-pages"></a>Passage de données pour les Pages de disposition

Vous pouvez avoir des données définies dans la page de contenu dont vous avez besoin pour faire référence à dans une page de disposition. Dans ce cas, vous devez passer les données à partir de la page de contenu à la page de disposition. Par exemple, vous souhaiterez peut-être afficher l’état de connexion d’un utilisateur, ou vous souhaiterez peut-être afficher ou masquer les zones de contenu basées sur l’entrée d’utilisateur.

Pour passer des données à partir d’une page de contenu à une page de disposition, vous pouvez placer des valeurs dans le `PageData` propriété de la page de contenu. Le `PageData` propriété est une collection de paires nom/valeur qui contiennent les données que vous voulez passer entre les pages. Dans la page de disposition, vous pouvez lire des valeurs de la `PageData` propriété.

Voici un autre diagramme. Il montre comment ASP.NET peut utiliser le `PageData` propriété pour transmettre des valeurs à partir d’une page de contenu à la page de disposition. Lorsque ASP.NET commence la construction de la page web, il crée le `PageData` collection. Dans la page de contenu, vous écrivez du code pour placer des données le `PageData` collection. Les valeurs dans le `PageData` collection est également accessible par d’autres sections de la page de contenu ou des blocs de contenu supplémentaires.

![Diagramme conceptuel qui montre comment une page de contenu peut remplir un dictionnaire PageData et transmettre ces informations à la page de disposition.](3-creating-a-consistent-look/_static/image8.jpg)

La procédure suivante montre comment passer des données à partir d’une page de contenu à une page de disposition. Lorsque la page s’exécute, il affiche un bouton qui permet à l’utilisateur de masquer ou afficher une liste qui est définie dans la page de disposition. Lorsque les utilisateurs cliquent sur le bouton, il définit une valeur vrai/faux (booléenne) dans le `PageData` propriété. La page de disposition lit cette valeur et si elle est false, masque la liste. La valeur est également utilisée dans la page de contenu pour déterminer s’il faut afficher le **masquer la liste** bouton ou le **afficher la liste de** bouton.

![[image]](3-creating-a-consistent-look/_static/image9.jpg)

1. Dans le dossier racine, créez un fichier nommé *Content3.cshtml* et remplacer le contenu existant par le code suivant :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Le code stocke deux éléments de données dans le `PageData` propriété &#8212; le titre de la page web et de la valeur true ou de false pour spécifier s’il faut afficher la liste.

    Notez que ASP.NET vous permet de mettre un balisage HTML dans la page de manière conditionnelle à l’aide d’un bloc de code. Par exemple, le `if/else` bloc dans le corps de la page détermine le formulaire à afficher selon que `PageData["ShowList"]` est définie sur true.
2. Dans le *partagé* dossier, créez un fichier nommé  *\_Layout3.cshtml* et remplacer le contenu existant par le code suivant :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    La page de disposition inclut une expression dans le `<title>` élément qui obtient la valeur de titre de la `PageData` propriété. Il utilise également le `ShowList` valeur de la `PageData` propriété afin de déterminer s’il faut afficher le bloc de contenu de liste.
3. Dans le *partagé* dossier, créez un fichier nommé  *\_List.cshtml* et remplacer le contenu existant par le code suivant :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Exécutez le *Content3.cshtml* page dans un navigateur. La page s’affiche avec la liste visible sur le côté gauche de la page et un **masquer la liste** bouton du bas.

    ![Capture d’écran montrant la page qui inclut la liste et un bouton intitulé « Masquer la liste ».](3-creating-a-consistent-look/_static/image10.jpg)
5. Cliquez sur **masquer la liste**. Disparaît de la liste et le bouton se transforme en **afficher la liste de**.

    ![Capture d’écran montrant la page qui n’inclut pas la liste et un bouton intitulé « Afficher la liste ».](3-creating-a-consistent-look/_static/image11.jpg)
6. Cliquez sur le **afficher la liste de** bouton et la liste s’affiche de nouveau.

## <a name="additional-resources"></a>Ressources supplémentaires


[Personnalisation du comportement de l’échelle du Site pour ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
