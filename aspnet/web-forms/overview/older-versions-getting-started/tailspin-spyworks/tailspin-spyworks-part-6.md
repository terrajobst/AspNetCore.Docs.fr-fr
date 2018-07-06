---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Partie 6 : L’appartenance d’ASP.NET | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 6 ajoute l’appartenance ASP.NET.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 303c1edf548db1da9ef61d94bdd8157d6afbb2e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804418"
---
<a name="part-6-aspnet-membership"></a>Partie 6 : L’appartenance d’ASP.NET
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 6 ajoute l’appartenance ASP.NET.


## <a id="_Toc260221672"></a>  Utilisation de l’appartenance ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Cliquez sur la sécurité

![](tailspin-spyworks-part-6/_static/image1.jpg)

Assurez-vous que nous utilisons l’authentification par formulaire.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Utilisez le lien « Create User » pour créer quelques utilisateurs.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Lorsque vous avez terminé, reportez-vous à la fenêtre Explorateur de solutions et actualisez l’affichage.

![](tailspin-spyworks-part-6/_static/image2.png)

Notez que le fichier ASPNETDB. MDF fine a été créé. Ce fichier contient les tables pour prendre en charge des services tels que l’appartenance ASP.NET core.

Maintenant, nous pouvons commencer le processus de validation de mise en œuvre.

Commencez par créer une page CheckOut.aspx.

La page CheckOut.aspx doit uniquement être disponible pour les utilisateurs connectés afin de nous sera restreindre l’accès à consignés dans utilisateurs et redirige les utilisateurs qui ne sont pas connectés à la page de connexion.

Pour ce faire, nous allons ajouter les éléments suivants à la section de configuration de notre fichier web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Le modèle pour les applications ASP.NET Web Forms est automatiquement ajouté une section de l’authentification à notre fichier web.config et établi la page de connexion par défaut.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Nous devons modifier la Login.aspx fichier code-behind pour migrer un panier d’achat anonyme lorsque l’utilisateur se connecte. Modifier la Page\_charge l’événement comme suit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Puis ajoutez un gestionnaire d’événements « LoggedIn » comme suit pour définir le nom de session pour l’utilisateur qui vient d’être connecté et de modifier l’id de session temporaire dans le panier d’achat à celle de l’utilisateur en appelant la méthode MigrateCart dans notre classe MyShoppingCart. (Implémenté dans le fichier .cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implémentez la méthode MigrateCart() comme suit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Dans checkout.aspx nous allons utiliser un contrôle EntityDataSource et un GridView dans notre page d’extraction autant que nous l’avons fait dans notre page de panier d’achat.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Notez que notre contrôle GridView spécifie un gestionnaire d’événements « ondatabound » nommé MyList\_RowDataBound par conséquent, nous allons implémenter ce gestionnaire d’événements comme suit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Méthode reste ainsi ce total en cours d’exécution de l’achat au panier car chaque ligne est lié et met à jour de la ligne du bas du contrôle GridView.

À ce stade, nous avons implémenté une présentation de « révision » de la commande à placer.

Nous allons traiter un scénario de panier vide en ajoutant quelques lignes de code à notre Page\_événement de chargement :

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Lorsque l’utilisateur clique sur le bouton « Submit » nous exécutera le code suivant dans le Gestionnaire d’événement de Click de bouton Envoyer.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

« Viande » du processus de soumission de commande consiste à être implémentée dans la méthode SubmitOrder() de notre classe MyShoppingCart.

SubmitOrder sera :

- Prendre tous les éléments de ligne dans le panier d’achat et les utiliser pour créer un nouvel enregistrement de commande et les enregistrements de OrderDetails associés.
- Calculer la Date d’expédition.
- Désactivez le panier d’achat.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Dans le cadre de cet exemple d’application, nous calculons une date d’expédition en ajoutant simplement les deux jours à la date actuelle.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Exécution de l’application maintenant autoriser nous permet de tester le processus d’achat à partir du début à la fin.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-5.md)
> [Suivant](tailspin-spyworks-part-7.md)
