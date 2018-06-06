---
title: Ajouter, télécharger et supprimer les données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core. Suppression des données par PIBR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: 23fd792c0d93c038f31ce947e7885ad6e36d119e
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758767"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Ajouter, télécharger et supprimer les données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet article explique comment :

* Ajouter des données utilisateur personnalisées pour une application de web ASP.NET Core.
* Décorer le modèle de données utilisateur personnalisée avec le [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) par conséquent, il est automatiquement disponible pour le téléchargement et la suppression d’attributs. Les données peut être téléchargé et supprimé aide à répondre aux [PIBR](xref:security/gdpr) configuration requise.

L’exemple de projet est créé à partir d’une application de web Pages Razor, mais les instructions sont similaires pour une application de web ASP.NET MVC de base.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**. Nommez le projet **WebApp1** si vous souhaitez qu’il correspond à l’espace de noms de la [télécharger l’exemple](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.
* Sélectionnez **Application ASP.NET Core Web** > **OK**
* Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante
* Sélectionnez **Application Web**  > **OK**
* Générez et exécutez le projet.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

------

## <a name="run-the-identity-scaffolder"></a>Exécutez le scaffolder d’identité

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **un nouvel élément structuré**.
* Dans le volet gauche de la **ajouter une vue de structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.
* Dans le **ajouter identité** boîte de dialogue, les options suivantes :
  * Sélectionnez votre fichier disposition *~/Pages/Shared/_Layout.cshtml*
  * Sélectionnez les fichiers suivants à substituer :
    * **Compte/inscrire**
    * **Compte/gestion/Index**
  * Sélectionnez le **+** bouton permettant de créer un nouveau **classe de contexte de données**. Acceptez le type (**WebApp1.Models.WebApp1Context** si vous avez nommé le projet **WebApp1**).
  * Sélectionnez le **+** bouton permettant de créer un nouveau **classe utilisateur**. Acceptez le type (**WebApp1User** si vous avez nommé le projet **WebApp1**) > **ajouter**.
* Sélectionnez **ajouter**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous n’avez pas encore installé le scaffolder ASP.NET, vous pouvez l’installer maintenant :

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au fichier projet (.csproj). Dans le répertoire du projet, exécutez la commande suivante :

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Exécutez la commande suivante pour répertorier les options de scaffolder d’identité :

```cli
dotnet aspnet-codegenerator identity -h
```

Dans le dossier du projet, exécutez le scaffolder d’identité :

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Suivez les instructions de [Migrations, UseAuthentication et la disposition](xref:security/authentication/scaffold-identity#efm) pour effectuer les étapes suivantes :

* Créer une migration et mise à jour de la base de données.
* Ajoutez `UseAuthentication` à `Startup.Configure`.
* Ajouter `<partial name="_LoginPartial" />` pour le fichier de disposition.
* Testez l’application :
  * Inscrire un utilisateur
  * Sélectionnez le nouveau nom d’utilisateur (à côté du **déconnexion** lien). Vous devrez peut-être agrandir la fenêtre ou sélectionnez l’icône de barre de navigation pour afficher le nom d’utilisateur et d’autres liens.
  * Sélectionnez le **données personnelles** onglet.
  * Sélectionnez le **télécharger** bouton et examiné le *PersonalData.json* fichier.
  * Test du **supprimer** bouton, ce qui supprime le connecté sur l’utilisateur.

## <a name="add-custom-user-data-to-the-identity-db"></a>Ajouter des données utilisateur personnalisées pour la base de données d’identité

Mise à jour le `IdentityUser` dérivée de la classe avec les propriétés personnalisées. Si vous avez nommé votre projet WebApp1, le fichier est nommé *Areas/Identity/Data/WebApp1User.cs*. Mettre à jour le fichier avec le code suivant :

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Propriétés décorée avec le [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribut sont :

* Sont supprimées lorsque le *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Page Razor appelle `UserManager.Delete`.
* Inclus dans les données téléchargées par le *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Page Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Mise à jour de la page Account/Manage/Index.cshtml

Mise à jour la `InputModel` dans *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* avec les éléments suivants en surbrillance le code :

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95)]

Mise à jour la *Areas/Identity/Pages/Account/Manage/Index.cshtml* avec le balisage mis en surbrillance suivant :

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Mise à jour de la page Account/Register.cshtml

Mise à jour la `InputModel` dans *Areas/Identity/Pages/Account/Register.cshtml.cs* avec les éléments suivants en surbrillance le code :

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Mise à jour la *Areas/Identity/Pages/Account/Register.cshtml* avec le balisage mis en surbrillance suivant :

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Générez le projet.

### <a name="add-a-migration-for-the-custom-user-data"></a>Ajouter une migration pour les données utilisateur personnalisées

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans Visual Studio **Package Manager Console**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Test de créer, afficher, télécharger, supprimer les données utilisateur personnalisées

Testez l’application :

* Inscrire un nouvel utilisateur.
* Afficher les données utilisateur personnalisées sur le `/Identity/Account/Manage` page.
* Télécharger et afficher les données personnelles les utilisateurs depuis la `/Identity/Account/Manage/PersonalData` page.