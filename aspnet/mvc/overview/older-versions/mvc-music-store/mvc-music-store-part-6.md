---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Partie 6 : À l’aide des Annotations de données pour la Validation de modèle | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 6 couvre à l’aide des Annotations de données pour le modèle V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b666c3cef0b09c6d68cee581a3c27c08e3357cca
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834633"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Partie 6 : À l’aide des Annotations de données pour la Validation de modèle
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 6 couvre à l’aide des Annotations de données pour la Validation de modèle.


Nous avons un problème majeur avec nos formulaires Create et Edit : qu’elles ne font pas de toute opération de validation. Nous pouvons effectuer des opérations comme laisser les champs obligatoires vides ou des lettres de type dans le champ prix, et la première erreur, que nous allons voir provient de la base de données.

Nous pouvons facilement ajouter la validation à notre application en ajoutant des Annotations de données à nos classes de modèle. Annotations de données nous permettent de décrire les règles que nous voulons appliquées aux propriétés de notre modèle et ASP.NET MVC s’occupera de les appliquer et d’afficher les messages appropriés à nos utilisateurs.

## <a name="adding-validation-to-our-album-forms"></a>Ajout d’une Validation à nos formulaires Album

Nous allons utiliser les attributs d’Annotation de données suivants :

- **Requis** – indique que la propriété est un champ obligatoire
- **DisplayName** – définit le texte que nous voulons utilisés sur les champs de formulaire et les messages de validation
- **StringLength** – définit une longueur maximale pour un champ de chaîne
- **Plage** – donne la valeur minimale et maximale pour un champ numérique
- **Lier** – répertorie les champs à exclure ou inclure lors de la liaison de paramètre ou la forme de valeurs aux propriétés de modèle
- **ScaffoldColumn** – permet de masquer les champs de formulaires de l’éditeur

*Remarque : Pour plus d’informations sur la Validation de modèle à l’aide des attributs d’Annotation de données, consultez la documentation MSDN à l’adresse*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Ouvrez la classe Album et ajoutez le code suivant *à l’aide de* instructions au début.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Ensuite, mettez à jour les propriétés pour ajouter des attributs d’affichage et la validation comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Parallèlement, nous avons également modifié le Genre et l’artiste propriétés virtuelles. Cela permet à Entity Framework chargés en différé-les en fonction des besoins.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Après avoir ajouté ces attributs à notre modèle Album, notre écran Create et Edit commencer immédiatement la validation des champs et en utilisant les noms d’affichage, nous avons choisi (par exemple, Album Art Url au lieu de AlbumArtUrl). Exécutez l’application et accédez à /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Ensuite, nous allons répartir des règles de validation. Entrez un prix de 0 et ne renseignez pas le titre. Lorsque nous cliquons sur le bouton Créer, nous verrons le formulaire affiché avec la validation des messages d’erreur affichant les champs qui ne répondait pas aux règles de validation, nous avons défini.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Test de la Validation côté Client

Validation côté serveur est très importante du point de vue de l’application, car les utilisateurs peuvent contourner la validation côté client. Cependant, les formulaires de page Web qui implémentent uniquement la validation côté serveur comportent trois problèmes importants.

1. L’utilisateur doit attendre pour le formulaire à valider, validé sur le serveur et pour la réponse à envoyer à leur navigateur.
2. L’utilisateur n’obtenir des retours immédiats lorsqu’elles corrigent un champ afin qu’il réussit maintenant les règles de validation.
3. Nous sommes gaspiller des ressources du serveur pour exécuter la logique de validation au lieu d’en tirant parti du navigateur de l’utilisateur.

Heureusement, les modèles de structure ASP.NET MVC 3 ont la validation côté client intégrée, ne nécessitant aucun travail supplémentaire que ce soit.

Tapez une lettre unique dans le champ titre satisfait les exigences de validation, donc le message de validation est immédiatement supprimé.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-5.md)
> [Suivant](mvc-music-store-part-7.md)
