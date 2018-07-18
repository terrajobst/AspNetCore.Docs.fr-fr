---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2018
ms.locfileid: "38216211"
---
# <a name="get-started-with-aspnet-core"></a>Bien démarrer avec ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Installez le [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Créez un projet ASP.NET Core. Ouvrez un interpréteur de commandes et entrez la commande suivante :

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]

3. Approuvez le certificat de développement HTTPS :

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
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
