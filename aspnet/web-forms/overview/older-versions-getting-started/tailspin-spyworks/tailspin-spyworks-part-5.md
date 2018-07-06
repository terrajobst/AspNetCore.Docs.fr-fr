---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Partie 5 : Une logique métier | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 5 ajoute une logique métier.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: fdb73c01d7b6076bc292c640376aa35cc5f52ea6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827046"
---
<a name="part-5-business-logic"></a>Partie 5 : Une logique métier
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 5 ajoute une logique métier.


## <a id="_Toc260221671"></a>  Ajout d’une logique métier

Nous voulons que notre expérience d’achat chaque fois que quelqu'un visite notre site web. Visiteurs sera en mesure de parcourir et ajouter des éléments au panier même s’ils ne sont pas inscrits ou connectés. Lorsqu’elles sont prêtes à consulter il aura la possibilité d’authentifier et si elles ne sont pas encore membres qu’ils seront en mesure de créer un compte.

Cela signifie que nous devons implémenter la logique permettant de convertir le panier d’achat à partir d’un état anonyme à un état « Utilisateur inscrit ».

Nous allons créer un répertoire nommé « Classes » avec le bouton droit sur le dossier puis créer un nouveau fichier de « Classe » nommé MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Comme mentionné précédemment nous allons étendre la classe qui implémente la page MyShoppingCart.aspx et nous fera à l’aide. Construction de puissantes « classe partielle » du NET.

Voici à quoi ressemble l’appel généré pour notre fichier MyShoppingCart.aspx.cf.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Notez l’utilisation du mot clé « partielle ».

Le fichier de classe qui nous vient d’être générées ressemble à ceci.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Nous allons fusionner nos implémentations en ajoutant le mot clé partiels à ce fichier ainsi.

Notre nouveau fichier de classe ressemble maintenant à ceci.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

La première méthode nous ajouterons à notre classe est la méthode « AddItem ». Il s’agit de la méthode qui sera finalement appelée lorsque l’utilisateur clique sur les liens « Ajouter à la pointe » sur les pages de liste de produits et les détails du produit.

Ajoutez le code suivant en utilisant les instructions en haut de la page.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Et ajoutez la méthode suivante à la classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Nous utilisons LINQ to Entities pour voir si l’élément est déjà dans le panier d’achat. Par conséquent, nous mise à jour la quantité commandée de l’élément, sinon nous créons une nouvelle entrée pour l’élément sélectionné

Pour pouvoir appeler cette méthode, nous implémenterons une page AddToCart.aspx qui non seulement cette méthode de classe, mais affiche ensuite un panier = après l’ajout de l’élément actuel.

Avec le bouton droit sur le nom de la solution dans l’Explorateur de solutions, puis ajoutez et page nommée AddToCart.aspx comme nous l’avons fait précédemment.

Pendant que nous pourrions utiliser cette page pour afficher les résultats intermédiaires telles que des problèmes de stock faible, etc., dans notre implémentation, la page ne sera pas réellement effectuer le rendu, mais plutôt appeler la logique de « Ajouter » et rediriger.

Pour ce faire, nous allons ajouter le code suivant à la Page\_événement de chargement.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Notez que nous récupérons le produit à ajouter au panier à partir d’un paramètre de chaîne de requête et en appelant la méthode AddItem de notre classe.

En supposant que sans erreurs sont rencontrées le contrôle est passé à la page SHoppingCart.aspx qui nous implémenterons entièrement ensuite. S’il doit comporter une erreur nous lève une exception.

Actuellement nous n'avons pas encore implémenté un gestionnaire d’erreurs global pour cette exception irait non prise en charge par notre application, mais nous sera résoudre ce problème sous peu.

Notez également l’utilisation de l’instruction Debug.Fail() (disponible via `using System.Diagnostics;)`

Est l’application s’exécute dans le débogueur, cette méthode affiche une boîte de dialogue détaillée avec des informations sur l’état des applications, ainsi que le message d’erreur que nous spécifions.

Lors de l’exécution en production la Debug.Fail() instruction est ignorée.

Vous remarquerez dans le code situé au-dessus d’un appel à une méthode dans notre noms de classe de panier d’achat « GetShoppingCartId ».

Ajoutez le code pour implémenter la méthode comme suit.

Notez que nous avons également ajouté une mise à jour et l’extraction des boutons et une étiquette où nous pouvons afficher le panier d’achat « total ».

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Nous pouvons maintenant ajouter des éléments à notre panier d’achat, mais nous n’avons pas implémenté la logique pour afficher le panier après l’ajout d’un produit.

Par conséquent, dans la page MyShoppingCart.aspx nous allons ajouter un contrôle EntityDataSource et un contrôle GridVire comme suit.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Appelez le formulaire dans le concepteur afin que vous pouvez double-cliquer sur le bouton de mise à jour le panier et générer un gestionnaire d’événements click qui est spécifié dans la déclaration dans le balisage.

Nous allons implémenter les détails plus tard, mais cela sera laissez-nous générer et exécuter notre application sans erreurs.

Lorsque vous exécutez l’application et ajoutez un élément au panier d’achat vous verrez ceci.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Notez que nous avons sonnette d’alarme à partir de l’affichage de grille « default » en implémentant les trois colonnes personnalisées.

Le premier est un champ « Lié » pour la quantité modifiable :

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

L’autre est une colonne « calculée » qui affiche l’élément de ligne total (l’élément de coût fois la quantité à commander) :

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Enfin, nous avons une colonne personnalisée qui contient un contrôle de case à cocher l’utilisateur utilisera pour indiquer que l’élément doit être supprimé du plan d’achat.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Comme vous pouvez le voir, l’ordre ligne Total est vide, nous allons donc ajouter une logique pour calculer le Total des commandes.

Nous allons tout d’abord implémenter une méthode « GetTotal » à la classe MyShoppingCart.

Dans le fichier MyShoppingCart.cs, ajoutez le code suivant.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Ensuite, dans la Page\_Gestionnaire d’événements Load nous pouvez appellerons notre méthode GetTotal. En même temps, nous allons ajouter un test pour voir si le panier d’achat est vide et ajuster en conséquence l’affichage s’il s’agit.

Maintenant si le panier d’achat est vide nous obtenons ce :

![](tailspin-spyworks-part-5/_static/image4.jpg)

Et dans le cas contraire, nous voyons le total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Toutefois, cette page n’est pas encore terminée.

Nous aurons besoin d’une logique supplémentaire pour recalculer le panier d’achat en supprimant les éléments marqués pour suppression et en déterminant les nouvelles valeurs de quantité certaines peuvent avoir été modifiée dans la grille par l’utilisateur.

Permet d’ajouter une méthode « RemoveItem » pour notre classe de panier d’achat dans MyShoppingCart.cs pour gérer les cas où un utilisateur marque un élément pour la suppression.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Maintenant nous allons ad une méthode pour gérer le cas lorsqu’un utilisateur modifie simplement la qualité pour être classés dans le contrôle GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Avec les fonctionnalités de mise à jour et supprimer la base en place, nous pouvons implémenter la logique qui met à jour le panier d’achat dans la base de données. (Dans MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Vous remarquerez que cette méthode nécessite deux paramètres. Un est l’Id de panier d’achat et l’autre est un tableau d’objets de type de défini par l’utilisateur.

Pour minimiser la dépendance de notre logique sur les spécificités d’interface utilisateur, nous avons défini une structure de données que nous pouvons utiliser pour transmettre des éléments du panier d’à notre code sans notre méthode n’ait à accéder directement le contrôle GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Dans notre fichier MyShoppingCart.aspx.cs nous pouvons utiliser cette structure dans notre gestionnaire d’événements de cliquez sur bouton mise à jour comme suit. Notez qu’en plus de la mise à jour le panier nous mettrons à jour le total du panier.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Notez, avec un intérêt particulier, cette ligne de code :

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() est une fonction d’assistance spéciales qui nous implémenterons dans MyShoppingCart.aspx.cs comme suit.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Cela fournit un moyen d’accéder aux valeurs des éléments liés dans notre contrôle GridView. Étant donné que notre contrôle de case à cocher « Supprimer un élément » n’est pas lié. nous allons y accéder via la méthode FindControl().

À ce stade dans le développement de votre projet nous nous préparons implémenter le processus de validation.

Avant cela nous allons utiliser Visual Studio pour générer la base de données d’appartenance et ajouter un utilisateur dans le référentiel de l’appartenance.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-4.md)
> [Suivant](tailspin-spyworks-part-6.md)
