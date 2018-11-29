---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 5b5384b0bfa933f40f82513b02f7a14367fbef76
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549087"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutoriel : Bien démarrer avec ASP.NET Core

Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer une application web ASP.NET Core. Vous allez apprendre à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Activer HTTPS localement.
> * Exécutez l’application.
> * Modifier une page Razor.

À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.

![Page d’accueil d’application web](_static/home-page.png)


## <a name="prerequisites"></a>Prérequis

* Installez le [!INCLUDE [](~/includes/2.1-SDK.md)].

## <a name="create-a-web-app-project"></a>Créer un projet application web

* Ouvrez un interpréteur de commandes, puis entrez la commande suivante :

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a>Activer HTTPS localement

* Approuvez le certificat de développement HTTPS :

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  La commande précédente affiche la boîte de dialogue suivante :

  ![Boîte de dialogue Avertissement de sécurité](_static/cert.png)

  Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  La commande précédente affiche le message suivant :

  *L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’a pas encore été approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
  * Cette commande peut vous demander votre mot de passe afin d’installer le certificat sur le trousseau système.
  
  Mot de passe :*

  Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.
   
---

## <a name="run-the-app"></a>Exécuter l'application

* Exécutez les commandes suivantes :

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* Accédez à [https://localhost:5001](https://localhost:5001). Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies. Cette application ne conserve pas les informations personnelles.

## <a name="edit-a-razor-page"></a>Modifier une page Razor

* Ouvrez *Pages/About.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* Accédez à [https://localhost:5001/About](https://localhost:5001/About) et vérifiez que les changements sont affichés.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Activer HTTPS localement.
> * Exécutez le projet.
> * Apportez la modification souhaitée.

Pour en savoir plus sur ASP.NET Core, consultez la présentation :

> [!div class="nextstepaction"]
> <xref:index>



> [!NOTE]
> Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.  Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).