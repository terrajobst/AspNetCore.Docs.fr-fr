---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Comment utiliser le contrôle de l’éditeur HTML ? (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor est un contrôle ASP.NET AJAX qui vous permet de facilement créer et modifier le contenu HTML via des boutons dans une barre d’outils.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7bd7c5a604c897ac6dce92123e9e7ae4157d3e0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838337"
---
<a name="how-do-i-use-the-html-editor-control-c"></a>Comment utiliser le contrôle de l’éditeur HTML ? (C#)
====================
par [Microsoft](https://github.com/microsoft)

> HTMLEditor est un contrôle ASP.NET AJAX qui vous permet de facilement créer et modifier le contenu HTML via des boutons dans une barre d’outils.


L’objectif de ce didacticiel est de vous fournir une vue d’ensemble du contrôle d’éditeur HTML accompagnant les outils de contrôle AJAX. L’éditeur HTML inclut des options pour la modification de la taille de police, en sélectionnant une police, modifier la couleur d’arrière-plan, la modification de la couleur de premier plan, l’ajout de liens, ajout d’images, modification de l’alignement de texte et effectuer les opérations couper, copier et coller des opérations (voir Figure 1).


[![L’éditeur HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Figure 01**: l’éditeur HTML ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


L’éditeur HTML vous permet d’entrer le contenu à l’aide d’un mode de conception, ou vous pouvez entrer directement HTML. Vous avez également fourni avec l’option pour afficher un aperçu de votre contenu HTML (voir Figure 2).


[![Conception, HTML et l’aperçu boutons](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Figure 02**: conception, HTML et l’aperçu des boutons ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


Dans ce didacticiel, vous découvrez comment afficher l’éditeur HTML, comment personnaliser les boutons de barre d’outils qui s’affichent dans l’éditeur HTML et comment éviter les attaques de script entre sites.

## <a name="displaying-the-html-editor"></a>Affichage de l’éditeur HTML

Avant de pouvoir utiliser l’éditeur HTML dans une page ASP.NET, vous devez d’abord ajouter un contrôle ScriptManager à la page. Le contrôle ScriptManager se trouve sous l’onglet Extensions AJAX dans la boîte à outils Visual Studio/Visual Web Developer Express.

Vous devez placer le contrôle ScriptManager en haut de la page avant les autres contrôles sur la page. Par exemple, vous pouvez placer immédiatement ci-dessous ouverture côté serveur &lt;formulaire&gt; balise.

Le contrôle de l’éditeur HTML se trouve dans la boîte à outils avec le reste des contrôles AJAX Control Toolkit. Elle se nomme le contrôle de l’éditeur (voir Figure 3).


[![Le contrôle de l’éditeur HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Figure 03**: contrôle de l’éditeur HTML ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


Une fois que vous faites glisser l’éditeur HTML sur une page, vous pouvez définir ses propriétés dans la feuille de propriétés. Par exemple, vous souhaitez normalement définir les propriétés Width et Height. Listing 1 contient la source d’une page ASP.NET qui contient un éditeur HTML.

**Liste 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

La page dans le Listing 1 contient un contrôle de l’éditeur HTML, un contrôle de bouton et un contrôle littéral. Lorsque vous cliquez sur le bouton, le contenu de l’éditeur HTML s’affiche dans le contrôle Literal (voir Figure 4).


[![Envoi d’un formulaire avec un éditeur HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Figure 04**: envoi d’un formulaire avec un éditeur HTML ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


La propriété de contenu de l’éditeur HTML est utilisée pour récupérer le contenu HTML entré dans l’éditeur HTML. N’oubliez pas que ce contenu HTML peut contenir de JavaScript. Dans la section suivante, nous abordons comment vous pouvez empêcher les attaques par Injection de JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personnalisation de la barre d’outils de l’éditeur HTML

Vous pouvez personnaliser exactement les boutons s’affichent dans l’éditeur. Par exemple, vous souhaiterez peut-être supprimer l’onglet HTML pour empêcher les utilisateurs de basculer l’éditeur HTML en mode HTML. Ou, vous souhaiterez peut-être supprimer la liste de déroulante de taille de police pour empêcher les utilisateurs de la création de texte trop important dans un forum de message post (voir Figure 5).


[![Un éditeur HTML personnalisé](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Figure 05**: A personnalisé éditeur HTML ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


Vous personnalisez les boutons de barre d’outils en dérivant un nouvel éditeur HTML de la classe de l’éditeur de base. Par exemple, l’éditeur personnalisé dans le Listing 2 contient uniquement des boutons de barre d’outils pour gras et italique. Tous les autres boutons de barre d’outils ont été supprimés. En outre, l’onglet HTML a été supprimée à partir du bas de l’éditeur (mais les onglets conception et aperçu sont toujours disponibles).

**Listing 2 - application\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Vous devez ajouter la classe dans le Listing 2 à votre application\_dossier de Code afin que la classe sera compilée automatiquement. Si l’application\_dossier de Code n’existe pas dans votre site Web, vous pouvez simplement ajouter le dossier.

Après avoir créé un éditeur personnalisé, vous pouvez l’ajouter à une page ASP.NET de la même façon lorsque vous ajoutez l’éditeur HTML normal (voir Listing 3).

**Liste 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>En évitant des attaques Cross Site Scripting (XSS)

Chaque fois que vous acceptez l’entrée d’un utilisateur et réaffichez cette entrée sur votre site Web, vous ouvrez potentiellement votre site Web à des attaques de Cross-Site Scripting (XSS). En théorie, un pirate pourrait envoyer le code JavaScript qui est exécuté quand l’entrée est réaffichée. Le code JavaScript peut être utilisé pour voler les mots de passe utilisateur ou d’autres informations sensibles.

Normalement, vous pouvez anéantir les attaques XSS par codage toute entrée vous récupérer à partir d’un utilisateur avant de les afficher dans une page web HTML. Toutefois, la sortie de l’éditeur HTML de codage ne serait pas uniquement encoder en HTML &lt;script&gt; balises, il serait également coder toutes les balises HTML. En d’autres termes, vous perdrez toute la mise en forme telles que le type de police, la taille de police et la couleur d’arrière-plan.

Si vous collectez des informations sensibles à partir de vos utilisateurs, telles que les mots de passe, les numéros de carte de crédit et les numéros de sécurité sociale - vous devez afficher pas de contenu non encodé que vous récupérez à partir d’un utilisateur avec l’éditeur HTML. Vous devez utiliser l’éditeur HTML uniquement dans les situations dans lesquelles vous ne sont pas réafficher le contenu HTML, ou le contenu HTML est envoyé à votre site Web par une partie de confiance.

Par exemple, imaginez que vous créez une application de blog. Dans ce cas, il est judicieux d’utiliser l’éditeur HTML lors de la composition de billets de blog. Vous êtes la seule personne qui envoie un billet de blog et, probablement, vous pouvez faire confiance vous-même afin de ne pas envoyer le JavaScript malveillant. Toutefois, il est inutile d’utiliser l’éditeur HTML lorsque vous autorisez les utilisateurs anonymes à publier des commentaires. Vous devez être particulièrement prudent dans les situations dans lesquelles les utilisateurs envoient des informations sensibles telles que les mots de passe. Potentiellement, un utilisateur malveillant peut publier un commentaire qui contient le code JavaScript approprié pour le vol d’un mot de passe.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous ont été fournies avec une vue d’ensemble du contrôle d’éditeur HTML inclus dans les outils de contrôle AJAX. Vous avez appris comment utiliser l’éditeur HTML pour accepter un contenu riche à partir d’un utilisateur et de soumettre le contenu vers le serveur. Nous avons abordé également comment vous pouvez personnaliser les boutons de barre d’outils qui sont affichés dans l’éditeur HTML. Enfin, vous avez appris à éviter les attaques de script entre sites lors de l’utilisation de l’éditeur HTML d’accepter l’entrée potentiellement malveillante.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-html-editor-control-vb.md)
