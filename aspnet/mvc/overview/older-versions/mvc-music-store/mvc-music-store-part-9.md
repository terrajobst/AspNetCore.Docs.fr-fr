---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Partie 9 : Inscription et validation | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 9 couvre l’inscription et l’extraction.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812871"
---
<a name="part-9-registration-and-checkout"></a>Partie 9 : Inscription et validation
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 9 couvre l’inscription et l’extraction.


Dans cette section, nous allons créer un CheckoutController qui recueillera les adresses du client et les informations de paiement. Nous oblige les utilisateurs à inscrire auprès de notre site avant la récupération, donc ce contrôleur nécessite une autorisation.

Les utilisateurs pour accéder à la caisse à partir de leur panier d’achat en cliquant sur le bouton « Valider ».

![](mvc-music-store-part-9/_static/image1.jpg)

Si l’utilisateur n’est pas connecté, il seront invité à.

![](mvc-music-store-part-9/_static/image1.png)

Une fois connectés, l’utilisateur voit ensuite la vue de l’adresse et de paiement.

![](mvc-music-store-part-9/_static/image2.png)

Une fois qu’ils ont rempli le formulaire et l’ordre, ils seront affichera l’écran de confirmation de commande.

![](mvc-music-store-part-9/_static/image3.png)

Essayez d’afficher une commande inexistant ou une commande qui n’appartient pas à vous affichera la vue de l’erreur.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrer le panier d’achat

Pendant le processus d’achat est anonyme, lorsque l’utilisateur clique sur le bouton de validation, ils seront requises pour l’inscription et connexion. Les utilisateurs s’attendent que nous met à jour leurs informations de panier d’achat entre les visites, donc nous devons associer les informations du panier d’un utilisateur lorsqu’elles s’achèvent d’inscription ou de connexion.

Il s’agit en fait très simple à réaliser, comme notre classe ShoppingCart possède déjà une méthode qui associe tous les éléments dans le panier d’achat en cours avec un nom d’utilisateur. Nous devrons simplement appeler cette méthode lorsqu’un utilisateur termine l’inscription ou connexion.

Ouvrez le **AccountController** classe que nous avons ajouté lorsque nous étions vous configurez l’appartenance et l’autorisation. Ajouter un à l’aide d’instruction faisant référence à MvcMusicStore.Models, puis ajoutez la méthode MigrateShoppingCart suivante :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Ensuite, modifiez l’action d’ouverture de session pour appeler MigrateShoppingCart une fois que l’utilisateur a été validé, comme indiqué ci-dessous :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Apporter la même modification au Registre à l’action, de publication immédiatement après que le compte d’utilisateur est créé avec succès :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

C’est tout - maintenant un panier d’achat anonyme est automatiquement transférée à un compte d’utilisateur lors de l’inscription réussie ou la connexion.

## <a name="creating-the-checkoutcontroller"></a>Création de la CheckoutController

Avec le bouton droit sur le dossier Controllers et ajoutez un nouveau contrôleur au projet nommé CheckoutController en utilisant le modèle de contrôleur vide.

![](mvc-music-store-part-9/_static/image5.png)

Tout d’abord, ajoutez l’attribut Authorize au-dessus de la déclaration de classe de contrôleur pour obliger les utilisateurs à inscrire avant d’extraction :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Remarque : Ceci est similaire à la modification que nous avons apportées précédemment à le StoreManagerController, mais dans ce cas l’attribut Authorize nécessaire que l’utilisateur soit dans un rôle d’administrateur. Dans le contrôleur de validation, nous allons nécessitant l’utilisateur est connecté, mais ne sont pas exiger qu’ils soient des administrateurs.*

Par souci de simplicité, nous ne sont pas affaire à des informations de paiement dans ce didacticiel. Au lieu de cela, nous autorisons les utilisateurs à consulter à l’aide d’un code promotionnel. Nous allons stocker ce code promotionnel à l’aide d’une constante nommée code promo.

Comme dans le StoreController, nous allons déclarer un champ devant contenir une instance de la classe MusicStoreEntities, nommée storeDB. Afin de rendre utilisation de la classe MusicStoreEntities, nous devons ajouter une à l’aide de l’instruction pour l’espace de noms MvcMusicStore.Models. La partie supérieure de notre contrôleur Checkout apparaît ci-dessous.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Le CheckoutController aura les actions de contrôleur suivantes :

**AddressAndPayment (méthode GET)** affiche un formulaire pour autoriser l’utilisateur à entrer leurs informations.

**AddressAndPayment (méthode POST)** valider l’entrée et de traiter la commande.

**Complète** s’affichera une fois un utilisateur a terminé avec succès le processus de validation. Cet affichage inclura le numéro de commande de l’utilisateur, en tant que confirmation.

Tout d’abord, nous allons renommer l’action de contrôleur d’Index (qui a été générée lorsque nous avons créé le contrôleur) à AddressAndPayment. Cette action de contrôleur affiche simplement le formulaire d’extraction, afin qu’il ne nécessite des informations sur le modèle.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Notre méthode AddressAndPayment POST suivra le même modèle que nous avons utilisé dans le StoreManagerController : il essaiera d’accepter l’envoi du formulaire et terminer la commande et ré-affiche le formulaire en cas d’échec.

Une fois la validation de l’entrée de formulaire répond à nos exigences de validation d’une commande, nous allons vérifier la valeur de formulaire code promo directement. En supposant que tout est correct, que nous allons enregistrer les informations mises à jour avec l’ordre, indiquer à l’objet ShoppingCart pour terminer le processus de commande et rediriger vers l’action terminée.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

En cas de réussite du processus d’extraction, les utilisateurs seront redirigés vers l’action de contrôleur complète. Cette action effectue une vérification simple pour valider que la commande appartient bien à l’utilisateur connecté avant d’afficher le numéro de commande en tant qu’une confirmation.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Remarque : La vue de l’erreur a été automatiquement créée pour nous dans le dossier /Views/Shared lorsque nous avons commencé le projet.*

Le code CheckoutController complet est comme suit :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Ajout de la vue AddressAndPayment

Maintenant, nous allons créer la vue AddressAndPayment. Avec le bouton droit sur l’une des actions de contrôleur AddressAndPayment et ajouter une vue nommée AddressAndPayment qui est fortement typée en tant que commande et utilise le modèle de modification, comme indiqué ci-dessous.

![](mvc-music-store-part-9/_static/image6.png)

Cette vue rendra utilise deux des techniques que nous avons examiné lors de la création de la vue StoreManagerEdit :

- Nous allons utiliser Html.EditorForModel() pour afficher les champs pour le modèle de commande
- Nous allons tirer parti des règles de validation à l’aide d’une classe de commande avec les attributs de validation

Nous allons commencer en mettant à jour le code de formulaire pour utiliser Html.EditorForModel(), suivi d’une zone de texte supplémentaire pour le Code de promotion. Le code complet pour la vue AddressAndPayment est indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Définition des règles de validation de la commande

Maintenant que notre vue est configuré, nous allons configurer les règles de validation pour notre modèle de commande comme nous l’avons fait précédemment pour le modèle de l’Album. Avec le bouton droit sur le dossier Models et ajoutez une classe nommée ordre. Outre les attributs de validation que nous avons utilisé précédemment pour l’Album, nous allons également utiliser une Expression régulière pour valider l’adresse de messagerie de l’utilisateur.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Essayez d’envoyer le formulaire avec manquant ou des informations non valides s’affichent désormais de message d’erreur à l’aide de la validation côté client.

![](mvc-music-store-part-9/_static/image7.png)

OK, nous l’avons fait plus partie du travail pour le processus de validation ; Nous avons simplement quelques terminaisons and de probabilités pour terminer. Nous devons ajouter deux vues simples, et nous avons besoin prendre en charge de la procédure de transfert des informations de panier pendant le processus de connexion.

## <a name="adding-the-checkout-complete-view"></a>Ajout de la vue complète d’extraction

La vue complète d’extraction est assez simple, car il a juste besoin d’afficher l’ID de commande. Avec le bouton droit sur l’action de contrôleur complète et ajouter une vue nommée complète qui est fortement typée en tant qu’un entier.

![](mvc-music-store-part-9/_static/image8.png)

Maintenant nous mettrons à jour le code de vue pour afficher l’ID de commande, comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Erreur lors de la vue de la mise à jour

Le modèle par défaut inclut une vue de l’erreur dans le dossier de vues Shared afin qu’il puisse être réutilisé ailleurs dans le site. Cette vue de l’erreur contient une erreur très simple et n’utilise pas notre site de mise en page, donc nous allons mettre à jour.

Dans la mesure où il s’agit d’une page d’erreur générique, le contenu est très simple. Nous allons inclure un message et un lien pour accéder à la page précédente dans l’historique si l’utilisateur souhaite réessayer leur action.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-8.md)
> [Suivant](mvc-music-store-part-10.md)
