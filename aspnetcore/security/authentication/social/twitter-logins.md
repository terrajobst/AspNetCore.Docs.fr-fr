---
title: Programme d’installation de la connexion externe Twitter avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708424"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Programme d’installation de la connexion externe Twitter avec ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel vous montre comment autoriser vos utilisateurs à [vous connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Créer l’application dans Twitter

* Accédez à [ https://apps.twitter.com/ ](https://apps.twitter.com/) et s’y connecter. Si vous ne disposez pas d’un compte Twitter, utilisez le **[s’inscrire maintenant](https://twitter.com/signup)** lien pour en créer un. Après vous être connecté, le **gestion des applications** page est affichée :

  ![Gestion des applications Twitter ouvert dans Microsoft Edge](index/_static/TwitterAppManage.png)

* Appuyez sur **créer une application** et remplissez la demande **nom**, **Description** et publics **site Web** URI (Cela peut être temporaire jusqu'à ce que vous Inscrivez le nom de domaine) :

  ![Créer une page d’application](index/_static/TwitterCreate.png)

* Entrez votre développement URI avec `/signin-twitter` ajoutées dans le **URI de redirection OAuth valides** champ (par exemple : `https://localhost:44320/signin-twitter`). Le schéma d’authentification Twitter configuré plus loin dans ce didacticiel gère automatiquement les demandes à `/signin-twitter` itinéraire pour implémenter le flux OAuth.

  > [!NOTE]
  > Le segment d’URI `/signin-twitter` est défini en tant que le rappel par défaut du fournisseur d’authentification Twitter. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification de Twitter par le biais de l’élément hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.

* Remplissez le reste du formulaire, puis appuyez sur **créer votre application Twitter**. Détails de l’application sont affichent :

  ![Onglet Détails sur la page d’Application](index/_static/TwitterAppDetails.png)

* Lorsque vous déployez le site, vous devez revoir la **gestion des applications** page et inscrire un nouvel URI public.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Stockage Twitter ConsumerKey et ConsumerSecret

Lier des paramètres sensibles comme Twitter `Consumer Key` et `Consumer Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets). Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`.

Vous trouverez ces jetons sur le **Keys and Access Tokens** onglet après avoir créé votre nouvelle application Twitter :

![Onglet clés et jetons d’accès](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Configurer l’authentification Twitter

Le modèle de projet utilisé dans ce didacticiel s’assure que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package est déjà installé.

* Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.
* Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Ajoutez le service Twitter dans le `ConfigureServices` méthode dans *Startup.cs* fichier :

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ajoutez l’intergiciel (middleware) Twitter dans le `Configure` méthode dans *Startup.cs* fichier :

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

Consultez le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter. Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-twitter"></a>Connectez-vous à Twitter

Exécutez votre application et cliquez sur **connectez-vous**. Une option pour vous connecter avec Twitter s’affiche :

![Application Web : utilisateur non authentifié](index/_static/DoneTwitter.png)

En cliquant sur **Twitter** redirige vers Twitter pour l’authentification :

![Page d’authentification Twitter](index/_static/TwitterLogin.png)

Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :

![Application Web : utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* **ASP.NET Core 2.x uniquement :** si identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : l’option 'SignInScheme' doit être fournie*. Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Twitter. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `ConsumerSecret` dans le portail des développeurs Twitter.

* Définir le `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
