---
title: Authentification cloud avec Azure Active Directory B2C dans ASP.NET Core
author: camsoper
description: Découvrez comment configurer l’authentification Azure Active Directory B2C avec ASP.NET Core.
ms.date: 01/25/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 731b25cac6f0d56fd34d12114a73e5cb5265dda6
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202638"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Authentification cloud avec Azure Active Directory B2C dans ASP.NET Core

Auteur : [Cam Soper](https://twitter.com/camsoper)

[Azure B2C Active de répertoire](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) est une solution de gestion des identités de cloud pour les applications web et mobiles. Le service fournit une authentification pour les applications hébergées dans le cloud et sur site. Types d’authentification, les comptes individuels, les comptes de réseau social et fédérés comptes d’entreprise. En outre, Azure AD B2C peut fournir une authentification multifacteur avec une configuration minimale.

> [!TIP]
> Azure Active Directory (Azure AD) et Azure AD B2C sont des offres de produits distinctes. Un locataire Azure AD représente une organisation, alors qu’un locataire Azure AD B2C représente une collection d’identités qui doivent être utilisées avec les applications de confiance. Pour plus d’informations, consultez [Azure AD B2C : Forum aux questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Dans ce didacticiel, découvrez comment :

> [!div class="checklist"]
> * Créer un locataire Azure Active Directory B2C
> * Inscrire une application dans Azure AD B2C
> * Utiliser Visual Studio pour créer une application web de ASP.NET Core configurée pour utiliser le locataire Azure AD B2C pour l’authentification
> * Configurer des stratégies de contrôle du comportement du locataire Azure AD B2C

## <a name="prerequisites"></a>Prérequis

Les éléments suivants sont requis pour cette procédure pas à pas :

* [Abonnement Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (toute édition)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Créer le client Azure Active Directory B2C

Créer un locataire Azure Active Directory B2C [comme décrit dans la documentation](/azure/active-directory-b2c/active-directory-b2c-get-started). Lorsque vous y êtes invité, associer le locataire à un abonnement Azure est facultative pour ce didacticiel.

## <a name="register-the-app-in-azure-ad-b2c"></a>Inscrire l’application dans Azure AD B2C

Dans le locataire Azure AD B2C nouvellement créé, inscrivez votre application en utilisant [les étapes décrites dans la documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sous le **inscrire une application web** section. Arrêter à la **créer un secret de client d’application web** section. Une clé secrète client n’est pas nécessaire pour ce didacticiel. 

Utilisez les valeurs suivantes :

| Paramètre                       | Value                     | Notes                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Nom de l’application&gt;*        | Entrez un **nom** pour l’application qui décrit votre application aux consommateurs.                                                                                                                                 |
| **Inclure l’application web / API web** | Oui                       |                                                                                                                                                                                                    |
| **Autoriser un flux implicite**       | Oui                       |                                                                                                                                                                                                    |
| **URL de réponse**                 | `https://localhost:44300/signin-oidc` | URL de réponse sont des points de terminaison auxquels Azure AD B2C retourne les jetons demandés par votre application. Visual Studio fournit l’URL de réponse à utiliser. Pour l’instant, entrez `https://localhost:44300/signin-oidc` pour remplir le formulaire. |
| **URI ID d’application**                | Laisser vide               | Non requis pour ce didacticiel.                                                                                                                                                                    |
| **Inclure le client natif**     | Non                        |                                                                                                                                                                                                    |

> [!WARNING]
> Si la configuration d’une URL de réponse non localhost, vous devez connaître le [contraintes sur ce qui est autorisé dans la liste des URL de réponse](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Une fois que l’application est inscrite, la liste des applications dans le client s’affiche. Sélectionnez l’application que vous venez d’inscrire. Sélectionnez le **copie** icône à droite de la **ID d’Application** champ pour le copier dans le Presse-papiers.

Rien n’est plus peuvent être configuré dans le locataire Azure AD B2C pour l’instant, mais laissant la fenêtre du navigateur ouverte. Il existe davantage de configuration après la création de l’application ASP.NET Core.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Créer une application ASP.NET Core dans Visual Studio 2017

Le modèle d’Application Web de Visual Studio peut être configuré pour utiliser le locataire Azure AD B2C pour l’authentification.

Dans Visual Studio :

1. Créez une application web ASP.NET Core. 
2. Sélectionnez **Web Application** à partir de la liste des modèles.
3. Sélectionnez le **modifier l’authentification** bouton.
    
    ![Bouton de l’authentification de modification](./azure-ad-b2c/_static/changeauth.png)

4. Dans le **modifier l’authentification** boîte de dialogue, sélectionnez **comptes d’utilisateur individuels**, puis sélectionnez **se connecter à un magasin d’utilisateurs existant dans le cloud** dans la liste déroulante. 
    
    ![Boîte de dialogue Modifier l’authentification](./azure-ad-b2c/_static/changeauthdialog.png)

5. Remplissez le formulaire avec les valeurs suivantes :
    
    | Paramètre                       | Value                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nom de domaine**               | *&lt;le nom de domaine de votre client B2C&gt;*          |
    | **ID d’application**            | *&lt;Collez l’ID d’Application à partir du Presse-papiers&gt;* |
    | **Chemin d’accès de rappel**             | *&lt;Utilisez la valeur par défaut&gt;*                       |
    | **Stratégie d’inscription ou connexion** | `B2C_1_SiUpIn`                                        |
    | **Stratégie de réinitialisation de mot de passe**     | `B2C_1_SSPR`                                          |
    | **Modifier la stratégie de profil**       | *&lt;Laisser vide&gt;*                                 |
    
    Sélectionnez le **copie** situé en regard **URI de réponse** pour copier l’URI de réponse dans le Presse-papiers. Sélectionnez **OK** pour fermer la **modifier l’authentification** boîte de dialogue. Sélectionnez **OK** pour créer l’application web.

## <a name="finish-the-b2c-app-registration"></a>Terminer l’inscription des applications B2C

Revenez à la fenêtre de navigateur avec les propriétés de l’application B2C toujours ouvertes. Modifier la fichier temporaire **URL de réponse** spécifié précédemment pour la valeur copiée à partir de Visual Studio. Sélectionnez **enregistrer** en haut de la fenêtre.

> [!TIP]
> Si vous n’avez pas de copier l’URL de réponse, utilisez l’adresse SSL à partir de l’onglet débogage dans les propriétés du projet web et ajouter la **CallbackPath** valeur *appsettings.json*.

## <a name="configure-policies"></a>Configurer des stratégies

Utilisez les étapes de la documentation d’Azure AD B2C pour [créer une stratégie d’inscription ou connexion](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), puis [créer une stratégie de réinitialisation de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Utilisez les exemples de valeurs fournies dans la documentation pour **fournisseurs d’identité**, **attributs d’inscription**, et **des revendications d’Application**. À l’aide de la **exécuter maintenant** bouton pour tester les stratégies, comme décrit dans la documentation est facultatif.

> [!WARNING]
> Vérifiez les noms de stratégie sont exactement comme décrit dans la documentation, car ces stratégies ont été utilisées dans le **modifier l’authentification** boîte de dialogue dans Visual Studio. Les noms de stratégie peuvent être vérifiés dans *appsettings.json*.

## <a name="run-the-app"></a>Exécuter l'application

Dans Visual Studio, appuyez sur **F5** pour générer et exécuter l’application. Une fois l’application web démarre, sélectionnez **connectez-vous**.

![Connectez-vous à l’application](./azure-ad-b2c/_static/signin.png)

Le navigateur le redirige vers le locataire Azure AD B2C. Connectez-vous avec un compte existant (si un a été créé les stratégies de test) ou sélectionnez **s’inscrire maintenant** pour créer un nouveau compte. Le **votre mot de passe oublié ?** lien est utilisé pour réinitialiser un mot de passe oublié.

![Connexion à Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Après vous être connecté avec succès, le navigateur le redirige vers l’application web.

![Opération réussie](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un locataire Azure Active Directory B2C
> * Inscrire une application dans Azure AD B2C
> * Utiliser Visual Studio pour créer une Application ASP.NET Core Web, configuré pour utiliser le locataire Azure AD B2C pour l’authentification
> * Configurer des stratégies de contrôle du comportement du locataire Azure AD B2C

Maintenant que l’application ASP.NET Core est configurée pour utiliser Azure AD B2C pour l’authentification, le [attribut Authorize](xref:security/authorization/simple) peut être utilisé pour sécuriser votre application. Continuer à développer votre application en apprenant à :

* [Personnaliser l’interface utilisateur de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurer les exigences de complexité de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Activer l’authentification multifacteur](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurer les fournisseurs d’identité supplémentaires, telles que [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)et d’autres.
* [Utiliser l’API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) pour récupérer des informations supplémentaires, telles que l’appartenance au groupe, à partir du locataire Azure AD B2C.
* [Sécuriser un ASP.NET Core API web à l’aide d’Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).
* [Appeler une API web .NET à partir d’une application web de .NET à l’aide d’Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
