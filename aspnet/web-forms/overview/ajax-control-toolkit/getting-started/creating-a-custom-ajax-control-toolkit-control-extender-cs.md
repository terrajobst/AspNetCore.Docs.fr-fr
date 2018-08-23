---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Création d’un AJAX personnalisé contrôler l’extendeur de contrôle Toolkit (c#) | Microsoft Docs
author: microsoft
description: Les extendeurs personnalisés permettent de personnaliser et étendre les fonctionnalités des contrôles ASP.NET sans avoir à créer de nouvelles classes.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: b9a3b9a8d5c86cc7aac6aeb8b4bac48af2e2edc7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829508"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Création d’un extendeur de contrôle de boîte à outils de contrôle AJAX personnalisés (c#)
====================
par [Microsoft](https://github.com/microsoft)

> Les extendeurs personnalisés permettent de personnaliser et étendre les fonctionnalités des contrôles ASP.NET sans avoir à créer de nouvelles classes.


Dans ce didacticiel, vous allez apprendre à créer un extendeur de contrôle AJAX Control Toolkit personnalisé. Nous créons une simple, mais utile, nouvelle extendeur qui modifie l’état d’un bouton de désactivé à activé lorsque vous tapez du texte dans une zone de texte. Après avoir lu ce didacticiel, vous serez en mesure d’étendre le Kit de ressources ASP.NET AJAX avec vos propres extendeurs de contrôle.

Vous pouvez créer des extendeurs de contrôle personnalisé à l’aide de Visual Studio ou Visual Web Developer (Vérifiez que vous disposez de la dernière version de Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Vue d’ensemble de l’extendeur DisabledButton

Notre nouvel extendeur de contrôle est nommé l’extendeur DisabledButton. Cet extendeur aura trois propriétés :

- TargetControlID - la zone de texte qui étend le contrôle.
- TargetButtonIID - le bouton qui est activée ou désactivée.
- DisabledText - le texte qui s’affiche initialement dans le bouton. Lorsque vous commencez à taper, le bouton affiche la valeur de la propriété de texte du bouton.

Raccordement de l’extendeur DisabledButton à un contrôle TextBox et Button. Avant de saisir n’importe quel texte, le bouton est désactivé et la zone de texte et un bouton ressemblent à ceci :


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Une fois que vous commencez à taper de texte, le bouton est activé et la zone de texte et un bouton ressemblent à ceci :


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Pour créer notre extendeur de contrôle, nous devons créer les trois fichiers suivants :

- DisabledButtonExtender.cs - ce fichier est la classe de contrôle côté serveur qui gère la création de votre extendeur et vous permettent de définir les propriétés au moment du design. Il définit également les propriétés qui peuvent être définies sur votre extendeur. Ces propriétés sont accessibles par le biais de code et au moment du design et correspondent aux propriétés définies dans le fichier DisableButtonBehavior.js.
- DisabledButtonBehavior.js--Ce fichier est où vous allez ajouter l’ensemble de votre logique de script client.
- DisabledButtonDesigner.cs - cette classe permet à des fonctionnalités au moment du design. Vous avez besoin de cette classe si vous souhaitez que l’extendeur du contrôle pour fonctionner correctement avec le Concepteur de développeur Visual Studio/Visual Web.

Par conséquent, un extendeur de contrôle se compose d’un contrôle côté serveur, un comportement côté client et une classe de concepteur côté serveur. Vous allez apprendre à créer les trois de ces fichiers dans les sections suivantes.

## <a name="creating-the-custom-extender-website-and-project"></a>Créer le projet et le site Web d’extendeur personnalisé

La première étape consiste à créer un projet bibliothèque de classes et le site Web dans Visual Studio/Visual Web Developer. Nous ll créer l’extendeur personnalisé dans le projet de bibliothèque de classes et tester l’extendeur personnalisé dans le site Web.

Laisser s démarrer avec le site Web. Suivez ces étapes pour créer le site Web :

1. Sélectionnez l’option de menu **fichier, nouveau Site Web**.
2. Sélectionnez le **Site Web ASP.NET** modèle.
3. Nommez le nouveau site Web *Website1*.
4. Cliquez sur le **OK** bouton.

Ensuite, nous devons créer le projet de bibliothèque de classes qui contient le code pour l’extendeur du contrôle :

1. Sélectionnez l’option de menu **fichier, ajouter, nouveau projet**.
2. Sélectionnez le **bibliothèque de classes** modèle.
3. Nommer la nouvelle bibliothèque de classes avec le nom **CustomExtenders**.
4. Cliquez sur le **OK** bouton.

Après avoir effectué ces étapes, votre fenêtre de l’Explorateur de solutions doit ressembler à la Figure 1.


[![Solution avec un projet de bibliothèque de site Web et de classe](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Figure 01**: Solution avec le projet de bibliothèque de site Web et de la classe ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Ensuite, vous devez ajouter toutes les références d’assembly nécessaires au projet de bibliothèque de classes :

1. Cliquez sur le projet CustomExtenders et sélectionnez l’option de menu **ajouter une référence**.
2. Sélectionnez l’onglet .NET.
3. Ajoutez des références aux assemblys suivants :

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Sélectionnez l’onglet Parcourir.
5. Ajoutez une référence à l’assembly AjaxControlToolkit.dll. Cet assembly se trouve dans le dossier où vous avez téléchargé les outils de contrôle AJAX.

Après avoir effectué ces étapes, votre dossier de références de projet de bibliothèque de classe doit ressembler à la Figure 2.


[![Dossier des références avec les références requises](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Figure 02**: dossier références avec les références requises ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Création de l’extendeur de contrôle personnalisé

Maintenant que nous avons notre bibliothèque de classes, nous pouvons commencer à créer notre contrôle d’extendeur. Permettent de démarrer avec le composant élémentaire d’une classe de contrôle d’extendeur personnalisé (voir Listing 1) s.

**Liste 1 - MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Il existe plusieurs choses que vous remarquez sur la classe d’extendeur de contrôle dans le Listing 1. Tout d’abord, notez que la classe hérite de la classe de base ExtenderControlBase. Tous les contrôles d’extendeur AJAX Control Toolkit dérivent de cette classe de base. Par exemple, la classe de base inclut la propriété TargetID qui est une propriété obligatoire de chaque extendeur de contrôle.

Ensuite, notez que la classe inclut les deux attributs suivants liés à un script client :

- WebResource - provoque un fichier à inclure en tant que ressource incorporée dans un assembly.
- ClientScriptResource - provoque une ressource de script à récupérer à partir d’un assembly.

L’attribut WebResource est utilisé pour incorporer le fichier MyControlBehavior.js JavaScript dans l’assembly lors de la compilation de l’extendeur personnalisé. L’attribut ClientScriptResource est utilisé pour récupérer le script MyControlBehavior.js à partir de l’assembly lors de l’extendeur personnalisé est utilisé dans une page web.


Dans l’ordre des attributs WebResource et ClientScriptResource fonctionne, vous devez compiler le fichier JavaScript en tant que ressource incorporée. Sélectionnez le fichier dans la fenêtre Explorateur de solutions, ouvrez la feuille de propriétés et affectez la valeur *ressource incorporée* à la **Action de génération** propriété.


Notez que l’extendeur du contrôle inclut également un attribut TargetControlType ne. Cet attribut est utilisé pour spécifier le type de contrôle qui est étendu par l’extendeur du contrôle. Dans le cas de liste 1, l’extendeur du contrôle est utilisé pour étendre une zone de texte.

Enfin, notez que l’extendeur personnalisé inclut une propriété nommée MyProperty. La propriété est marquée avec l’attribut ExtenderControlProperty. Les méthodes GetPropertyValue() et SetPropertyValue() sont utilisées pour passer la valeur de propriété à partir de l’extendeur de contrôle côté serveur pour le comportement côté client.

Permettent de s continuez et implémentez le code pour notre extendeur DisabledButton. Vous trouverez le code pour cet extendeur dans le Listing 2.

**Listing 2 - DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

L’extendeur DisabledButton dans le Listing 2 a deux propriétés nommées TargetButtonID et DisabledText. Le IDReferenceProperty appliquée à la propriété TargetButtonID vous empêche d’autre que l’ID d’un contrôle bouton affectation à cette propriété.

Les attributs WebResource et ClientScriptResource associent un comportement côté client situé dans un fichier nommé DisabledButtonBehavior.js avec cet extendeur. Nous abordons ce fichier JavaScript dans la section suivante.

## <a name="creating-the-custom-extender-behavior"></a>Création d’un comportement d’extendeur personnalisé

Le composant côté client d’un extendeur de contrôle est appelé un comportement. La logique réelle de la désactivation et l’activation du bouton est contenue dans le comportement de DisabledButton. Le code JavaScript pour le comportement est inclus dans le Listing 3.

**Liste 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Le fichier JavaScript dans le Listing 3 contient une classe côté client nommée DisabledButtonBehavior. Cette classe, comme sa représentation côté serveur, inclut deux propriétés nommées TargetButtonID et DisabledText auquel vous pouvez accéder à l’aide de get\_TargetButtonID/set\_TargetButtonID et obtenez\_DisabledText/set\_ DisabledText.

La méthode initialize() associe un gestionnaire d’événements de touche relâchée à l’élément cible pour le comportement. Chaque fois que vous tapez une lettre dans la zone de texte associé à ce comportement, le Gestionnaire de keyup s’exécute. Le Gestionnaire de keyup Active ou désactive le bouton selon que la zone de texte associée au comportement contient tout texte.

N’oubliez pas que vous devez compiler le fichier JavaScript dans la liste de 3 comme ressource incorporée. Sélectionnez le fichier dans la fenêtre Explorateur de solutions, ouvrez la feuille de propriétés et affectez la valeur *ressource incorporée* à la **Action de génération** propriété (voir Figure 3). Cette option est disponible dans Visual Studio et Visual Web Developer.


[![Ajout d’un fichier JavaScript en tant que ressource incorporée](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Figure 03**: ajout d’un fichier JavaScript comme une ressource incorporée ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Création du Concepteur d’extendeur personnalisé

Il existe une classe dernière que nous devons créer pour terminer notre extendeur. Nous devons créer la classe de concepteur dans la liste 4. Cette classe est nécessaire pour rendre l’extendeur se comportent correctement avec le Concepteur de développeur Visual Studio/Visual Web.

**Liste 4 - DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Vous associez le concepteur dans la liste 4 de l’extendeur DisabledButton avec l’attribut de concepteur. Vous devez appliquer l’attribut de concepteur pour la classe DisabledButtonExtender comme suit :

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>À l’aide de l’extendeur personnalisé

Maintenant que nous avons terminé la création de l’extendeur du contrôle DisabledButton, il est temps de les utiliser dans notre site Web ASP.NET. Tout d’abord, nous devons ajouter l’extendeur personnalisé à la boîte à outils. Procédez comme suit :

1. Ouvrez une page ASP.NET en double-cliquant sur la page dans la fenêtre Explorateur de solutions.
2. Avec le bouton droit de la boîte à outils et sélectionnez l’option de menu **choisir des éléments de**.
3. Dans la boîte de dialogue Choisir des éléments de boîte à outils, accédez à l’assembly CustomExtenders.dll.
4. Cliquez sur le **OK** bouton pour fermer la boîte de dialogue.

Après avoir effectué ces étapes, l’extendeur du contrôle DisabledButton doit apparaître dans la boîte à outils (voir Figure 4).


[![DisabledButton dans la boîte à outils](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Figure 04**: DisabledButton dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Ensuite, nous devons créer une page ASP.NET. Procédez comme suit :

1. Créer une nouvelle page ASP.NET nommée ShowDisabledButton.aspx.
2. Faites glisser un ScriptManager sur la page.
3. Faites glisser un contrôle de zone de texte sur la page.
4. Faites glisser un contrôle de bouton sur la page.
5. Dans la fenêtre Propriétés, modifiez la propriété ID de bouton à la valeur <em>btnSave</em> et la propriété de texte sur la valeur *enregistrer\**.
  

Nous avons créé une page avec un contrôle ASP.NET TextBox et Button standard.

Ensuite, nous devons étendre le contrôle de zone de texte avec l’extendeur DisabledButton :

1. Sélectionnez le **ajouter un extendeur** option pour ouvrir la boîte de dialogue Assistant extendeur de tâche (voir Figure 5). Notez que la boîte de dialogue inclut notre extendeur DisabledButton personnalisé.
2. Sélectionnez l’extendeur DisabledButton et cliquez sur le **OK** bouton.


[![La boîte de dialogue Assistant extendeur](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Figure 05**: boîte de dialogue de l’Assistant d’extendeur ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Enfin, nous pouvons définir les propriétés de l’extendeur DisabledButton. Vous pouvez modifier les propriétés de l’extendeur DisabledButton en modifiant les propriétés du contrôle de zone de texte :

1. Sélectionnez la zone de texte dans le concepteur.
2. Dans la fenêtre Propriétés, développez le nœud d’extendeurs (voir Figure 6).
3. Affectez la valeur *enregistrer* à la propriété DisabledText et la valeur *btnSave* à la propriété TargetButtonID.


[![Définition des propriétés d’extendeur](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Figure 06**: définition des propriétés d’extendeur ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Lorsque vous exécutez la page (en appuyant sur F5), le contrôle Button est initialement désactivé. Dès que vous commencez la saisie de texte dans la zone de texte, le bouton de contrôle est activé (voir Figure 7).


[![L’extendeur DisabledButton en action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Figure 07**: The DisabledButton l’extendeur en action ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel est d’expliquer comment vous pouvez étendre les outils de contrôle AJAX avec les contrôles d’extendeur personnalisé. Dans ce didacticiel, nous avons créé un extendeur de contrôle DisabledButton simple. Nous avons implémenté Cet extendeur en créant une classe DisabledButtonExtender, un comportement DisabledButtonBehavior JavaScript et une classe DisabledButtonDesigner. Vous suivez un ensemble similaire d’étapes chaque fois que vous créez un extendeur de contrôle personnalisé.

> [!div class="step-by-step"]
> [Précédent](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Suivant](get-started-with-the-ajax-control-toolkit-vb.md)
