---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Ajout d’une vue | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: e3b923aa5572781261c5a75546700faf223703d4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834502"
---
<a name="adding-a-view"></a>Ajout d’une vue
====================
par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Dans cette section, nous allons examiner comment nous pouvons avoir notre classe HelloWorldController utiliser un fichier de modèle de vue pour encapsuler proprement génération réponses HTML à un client.

Nous allons commencer en utilisant un modèle de vue avec notre méthode de l’Index. Notre méthode est appelée Index et il se trouve dans le HelloWorldController. Actuellement, notre méthode Index() retourne une chaîne avec un message qui est codé en dur dans la classe de contrôleur.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Nous allons maintenant modifier la méthode Index au lieu de cela ressemble à ceci :

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Nous allons maintenant ajouter un modèle de vue à notre projet que nous pouvons utiliser pour notre méthode Index(). Pour ce faire, le bouton droit de votre souris quelque part au milieu de la méthode Index, puis cliquez sur Ajouter une vue...

![image](getting-started-with-mvc-part3/_static/image1.png)

Cela fera apparaître la boîte de dialogue « Ajouter une vue » nous fournit des options pour la façon dont nous voulons créer un modèle de vue peut être utilisé par notre méthode de l’Index. Pour l’instant, ne modifiez rien et cliquez simplement sur le bouton Ajouter.

[![Afficher la boîte de dialogue Ajouter](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Après avoir cliqué sur Ajouter, un nouveau dossier et un nouveau fichier apparaîtront dans le dossier de Solution, comme illustré ci-après. J’ai maintenant un dossier HelloWorld sous les vues et un fichier Index.aspx à l’intérieur de ce dossier.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Le fichier d’Index est également déjà ouvert et prêt pour l’édition. Ajouter du texte sous le premier &lt;h2&gt;Index&lt;/h2&gt; comme « Hello World ».

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Exécutez votre application et visitez [ `http://localhost:xx/HelloWorld` ](http://localhostxx) à nouveau dans votre navigateur. La méthode Index dans notre contrôleur dans cet exemple n’a pas d’effectuer le travail, mais n’a appelé « retour View() », ce qui indiqué que nous souhaitons utiliser un fichier de modèle de vue pour restituer une réponse au client. Car nous ne spécifie pas explicitement le nom du fichier de modèle de vue à utiliser, ASP.NET MVC par défaut à l’aide du fichier d’Affichage de Index.aspx dans le dossier \Views\HelloWorld. Maintenant, nous voyons la chaîne que nous avons codées en dur dans notre vue.

[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Il semble assez bonne. Toutefois, notez que que titre du navigateur indique « Index » et que le titre big sur la page indique « Mon Application MVC ». Nous allons modifier ceux.

### <a name="changing-views-and-master-pages"></a>Modification des vues et des Pages maîtres

Tout d’abord, nous allons modifier le texte « Mon Application MVC ». Ce texte est partagé et apparaît sur chaque page. Il apparaît réellement dans le seul endroit dans notre code, même s’il est sur chaque page dans notre application. Accédez au dossier /Views/Shared dans l’Explorateur de solutions et ouvrez le fichier Site.Master. Ce fichier s’appelle une Page maître et il est le partagé « shell » qui utilisent de nos autres pages.

Notez que du texte indiquant « MainContent » de ContentPlaceholder dans ce fichier.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Cet espace réservé est où toutes les pages que vous créez seront affichera, « encapsulées » dans la page maître. Essayez de modifier le titre, puis exécutez votre application, vous accédez à plusieurs pages. Vous remarquerez que votre une répercussion sur plusieurs pages.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Maintenant chaque page aura l’en-tête principal - c'est-à-dire H1 - de « Mon MVC Movie Application ». Qui gère le texte blanc en haut, il est partagé entre toutes les pages.

Voici la page Site.Master dans son intégralité avec notre titre a été modifié :

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Maintenant, nous allons modifier le titre de la page d’Index.

Ouvrez /HelloWorld/Index.aspx. Il existe deux emplacements à modifier. Tout d’abord, le titre qui apparaît dans le titre du navigateur, puis l’en-tête secondaire - c'est-à-dire H2 - également. Nous voulons chacune légèrement différente afin que vous puissiez voir quel morceau de code modifie quelle partie de l’application.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Exécutez votre application et visitez /Movies. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont été modifiés. Il est facile d’effectuer des modifications importantes dans votre application avec des modifications mineures à votre vue.

[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nos quelques « données » (dans ce cas « Hello World ! » message) était difficile codé cependant. Nous avons V (vues) et nous avons C (contrôleurs), mais encore aucune M (modèle). Nous allons bientôt Guide pas à créer une base de données et récupérer des données de modèle à partir de celui-ci.

## <a name="passing-a-viewmodel"></a>En passant un ViewModel

Avant de nous accédez à une base de données et parler de modèles, cependant, tout d’abord parlons de « ViewModels. » Il s’agit d’objets qui représentent ce qui nécessite un modèle de vue pour restituer une réponse HTML à un client. Ils sont généralement créés passé par une classe de contrôleur à un modèle de vue et doivent contenir uniquement les données nécessitant le modèle de vue - et pas plus.

Précédemment avec notre exemple HelloWorld, notre méthode d’action Welcome() ont eu un nom et un paramètre numTimes et les résultats dans le navigateur. Au lieu que le contrôleur de continuer à afficher cette réponse directement, nous allons apporter à la place une petite classe pour contenir ces données et le passer ensuite au fil à un modèle de vue à restituer dans la réponse HTML à l’utiliser. De cette façon, le contrôleur s’attache à une chose et le modèle de vue autre – nous permet de mettre à jour de « séparation claire des préoccupations » au sein de notre application.

Revenez dans le fichier HelloWorldController.cs et ajoutez une nouvelle classe « WelcomeViewModel » et modifier la méthode Bienvenue à l’intérieur de votre contrôleur. Voici le HelloWorldController.cs complète avec la nouvelle classe dans le même fichier.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Même s’il est sur plusieurs lignes, notre méthode Bienvenue n’existe que deux instructions de code. La première instruction emballe nos deux paramètres dans un objet ViewModel et la deuxième passe de l’objet résultant dans la vue.

Nous devons à présent d’un modèle de vue Bienvenue ! Cliquez avec le bouton droit dans la méthode de bienvenue et sélectionnez Ajouter une vue. Cette fois-ci, nous allons vérifier « Créer une vue fortement typée » et sélectionnez notre classe WelcomeViewModel dans la liste déroulante. Cette nouvelle vue sera uniquement savoir sur WelcomeViewModels et aucun autre type d’objets.

> *Remarque : Vous devez avoir compilé une fois après l’ajout de votre WelcomeViewModel pour s’affichent dans la liste déroulante.*


Voici à quoi doit ressembler votre boîte de dialogue Ajouter une vue. Cliquez sur le bouton Ajouter. ![Ajouter la que vue encerclé](getting-started-with-mvc-part3/_static/image10.png)

Ajoutez le code suivant sous le &lt;h2&gt; dans votre nouveau Welcome.aspx. Nous allons effectuer une boucle et saluez aussi souvent que l’utilisateur dit que nous devrions !

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Notez également que lorsque vous tapez qui car nous dit à cette vue concernant le WelcomeViewModel (ils sont mariés, n’oubliez pas ?) que nous obtenons Intellisense utile chaque fois que nous faisons référence notre objet de modèle, comme illustré dans la capture d’écran ci-dessous :

[![Code Source de NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Exécutez votre application et visitez `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` à nouveau. Maintenant nous redirigeons les données à partir de l’URL, il est passé automatiquement dans notre contrôleur, notre contrôleur empaquette les données dans un ViewModel et passe cet objet vers notre affichage. La vue qu’affiche les données au format HTML à l’utilisateur.

[![Bienvenue - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

C’était une sorte de « M » pour le modèle, mais pas le type de base de données. Voyons ce que nous avez appris et créer une base de données de films.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part2.md)
> [Suivant](getting-started-with-mvc-part4.md)
