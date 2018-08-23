---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Stockage des informations utilisateur supplémentaires (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel nous répondrons à cette question par la création d’une application de livre d’or très rudimentaires. Ce faisant, nous examinerons les différentes options pour modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 76e6cd1ec290cf572023aef35e349b1146b2b432
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831763"
---
<a name="storing-additional-user-information-c"></a>Stocker les informations utilisateur supplémentaires (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> Dans ce didacticiel nous répondrons à cette question par la création d’une application de livre d’or très rudimentaires. Ce faisant, nous examiner les différentes options pour modéliser les informations utilisateur dans une base de données et puis regardez comment associer ces données avec les comptes d’utilisateurs créés par l’infrastructure d’appartenance.


## <a name="introduction"></a>Introduction

ASP. Infrastructure de l’appartenance du NET offre une interface flexible pour la gestion des utilisateurs. L’API d’appartenance inclut des méthodes pour valider les informations d’identification, récupérer des informations sur l’utilisateur actuellement connecté, création d’un nouveau compte d’utilisateur et supprimer un compte d’utilisateur, entre autres. Chaque compte d’utilisateur dans le cadre de l’appartenance contient uniquement les propriétés nécessaires pour valider les informations d’identification et l’exécution des tâches relatives aux comptes d’utilisateur essentiels. Cela est démontrée par les méthodes et propriétés de la [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), qui modélise un compte d’utilisateur dans le cadre de l’appartenance. Cette classe possède des propriétés comme [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), et [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), et les méthodes comme [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) et [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Souvent, les applications doivent stocker des informations utilisateur supplémentaires non incluses dans l’infrastructure de l’appartenance. Par exemple, un détaillant en ligne devrez permettre à chaque utilisateur de stocker ses adresses de livraison et de facturation, les informations de paiement, les préférences de livraison et numéro de téléphone du contact. En outre, chaque commande dans le système est associé à un compte d’utilisateur particulier.

Le `MembershipUser` classe n’inclut pas de propriétés telles que `PhoneNumber` ou `DeliveryPreferences` ou `PastOrders`. Par conséquent, comment nous suivre les informations utilisateur requises par l’application et l’avez intégrer à l’infrastructure de l’appartenance ? Dans ce didacticiel nous répondrons à cette question par la création d’une application de livre d’or très rudimentaires. Ce faisant, nous examiner les différentes options pour modéliser les informations utilisateur dans une base de données et puis regardez comment associer ces données avec les comptes d’utilisateurs créés par l’infrastructure d’appartenance. C’est parti !

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Étape 1 : Création de modèle de données de l’Application de livre d’or

Il existe une variété de techniques qui peuvent être utilisés pour capturer les informations utilisateur dans une base de données et l’associer avec les comptes d’utilisateurs créés par l’infrastructure de l’appartenance. Afin d’illustrer ces techniques, nous devrons permettent d’enrichir l’application web didacticiel afin qu’elles intègrent une sorte de données relatifs à l’utilisateur. (Actuellement, le modèle de données de l’application contient uniquement l’application services tables requises par le `SqlMembershipProvider`.)

Nous allons créer une application de livre d’or très simple où un utilisateur authentifié peut laisser un commentaire. Outre le stockage des commentaires de livre d’or, nous allons autoriser chaque utilisateur stocker sa ville natale, la page d’accueil et la signature. S’il est fourni, ville de domicile de l’utilisateur, page d’accueil et de signature seront affiche sur chaque message qu’il a laissé dans le livre d’or.

### <a name="adding-theguestbookcommentstable"></a>Ajout de la`GuestbookComments`Table

Pour capturer les commentaires du livre d’or, nous devons créer une table de base de données nommée `GuestbookComments` qui comporte des colonnes comme `CommentId`, `Subject`, `Body`, et `CommentDate`. Nous devons également disposer de chaque enregistrement le `GuestbookComments` référence de l’utilisateur ayant laissé le commentaire à la table.

Pour ajouter cette table à notre base de données, accédez à l’Explorateur de base de données dans Visual Studio et explorez le `SecurityTutorials` base de données. Avec le bouton droit sur le dossier Tables et choisissez Ajouter une nouvelle Table. Ceci fait apparaître une interface qui nous permet de définir les colonnes de la nouvelle table.


[![Ajouter une nouvelle Table à la base de données SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Figure 1**: ajouter une nouvelle Table à la `SecurityTutorials` base de données ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image3.png))


Ensuite, définissez le `GuestbookComments`de colonnes. Commencez par ajouter une colonne nommée `CommentId` de type `uniqueidentifier`. Cette colonne s’identifier de façon unique chaque commentaire dans le livre d’or, donc interdire `NULL` s et marquez-la en tant que clé primaire de la table. Au lieu de fournir une valeur pour le `CommentId` chaque champ `INSERT`, nous pouvons indiquer qu’un nouveau `uniqueidentifier` valeur doit être générée automatiquement pour ce champ sur `INSERT` en définissant la valeur par défaut de la colonne `NEWID()`. Après avoir ajouté ce premier champ, en marquant comme clé primaire, les paramètres de sa valeur par défaut, votre écran doit ressembler à l’écran présenté en Figure 2.


[![Ajouter une colonne principale nommée CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Figure 2**: ajouter une colonne nommée principal `CommentId` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image6.png))


Ensuite, ajoutez une colonne nommée `Subject` de type `nvarchar(50)` et une colonne nommée `Body` de type `nvarchar(MAX)`, interdire `NULL` s dans les deux colonnes. Ensuite, ajoutez une colonne nommée `CommentDate` de type `datetime`. Interdire `NULL` s et définissez le `CommentDate` valeur par défaut de la colonne à `getdate()`.

Ne reste qu’à ajouter une colonne qui associe un compte d’utilisateur à chaque commentaire de livre d’or. Une option consisterait à ajouter une colonne nommée `UserName` de type `nvarchar(256)`. Il s’agit d’un choix approprié lors de l’utilisation d’un fournisseur d’appartenances autre que le `SqlMembershipProvider`. Mais lorsque vous utilisez le `SqlMembershipProvider`, comme nous sommes dans cette série de didacticiels, le `UserName` colonne dans la `aspnet_Users` table n’est pas garantie pour être unique. Le `aspnet_Users` clé primaire de la table est `UserId` et est de type `uniqueidentifier`. Par conséquent, le `GuestbookComments` table doit être une colonne nommée `UserId` de type `uniqueidentifier` (interdiction `NULL` valeurs). Continuez et ajoutez cette colonne.

> [!NOTE]
> Comme expliqué dans la [ *création du schéma d’appartenance dans SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) didacticiel, l’infrastructure de l’appartenance est conçu pour permettre plusieurs applications web avec des comptes d’utilisateurs différents partagent le même magasin d’utilisateurs. Il effectue cela en partitionnant les comptes d’utilisateur dans différentes applications. Et tandis que chaque nom d’utilisateur est garanti être unique au sein d’une application, le même nom d’utilisateur peut être utilisé dans différentes applications à l’aide de la même banque utilisateur. Il existe un composite `UNIQUE` contrainte dans le `aspnet_Users` table sur le `UserName` et `ApplicationId` champs, mais pas sur uniquement le `UserName` champ. Par conséquent, il est possible pour le compte aspnet\_table aux utilisateurs d’avoir deux (ou plusieurs) des enregistrements avec le même `UserName` valeur. Cependant, il existe un `UNIQUE` contrainte sur la `aspnet_Users` la table `UserId` champ (dans la mesure où il s’agit de la clé primaire). Un `UNIQUE` contrainte est importante, car sans cela, nous ne pouvons pas établir une contrainte de clé étrangère entre la `GuestbookComments` et `aspnet_Users` tables.


Après avoir ajouté le `UserId` colonne, enregistrer la table en cliquant sur l’icône Enregistrer dans la barre d’outils. Nommez la nouvelle table `GuestbookComments`.

Nous avons un dernier problème assister à avec le `GuestbookComments` table : nous devons créer un [contrainte de clé étrangère](https://msdn.microsoft.com/library/ms175464.aspx) entre le `GuestbookComments.UserId` colonne et le `aspnet_Users.UserId` colonne. Pour ce faire, cliquez sur l’icône de relation dans la barre d’outils pour lancer la boîte de dialogue relations de clé étrangère. (Vous pouvez également lancer cette boîte de dialogue en sélectionnant le menu Concepteur de tables et en choisissant de relations.)

Cliquez sur le bouton Ajouter dans le coin inférieur gauche de la boîte de dialogue relations de clé étrangère. Cette opération ajoute une nouvelle contrainte de clé étrangère, bien que nous devons encore définir les tables impliquées dans la relation.


[![Utilisez la boîte de dialogue relations de clé étrangère pour gérer les contraintes de clé étrangère d’une Table](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Figure 3**: utilisez la boîte de dialogue relations de clé étrangère pour gérer les contraintes de clé étrangère d’une Table ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image9.png))


Ensuite, cliquez sur l’icône de points de suspension dans la ligne « Table et colonnes Specifications » sur la droite. Cette action lance la boîte de dialogue Tables et colonnes à partir de laquelle nous pouvons spécifier la table de clé primaire et de colonne et de la colonne de clé étrangère à partir de la `GuestbookComments` table. En particulier, sélectionnez `aspnet_Users` et `UserId` en tant que la table de clé primaire et la colonne, et `UserId` à partir de la `GuestbookComments` table en tant que la colonne clé étrangère (voir Figure 4). Après avoir défini les tables de clés primaires et étrangères et les colonnes, cliquez sur OK pour revenir à la boîte de dialogue relations de clé étrangère.


[![Établir un étrangère clé contrainte entre l’aspnet_Users et GuesbookComments Tables](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Figure 4**: établir une Foreign Key contrainte entre le `aspnet_Users` et `GuesbookComments` Tables ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image12.png))


À ce stade, la contrainte de clé étrangère a été établie. Garantit la présence de cette contrainte [l’intégrité relationnelle](http://en.wikipedia.org/wiki/Referential_integrity) entre les deux tables en garantissant que ne peut pas contenir une entrée de livre d’or faisant référence à un compte d’utilisateur inexistante. Par défaut, une contrainte de clé étrangère empêche un enregistrement parent est supprimé s’il existe correspondants des enregistrements enfants. Autrement dit, si un utilisateur effectue une ou plusieurs remarques de livre d’or, et puis nous tentons de supprimer ce compte d’utilisateur, la suppression échoue, sauf si ses commentaires du livre d’or sont supprimés en premier.

Contraintes de clé étrangère peuvent être configurées pour supprimer automatiquement les enregistrements enfants associées lorsqu’un enregistrement parent est supprimé. En d’autres termes, nous pouvons configurer cette contrainte de clé étrangère afin que les entrées du livre d’or d’un utilisateur sont automatiquement supprimées lorsque son compte d’utilisateur est supprimé. Pour ce faire, développez la section « Spécification d’insérer et de mise à jour » et la valeur de la propriété « Supprimer la règle » Cascade.


[![Configurer la contrainte de clé étrangère à des suppressions en Cascade](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Figure 5**: configurer la contrainte de clé étrangère pour la suppression en Cascade ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image15.png))


Pour enregistrer la contrainte de clé étrangère, cliquez sur le bouton Fermer pour quitter les relations de clé étrangère. Puis cliquez sur l’icône Enregistrer dans la barre d’outils pour enregistrer la table et cette relation.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Stockage Home Town, page d’accueil et Signature de l’utilisateur

Le `GuestbookComments` tableau illustre comment stocker des informations qui partage une relation un-à-plusieurs comptes d’utilisateur. Étant donné que chaque compte d’utilisateur peut avoir un nombre arbitraire de commentaires associés, cette relation est modélisée en créant une table pour stocker l’ensemble de commentaires qui inclut une colonne que revient chaque commentaire à un utilisateur particulier. Lorsque vous utilisez le `SqlMembershipProvider`, ce lien est établi au mieux en créant une colonne nommée `UserId` de type `uniqueidentifier` et une contrainte de clé étrangère entre cette colonne et `aspnet_Users.UserId`.

Nous devons maintenant associer trois colonnes de chaque compte d’utilisateur pour stocker l’utilisateur ville natale, page d’accueil et la signature, qui apparaîtra dans ses commentaires du livre d’or. Il existe plusieurs moyens différents pour effectuer cette opération :

- <strong>Ajouter de nouvelles colonnes à la</strong><strong>`aspnet_Users`</strong><strong>ou</strong><strong>`aspnet_Membership`</strong><strong>tables.</strong> Je ne recommande pas cette approche, car il modifie le schéma utilisé par le `SqlMembershipProvider`. Cette décision peut se retourner contre vous deVisual. Par exemple, que se passe-t-il si une future version de ASP.NET utilise un autre `SqlMembershipProvider` schéma. Microsoft peut inclure un outil pour migrer d’ASP.NET 2.0 `SqlMembershipProvider` données vers le nouveau schéma, mais si vous avez modifié l’ASP.NET 2.0 `SqlMembershipProvider` schéma, une telle conversion ne peut pas être possible.

- **Utilisez ASP. Framework de profil du .NET, définition d’une propriété de profil pour la ville natale, la page d’accueil et la signature.** ASP.NET propose une infrastructure de profil qui est conçue pour stocker des données supplémentaires spécifiques à l’utilisateur. Comme l’infrastructure de l’appartenance, l’infrastructure de profil est construit sur le modèle de fournisseur. Le .NET Framework est livré avec un `SqlProfileProvider` sthat stocke les données de profil dans une base de données SQL Server. En fait, notre base de données a déjà la table utilisée par le `SqlProfileProvider` (`aspnet_Profile`), comme il a été ajouté quand nous avons ajouté les services d’application de nouveau la <a id="_msoanchor_2"> </a> [ *création du schéma d’appartenance dans SQL Serveur* ](creating-the-membership-schema-in-sql-server-cs.md) didacticiel.   
  Le principal avantage de l’infrastructure de profil est de permettre aux développeurs de définir les propriétés de profil dans `Web.config` – aucun code ne doit être écrit pour sérialiser les données de profil vers et depuis le magasin de données sous-jacent. En bref, il est incroyablement facile pour définir un ensemble de propriétés de profil et à les utiliser dans le code. Toutefois, le système de profil laisse beaucoup à désirer lorsqu’il s’agit du contrôle de version, si vous avez une application où vous attendez des nouvelles propriétés spécifiques à l’utilisateur à ajouter à un moment ultérieur, existants ou nouveaux à être supprimées ou modifiées, puis l’infrastructure de profil ne peut pas être le  meilleure option. En outre, le `SqlProfileProvider` stocke les propriétés de profil de manière extrêmement dénormalisée, rendant pratiquement à exécuter des requêtes directement sur les données de profil (par exemple, combien d’utilisateurs ont un accueil ville de New York).   
  Pour plus d’informations sur l’infrastructure de profil, consultez la section « Autres lectures » à la fin de ce didacticiel.

- <strong>Ajoutez ces trois colonnes dans une nouvelle table dans la base de données et établir une relation un à un entre cette table et</strong><strong>`aspnet_Users`</strong><strong>.</strong> Cette approche implique un peu plus de travail qu’avec l’infrastructure de profil, mais offre une flexibilité maximale dans la façon dont les propriétés de l’utilisateur supplémentaires sont modélisées dans la base de données. Il s’agit de l’option que nous allons utiliser dans ce didacticiel.

Nous allons créer une nouvelle table appelée `UserProfiles` pour enregistrer la ville natale, la page d’accueil et la signature pour chaque utilisateur. Avec le bouton droit sur le dossier Tables dans la fenêtre Explorateur de base de données et choisir de créer une nouvelle table. Nom de la première colonne `UserId` et définissez son type sur `uniqueidentifier`. Interdire `NULL` valeurs et marquer la colonne comme clé primaire. Ensuite, ajoutez les colonnes nommées : `HomeTown` de type `nvarchar(50)`; `HomepageUrl` de type `nvarchar(100)`; et la Signature de type `nvarchar(500)`. Chacune de ces trois colonnes peut accepter un `NULL` valeur.


[![Créer la Table UserProfiles](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Figure 6**: créer le `UserProfiles` Table ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image18.png))


Enregistrez la table et nommez-le `UserProfiles`. Enfin, établir une contrainte de clé étrangère entre la `UserProfiles` la table `UserId` champ et le `aspnet_Users.UserId` champ. Comme nous l’avons fait avec la contrainte de clé étrangère entre la `GuestbookComments` et `aspnet_Users` tables, avoir cette contrainte suppressions en cascade. Dans la mesure où le `UserId` champ `UserProfiles` est le principal clé, cela permet de s’assurer qu’il y aura pas plus de plusieurs enregistrements dans la `UserProfiles` table pour chaque compte d’utilisateur. Ce type de relation est appelé par un à un.

Maintenant que nous avons créé le modèle de données, nous sommes prêts à l’utiliser. Dans les étapes 2 et 3, nous allons examiner la façon dont l’utilisateur actuellement connecté peut afficher et modifier leurs informations town, page d’accueil et la signature. À l’étape 4, nous créerons l’interface pour les utilisateurs authentifiés à soumettre les nouveaux commentaires sur le livre d’or et afficher les serveurs existants.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Étape 2 : Affichage de l’utilisateur Home Town, page d’accueil et Signature

Il existe de différentes façons pour autoriser l’utilisateur actuellement connecté afficher et modifier ses informations town, page d’accueil et la signature. Nous pourrions créer manuellement l’interface utilisateur avec la zone de texte et les contrôles Label ou nous pourrions utiliser une des données des contrôles Web, tels que le contrôle DetailsView. Pour effectuer la base de données `SELECT` et `UPDATE` instructions nous pourrions écrire ADO.NET dans la classe de code-behind de notre page de code ou, vous pouvez également employer une approche déclarative avec SqlDataSource. Dans l’idéal, notre application contiendrait une architecture à plusieurs niveaux, nous pouvons appeler par programmation à partir de la classe code-behind de la page ou de façon déclarative via le contrôle ObjectDataSource.

Étant donné que cette série de didacticiels se concentre sur l’authentification par formulaire, l’autorisation, comptes d’utilisateurs et rôles, il ne sera pas une discussion détaillée de ces options d’accès des données différentes ou pourquoi une architecture à plusieurs niveaux est préférable à l’exécution des instructions SQL directement à partir de la page ASP.NET. Je vais vous guider en utilisant un contrôle DetailsView et le SqlDataSource-option plus rapide et plus simple- mais les concepts étudiés peuvent certainement être appliqués à autre logique d’accès aux données et les contrôles Web. Pour plus d’informations sur l’utilisation des données dans ASP.NET, reportez-vous à mon *[utilisation des données dans ASP.NET 2.0](../../data-access/index.md)* série de didacticiels.

Ouvrir le `AdditionalUserInfo.aspx` page dans le `Membership` dossier et ajouter un contrôle DetailsView à la page, en définissant son `ID` propriété `UserProfile` et éliminant son `Width` et `Height` propriétés. Développez la balise active de DetailsView et choisissez de le lier à un nouveau contrôle de source de données. Cette action lance l’Assistant Configuration de source de données (voir la Figure 7). La première étape vous invite à spécifier le type de source de données. Étant donné que nous allons connecter directement à la `SecurityTutorials` de base de données, cliquez sur l’icône de la base de données, en spécifiant le `ID` comme `UserProfileDataSource`.


[![Ajouter un nouveau contrôle SqlDataSource nommé UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Figure 7**: ajouter un nouveau contrôle SqlDataSource nommé `UserProfileDataSource` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image21.png))


L’écran suivant vous invite à entrer pour la base de données à utiliser. Nous avons déjà défini une chaîne de connexion dans `Web.config` pour le `SecurityTutorials` base de données. Ce nom de chaîne de connexion – `SecurityTutorialsConnectionString` – doit se trouver dans la liste déroulante. Sélectionnez cette option et cliquez sur Suivant.


[![Choisissez SecurityTutorialsConnectionString dans la liste déroulante](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Figure 8**: choisissez `SecurityTutorialsConnectionString` dans la liste déroulante ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image24.png))


L’écran suivant nous demande de spécifier la table et les colonnes à la requête. Choisissez le `UserProfiles` de table dans la liste déroulante et vérifiez toutes les colonnes.


[![Afficher toutes les colonnes automatiquement à partir de la Table UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Figure 9**: mettre revenir toutes les colonnes de la `UserProfiles` Table ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image27.png))


La requête actuelle dans la Figure 9 retourne *tous les* des enregistrements de `UserProfiles`, mais nous nous intéressons uniquement dans l’enregistrement de l’utilisateur actuellement connecté. Pour ajouter un `WHERE` clause, cliquez sur le `WHERE` bouton pour afficher l’ajouter `WHERE` boîte de dialogue de la Clause (voir Figure 10). Ici, vous pouvez sélectionner la colonne à filtrer, l’opérateur et la source du paramètre de filtre. Sélectionnez `UserId` en tant que la colonne et l’opérateur « = ».

Malheureusement, il n’a aucune source de paramètres intégré pour renvoyer l’utilisateur actuellement connecté `UserId` valeur. Nous devons récupérer cette valeur par programmation. Par conséquent, définissez la liste déroulante Source « None, « cliquez sur Ajouter bouton pour ajouter le paramètre, puis cliquez sur OK.


[![Ajouter un paramètre de filtre sur la colonne UserId](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Figure 10**: ajouter un paramètre de filtre sur le `UserId` colonne ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image30.png))


Après avoir cliqué sur OK, vous serez redirigé vers l’écran illustré à la Figure 9. Cette fois, cependant, la requête SQL en bas de l’écran doit inclure un `WHERE` clause. Cliquez sur Suivant pour passer à l’écran « Requête de Test ». Ici, vous pouvez exécuter la requête et afficher les résultats. Cliquez sur Terminer pour terminer l’Assistant.

À la fin de l’Assistant Configuration de source de données, Visual Studio crée le contrôle SqlDataSource selon les paramètres spécifiés dans l’Assistant. En outre, il ajoute manuellement BoundFields pour le contrôle DetailsView pour chaque colonne renvoyée par le SqlDataSource `SelectCommand`. Il n’est pas nécessaire pour afficher le `UserId` champ dans le contrôle DetailsView, étant donné que l’utilisateur n’a pas besoin de connaître cette valeur. Vous pouvez supprimer ce champ directement à partir de balisage déclaratif du contrôle DetailsView ou en cliquant sur le « modifier les champs » lien à partir de sa balise active.

À ce stade balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Nous devons définir par programmation le contrôle SqlDataSource `UserId` paramètre à l’utilisateur actuellement connecté `UserId` avant que les données sont sélectionnées. Cela est possible en créant un gestionnaire d’événements pour le SqlDataSource `Selecting` code il des événements et ajoutant le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Le code ci-dessus démarre en obtenant une référence à l’utilisateur actuellement connecté en appelant le `Membership` la classe `GetUser` (méthode). Cette commande renvoie un `MembershipUser` de l’objet, dont la propriété `ProviderUserKey` propriété contient le `UserId`. Le `UserId` valeur est ensuite assignée à la SqlDataSource `@UserId` paramètre.

> [!NOTE]
> Le `Membership.GetUser()` méthode retourne des informations sur l’utilisateur actuellement connecté. Si un utilisateur anonyme se trouve dans la page, elle retournera la valeur `null`. Dans ce cas, cela entraîne un `NullReferenceException` sur la ligne suivante de code lorsque vous tentez de lire le `ProviderUserKey` propriété. Bien sûr, nous n’avons pas à vous soucier `Membership.GetUser()` retournant un `null` valeur dans le `AdditionalUserInfo.aspx` page car nous avons configuré une autorisation d’URL dans le didacticiel précédent, afin que seuls les utilisateurs authentifiés peuvent accéder aux ressources ASP.NET dans ce dossier. Si vous avez besoin d’accéder aux informations relatives à l’utilisateur actuellement connecté dans une page où l’accès anonyme est autorisé, veillez à vérifier que non -`null MembershipUser` est retourné à partir de la `GetUser()` méthode avant de le référencer ses propriétés.


Si vous visitez le `AdditionalUserInfo.aspx` page via un navigateur s’affiche une page vierge parce que nous devons encore ajouter des lignes à la `UserProfiles` table. À l’étape 6, nous allons examiner comment personnaliser le contrôle CreateUserWizard pour ajouter automatiquement une nouvelle ligne à la `UserProfiles` table lorsqu’un nouveau compte d’utilisateur est créé. Pour l’instant, toutefois, nous devrons créer manuellement un enregistrement dans la table.

Accédez à l’Explorateur de base de données dans Visual Studio et développez le dossier Tables. Avec le bouton droit sur le `aspnet_Users` table et choisissez « Afficher les données de Table » pour afficher les enregistrements dans la table ; faire la même chose le `UserProfiles` table. Figure 11 illustre ces résultats lorsque disposées en mosaïque verticalement. Dans ma base de données est actuellement `aspnet_Users` enregistrements Bruce Fred et Tito, mais aucun enregistrement dans le `UserProfiles` table.


[![Le contenu de l’aspnet_Users et UserProfiles Tables sont affichées.](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Figure 11**: le contenu de la `aspnet_Users` et `UserProfiles` Tables sont affichées ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image33.png))


Ajouter un nouvel enregistrement à la `UserProfiles` table en tapant manuellement des valeurs pour le `HomeTown`, `HomepageUrl`, et `Signature` champs. Le moyen le plus simple pour obtenir une liste valide `UserId` valeur dans le nouveau `UserProfiles` enregistrement consiste à sélectionner le `UserId` champ à partir d’un compte d’utilisateur particulier dans le `aspnet_Users` table et copier et coller dans le `UserId` champ `UserProfiles`. La figure 12 illustre le `UserProfiles` après l’ajout d’un nouvel enregistrement de Bruce de table.


[![Un enregistrement a été ajouté à UserProfiles pour Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Figure 12**: un enregistrement a été ajouté à `UserProfiles` de Bruce ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image36.png))


Retour à la `AdditionalUserInfo.aspx` page, connecté en tant que Bruce. Comme le montre la Figure 13, les paramètres de Bruce sont affichés.


[![L’utilisateur visite actuellement est indiqué les paramètres de His](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Figure 13**: l’utilisateur actuellement visitant est indiqué les paramètres de His ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> Accédez à l’avance et manuellement ajouter des enregistrements dans la `UserProfiles` table pour chaque utilisateur d’appartenance. À l’étape 6, nous allons examiner comment personnaliser le contrôle CreateUserWizard pour ajouter automatiquement une nouvelle ligne à la `UserProfiles` table lorsqu’un nouveau compte d’utilisateur est créé.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Étape 3 : Autoriser l’utilisateur à modifier son Home Town, la page d’accueil et la Signature

À ce stade l’utilisateur actuellement connecté peut afficher leur ville natale, la page d’accueil et le paramètre de la signature, mais ils ne peuvent pas encore les modifier. Nous allons mettre à jour le contrôle DetailsView afin que les données peuvent être modifiées.

La première chose que nous devons faire est ajouter un `UpdateCommand` pour SqlDataSource, en spécifiant la `UPDATE` instruction à exécuter et ses paramètres correspondants. Sélectionnez SqlDataSource et, dans la fenêtre Propriétés, cliquez sur les points de suspension en regard de la propriété UpdateQuery pour afficher la boîte de dialogue Éditeur de commandes et paramètres. Entrez les informations suivantes `UPDATE` instruction dans la zone de texte :

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Ensuite, cliquez sur le bouton « Actualiser les paramètres », qui crée un paramètre dans le contrôle SqlDataSource `UpdateParameters` collection pour chacun des paramètres dans le `UPDATE` instruction. Conserver la source pour l’ensemble de l’ensemble de paramètres none et cliquez sur le bouton OK pour fermer la boîte de dialogue.


[![Spécifiez le SqlDataSource UpdateCommand et UpdateParameters](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Figure 14**: spécifiez le SqlDataSource `UpdateCommand` et `UpdateParameters` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image42.png))


En raison des ajouts, nous avons apporté au contrôle SqlDataSource, le contrôle DetailsView contrôle pouvez prennent désormais en charge la modification. À partir de la balise active de DetailsView, cochez la case « Activer la modification ». Cette opération ajoute une CommandField du contrôle `Fields` collection avec son `ShowEditButton` propriété définie sur True. Cela restitue un bouton modifier lorsque le contrôle DetailsView s’affiche dans le mode lecture seule et de mise à jour et les boutons Annuler lorsque affichés dans le mode édition. Au lieu d’obliger l’utilisateur à cliquer sur Modifier, cependant, nous pouvons avoir le rendu de DetailsView dans un état « toujours modifiable » en définissant le contrôle DetailsView [ `DefaultMode` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) à `Edit`.

Avec ces modifications, balisage déclaratif de votre contrôle DetailsView doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Notez l’ajout de la CommandField et `DefaultMode` propriété.

Continuons et tester cette page via un navigateur. Lors de la visite avec un utilisateur qui dispose d’un enregistrement correspondant dans `UserProfiles`, les paramètres de l’utilisateur sont affichés dans une interface modifiable.


[![Le contrôle DetailsView restitue une Interface modifiable](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Figure 15**: le contrôle DetailsView restitue une Interface modifiable ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image45.png))


Essayez de modifier les valeurs et en cliquant sur le bouton de mise à jour. Il s’affiche comme si rien ne se produit. Il existe une publication (postback) et les valeurs sont enregistrées dans la base de données, mais il n’existe aucun retour visuel qui s’est produite lors de l’enregistrement.

Pour résoudre ce problème, revenez à Visual Studio et ajouter un contrôle étiquette ci-dessus le contrôle DetailsView. Définir son `ID` à `SettingsUpdatedMessage`, ses `Text` propriété « vos paramètres ont été mis à jour, » et ses `Visible` et `EnableViewState` propriétés à `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Nous avons besoin afficher le `SettingsUpdatedMessage` étiqueter chaque fois que le contrôle DetailsView est mis à jour. Pour ce faire, créez un gestionnaire d’événements pour le DetailsView `ItemUpdated` événement et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Retour à la `AdditionalUserInfo.aspx` page via un navigateur et de mettre à jour les données. Cette fois, un message d’état utiles s’affiche.


[![Un bref Message est affiché lorsque les paramètres sont mis à jour](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Figure 16**: un bref Message s’affiche lorsque les paramètres sont mis à jour ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> Le contrôle DetailsView d’édition du interface laisse beaucoup à désirer. Il utilise le format standard les zones de texte, mais le champ de Signature doit être probablement d’une zone de texte multiligne. RegularExpressionValidator doit être utilisé pour vous assurer que l’URL de la page d’accueil, si vous avez entré, commence par « http:// » ou « https:// ». En outre, depuis le contrôle DetailsView a contrôle ses `DefaultMode` propriété définie sur `Edit`, le bouton Annuler ne fait rien. Il doit soit être supprimé ou, lorsque vous cliquez dessus, rediriger l’utilisateur vers une autre page (tel que `~/Default.aspx`). Je laisse ces améliorations en guise d’exercice pour le lecteur.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Ajout d’un lien vers le`AdditionalUserInfo.aspx`Page dans la Page maître

Actuellement, le site Web ne fournit pas tous les liens vers les `AdditionalUserInfo.aspx` page. La seule façon d’y accéder consiste à entrer les URL de la page directement dans la barre d’adresses du navigateur. Nous allons ajouter un lien vers cette page dans le `Site.master` page maître.

Rappelez-vous que la page maître contienne un contrôle LoginView Web dans son `LoginContent` ContentPlaceHolder qui affiche les différents balisages pour les visiteurs authentifiées et anonymes. Mettre à jour le contrôle LoginView `LoggedInTemplate` pour inclure un lien vers le `AdditionalUserInfo.aspx` page. Après avoir apporté ces modifications le LoginView balisage déclaratif du contrôle doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Notez l’ajout de la `lnkUpdateSettings` contrôle de lien hypertexte pour le `LoggedInTemplate`. Avec ce lien en place, les utilisateurs authentifiés peuvent accéder rapidement à la page pour afficher et modifier leurs paramètres town, page d’accueil et signature domestiques.

## <a name="step-4-adding-new-guestbook-comments"></a>Étape 4 : Ajout de nouveaux commentaires du livre d’or

Le `Guestbook.aspx` page est où les utilisateurs authentifiés peuvent consulter le livre d’or et laisser un commentaire. Commençons par la création de l’interface pour ajouter de nouveaux commentaires du livre d’or.

Ouvrez le `Guestbook.aspx` page dans Visual Studio et construire une interface utilisateur composée de deux contrôles de zone de texte, un pour l’objet du nouveau commentaire et un pour son corps. Définir le premier contrôle de zone de texte `ID` propriété `Subject` et son `Columns` propriété à 40 ; la valeur du second défini `ID` à `Body`, ses `TextMode` à `MultiLine`et son `Width` et `Rows` propriétés de « 95 % » et 8, respectivement. Pour exécuter l’interface utilisateur, ajoutez un contrôle bouton Web nommé `PostCommentButton` et définissez son `Text` propriété à « Post votre commentaire ».

Dans la mesure où chaque commentaire de livre d’or nécessite un objet et le corps, ajoutez un contrôle RequiredFieldValidator pour chacune des zones de texte. Définir le `ValidationGroup` propriété de ces contrôles pour « EnterComment » ; de même, définissez le `PostCommentButton` du contrôle `ValidationGroup` propriété à « EnterComment ». Pour plus d’informations sur ASP. Contrôles de validation de NET, consultez [Validation de formulaire dans ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [dissection les contrôles de Validation dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)et le [didacticiel de contrôles de Validation serveur](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) sur [W3Schools](http://www.w3schools.com/).

Après la création de l’interface utilisateur de balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Avec l’interface utilisateur complète, la tâche suivante consiste à insérer un nouvel enregistrement dans le `GuestbookComments` table lorsque le `PostCommentButton` est cliqué. Cela peut être accompli de plusieurs façons : nous pouvons écrire du code ADO.NET dans le bouton `Click` Gestionnaire d’événements ; nous peut ajouter un contrôle SqlDataSource à la page, configurez sa `InsertCommand`, puis appelez ses `Insert` méthode à partir de la `Click` événement Gestionnaire ; ou nous pouvons créer un niveau intermédiaire qui a été chargé pour l’insertion de nouveaux commentaires du livre d’or et appeler cette fonctionnalité à partir du `Click` Gestionnaire d’événements. Étant donné que nous avons vu à l’aide d’un SqlDataSource à l’étape 3, utilisons ici le code ADO.NET.

> [!NOTE]
> Les classes ADO.NET permet d’accéder par programmation aux données à partir d’une base de données Microsoft SQL Server se trouvent dans le `System.Data.SqlClient` espace de noms. Vous devrez peut-être importer cet espace de noms de classe de code-behind de votre page (par exemple, `using System.Data.SqlClient;`).


Créer un gestionnaire d’événements pour le `PostCommentButton`de `Click` événement et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

Le `Click` Gestionnaire d’événements démarre en vérifiant que les données fournies par l’utilisateur soient valides. Si elle n’est pas le cas, le Gestionnaire d’événements s’arrête avant d’insérer un enregistrement. En supposant que les données fournies sont valides, l’utilisateur actuellement connecté `UserId` valeur est récupérée et stockée dans le `currentUserId` variable locale. Cette valeur est nécessaire, car nous devons fournir un `UserId` valeur lors de l’insertion d’un enregistrement dans `GuestbookComments`.

Ensuite, la chaîne de connexion pour le `SecurityTutorials` base de données est récupérée à partir de `Web.config` et le `INSERT` instruction SQL est spécifiée. Un `SqlConnection` objet est ensuite créé et ouvert. Ensuite, un `SqlCommand` objet est construit et les valeurs pour les paramètres utilisés dans le `INSERT` requête sont affectés. La `INSERT` instruction est exécutée et la connexion fermée. À la fin du Gestionnaire d’événements, le `Subject` et `Body` zones de texte `Text` propriétés sont effacées afin que les valeurs de l’utilisateur ne sont pas persistants sur la publication (postback).

Continuons et tester cette page dans un navigateur. Étant donné que cette page est dans le `Membership` dossier n’est pas accessible pour les visiteurs anonymes. Par conséquent, vous devez tout d’abord ouvrir une session (si vous n’avez pas déjà le cas). Entrez une valeur dans le `Subject` et `Body` zones de texte et cliquez sur le `PostCommentButton` bouton. Cela entraîne un nouvel enregistrement à ajouter à `GuestbookComments`. Lors de la publication, l’objet et le corps que vous avez fourni sont réinitialisés à partir de zones de texte.

Après avoir cliqué sur le `PostCommentButton` bouton il n’est pas de signal visuel qui le commentaire a été ajouté pour le livre d’or. Nous devons encore mettre à jour de cette page pour afficher les commentaires de livre d’or existants, que nous allons effectuer à l’étape 5. Une fois que nous cela, le commentaire ajouté juste apparaîtra dans la liste des commentaires, la fourniture de commentaires visuels adéquates. Pour l’instant, vérifiez que votre commentaire de livre d’or a été enregistré en examinant le contenu de la `GuestbookComments` table.

Figure 17 illustre le contenu de la `GuestbookComments` après deux commentaires ont été laissés de table.


[![Vous pouvez voir les commentaires du livre d’or dans la Table GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Figure 17**: vous pouvez voir les commentaires du livre d’or dans le `GuestbookComments` Table ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> Si un utilisateur tente d’insérer un commentaire de livre d’or contient potentiellement dangereuse balisage – tels que HTML, ASP.NET lève une `HttpRequestValidationException`. Pour en savoir plus sur cette exception, pourquoi l’exception est levée, et comment permettre aux utilisateurs d’envoyer des valeurs potentiellement dangereuses, consultez le [livre blanc sur la Validation de demande](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Étape 5 : Répertorier les commentaires du livre d’or existants

En plus de laisser des commentaires, un utilisateur visitant le `Guestbook.aspx` page doit également être en mesure d’afficher les commentaires existants du livre d’or. Pour ce faire, ajoutez un contrôle ListView nommé `CommentList` vers le bas de la page.

> [!NOTE]
> Le contrôle ListView est une nouveauté pour ASP.NET version 3.5. Il est conçu pour afficher une liste d’éléments dans une mise en page très personnalisable et flexible, mais offrent intégrés modification, insertion, suppression, la pagination et tri des fonctionnalités telles que le contrôle GridView. Si vous utilisez ASP.NET 2.0, vous devez utiliser le contrôle DataList ou Repeater à la place. Pour plus d’informations sur l’utilisation de la ListView, consultez [Scott Guthrie](https://weblogs.asp.net/scottgu/)d’entrée de blog, [le contrôle asp : ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)et mon article, [affichage des données avec le contrôle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Ouvrez la balise active de la ListView et, dans la liste déroulante Choisir la Source de données, lier le contrôle à une source de données. Comme nous l’avons vu à l’étape 2, cette action lance l’Assistant de Configuration de Source de données. Sélectionnez l’icône de la base de données, le nom résultant SqlDataSource `CommentsDataSource`, puis cliquez sur OK. Ensuite, sélectionnez le `SecurityTutorialsConnectionString` connexion chaîne obtenue à partir de la liste déroulante et cliquez sur Suivant.

À ce stade à l’étape 2 nous avons spécifié les données à la requête par prélèvement le `UserProfiles` de table dans la liste déroulante et en sélectionnant les colonnes à retourner (voir la Figure 9). Cette fois, cependant, nous souhaitons créer une instruction SQL qui extrait précédent non seulement les enregistrements à partir de `GuestbookComments`, mais également de commentateur ville natale, page d’accueil, la signature et nom d’utilisateur. Par conséquent, sélectionnez la case d’option « Spécifier une instruction SQL personnalisée ou une procédure stockée » et cliquez sur Suivant.

Cela fera apparaître l’écran « Définir des instructions ou procédures stockées ». Cliquez sur le bouton Générateur de requêtes pour créer graphiquement la requête. Le Générateur de requêtes commence par celle-ci vous invite à spécifier les tables de que nous souhaitons interroger à partir. Sélectionnez le `GuestbookComments`, `UserProfiles`, et `aspnet_Users` tables et cliquez sur OK. Cette opération ajoute les trois tables à l’aire de conception. Dans la mesure où il existe des contraintes de clé étrangère entre la `GuestbookComments`, `UserProfiles`, et `aspnet_Users` des tables, le Générateur de requêtes automatiquement `JOIN` s ces tables.

Tout ce qui reste consiste à spécifier les colonnes à retourner. À partir de la `GuestbookComments` select de la table le `Subject`, `Body`, et `CommentDate` colonnes ; retour le `HomeTown`, `HomepageUrl`, et `Signature` colonnes à partir de la `UserProfiles` table ; et retourner `UserName` à partir de `aspnet_Users`. En outre, ajoutez «`ORDER BY CommentDate DESC`» à la fin de la `SELECT` interroger afin que les articles les plus récents sont retournées en premier. Une fois ces sélections effectuées, votre interface de générateur de requêtes doit ressembler à la capture d’écran de la Figure 18.


[![La requête construits joint le GuestbookComments, UserProfiles et aspnet_Users Tables](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Figure 18**: la requête construit `JOIN` s le `GuestbookComments`, `UserProfiles`, et `aspnet_Users` Tables ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image54.png))


Cliquez sur OK pour fermer la fenêtre du Générateur de requêtes et de revenir à l’écran « Définir des instructions ou procédures stockées ». Cliquez sur Suivant pour passer à l’écran « Requête de Test », où vous pouvez consulter les résultats de requête en cliquant sur le bouton Tester la requête. Lorsque vous êtes prêt, cliquez sur Terminer pour terminer l’Assistant Configurer la Source de données.

Lorsque nous avons terminé l’Assistant Configurer la Source de données à le du étape 2, le contrôle DetailsView associé `Fields` regroupement a été mis à jour pour inclure un BoundField pour chaque colonne renvoyée par le `SelectCommand`. Toutefois, le ListView, demeure inchangé. Nous devons encore définir sa disposition. Mise en page de la ListView peut être construite manuellement à partir de son balisage déclaratif ou de l’option « Configurer ListView » dans sa balise active. J’ai généralement préférez définissant le balisage à la main, mais utiliser toute autre méthode qui est le plus naturel.

J’ai fini par à l’aide de ce qui suit `LayoutTemplate`, `ItemTemplate`, et `ItemSeparatorTemplate` pour mon contrôle ListView :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

Le `LayoutTemplate` définit le balisage émis par le contrôle, tandis que le `ItemTemplate` restitue chaque élément retourné par SqlDataSource. Le `ItemTemplate`de balisage qui en résulte est placé dans le `LayoutTemplate`de `itemPlaceholder` contrôle. Outre le `itemPlaceholder`, le `LayoutTemplate` inclut un contrôle DataPager, ce qui limite le ListView à l’affichage des commentaires de livre d’or que 10 par page (il s’agit de la valeur par défaut) et affiche une interface de pagination.

Mon `ItemTemplate` affiche le sujet du commentaire de chaque livre d’or dans une `<h4>` élément avec le corps situé au-dessous de l’objet. Notez que syntaxe permettant d’afficher le corps prend les données retournées par le `Eval("Body")` instruction de liaison de données, il convertit en une chaîne et les sauts de ligne de la remplace par le `<br />` élément. Cette conversion est nécessaire pour afficher les sauts de ligne entrés lors de la soumission du commentaire dans la mesure où un espace blanc est ignoré par HTML. Signature de l’utilisateur est affiché sous le corps en italique, suivi par ville accueil de l’utilisateur, un lien vers sa page d’accueil, la date et heure que le commentaire a été effectué et le nom d’utilisateur de la personne ayant laissé le commentaire.

Prenez un moment pour afficher la page via un navigateur. Vous devez voir les commentaires que vous avez ajouté pour le livre d’or à l’étape 5 affichés ici.


[![Les commentaires du livre d’or affiche à présent de GuestBook.aspx](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Figure 19**: `Guestbook.aspx` affiche maintenant les commentaires du livre d’or ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image57.png))


Essayez d’ajouter un nouveau commentaire à du livre d’or. Lorsque vous cliquez sur le `PostCommentButton` bouton la page effectue une publication automatique et le commentaire est ajouté à la base de données, mais le contrôle ListView n’est pas mis à jour pour afficher le nouveau commentaire. Cela peut être résolu à l’aide :

- La mise à jour le `PostCommentButton` du bouton `Click` Gestionnaire d’événements afin qu’elle appelle le contrôle ListView `DataBind()` méthode après avoir inséré le nouveau commentaire dans la base de données, ou
- Définition du contrôle ListView `EnableViewState` propriété `false`. Cette approche fonctionne, car en désactivant l’état d’affichage du contrôle, il doit relier aux données sous-jacentes à chaque publication.

Le site Web du didacticiel téléchargeable à partir de ce didacticiel illustre ces deux techniques. Le contrôle ListView `EnableViewState` propriété `false` et le code nécessaire pour relier par programmation les données pour le ListView est présent dans le `Click` Gestionnaire d’événements, mais il est commenté.

> [!NOTE]
> Actuellement le `AdditionalUserInfo.aspx` page permet à l’utilisateur afficher et modifier leurs paramètres town, page d’accueil et signature domestiques. Il peut être utile mettre à jour `AdditionalUserInfo.aspx` à afficher connecté dans les commentaires de livre d’or de l’utilisateur. Autrement dit, en plus d’examiner et de modifier ses informations, un utilisateur peut visiter le `AdditionalUserInfo.aspx` page pour consulter le livre d’or commentaires qu’elle est effectuée dans le passé. Je laisse cela en guise d’exercice pour les lecteurs que cela intéresse.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Étape 6 : Personnalisation du contrôle CreateUserWizard pour inclure une Interface pour le Home Town, la page d’accueil et la Signature

Le `SELECT` requête utilisée par le `Guestbook.aspx` page utilise un `INNER JOIN` pour combiner les enregistrements associés entre les `GuestbookComments`, `UserProfiles`, et `aspnet_Users` tables. Si un utilisateur qui n’a aucun enregistrement dans `UserProfiles` effectue un livre d’or de commentaire, le commentaire ne s’afficheront dans le ListView, car le `INNER JOIN` retourne uniquement `GuestbookComments` enregistrements lorsqu’il existe des enregistrements correspondants dans `UserProfiles` et `aspnet_Users`. Et comme nous l’avons vu à l’étape 3, si un utilisateur ne dispose pas d’un enregistrement dans `UserProfiles` elle ne peut pas afficher ou modifier ses paramètres dans le `AdditionalUserInfo.aspx` page.

Inutile de dire que, en raison de notre conception décisions, il est important que chaque compte d’utilisateur dans le système d’appartenance qui ont une correspondance enregistrent dans le `UserProfiles` table. Ce que nous voulons concerne un enregistrement correspondant à ajouter à `UserProfiles` chaque fois qu’un nouveau compte d’utilisateur d’appartenance est créé via le contrôle CreateUserWizard.

Comme indiqué dans le [ *création de comptes utilisateur* ](creating-user-accounts-cs.md) (didacticiel), une fois le nouveau compte d’utilisateur d’appartenance est créé le contrôle CreateUserWizard déclenche son [ `CreatedUser` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Nous pouvons créer un gestionnaire d’événements pour cet événement, obtenir l’ID d’utilisateur pour l’utilisateur nouvellement créé, puis insérer un enregistrement dans le `UserProfiles` table avec les valeurs par défaut pour le `HomeTown`, `HomepageUrl`, et `Signature` colonnes. De plus, il est possible pour inviter l’utilisateur à ces valeurs en personnalisant l’interface du contrôle CreateUserWizard pour inclure des zones de texte supplémentaires.

Commençons par examiner comment ajouter une nouvelle ligne à la `UserProfiles` table dans le `CreatedUser` Gestionnaire d’événements avec les valeurs par défaut. Ensuite, nous verrons comment personnaliser l’interface utilisateur du contrôle CreateUserWizard pour inclure des champs de formulaire supplémentaires pour collecter la ville natale, page d’accueil et signature du nouvel utilisateur.

### <a name="adding-a-default-row-touserprofiles"></a>Ajout d’une ligne par défaut pour`UserProfiles`

Dans le [ *création de comptes utilisateur* ](creating-user-accounts-cs.md) didacticiel, nous avons ajouté un contrôle CreateUserWizard pour le `CreatingUserAccounts.aspx` page dans le `Membership` dossier. Pour disposer le contrôle CreateUserWizard contrôle ajouter un enregistrement à `UserProfiles` table lors de la création de compte utilisateur, nous devons mettre à jour les fonctionnalités du contrôle CreateUserWizard. Plutôt que d’effectuer ces modifications sur le `CreatingUserAccounts.aspx` page, nous allons ajouter à la place un nouveau contrôle CreateUserWizard à `EnhancedCreateUserWizard.aspx` page et apportez les modifications pour ce didacticiel il.

Ouvrez le `EnhancedCreateUserWizard.aspx` page dans Visual Studio et faites glisser un contrôle CreateUserWizard à partir de la boîte à outils vers la page. Définir le contrôle CreateUserWizard `ID` propriété `NewUserWizard`. Comme expliqué dans la <a id="_msoanchor_5"> </a> [ *création de comptes utilisateur* ](creating-user-accounts-cs.md) didacticiel, l’interface utilisateur de CreateUserWizard par défaut vous invite à entrer le visiteur pour les informations nécessaires. Une fois que ces informations ont été fournies, le contrôle crée en interne un nouveau compte d’utilisateur dans le cadre de l’appartenance, tout cela sans nous avoir à écrire une seule ligne de code.

Le contrôle CreateUserWizard élève un nombre d’événements au cours de son flux de travail. Une fois un visiteur fournit des informations de la demande et envoie le formulaire, le contrôle CreateUserWizard initialement déclenche son [ `CreatingUser` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). S’il existe un problème pendant le processus de création, le [ `CreateUserError` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) est déclenché ; Toutefois, si l’utilisateur est créé avec succès, puis le [ `CreatedUser` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) est déclenché. Dans le <a id="_msoanchor_6"> </a> [ *création de comptes utilisateur* ](creating-user-accounts-cs.md) didacticiel, nous avons créé un gestionnaire d’événements pour le `CreatingUser` événements pour vous assurer que le nom d’utilisateur fourni ne contenait pas tout début ou les espaces de fin, et que le nom d’utilisateur ne pas apparaître n’importe où dans le mot de passe.

Pour ajouter une ligne dans le `UserProfiles` table pour l’utilisateur récemment créée, nous devons créer un gestionnaire d’événements pour le `CreatedUser` événement. Au moment où le `CreatedUser` événement a été déclenché, le compte d’utilisateur a déjà été créé dans le cadre de l’appartenance, nous permet de récupérer la valeur d’ID d’utilisateur du compte.

Créer un gestionnaire d’événements pour le `NewUserWizard`de `CreatedUser` événement et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Êtres de code ci-dessus en récupérant l’ID d’utilisateur du compte d’utilisateur juste-ajouté. Cela est accompli en utilisant le `Membership.GetUser(username)` méthode pour retourner des informations concernant un utilisateur particulier, puis en utilisant le `ProviderUserKey` propriété à récupérer leur ID d’utilisateur. Le nom d’utilisateur entré par l’utilisateur dans le contrôle CreateUserWizard est disponible via son [ `UserName` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Ensuite, la chaîne de connexion est récupérée à partir de `Web.config` et `INSERT` instruction est spécifiée. Les objets ADO.NET sont instanciés et la commande exécutée. Le code assigne un [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) l’instance à la `@HomeTown`, `@HomepageUrl`, et `@Signature` paramètres, ce qui a pour effet de l’insertion de base de données `NULL` valeurs pour le `HomeTown`, `HomepageUrl`, et `Signature` champs.

Visitez le `EnhancedCreateUserWizard.aspx` page via un navigateur et créer un nouveau compte d’utilisateur. Après cela, retournez dans Visual Studio et examinez le contenu de la `aspnet_Users` et `UserProfiles` tables (par exemple, nous l’avons fait dans la Figure 12). Vous devez voir le nouveau compte d’utilisateur dans `aspnet_Users` et correspondante `UserProfiles` ligne (avec `NULL` valeurs pour `HomeTown`, `HomepageUrl`, et `Signature`).


[![Un nouveau compte d’utilisateur et un enregistrement de UserProfiles ont été ajoutés.](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Figure 20**: un nouveau compte d’utilisateur et `UserProfiles` enregistrement ont été ajoutés ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image60.png))


Une fois que le visiteur a fourni des informations sur son nouveau compte et cliqué sur le bouton « Create User », le compte d’utilisateur est créé et une ligne ajoutée à la `UserProfiles` table. CreateUserWizard puis affiche son `CompleteWizardStep`, ce qui affiche un message de réussite et un bouton Continuer. En cliquant sur le bouton Continuer entraîne une publication, mais aucune action n’est effectuée, en laissant de l’utilisateur bloqué sur le `EnhancedCreateUserWizard.aspx` page.

Nous pouvons spécifier une URL pour envoyer l’utilisateur lorsque vous cliquez sur le bouton Continuer via le contrôle CreateUserWizard [ `ContinueDestinationPageUrl` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Définir le `ContinueDestinationPageUrl` propriété à « ~ / Membership/AdditionalUserInfo.aspx ». Accédez au nouvel utilisateur `AdditionalUserInfo.aspx`, où ils peuvent afficher et mettre à jour leurs paramètres.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personnalisation d’Interface de CreateUserWizard à l’invite pour le nouvel utilisateur Home Town, page d’accueil et Signature

L’interface par défaut du contrôle CreateUserWizard est suffisant pour les scénarios de création de compte simple où seules informations de compte d’utilisateur core comme nom d’utilisateur, mot de passe et e-mail doivent être collectées. Mais que se passe-t-il si nous souhaitons inviter le visiteur à entrer son ville natale, la page d’accueil et la signature lors de la création de son compte ? Il est possible de personnaliser l’interface du contrôle CreateUserWizard pour collecter des informations supplémentaires à l’inscription, et ces informations peuvent être utilisées dans le `CreatedUser` Gestionnaire d’événements pour insérer des enregistrements supplémentaires dans la base de données sous-jacente.

Le contrôle CreateUserWizard étend le contrôle de l’Assistant ASP.NET, qui est un contrôle qui permet à un développeur de page définir une série d’ordonnée `WizardSteps`. Le contrôle de l’Assistant affiche l’étape active et fournit une interface de navigation qui permet le visiteur pour vous déplacer dans ces étapes. Le contrôle de l’Assistant est idéal pour décomposer une tâche long en plusieurs étapes. Pour plus d’informations sur le contrôle de l’Assistant, consultez [création d’une Interface utilisateur de pas à pas avec le contrôle de l’Assistant ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Balisage de par défaut du contrôle CreateUserWizard définit deux `WizardSteps`: `CreateUserWizardStep` et `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

La première `WizardStep`, `CreateUserWizardStep`, restitue l’interface qui vous invite à entrer le nom d’utilisateur, mot de passe, e-mail, et ainsi de suite. Une fois le visiteur fournit ces informations et clique sur « Créer un utilisateur », elle s’affiche le `CompleteWizardStep`, qui affiche le message de réussite et un bouton Continuer.

Pour personnaliser l’interface du contrôle CreateUserWizard pour inclure d’autres champs du formulaire, nous pouvons :

- <strong>Créer un ou plusieurs nouveaux</strong><strong>`WizardStep`</strong><strong>s pour contenir les éléments d’interface utilisateur</strong>. Pour ajouter un nouveau `WizardStep` pour le contrôle CreateUserWizard, cliquez sur le « Ajout/suppression `WizardSteps`« lien à partir de sa balise active pour lancer le `WizardStep` éditeur de collections. À partir de là, vous pouvez ajouter, supprimer ou réorganiser les étapes dans l’Assistant. Il s’agit de l’approche que nous utiliserons pour ce didacticiel.

- <strong>Convertir le</strong><strong>`CreateUserWizardStep`</strong><strong>dans une liste modifiable</strong><strong>`WizardStep`</strong><strong>.</strong> Cela remplace le `CreateUserWizardStep` avec un équivalent `WizardStep` dont balisage définit une interface utilisateur qui correspond à la `CreateUserWizardStep`' s. En convertissant le `CreateUserWizardStep` dans un `WizardStep` nous pouvons repositionner les contrôles ou ajouter des éléments d’interface utilisateur supplémentaires à cette étape. Pour convertir le `CreateUserWizardStep` ou `CompleteWizardStep` dans une liste modifiable `WizardStep`, cliquez sur « Créer un utilisateur personnaliser Step » ou « Personnaliser l’étape » lien à partir de la balise active du contrôle.

- **Utiliser une combinaison des deux options ci-dessus.**

Une chose importante à garder à l’esprit est que le contrôle CreateUserWizard exécute son processus de création de compte utilisateur lorsque le bouton « Create User » est activé depuis son `CreateUserWizardStep`. Peu importe si d’autres `WizardStep` s après le `CreateUserWizardStep` ou non.

Lorsque vous ajoutez un personnalisé `WizardStep` au contrôle CreateUserWizard pour recueillir les entrées d’utilisateur supplémentaires, personnalisé `WizardStep` peut être placé avant ou après le `CreateUserWizardStep`. Si elle vient avant le `CreateUserWizardStep` puis l’entrée d’utilisateur supplémentaires collectées à partir de personnalisé `WizardStep` est disponible pour le `CreatedUser` Gestionnaire d’événements. Toutefois, si personnalisé `WizardStep` vient après `CreateUserWizardStep` puis par l’heure personnalisé `WizardStep` s’affiche le nouveau compte d’utilisateur a déjà été créé et le `CreatedUser` événement a déjà été déclenché.

La figure 21 illustre le flux de travail lors de l’ajout `WizardStep` précède le `CreateUserWizardStep`. Étant donné que les informations utilisateur supplémentaires ont été collectées par le temps le `CreatedUser` événement est déclenché, il nous suffit est mise à jour le `CreatedUser` Gestionnaire d’événements à récupérer ces entrées et utilisez-les pour le `INSERT` les valeurs de paramètre de l’instruction (au lieu de `DBNull.Value`).


[![Le flux de travail CreateUserWizard quand un WizardStep supplémentaire précède le CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Figure 21**: The CreateUserWizard Workflow lorsqu’un supplémentaires `WizardStep` Precedes le `CreateUserWizardStep` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image63.png))


Si personnalisé `WizardStep` est placé *après* le `CreateUserWizardStep`, toutefois, le processus de compte d’utilisateur créer se produit avant que l’utilisateur a eu l’occasion d’entrer son ville natale, la page d’accueil ou la signature. Dans ce cas, ces informations supplémentaires doivent être insérées dans la base de données une fois que le compte d’utilisateur a été créé, comme l’illustre la Figure 22.


[![Le flux de travail CreateUserWizard quand un WizardStep supplémentaire vient après le CreateUserWizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Figure 22**: The CreateUserWizard Workflow lorsqu’un supplémentaires `WizardStep` est fourni après le `CreateUserWizardStep` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image66.png))


Le workflow illustré à la Figure 22 attend pour insérer un enregistrement dans le `UserProfiles` jusqu'à ce que la table issue de l’étape 2. Si le visiteur ferme son navigateur après l’étape 1, toutefois, nous sera ont atteint un état où un compte d’utilisateur a été créé, mais aucun enregistrement a été ajouté à `UserProfiles`. Une solution de contournement consiste à disposer d’un enregistrement avec `NULL` ou par défaut les valeurs insérées dans `UserProfiles` dans le `CreatedUser` Gestionnaire d’événements (qui se déclenche après l’étape 1), puis mettez à jour ce enregistrer une fois que l’étape 2 est terminée. Cela garantit qu’un `UserProfiles` enregistrement est ajouté pour le compte d’utilisateur, même si l’utilisateur quitte l’inscription processus au milieu.

Pour ce didacticiel nous allons créer un `WizardStep` qui se produit après la `CreateUserWizardStep` , mais avant le `CompleteWizardStep`. Commencez par en obtenir la WizardStep dans placer et configuré, puis nous allons nous allons examiner le code.

À partir de la balise active du contrôle CreateUserWizard, sélectionnez le « Ajout/suppression `WizardStep` s », qui fait apparaître le `WizardStep` boîte de dialogue Éditeur de Collection. Ajouter un nouveau `WizardStep`, ce qui affecte ses `ID` à `UserSettings`, ses `Title` à « Paramètres de votre » et ses `StepType` à `Step`. Puis de les positionner afin qu’il vient après le `CreateUserWizardStep` (« s’abonner pour votre nouveau compte ») et avant le `CompleteWizardStep` (« Complete »), comme illustré dans la Figure 23.


[![Ajouter un nouveau WizardStep au contrôle CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Figure 23**: ajout d’un nouveau `WizardStep` au contrôle CreateUserWizard ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image69.png))


Cliquez sur OK pour fermer la `WizardStep` boîte de dialogue Éditeur de Collection. La nouvelle `WizardStep` atteste de balisage déclaratif mis à jour du contrôle CreateUserWizard :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Notez la nouvelle `<asp:WizardStep>` élément. Nous devons ajouter l’interface utilisateur pour collecter le nouvel utilisateur ville natale, page d’accueil et signature ici. Vous pouvez entrer ce contenu dans la syntaxe déclarative ou par le biais du concepteur. Pour utiliser le concepteur, sélectionnez l’étape « Vos paramètres » dans la liste déroulante dans la balise active pour voir l’étape dans le concepteur.

> [!NOTE]
> Sélection d’une étape de la liste déroulante de la balise active par le biais des mises à jour le contrôle CreateUserWizard [ `ActiveStepIndex` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), qui spécifie l’index de l’étape de départ. Par conséquent, si vous utilisez cette liste déroulante pour modifier l’étape « Vos paramètres » dans le concepteur, veillez à définir sur « Sign Up for votre nouveau compte » afin que cette étape s’affiche lorsque les utilisateurs visitent tout d’abord le `EnhancedCreateUserWizard.aspx` page.


Créer une interface utilisateur dans l’étape « Vos paramètres » qui contient trois contrôles TextBox nommés `HomeTown`, `HomepageUrl`, et `Signature`. Après la construction de cette interface, balisage déclaratif de CreateUserWizard doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Continuez et visitez cette page via un navigateur et créer un nouveau compte utilisateur, en spécifiant des valeurs pour la ville natale, la page d’accueil et la signature. Après avoir effectué la `CreateUserWizardStep` le compte d’utilisateur est créé dans le cadre de l’appartenance et la `CreatedUser` événement gestionnaire s’exécute, ce qui ajoute une nouvelle ligne à `UserProfiles`, mais avec une base de données `NULL` valeur pour `HomeTown`, `HomepageUrl`, et `Signature`. Les valeurs entrées pour la ville natale, la page d’accueil et la signature ne sont jamais utilisés. Le résultat net est un nouveau compte d’utilisateur avec un `UserProfiles` enregistrer dont `HomeTown`, `HomepageUrl`, et `Signature` champs qui doivent être spécifiés.

Nous avons besoin d’exécuter du code après l’étape « Vos paramètres » qui prend les valeurs de ville, honepage et signature accueil entrés par l’utilisateur et met à jour approprié `UserProfiles` enregistrement. Chaque fois que l’utilisateur se déplace entre les étapes de l’Assistant contrôle, l’Assistant [ `ActiveStepChanged` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) se déclenche. Nous pouvons créer un gestionnaire d’événements pour cet événement et la mise à jour le `UserProfiles` table lors de l’étape « Vos paramètres » est terminée.

Ajouter un gestionnaire d’événements pour le CreateUserWizard `ActiveStepChanged` événement et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Le code ci-dessus commence par déterminer si nous avons atteint l’étape « Terminé ». Étant donné que l’étape « Terminé » se produit immédiatement après l’étape « Vos paramètres », puis lorsque le visiteur atteint « Complete » détaillé qui signifie qu’elle vient de terminer à l’étape « Vos paramètres ».

Dans ce cas, nous devons référencer par programme les contrôles de zone de texte dans le `UserSettings WizardStep`. Pour cela utiliser d’abord le `FindControl` méthode à référencer par programme le `UserSettings WizardStep`, puis à nouveau pour référencer les zones de texte à partir la `WizardStep`. Une fois que les zones de texte ont été référencés, nous sommes prêts à exécuter le `UPDATE` instruction. Le `UPDATE` instruction a le même nombre de paramètres que ceux du `INSERT` instruction dans le `CreatedUser` Gestionnaire d’événements, mais ici, nous utilisons les valeurs de ville, page d’accueil et signature accueil fournies par l’utilisateur.

Avec ce gestionnaire d’événements en place, visitez le `EnhancedCreateUserWizard.aspx` page via un navigateur et créer un nouveau compte d’utilisateur spécifiant des valeurs pour la ville natale, la page d’accueil et la signature. Après avoir créé le nouveau compte, vous devez être redirigé vers le `AdditionalUserInfo.aspx` page, où les entrées juste accueil town, page d’accueil et signature informations s’affichent.

> [!NOTE]
> Notre site Web a actuellement deux pages à partir de laquelle un visiteur peut créer un nouveau compte : `CreatingUserAccounts.aspx` et `EnhancedCreateUserWizard.aspx`. Page de connexion et le plan de site du site Web pointent vers le `CreatingUserAccounts.aspx` page, mais la `CreatingUserAccounts.aspx` page n’invite pas l’utilisateur pour obtenir ses informations de ville, page d’accueil et signature domestiques et n’ajoute pas d’une ligne correspondante à `UserProfiles`. Par conséquent, mettre à jour le `CreatingUserAccounts.aspx` page afin qu’il offre cette fonctionnalité ou de mettre à jour de la page plan de site et de connexion pour faire référence à `EnhancedCreateUserWizard.aspx` au lieu de `CreatingUserAccounts.aspx`. Si vous choisissez cette dernière option, veillez à mettre à jour le `Membership` du dossier `Web.config` fichier afin d’autoriser l’accès aux utilisateurs anonymes le `EnhancedCreateUserWizard.aspx` page.


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu les techniques de modélisation des données relatives aux comptes d’utilisateurs dans le cadre de l’appartenance. En particulier, nous avons vu de modélisation des entités qui partagent une relation un-à-plusieurs avec des comptes d’utilisateur, ainsi que des données qui partagent une relation un à un. En outre, nous avons vu comment cela lié informations peuvent être affichées, insérées et mis à jour, avec quelques exemples utilisant le contrôle SqlDataSource et autres à l’aide de code ADO.NET.

Ce didacticiel termine la présentation des comptes d’utilisateur. En commençant par le didacticiel suivant, nous sera porter notre attention aux rôles. Le prochain plusieurs didacticiels, nous allons examiner le framework de rôles, consultez Comment créer de nouveaux rôles, comment attribuer des rôles aux utilisateurs, comment pour déterminer quels rôles un utilisateur appartient à et pour appliquer l’autorisation par rôle.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [L’accès et la mise à jour des données dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Contrôle de l’Assistant 2.0 ASP.NET](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Création d’une Interface utilisateur pas à pas avec le contrôle de l’Assistant 2.0 ASP.NET](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Création de paramètres de contrôle de source de données personnalisé](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personnalisation du contrôle CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Démarrages rapides de contrôle DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Affichage des données avec le contrôle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [DISSECTION les contrôles de Validation dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Insertion de modification et suppression de données](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Validation de formulaire dans ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Collecte des informations de l’enregistrement d’utilisateur personnalisées](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profils dans ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Le contrôle asp : ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Démarrage rapide de profils utilisateur](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](user-based-authorization-cs.md)
> [Suivant](creating-the-membership-schema-in-sql-server-vb.md)
