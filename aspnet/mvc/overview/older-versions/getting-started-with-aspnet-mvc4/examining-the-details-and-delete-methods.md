---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Examen des méthodes Details et Delete | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 1e23189fe927a5145647baa1f8886c4aed057b78
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578339"
---
<a name="examining-the-details-and-delete-methods"></a>Examen des méthodes Details et Delete
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.


Dans cette partie du didacticiel, vous allez examiner généré automatiquement `Details` et `Delete` méthodes.

## <a name="examining-the-details-and-delete-methods"></a>Examen des méthodes Details et Delete

Ouvrez le `Movie` contrôleur et examinez le `Details` (méthode).

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Le moteur de génération de modèles automatique MVC qui a créé cette méthode d’action ajoute un commentaire montrant une requête HTTP qui appelle la méthode. Dans ce cas, il est un `GET` demande avec trois segments d’URL, le `Movies` contrôleur, le `Details` (méthode) et un `ID` valeur.

Code tout d’abord permet de facilement rechercher des données à l’aide du `Find` (méthode). Une fonctionnalité de sécurité importante intégrée à la méthode est que le code vérifie que le `Find` méthode a trouvé un film avant que le code essaie de faire quoi que ce soit avec lui. Par exemple, un pirate informatique pourrait induire des erreurs dans le site en modifiant l’URL créée par les liens à partir de `http://localhost:xxxx/Movies/Details/1` à quelque chose comme `http://localhost:xxxx/Movies/Details/12345` (ou une autre valeur qui ne représente pas un film réel). Si vous n’avez pas recherché un film non valide, un film non valide provoque une erreur de base de données.

Examinez les méthodes `Delete` et `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Notez que le `HTTP Get``Delete` méthode ne supprime pas le film spécifié, il retourne une vue du film où vous pouvez soumettre (`HttpPost`) la suppression... L’exécution d’une opération de suppression en réponse à une requête GET (ou encore l’exécution d’une opération de modification, d’une opération de création ou de toute autre opération qui modifie des données) génère une faille de sécurité. Pour plus d’informations à ce sujet, consultez l’entrée de blog de Stephen Walther [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

La méthode `HttpPost` qui supprime les données est nommée `DeleteConfirmed` pour donner à la méthode HTTP POST une signature ou un nom unique. Les signatures des deux méthodes sont illustrées ci-dessous :

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Le Common Language Runtime (CLR) nécessite des méthodes surchargées pour avoir une signature à paramètre unique (même nom de méthode, mais liste de paramètres différentes). Toutefois, vous devez ici deux méthodes de suppression : une pour GET et une pour POST que les deux ont la même signature de paramètre. (Elles doivent toutes les deux accepter un entier unique comme paramètre.)

Pour trier à ce stade, vous pouvez faire deux choses. Une consiste à attribuer aux méthodes des noms différents. C’est ce qu’a fait le mécanisme de génération de modèles automatique dans l’exemple précédent. Toutefois, elle présente un petit problème : ASP.NET mappe des segments d’une URL à des méthodes d’action par nom. Si vous renommez une méthode, il est probable que le routage ne pourra pas trouver cette méthode. La solution consiste à faire ce que vous voyez dans l’exemple, c’est-à-dire à ajouter l’attribut `ActionName("Delete")` à la méthode `DeleteConfirmed`. Il effectue efficacement mappage pour le système de routage afin qu’une URL qui inclut <em>/Delete/</em>un billet demande trouveront le `DeleteConfirmed` (méthode).

Une autre méthode pour éviter un problème avec les méthodes qui ont des signatures et des noms identiques consiste à modifier artificiellement la signature de la méthode POST pour inclure un paramètre inutilisé. Par exemple, certains développeurs ajouter un type de paramètre `FormCollection` qui est passé à la méthode POST et n’utilisez simplement le paramètre :

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Récapitulatif

Vous disposez maintenant d’une application ASP.NET MVC complète qui stocke les données dans une base de données de base de données locale. Vous pouvez créer, lire, mettre à jour, supprimer et rechercher des films.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez créé et testé une application web, l’étape suivante consiste à rendre disponible à d’autres personnes à utiliser sur Internet. Pour ce faire, vous devez déployer sur un fournisseur d’hébergement web. Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un [compte d’évaluation de Windows Azure gratuit](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Je vous suggère de suivre ensuite me didacticiel [déployer une application ASP.NET MVC sécurisée avec appartenance, OAuth et base de données SQL à un Site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un excellent didacticiel est de niveau intermédiaire de Tom Dykstra [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) et [forums d’ASP.NET MVC](https://forums.asp.net/1146.aspx) sont une bonne place pour poser des questions. Suivez [me](https://twitter.com/RickAndMSFT) sur twitter afin d’obtenir les mises à jour sur mon derniers didacticiels.

Commentaires sont les bienvenus.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter : [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter : [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Précédent](adding-validation-to-the-model.md)
