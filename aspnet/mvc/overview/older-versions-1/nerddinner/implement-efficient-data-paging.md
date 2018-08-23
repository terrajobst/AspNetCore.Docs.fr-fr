---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implémenter la pagination de données efficace | Microsoft Docs
author: microsoft
description: Étape 8 montre comment ajouter la prise en charge la pagination à notre URL /Dinners afin qu’au lieu d’afficher 1000 dîners en même temps, nous affichons uniquement 10 dîners à venir à...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2bef690355cd1f89a15a67f0c49775296d551136
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833178"
---
<a name="implement-efficient-data-paging"></a>Implémenter la pagination de données efficace
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 8 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 8 montre comment ajouter la prise en charge la pagination à notre URL /Dinners afin qu’au lieu d’afficher 1000 dîners en même temps, nous allons uniquement afficher 10 dîners à venir à la fois - et permettre aux utilisateurs finaux de page retour avant et, dans la liste entière d’une manière conviviale de moteurs de recherche.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner étape 8 : Prise en charge la pagination

Si notre site réussit, elle aura des milliers de dîners à venir. Nous avons besoin pour vous assurer que notre interface utilisateur met à l’échelle pour gérer toutes ces dîners et permet aux utilisateurs de parcourir les. Pour ce faire, nous allons ajouter la prise en charge la pagination pour notre */Dinners* URL afin qu’au lieu d’afficher des 1000 dîners à la fois, nous allons uniquement afficher 10 dîners à venir à la fois - et autoriser les utilisateurs finaux à la page principale avant et, dans la liste entière dans une manière conviviale de moteurs de recherche.

### <a name="index-action-method-recap"></a>Récapitulatif de méthode d’Action index()

La méthode d’action Index() au sein de notre classe DinnersController actuellement se présente comme ci-dessous :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quand une demande est faite pour le */Dinners* URL, il récupère une liste de tous les prochains dîners et restitue ensuite une liste de toutes les :

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Présentation IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  est une interface qui a été introduite avec LINQ en tant que partie du .NET 3.5. Il permet des scénarios puissants « exécution différée » dont nous pouvons tirer parti pour implémenter la prise en charge la pagination.

Dans notre DinnerRepository nous mettons en renvoyant un IQueryable&lt;dîner&gt; séquence à partir de notre méthode FindUpcomingDinners() :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;dîner&gt; objet retourné par notre méthode FindUpcomingDinners() encapsule une requête pour récupérer des objets de dîner à partir de notre base de données à l’aide de LINQ to SQL. Plus important encore, il s’exécute la requête sur la base de données jusqu'à ce que nous tentons d’accès/itérer sur les données dans la requête, ou jusqu'à ce que nous appelons la méthode ToList() dessus. Le code appelant notre méthode FindUpcomingDinners() pouvez éventuellement choisir d’ajouter « chaînées » opérations/filtres supplémentaires au IQueryable&lt;dîner&gt; objet avant d’exécuter la requête. LINQ to SQL, puis est suffisamment intelligente pour exécuter la requête combinée sur la base de données lorsque les données sont demandées.

Pour implémenter la logique de pagination nous pouvons mettre à jour méthode d’action de notre DinnersController Index() afin qu’elle s’applique des opérateurs « Skip » et « Take » supplémentaires au IQueryable retourné&lt;dîner&gt; séquence avant d’appeler ToList() dessus :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Le code ci-dessus ignore les 10 premières dîners à venir dans la base de données et renvoie ensuite 20 dîners. LINQ to SQL est suffisamment intelligent pour construire une requête SQL optimisée cette vérification est ignorée logique dans la base de données SQL et non dans le serveur web. Cela signifie que même si nous ont des millions de dîners à venir dans la base de données, seules les 10 que nous voulons sont récupérées dans le cadre de cette demande (rendant efficace et évolutive).

### <a name="adding-a-page-value-to-the-url"></a>Ajout d’une valeur de « page » à l’URL

Au lieu de coder en dur une plage de pages spécifique, nous devrons attraper nos URL pour inclure un paramètre « page » qui indique la plage qui dîner, un utilisateur demande.

#### <a name="using-a-querystring-value"></a>À l’aide d’une valeur de chaîne de requête

Le code ci-dessous montre comment nous pouvons mettre à jour notre méthode d’action Index() pour prendre en charge un paramètre de chaîne de requête et activer des URL telles que */Dinners ? page = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

La méthode d’action Index() ci-dessus a un paramètre nommé « page ». Le paramètre est déclaré comme un entier nullable (c'est-à-dire l’int ? indique). Cela signifie que le */Dinners ? page = 2* URL entraîne une valeur de « 2 » à passer en tant que la valeur du paramètre. Le */Dinners* URL (sans une valeur de chaîne de requête) entraîne une valeur null à passer.

Nous sommes en multipliant la valeur de la page par la taille de page (dans ce cas 10 lignes) pour déterminer combien dîners à ignorer. Nous utilisons le [c# » « opérateur de fusion null ( ?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) ce qui est utile lorsque vous traitez des types nullable. Le code ci-dessus assigne page à la valeur 0 si le paramètre de page est null.

#### <a name="using-embedded-url-values"></a>À l’aide de valeurs d’URL incorporées

Une alternative à l’aide d’une valeur de chaîne de requête serait d’incorporer le paramètre de page au sein de l’URL réelle proprement dite. Par exemple : */Dinners/Page/2* ou */dîners/2*. ASP.NET MVC inclut un puissant moteur de routage d’URL qui facilite la prise en charge de tels scénarios.

Nous pouvons enregistrer des règles de routage personnalisées qui correspondent à n’importe quel URL ou les URL entrantes format à n’importe quelle méthode de classe ou une action de contrôleur nous voulons. Il nous suffit d’action consiste à ouvrir le fichier Global.asax dans notre projet :

![](implement-efficient-data-paging/_static/image2.png)

Puis enregistrez une nouvelle règle de mappage à l’aide de la méthode d’assistance MapRoute() comme le premier appel à des itinéraires. MapRoute() ci-dessous :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Ci-dessus, nous enregistrons une nouvelle règle de routage nommée « UpcomingDinners ». Il a le format d’URL, nous indiquons « dîners/Page / {page} », où {page} est une valeur de paramètre incorporée dans l’URL. Le troisième paramètre à la méthode MapRoute() indique que nous devons mapper des URL qui correspondent à ce format pour la méthode d’action Index() sur la classe DinnersController.

Nous pouvons utiliser le même Index() code que nous avions auparavant avec notre scénario de chaîne de requête – sauf venu de notre paramètre « page » à partir de l’URL et pas la chaîne de requête :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Et maintenant quand nous pouvons exécuter l’application et que vous tapez dans */Dinners* nous verrons les 10 premières dîners à venir :

![](implement-efficient-data-paging/_static/image3.png)

Lorsque nous tapez dans */Dinners/Page/1* nous allons voir la page suivante de dîners :

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Ajout d’interface utilisateur de navigation de page

La dernière étape pour terminer notre scénario d’échange sera pour implémenter des « suivant » et « précédent » IU de navigation au sein de notre modèle de vue permet aux utilisateurs d’ignorer facilement les données dîner.

Pour implémenter correctement, nous allons devoir connaître le nombre total de dîners dans la base de données, ainsi que la manière dont plusieurs pages de données cela se traduit par. Nous devons ensuite calculer si la valeur de la page actuellement demandée « » est au début ou à la fin des données et afficher ou masquer l’interface utilisateur « précédent » et « next » en conséquence. Nous pourrions implémenter cette logique dans notre méthode d’action Index(). Nous pouvons également ajouter une classe d’assistance à notre projet qui encapsule cette logique de manière plus réutilisable.

Voici une classe d’assistance « PaginatedList » simple qui dérive de la liste&lt;T&gt; classe de collection basée sur le .NET Framework. Il implémente une classe de collection réutilisable qui peut être utilisée pour paginer n’importe quelle séquence de données de IQueryable. Dans notre application NerdDinner nous aurons il fonctionne sur un IQueryable&lt;dîner&gt; résultats, mais il pourrait tout aussi facilement être utilisée contre IQueryable&lt;produit&gt; ou IQueryable&lt;client&gt;entraîne d’autres scénarios d’application :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Vous remarquerez ci-dessus de la façon dont il calcule et puis expose des propriétés telles que « PageIndex », « PageSize », « TotalCount » et « TotalPages ». Il expose également deux propriétés d’assistance « HasPreviousPage » et « HasNextPage » qui indiquent si la page de données dans la collection est au début ou à la fin de la séquence d’origine. Le code ci-dessus génère deux requêtes SQL à exécuter - la première pour récupérer le nombre du nombre total d’objets de dîner (cela ne retourne pas les objets : au lieu de cela, il exécute une instruction « SELECT COUNT » qui retourne un entier) et le second pour récupérer uniquement les lignes de données que nous avons besoin de notre base de données pour la page de données actuelle.

Nous pouvons ensuite à jour notre méthode d’assistance de DinnersController.Index() pour créer un PaginatedList&lt;dîner&gt; à partir de notre DinnerRepository.FindUpcomingDinners() entraîner et transmettez-le à notre modèle de vue :

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Nous pouvons puis mettez à jour le modèle de vue \Views\Dinners\Index.aspx hériter ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;dîner&gt; &gt; au lieu de ViewPage&lt;IEnumerable&lt;Dîner&gt;&gt;, puis ajoutez le code suivant au bas de notre modèle de vue pour afficher ou masquer l’interface utilisateur de navigation suivant et précédent :

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Vous remarquerez ci-dessus de la façon dont nous utilisons la méthode d’assistance Html.RouteLink() générer notre des liens hypertexte. Cette méthode est similaire à la méthode d’assistance Html.ActionLink() que nous avons utilisée précédemment. La différence est que nous allons générer l’URL à l’aide de la règle que nous préparons au sein de notre fichier Global.asax de routage « UpcomingDinners ». Cela garantit que nous allons générer des URL à notre méthode d’action Index() qui ont le format : */Dinners/Page / {page}* – où la valeur {page} est une variable, nous offrons ci-dessus selon le PageIndex actuel.

Et lorsque nous exécutons notre application à nouveau nous allons maintenant voir 10 dîners à la fois dans le navigateur :

![](implement-efficient-data-paging/_static/image5.png)

Nous avons également &lt; &lt; &lt; et &gt; &gt; &gt; IU en bas de la page qui nous permette d’ignorer vers l’avant et vers l’arrière sur nos données à l’aide de rechercher les URL accessibles de moteur de navigation :

![](implement-efficient-data-paging/_static/image6.png)

| **Rubrique de côté : Comprendre les implications de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; est une fonctionnalité très puissante qui permet une variété de scénarios intéressants de l’exécution différée (telles que la pagination et la composition en fonction des requêtes). Comme avec toutes les fonctionnalités puissantes, vous souhaitez être prudent avec leur utilisation et vous assurer qu’il n’est pas abusé. Il est important de reconnaître que retourner un IQueryable&lt;T&gt; résultat à partir de votre référentiel permet au code appelant ajouter sur les méthodes d’opérateur chaînées à celui-ci et ainsi participer à l’exécution des requêtes ultimate. Si vous ne souhaitez pas fournir de code appelant cette possibilité, vous devez retourner sauvegarder IList&lt;T&gt; ou IEnumerable&lt;T&gt; résultats - qui contiennent les résultats d’une requête qui a déjà été exécutée. Pour les scénarios de pagination, cela nécessiterait vous permettant de transmettre la logique de la pagination des données réelles à la méthode de référentiel appelée. Dans ce scénario nous pouvons mettre à jour notre méthode de recherche FindUpcomingDinners() pour avoir une signature que soit retourné une PaginatedList : PaginatedList&lt; dîner&gt; {} FindUpcomingDinners (pageIndex d’int, int pageSize) ou retour vers l’arrière un IList &lt;Dîner&gt;et un « totalCount » out param pour renvoyer le nombre total de dîners : IList&lt;dîner&gt; FindUpcomingDinners (int pageIndex, pageSize int, out int totalCount) {} |

### <a name="next-step"></a>Étape suivante

Voyons maintenant comment nous pouvons ajouter la prise en charge de l’authentification et autorisation à notre application.

> [!div class="step-by-step"]
> [Précédent](re-use-ui-using-master-pages-and-partials.md)
> [Suivant](secure-applications-using-authentication-and-authorization.md)
