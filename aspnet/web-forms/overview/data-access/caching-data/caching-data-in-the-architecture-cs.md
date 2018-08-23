---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: La mise en cache des données dans l’Architecture (c#) | Microsoft Docs
author: rick-anderson
description: Dans le didacticiel précédent, nous avons appris à appliquer la mise en cache au niveau de la couche de présentation. Dans ce didacticiel, nous apprendre à tirer parti de notre architectu en couche...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 3971140aa7a6c829287e74df804694c19e34adcf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826159"
---
<a name="caching-data-in-the-architecture-c"></a>La mise en cache des données dans l’Architecture (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) ou [télécharger le PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> Dans le didacticiel précédent, nous avons appris à appliquer la mise en cache au niveau de la couche de présentation. Dans ce didacticiel, nous Apprenez à tirer parti de notre architecture en couches en cache des données à la couche de logique métier. Nous cela en étendant l’architecture pour inclure une couche de mise en cache.


## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans le didacticiel précédent, la mise en cache les données de s ObjectDataSource est aussi simple que de définir deux propriétés. Malheureusement, ObjectDataSource s’applique la mise en cache dans la couche de présentation, qui associe étroitement les stratégies de mise en cache avec la page ASP.NET. Une des raisons pour la création d’une architecture en couches consiste à autoriser ces couplages être rompu. La couche de logique métier, dissocie par exemple, la logique métier à partir des pages ASP.NET, tandis que la couche Data Access dissocie les détails d’accès aux données. Ce découplage des détails d’accès logique et les données métier est recommandée, en partie, car elle rend le système plus lisible, plus facile à gérer et plus flexible pour la modifier. Il permet également de connaissance du domaine et de répartition du travail d’un développeur qui travaille sur la couche de présentation ne t devez être familiarisé avec les détails de s de base de données pour faire son travail. Découplage de la stratégie de mise en cache à partir de la couche de présentation offre des avantages similaires.

Dans ce didacticiel, nous augmenterons notre architecture d’inclure un *la mise en cache de couche* (ou CL pour faire plus court) qui utilise notre stratégie de mise en cache. La couche de mise en cache inclura un `ProductsCL` classe qui fournit l’accès aux informations sur les produits avec des méthodes telles que `GetProducts()`, `GetProductsByCategoryID(categoryID)`, et ainsi de suite, que, lorsqu’elle est appelée, sera première tentative pour récupérer les données à partir du cache. Si le cache est vide, ces méthodes appellera approprié `ProductsBLL` méthode dans la couche BLL, qui à son tour recevriez les données à partir de la couche DAL. Le `ProductsCL` méthodes mettre en cache les données récupérées à partir de la couche BLL avant de le renvoyer.

Comme le montre la Figure 1, le CL réside entre la présentation et les couches de logique métier.


![La couche de mise en cache (CL) est une autre couche dans notre Architecture](caching-data-in-the-architecture-cs/_static/image1.png)

**Figure 1**: la mise en cache de couche (CL) est une autre couche dans notre Architecture


## <a name="step-1-creating-the-caching-layer-classes"></a>Étape 1 : Création des Classes de couche mise en cache

Dans ce didacticiel, nous allons créer un CL très simple avec une seule classe `ProductsCL` qui a uniquement un certain nombre de méthodes. Création d’une couche de mise en cache complète pour l’application entière nécessiterait la création `CategoriesCL`, `EmployeesCL`, et `SuppliersCL` classes et en fournissant une méthode dans ces classes de la couche de mise en cache pour chaque méthode d’accès ou modification des données dans la couche BLL. Comme avec la couche BLL et la couche DAL, la couche de mise en cache doit idéalement être implémentée comme un projet de bibliothèque de classes distinct ; Toutefois, nous sera l’implémenter en tant que classe dans le `App_Code` dossier.

Aux classes distinct proprement le CL plus à partir des classes DAL et la couche BLL, s permettent de créer un nouveau sous-dossier dans le `App_Code` dossier. Avec le bouton droit sur le `App_Code` dossier dans l’Explorateur de solutions, choisissez le nouveau dossier et nommez le nouveau dossier `CL`. Après avoir créé ce dossier, ajouter à celui-ci, une nouvelle classe nommée `ProductsCL.cs`.


![Ajouter un nouveau dossier nommé CL et une classe nommée ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Figure 2**: ajouter un nouveau dossier nommé `CL` et une classe nommée `ProductsCL.cs`


Le `ProductsCL` classe doit inclure le même ensemble de méthodes d’accès et de modification de données telle qu’elle figure dans sa classe correspondante de la couche de logique métier (`ProductsBLL`). Plutôt que de créer toutes ces méthodes, let s construisez simplement quelques ici pour avoir une idée pour les modèles utilisés par le CL. En particulier, nous allons ajouter le `GetProducts()` et `GetProductsByCategoryID(categoryID)` méthodes à l’étape 3 et un `UpdateProduct` de surcharge à l’étape 4. Vous pouvez ajouter les autres `ProductsCL` méthodes et `CategoriesCL`, `EmployeesCL`, et `SuppliersCL` classes à votre convenance.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Étape 2 : Lire et écrire dans le Cache de données

ObjectDataSource exploré en interne dans le didacticiel précédent de mise en cache utilise le cache de données ASP.NET pour stocker les données récupérées à partir de la couche BLL. Le cache de données est également accessible par programmation à partir de classes de code-behind de pages ASP.NET ou à partir des classes dans l’architecture de s d’application web. Pour lire et écrire dans le cache de données à partir de la classe code-behind s de pages ASP.NET, utilisez le modèle suivant :


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

Le [ `Cache` classe](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` méthode](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) a un nombre de surcharges. `Cache["key"] = value` et `Cache.Insert(key, value)` sont synonymes et à la fois ajoutent un élément au cache à l’aide de la clé spécifiée sans une expiration définie. En règle générale, nous voulons spécifier une expiration lors de l’ajout d’un élément au cache, soit comme une dépendance, une expiration temporelle ou les deux. Utilisez une des autres `Insert` des surcharges de méthode s pour fournir des informations d’expiration en fonction de dépendance ou de temps.

La couche de mise en cache des méthodes de s doivent d’abord vérifier si les données demandées sont dans le cache et, dans ce cas, il permet de retourner à partir de là. Si les données demandées ne sont pas dans le cache, la méthode appropriée de la couche BLL doit être appelé. Sa valeur de retour doit être mis en cache et puis retourné, comme l’illustre le diagramme de séquence suivant.


![Les méthodes de s de couche de mise en cache de retournent des données à partir du Cache si elle s disponible](caching-data-in-the-architecture-cs/_static/image3.png)

**Figure 3**: la couche de mise en cache s méthodes retournent des données à partir du Cache si elle s disponible


La séquence présentée dans la Figure 3 s’effectue dans les classes CL au format suivant :


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

Ici, *Type* est le type de données stockées dans le cache `Northwind.ProductsDataTable`, par exemple *clé* est la clé qui identifie de façon unique l’élément de cache. Si l’élément avec la valeur *clé* n’est pas dans le cache, alors *instance* sera `null` et les données sont récupérées à partir de la méthode appropriée de la couche BLL et ajoutées au cache. Au terme du délai `return instance` est atteinte, *instance* contient une référence aux données, soit à partir du cache ou extraites à partir de la couche BLL.

Veillez à utiliser le modèle ci-dessus lorsqu’ils accèdent à des données à partir du cache. Le modèle suivant, qui, à première vue, recherche équivalent, contient une différence subtile qui présente une condition de concurrence. Conditions de concurrence sont difficiles à déboguer, car ils se révèlent sporadique et sont difficiles à reproduire.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

La différence dans ce second, extrait de code incorrect est qui plutôt que de stocker une référence à l’élément mis en cache dans une variable locale, le cache de données est accessible directement dans l’instruction conditionnelle *et* dans le `return`. Imaginez que lorsque ce code est atteinte, `Cache["key"]` est non -`null`, mais avant que le `return` instruction est atteinte, le système évince *clé* à partir du cache. Dans ce cas rares, le code retournera un `null` valeur au lieu d’un objet du type attendu.

> [!NOTE]
> Le cache de données est thread-safe, donc vous n’avez pas besoin pour synchroniser l’accès de thread pour la simple lecture ou d’écriture. Toutefois, si vous avez besoin effectuer plusieurs opérations sur les données dans le cache qui doivent être atomiques, vous êtes chargé d’implémenter un verrou ou tout autre mécanisme pour garantir la sécurité des threads. Consultez [synchroniser l’accès au Cache ASP.NET](http://www.ddj.com/184406369) pour plus d’informations.


Un élément peut être supprimé par programme à partir du cache de données à l’aide de la [ `Remove` méthode](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) comme suit :


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Étape 3 : Retour d’informations de produit à partir de la`ProductsCL`classe

Pour ce didacticiel permettre s implémenter deux méthodes pour retourner des informations de produit à partir de la `ProductsCL` classe : `GetProducts()` et `GetProductsByCategoryID(categoryID)`. Comme avec la `ProductsBL` classe dans la couche de logique métier, le `GetProducts()` méthode dans le CL retourne des informations sur tous les produits comme un `Northwind.ProductsDataTable` objet, tandis que `GetProductsByCategoryID(categoryID)` retourne tous les produits à partir d’une catégorie spécifiée.

Le code suivant montre une partie des méthodes dans la `ProductsCL` classe :


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

Tout d’abord, notez le `DataObject` et `DataObjectMethodAttribute` attributs appliqués à la classe et les méthodes. Ces attributs fournissent des informations à l’Assistant de s ObjectDataSource, indiquant que les classes et méthodes doivent apparaître dans les étapes de l’Assistant s. Étant donné que les classes de CL et méthodes seront accessible à partir de l’ObjectDataSource dans la couche de présentation, j’ai ajouté ces attributs pour améliorer l’expérience au moment du design. Faire référence à la [création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-cs.md) didacticiel pour obtenir une description plus approfondie sur ces attributs et leurs effets.

Dans le `GetProducts()` et `GetProductsByCategoryID(categoryID)` méthodes, les données retournées à partir de la `GetCacheItem(key)` méthode est attribuée à une variable locale. Le `GetCacheItem(key)` (méthode), ce qui nous examinerons rapidement, retourne un élément particulier à partir du cache en fonction du *clé*. Si aucune donnée n’est trouvée dans le cache, il est récupéré à partir de le correspondantes `ProductsBLL` méthode de classe, puis ajouté au cache en utilisant le `AddCacheItem(key, value)` (méthode).

Le `GetCacheItem(key)` et `AddCacheItem(key, value)` méthodes créent une interface avec le cache de données, la lecture et l’écriture des valeurs, respectivement. Le `GetCacheItem(key)` méthode est la plus simple des deux. Il renvoie simplement la valeur de la classe de Cache passé dans le *clé*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` n’utilise pas *clé* valeur tel que fourni, mais plutôt des appels le `GetCacheKey(key)` (méthode), qui retourne le *clé* avec ProductsCache - préfixe. Le `MasterCacheKeyArray`, qui contient la chaîne ProductsCache, est également utilisé par le `AddCacheItem(key, value)` (méthode), comme nous allons le voir momentanément.

À partir d’une classe code-behind de pages ASP.NET, le cache de données est accessible à l’aide de la `Page` classe s [ `Cache` propriété](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)et permet une syntaxe telle que `Cache["key"] = value`, comme indiqué à l’étape 2. À partir d’une classe au sein de l’architecture, le cache de données est accessible à l’aide `HttpRuntime.Cache` ou `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)d’entrée de blog [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) notes à l’avantage légèrement les performances à l’aide de `HttpRuntime` au lieu de `HttpContext.Current`; par conséquent, `ProductsCL` utilise `HttpRuntime`.

> [!NOTE]
> Si votre architecture est implémentée à l’aide de projets de bibliothèque de classes, vous devez ajouter une référence à la `System.Web` assembly afin d’utiliser le [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) et [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) classes.


Si l’élément est introuvable dans le cache, le `ProductsCL` méthodes de la classe s obtenir les données à partir de la couche BLL et l’ajouter au cache en utilisant le `AddCacheItem(key, value)` (méthode). Pour ajouter *valeur* au cache, nous pourrions utiliser le code suivant, qui utilise une expiration de 60 secondes :


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` Spécifie l’expiration temporelle 60 secondes dans le futures while [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) indique il s sans date d’expiration décalée. Bien que ceci `Insert` surcharge de méthode a paramètres d’entrée pour les deux une absolue et décalée d’expiration, vous pouvez fournir uniquement une des deux. Si vous tentez de spécifier une heure absolue et un intervalle de temps, le `Insert` méthode lèvera une `ArgumentException` exception.

> [!NOTE]
> Cette implémentation de la `AddCacheItem(key, value)` méthode présente actuellement quelques défauts. Nous allons répondre et résoudre ces problèmes à l’étape 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Étape 4 : Invalider les données du Cache lorsque l’est modifié par le biais de l’Architecture

En même temps que les méthodes de récupération de données, la couche de mise en cache doit fournir les mêmes méthodes que la couche BLL pour l’insertion, la mise à jour et suppression de données. Les méthodes de modification de données CL s ne modifient pas les données en cache, mais plutôt appeler la méthode de modification de données correspondante BLL s et puis invalident le cache. Comme nous l’avons vu dans le didacticiel précédent, il s’agit du même comportement que ObjectDataSource s’applique lorsque ses fonctionnalités de mise en cache sont activées et son `Insert`, `Update`, ou `Delete` méthodes sont appelées.

Ce qui suit `UpdateProduct` surcharge illustre comment implémenter les méthodes de modification de données dans le CL :


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

La modification de données approprié méthode de la couche de logique métier est appelée, mais avant sa réponse est retournée que nous avons besoin invalider le cache. Malheureusement, invalider le cache n’est pas simple, car le `ProductsCL` classe s `GetProducts()` et `GetProductsByCategoryID(categoryID)` méthodes chacun ajoutent des éléments au cache avec des clés différentes et le `GetProductsByCategoryID(categoryID)` méthode ajoute un élément de cache différent pour chaque unique *categoryID*.

Lors de l’invalidation du cache, nous devons supprimer *tous les* des éléments qui peuvent avoir été ajoutés par le `ProductsCL` classe. Cela est possible en associant un *la dépendance de cache* avec chaque élément ajouté au cache dans le `AddCacheItem(key, value)` (méthode). En règle générale, une dépendance de cache peut être un autre élément dans le cache, un fichier sur le système de fichiers ou des données à partir d’une base de données Microsoft SQL Server. Lorsque la dépendance est modifié ou supprimé du cache, les éléments du cache lui est associée sont automatiquement supprimés du cache. Pour ce didacticiel, nous voulons créer un élément supplémentaire dans le cache qui sert d’une dépendance de cache pour tous les éléments ajoutés via la `ProductsCL` classe. De cette façon, tous ces éléments peuvent être supprimé du cache en supprimant simplement la dépendance de cache.

Mise à jour s Let le `AddCacheItem(key, value)` méthode afin que chaque élément ajouté au cache via cette méthode est associée à une dépendance de cache unique :


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` est un tableau de chaînes qui contient une valeur unique, ProductsCache. Tout d’abord, un élément de cache est ajouté au cache et affecté la date et heure actuelles. Si l’élément de cache existe déjà, il est mis à jour. Ensuite, une dépendance de cache est créée. Le [ `CacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) constructeur de s a un nombre de surcharges, mais celui qui est utilisé ici attend deux `string` entrées de tableau. La première Spécifie l’ensemble de fichiers à utiliser en tant que dépendances. Étant donné que nous n t préférable d’utiliser les dépendances basées sur fichier, une valeur de `null` est utilisé pour le premier paramètre d’entrée. Le deuxième paramètre d’entrée spécifie le jeu de clés de cache à utiliser en tant que dépendances. Ici, nous spécifions notre dépendance unique, `MasterCacheKeyArray`. Le `CacheDependency` est ensuite passé à la `Insert` (méthode).

Avec cette modification à `AddCacheItem(key, value)`, invaliding le cache est aussi simple que la suppression de la dépendance.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Étape 5 : Appel de la couche de mise en cache à partir de la couche de présentation

La mise en cache de couche s classes et des méthodes peuvent servir à manipuler les données en utilisant les techniques nous ve examiné tout au long de ces didacticiels. Pour illustrer l’utilisation des données mises en cache, enregistrer vos modifications dans le `ProductsCL` classe, puis ouvrez le `FromTheArchitecture.aspx` page dans le `Caching` dossier et ajoutez un GridView. À partir de la balise active de s GridView, créez un nouveau ObjectDataSource. Dans l’étape de première Assistant s vous devriez voir le `ProductsCL` classe en tant qu’une des options dans la liste déroulante.


[![La classe ProductsCL est incluse dans la liste déroulante d’objets Business](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Figure 4**: le `ProductsCL` classe est incluse dans la liste déroulante d’objets Business ([cliquez pour afficher l’image en taille réelle](caching-data-in-the-architecture-cs/_static/image6.png))


Après avoir sélectionné `ProductsCL`, cliquez sur Suivant. La liste déroulante dans l’onglet sélection a deux éléments - `GetProducts()` et `GetProductsByCategoryID(categoryID)` et l’onglet de mise à jour a l’unique `UpdateProduct` de surcharge. Choisissez le `GetProducts()` méthode à partir de l’onglet Sélection et la `UpdateProducts` méthode à partir de l’onglet de mise à jour et cliquez sur Terminer.


[![Les méthodes de classe ProductsCL s sont répertoriés dans la liste déroulante répertorie](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Figure 5**: le `ProductsCL` les méthodes de classe s sont répertoriées dans la liste déroulante répertorie ([cliquez pour afficher l’image en taille réelle](caching-data-in-the-architecture-cs/_static/image9.png))


À l’issue de l’Assistant, Visual Studio définit les opérations de mappage ObjectDataSource `OldValuesParameterFormatString` propriété `original_{0}` et ajoutez les champs appropriés pour le contrôle GridView. Modifier le `OldValuesParameterFormatString` propriété à sa valeur par défaut, `{0}`et configurer le contrôle GridView pour prendre en charge la pagination, de tri et de modification. Dans la mesure où le `UploadProducts` surcharge utilisée par le CL accepte uniquement le nom de produit modifié s et les prix, limiter le contrôle GridView afin que seuls ces champs sont modifiables.

Dans le didacticiel précédent, nous avons défini un GridView pour inclure des champs pour le `ProductName`, `CategoryName`, et `UnitPrice` champs. N’hésitez pas à répliquer cette mise en forme et la structure, auquel cas votre s GridView et ObjectDataSource déclarative balisage doit ressembler à ce qui suit :


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

À ce stade, nous avons une page qui utilise la couche de mise en cache. Pour afficher le cache en action, définissez des points d’arrêt le `ProductsCL` classe s `GetProducts()` et `UpdateProduct` méthodes. Visitez la page dans un navigateur et les parcourir le code lors du tri et pagination afin de voir les données extraites à partir du cache. Mettre à jour un enregistrement, puis notez que le cache est invalidé et, par conséquent, il est récupéré à partir de la couche BLL lorsque les données sont de nouveau liées au GridView.

> [!NOTE]
> La couche de mise en cache fournis dans le téléchargement qui accompagne cet article n’est pas terminée. Il contient uniquement une classe, `ProductsCL`, qui arbore uniquement un certain nombre de méthodes. En outre, une seule page ASP.NET utilise le CL (`~/Caching/FromTheArchitecture.aspx`) tous les autres toujours faire référence à la couche BLL directement. Si vous prévoyez d’utiliser un CL dans votre application, tous les appels à partir de la couche de présentation doivent atteindre CL, ce qui nécessiterait que les classes de s CL et méthodes couvert ces classes et méthodes dans la couche BLL actuellement utilisée par la couche de présentation.


## <a name="summary"></a>Récapitulatif

Alors que la mise en cache peut être appliquée à la couche de présentation avec ASP.NET 2.0 s SqlDataSource et ObjectDataSource contrôles, dans l’idéal, la mise en cache responsabilités est déléguée à une couche distincte dans l’architecture. Dans ce didacticiel, nous avons créé une couche de mise en cache qui se trouve entre la couche de présentation et la couche de logique métier. La couche de mise en cache doit fournir le même ensemble de classes et méthodes qui existent dans la couche BLL et sont appelées à partir de la couche de présentation.

Les exemples de mise en cache de couche que nous avons exploré dans cette section et les didacticiels précédents affichait *chargement réactive*. Lorsque le chargement réactive, les données sont chargées dans le cache uniquement lorsqu’une demande de données est effectuée et que les données manquantes à partir du cache. Données peuvent également être *chargé de façon proactive* dans le cache, une technique qui charge les données dans le cache avant qu’il est réellement nécessaire. Dans le didacticiel suivant, nous verrons un exemple de chargement proactive lorsque nous examinerons comment stocker des valeurs statiques dans le cache au démarrage de l’application.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Teresa Murph. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](caching-data-with-the-objectdatasource-cs.md)
> [Suivant](caching-data-at-application-startup-cs.md)
