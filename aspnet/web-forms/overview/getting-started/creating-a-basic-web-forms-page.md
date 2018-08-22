---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Création d’une base ASP.NET 4.5 Web Forms Page dans Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 3efd772854cb97c805d61b63c03f6bae031619de
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827895"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Création d’une base ASP.NET 4.5 Web Forms Page dans Visual Studio 2013
====================
par [Erik Reitan](https://github.com/Erikre)

Cette procédure pas à pas vous fournit une introduction à l’environnement de développement Web dans [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) et dans [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Cette procédure pas à pas vous guide à travers la création d’une page Web Forms ASP.NET simple et illustre les techniques de base de création d’une page, d’ajout de contrôles et d’écriture de code.

Cette procédure pas à pas décrit notamment les tâches suivantes :

- Création d’un projet d’application Web Forms fichier système.
- Familiarisation avec Visual Studio.
- Création d’une page ASP.NET.
- Ajout de contrôles.
- Ajout de gestionnaires d’événements.
- En cours d’exécution et testez une page à partir de Visual Studio.

## <a name="prerequisites"></a>Prérequis


Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Le .NET Framework est installé automatiquement. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web seront souvent être appelé Visual Studio tout au long de cette série de didacticiels.  
    >   
    > Si vous utilisez Visual Studio, cette procédure pas à pas suppose que vous avez choisi le **développement Web** collection de paramètres de la première fois que vous avez démarré Visual Studio. Pour plus d’informations, consultez [Comment : sélectionner des paramètres d’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Création d’un projet d’application Web et une Page

<a id="sectionToggle0"></a>

Dans cette partie de la procédure pas à pas, vous créez un projet d’application Web et ajoutez une nouvelle page à celui-ci. Vous allez également ajouter du texte HTML et exécuter de la page dans votre navigateur.

### <a name="to-create-a-web-application-project"></a>Pour créer un projet d’application Web

1. Ouvrez Microsoft Visual Studio.
2. Dans le menu **Fichier**, sélectionnez **Nouveau projet**.  
    ![Menu fichier](creating-a-basic-web-forms-page/_static/image1.png)

    La boîte de dialogue **Nouveau projet** s’affiche.
3. Sélectionnez le **modèles**  - &gt; **Visual C#**  - &gt; **Web** groupe de modèles sur la gauche.
4. Choisissez le **Application Web ASP.NET** modèle dans la colonne centrale.
5. Nommez votre projet ***BasicWebApp*** et cliquez sur le **OK** bouton.   
![Boîte de dialogue Nouveau projet](creating-a-basic-web-forms-page/_static/image2.png)
6. Ensuite, sélectionnez le **Web Forms** modèle et cliquez sur le **OK** bouton pour créer le projet.  
![Boîte de dialogue Nouveau projet ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio crée un nouveau projet qui inclut les fonctionnalités prégénérées basée sur le modèle Web Forms. Il vous fournit non seulement un *Home.aspx* page, un *About.aspx* page, un *Contact.aspx* page, mais inclut également des fonctionnalités d’appartenance qui enregistre les utilisateurs et l’enregistre leurs informations d’identification afin qu’ils peuvent se connecter à votre site Web. Lorsqu’une nouvelle page est créée, par défaut, Visual Studio affiche la page dans **Source** vue, où vous pouvez voir les éléments HTML de la page. L’illustration suivante montre ce que vous verriez dans **Source** afficher si vous avez créé une page Web nommée *BasicWebApp.aspx*.  
    ![Mode Source](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Visite guidée de l’environnement de développement Web Visual Studio


Avant de continuer en modifiant la page, il est utile pour vous familiariser avec l’environnement de développement Visual Studio. L’illustration suivante vous montre les fenêtres et les outils qui sont disponibles dans Visual Studio et Visual Studio Express pour le Web.

> [!NOTE] 
> 
> Ce diagramme illustre les emplacements de fenêtre et des fenêtres par défaut. Le **vue** menu vous permet d’afficher des fenêtres supplémentaires et réorganisez et redimensionnez windows pour l’adapter à vos préférences. Si les modifications ont déjà été apportées à la disposition de fenêtre, ce que vous voyez ne correspondre pas l’illustration.


 L’environnement Visual Studio

![Environnement de Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Familiarisez-vous avec le Concepteur Web

Examinez l’illustration ci-dessus et correspond au texte à la liste suivante, windows et les outils qui décrit les plus couramment utilisés. (Pas toutes les fenêtres et tous les outils que vous voyez sont répertoriés ici, uniquement ceux marqués dans l’illustration précédente.)

- Barres d’outils. Fournir des commandes de mise en forme de texte, recherche de texte et ainsi de suite. Certaines barres d’outils sont disponibles uniquement lorsque vous travaillez dans **conception** vue.
- **L’Explorateur de solutions** fenêtre. Affiche les fichiers et dossiers dans votre application Web.
- Fenêtre de document. Affiche les documents que vous travaillez dans des fenêtres à onglets. Vous pouvez basculer entre les documents en cliquant sur les onglets.
- **Propriétés** fenêtre. Vous permet de modifier les paramètres pour la page, les éléments HTML, les contrôles et les autres objets.
- Afficher les onglets. Présenter différentes vues du même document. **Conception** vue est une surface d’édition proche du WYSIWYG. **Source** affichage est l’éditeur HTML de la page. **Fractionnement** vue affiche à la fois le **conception** vue et le **Source** le document. Vous allez travailler avec les **conception** et **Source** vues plus loin dans cette procédure pas à pas. Si vous préférez ouvrir les pages Web dans **conception** afficher, dans le **outils** menu, cliquez sur **Options**, sélectionnez le **Concepteur HTML** nœud, puis remplacez le **Démarrer les Pages en** option.
- **Boîte à outils**. Fournit des contrôles et des éléments HTML que vous pouvez faire glisser sur votre page. **Boîte à outils** éléments sont regroupés selon leur fonction.
- S **erver Explorer**. Affiche les connexions de base de données. Si l’Explorateur de serveurs n’est pas visible, dans le menu Affichage, cliquez sur Explorateur de serveurs.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Création d’une Page ASP.NET Web Forms


Lorsque vous créez une nouvelle application Web Forms à l’aide du **Application Web ASP.NET** modèle de projet, Visual Studio ajoute une page ASP.NET (page Web Forms) nommée *Default.aspx*, ainsi que plusieurs autres fichiers et les dossiers. Vous pouvez utiliser la *Default.aspx* page comme page d’accueil pour votre application Web. Toutefois, pour cette procédure pas à pas, vous créez et travailler avec une nouvelle page.

### <a name="to-add-a-page-to-the-web-application"></a>Pour ajouter une page à l’application Web


1. Fermer le *Default.aspx* page. Pour ce faire, cliquez sur l’onglet qui affiche le nom de fichier, puis sur l’option ferme.
2. Dans **l’Explorateur de solutions**, cliquez sur le nom d’application Web (dans ce didacticiel est le nom de l’application **BasicWebSite**), puis cliquez sur **ajouter**  - &gt; **Un nouvel élément**.   
La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
3. Sélectionnez le **Visual C#**  - &gt; **Web** groupe de modèles sur la gauche. Ensuite, sélectionnez **Web Form** à partir du milieu de liste et nommez-le *PremièrePageWeb.aspx*.   
    ![Ajouter un nouvel élément](creating-a-basic-web-forms-page/_static/image6.png)
4. Cliquez sur **ajouter** pour ajouter la page web à votre projet.  
Visual Studio crée la page et l’ouvre.


### <a name="adding-html-to-the-page"></a>Ajout de code HTML à la Page


Dans cette partie de la procédure pas à pas, vous allez ajouter du texte statique à la page.

### <a name="to-add-text-to-the-page"></a>Pour ajouter du texte à la page


1. En bas de la fenêtre de document, cliquez sur le **conception** tab pour passer à **conception** vue.

    Mode Design affiche la page en cours d’une manière similaire à WYSIWYG. À ce stade, il est inutile n’importe quel texte ou les contrôles dans la page, par conséquent, la page est vide à l’exception d’une ligne en pointillé qui décrit un rectangle. Ce rectangle représente un **div** élément sur la page.
2. Cliquez à l’intérieur du rectangle qui est décrite par une ligne en pointillés.
3. Type **Bienvenue dans Visual Web Developer** et appuyez sur **entrée** à deux reprises.

    L’illustration suivante montre le texte que vous avez tapé dans **conception** vue.

    ![Texte de bienvenue en mode conception](creating-a-basic-web-forms-page/_static/image7.png "texte de bienvenue en mode Design")
4. Basculez vers **Source** vue.

    Vous pouvez voir le code HTML dans **Source** vue que vous avez créé lorsque vous avez tapé dans **conception** vue.  
    ![Page Web avec du texte statique](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Exécution de la Page


Avant de continuer en ajoutant des contrôles à la page, vous pouvez l’exécuter.

### <a name="to-run-the-page"></a>Pour exécuter la page


1. Dans **l’Explorateur de solutions**, avec le bouton droit *PremièrePageWeb.aspx* et sélectionnez **définir comme Page de démarrage**.
2. Appuyez sur **CTRL + F5** pour exécuter la page.

    La page s’affiche dans le navigateur. Bien que la page que vous avez créé a une extension de nom de fichier de *.aspx*, il s’exécute actuellement comme n’importe quelle page HTML.

    Pour afficher une page dans le navigateur vous pouvez également cliquer sur la page dans **l’Explorateur de solutions** et sélectionnez **afficher dans le navigateur**.
3. Fermez le navigateur pour arrêter l’application Web.


## <a name="adding-and-programming-controls"></a>Ajout et la programmation de contrôles


<a id="sectionToggle1"></a>

Vous allez maintenant ajouter des contrôles serveur à la page. Les contrôles de serveur, tels que des boutons, des étiquettes, des zones de texte et d’autres contrôles familiers, fournissent des capacités de traitement de formulaire standard pour vos pages Web Forms. Toutefois, vous pouvez programmer les contrôles avec le code qui s’exécute sur le serveur, plutôt que le client.

Vous ajouterez un [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôle, un [zone de texte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) contrôle et un [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) le contrôle à la page et d’écrire du code pour gérer les [cliquez sur](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) événement pour le [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôle.

### <a name="to-add-controls-to-the-page"></a>Pour ajouter des contrôles à la page


1. Cliquez sur le **conception** tab pour passer à **conception** vue.
2. Placez le point d’insertion à la fin de la **Bienvenue dans Visual Web Developer** texte et appuyez sur **entrée** cinq fois ou plus pour libérer de l’espace dans le **div** boîte de l’élément.
3. Dans le **boîte à outils**, développez le **Standard** groupe s’il n’est pas déjà développé.  
Notez que vous devrez peut-être développer le **boîte à outils** fenêtre sur la gauche pour l’afficher.
4. Faites glisser un [zone de texte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) contrôler sur la page et déposez-le au milieu de la **div** boîte de l’élément qui a **Bienvenue dans Visual Web Developer** dans la première ligne.
5. Faites glisser un [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôler sur la page et déposez-la à droite de la [zone de texte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) contrôle.
6. Faites glisser un [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôler sur la page et déposez-le sur une ligne distincte ci-dessous le [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôle.
7. Placez le point d’insertion ci-dessus le [zone de texte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) contrôler et tapez **Entrez votre nom :** .

    Ce texte HTML statique est la légende pour le [zone de texte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) contrôle. Vous pouvez combiner HTML statique et des contrôles serveur sur la même page. L’illustration suivante montre comment les trois contrôles apparaissent dans **conception** vue.

    ![Trois contrôles en mode Design](creating-a-basic-web-forms-page/_static/image9.png "trois contrôles en mode Design")


### <a name="setting-control-properties"></a>Définition des propriétés de contrôle


Visual Studio vous propose différentes façons de définir les propriétés des contrôles sur la page. Dans cette partie de la procédure pas à pas, vous allez définir des propriétés dans les deux **conception** vue et **Source** vue.

### <a name="to-set-control-properties"></a>Pour définir les propriétés de contrôle


1. Tout d’abord, afficher le **propriétés** windows en sélectionnant à partir de la **vue** menu -&gt; **Windows autres**  - &gt; **Fenêtre de propriétés**. Vous pouvez également sélectionner **F4** pour afficher le **propriétés** fenêtre.
2. Sélectionnez le [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôle, puis, dans le **propriétés** fenêtre, définissez la valeur de **texte** à **nom d’affichage**. Le texte que vous avez entré s’affiche sur le bouton dans le concepteur, comme indiqué dans l’illustration suivante.

    ![Définir le texte du bouton](creating-a-basic-web-forms-page/_static/image10.png "texte du bouton Définir")
3. Basculez vers **Source** vue.

    **Source** vue affiche le HTML de la page, y compris les éléments que Visual Studio a créé pour les contrôles serveur. Les contrôles sont déclarés à l’aide de la syntaxe de type HTML, à ceci près que les balises utilisent le préfixe **asp :** et incluent l’attribut **runat =&quot;server&quot;**.

    Propriétés du contrôle sont déclarées en tant qu’attributs. Par exemple, lorsque vous définissez la [texte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) propriété pour le [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôler, à l’étape 1, vous avez réellement défini le **texte** attribut dans le balisage du contrôle.

    > [!NOTE] 
    > 
    > Tous les contrôles sont trouvent dans un **formulaire** élément, qui possède également l’attribut **runat =&quot;server&quot;**. Le **runat =&quot;server&quot;**  attribut et la **asp :** préfixe des balises de contrôle marquent les contrôles afin qu’ils soient traités par ASP.NET sur le serveur lorsque la page s’exécute. Code en dehors de **&lt;forment runat =&quot;server&quot; &gt;** et **&lt;script runat =&quot;server&quot; &gt;** éléments est envoyé sans modification vers le navigateur, c’est pourquoi le code ASP.NET doit être à l’intérieur d’un élément dont la balise ouvrante contient le **runat =&quot;server&quot;**  attribut.
4. Ensuite, vous allez ajouter une propriété supplémentaire pour le [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôle. Placez le point d’insertion directement après **asp : Label** dans le **&lt;asp : Label&gt;** balise, puis appuyez sur **espace**.

    Une liste déroulante apparaît et affiche la liste des propriétés disponibles, vous pouvez définir pour un [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôle. Cette fonctionnalité, appelée **IntelliSense**, vous aide dans **Source** vue avec la syntaxe des contrôles serveur, les éléments HTML et les autres éléments dans la page. L’illustration suivante montre le **IntelliSense** liste déroulante pour le [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôle.

    ![Attributs IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "attributs IntelliSense")
5. Sélectionnez **ForeColor** puis tapez un signe égal.

    IntelliSense affiche une liste de couleurs.

    > [!NOTE] 
    > 
    > Vous pouvez afficher un **IntelliSense** liste déroulante, à tout moment en appuyant sur **CTRL + J** lors de l’affichage de code.
6. Sélectionnez une couleur pour le **[étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** texte du contrôle. Veillez à que sélectionner une couleur sombre suffisamment pour lire sur un arrière-plan blanc.

    Le **ForeColor** attribut se termine avec la couleur que vous avez sélectionnées, y compris les guillemets fermants.


### <a name="programming-the-button-control"></a>Programmation du contrôle de bouton


Pour cette procédure pas à pas, vous allez écrire du code qui lit le nom que l’utilisateur entre dans la zone de texte et affiche ensuite le nom dans la [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôle.

### <a name="add-a-default-button-event-handler"></a>Ajoutez un gestionnaire d’événements de bouton par défaut


1. Basculez vers **conception** vue.
2. Double-cliquez sur le [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) contrôle.

    Par défaut, Visual Studio bascule vers un fichier code-behind et crée un gestionnaire d’événements squelette pour le [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) l’événement par défaut du contrôle, le [cliquez sur](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) événement. Le fichier code-behind sépare votre balisage de l’interface utilisateur (par exemple, HTML) à partir de votre code serveur (tel que c#).   
   Le curseur est positionné à ajouté du code pour ce gestionnaire d’événements.

    > [!NOTE] 
    > 
    > Double-cliquez sur un contrôle dans **conception** vue est simplement une des différentes méthodes que vous pouvez créer des gestionnaires d’événements.
3. À l’intérieur de la **Button1\_cliquez sur** Gestionnaire d’événements, tapez **Label1** suivi d’un point (**.**).

    Lorsque vous tapez le point après le **ID** de l’étiquette (**Label1**), Visual Studio affiche une liste des membres disponibles pour le [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôler, comme indiqué dans l’exemple suivant illustration. Une méthode couramment une propriété de membre ou un événement.

    ![IntelliSense en mode Code](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense en mode Code")
4. Terminer la **cliquez sur** Gestionnaire d’événements pour le bouton afin qu’il s’affiche comme illustré dans l’exemple de code suivant.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Revenez à l’affichage le **Source** affichage de votre balisage HTML en double-cliquant sur *PremièrePageWeb.aspx* dans le **l’Explorateur de solutions** et en sélectionnant **vue Balisage**.
6. Faites défiler vers le **&lt;asp : Button&gt;** élément. Notez que le **&lt;asp : Button&gt;** élément a maintenant l’attribut **onclick =&quot;Button1\_cliquez sur&quot;**.

    Cet attribut lie le bouton [cliquez sur](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) événement à la méthode de gestionnaire que vous avez ajouté à l’étape précédente.

    Méthodes de gestionnaire d’événements peuvent avoir n’importe quel nom ; le nom que vous voyez est le nom par défaut créé par Visual Studio. Le point important est que le nom utilisé pour le **OnClick** attribut dans le code HTML doit correspondre au nom d’une méthode définie dans le code-behind.


### <a name="running-the-page"></a>Exécution de la Page


Vous pouvez maintenant tester les contrôles serveur dans la page.

### <a name="to-run-the-page"></a>Pour exécuter la page


1. Appuyez sur **CTRL + F5** pour exécuter la page dans le navigateur. Si une erreur se produit, vérifiez à nouveau les étapes ci-dessus.
2. Entrez un nom dans la zone de texte et cliquez sur le **surnom** bouton.

    Le nom que vous avez entré est affiché dans le [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôle. Notez que lorsque vous cliquez sur le bouton, la page est publiée sur le serveur Web. ASP.NET puis recrée la page, exécute votre code (dans ce cas, le [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) du contrôle [cliquez sur](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) exécutions de gestionnaire d’événements), puis envoie la nouvelle page au navigateur. Si vous regardez la barre d’état dans le navigateur, vous pouvez voir que la page fait un aller-retour vers le serveur Web chaque fois que vous cliquez sur le bouton.
3. Dans le navigateur, affichez la source de la page en cours d’exécution en cliquant sur la page et en sélectionnant **afficher la source**.

    Dans le code source de la page, vous voyez HTML sans aucun code serveur. Plus précisément, vous ne voyez pas le **&lt;asp :&gt;** les éléments que vous utilisiez dans **Source** vue. Lorsque la page s’exécute, ASP.NET traite les contrôles serveur et restitue des éléments HTML à la page qui effectuent les fonctions qui représentent le contrôle. Par exemple, le **&lt;asp : Button&gt;** contrôle est restitué en tant que le code HTML **&lt;type d’entrée =&quot;soumettre&quot; &gt;** élément.
4. Fermez le navigateur.


## <a name="working-with-additional-controls"></a>Utilisation des contrôles supplémentaires

<a id="sectionToggle2"></a>

Dans cette partie de la procédure pas à pas, vous allez travailler avec les [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôle, qui affiche les dates mois à la fois. Le [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôle est un contrôle plus complexe que le bouton, la zone de texte et l’étiquette que vous avez travaillé avec et illustre quelques fonctions supplémentaires de contrôles serveur.

Dans cette section, vous allez ajouter un [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) le contrôle à la page et de mettre en forme.

### <a name="to-add-a-calendar-control"></a>Pour ajouter un contrôle de calendrier


1. Dans Visual Studio, basculez vers **conception** vue.
2. À partir de la **Standard** section de la **boîte à outils**, faites glisser un [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôler sur la page et déposez-la en dessous le **div** élément qui contient les autres contrôles.

    Panneau des balises actives du calendrier s’affiche. Le panneau affiche les commandes qui vous permettent d’effectuer les tâches les plus courantes pour le contrôle sélectionné. L’illustration suivante montre le [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôler le rendu dans **conception** vue.

    ![Contrôle en mode Design du calendrier](creating-a-basic-web-forms-page/_static/image13.png "contrôle en mode Design du calendrier")
3. Dans le panneau des balises actives, choisissez **mise en forme automatique**.

    Le **mise en forme automatique** boîte de dialogue s’affiche, qui vous permet de sélectionner un schéma de mise en forme pour le calendrier. L’illustration suivante montre le **mise en forme automatique** boîte de dialogue pour le [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôle.

    ![Forme automatique, boîte de dialogue (contrôle Calendar)](creating-a-basic-web-forms-page/_static/image14.png "boîte de dialogue Mise en forme automatique (contrôle Calendar)")
4. À partir de la **sélectionner un schéma** , sélectionnez **Simple** puis cliquez sur **OK**.
5. Basculez vers **Source** vue.

    Vous pouvez voir le **&lt;asp : Calendar&gt;** élément. Cet élément est beaucoup plus long que les éléments des contrôles simples que vous avez créé précédemment. Il inclut également des sous-éléments, tel que  **&lt;WeekEndDayStyle&gt;**, qui représentent les différents paramètres de mise en forme. L’illustration suivante montre le [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) dans contrôler **Source** vue. (La balise exacte que vous voyez dans **Source** vue peut-être différer légèrement de l’illustration.)

    ![Contrôle dans la vue de Source de calendrier](creating-a-basic-web-forms-page/_static/image15.png "Calendar control dans la vue de Source")


### <a name="programming-the-calendar-control"></a>Programmation du contrôle de calendrier


Dans cette section, vous programmerez le [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôle pour afficher la date actuellement sélectionnée.

### <a name="to-program-the-calendar-control"></a>Pour programmer le contrôle Calendar


1. Dans **conception** , double-cliquez sur le [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôle.

    Un nouveau gestionnaire d’événements est créé et affiché dans le fichier code-behind nommé *FirstWebPage.aspx.cs*.
2. Terminer la [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) Gestionnaire d’événements par le code suivant.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Le code ci-dessus définit le texte du contrôle label à la date sélectionnée du contrôle calendar.


### <a name="running-the-page"></a>Exécution de la Page


Vous pouvez maintenant tester le calendrier.

### <a name="to-run-the-page"></a>Pour exécuter la page


1. Appuyez sur **CTRL + F5** pour exécuter la page dans le navigateur.
2. Cliquez sur une date dans le calendrier.

    La date que vous avez cliqué est affichée dans le [étiquette](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) contrôle.
3. Dans le navigateur, affichez le code source pour la page.

    Notez que le [calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) contrôle a été restitué à la page comme un **table**, avec chaque jour comme un **td** élément.
4. Fermez le navigateur.


## <a name="next-steps"></a>Étapes suivantes


<a id="nextStepsToggle"></a>

Cette procédure pas à pas a illustré les fonctionnalités de base du Concepteur de page de Visual Studio. Maintenant que vous comprenez comment créer et modifier une page Web Forms dans Visual Studio, vous souhaiterez Explorer d’autres fonctionnalités. Par exemple, vous souhaiterez peut-être effectuer les opérations suivantes :

- En savoir plus sur Web Forms ASP.NET en suivant la série de didacticiels pas à pas [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- En savoir plus sur les feuilles de style en cascade (CSS). Pour plus d’informations, consultez [Working with CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).
