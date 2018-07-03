---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Affichage d’une Page d’erreur personnalisés (c#) | Microsoft Docs
author: rick-anderson
description: Ce que voit l’utilisateur lorsqu’une erreur d’exécution se produit dans une application web ASP.NET ? La réponse dépend du site Web &lt;customErrors&gt; configuration...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f2e9165acf86ce62d51e7c8d67205684d68e214
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362941"
---
<a name="displaying-a-custom-error-page-c"></a>Affichage d’une Page d’erreur personnalisés (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Ce que voit l’utilisateur lorsqu’une erreur d’exécution se produit dans une application web ASP.NET ? La réponse dépend du site Web &lt;customErrors&gt; configuration. Par défaut, les utilisateurs sont affichés à un écran jaune affreux vantant qu’une erreur d’exécution s’est produite. Ce didacticiel montre comment personnaliser ces paramètres à la page d’erreur personnalisée affichage un esthétiques qui correspond à apparence de votre site.


## <a name="introduction"></a>Introduction

Dans un monde parfait il n’y aura aucune erreur d’exécution. Les programmeurs seraient écrire du code avec n-aires un bogue et la validation d’entrée utilisateur robuste et externes ressources comme les serveurs de base de données et des serveurs de messagerie seraient jamais hors ligne. Bien sûr, en réalité, les erreurs sont inévitables. Les classes dans le .NET Framework indiquent une erreur en levant une exception. Par exemple, la méthode Open de l’objet appelant un objet SqlConnection établit une connexion à la base de données spécifié par une chaîne de connexion. Toutefois, si la base de données est arrêté, ou si les informations d’identification dans la chaîne de connexion ne sont pas valides la méthode Open lève un `SqlException`. Exceptions peuvent être gérées à l’aide de `try/catch/finally` blocs. Si le code situé dans un `try` bloc lève une exception, le contrôle est transféré au bloc catch approprié dans lequel le développeur peut tenter de récupérer à partir de l’erreur. S’il n’existe aucun bloc catch correspondant, ou si le code qui a levé l’exception n’est pas dans un bloc try, l’exception percolates la pile des appels à search de `try/catch/finally` blocs.

Si l’exception se propage jusqu’au runtime ASP.NET sans être géré, le [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)de [ `Error` événement](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) est déclenché et configuré *page d’erreur*  s’affiche. Par défaut, ASP.NET affiche une page d’erreur affectueusement parle le [écran jaune de décès](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Il existe deux versions de la YSOD : affiche les détails d’exception, une trace de pile et d’autres informations utiles aux développeurs de débogage de l’application (consultez **Figure 1**) ; l’autre indique simplement qu’il a été une erreur d’exécution (voir  **Figure 2**).

Détails de l’exception YSOD est très utile pour les développeurs de débogage de l’application, mais montrant un YSOD aux utilisateurs finaux est médiocre et non professionnelle. Au lieu de cela, les utilisateurs finaux doivent veiller à une page d’erreur qui gère l’apparence du site avec prose plus conviviaux décrivant la situation. La bonne nouvelle est que la création d’une telle page d’erreur personnalisée est très simple. Ce didacticiel commence par examiner ASP. Pages d’erreur différent de NET. Il montre ensuite comment configurer l’application web pour afficher les utilisateurs à une page d’erreur personnalisée face à une erreur.

### <a name="examining-the-three-types-of-error-pages"></a>Examiner les trois Types de Pages d’erreurs

Quand une exception non gérée se produit dans une application ASP.NET, un des trois types de pages d’erreur s’affiche :

- La page d’erreur Exception détails écran jaune de mort,
- La page d’erreur Runtime erreur écran jaune de mort, ou
- Une page d’erreur personnalisée

Les développeurs de pages d’erreur sont plus familier avec est le YSOD détails de l’Exception. Par défaut, cette page s’affiche aux utilisateurs qui visitent localement et par conséquent est la page que vous voyez quand une erreur se produit lorsque vous testez le site dans l’environnement de développement. Comme son nom l’indique, le YSOD détails de l’Exception fournit des détails sur l’exception : le type, le message et la trace de pile. De plus, si l’exception a été levée par le code dans la classe de code-behind de votre page ASP.NET, et si l’application est configurée pour le débogage puis le YSOD détails de l’Exception affiche également cette ligne de code (et quelques lignes de code ci-dessus et en dessous).

**Figure 1** montre la page YSOD des détails de l’Exception. Notez l’URL dans la fenêtre du navigateur adresse : `http://localhost:62275/Genre.aspx?ID=foo`. N’oubliez pas que le `Genre.aspx` page répertorie les critiques de livres d’un genre particulier. Il requiert que `GenreId` valeur (une `uniqueidentifier`) être acheminé à travers la chaîne de requête ; par exemple, l’URL appropriée pour afficher les révisions de fiction est `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Si un non -`uniqueidentifier` la valeur est passée dans la chaîne de requête (par exemple, « foo ») par le biais une exception est levée.

> [!NOTE]
> Pour reproduire cette erreur dans l’application web de démonstration disponible au téléchargement, vous pouvez visiter `Genre.aspx?ID=foo` directement ou cliquez sur le lien « Générer une erreur d’exécution » dans `Default.aspx`.


Notez les informations d’exception présentées dans **Figure 1**. Le message d’exception, « Échec de la Conversion lors de la conversion à partir d’une chaîne de caractères en uniqueidentifier » se trouve en haut de la page. Le type de l’exception, `System.Data.SqlClient.SqlException`, est également répertorié. Il existe également une trace de la pile.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figure 1**: détails de l’Exception YSOD inclut des informations relatives à l’Exception  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image3.png))

L’autre type de YSOD est la YSOD d’erreur de Runtime et est illustrée **Figure 2**. Le YSOD erreur Runtime informe le visiteur une erreur d’exécution s’est produite, mais elle n’inclut pas toutes les informations relatives à l’exception qui a été levée. (Il est le cas, toutefois, fournissent des instructions sur la façon d’afficher les détails de l’erreur en modifiant le `Web.config` fichier, qui fait partie de ce qui rend ce YSOD un aspect peu professionnel.)

Par défaut, le YSOD d’erreur de Runtime est affichée pour les utilisateurs qui accèdent à distance (via http://www.yoursite.com), comme le démontre l’URL dans la barre d’adresses du navigateur dans **Figure 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Les deux écrans YSOD différents existent parce que les développeurs sont intéressés de connaître les détails de l’erreur, mais ces informations ne doivent pas s’afficher sur un site de production comme il peut révéler des failles de sécurité potentielles ou d’autres informations sensibles à toute personne qui consulte votre site.

> [!NOTE]
> Si vous suivez et DiscountASP.NET comme votre hôte web, vous pouvez remarquer que le YSOD d’erreur de Runtime n’affiche pas lorsque vous visitez le site actif. Il s’agit, car DiscountASP.NET a leurs serveurs configurés pour afficher le YSOD de détails d’Exception par défaut. La bonne nouvelle est que vous pouvez remplacer ce comportement par défaut en ajoutant un `<customErrors>` section à votre `Web.config` fichier. La section « Configuration qui erreur Page s’affiche » examine le `<customErrors>` section en détail.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figure 2**: erreur d’exécution YSOD n’inclut pas les détails de l’erreur  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image6.png))

Le troisième type de page d’erreur est la page d’erreur personnalisée, qui est une page web que vous créez. L’avantage d’une page d’erreur personnalisés est que vous avez le contrôle total sur les informations qui s’affiche à l’utilisateur, ainsi qu’apparence de la page ; la page d’erreur personnalisée peut utiliser la même page maître et les styles en tant que vos autres pages. La section « À l’aide d’une Page d’erreur personnalisée » Guide de création d’une page d’erreur personnalisé et sa configuration à afficher en cas d’une exception non gérée. **Figure 3** offre en avant-première de cette page d’erreur personnalisée. Comme vous pouvez le voir, l’apparence de la page d’erreur est beaucoup plus qualité professionnelle à une des jaune écrans de décès indiqué dans les Figures 1 et 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figure 3**: une Page d’erreur personnalisée offre une apparence plus personnalisée  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image9.png))

Prenez un moment pour inspecter la barre d’adresses du navigateur dans **Figure 3**. Notez que la barre d’adresse affiche l’URL de la page d’erreur personnalisée (`/ErrorPages/Oops.aspx`). Dans les Figures 1 et 2, les écrans de décès jaune sont affiché dans la même page que l’origine de l’erreur (`Genre.aspx`). La page d’erreur personnalisée est passée à l’URL de la page où l’erreur s’est produite via le `aspxerrorpath` paramètre querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Configuration de Page d’erreur qui s’affiche.

Parmi les trois pages d’erreur possibles s’affiche est basée sur deux variables :

- Les informations de configuration dans la `<customErrors>` section, et
- Indique si l’utilisateur visite le site localement ou à distance.

Le [ `<customErrors>` section](https://msdn.microsoft.com/library/h0hfz6fc.aspx) dans `Web.config` possède deux attributs qui affectent la page d’erreur s’affiche : `defaultRedirect` et `mode`. L'attribut `defaultRedirect` est facultatif. Si fourni, il indique l’URL de la page d’erreur personnalisée et que la page d’erreur personnalisée doit être affichée à la place le YSOD erreur Runtime. Le `mode` attribut est requis et qu’il accepte l’une des trois valeurs suivantes : `On`, `Off`, ou `RemoteOnly`. Ces valeurs ont le comportement suivant :

- `On` -Indique que la page d’erreur personnalisée ou la YSOD d’erreur de Runtime est affichée pour tous les visiteurs, qu’ils soient locaux ou distants.
- `Off` -Spécifie que le YSOD détails de l’Exception est affichée pour tous les visiteurs, qu’ils soient locaux ou distants.
- `RemoteOnly` -Indique que la page d’erreur personnalisée ou la YSOD erreur Runtime est affichée pour les visiteurs à distance, tandis que le YSOD détails de l’Exception est présenté aux visiteurs locales.

Sauf indication contraire, ASP.NET se comporte comme si vous aviez la valeur est l’attribut mode `RemoteOnly` et n’avait pas spécifié un `defaultRedirect` valeur. En d’autres termes, le comportement par défaut est que le YSOD détails de l’Exception s’affiche aux visiteurs locales tandis que le YSOD d’erreur de Runtime est présenté aux visiteurs à distance. Vous pouvez substituer ce comportement par défaut en ajoutant un `<customErrors>` section à votre application web `Web.config file.`

## <a name="using-a-custom-error-page"></a>À l’aide d’une Page d’erreur personnalisée

Chaque application web doit avoir une page d’erreur personnalisée. Il fournit une alternative plus professionnelle à l’YSOD d’erreur de Runtime, il est facile de créer et configuration de l’application pour utiliser la page d’erreur personnalisé accepte uniquement un certain temps. La première étape consiste à créer la page d’erreur personnalisée. J’ai ajouté un nouveau dossier à l’application de critiques de livres nommée `ErrorPages` et ajouté à qu’une nouvelle page ASP.NET nommé `Oops.aspx`. Que la page à utiliser la même page maître en tant que le reste des pages de votre site afin qu’elle hérite automatiquement de la même apparence.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figure 4**: créer une Page d’erreur personnalisée

Ensuite, passez quelques minutes créer le contenu de la page d’erreurs. J’ai créé une page d’erreur personnalisée plutôt simple avec un message indiquant qu’une erreur inattendue est survenue et un lien renvoyant à la page d’accueil du site.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figure 5**: concevoir votre Page d’erreur personnalisée  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image14.png))

La page d’erreur terminé, configurez l’application web pour utiliser la page d’erreur personnalisée à la place la YSOD d’erreur de Runtime. Cela est accompli en spécifiant l’URL de la page d’erreur dans le `<customErrors>` de section `defaultRedirect` attribut. Ajoutez le balisage suivant à votre application `Web.config` fichier :

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Le balisage ci-dessus configure l’application pour afficher le YSOD de détails d’Exception pour les utilisateurs qui accèdent localement, tout en utilisant la page d’erreur personnalisée Oops.aspx pour ces utilisateurs accédant à distance. Pour voir cela en action, déployez votre site Web dans l’environnement de production, puis visitez la page Genre.aspx sur le site en direct avec une valeur de chaîne de requête non valide. Vous devez voir la page d’erreur personnalisée (font référence aux **Figure 3**).

Pour vérifier que la page d’erreurs personnalisées est uniquement visibles par les utilisateurs distants, visitez le `Genre.aspx` page avec une chaîne de requête non valide à partir de l’environnement de développement. Vous devez toujours voir les YSOD détails de l’Exception (font référence aux **Figure 1**). Le `RemoteOnly` paramètre s’assure que les utilisateurs qui visitent le site sur l’environnement de production consultez la page d’erreur personnalisée, tandis que les développeurs qui travaillent localement continuent de voir les détails de l’exception.

## <a name="notifying-developers-and-logging-error-details"></a>Informer les développeurs et la journalisation des détails de l’erreur

Les erreurs qui se produisent dans l’environnement de développement ont été provoquées par le développeur assis à son ordinateur. Elle s’affiche les informations de l’exception dans le YSOD détails de l’Exception, et elle sait quelle procédure elle effectuait lorsque l’erreur s’est produite. Mais lorsqu’une erreur se produit sur la production, le développeur ne connaît pas qu’une erreur s’est produite, sauf si l’utilisateur final sur le site prend du temps pour signaler l’erreur. Et même si l’utilisateur est hors de sa façon pour alerter l’équipe de développement, une erreur s’est produite, sans connaître le type d’exception, le message et le suivi de pile il peut être difficile de diagnostiquer la cause de l’erreur, sans parler de résoudre le problème.

Pour ces raisons, qu'il est essentiel que toute erreur dans l’environnement de production est enregistrée dans un magasin persistant (par exemple, une base de données) et que les développeurs sont informés de cette erreur. La page d’erreur personnalisée peut sembler un bon point pour effectuer cette journalisation et la notification. Malheureusement, la page d’erreur personnalisée n’a pas accès aux détails de l’erreur et ne peut donc pas être utilisée pour se connecter à ces informations. La bonne nouvelle est qu’il existe plusieurs façons d’intercepter les détails de l’erreur et de les consigner, et les trois didacticiels ce sujet, consultez plus en détail.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>À l’aide de Pages d’erreurs personnalisées différentes pour les États d’erreur HTTP différente

Lorsqu’une exception est levée par une page ASP.NET et n’est pas gérée, l’exception percolates jusqu'à l’exécution d’ASP.NET, qui affiche la page d’erreurs configuré. Si une demande est fourni dans le moteur ASP.NET mais ne peut pas être traitée pour une raison quelconque, par exemple le fichier demandé est introuvable ou lire les autorisations ont été désactivées pour le fichier - puis le moteur ASP.NET déclenche une `HttpException`. Cette exception, telles que les exceptions levées à partir de pages ASP.NET se propage jusqu'à l’exécution, à l’origine de la page d’erreur approprié à afficher.

Cela signifie pour l’application web en production est que si un utilisateur demande une page qui n’est pas trouvée, ils ne verront pas la page d’erreur personnalisée. **Figure 6** montre un exemple de ce type. Étant donné que la demande concerne une page inexistante (`NoSuchPage.aspx`), un `HttpException` est levée et la page d’erreur personnalisée s’affiche (Notez la référence à `NoSuchPage.aspx` dans le `aspxerrorpath` paramètre querystring).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figure 6**: le Runtime ASP.NET affiche la configuré erreur Page en réponse à une demande non valide ([cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image17.png))

Par défaut, tous les types d’erreurs provoquent la même page d’erreur personnalisée à afficher. Toutefois, vous pouvez spécifier une page d’erreur personnalisée différente pour un spécifique HTTP état code à l’aide `<error>` les éléments enfants dans la `<customErrors>` section. Par exemple, pour avoir une page d’erreur différents affichée en cas d’une page non trouvée erreur, ce qui a un code d’état HTTP de 404, mettre à jour le `<customErrors>` section à inclure le balisage suivant :

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Avec cette modification en place, chaque fois qu’un utilisateur visite à distance demande une ressource ASP.NET qui n’existe pas, ils seront redirigés vers le `404.aspx` page d’erreur personnalisée au lieu de `Oops.aspx`. En tant que **Figure 7** illustre, le `404.aspx` page peut inclure un message plus spécifique à la page d’erreur personnalisée générale.

> [!NOTE]
> Découvrez [des Pages d’erreur 404, une fois plus](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) pour obtenir des conseils sur la création de pages d’erreur 404 efficace.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Figure 7**: la Page d’erreur 404 personnalisée affiche un Message plus ciblé que `Oops.aspx`  
 ([Cliquez pour afficher l’image en taille réelle](displaying-a-custom-error-page-cs/_static/image20.png)) 

Étant donné que vous savez que le `404.aspx` page est atteint uniquement quand l’utilisateur effectue une demande pour une page qui n’a été trouvée, vous pouvez améliorer cette page d’erreur personnalisée pour inclure les fonctionnalités pour aider l’utilisateur à résoudre ce type d’erreur spécifique. Par exemple, vous pouvez créer une table de base de données qui mappe connue une URL incorrecte pour la bonne URL, puis le `404.aspx` personnalisé page d’erreur exécuter une requête sur la table et suggèrent des pages de l’utilisateur essaie d’atteindre.

> [!NOTE]
> La page d’erreur personnalisée s’affiche uniquement lorsqu’une demande est faite à une ressource gérée par le moteur ASP.NET. Comme expliqué dans la [principales différences entre IIS et le serveur de développement ASP.NET](core-differences-between-iis-and-the-asp-net-development-server-cs.md) didacticiel, le serveur web peut gérer certaines demandes lui-même. Par défaut, IIS web server traite les requêtes de contenu statique tel que des images et les fichiers HTML sans appeler le moteur ASP.NET. Par conséquent, si l’utilisateur demande un fichier inexistant image ils obtiennent en retour du message d’erreur 404 d’IIS par défaut plutôt que ASP. Page d’erreurs configuré du NET.


## <a name="summary"></a>Récapitulatif

Lorsqu’une exception non gérée se produit dans une application ASP.NET, l’utilisateur voit un des trois pages d’erreur : Exception détails jaune écran de décès ; Erreur d’exécution jaune écran décès. ou une page d’erreur personnalisée. Page erreur affichée dépend de l’application `<customErrors>` configuration et indique si l’utilisateur visite localement ou à distance. Le comportement par défaut consiste à afficher le YSOD de détails d’Exception aux visiteurs locales et le YSOD d’erreur de Runtime pour les visiteurs à distance.

Alors que le YSOD erreur Runtime masque les informations d’erreur potentiellement sensibles à partir de l’utilisateur visite le site, il s’arrête à partir de l’apparence de votre site et rende comportant des bogues de votre application. Une meilleure approche consiste à utiliser une page d’erreur personnalisée, ce qui implique la création et conception de la page d’erreur personnalisée et en spécifiant son URL dans le `<customErrors>` de section `defaultRedirect` attribut. Vous pouvez même avoir plusieurs pages d’erreurs personnalisées pour les différents États d’erreur HTTP.

La page d’erreur personnalisée est la première étape dans une stratégie pour un site Web en production de gestion d’erreur complète. Le développeur de l’erreur de génération d’alertes et les détails de la journalisation sont également des étapes importantes. Les trois didacticiels examinerai des techniques de notification d’erreur et la journalisation.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Pages d’erreur, une fois de plus](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Instructions de conception pour les exceptions](https://msdn.microsoft.com/library/ms229014.aspx)
- [Pages d’erreur convivial](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gestion et levée des Exceptions](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Correctement à l’aide des Pages d’erreurs personnalisées dans ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Précédent](strategies-for-database-development-and-deployment-cs.md)
> [Suivant](processing-unhandled-exceptions-cs.md)
