---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Partie 7 : Ajout de fonctionnalités | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 7 ajoute des fonctionnalités supplémentaires, telles que le volet du compte...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: cada8d9aee649e4f2a5afc1ca2b46863ea458207
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824295"
---
<a name="part-7-adding-features"></a>Partie 7 : Ajout de fonctionnalités
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 7 ajoute des fonctionnalités supplémentaires, telles que la révision de compte, les évaluations de produits et les « éléments populaires » et les contrôles utilisateur « également acheté ».


## <a id="_Toc260221673"></a>  Ajout de fonctionnalités

Bien que les utilisateurs peuvent parcourir notre catalogue, placer des éléments dans leur panier d’achat et le processus de validation, il existe qu'un nombre de prise en charge des fonctionnalités que nous inclurons pour améliorer notre site.

1. Examen du compte (liste de commandes placées et afficher les détails.)
2. Ajouter un contenu spécifique de contexte à la première page.
3. Ajouter une fonctionnalité pour permettre aux utilisateurs de vérifier les produits dans le catalogue.
4. Créer un contrôle utilisateur pour afficher les éléments les plus courants et sur Place qui contrôlent sur la première page.
5. Créer un contrôle utilisateur « Également acheté » et l’ajouter à la page Détails du produit.
6. Ajouter un Contact Page.
7. Ajouter un sur la Page.
8. Erreur globale

## <a id="_Toc260221674"></a>  Votre compte

Dans le dossier « Compte », créez deux pages .aspx un OrderList.aspx nommés et les autres OrderDetails.aspx nommé

OrderList.aspx s’appuieront sur les contrôles GridView et EntityDataSoure autant que nous avons précédemment.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Le EntityDataSoure sélectionne les enregistrements de la table Orders filtrée sur le nom d’utilisateur (voir la WhereParameter) qui nous défini dans une variable de session lorsque le journal utilisateur 's.

Notez également ces paramètres dans le HyperlinkField du contrôle GridView :

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Ils spécifient le lien vers la vue de détails de commande pour chaque produit spécifiant le champ OrderID comme un paramètre de chaîne de requête à la page OrderDetails.aspx.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Nous allons utiliser un contrôle EntityDataSource pour accéder aux commandes et un FormView afin d’afficher les données de commande et un autre contrôle EntityDataSource avec un GridView pour afficher les éléments de ligne de toutes les la commande.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

Dans le fichier Code-Behind (OrderDetails.aspx.cs), nous avons deux bits peu de ménage.

Nous devons d’abord vous assurer que OrderDetails obtient toujours un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Nous devons également calculer et afficher le total à partir des éléments de ligne de commande.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  La Page d’accueil

Nous allons ajouter du contenu statique à la page Default.aspx.

Tout d’abord, je vais créer un dossier « Contenu » et qu’il contient un dossier Images (et je vais inclure une image à utiliser sur la page d’accueil.)

Dans l’espace réservé au bas de la page Default.aspx, ajoutez le balisage suivant.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Évaluations de produits

Tout d’abord, nous allons ajouter un bouton avec un lien à un formulaire que nous pouvons utiliser pour entrer une évaluation de produit.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Notez que nous passons ProductID dans la chaîne de requête

Suivant nous allons ajouter une page nommée ReviewAdd.aspx

Cette page utilise ASP.NET AJAX Control Toolkit. Si vous n'avez pas déjà fait donc vous pouvez le télécharger à partir de [DevExpress](http://devexpress.com/act) et il existe des conseils sur la configuration de la boîte à outils à utiliser avec Visual Studio ici [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

En mode Création, faites glisser des contrôles et des validateurs à partir de la boîte à outils et créer un formulaire similaire à celui ci-dessous.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Le balisage ressemblera à ceci.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Maintenant que nous pouvons saisir des révisions, permet d’afficher ces avis sur la page du produit.

Ajoutez ce balisage à la page ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

À présent, notre application en cours d’exécution et en accédant à un produit affiche les informations de produit, y compris les avis des utilisateurs.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Contrôle les éléments demandés (création de contrôles utilisateur)

Afin d’augmenter les ventes sur votre site web, nous allons ajouter quelques fonctionnalités aux produits populaires ou connexes de « sell suggérées ».

La première de ces fonctionnalités sera une liste de produits plus populaires dans notre catalogue de produits.

Nous allons créer un « contrôle utilisateur » pour afficher le haut de vente des articles sur la page d’accueil de notre application. Dans la mesure où il s’agit d’un contrôle, nous pouvons l’utiliser sur n’importe quelle page en faisant simplement glisser le contrôle dans le Concepteur de Visual Studio sur n’importe quelle page Nous aimons.

Dans l’Explorateur de solutions de Visual Studio, avec le bouton droit sur le nom de la solution et créez un répertoire nommé « Contrôles ». Il n’est pas nécessaire pour ce faire, nous permettent de maintenir notre projet organisé en créant tous nos contrôles utilisateur dans le répertoire « Contrôles ».

Avec le bouton droit sur le dossier de contrôles et choisir « Nouvel élément » :

![](tailspin-spyworks-part-7/_static/image4.jpg)

Spécifiez un nom pour notre contrôle de « PopularItems ». Notez que l’extension de fichier pour les contrôles utilisateur est .ascx pas .aspx.

Notre contrôle utilisateurs éléments courants est définie comme suit.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Ici, nous utilisons une méthode que nous n'avons pas encore utilisé dans cette application. Nous utilisons le contrôle repeater et au lieu d’utiliser un contrôle de source de données nous le lions le contrôle Repeater avec les résultats d’une requête LINQ to Entities.

Dans le code-behind de notre contrôle nous le faire comme suit.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Notez également cette ligne importante en haut du balisage de notre contrôle.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Dans la mesure où les articles les plus populaires ne modifiera pas les minutes à toutes les minutes, nous pouvons ajouter une directive aching pour améliorer les performances de notre application. Cette directive amène le code de contrôles être exécutée uniquement lorsque la sortie mise en cache du contrôle arrive à expiration. Sinon, la version mise en cache de sortie du contrôle servira.

Il nous suffit est maintenant inclure notre nouveau contrôle dans notre page Default.aspc.

Utilisation et faites-la glisser pour placer une instance du contrôle dans la colonne ouvre notre formulaire par défaut.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Lorsque nous exécutons notre application, la page d’accueil affiche maintenant les articles les plus populaires.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  « Également acheté » (contrôles utilisateur avec des paramètres) du contrôle

Le deuxième contrôle utilisateur que nous allons créer prendra suggéré convaincre le niveau suivant en ajoutant la spécificité du contexte.

La logique pour calculer les premiers éléments « Également acheté » est non trivial.

Notre contrôle « Également acheté » sélectionne les enregistrements OrderDetails (précédemment achetés) pour l’ID de produit actuellement sélectionné et saisir la OrderIDs pour chaque commande unique est trouvé.

Ensuite, sélectionnez al les produits à partir de toutes ces commandes et somme de quantités achetées. Nous allons trier les produits par cette somme de la quantité, afficher les éléments de cinq.

Étant donné la complexité de cette logique, nous implémenterons cet algorithme comme une procédure stockée.

Le T-SQL pour la procédure stockée est comme suit.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Notez que cette procédure stockée (SelectPurchasedWithProducts) existait dans la base de données lorsque nous l’inclus dans notre application et lorsque nous avons généré l’Entity Data Model que nous avons spécifié qui, en plus des Tables et vues que nous devions, l’Entity Data Model doit inclure cette procédure stockée.

Pour accéder à la procédure stockée à partir de l’Entity Data Model que nous avons besoin d’importer la fonction.

Double-cliquez sur l’Entity Data Model dans l’Explorateur de Solutions pour l’ouvrir dans le concepteur et ouvrez l’Explorateur de modèles, puis avec le bouton droit dans le concepteur et sélectionnez « Ajouter une importation de fonction ».

![](tailspin-spyworks-part-7/_static/image1.png)

Ce faisant, cette boîte de dialogue s’ouvre.

![](tailspin-spyworks-part-7/_static/image2.png)

Renseignez les champs comme vous le constater ci-dessus, en sélectionnant le « SelectPurchasedWithProducts » et utiliser le nom de la procédure pour le nom de notre fonction importée.

Cliquez sur « Ok ».

Avoir effectué cette opération que nous pouvons simplement programmer par rapport à la procédure stockée comme nous peut être n’importe quel autre élément dans le modèle.

Par conséquent, dans notre dossier « Contrôles » vous devez créer un contrôle utilisateur nommé AlsoPurchased.ascx.

Le balisage de ce contrôle sera sembler très familière pour le contrôle PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La différence notable est qui ne sont pas de mise en cache la sortie dans la mesure où l’élément doit être restitué varient par produit.

L’ID de produit sera une « property » au contrôle.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Dans le Gestionnaire du contrôle événement PreRender nous seau faire trois choses.

1. Assurez-vous que l’ID de produit est défini.
2. Voir s’il existe des produits qui ont été achetés avec l’instance actuelle.
3. Sortie de certains éléments, comme déterminé dans #2.

Remarquez combien il est facile d’appeler la procédure stockée via le modèle.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Après avoir déterminé qu’il sont « également acheté », nous pouvons simplement lier le contrôle repeater aux résultats retournés par la requête.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

S’il n’existait pas tous les éléments « également acheté » nous affichons simplement des autres éléments les plus courants à partir de notre catalogue.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Pour afficher les éléments « Également acheté », ouvrez la page ProductDetails.aspx et faites glisser le contrôle AlsoPurchased à partir de l’Explorateur de Solutions afin qu’il apparaisse dans cette position dans le balisage.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Cela créera une référence au contrôle en haut de la page ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Étant donné que le contrôle utilisateur AlsoPurchased requiert un nombre de ProductId, nous allons définir la propriété ProductID de notre contrôle à l’aide d’une instruction Eval par rapport à l’élément de modèle de données en cours de la page.

![](tailspin-spyworks-part-7/_static/image3.png)

Lorsque nous créons et exécuter maintenant et accédez à un produit, nous voyons les éléments « Également acheté ».

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-6.md)
> [Suivant](tailspin-spyworks-part-8.md)
