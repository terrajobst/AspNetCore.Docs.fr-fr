---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Itération #2 : donner l’application une apparence agréable (c#) | Microsoft Docs'
author: microsoft
description: Dans cette itération, nous améliorer l’apparence de l’application en modifiant la valeur par défaut de page maître de vue ASP.NET MVC et en cascade de feuille de style.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: e78383516c752748da67f058c37aeb66d7004707
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377927"
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Itération #2 : donner l’application une apparence agréable (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le Code](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> Dans cette itération, nous améliorer l’apparence de l’application en modifiant la valeur par défaut de page maître de vue ASP.NET MVC et en cascade de feuille de style.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Création d’une Application ASP.NET MVC de gestion des contacts (c#)
  

Dans cette série de didacticiels, nous créer une application de gestion des contacts entière à partir du début à la fin. L’application Gestionnaire de Contact permet vous permettent de stocker les informations de contact (noms, numéros de téléphone et adresses de messagerie) pour obtenir la liste de personnes.

Nous générer l’application sur de multiples itérations. Avec chaque itération, nous améliorer progressivement l’application. L’objectif de cette approche à plusieurs itération consiste à vous permettre de comprendre la raison de chaque modification.

- Itération #1 : créer l’application. Dans la première itération, nous créons le Gestionnaire de Contact de la façon la plus simple possible. Nous ajoutons la prise en charge pour les opérations de base de données : création, lecture, mise à jour et suppression (CRUD).

- Itération #2 : donner l’application une apparence intéressante. Dans cette itération, nous améliorer l’apparence de l’application en modifiant la valeur par défaut de page maître de vue ASP.NET MVC et en cascade de feuille de style.

- Itération #3 - Ajouter une validation de formulaire. Dans la troisième itération, nous ajouter la validation de formulaire de base. Nous empêcher des personnes à partir de l’envoi d’un formulaire sans compléter les champs obligatoires. Nous avons également valider que les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendre l’application faiblement couplée. Dans cette troisième itération, nous profitons de plusieurs modèles de conception de logiciels pour le rendre plus facile à gérer et modifier l’application Gestionnaire de contacts. Par exemple, nous refactoriser notre application pour utiliser le modèle dépôt et le modèle d’Injection de dépendance.

- Itération #5 - créer des tests unitaires. Dans la cinquième itération, nous faciliter notre application mettre à jour et modifier en ajoutant des tests unitaires. Nous simuler nos classes de modèle de données et générer des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6 - utiliser le développement piloté par test. Dans cette itération sixième, nous ajoutons les nouvelles fonctionnalités à notre application en écrivant des tests unitaires tout d’abord et écrire du code pour les tests unitaires. Dans cette itération, nous ajouter des groupes de contacts.

- Itération #7 - ajouter des fonctionnalités Ajax. Dans l’itération septième, nous améliorer la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

L’objectif de cette itération consiste à améliorer l’apparence de l’application Gestionnaire de contacts. Actuellement, le Gestionnaire de Contact utilise la page maître de vue ASP.NET MVC par défaut et la feuille de style en cascade (voir Figure 1). Ces ne pas sembler incorrecte, mais je ne voulez pas le Gestionnaire de contacts pour rechercher tout comme chaque autre site Web ASP.NET MVC. Je souhaite remplacer ces fichiers avec des fichiers personnalisés.


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figure 01**: l’apparence par défaut d’une Application ASP.NET MVC ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


Dans cette itération, j’aborde deux approches pour améliorer la conception visuelle de notre application. Tout d’abord, je vous montrer comment tirer parti de la galerie de conception de MVC ASP.NET pour télécharger un modèle de conception MVC ASP.NET gratuit. La bibliothèque ASP.NET MVC vous permet de créer une application web professionnelle sans effectuer aucun travail.

J’ai décidé de ne pas utiliser un modèle à partir de la galerie de conception de MVC ASP.NET pour l’application Gestionnaire de contacts. Au lieu de cela, j’ai dû une conception personnalisée créée par une société spécialisée. Dans la deuxième partie de ce didacticiel, j’ai expliquer comment j’ai travaillé avec une société de conception Professionnel pour créer la conception finale de ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>La galerie de conception MVC ASP.NET

La galerie de conception MVC ASP.NET est une ressource gratuite fournie par Microsoft. La galerie de MVC ASP.NET se trouve à l’adresse suivante :

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La galerie de conception MVC ASP.NET héberge une collection de conceptions de site Web gratuit qui ont été créées spécifiquement pour l’utilisation dans un projet ASP.NET MVC. Conceptions sont chargées par les membres de la Communauté. Les visiteurs de la galerie peuvent voter pour leurs conceptions Favoris (voir Figure 2).


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figure 02**: la bibliothèque de présentations de MVC ASP.NET ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Alors que je rédige ce didacticiel, la conception plus populaires dans la galerie est une conception nommée octobre par David Hauser. Vous pouvez utiliser cette conception pour un projet ASP.NET MVC en effectuant les étapes suivantes :

1. Cliquez sur le **télécharger** bouton pour télécharger le fichier October.zip sur votre ordinateur.
2. Cliquez sur le fichier October.zip téléchargé, puis cliquez sur le **Unblock** bouton (voir Figure 3).
3. Décompressez le fichier dans un dossier nommé octobre.
4. Sélectionnez tous les fichiers à partir du dossier DesignTemplate contenu dans le dossier d’octobre, cliquez sur les fichiers et sélectionnez l’option de menu **copie**.
5. Cliquez sur le nœud de projet ContactManager dans la fenêtre Explorateur de solutions Visual Studio et sélectionnez l’option de menu **coller** (voir Figure 4).
6. Sélectionnez l’option de menu de Visual Studio **Édition, rechercher et remplacer, remplacement rapide** et remplacez *[Nomdemonprojet]* avec *ContactManager* (voir Figure 5).


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figure 03**: déblocage d’un fichier téléchargé à partir du web ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figure 04**: en remplaçant les fichiers dans l’Explorateur de solutions ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figure 05**: en remplaçant [nom_projet] avec ContactManager ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Après avoir effectué ces étapes, votre application web utilise la nouvelle conception. La page dans la Figure 6 illustre l’apparence de l’application Gestionnaire de contacts avec la conception d’octobre.


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figure 06**: ContactManager avec le modèle d’octobre ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Création d’une conception MVC ASP.NET personnalisés

La galerie de conception MVC ASP.NET a une bonne sélection de styles de conception différentes. La galerie vous offre un moyen simple de personnaliser l’apparence de vos applications ASP.NET MVC. Et, bien sûr, la galerie est le grand avantage d’être entièrement gratuit.

Toutefois, vous devrez peut-être créer une conception totalement unique pour votre site Web. Dans ce cas, il est judicieux pour travailler avec une société de conception de site Web. J’ai décidé d’adopter cette approche pour la conception de l’application Gestionnaire de contacts.

J’ai compressé le Gestionnaire de Contact à partir de l’itération #1 et envoyé le projet à la société de conception. Il ne sont pas propriétaire Visual Studio (dommage que leur !), mais que fonctionné t posent un problème. Ils ont été en mesure de télécharger Microsoft Visual Web Developer gratuitement à partir de la [ https://www.asp.net ](https://www.asp.net) site Web et ouvrez l’application Gestionnaire de contacts dans Visual Web Developer. Dans quelques jours, ils avaient produites la conception dans la Figure 7.


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figure 07**: ASP.NET MVC Contact Manager Design ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


La nouvelle conception est composé de deux fichiers principaux : un nouveau fichier de feuille de style en cascade et un nouveau fichier page maître de vue. Une page de vue maître contient la disposition et le contenu partagé pour les vues dans une application ASP.NET MVC. Par exemple, la page maître de vue inclut l’en-tête, onglets de navigation et un pied de page qui s’affichent dans la Figure 7. J’ai a remplacé la page maître existante Site.Master vue dans le dossier Views\Shared avec le nouveau fichier Site.Master de la société de conception,

La société a également créé une nouvelle feuille de style en cascade et un ensemble d’images. J’ai placé ces nouveaux fichiers dans le dossier de contenu et a remplacé le fichier Site.css existant. Vous devez placer tout le contenu statique dans le dossier de contenu.

Notez que la nouvelle conception pour le Gestionnaire de contacts inclut des images pour modifier et supprimer des contacts. Une image Edit et Delete s’affichent en regard de chaque contact dans la table HTML de contacts.

À l’origine, ces liens ont été rendus avec le code HTML. Application d’assistance de ActionLink() comme suit :

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

La méthode Html.ActionLink() ne prend pas en charge les images (la méthode HTML encode le texte du lien pour des raisons de sécurité). Par conséquent, j’ai remplacé les appels à Html.ActionLink() avec les appels à Url.Action() comme suit :

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

La méthode Html.ActionLink() restitue un lien HTML complet. La méthode Url.Action(), quant à eux, effectue le rendu uniquement l’adresse URL sans le &lt;un&gt; balise.

En outre, notez que la nouvelle conception inclut à la fois sélectionnés et des onglets. Par exemple, dans la Figure 8, le **créer un nouveau Contact** onglet est sélectionné et le **mes Contacts** onglet n’est pas sélectionnée.


[![La boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figure 08**: sélectionnés et onglets ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Pour prendre en charge le rendu à la fois sélectionnés et onglets, j’ai créé un programme d’assistance HTML personnalisé, nommé le MenuItemHelper. Cette méthode d’assistance affiche soit un &lt;li&gt; balise ou une &lt;classe de li = « selected »&gt; balise selon que le contrôleur actuel et l’action correspond au nom de contrôleur et action passé à l’application d’assistance. Le code pour le MenuItemHelper est contenu dans le Listing 1.

**Liste 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Le MenuItemHelper utilise en interne la classe TagBuilder pour générer le &lt;li&gt; balise HTML. La classe TagBuilder est une classe utilitaire très utile que vous pouvez utiliser chaque fois que vous avez besoin créer une nouvelle balise HTML. Il inclut des méthodes pour l’ajout d’attributs, l’ajout de classes CSS, générant des identifiants et modification de la balise s HTML interne.

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons amélioré la conception visuelle de notre application ASP.NET MVC. Tout d’abord, vous a présenté à la galerie de conception MVC ASP.NET. Vous avez appris à télécharger des modèles de conception gratuit à partir de la galerie de conception MVC ASP.NET que vous pouvez utiliser dans vos applications ASP.NET MVC.

Ensuite, nous avons abordé la façon dont vous pouvez créer une conception personnalisée en modifiant le fichier de feuille de style en cascade par défaut et le fichier de page de vue maître. Pour prendre en charge la nouvelle conception, nous avons dû apporter quelques changements mineurs à notre application de gestionnaire de contacts. Par exemple, nous avons ajouté une nouvelle assistance HTML nommée MenuItemHelper qui affiche les onglets sélectionnés et.

Dans l’itération suivante, nous attaquer le sujet très important de validation. Nous ajoutons code de validation à notre application afin qu’un utilisateur ne peut pas créer un contact sans fournir tout d’abord les valeurs requises par exemple une personne s et nom.

> [!div class="step-by-step"]
> [Précédent](iteration-1-create-the-application-cs.md)
> [Suivant](iteration-3-add-form-validation-cs.md)
