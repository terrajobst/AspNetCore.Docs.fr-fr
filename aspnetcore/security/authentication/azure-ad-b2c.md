---
title: Authentification Cloud avec Azure Active Directory B2C dans ASP.NET Core
author: camsoper
description: Découvrez comment configurer l’authentification Azure Active Directory B2C avec ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 136fa47788456492a9a7fe6d9d9e5996c13e8c20
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663659"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Authentification Cloud avec Azure Active Directory B2C dans ASP.NET Core

Auteur : [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure ad B2C) est une solution de gestion des identités dans le Cloud pour les applications Web et mobiles. Le service fournit une authentification pour les applications hébergées dans le cloud et sur site. Types d’authentification, les comptes individuels, les comptes de réseau social et fédérés comptes d’entreprise. En outre, Azure AD B2C pouvez fournir une authentification multifacteur avec une configuration minimale.

> [!TIP]
> Azure Active Directory (Azure AD) et Azure AD B2C sont des offres de produits distinctes. Un locataire Azure AD représente une organisation, alors qu’un locataire Azure AD B2C représente une collection d’identités qui doivent être utilisées avec les applications de confiance. Pour plus d’informations, consultez [Azure ad B2C : Forum aux questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Dans ce tutoriel, vous allez apprendre à effectuer les actions suivantes :

> [!div class="checklist"]
> * Créer un locataire Azure Active Directory B2C
> * Inscrire une application dans Azure AD B2C
> * Utiliser Visual Studio pour créer une application Web ASP.NET Core configurée pour utiliser le locataire Azure AD B2C pour l’authentification
> * Configurer des stratégies contrôlant le comportement du locataire Azure AD B2C

## <a name="prerequisites"></a>Conditions préalables requises

Les éléments suivants sont requis pour cette procédure pas à pas :

* [Abonnement Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Créer le client Azure Active Directory B2C

Créez un locataire Azure Active Directory B2C [comme décrit dans la documentation](/azure/active-directory-b2c/active-directory-b2c-get-started). Lorsque vous y êtes invité, associer le locataire à un abonnement Azure est facultative pour ce didacticiel.

## <a name="register-the-app-in-azure-ad-b2c"></a>Inscrire l’application dans Azure AD B2C

Dans le locataire nouvellement créé Azure AD B2C, inscrivez votre application en suivant [les étapes décrites dans la documentation de](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) la section **inscrire une application Web** . Arrêtez dans la section **créer une clé secrète client d’application Web** . Une clé secrète client n’est pas nécessaire pour ce didacticiel. 

Utilisez les valeurs suivantes :

| Paramètre                       | Valeur                     | Notes                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nom**                      | *nom de l’application &lt;&gt;*        | Entrez un **nom** pour l’application qui décrit votre application aux consommateurs.                                                                                                                                 |
| **Inclure une application/API web** | Oui                       |                                                                                                                                                                                                    |
| **Autoriser le flux implicite**       | Oui                       |                                                                                                                                                                                                    |
| **URL de réponse**                 | `https://localhost:44300/signin-oidc` | Les URL de réponse sont des points de terminaison auxquels Azure AD B2C retourne les jetons demandés par votre application. Visual Studio fournit l’URL de réponse à utiliser. Pour le moment, entrez `https://localhost:44300/signin-oidc` pour remplir le formulaire. |
| **URI ID d’application**                | Laisser vide               | Non requis pour ce didacticiel.                                                                                                                                                                    |
| **Inclure le client natif**     | Non                        |                                                                                                                                                                                                    |

> [!WARNING]
> Si vous configurez une URL de réponse non-localhost, tenez compte des [contraintes sur ce qui est autorisé dans la liste des URL de réponse](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application). 

Une fois l’application inscrite, la liste des applications du locataire s’affiche. Sélectionnez l’application qui vient d’être inscrite. Sélectionnez l’icône de **copie** à droite du champ **ID** de l’application pour la copier dans le presse-papiers.

Rien ne peut être configuré dans le locataire Azure AD B2C pour l’instant, mais laissez la fenêtre du navigateur ouverte. Une fois l’application ASP.NET Core créée, il y a plus de configuration.

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>Créer une application ASP.NET Core dans Visual Studio

Le modèle d’Application Web de Visual Studio peut être configuré pour utiliser le locataire Azure AD B2C pour l’authentification.

Dans Visual Studio :

1. Créez une application web ASP.NET Core. 
2. Dans la liste des modèles, sélectionnez **application Web** .
3. Sélectionnez le bouton **modifier l’authentification** .
    
    ![Bouton de l’authentification de modification](./azure-ad-b2c/_static/changeauth.png)

4. Dans la boîte de dialogue **modifier l’authentification** , sélectionnez **comptes d’utilisateur individuels**, puis sélectionnez **se connecter à un magasin d’utilisateurs existant dans le Cloud** dans la liste déroulante. 
    
    ![Boîte de dialogue Modifier l’authentification](./azure-ad-b2c/_static/changeauthdialog.png)

5. Remplissez le formulaire avec les valeurs suivantes :
    
    | Paramètre                       | Valeur                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nom de domaine**               | *&lt;le nom de domaine de votre locataire B2C&gt;*          |
    | **ID d’application**            | *&lt;coller l’ID d’application à partir du presse-papiers&gt;* |
    | **Chemin de rappel**             | *&lt;utiliser la valeur par défaut&gt;*                       |
    | **Stratégie d’inscription ou de connexion** | `B2C_1_SiUpIn`                                        |
    | **Réinitialiser la stratégie de mot de passe**     | `B2C_1_SSPR`                                          |
    | **Modifier la stratégie de profil**       | *&lt;laisser vide&gt;*                                 |
    
    Sélectionnez le lien **copier** en regard de **URI de réponse** pour copier l’URI de réponse dans le presse-papiers. Sélectionnez **OK** pour fermer la boîte de dialogue **modifier l’authentification** . Sélectionnez **OK** pour créer l’application Web.

## <a name="finish-the-b2c-app-registration"></a>Terminer l’inscription de l’application B2C

Revenez à la fenêtre du navigateur avec les propriétés de l’application B2C toujours ouverte. Modifiez l' **URL de réponse** temporaire spécifiée précédemment pour la valeur copiée à partir de Visual Studio. Sélectionnez **Enregistrer** en haut de la fenêtre.

> [!TIP]
> Si vous n’avez pas copié l’URL de réponse, utilisez l’adresse HTTPs à partir de l’onglet Déboguer dans les propriétés du projet Web, puis ajoutez la valeur **CallbackPath** à partir de *appSettings. JSON*.

## <a name="configure-policies"></a>Configurer des stratégies

Utilisez les étapes de la documentation de Azure AD B2C pour [créer une stratégie d’inscription ou de connexion](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), puis [créez une stratégie de réinitialisation de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions). Utilisez les exemples de valeurs fournis dans la documentation pour les **fournisseurs d’identité**, les **attributs d’inscription**et les revendications d' **application**. L’utilisation du bouton **Exécuter maintenant** pour tester les stratégies, comme décrit dans la documentation, est facultative.

> [!WARNING]
> Vérifiez que les noms de stratégie sont exactement comme décrit dans la documentation, car ces stratégies ont été utilisées dans la boîte de dialogue **modifier l’authentification** dans Visual Studio. Les noms de stratégie peuvent être vérifiés dans *appSettings. JSON*.

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a>Configurer les options OpenIdConnectOptions/JwtBearer/cookie sous-jacentes

Pour configurer les options sous-jacentes directement, utilisez la constante de schéma appropriée dans `Startup.ConfigureServices`:

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a>Exécuter l’application

Dans Visual Studio, appuyez sur **F5** pour générer et exécuter l’application. Après le lancement de l’application Web, sélectionnez **accepter** pour accepter l’utilisation des cookies (si vous y êtes invité), puis sélectionnez **se connecter**.

![Connectez-vous à l’application](./azure-ad-b2c/_static/signin.png)

Le navigateur redirige vers le locataire Azure AD B2C. Connectez-vous avec un compte existant (s’il a été créé en testant les stratégies) ou sélectionnez **Inscrivez-vous maintenant** pour créer un nouveau compte. Le lien **vous avez oublié votre mot de passe ?** est utilisé pour réinitialiser un mot de passe oublié.

![Connexion à Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Une fois la connexion établie, le navigateur redirige vers l’application Web.

![Succès](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un locataire Azure Active Directory B2C
> * Inscrire une application dans Azure AD B2C
> * Utiliser Visual Studio pour créer une application Web ASP.NET Core configurée pour utiliser le locataire Azure AD B2C pour l’authentification
> * Configurer des stratégies contrôlant le comportement du locataire Azure AD B2C

Maintenant que l’application ASP.NET Core est configurée pour utiliser Azure AD B2C pour l’authentification, vous pouvez utiliser l' [attribut Authorize](xref:security/authorization/simple) pour sécuriser votre application. Poursuivez le développement de votre application en apprenant à :

* [Personnaliser l’interface utilisateur Azure ad B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurer les exigences de complexité du mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Activez Multi-Factor Authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurez des fournisseurs d’identité supplémentaires, tels que [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), etc.
* [Utilisez le API Graph Azure ad](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) pour récupérer des informations utilisateur supplémentaires, telles que l’appartenance à un groupe, à partir du locataire Azure ad B2C.
* [Sécuriser une API web ASP.net core à l’aide de Azure ad B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).
* [Appeler une API Web .net à partir d’une application Web .net à l’aide de Azure ad B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).