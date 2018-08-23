---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Modifiez la clé primaire pour les utilisateurs dans ASP.NET Identity | Microsoft Docs
author: tfitzmac
description: Dans Visual Studio 2013, l’application web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur. ASP.NET Identity vous permet de modifier le type de la...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2ec9894df2f9a48ef482715ce71bb09fb4758127
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837448"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Modifier la clé primaire pour les utilisateurs dans ASP.NET Identity
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Dans Visual Studio 2013, l’application web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur. ASP.NET Identity vous permet de modifier le type de la clé pour répondre aux besoins de vos données. Par exemple, vous pouvez modifier le type de la clé à partir d’une chaîne en entier.
> 
> Cette rubrique montre comment commencer avec la valeur par défaut application web et modifier la clé de compte utilisateur à un entier. Vous pouvez utiliser les mêmes modifications à implémenter n’importe quel type de clé dans votre projet. Il montre comment effectuer ces modifications dans l’application web par défaut, mais vous pouvez appliquer des modifications similaires à une application personnalisée. Il montre les modifications nécessaires lorsque vous travaillez avec MVC ou Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - Visual Studio 2013 avec Update 2 (ou version ultérieure)
> - ASP.NET Identity 2.1 ou version ultérieure


Pour effectuer les étapes décrites dans ce didacticiel, vous devez disposer de Visual Studio 2013 Update 2 (ou version ultérieure) et une application web créée à partir du modèle d’Application Web ASP.NET. Le modèle modifié dans Update 3. Cette rubrique montre comment modifier le modèle dans Update 2 et 3 de la mise à jour.

Cette rubrique contient les sections suivantes :

- [Modifier le type de la clé dans la classe d’identité utilisateur](#userclass)
- [Ajouter des classes personnalisées de l’identité qui utilisent le type de clé](#customclass)
- [Modifier le Gestionnaire de contexte utilisateur et pour utiliser le type de clé](#context)
- [Modifier la configuration de démarrage à utiliser le type de clé](#startup)
- [Pour MVC avec Update 2, modifiez le AccountController pour passer le type de clé](#mvcupdate2)
- [Pour MVC avec Update 3, modifier AccountController les ManageController pour passer le type de clé](#mvcupdate3)
- [Pour les formulaires Web avec Update 2, modifiez les pages de compte pour passer le type de clé](#webformsupdate2)
- [Pour les formulaires Web avec Update 3, modifiez les pages de compte pour passer le type de clé](#webformsupdate3)
- [Exécuter l’application](#run)
- [Autres ressources](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Modifier le type de la clé dans la classe d’identité utilisateur

Dans votre projet à partir du modèle d’Application Web ASP.NET, spécifiez que la classe ApplicationUser utilise un entier pour la clé pour les comptes d’utilisateur. Dans IdentityModels.cs, modifiez la classe ApplicationUser d’hériter de IdentityUser qui a un type de **int** pour le paramètre générique TKey. Vous transmettez également les noms des trois classes personnalisées dont vous n’avez pas encore implémenté.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Vous avez changé le type de la clé, mais par défaut, le reste de l’application suppose toujours que la clé est une chaîne. Vous devez indiquer explicitement le type de la clé dans le code qui suppose une chaîne.

Dans le **ApplicationUser** de classe, de modifier le **GenerateUserIdentityAsync** (méthode) à inclure int, comme indiqué dans le code en surbrillance ci-dessous. Cette modification n’est pas nécessaire pour les projets Web Forms avec le modèle de mise à jour 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Ajouter des classes personnalisées de l’identité qui utilisent le type de clé

Les autres classes d’identité, telles que les IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, sont toujours configurées pour utiliser une clé de chaîne. Créer de nouvelles versions de ces classes qui spécifient un entier pour la clé. Vous n’avez pas besoin de fournir beaucoup code d’implémentation dans ces classes, vous définissez principalement simplement int comme clé.

Ajoutez les classes suivantes à votre fichier IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Modifier le Gestionnaire de contexte utilisateur et pour utiliser le type de clé

Dans IdentityModels.cs, modifiez la définition de la **ApplicationDbContext** classe à utiliser votre nouveau personnalisé des classes et une **int** pour la clé, comme indiqué dans le code en surbrillance.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Le paramètre ThrowIfV1Schema n’est plus valide dans le constructeur. Modifiez le constructeur afin qu’il ne passe pas une valeur ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Ouvrez IdentityConfig.cs et modifiez le **ApplicationUserManger** classe pour la conservation des données de banque de classe à utiliser votre nouvel utilisateur et un **int** pour la clé.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

Dans le modèle de mise à jour 3, vous devez modifier la classe ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Modifier la configuration de démarrage à utiliser le type de clé

Dans Startup.Auth.cs, remplacez le code OnValidateIdentity, comme indiqué ci-dessous. Notez que la définition de getUserIdCallback, analyse la valeur de chaîne en entier.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Si votre projet ne reconnaît pas l’implémentation générique de la **GetUserId** (méthode), vous devrez peut-être mettre à jour le package NuGet d’identité ASP.NET à la version 2.1

Vous avez effectué un grand nombre de modifications pour les classes d’infrastructure utilisées par ASP.NET Identity. Si vous essayez de compiler le projet, vous remarquerez un grand nombre d’erreurs. Heureusement, les erreurs restantes sont similaires. La classe Identity attend un entier pour la clé, mais le contrôleur (ou un formulaire Web) est en passant une valeur de chaîne. Dans chaque cas, vous devez convertir à partir d’une chaîne et un entier en appelant **GetUserId&lt;int&gt;**. Vous pouvez parcourir la liste d’erreurs de compilation ou suivre les modifications ci-dessous.

Les modifications restantes varient selon le type de projet que vous créez et quelle mise à jour que vous avez installé dans Visual Studio. Vous pouvez accéder directement à la section appropriée via les liens suivants

- [Pour MVC avec Update 2, modifiez le AccountController pour passer le type de clé](#mvcupdate2)
- [Pour MVC avec Update 3, modifier AccountController les ManageController pour passer le type de clé](#mvcupdate3)
- [Pour les formulaires Web avec Update 2, modifiez les pages de compte pour passer le type de clé](#webformsupdate2)
- [Pour les formulaires Web avec Update 3, modifiez les pages de compte pour passer le type de clé](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Pour MVC avec Update 2, modifiez le AccountController pour passer le type de clé

Ouvrez le fichier AccountController.cs. Vous devez modifier les méthodes suivantes.

**ConfirmEmail** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Dissocier** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Vous pouvez à présent [exécuter l’application](#run) et inscrire un nouvel utilisateur.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Pour MVC avec Update 3, modifier AccountController les ManageController pour passer le type de clé

Ouvrez le fichier AccountController.cs. Vous devez modifier la méthode suivante.

**ConfirmEmail** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Ouvrez le fichier ManageController.cs. Vous devez modifier les méthodes suivantes.

**Index** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** méthodes

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** méthodes

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** (méthode)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Vous pouvez à présent [exécuter l’application](#run) et inscrire un nouvel utilisateur.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Pour les formulaires Web avec Update 2, modifiez les pages de compte pour passer le type de clé

Pour les formulaires Web avec Update 2, vous devez modifier les pages suivantes.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Vous pouvez à présent [exécuter l’application](#run) et inscrire un nouvel utilisateur.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Pour les formulaires Web avec Update 3, modifiez les pages de compte pour passer le type de clé

Pour les formulaires Web avec Update 3, vous devez modifier les pages suivantes.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Exécuter l’application

Vous avez terminé toutes les modifications nécessaires dans le modèle d’Application Web par défaut. Exécutez l’application et inscrire un nouvel utilisateur. Après avoir inscrit l’utilisateur, vous remarquerez que la table AspNetUsers possède une colonne d’Id qui est un entier.

![nouvelle clé primaire](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Si vous avez précédemment créé à l’identité ASP.NET tables avec une autre clé primaire, vous devez apporter des modifications supplémentaires. Si possible, supprimez simplement la base de données existante. La base de données sera recréée avec la conception correcte lorsque vous exécutez l’application web et ajoutez un nouvel utilisateur. Si la suppression n’est pas possible, exécutez les migrations code first pour modifier les tables. Toutefois, la nouvelle clé primaire entière ne sera pas être définie comme une propriété d’identité de SQL dans la base de données. Vous devez définir manuellement la colonne Id comme une identité.

<a id="other"></a>
## <a name="other-resources"></a>Autres ressources

- [Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migration d’un site web existant de l’appartenance SQL vers ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migration des données du fournisseur universel pour l’appartenance et les profils utilisateur vers ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Exemple d’application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) avec la clé primaire modifiée
