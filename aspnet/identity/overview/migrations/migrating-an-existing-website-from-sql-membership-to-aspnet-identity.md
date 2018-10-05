---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migration d’un site Web existant à partir de l’appartenance SQL vers ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel illustre les étapes pour migrer une application web existante avec les données d’utilisateur et rôle créées à l’aide de l’appartenance SQL vers la nouvelle identité ASP.NET s...
ms.author: riande
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 393d14799973e9126379743f63f79a7131206f38
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577611"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migration d’un site Web existant à partir de l’appartenance SQL vers ASP.NET Identity
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Ce didacticiel illustre les étapes pour migrer une application web existante avec les données d’utilisateur et rôle créées à l’aide de l’appartenance SQL vers le nouveau système d’identité ASP.NET. Cette approche implique la modification du schéma de base de données existant à celui requis par l’identité ASP.NET et le crochet dans les classes anciens et nouveaux. Une fois que vous adoptez cette approche, une fois que la migration de votre base de données, les futures mises à jour à l’identité seront gérées sans effort.


Pour ce didacticiel, nous allons prendre un modèle d’application web (Web Forms) créé à l’aide de Visual Studio 2010 pour créer des données utilisateur et le rôle. Nous utiliserons ensuite les scripts SQL pour migrer la base de données existante vers les tables requises par le système d’identité. Nous allons ensuite installer les packages NuGet nécessaires et ajouter de nouvelles pages de gestion de compte qui utilisent le système d’identité pour la gestion de l’appartenance. Comme un test de migration, les utilisateurs créés à l’aide de l’appartenance SQL doivent être en mesure de se connecter et nouveaux utilisateurs doivent être en mesure de vous inscrire. Vous trouverez l’exemple complet [ici](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Voir aussi [migration à partir de l’appartenance ASP.NET vers ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Bien démarrer

### <a name="creating-an-application-with-sql-membership"></a>Création d’une application avec l’appartenance SQL

1. Nous devons commencer par une application existante qui utilise l’appartenance SQL et ses données utilisateur et le rôle. Pour les besoins de cet article, nous allons créer une application web dans Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. À l’aide de l’outil de Configuration ASP.NET, créer des 2 utilisateurs : **oldAdminUser** et **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Créer un rôle nommé Administrateur et ajoutez « oldAdminUser » en tant qu’utilisateur appartenant à ce rôle.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Créez une section d’administration du site avec un Default.aspx. Définir la balise d’autorisation dans le fichier web.config pour activer l’accès uniquement aux utilisateurs de rôles d’administrateur. Trouverez ici plus d’informations [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Afficher la base de données dans l’Explorateur de serveurs pour comprendre les tables créées par le système d’appartenance SQL. Les données de connexion utilisateur sont stockées dans le compte aspnet\_utilisateurs et aspnet\_l’appartenance des tables, tandis que les données de rôle sont stockées dans le compte aspnet\_table Roles. Informations sur les utilisateurs dans les rôles qui sont stockées dans le compte aspnet\_UsersInRoles table. Pour la gestion de l’adhésion de base, il suffit de porter les informations contenues dans les tables ci-dessus pour le système d’identité ASP.NET.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migration vers Visual Studio 2013

1. Installer Visual Studio Express 2013 pour le Web ou de Visual Studio 2013 avec le [dernières mises à jour](https://www.microsoft.com/download/details.aspx?id=44921).
2. Ouvrez le projet ci-dessus dans votre version installée de Visual Studio. Si SQL Server Express n’est pas installé sur l’ordinateur, une invite s’affiche lorsque vous ouvrez le projet, étant donné que la chaîne de connexion utilise SQL Express. Vous pouvez soit choisir d’installer SQL Express ou en tant que contourner modification de la chaîne de connexion à la base de données locale. Pour cet article, nous allons le modifier à la base de données locale.
3. Ouvrez le fichier web.config et modifiez la chaîne de connexion. SQLExpess à (LocalDb) v11.0. Supprimer « User Instance = true' à partir de la chaîne de connexion.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Ouvrez l’Explorateur de serveurs et vérifiez que le schéma de table et les données peuvent être observées.
5. Le système d’identité ASP.NET fonctionne avec la version 4.5 ou version ultérieure du framework. Recibler l’application vers la version 4.5 ou version ultérieure.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Générez le projet pour vérifier qu’il n’y a aucune erreur.

### <a name="installing-the-nuget-packages"></a>Installation des packages Nuget

1. Dans l’Explorateur de solutions, cliquez sur le projet &gt; **gérer les Packages NuGet**. Dans la zone de recherche, entrez « Asp.net Identity ». Sélectionnez le package dans la liste des résultats et cliquez sur Installer. Acceptez le contrat de licence en cliquant sur le bouton « J’accepte ». Notez que ce package installe les packages de dépendance : Entity Framework et Microsoft ASP.NET Core Identity. De même installer les packages suivants (ignorer les 4 derniers packages OWIN si vous ne souhaitez pas activer le journal dans OAuth) :

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrer la base de données vers le nouveau système d’identité

L’étape suivante consiste à migrer la base de données existante à un schéma requis par le système d’identité ASP.NET. Pour obtenir ce nous exécutons une SQL script qui dispose d’un jeu de commandes pour créer des tables et de migrer les informations utilisateur existantes vers les nouvelles tables. Le fichier de script se trouve [ici](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Ce fichier de script est spécifique à cet exemple. Si le schéma pour les tables créées à l’aide de l’appartenance SQL personnalisé ou modifié les scripts devoir être modifiée en conséquence.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Comment générer le script SQL pour la migration de schéma

Pour les classes d’identité ASP.NET fonctionne directement avec les données des utilisateurs existants, nous devons migrer le schéma de base de données à celui requis par ASP.NET Identity. Nous pouvons le faire par l’ajout de nouvelles tables et de copier les informations existantes sur ces tables. Par défaut ASP.NET Identity utilise Entity Framework pour mapper les classes de modèle d’identité à la base de données à stocker/récupérer des informations. Ces classes de modèle implémentent les interfaces d’identité de core définissant les objets utilisateur et rôle. Les tables et les colonnes dans la base de données sont basées sur ces classes de modèle. Les classes du modèle Entity Framework dans v2.1.0 d’identité et de leurs propriétés sont tel que défini ci-dessous

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | chaîne | Id | RoleId | ProviderKey | Id |
| Utilisateur | chaîne | Name | ID d’utilisateur | ID d’utilisateur | ClaimType |
| PasswordHash | chaîne |  |  | LoginProvider | ClaimValue |
| SecurityStamp | chaîne |  |  |  | Utilisateur\_Id |
| Messagerie | chaîne |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Numéro de téléphone | chaîne |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Nous avons besoin d’avoir des tables pour chacun de ces modèles avec des colonnes qui correspondent aux propriétés. Le mappage entre les classes et les tables est défini dans le `OnModelCreating` méthode de la `IdentityDBContext`. On parle de la méthode d’API fluent de configuration et vous pouvez trouver plus d’informations [ici](https://msdn.microsoft.com/data/jj591617.aspx). La configuration pour les classes est comme indiqué ci-dessous

| **Classe** | **Table** | **Clé primaire** | **Clé étrangère** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | Utilisateur\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Utilisateur\_Id -&gt;AspnetUsers |

Avec ces informations nous pouvons créer des instructions SQL pour créer des tables. Nous pouvons écrire chaque instruction individuellement ou générer la totalité du script à l’aide des commandes EntityFramework PowerShell que nous pouvons ensuite modifier en fonction des besoins. Pour ce faire, dans Visual Studio open le **Console du Gestionnaire de Package** à partir de la **vue** ou **outils** menu

- Exécutez la commande « Enable-Migrations » pour permettre des migrations d’Entity Framework.
- Exécutez la commande « Add-migration initial », qui crée le code de la configuration initiale pour créer la base de données en C# / VB.
- L’étape finale consiste à exécuter « Update-Database – Script « commande qui génère le script SQL basé sur les classes de modèle.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Ce script de génération de base de données peut être utilisé comme point de départ où nous allons apporter des modifications supplémentaires à ajouter de nouvelles colonnes et de copier des données. L’avantage de ceci est que nous générons la `_MigrationHistory` table qui est utilisée par Entity Framework pour modifier le schéma de base de données lorsque le modèle de classes de modification pour les versions futures de versions d’identité. 

Les informations utilisateur d’appartenance SQL avaient d’autres propriétés en plus de ceux dans la classe de modèle identité utilisateur à savoir le courrier électronique, la tentatives de mot de passe, la dernière date de connexion, la dernière date d’expiration de verrouillage etc. Il s’agit des informations utiles et nous aimerions être reporté sur le système d’identité. Cela est possible en ajoutant des propriétés supplémentaires au modèle utilisateur et en les mappant vers les colonnes de table dans la base de données. Nous pouvons faire cela en ajoutant une classe qui sous-classe la `IdentityUser` modèle. Nous pouvons ajouter les propriétés pour cette classe personnalisée et modifiez le script SQL pour ajouter les colonnes correspondantes lors de la création de la table. Le code de cette classe est décrit plus loin dans l’article. Le script SQL pour créer la `AspnetUsers` après l’ajout de nouvelles propriétés serait de table

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Nous devons ensuite copier les informations existantes à partir de la base de données d’appartenance SQL pour les tables nouvellement ajoutés pour l’identité. Cela est possible via SQL en copiant les données directement à partir d’une table à l’autre. Pour ajouter des données dans les lignes de table, nous utilisons le `INSERT INTO [Table]` construire. Pour copier à partir d’une autre table, nous pouvons utiliser le `INSERT INTO` instruction avec la `SELECT` instruction. Pour obtenir toutes les informations utilisateur que nous avons besoin pour interroger le *aspnet\_utilisateurs* et *aspnet\_appartenance* tables et de copier les données à la *AspNetUsers*table. Nous utilisons le `INSERT INTO` et `SELECT` avec `JOIN` et `LEFT OUTER JOIN` instructions. Pour plus d’informations sur l’interrogation et la copie de données entre les tables, consultez [cela](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) lien. En outre les tables AspnetUserLogins et AspnetUserClaims sont vides pour commencer, car il n’existe aucune information dans l’appartenance SQL qui mappe à cette par défaut. La seule information copiée est pour les utilisateurs et rôles. Pour le projet créé dans les étapes précédentes, la requête SQL pour copier les informations à la table users serait

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Dans l’instruction SQL ci-dessus, les informations relatives à chaque utilisateur à partir de la *aspnet\_utilisateurs* et *aspnet\_appartenance* tables est copié dans les colonnes de la  *AspnetUsers* table. La seule modification faite ici est lorsque nous copions le mot de passe. Étant donné que l’algorithme de chiffrement pour les mots de passe dans l’appartenance SQL utilisé « PasswordSalt du client » et « PasswordFormat », nous copions qui trop, ainsi que le mot de passe haché afin qu’il peut être utilisé pour déchiffrer le mot de passe par identité. Ceci est expliqué dans l’article lors de raccorder un hacheur de mot de passe personnalisé. 

Ce fichier de script est spécifique à cet exemple. Pour les applications qui comportent des tables supplémentaires, les développeurs peuvent suivre une approche similaire pour ajouter des propriétés supplémentaires sur la classe de modèle d’utilisateur et les mapper aux colonnes dans la table AspnetUsers. Pour exécuter le script,

1. Ouvrez l’Explorateur de serveurs. Développez la connexion « ApplicationServices » pour afficher les tables. Cliquez avec le bouton droit sur le nœud Tables et sélectionnez l’option « Nouvelle requête »

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Dans la fenêtre de requête, copiez et collez la totalité du script SQL à partir du fichier Migrations.sql. Exécutez le fichier de script en appuyant sur le bouton de flèche « Exécution ».

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Actualisez la fenêtre Explorateur de serveurs. Cinq nouvelles tables sont créées dans la base de données.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Voici comment les informations contenues dans les tables d’appartenances SQL sont mappés vers le nouveau système d’identité.

    ASPNET\_rôles--&gt; AspNetRoles

    ASP\_netUsers et asp\_netMembership--&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Comme expliqué dans la section ci-dessus, les tables AspNetUserClaims et AspNetUserLogins sont vides. Le champ « Discriminateur » dans la table AspNetUser doit correspondre au nom de classe de modèle qui est défini comme une étape suivante. La colonne PasswordHash est également sous la forme « mot de passe chiffré | salt du mot de passe | format de mot de passe ». Cela vous permet d’utiliser une logique de chiffrement d’appartenance SQL spéciale afin que vous pouvez réutiliser d’anciens mots de passe. Dans, qui est expliquée plus loin dans l’article.

### <a name="creating-models-and-membership-pages"></a>Création de modèles et des pages d’appartenance

Comme mentionné précédemment, la fonctionnalité d’identité utilise Entity Framework pour communiquer avec la base de données pour stocker les informations de compte par défaut. Pour travailler avec des données existantes dans la table, nous devons créer des classes de modèle qui mappent vers les tables et de les raccordement dans le système d’identité. Dans le cadre du contrat d’identité, les classes de modèle doivent implémenter les interfaces définies dans la dll Identity.Core ou étendent l’implémentation existante de ces interfaces disponibles dans Microsoft.AspNet.Identity.EntityFramework.

Dans notre exemple, les tables AspNetRoles, AspNetUserClaims, AspNetLogins et AspNetUserRole ont des colonnes qui sont similaires à l’implémentation existante du système d’identité. Par conséquent, nous pouvons réutiliser les classes existantes pour mapper à ces tables. La table AspNetUser a quelques colonnes supplémentaires qui sont utilisés pour stocker des informations supplémentaires à partir des tables d’appartenances SQL. Ceci peut être mappé en créant une classe de modèle qui étendent l’implémentation existante de « IdentityUser » et ajoutez les propriétés supplémentaires.

1. Dossier de modèles de cette méthode crée dans le projet et ajouter une classe d’utilisateur. Le nom de la classe doit correspondre les données ajoutées dans la colonne « Discriminateur » de la table « AspnetUsers ».

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La classe d’utilisateur doit étendre la classe IdentityUser trouvée dans le *Microsoft.AspNet.Identity.EntityFramework* dll. Déclarer des propriétés de la classe qui mappent vers les colonnes AspNetUser. Les propriétés ID, nom d’utilisateur, PasswordHash et SecurityStamp sont définies dans le IdentityUser et par conséquent, sont omises. Voici le code de la classe d’utilisateur qui possède toutes les propriétés

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Une classe DbContext d’Entity Framework est requise afin de conserver les données dans les modèles à partir de tables et de récupérer des données à partir des tables pour remplir les modèles. *Microsoft.AspNet.Identity.EntityFramework* dll définit la classe IdentityDbContext qui interagit avec les tables d’identité pour récupérer et stocker des informations. L’IdentityDbContext&lt;tuser&gt; prend une classe « TUser » qui peut être toute classe qui étend la classe IdentityUser.

    Créez une classe ApplicationDBContext qui étend IdentityDbContext sous le dossier « Models », en passant dans la classe « User » créée à l’étape 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Gestion des utilisateurs dans le nouveau système d’identité s’effectue à l’aide de la UserManager&lt;tuser&gt; classe définie dans le *Microsoft.AspNet.Identity.EntityFramework* dll. Nous devons créer une classe personnalisée qui étend UserManager, en passant dans la classe « User » créée à l’étape 1.

    Dans le dossier Models, créez une classe UserManager qui étend UserManager&lt;utilisateur&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Les mots de passe des utilisateurs de l’application sont chiffrées et stockées dans la base de données. L’algorithme de chiffrement utilisé dans l’appartenance SQL est différent de celui dans le nouveau système d’identité. Pour réutiliser d’anciens mots de passe, nous devons sélectivement déchiffrer les mots de passe quand anciens utilisateurs de se connectent à l’aide de l’algorithme d’appartenances SQL lors de l’utilisation de l’algorithme de chiffrement dans l’identité pour les nouveaux utilisateurs.

    La classe UserManager a une propriété « PasswordHasher » qui stocke une instance d’une classe qui implémente l’interface « Ipasswordhasher. ». Cela est utilisé pour chiffrer/déchiffrer les mots de passe pendant les transactions de l’authentification utilisateur. Dans la classe UserManager est définie à l’étape 3, créez une classe SQLPasswordHasher et copiez le code ci-dessous.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Résoudre les erreurs de compilation en important les espaces de noms System.Text et System.Security.Cryptography.

    La méthode EncodePassword chiffre le mot de passe en fonction de l’implémentation de services de chiffrement d’appartenance SQL par défaut. Cela provient de la dll de System.Web. Si l’ancienne application utilisé une implémentation personnalisée puis il doit être indiqué ici. Nous devons définir deux autres méthodes *HashPassword* et *VerifyHashedPassword* qui utilisent le *EncodePassword* pour hacher un mot de passe donné ou de vérifier un texte brut (méthode) mot de passe avec un existant dans la base de données.

    Le système d’appartenance SQL utilisé PasswordHash et PasswordSalt du client de PasswordFormat pour hacher le mot de passe entré par les utilisateurs lorsqu’ils s’inscrire ou de modifier leur mot de passe. Lors de la migration, les trois champs sont stockées dans la colonne de PasswordHash dans la table AspNetUser séparée par le ' |' caractères. Lorsqu’un utilisateur se connecte et le mot de passe a ces champs, nous utilisons la cryptographie d’appartenance SQL pour vérifier le mot de passe ; dans le cas contraire, nous utilisons crypto de valeur par défaut du système d’identité pour vérifier le mot de passe. Cette façon d’anciens utilisateurs ne seraient pas obligé de modifier leurs mots de passe une fois l’application migrée.
5. Déclarez le constructeur de la classe UserManager et le transmettre en tant que le SQLPasswordHasher à la propriété dans le constructeur.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Créer des pages de gestion de compte

L’étape suivante de la migration consiste à ajouter des pages de gestion de compte qui permettent un utilisateur enregistrez-vous et connectez-vous. Les pages de compte ancien à partir de l’appartenance SQL utilisent des contrôles qui ne fonctionnent pas avec le nouveau système d’identité. Pour ajouter le nouvel utilisateur des pages de gestion suivent le didacticiel ce lien [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) à partir de l’étape « Ajout de Web Forms pour l’inscription des utilisateurs à votre application » dans la mesure où nous avons déjà créé le projet et ajouté le package NuGet packages.

Nous devons apporter des modifications pour l’exemple fonctionne avec le projet que nous avons ici.

- Le code Register.aspx.cs et Login.aspx.cs derrière l’utilisation de classes le `UserManager` à partir des packages d’identité pour créer un utilisateur. Pour cet exemple montre comment utiliser le UserManager ajoutée dans le dossier de modèles en suivant les étapes mentionnées précédemment.
- Utilisez la classe d’utilisateur créée au lieu du IdentityUser dans Register.aspx.cs et Login.aspx.cs code derrière les classes. Il intercepte dans notre classe d’utilisateur personnalisée dans le système d’identité.
- Le composant à créer la base de données peut être ignoré.
- Le développeur doit définir l’ApplicationId pour le nouvel utilisateur pour faire correspondre l’ID d’application actuel. Cela est possible en interrogeant ApplicationId pour cette application avant la création d’un objet utilisateur dans la classe Register.aspx.cs et en lui affectant avant de créer l’utilisateur. 

    Exemple :

    Définir une méthode dans la page Register.aspx.cs pour interroger le compte aspnet\_Applications table et obtenir l’Id d’application en fonction du nom de l’application

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Maintenant obtenir définir sur l’objet utilisateur

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Utiliser l’ancien nom d’utilisateur et le mot de passe pour se connecter à un utilisateur existant. Pour créer un nouvel utilisateur, utilisez la page d’inscription. Vérifiez également que les utilisateurs sont dans des rôles, comme prévu.

Portage vers le système d’identité aide l’utilisateur à ajouter OAuth (Open Authentication) à l’application. Reportez-vous à l’exemple [ici](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) qui a l’authentification OAuth est activée.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, nous avons montré comment porter des utilisateurs de l’appartenance SQL vers ASP.NET Identity, mais nous n’avez pas déplacer les données de profil. Dans le didacticiel suivant, nous allons voir dans le portage des données de profil de l’appartenance SQL vers le nouveau système d’identité.

Vous pouvez laisser des commentaires en bas de cet article.

*Je remercie Tom Dykstra et Rick Anderson d’avoir relu cet article.*
