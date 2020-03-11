---
title: Ajouter, télécharger et supprimer des données utilisateur à l’identité dans un projet ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des données utilisateur personnalisées à l’identité dans un projet ASP.NET Core. Supprimer des données par RGPD.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 7a67f55da0e685ed3fd5badb30e8be683411a5ae
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662686"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet article montre comment :

* Ajouter des données utilisateur personnalisées pour une application web ASP.NET Core.
* Marquez le modèle de données utilisateur personnalisé avec l’attribut <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> pour qu’il soit automatiquement disponible au téléchargement et à la suppression. La possibilité de télécharger et de supprimer les données vous aide à répondre aux exigences de [RGPD](xref:security/gdpr) .

L’exemple de projet est créé à partir d’une application web de Pages Razor, mais les instructions sont similaires pour une application web ASP.NET Core MVC.

[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Conditions préalables requises

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**. Nommez le projet **application Web 1** si vous souhaitez qu’il corresponde à l’espace de noms de l’exemple de code de [Téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
* Sélectionnez **ASP.net Core application Web** > **OK**
* Sélectionnez **ASP.NET Core 3,0** dans la liste déroulante
* Sélectionnez **application Web** > **OK**
* Générez et exécutez le projet.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**. Nommez le projet **application Web 1** si vous souhaitez qu’il corresponde à l’espace de noms de l’exemple de code de [Téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
* Sélectionnez **ASP.net Core application Web** > **OK**
* Sélectionnez **ASP.NET Core 2,2** dans la liste déroulante
* Sélectionnez **application Web** > **OK**
* Générez et exécutez le projet.

::: moniker-end


# <a name="net-core-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Exécuter le Générateur de modèles automatique identité

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet > **Ajouter** > **nouvel élément de génération de modèles**automatique.
* Dans le volet gauche de la boîte de dialogue **Ajouter une structure** , sélectionnez **identité** > **Ajouter**.
* Dans la boîte de dialogue **Ajouter une identité** , les options suivantes sont disponibles :
  * Sélectionner le fichier de disposition existant *~/Pages/Shared/_Layout. cshtml*
  * Sélectionnez les fichiers suivants pour remplacer :
    * **Compte/inscription**
    * **Compte/gestion/index**
  * Sélectionnez le bouton **+** pour créer une **classe de contexte de données**. Acceptez le type (**application Web 1. Models. WebApp1Context** si le projet est nommé **application Web 1**).
  * Sélectionnez le bouton **+** pour créer une nouvelle **classe d’utilisateur**. Acceptez le type (**WebApp1User** si le projet est nommé **application Web 1**) > **Ajouter**.
* Sélectionnez **Ajouter**.

# <a name="net-core-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Ajoutez une référence de package à [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) dans le fichier projet (. csproj). Dans le répertoire du projet, exécutez la commande suivante :

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Dans le dossier du projet, exécutez le Générateur de modèles automatique identité :

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Suivez les instructions dans [migrations, UseAuthentication et Layout](xref:security/authentication/scaffold-identity#efm) pour effectuer les étapes suivantes :

* Créer une migration et mettre à jour de la base de données.
* Ajout de `UseAuthentication` à `Startup.Configure`.
* Ajoutez `<partial name="_LoginPartial" />` au fichier de disposition.
* Testez l’application :
  * Inscrire un utilisateur
  * Sélectionnez le nouveau nom d’utilisateur (à côté du lien de **déconnexion** ). Vous devrez peut-être agrandir la fenêtre ou sélectionnez l’icône de barre de navigation pour afficher le nom d’utilisateur et d’autres liens.
  * Sélectionnez l’onglet **données personnelles** .
  * Sélectionnez le bouton **Télécharger** et examinez le fichier *PersonalData. JSON* .
  * Testez le bouton **supprimer** , qui supprime l’utilisateur connecté.

## <a name="add-custom-user-data-to-the-identity-db"></a>Ajouter des données utilisateur personnalisées pour la base de données d’identité

Mettez à jour la classe dérivée `IdentityUser` avec des propriétés personnalisées. Si vous avez nommé le projet application Web 1, le fichier est nommé *Areas/Identity/Data/WebApp1User. cs*. Mettre à jour le fichier avec le code suivant :

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Les propriétés avec l’attribut [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) sont les suivantes :

* Supprimé lorsque la page Razor *Areas/Identity/pages/Account/Manage/DeletePersonalData. cshtml* appelle `UserManager.Delete`.
* Inclus dans les données téléchargées par la page Razor *Areas/Identity/pages/Account/Manage/DownloadPersonalData. cshtml* .

### <a name="update-the-accountmanageindexcshtml-page"></a>Mettre à jour de la page Account/Manage/Index.cshtml

Mettez à jour le `InputModel` dans *Areas/Identity/pages/Account/Manage/index. cshtml. cs* avec le code en surbrillance suivant :

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Mettez à jour les *zones/Identity/pages/Account/Manage/index. cshtml* avec le balisage en surbrillance suivant :

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Mettez à jour les *zones/Identity/pages/Account/Manage/index. cshtml* avec le balisage en surbrillance suivant :

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Mettre à jour de la page Account/Register.cshtml en

Mettez à jour le `InputModel` dans *Areas/Identity/pages/Account/Register. cshtml. cs* avec le code en surbrillance suivant :

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Mettez à jour les *zones/Identity/pages/Account/Register. cshtml* avec le balisage en surbrillance suivant :

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Mettez à jour les *zones/Identity/pages/Account/Register. cshtml* avec le balisage en surbrillance suivant :

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Créez le projet.

### <a name="add-a-migration-for-the-custom-user-data"></a>Ajoutez une migration pour les données utilisateur personnalisées

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans la console du **Gestionnaire de package**Visual Studio :

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Test de créer, afficher, télécharger, supprimer les données utilisateur personnalisées

Testez l’application :

* Inscrire un nouvel utilisateur.
* Affichez les données utilisateur personnalisées sur la page `/Identity/Account/Manage`.
* Téléchargez et affichez les données personnelles des utilisateurs à partir de la page `/Identity/Account/Manage/PersonalData`.
