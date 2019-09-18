---
title: Introduction à Identity sur ASP.NET Core
author: rick-anderson
description: Utiliser Identity à une application ASP.NET Core Découvrez comment définir les exigences de mot de passe (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 979681cfc196aca9fb5097583d99a086e1c597ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082451"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introduction à Identity sur ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core identité est un système d’appartenance qui ajoute des fonctionnalités de connexion aux applications ASP.NET Core. Les utilisateurs peuvent créer un compte avec les informations de connexion stockées dans l’identité, ou ils peuvent utiliser un fournisseur de connexion externe. Les fournisseurs de connexion externes pris en charge incluent [Facebook, Google, Microsoft Account et Twitter](xref:security/authentication/social/index).

L’identité peut être configurée à l’aide d’une base de données SQL Server pour stocker les noms d’utilisateur, les mots de passe et les données de profil. Vous pouvez également utiliser un autre magasin persistant, par exemple, le stockage table Azure.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([procédure de téléchargement)](xref:index#how-to-download-a-sample).

Dans cette rubrique, vous allez apprendre à utiliser l’identité pour vous inscrire, vous connecter et déconnecter un utilisateur. Pour obtenir des instructions plus détaillées sur la création d’applications qui utilisent l’identité, consultez la section étapes suivantes à la fin de cet article.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity et AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) a été introduite dans ASP.net Core 2,1. L' `AddDefaultIdentity` appel de est similaire à l’appel de ce qui suit :

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Pour plus d’informations, consultez [source AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Créer une application Web avec l’authentification

Créez un projet d’application Web ASP.NET Core avec des comptes d’utilisateur individuels.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Sélectionnez **Fichier** > **Nouveau** > **Projet**.
* Sélectionnez **Nouvelle application web ASP.NET Core**. Nommez le projet **application Web 1** pour avoir le même espace de noms que le projet à télécharger. Cliquez sur **OK**.
* Sélectionnez une **application Web**ASP.net Core, puis sélectionnez **modifier l’authentification**.
* Sélectionnez **comptes d’utilisateur individuels** , puis cliquez sur **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

Le projet généré fournit une [identité de ASP.net Core](xref:security/authentication/identity) sous forme de bibliothèque de [classes Razor](xref:razor-pages/ui-class). La bibliothèque de classes Razor d’identité expose des points `Identity` de terminaison avec la zone. Par exemple :

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>Appliquer des migrations

Appliquez les migrations pour initialiser la base de données.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Exécutez la commande suivante dans la console du gestionnaire de package (PMC) :

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>Registre de test et connexion

Exécutez l’application et inscrivez un utilisateur. Selon la taille de votre écran, vous devrez peut-être sélectionner le bouton bascule de navigation pour afficher les liens de **connexion** et de **Registre** .

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>Configurer les services d’identité

Les services sont ajoutés `ConfigureServices`dans. Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

Le code précédent configure l’identité avec les valeurs d’option par défaut. Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).

   L’identité est activée en appelant [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Les services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).

   Identity est activée pour l’application en appelant `UseAuthentication` dans le `Configure` (méthode). `UseAuthentication`Ajoute l’authentification [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Ces services sont mis à la disposition de l’application via l' [injection de dépendances](xref:fundamentals/dependency-injection).

   Identity est activée pour l’application en appelant `UseIdentity` dans le `Configure` (méthode). `UseIdentity`Ajoute l’authentification par cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) au pipeline de demande.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Pour plus d’informations, consultez la [classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) et le démarrage de l' [application](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Registre de génération de modèles, connexion et déconnexion

Suivez l' [identité de l’échafaudage dans un projet Razor avec des instructions d’autorisation](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) pour générer le code présenté dans cette section.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ajoutez les fichiers Register, login et LogOut.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Si vous avez créé le projet avec le nom **application Web 1**, exécutez les commandes suivantes. Sinon, utilisez l’espace de noms correct `ApplicationDbContext`pour :

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell utilise un point-virgule comme séparateur de commande. Quand vous utilisez PowerShell, échappez les points-virgules dans la liste de fichiers ou placez la liste de fichiers entre guillemets doubles, comme le montre l’exemple précédent.

---

### <a name="examine-register"></a>Examiner le registre

::: moniker range=">= aspnetcore-2.1"

   Quand un utilisateur clique sur le lien register `RegisterModel.OnPostAsync` , l’action est appelée. L’utilisateur est créé par [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sur l' `_userManager` objet. `_userManager`est fourni par l’injection de dépendances) :

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Quand un utilisateur clique sur le lien register `Register` , l’action est appelée `AccountController`sur. L' `Register` action crée l’utilisateur en appelant `CreateAsync` sur l' `_userManager` objet (fourni à `AccountController` par l’injection de dépendances) :

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Si l’utilisateur a été créé avec succès, l’utilisateur est connecté par l’appel `_signInManager.SignInAsync`à.

   **Remarque :** Consultez [confirmation du compte](xref:security/authentication/accconfirm#prevent-login-at-registration) pour connaître les étapes permettant d’empêcher la connexion immédiate lors de l’inscription.

### <a name="log-in"></a>Se connecter

::: moniker range=">= aspnetcore-2.1"

Le formulaire de connexion s’affiche lorsque :

* Le lien **connexion** est sélectionné.
* Un utilisateur tente d’accéder à une page restreinte à laquelle il n’est pas autorisé à accéder **ou** lorsqu’il n’a pas été authentifié par le système.

Lorsque le formulaire de la page de connexion est envoyé, `OnPostAsync` l’action est appelée. `PasswordSignInAsync`est appelé sur l' `_signInManager` objet (fourni par l’injection de dépendances).

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   La classe `Controller` de base expose `User` une propriété à laquelle vous pouvez accéder à partir des méthodes de contrôleur. Par exemple, vous pouvez énumérer `User.Claims` et prendre des décisions d’autorisation. Pour plus d'informations, consultez <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Le formulaire de connexion s’affiche lorsque les utilisateurs sélectionnent le lien **de** connexion ou sont redirigés lors de l’accès à une page qui requiert une authentification. Lorsque l’utilisateur envoie le formulaire sur la page de connexion, l' `AccountController` `Login` action est appelée.

L' `Login` action appelle `PasswordSignInAsync` sur l' `_signInManager` objet (fourni à `AccountController` par l’injection de dépendances).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

La classe de`Controller` base `PageModel`(ou) expose `User` une propriété. Par exemple, `User.Claims` peut être énuméré pour prendre des décisions d’autorisation.

::: moniker-end

### <a name="log-out"></a>Se déconnecter

::: moniker range=">= aspnetcore-2.1"

Le lien de **déconnexion** appelle l' `LogoutModel.OnPost` action. 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) efface les revendications de l’utilisateur stockées dans un cookie.

La publication est spécifiée dans *pages/Shared/_LoginPartial. cshtml*:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Cliquer sur le lien **déconnexion** appelle `LogOut` l’action.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Le code précédent appelle la `_signInManager.SignOutAsync` méthode. La `SignOutAsync` méthode efface les revendications de l’utilisateur stockées dans un cookie.

::: moniker-end

## <a name="test-identity"></a>Identité du test

Les modèles de projet Web par défaut autorisent l’accès anonyme aux pages d’hébergement. Pour tester l’identité, [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) ajoutez à la page confidentialité.

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

Si vous êtes connecté, déconnectez-vous. Exécutez l’application et sélectionnez le lien **confidentialité** . Vous êtes redirigé vers la page de connexion.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Explorer l’identité

Pour explorer l’identité plus en détail :

* [Créer une source d’interface utilisateur d’identité complète](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Examinez la source de chaque page et parcourez le débogueur.

::: moniker-end

## <a name="identity-components"></a>Composants d'Identity

::: moniker range=">= aspnetcore-2.1"

Tous les packages NuGet dépendants de l’identité sont inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

::: moniker-end

Le package principal pour l’identité est [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Ce package contient l’ensemble principal d’interfaces pour ASP.NET Core Identity et est inclus par `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Migration vers ASP.NET Core Identity

Pour plus d’informations et de conseils sur la migration de votre magasin d’identités existant, consultez [migrer l’authentification et l’identité](xref:migration/identity).

## <a name="setting-password-strength"></a>Définition de la force du mot de passe

Consultez [configuration](#pw) d’un exemple qui définit la configuration minimale requise pour le mot de passe.

## <a name="next-steps"></a>Étapes suivantes

* [Configurer Identity](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
