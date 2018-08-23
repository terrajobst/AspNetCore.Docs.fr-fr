---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: La mise en cache des données au démarrage de l’Application (VB) | Microsoft Docs
author: rick-anderson
description: Dans n’importe quelle application Web certaines données sont fréquemment utilisées et certaines données seront rarement utilisées. Nous pouvons améliorer les performances de notre b d’application ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 9d997cb0af8487b2a9d849f24f3bff8df8611f6d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827480"
---
<a name="caching-data-at-application-startup-vb"></a>La mise en cache des données au démarrage de l’Application (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> Dans n’importe quelle application Web certaines données sont fréquemment utilisées et certaines données seront rarement utilisées. Nous pouvons améliorer les performances de notre application ASP.NET en chargeant à l’avance les données fréquemment utilisées, une technique appelée. Ce didacticiel illustre une approche proactive chargement, qui consiste à charger des données dans le cache au démarrage de l’application.


## <a name="introduction"></a>Introduction

Les didacticiels précédents deux examiné la mise en cache des données dans la présentation et les couches de la mise en cache. Dans [la mise en cache des données avec ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), nous avons examiné à l’aide de la s ObjectDataSource fonctionnalités en cache des données dans la couche de présentation de la mise en cache. [La mise en cache des données dans l’Architecture](caching-data-in-the-architecture-vb.md) examiné la mise en cache dans une couche de mise en cache séparée. Les deux de ces didacticiels utilisés *chargement réactive* dans l’utilisation du cache de données. Avec réactive le chargement des, chaque fois que les données sont demandées, le système vérifie d’abord si elle s dans le cache. Si ce n’est pas le cas, il extrait les données à partir de la source d’origine, telles que la base de données, puis la stocke dans le cache. Le principal avantage de chargement réactive est sa facilité d’implémentation. Un de ses inconvénients est ses performances inégale entre les requêtes. Imaginez une page qui utilise la couche de mise en cache du didacticiel précédent pour afficher des informations de produit. Lorsque cette page est visitée pour la première fois ou visitée pour la première fois après que les données en cache a été supprimées en raison des contraintes de mémoire ou l’expiration spécifiée ayant été atteint, les données doivent être récupérées à partir de la base de données. Par conséquent, ces demandes d’utilisateurs prendront plus de demandes d’utilisateurs qui peuvent être fournis par le cache.

*Chargement proactive* fournit une stratégie de gestion de cache autre qui lisse les performances entre les requêtes en chargeant les données mises en cache avant leur s nécessité. En règle générale, le chargement proactive utilise un processus qui vérifie périodiquement ou est averti en cas d’une mise à jour les données sous-jacentes. Ce processus met à jour le cache pour conserver la nouvelle. Chargement proactive est particulièrement utile si les données sous-jacentes proviennent d’une connexion lente de la base de données, un service Web ou une autre source de données particulièrement lente. Mais cette approche proactive chargement est plus difficile à implémenter, car il nécessite la création, la gestion et le déploiement d’un processus pour vérifier les modifications et mettre à jour le cache.

Une autre version du chargement proactive et dans ce didacticiel, nous allons examiner le type de chargement des données dans le cache au démarrage de l’application. Cette approche est particulièrement utile pour la mise en cache des données statiques, telles que les enregistrements dans les tables de recherche de base de données.

> [!NOTE]
> Pour un aperçu plus approfondi les différences entre proactif et réactif le chargement, ainsi que les listes de professionnels de l’informatique, les inconvénients et les recommandations d’implémentation, reportez-vous à la [gestion du contenu d’un Cache](https://msdn.microsoft.com/library/ms978503.aspx) section de la [ Guide d’Architecture pour les Applications .NET Framework de la mise en cache](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Étape 1 : Déterminer quelles données au Cache au démarrage de l’Application

Les exemples de mise en cache à l’aide du chargement réactive qui nous avons examiné dans le précédent travail deux didacticiels bien avec les données qui peuvent changer régulièrement et ne sera pas exorbitantly long à générer. Mais si les données mises en cache ne changent jamais, l’expiration utilisée par le chargement réactive est superflue. De même, si les mise en cache de données prennent un temps extrêmement long à générer, les utilisateurs dont les demandes de trouver le cache vide devra subir une longue attente alors que les données sous-jacentes sont récupérées. Envisagez la mise en cache des données statiques et données qui prend un temps exceptionnellement long à générer au démarrage de l’application.

Bien que les bases de données dynamiques de nombreux, valeurs changent souvent, la plupart ont également une grande quantité de données statiques. Par exemple, pratiquement tous les modèles de données ont une ou plusieurs colonnes qui contiennent une valeur particulière à partir d’un ensemble fixe de choix. Un `Patients` table de base de données peut contenir un `PrimaryLanguage` colonne, dont l’ensemble de valeurs peut être en anglais, espagnol, Français, russe, japonais et ainsi de suite. Souvent, ces types de colonnes sont implémentées à l’aide de *tables de recherche*. Au lieu d’enregistrer la chaîne anglais ou Français dans le `Patients` table, une deuxième table est créée avec, en général, deux colonnes : un identificateur unique et une description de chaîne - avec un enregistrement pour chaque valeur possible. Le `PrimaryLanguage` colonne dans la `Patients` table stocke l’identificateur unique correspondant dans la table de recherche. Dans la Figure 1, patient langue principale de John Doe s est anglais, tandis que Ed Johnson s est russe.


![La Table de langues est une Table de recherche utilisée par la Table Patients](caching-data-at-application-startup-vb/_static/image1.png)

**Figure 1**: le `Languages` Table est une Table de recherche utilisée par le `Patients` Table


L’interface utilisateur pour la modification ou la création d’un nouveau patient comprend une liste déroulante des langues autorisées remplie par les enregistrements dans la `Languages` table. Sans mise en cache, chaque fois que cette interface est visité le système doit interroger la `Languages` table. C’est inutile et inutile dans la mesure où les valeurs de table de recherche modifier très rarement, voire jamais.

Nous pouvons mettre en cache le `Languages` données en utilisant les mêmes techniques de chargement réactive examinés dans les didacticiels précédents. Réactive le chargement, cependant, utilise une expiration temporelle, ce qui n’est pas nécessaire pour les données de table de recherche statiques. Alors que la mise en cache à l’aide de chargement réactive serait mieux que tout aucune mise en cache, la meilleure approche serait de manière proactive charger les données de table de recherche dans le cache au démarrage de l’application.

Dans ce didacticiel, nous allons examiner comment les données du cache de la table de recherche et d’autres informations statiques.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Étape 2 : Examiner les différentes façons de mettre en Cache des données

Informations peuvent être mis en cache par programmation dans une application ASP.NET à l’aide de diverses approches. Nous ve déjà vu comment utiliser le cache de données dans les didacticiels précédents. Vous pouvez également les objets peuvent être par programmation mis en cache à l’aide de *membres statiques* ou *état de l’application*.

Lorsque vous travaillez avec une classe, généralement la classe doit tout d’abord être instanciée avant que ses membres sont accessibles. Par exemple, pour appeler une méthode à partir d’une des classes dans notre couche de logique métier, nous devons tout d’abord créer une instance de la classe :


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Avant de nous pouvons appeler *SomeMethod* ou travailler avec *SomeProperty*, nous devons tout d’abord créer une instance de la classe à l’aide de la `New` mot clé. *SomeMethod* et *SomeProperty* sont associés à une instance particulière. La durée de vie de ces membres est liée à la durée de vie de leur objet associé. *Les membres statiques*, quant à eux, sont variables, propriétés et méthodes qui sont partagées entre *tous les* instances de la classe et, par conséquent, ont une durée de vie tant que la classe. Les membres statiques sont signalées par le mot clé `Shared`.

En plus de membres statiques, données peuvent être mis en cache en utilisant l’état de l’application. Chaque application ASP.NET gère une collection nom/valeur que s partagées entre tous les utilisateurs et les pages de l’application. Cette collection est accessible à l’aide de la [ `HttpContext` classe](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` propriété](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)et utilisé à partir d’une classe code-behind de page s ASP.NET comme suit :


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

Le cache de données fournit une interface plus riche API pour la mise en cache des données, offrant des mécanismes pour basées sur les temps et les dépendances des expirations dans le, les priorités des éléments du cache et ainsi de suite. Avec des membres statiques et l’état de l’application, ces fonctionnalités doivent être ajoutées manuellement par le développeur de pages. Toutefois, lors de la mise en cache des données au démarrage de l’application pour la durée de vie de l’application, les avantages de cache s de données sont hypothétiques. Dans ce didacticiel, nous allons examiner le code qui utilise ces trois techniques de mise en cache des données statiques.

## <a name="step-3-caching-thesupplierstable-data"></a>Étape 3 : Mise en cache le`Suppliers`données de Table

Le tables de base de données nous ve implémentée pour la date de Northwind n’incluent pas des tables de recherche classiques. Les quatre tables de données implémenté dans notre DAL toutes les tables de modèle dont les valeurs ne sont pas statiques. Au lieu de passer le temps d’ajouter un nouveau DataTable à la couche DAL puis une nouvelle classe et méthodes pour la couche BLL, pour ce didacticiel permettre simplement de s prétendre que le `Suppliers` données de la table s sont statiques. Par conséquent, nous pouvons mettre en cache ces données au démarrage de l’application.

Pour commencer, créez une nouvelle classe nommée `StaticCache.cs` dans le `CL` dossier.


![Créer la classe StaticCache.vb dans le dossier CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figure 2**: créer le `StaticCache.vb` classe dans le `CL` dossier


Nous devons ajouter une méthode qui charge les données au démarrage dans le magasin de cache appropriée, ainsi que les méthodes qui retournent des données à partir de ce cache.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Le code ci-dessus utilise une variable membre statique, `suppliers`, pour stocker les résultats de la `SuppliersBLL` classe s `GetSuppliers()` (méthode), qui est appelée à partir de la `LoadStaticCache()` (méthode). Le `LoadStaticCache()` méthode est destinée à être appelée lors du démarrage de s d’application. Une fois ces données a été chargées au démarrage de l’application, n’importe quelle page qui doit fonctionner avec les données des fournisseurs peut appeler le `StaticCache` classe s `GetSuppliers()` (méthode). Par conséquent, l’appel à la base de données pour obtenir les fournisseurs se produit uniquement une seule fois, au démarrage de l’application.

Au lieu d’utiliser une variable membre statique en tant que le stockage de cache, nous aurions également pu utiliser état de l’application ou le cache de données. Le code suivant montre la classe remaniée pour utiliser l’état de l’application :


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

Dans `LoadStaticCache()`, les informations de fournisseur sont stockées dans la variable d’application *clé*. Il s retourné en type approprié (`Northwind.SuppliersDataTable`) à partir de `GetSuppliers()`. Bien que l’état de l’application sont accessibles dans les classes de code-behind des pages ASP.NET à l’aide de `Application("key")`, dans l’architecture que nous devons utiliser `HttpContext.Current.Application("key")` afin d’obtenir des cours `HttpContext`.

De même, le cache de données utilisable comme un magasin de cache, comme le montre le code suivant :


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Pour ajouter un élément au cache de données sans expiration temporelle, utilisez le `System.Web.Caching.Cache.NoAbsoluteExpiration` et `System.Web.Caching.Cache.NoSlidingExpiration` valeurs en tant que paramètres d’entrée. Cette surcharge particulière du cache de données s `Insert` méthode a été sélectionnée afin que nous pouvions spécifier la *priorité* de l’élément de cache. La priorité est utilisée pour déterminer les éléments à nettoyer à partir du cache lorsque la mémoire disponible est insuffisante. Ici, nous utilisons la priorité `NotRemovable`, ce qui garantit que cet élément de cache a gagné t être nettoyée.

> [!NOTE]
> Ce téléchargement didacticiel s implémente la `StaticCache` classe à l’aide de l’approche de variable membre statique. Le code pour les techniques de cache application état et les données est disponible dans les commentaires dans le fichier de classe.


## <a name="step-4-executing-code-at-application-startup"></a>Étape 4 : L’exécution de Code au démarrage de l’Application

Pour exécuter du code de démarrage d’une application web, nous devons créer un fichier spécial nommé `Global.asax`. Ce fichier peut contenir des gestionnaires d’événements pour l’application-, session-, et les événements au niveau de la demande et il est ici où nous pouvons ajouter le code qui sera exécuté chaque fois que l’application démarre.

Ajouter le `Global.asax` fichier à votre répertoire de racine de s d’application web en cliquant sur le nom de projet de site Web dans Visual Studio s l’Explorateur de solutions et en choisissant Ajouter un nouvel élément. À partir de la boîte de dialogue Ajouter un nouvel élément, sélectionnez le type d’élément de classe d’Application globale, puis sur le bouton Ajouter.

> [!NOTE]
> Si vous avez déjà un `Global.asax` fichier dans votre projet, la classe d’Application globale type d’élément ne sera pas répertorié dans la boîte de dialogue Ajouter un nouvel élément.


[![Ajoutez le fichier Global.asax à votre répertoire racine de Web Application s](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figure 3**: ajouter la `Global.asax` fichier à votre Application Web s répertoire racine ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-vb/_static/image5.png))


La valeur par défaut `Global.asax` modèle de fichier inclut cinq méthodes au sein d’une côté serveur `<script>` balise :

- **`Application_Start`** s’exécute lors du premier démarrage de l’application web
- **`Application_End`** s’exécute lorsque l’application s’arrête.
- **`Application_Error`** s’exécute chaque fois qu’une exception non gérée atteint l’application
- **`Session_Start`** s’exécute lorsqu’une nouvelle session est créée.
- **`Session_End`** s’exécute lorsqu’une session est arrivé à expiration ou abandonnée

Le `Application_Start` Gestionnaire d’événements est appelé une seule fois pendant un cycle de vie d’application s. Démarrage de l’application la première fois une ressource ASP.NET est demandée par l’application et continue à s’exécuter jusqu'à ce que l’application est redémarrée, ce qui peut se produire en modifiant le contenu de la `/Bin` dossier, modification `Global.asax`, modification le contenu dans le `App_Code` dossier, ou en modifiant le `Web.config` fichier, entre autres. Reportez-vous à [vue d’ensemble du Cycle de vie ASP.NET Application](https://msdn.microsoft.com/library/ms178473.aspx) pour une présentation plus détaillée sur le cycle de vie d’application.

Pour ces didacticiels, nous devons uniquement ajouter du code pour le `Application_Start` (méthode), c’est le cas hésitez pas à supprimer les autres. Dans `Application_Start`, appelez simplement la `StaticCache` classe s `LoadStaticCache()` (méthode), qui charge et mettre en cache les informations de fournisseur :


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

S résume-t-elle est ! Au démarrage de l’application, le `LoadStaticCache()` méthode saisir les informations de fournisseur à partir de la couche BLL et stockez-le dans une variable membre statique (ou le cache de stocker le code de fin à l’aide de la `StaticCache` classe). Pour vérifier ce comportement, définissez un point d’arrêt le `Application_Start` (méthode) et exécuter votre application. Notez que le point d’arrêt est atteint au démarrage de l’application. Les demandes suivantes, toutefois, n’entraînent pas la `Application_Start` méthode à exécuter.


[![Utiliser un point d’arrêt pour vérifier que le Gestionnaire d’événements Application_Start est en cours d’exécution](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figure 4**: utiliser un point d’arrêt pour vérifier que le `Application_Start` Gestionnaire d’événements est en cours d’exécution ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Si vous ne cliquez pas sur le `Application_Start` point d’arrêt lorsque vous commencez le débogage, il est, car votre application a déjà démarré. Forcer l’application à redémarrer en modifiant votre `Global.asax` ou `Web.config` des fichiers, puis réessayez. Vous pouvez simplement ajouter (ou supprimer) une ligne vide à la fin d’un de ces fichiers pour redémarrer rapidement l’application.


## <a name="step-5-displaying-the-cached-data"></a>Étape 5 : Afficher les données mises en cache

À ce stade le `StaticCache` classe a une version des données de fournisseur mis en cache au démarrage de l’application qui est accessible via son `GetSuppliers()` (méthode). Pour travailler avec ces données à partir de la couche de présentation, nous pouvons utiliser un ObjectDataSource ou appeler par programme la `StaticCache` classe s `GetSuppliers()` méthode à partir d’une classe code-behind de pages ASP.NET. Permettent d’examiner l’utilisation des contrôles ObjectDataSource et GridView pour afficher les informations de mise en cache de fournisseur s.

Commencez par ouvrir le `AtApplicationStartup.aspx` page dans le `Caching` dossier. Faites glisser un GridView à partir de la boîte à outils vers le concepteur, en définissant son `ID` propriété `Suppliers`. Ensuite, dans le contrôle GridView balise active s Choisissez Créer un nouveau ObjectDataSource nommé `SuppliersCachedDataSource`. Configurer l’ObjectDataSource à utiliser le `StaticCache` classe s `GetSuppliers()` (méthode).


[![Configurer pour utiliser la classe StaticCache ObjectDataSource](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figure 5**: configurer ObjectDataSource à utiliser le `StaticCache` classe ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-vb/_static/image11.png))


[![Utilisez la méthode GetSuppliers() pour récupérer les données des fournisseurs mis en cache](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figure 6**: utilisez le `GetSuppliers()` méthode pour récupérer les données mises en cache des fournisseurs ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-vb/_static/image14.png))


À l’issue de l’Assistant, Visual Studio ajoute automatiquement BoundFields pour chacun des champs de données `SuppliersDataTable`. Votre balisage déclaratif s GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Figure 7 illustre la page lorsqu’ils sont affichés via un navigateur. La sortie est la même, nous avions extrait des données à partir de la couche BLL s `SuppliersBLL` classe, mais en utilisant la `StaticCache` classe renvoie les données des fournisseurs comme mis en cache au démarrage de l’application. Vous pouvez définir des points d’arrêt dans le `StaticCache` classe s `GetSuppliers()` méthode pour vérifier ce comportement.


[![Les données des fournisseurs mis en cache s’affiche dans un GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figure 7**: la mise en cache les données des fournisseurs s’affiche dans un GridView ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Récapitulatif

La plupart des chaque modèle de données contient une grande quantité de données statiques, généralement implémentées sous la forme de tables de recherche. Dans la mesure où ces informations sont statiques, cet emplacement s aucune raison de ces informations doivent être affichée en permanence pour accéder à la base de données chaque heure. En outre, en raison de sa nature statique, lors de la mise en cache les données là s pas besoin d’une échéance. Dans ce didacticiel, nous avons vu comment prendre ces données et de mettre en cache dans le cache de données, état de l’application et via une variable membre statique. Ces informations sont mises en cache au démarrage de l’application et restent dans le cache tout au long de la durée de vie d’application s.

Dans ce didacticiel et les deux derniers, nous ve examiné la mise en cache des données pendant la durée de la durée de vie d’application s, ainsi que de l’utilisation temporelle des expirations dans le. Lors de la mise en cache de la base de données, cependant, une expiration temporelle est peut-être pas idéale. Au lieu de vider régulièrement le cache, il serait optimal pour supprimer uniquement l’élément mis en cache lors de la modification de la base de données sous-jacente. Cette valeur est possible grâce à l’utilisation des dépendances de cache SQL, nous allons examiner dans notre didacticiel suivant.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Teresa Murphy et Zack Jones. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](caching-data-in-the-architecture-vb.md)
> [Suivant](using-sql-cache-dependencies-vb.md)
