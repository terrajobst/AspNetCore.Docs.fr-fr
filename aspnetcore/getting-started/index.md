---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Bref tutoriel qui crée et exécute une application Hello World de base à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 4f7e67e1e422afe3f7e2970e0c40380f065390ac
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928318"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutoriel : Bien démarrer avec ASP.NET Core

Ce didacticiel montre comment utiliser l’CLI .NET Core pour créer et exécuter une application Web ASP.NET Core.

Vous allez apprendre à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Approuver le certificat de développement.
> * Exécutez l’application.
> * Modifier une page Razor.

À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.

![Page d’accueil d’application web](_static/home-page.png)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a>Créer un projet application web

Ouvrez un interpréteur de commandes, puis entrez la commande suivante :

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

La commande précédente :

* Crée une application Web.  
* Le paramètre `-o aspnetcoreapp` crée un répertoire nommé *aspnetcoreapp* avec les fichiers sources de l’application.

### <a name="trust-the-development-certificate"></a>Approuver le certificat de développement

Approuvez le certificat de développement HTTPS :

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

La commande précédente affiche la boîte de dialogue suivante :

![Boîte de dialogue Avertissement de sécurité](~/getting-started/_static/cert.png)

Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

La commande précédente affiche le message suivant :

*L’approbation du certificat de développement https a été demandée. Si le certificat n’est pas déjà approuvé, nous allons exécuter la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

Cette commande risque de vous demander votre mot de passe afin d’installer le certificat sur le trousseau système. Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.

---

Pour plus d’informations, consultez [Approuver le certificat de développement ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Exécuter l'application

Exécutez les commandes suivantes :

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

Une fois que l’interface de commande indique que l’application a démarré, accédez à [https://localhost:5001](https://localhost:5001).

## <a name="edit-a-razor-page"></a>Modifier une page Razor

Ouvrez *pages/index. cshtml* et modifiez et enregistrez la page avec le balisage en surbrillance suivant :

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Accédez à [https://localhost:5001](https://localhost:5001), actualisez la page et vérifiez que les modifications sont affichées.

## <a name="next-steps"></a>Étapes suivantes :

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un projet application web.
> * Approuver le certificat de développement.
> * Exécuter le projet.
> * Apportez la modification souhaitée.

Pour en savoir plus sur ASP.NET Core, consultez le parcours d’apprentissage recommandé dans la présentation :

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
