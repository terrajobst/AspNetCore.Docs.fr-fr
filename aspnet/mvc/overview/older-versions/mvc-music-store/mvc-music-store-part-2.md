---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Partie 2 : Contrôleurs | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 2 couvre les contrôleurs.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841093"
---
<a name="part-2-controllers"></a>Partie 2 : contrôleurs
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 2 couvre les contrôleurs.


Avec les infrastructures web traditionnelles, les URL entrantes sont généralement mappés à des fichiers sur disque. Par exemple : une requête pour une URL semblable à « / Products.aspx » ou « /.PHP » peut être traitée par un fichier « Products.aspx » ou «.php ».

Les infrastructures MVC Web mappent des URL au code serveur d’une manière légèrement différente. Au lieu de mapper des URL entrantes aux fichiers, ils mappent à la place des URL aux méthodes sur les classes. Ces classes sont appelées « Contrôleurs » et ils sont responsables du traitement des requêtes HTTP entrantes, gestion des entrées utilisateur, de récupérer et de l’enregistrement des données et de déterminer la réponse à envoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers un autre URL, etc.).

## <a name="adding-a-homecontroller"></a>Ajout d’un HomeController

Nous allons commencer notre application de Store de musique MVC en ajoutant une classe de contrôleur qui gérera les URL pour la page d’accueil de notre site. Nous allons suivre les conventions d’affectation de noms par défaut d’ASP.NET MVC et l’appeler HomeController.

Cliquez sur le dossier « Contrôleurs » dans l’Explorateur de solutions et sélectionnez « Ajouter », puis la commande « Contrôleur... » :

![](mvc-music-store-part-2/_static/image1.jpg)

Cela fera apparaître la boîte de dialogue « Ajouter un contrôleur ». Nommez le contrôleur « HomeController » et appuyez sur le bouton Ajouter.

![](mvc-music-store-part-2/_static/image1.png)

Cela va créer un nouveau fichier, HomeController.cs, avec le code suivant :

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Pour démarrer la façon la plus simple que possible, nous allons remplacer la méthode Index avec une méthode simple qui retourne simplement une chaîne. Nous allons apporter deux modifications :

- Modifier la méthode pour retourner une chaîne au lieu d’une classe ActionResult
- Modifier l’instruction return pour retourner « Hello à partir de Home »

La méthode doit maintenant ressembler à ceci :

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Exécution de l'application

Maintenant nous allons exécuter le site. Nous pouvons démarrer notre serveur web et testez le site en utilisant l’une des opérations suivantes :

- Choisissez l’élément de menu Débogage ⇨ démarrer le débogage
- Cliquez sur le bouton de flèche verte dans la barre d’outils ![](mvc-music-store-part-2/_static/image2.jpg)
- Utilisez le raccourci clavier, F5.

En utilisant l’une des étapes ci-dessus compilera notre projet et forcer le serveur de développement ASP.NET qui est intégrée à Visual Web Developer pour démarrer. Une notification s’affiche dans l’angle inférieur de l’écran pour indiquer que le serveur de développement ASP.NET a démarré et affiche le numéro de port qu’il s’exécute sous.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer puis ouvre automatiquement une fenêtre de navigateur dont l’URL pointe vers notre serveur web. Cela nous permettra de tester rapidement notre application web :

![](mvc-music-store-part-2/_static/image3.png)

OK, ce qui a été assez rapide : nous avons créé un nouveau site Web, ajouté une fonction de trois lignes, et nous avons le texte dans un navigateur. Pas de fusée science, mais c’est un début.

*Remarque : Visual Web Developer inclut le serveur de développement ASP.NET, qui exécutera votre site Web sur un nombre aléatoire gratuit « port ». Dans la capture d’écran ci-dessus, le site est en cours d’exécution à `http://localhost:26641/`, il utilise le port 26641. Votre numéro de port sera différent. Lorsque nous parlons /Store/Browse like de l’URL dans ce didacticiel, qui passe une fois le numéro de port. En supposant qu’un numéro de port de 26641, accédez à/Store/Parcourir signifiera accédant à `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Ajout d’un StoreController

Nous avons ajouté un HomeController simple qui implémente la Page d’accueil de notre site. Nous allons maintenant ajouter un autre contrôleur que nous allons utiliser pour implémenter les fonctionnalités de navigation de notre magasin de musique. Notre contrôleur store prendra en charge trois scénarios :

- Une page de liste de genres de musique dans notre magasin de musique
- Une page de navigation qui répertorie tous les albums de musique d’un genre particulier
- Une page de détails qui affiche des informations sur un album musical spécifique

Nous allons commencer en ajoutant une nouvelle classe StoreController... Si vous n’avez pas déjà, arrêtez l’exécution de l’application soit par la fermeture du navigateur ou en sélectionnant l’élément de menu Débogage ⇨ arrêter le débogage.

Ajoutez maintenant un nouveau StoreController. Comme nous l’avons fait avec le HomeController, nous aborderons ce en cliquant sur le dossier « Contrôleurs » dans l’Explorateur de solutions et en choisissant Add -&gt;élément de menu de contrôleur

![](mvc-music-store-part-2/_static/image4.png)

Notre nouvelle StoreController a déjà une méthode « Index ». Nous allons utiliser cette méthode « Index » pour implémenter notre page de liste qui répertorie tous les genres dans notre magasin de musique. Nous allons également ajouter deux méthodes supplémentaires pour implémenter les deux autres scénarios nous voulons que notre StoreController à gérer : Parcourir et détails.

Ces méthodes (Index, parcourir et Details) dans notre contrôleur sont appelées « Contrôleur Actions », et que vous avez déjà vu avec la méthode d’action HomeController.Index (), leur travail consiste à répondre aux demandes d’URL et déterminer (en règle générale) le contenu doit être renvoyé au navigateur ou de l’utilisateur qui a appelé l’URL.

Nous allons commencer notre implémentation StoreController en modifiant la méthode theIndex() pour retourner la chaîne « Hello de Store.Index() » et nous allons ajouter des méthodes similaires pour Browse() et Details() :

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Exécuter à nouveau le projet et parcourir les URL suivantes :

- / Store
- / Store/Parcourir
- / Store/détails

L’accès à ces URL pour appeler les méthodes d’action dans notre contrôleur et renvoyer des réponses de la chaîne :

![](mvc-music-store-part-2/_static/image5.png)

C’est génial, mais il s’agit de chaînes constantes uniquement. Nous allons rendre dynamique, afin de tirer des informations à partir de l’URL et affichez-la dans la sortie de page.

Tout d’abord, nous allons modifier la méthode d’action Parcourir pour récupérer une valeur de chaîne de requête dans l’URL. Pour ce faire, nous pouvons ajouter un paramètre de « genre » à notre méthode d’action. Lorsque nous faisons cela ASP.NET MVC passe automatiquement les paramètres post chaîne de requête ou un formulaire nommés « genre » à notre méthode d’action lorsqu’il est appelé.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Remarque : Nous utilisons la méthode d’utilitaire HttpUtility.HtmlEncode à assainir la saisie utilisateur. Cela empêche les utilisateurs d’injecter du Javascript dans notre vue avec un lien comme /Store/Browse ? Genre =&lt;script&gt;Window.location = 'http://hackersite.com'&lt;/script&gt;.*

Maintenant nous allons accédez à/Store/Parcourir ? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Nous allons ensuite modifier l’action de détails pour lire et afficher un paramètre d’entrée nommé ID. Contrairement à notre méthode précédente, nous ne sont pas incorporer la valeur d’ID comme paramètre de chaîne de requête. Nous allons à la place l’incorporer directement dans l’URL proprement dite. Par exemple : /Store/Details/5.

ASP.NET MVC vous permet de nous le faire facilement sans avoir à configurer quoi que ce soit. Convention de routage par défaut de ASP.NET MVC consiste à traiter le segment d’URL après le nom de la méthode d’action en tant que paramètre nommé « ID ». Si votre méthode d’action possède un paramètre nommé ID puis ASP.NET MVC automatiquement passera le segment d’URL pour vous en tant que paramètre.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Exécutez l’application et accédez à /Store/Details/5 :

![](mvc-music-store-part-2/_static/image7.png)

Récapitulons ce que nous avons faites jusqu'à présent :

- Nous avons créé un nouveau projet ASP.NET MVC dans Visual Web Developer
- Nous avons vu la structure de dossiers de base d’une application ASP.NET MVC
- Nous avons appris à exécuter notre site Web à l’aide du serveur de développement ASP.NET
- Nous avons créé deux classes de contrôleur : un HomeController et un StoreController
- Nous avons ajouté des méthodes d’Action à nos contrôleurs qui répondent aux demandes d’URL et retournent du texte dans le navigateur


> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-1.md)
> [Suivant](mvc-music-store-part-3.md)
