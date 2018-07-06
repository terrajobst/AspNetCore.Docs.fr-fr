---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Amélioration des méthodes Details et Delete (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 5b34c7d71e615215e933b607562a06f683c6c1ca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819862"
---
<a name="improving-the-details-and-delete-methods-vb"></a>Amélioration des méthodes Details et Delete (VB)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/improving-the-details-and-delete-methods.md) de ce didacticiel.


Dans cette partie du didacticiel, vous allez apporter des améliorations aux généré automatiquement `Details` et `Delete` méthodes. Ces modifications ne sont pas nécessaires, mais avec quelques petits extraits de code, vous pouvez facilement améliorer l’application.

## <a name="improving-the-details-and-delete-methods"></a>Amélioration des méthodes Details et Delete

Lorsque vous structuré le `Movie` contrôleur, cela fonctionne très bien, mais qui peuvent le rendre plus robuste avec seulement quelques petites modifications de code généré par ASP.NET MVC.

Ouvrez le `Movie` contrôleur et modifier le `Details` méthode en retournant `HttpNotFound` quand un film n’est pas trouvé. Vous devez également modifier le `Details` méthode pour définir une valeur par défaut pour l’ID qui lui est passé. (Vous apporté des modifications similaires à la `Edit` méthode dans [partie 6](examining-the-edit-methods-and-edit-view.md) de ce didacticiel.) Toutefois, vous devez modifier le type de retour de la `Details` méthode à partir de `ViewResult` à `ActionResult`, car le `HttpNotFound` méthode ne retourne pas une `ViewResult` objet. L’exemple suivant montre le texte modifié `Details` (méthode).

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Code tout d’abord permet de facilement rechercher des données à l’aide du `Find` (méthode). Une fonctionnalité de sécurité important que nous avons créé dans la méthode est que le code vérifie que le `Find` méthode a trouvé un film avant que le code essaie de faire quoi que ce soit avec lui. Par exemple, un pirate informatique pourrait induire des erreurs dans le site en modifiant l’URL créée par les liens à partir de `http://localhost:xxxx/Movies/Details/1` à quelque chose comme `http://localhost:xxxx/Movies/Details/12345` (ou une autre valeur qui ne représente pas un film réel). Si vous ne la vérification pour un film non valide, cela pourrait entraîner une erreur de base de données.

De même, changer le `Delete` et `DeleteConfirmed` méthodes pour spécifier une valeur par défaut pour le paramètre ID et retourner `HttpNotFound` quand un film n’est pas trouvé. La mise à jour `Delete` méthodes dans le `Movie` contrôleur sont présentées ci-dessous.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Notez que le `Delete` méthode ne supprime pas les données. L’exécution d’une opération de suppression en réponse à une requête GET (ou encore l’exécution d’une opération de modification, d’une opération de création ou de toute autre opération qui modifie des données) génère une faille de sécurité. Pour plus d’informations à ce sujet, consultez l’entrée de blog de Stephen Walther [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

La méthode `HttpPost` qui supprime les données est nommée `DeleteConfirmed` pour donner à la méthode HTTP POST une signature ou un nom unique. Les signatures des deux méthodes sont illustrées ci-dessous :

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Le common language runtime (CLR), les méthodes surchargées pour avoir une signature unique (même nom, liste de paramètres différentes). Toutefois, vous devez ici deux méthodes de suppression : une pour GET et une pour POST que les deux nécessitent la même signature. (Elles doivent toutes les deux accepter un entier unique comme paramètre.)

Pour trier à ce stade, vous pouvez faire deux choses. Une consiste à attribuer aux méthodes des noms différents. C’est ce que nous l’avons fait dans le précédent exemple. Toutefois, elle présente un petit problème : ASP.NET mappe des segments d’une URL à des méthodes d’action par nom. Si vous renommez une méthode, il est probable que le routage ne pourra pas trouver cette méthode. La solution consiste à faire ce que vous voyez dans l’exemple, c’est-à-dire à ajouter l’attribut `ActionName("Delete")` à la méthode `DeleteConfirmed`. Il effectue efficacement mappage pour le système de routage afin qu’une URL qui inclut <em>/Delete/</em>un billet demande trouveront le `DeleteConfirmed` (méthode).

Une autre façon d’éviter un problème avec les méthodes qui ont des signatures et des noms identiques consiste à modifier artificiellement la signature de la méthode POST pour inclure un paramètre inutilisé. Par exemple, certains développeurs ajouter un type de paramètre `FormCollection` qui est passé à la méthode POST et n’utilisez simplement le paramètre :

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Conclusion

Vous disposez maintenant d’une application ASP.NET MVC complète qui stocke les données dans une base de données SQL Server Compact. Vous pouvez créer, lire, mettre à jour, supprimer et rechercher des films.

![](improving-the-details-and-delete-methods/_static/image1.png)

Ce didacticiel de base a été de vous lancer dans la création de contrôleurs, leur association avec des vues et la transmission autour des données codées en dur. Puis vous avez créé et conçu un modèle de données. Entity Framework Code First créé une base de données à partir du modèle de données à la volée, et le système de génération de modèles automatique ASP.NET MVC est généré automatiquement les méthodes d’action et les vues pour les opérations CRUD de base. Vous avez ensuite ajouté un formulaire de recherche qui permettent aux utilisateurs de rechercher la base de données. Votre modifié la base de données pour inclure une nouvelle colonne de données, puis mis à jour deux pages pour créer et afficher ces nouvelles données. Vous avez ajouté la validation en marquant le modèle de données avec des attributs à partir de la `DataAnnotations` espace de noms. La validation qui en résulte s’exécute sur le client et sur le serveur.

Si vous souhaitez déployer votre application, il est utile pour le premier test de l’application sur votre serveur IIS 7 local. Vous pouvez utiliser cette [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) lien pour activer le paramètre IIS pour les applications ASP.NET. Consultez les liens de déploiement suivants :

- [Organigramme de déploiement ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [L’activation d’IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Déploiement de projets d’Application Web](https://msdn.microsoft.com/library/dd394698.aspx)

J’ai maintenant vous encourageons à passer à notre niveau intermédiaire [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) et [Store de musique MVC](../../mvc-music-store/mvc-music-store-part-1.md) didacticiels, pour Explorer le [ASP.NET articles sur MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)et à consulter les nombreuses vidéos et à vos ressources [ https://asp.net/mvc ](https://asp.net/mvc) pour en savoir plus sur ASP.NET MVC ! Le [forums d’ASP.NET MVC](https://forums.asp.net/1146.aspx) sont l’endroit idéal pour poser des questions.

Bonne lecture !

-Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) et [ @shanselman ](http://twitter.com/shanselman) sur Twitter) et Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Précédent](adding-validation-to-the-model.md)
