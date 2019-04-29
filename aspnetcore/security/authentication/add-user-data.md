---
title: Ajouter, télécharger et supprimer des données utilisateur à l’identité dans un projet ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des données utilisateur personnalisées à l’identité dans un projet ASP.NET Core. Supprimer des données par RGPD.
ms.author: riande
ms.date: 6/16/2018
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 1b7cac9206776e9290326511247b6db33f13cb32
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165087"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet article explique comment :

* Ajouter des données utilisateur personnalisées pour une application web ASP.NET Core.
* Décorez le modèle de données utilisateur personnalisée avec le [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) par conséquent, il est automatiquement disponible pour téléchargement et la suppression d’attributs. Rendre les données peuvent être téléchargés et supprimés à mieux répondre à [RGPD](xref:security/gdpr) configuration requise.

L’exemple de projet est créé à partir d’une application web de Pages Razor, mais les instructions sont similaires pour une application web ASP.NET Core MVC.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/2.2-SDK.md)]

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**. Nommez le projet **application Web 1** si vous souhaitez qu’il correspond à l’espace de noms de la [télécharger l’exemple](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.
* Sélectionnez **Application Web ASP.NET Core** > **OK**
* Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante
* Sélectionnez **Application Web**  > **OK**
* Générez et exécutez le projet.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Exécuter le Générateur de modèles automatique identité

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.
* Dans le volet gauche de la **ajouter une structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.
* Dans le **identité ADD** boîte de dialogue, les options suivantes :
  * Sélectionnez le fichier de disposition existante *~/Pages/Shared/_Layout.cshtml*
  * Sélectionnez les fichiers suivants pour remplacer :
    * **Compte/inscrire**
    * **Compte/gérer/Index**
  * Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**. Acceptez le type (**WebApp1.Models.WebApp1Context** si le projet est nommé **application Web 1**).
  * Sélectionnez le **+** bouton pour créer un nouveau **classe utilisateur**. Acceptez le type (**WebApp1User** si le projet est nommé **application Web 1**) > **ajouter**.
* Sélectionnez **ajouter**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au fichier projet (.csproj). Dans le répertoire du projet, exécutez la commande suivante :

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :

```cli
dotnet aspnet-codegenerator identity -h
```

Dans le dossier du projet, exécutez le Générateur de modèles automatique identité :

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Suivez les instructions de [Migrations, UseAuthentication et disposition](xref:security/authentication/scaffold-identity#efm) pour effectuer les étapes suivantes :

* Créer une migration et mettre à jour de la base de données.
* Ajoutez `UseAuthentication` à `Startup.Configure`.
* Ajouter `<partial name="_LoginPartial" />` pour le fichier de disposition.
* Testez l’application :
  * Inscrire un utilisateur
  * Sélectionnez le nouveau nom d’utilisateur (à côté du **déconnexion** lien). Vous devrez peut-être agrandir la fenêtre ou sélectionnez l’icône de barre de navigation pour afficher le nom d’utilisateur et d’autres liens.
  * Sélectionnez le **les données personnelles** onglet.
  * Sélectionnez le **télécharger** bouton et examiné la *PersonalData.json* fichier.
  * Test du **supprimer** bouton, ce qui supprime le connecté sur l’utilisateur.

## <a name="add-custom-user-data-to-the-identity-db"></a>Ajouter des données utilisateur personnalisées pour la base de données d’identité

Mise à jour le `IdentityUser` dérivée de la classe avec des propriétés personnalisées. Si vous avez nommé le projet d’application Web 1, le fichier est nommé *Areas/Identity/Data/WebApp1User.cs*. Mettre à jour le fichier avec le code suivant :

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Data/WebApp1User.cs)]

Propriétés décorée avec le [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribut sont :

* Supprimé quand le *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Page Razor appelle `UserManager.Delete`.
* Inclus dans les données téléchargées par le *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Page Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Mettre à jour de la page Account/Manage/Index.cshtml

Mise à jour le `InputModel` dans *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* code en surbrillance avec les éléments suivants :

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Mise à jour le *Areas/Identity/Pages/Account/Manage/Index.cshtml* avec le balisage en surbrillance suivant :

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

### <a name="update-the-accountregistercshtml-page"></a>Mettre à jour de la page Account/Register.cshtml en

Mise à jour le `InputModel` dans *Areas/Identity/Pages/Account/Register.cshtml.cs* code en surbrillance avec les éléments suivants :

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Mise à jour le *Areas/Identity/Pages/Account/Register.cshtml* avec le balisage en surbrillance suivant :

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Générez le projet.

### <a name="add-a-migration-for-the-custom-user-data"></a>Ajoutez une migration pour les données utilisateur personnalisées

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans Visual Studio **Console du Gestionnaire de Package**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Test de créer, afficher, télécharger, supprimer les données utilisateur personnalisées

Testez l’application :

* Inscrire un nouvel utilisateur.
* Afficher les données utilisateur personnalisées sur le `/Identity/Account/Manage` page.
* Télécharger et afficher les données personnelles les utilisateurs à partir de la `/Identity/Account/Manage/PersonalData` page.
