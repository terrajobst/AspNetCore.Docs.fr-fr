---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migration des données du fournisseur universel pour l’appartenance et les profils utilisateur vers ASP.NET Identity (c#) | Microsoft Docs
author: rustd
description: Ce didacticiel décrit les étapes nécessaires à la migration des utilisateurs et les données de rôle et les données de profil utilisateur créées à l’aide de fournisseurs universels d’une application existante...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: dce27af273adb45c1ad8f07790c98d2fa5862f67
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377958"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migration des données du fournisseur universel pour l’appartenance et les profils utilisateur vers ASP.NET Identity (c#)
====================
par [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Ce didacticiel décrit les étapes nécessaires à la migration des utilisateurs et les données de rôle et les données de profil utilisateur créées à l’aide de fournisseurs universels d’une application existante vers le modèle d’identité ASP.NET. L’approche mentionnée ici pour migrer les données de profil utilisateur peut être utilisé dans une application avec appartenance SQL également.


Avec la version de Visual Studio 2013, l’équipe ASP.NET a introduit un nouveau système d’identité ASP.NET, et vous pouvez en savoir plus sur cette version [ici](../../index.md). Suite à l’article pour migrer des applications web à partir de [appartenance SQL vers le nouveau système d’identité](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), cet article illustre les étapes permettant de migrer des applications existantes qui suivent le modèle de fournisseurs pour la gestion des rôles et des utilisateurs pour le nouveau modèle d’identité. L’objectif de ce didacticiel sera principalement sur la migration sur les données de profil utilisateur pour connecter en toute transparence dans le nouveau système. Migration des informations utilisateur et le rôle sont similaires pour l’appartenance SQL. L’approche suivie pour migrer les données de profil peut être utilisé dans une application avec appartenance SQL ainsi.

Par exemple, nous allons commencer par une application web créée à l’aide de Visual Studio 2012 qui utilise le modèle de fournisseurs. Nous allons ensuite ajouter du code pour la gestion de profil, inscrire un utilisateur, ajouter des données de profil pour les utilisateurs, migrer le schéma de base de données et modifiez l’application pour utiliser le système d’identité pour la gestion des rôles et des utilisateurs. Comme un test de migration, les utilisateurs créés à l’aide de fournisseurs universels doivent pouvoir se connecter et de nouveaux utilisateurs doivent être en mesure d’enregistrer.

> [!NOTE]
> Vous trouverez l’exemple complet à [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Résumé de migration de données de profil

Avant de commencer avec les migrations, examinons l’expérience du stockage des données de profil dans le modèle de fournisseurs. Données de profil pour l’application, les utilisateurs peuvent être stockées de plusieurs façons, les plus courantes entre eux en cours à l’aide les fournisseurs de profils intégré fournis avec les fournisseurs universels. Incluent les étapes

1. Ajouter une classe qui a les propriétés utilisées pour stocker les données de profil.
2. Ajoutez une classe qui étend « ProfileBase » et implémente des méthodes pour obtenir les données de profil ci-dessus pour l’utilisateur.
3. Activer à l’aide de fournisseurs de profils par défaut dans le *web.config* de fichiers et de définir la classe déclarée à l’étape #2 pour être utilisé dans l’accès aux informations de profil.

Les informations de profil sont stockées en tant que code xml sérialisé et des données binaires dans la table « Profiles » dans la base de données.

Après la migration de l’application pour utiliser le nouveau système d’identité ASP.NET, les informations de profil sont désérialisées et stockées en tant que propriétés sur la classe d’utilisateur. Chaque propriété peut alors être mappée sur les colonnes de la table utilisateur. L’avantage est que les propriétés pouvant être utilisées directement à l’aide de la classe d’utilisateur en plus de ne pas avoir à sérialiser/désérialiser les informations de données chaque fois lorsque vous y accédez.

## <a name="getting-started"></a>Prise en main

1. Créer une nouvelle application ASP.NET 4.5 Web Forms dans Visual Studio 2012. L’exemple actuel utilise le modèle Web Forms, mais vous pouvez utiliser également Application MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Créez un dossier « Modèles » pour stocker les informations de profil  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Par exemple, nous stocker la date de naissance, ville, hauteur et poids de l’utilisateur dans le profil. La hauteur et le poids sont stockés sous la forme d’une classe personnalisée appelée « PersonalStats ». Pour stocker et récupérer le profil, nous avons besoin d’une classe qui étend 'ProfileBase'. Nous allons créer une nouvelle classe « AppProfile » pour obtenir et stocker des informations de profil.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Activer le profil dans le *web.config* fichier. Entrez le nom de classe à utiliser pour enregistrer/récupérer les informations utilisateur créées à l’étape 3 de #.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Ajouter une page web forms dans le dossier « Compte » pour obtenir les données de profil de l’utilisateur et le stocker. Cliquez avec le bouton droit sur le projet et sélectionnez « Ajouter un nouvel élément ». Ajouter une nouvelle page Web Forms avec page maître 'AddProfileData.aspx'. Copiez ce qui suit dans la section « MainContent » :

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Ajoutez le code suivant dans le code-behind :

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Ajoutez l’espace de noms sous le AppProfile classe est définie pour supprimer les erreurs de compilation.
6. Exécutez l’application et créer un nouvel utilisateur avec le nom d’utilisateur «**olduser ».** Accédez à la page « AddProfileData » et ajoutez les informations de profil pour l’utilisateur.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Vous pouvez vérifier que les données sont stockées en tant que code xml sérialisé dans la table « Profiles » à l’aide de la fenêtre Explorateur de serveurs. Dans Visual Studio, à partir du menu « Affichage », choisissez « Explorateur de serveurs ». Il doit y avoir une connexion de données pour la base de données définie dans le *web.config* fichier. Cliquez sur la connexion de données pour afficher différentes sous-catégories. Développez « Tables » pour afficher les tables différentes dans votre base de données, puis cliquez avec le bouton droit sur « Profiles » et choisissez « Afficher les données de Table » pour afficher les données de profil stockées dans la table de profils.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migration du schéma de base de données

Pour que la base de données existante fonctionne avec le système d’identité, nous devons mettre à jour le schéma dans la base de données d’identité pour prendre en charge les champs que nous avons ajouté à la base de données d’origine. Cela est possible à l’aide de scripts SQL pour créer des tables et de copier les informations existantes. Dans la fenêtre « Explorateur de serveurs », développez « DefaultConnection » pour afficher les tables. Cliquez avec le bouton droit sur les Tables et sélectionnez « Nouvelle requête »

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Collez le script SQL à partir de [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) et exécutez-le. Si la « DefaultConnection » est actualisée, nous pouvons voir que les nouvelles tables sont ajoutées. Vous pouvez vérifier les données contenues dans les tables pour voir que les informations a été migrées.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migration de l’application à utiliser ASP.NET Identity

1. Installez les packages Nuget nécessaires pour ASP.NET Identity :

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Vous pouvez trouver plus d’informations sur la gestion des packages Nuget [ici](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Pour travailler avec des données existantes dans la table, nous devons créer des classes de modèle qui mappent vers les tables et de les raccordement dans le système d’identité. Dans le cadre du contrat d’identité, les classes de modèle doivent implémenter les interfaces définies dans la dll Identity.Core ou étendent l’implémentation existante de ces interfaces disponibles dans Microsoft.AspNet.Identity.EntityFramework. Nous utiliserons les classes existantes pour le rôle, les connexions utilisateur et les revendications d’utilisateur. Nous devons utiliser un utilisateur personnalisé pour notre exemple. Cliquez avec le bouton droit sur le projet et créer un nouveau dossier « IdentityModels ». Ajoutez une nouvelle classe « User » comme indiqué ci-dessous :

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Notez que le « ProfileInfo » est désormais une propriété sur la classe d’utilisateur. Par conséquent, nous pouvons utiliser la classe d’utilisateur pour travailler directement avec les données de profil.

Copiez les fichiers dans le **IdentityModels** et **IdentityAccount** dossiers à partir de la source de téléchargement ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Ces messages ont les autres classes de modèle et les nouvelles pages nécessaires pour l’utilisateur et de gestion des rôles à l’aide de l’API d’identité ASP.NET. L’approche utilisée est similaire à l’appartenance SQL et une explication détaillée peut être trouvée [ici](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copie de données de profil pour les nouvelles tables

Comme mentionné précédemment, nous avons besoin désérialiser les données xml dans les tables de profils et les stocker dans les colonnes de la table AspNetUsers. Les nouvelles colonnes ont été créés dans la table users dans l’étape précédente, tout ce qui reste consiste donc à remplir les colonnes avec les données nécessaires. Pour ce faire, nous allons utiliser une application de console qui est exécutée une fois pour remplir les colonnes nouvellement créées dans le tableau des utilisateurs.

1. Créer une nouvelle application console dans la solution existante.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installez la dernière version du package d’Entity Framework.
3. Ajouter l’application web créée ci-dessus en tant que référence à l’application de console. Pour faire ce avec le bouton droit, cliquez sur le projet, puis « Ajouter des références », puis la Solution, cliquez sur le projet, puis cliquez sur OK.
4. Copiez le code ci-dessous dans la classe Program.cs. Cette logique lit les données de profil pour chaque utilisateur, le sérialise en tant qu’objet de « ProfileInfo » et le stocke dans la base de données.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Certains des modèles utilisés sont définies dans le dossier « IdentityModels » du projet d’application web, vous devez inclure les espaces de noms correspondant.
5. Le code ci-dessus fonctionne sur le fichier de base de données dans l’application\_dossier de données du projet d’application web est créé dans les étapes précédentes. Pour faire référence à qui, vous devez mettre à jour la chaîne de connexion dans le fichier app.config de l’application de console avec la chaîne de connexion dans le fichier web.config de l’application web. Également fournir le chemin d’accès physique complet dans la propriété « AttachDbFilename ».
6. Ouvrez une invite de commandes et accédez au dossier bin de l’application de console ci-dessus. Exécutez le fichier exécutable et vérifiez la sortie du journal, comme indiqué dans l’image suivante.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Ouvrez la table 'AspNetUsers' dans l’Explorateur de serveurs et vérifier les données dans les nouvelles colonnes qui contiennent les propriétés. Ils doivent être mis à jour avec les valeurs de propriété correspondantes.

## <a name="verify-functionality"></a>Vérifier la fonctionnalité

Utilisez les pages d’appartenance nouvellement ajouté qui sont implémentées à l’aide d’ASP.NET Identity à se connecter à un utilisateur à partir de l’ancienne base de données. L’utilisateur doit être en mesure de vous connecter en utilisant les informations d’identification. Essayez les autres fonctionnalités telles que l’ajout d’OAuth, création d’un utilisateur, modifier un mot de passe, ajout de rôles, d’ajouter des utilisateurs aux rôles, etc.

Les données de profil pour l’ancien utilisateur et les nouveaux utilisateurs doivent être récupérées et stockées dans la table des utilisateurs. L’ancienne table ne doit plus être référencé.

## <a name="conclusion"></a>Conclusion

L’article décrit le processus de migration d’applications web qui a utilisé le modèle de fournisseur pour l’appartenance à ASP.NET Identity. En outre, l’article décrites pour les utilisateurs à être raccordés dans le système d’identité, la migration des données de profil. Veuillez laisser des commentaires ci-dessous pour les questions et problèmes rencontrés lors de la migration de votre application.

*Merci à Rick Anderson et Robert McMurray d’avoir relu cet article.*
