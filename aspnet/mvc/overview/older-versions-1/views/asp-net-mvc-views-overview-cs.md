---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Vue d’ensemble (c#) des vues ASP.NET MVC | Microsoft Docs
author: StephenWalther
description: Qu’est une vue de MVC ASP.NET, et en quoi est-il différent d’une page HTML ? Dans ce didacticiel, Stephen Walther présente les vues et montre comment vous pouvez t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: adf995529b34c84969125adcc6249ba8e22a7af4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387788"
---
<a name="aspnet-mvc-views-overview-c"></a>Vue d’ensemble (c#) des vues ASP.NET MVC
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Qu’est une vue de MVC ASP.NET, et en quoi est-il différent d’une page HTML ? Dans ce didacticiel, Stephen Walther vous présente les vues et montre comment vous pouvez tirer parti des données d’affichage et de programmes d’assistance HTML dans une vue.


L’objectif de ce didacticiel est de vous fournir une brève introduction aux vues ASP.NET MVC, afficher les données et les programmes d’assistance HTML. À la fin de ce didacticiel, vous devez comprendre comment créer des vues, passer des données à partir d’un contrôleur à une vue et utiliser des programmes d’assistance HTML pour générer le contenu dans une vue.

## <a name="understanding-views"></a>Présentation des vues

Pour ASP.NET ou des pages Active Server Pages, ASP.NET MVC n’inclut pas tout ce qui correspond directement à une page. Dans une application ASP.NET MVC, il n’est pas une page sur le disque qui correspond au chemin dans l’URL que vous tapez dans la barre d’adresses de votre navigateur. La chose la plus proche à une page dans une application ASP.NET MVC est quelque chose appelé un *vue*.

ASP.NET MVC, les demandes d’application du navigateur entrantes sont mappées aux actions de contrôleur. Une action de contrôleur peut retourner une vue. Toutefois, une action de contrôleur peut effectuer un autre type d’action, telle que la redirection vers une autre action de contrôleur.

Listing 1 contient un simple contrôleur nommé le HomeController. Le HomeController expose deux actions de contrôleur nommées Index() et Details().

**Liste 1 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Vous pouvez appeler la première action, l’action Index(), en tapant l’URL suivante dans votre barre d’adresses du navigateur :

/ Home/Index

Vous pouvez appeler la seconde action, l’action Details(), en tapant cette adresse dans votre navigateur :

/ Home/détails

L’action Index() retourne une vue. La plupart des actions que vous créez retournera des vues. Toutefois, une action peut retourner d’autres types de résultats d’action. Par exemple, l’action Details() retourne un RedirectToActionResult qui redirige la requête entrante à l’action Index().

L’action Index() contienne la ligne de code unique suivante :

View() ;

Cette ligne de code retourne une vue qui doit se trouve à l’emplacement suivant sur votre serveur web :

\Views\Home\Index.aspx

Le chemin d’accès à la vue est déduit à partir du nom du contrôleur et le nom de l’action du contrôleur.

Si vous préférez, vous pouvez être explicite sur la vue. La ligne de code suivante retourne une vue nommée Fred :

Vue (Fred) ;

Lorsque cette ligne de code est exécutée, une vue est retournée à partir de l’emplacement suivant :

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Si vous envisagez de créer des tests unitaires pour votre application ASP.NET MVC puis il est judicieux d’être explicite sur les noms d’affichage. De cette façon, vous pouvez créer un test unitaire pour vérifier que la vue attendue a été retournée par une action de contrôleur.


## <a name="adding-content-to-a-view"></a>Ajout de contenu à une vue

Une vue est une norme de (document HTML qui peut contenir des scripts X). Vous utilisez des scripts pour ajouter du contenu dynamique à une vue.

Par exemple, la vue dans la liste 2 affiche la date et heure actuelles.

**Listing 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Notez que le corps de la page HTML dans le Listing 2 contient le script suivant :

&lt;% Response.Write(DateTime.Now);%&gt;

Vous utilisez les délimiteurs de script &lt;et %&gt; pour marquer le début et la fin d’un script. Ce script est écrit en c#. Il affiche la date et heure actuelles en appelant la méthode Response.Write () pour afficher le contenu dans le navigateur. Les délimiteurs de script &lt;et %&gt; peut être utilisée pour exécuter une ou plusieurs instructions.

Étant donné que vous appelez donc souvent Response.Write (), Microsoft vous offre un raccourci pour appeler la méthode Response.Write (). La vue dans la liste 3 utilise les délimiteurs &lt;% = et %&gt; sous forme de raccourci pour l’appel Response.Write ().

**Liste 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Vous pouvez utiliser n’importe quel langage .NET pour générer le contenu dynamique dans une vue. Normalement, vous allez utiliser Visual Basic .NET ou c# écrire vos contrôleurs et les vues.

## <a name="using-html-helpers-to-generate-view-content"></a>À l’aide de programmes d’assistance HTML pour générer le contenu de la vue

Pour le rendre plus facile d’ajouter du contenu à une vue, vous pouvez tirer parti de ce que l'on appelle un *programme d’assistance HTML*. Une application d’assistance HTML, en général, est une méthode qui génère une chaîne. Vous pouvez utiliser des programmes d’assistance HTML pour générer des éléments HTML standard tels que les zones de texte, les liens, les listes déroulantes et les zones de liste.

Par exemple, la vue dans la liste 4 tire parti de trois programmes d’assistance HTML--les programmes d’assistance BeginForm(), TextBox() et Password()--pour générer un compte de connexion forment (voir Figure 1).

**Liste 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![La boîte de dialogue Nouveau projet](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Figure 01**: un formulaire de connexion standard ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-views-overview-cs/_static/image2.png))


Toutes les méthodes de programmes d’assistance HTML sont appelées sur la propriété Html de la vue. Par exemple, vous afficher une zone de texte en appelant la méthode Html.TextBox().

Notez que vous utilisez les délimiteurs de script &lt;% = et %&gt; lors de l’appel de programmes d’assistance à la fois le Html.TextBox() et Html.Password(). Ces programmes d’assistance simplement retournent une chaîne. Vous devez appeler Response.Write () pour restituer la chaîne dans le navigateur.

L’utilisation de méthodes d’assistance HTML est facultative. Ils facilitent la vie en réduisant la quantité de code HTML et de script dont vous avez besoin d’écrire. La vue dans la liste 5 restitue la même forme exacte que la vue sur la liste 4 sans utiliser les programmes d’assistance HTML.

**Liste 5--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

Vous avez également la possibilité de créer vos propres programmes d’assistance HTML. Par exemple, vous pouvez créer une méthode d’assistance GridView() qui s’affiche automatiquement un ensemble d’enregistrements de base de données dans une table HTML. Nous explorons cette rubrique dans le didacticiel **création de programmes d’assistance HTML personnalisée**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>À l’aide de données d’affichage pour passer des données à une vue

Afficher les données vous permet de passer des données à partir d’un contrôleur à une vue. Considérez les données d’affichage comme un package que vous envoyez par courrier. Toutes les données transmises à partir d’un contrôleur à une vue doivent être envoyées à l’aide de ce package. Par exemple, le contrôleur dans la liste 6 ajoute un message à afficher les données.

**Liste 6 - ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

Le contrôleur de propriété ViewData représente une collection de paires nom / valeur. Dans la liste 6, la méthode Index() ajoute un élément à la collection de données d’affichage nommée message avec la valeur Hello World !. Lorsque la vue est retournée par la méthode Index(), les données d’affichage sont automatiquement passées à la vue.

La vue dans la liste 7 récupère le message à partir des données d’affichage et affiche le message dans le navigateur.

**Liste 7--\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Notez que la vue tire parti de la méthode d’assistance HTML de Html.Encode() lors du rendu du message. Le programme d’assistance HTML de Html.Encode() encode les caractères spéciaux tels que &lt; et &gt; en caractères qui sont sécurisés à afficher dans une page web. Chaque fois que vous restituez contenu fournie par un utilisateur à un site Web, vous devez coder le contenu pour empêcher les attaques par injection de JavaScript.

(Étant donné que nous avons créé le message nous-mêmes dans le ProductController, nous ne pas vraiment besoin encoder le message. Toutefois, il est une bonne habitude de toujours appeler la méthode Html.Encode() lors de l’affichage du contenu récupéré à partir de données d’affichage dans une vue.)

Dans la liste 7, nous avons tiré parti de données d’affichage pour passer un message de chaîne simple à partir d’un contrôleur à une vue. Afficher les données permettent également de transmettre d’autres types de données, telle qu’une collection d’enregistrements de base de données, à partir d’un contrôleur à une vue. Par exemple, si vous souhaitez afficher le contenu de la table de base de données de produits dans une vue, vous pouvez transmettre à la collection de base de données enregistre dans la vue données.

Vous avez également la possibilité de passer des données d’affichage fortement typées à partir d’un contrôleur à une vue. Nous explorons cette rubrique dans le didacticiel **compréhension fortement typées afficher les données et les vues**.

## <a name="summary"></a>Récapitulatif

Ce didacticiel fournit une brève introduction aux vues ASP.NET MVC, afficher les données et les programmes d’assistance HTML. Dans la première section, vous avez appris à ajouter de nouvelles vues à votre projet. Vous avez appris que vous devez ajouter une vue vers le dossier approprié pour appeler à partir d’un contrôleur spécifique. Ensuite, nous avons abordé le sujet de programmes d’assistance HTML. Vous avez appris comment HTML Helpers permettent de générer facilement du contenu HTML standard. Enfin, vous avez appris à tirer parti des données d’affichage pour passer des données à partir d’un contrôleur à une vue.

> [!div class="step-by-step"]
> [Next](creating-custom-html-helpers-cs.md)
