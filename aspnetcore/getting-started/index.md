---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228580"
---
# <a name="get-started-with-aspnet-core"></a>Bien démarrer avec ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Installez le [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Créez un projet ASP.NET Core. Ouvrez un interpréteur de commandes et entrez la commande suivante :

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. Approuvez le certificat de développement HTTPS :

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

   *L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’est pas déjà approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Cette commande risque de vous demander votre mot de passe pour installer le certificat sur le trousseau système.    Mot de passe :*

   Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a>Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS
---

4. Exécutez l’application :

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Accédez à [http://localhost:5001](http://localhost:5001).  Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies. Cette application ne conserve pas les informations personnelles.

6. Ouvrez *Pages/About.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Accédez à [http://localhost:5001/About](http://localhost:5001/About) et vérifiez que les changements sont affichés.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Installez le [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Créez un projet ASP.NET Core.

   Ouvrez un interpréteur de commandes. Entrez la commande suivante :

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Exécutez l’application avec les commandes suivantes :

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Accédez à [http://localhost:5000](http://localhost:5000).

5. Ouvrez *Pages/About.cshtml*, puis modifiez la page de façon à afficher le message « Hello, world! The time on the server is @DateTime.Now » :

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Accédez à [http://localhost:5000/About](http://localhost:5000/About) et vérifiez les changements.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Installez le **programme d’installation du SDK** .NET Core pour le SDK 1.0.4 à partir de la [page de tous les téléchargements .NET Core](https://www.microsoft.com/net/download/all).

2. Créez un dossier pour un nouveau projet ASP.NET Core.

   Ouvrez un interpréteur de commandes. Entrez les commandes suivantes :

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Si vous avez installé une version ultérieure du SDK sur votre ordinateur, créez un fichier *global.json* pour sélectionner le SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Créez un projet ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Restaurez les packages.

    ```console
    dotnet restore
    ```

6. Exécutez l’application.

   ```console
   dotnet run
   ```

   La commande [dotnet run](/dotnet/core/tools/dotnet-run) commence par créer l’application, si nécessaire.

7. Accédez à `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
