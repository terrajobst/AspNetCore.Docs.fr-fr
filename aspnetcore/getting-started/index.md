---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: dc85fe9189c93476859a6c00d60ec24d6eeec3fa
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335310"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutoriel : Bien démarrer avec ASP.NET Core

Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer une application web ASP.NET Core.

Vous allez apprendre à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Activer HTTPS localement.
> * Exécutez l’application.
> * Modifier une page Razor.

À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.

![Page d’accueil d’application web](_static/home-page.png)

## <a name="prerequisites"></a>Prérequis

* [SDK .NET Core 2.2](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Créer un projet application web

Ouvrez un interpréteur de commandes, puis entrez la commande suivante :

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a>Activer HTTPS localement

Approuvez le certificat de développement HTTPS :

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

Exécutez les commandes suivantes :

```console
cd aspnetcoreapp
dotnet run
```

Accédez à [https://localhost:5001](https://localhost:5001). Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies. Cette application ne conserve pas les informations personnelles.

## <a name="edit-a-razor-page"></a>Modifier une page Razor

Ouvrez *Pages/Index.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Accédez à [https://localhost:5001](https://localhost:5001) et vérifiez que les changements sont affichés.

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
