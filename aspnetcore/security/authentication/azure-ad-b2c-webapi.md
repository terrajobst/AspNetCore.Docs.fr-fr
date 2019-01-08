---
title: Authentification dans web API avec Azure Active Directory B2C dans ASP.NET Core
author: camsoper
description: Découvrez comment configurer l’authentification Azure Active Directory B2C avec l’API Web ASP.NET Core. Tester l’API avec Postman de web authentifié.
ms.author: casoper
ms.date: 09/21/2018
ms.custom: mvc, seodec18
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 6d0365b103572d6059ce61c54b9b3406da9e5bd4
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098699"
---
# <a name="authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Authentification dans web API avec Azure Active Directory B2C dans ASP.NET Core

Auteur : [Cam Soper](https://twitter.com/camsoper)

[Azure B2C Active de répertoire](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) est une solution de gestion des identités de cloud pour les applications web et mobiles. Le service fournit une authentification pour les applications hébergées dans le cloud et sur site. Types d’authentification, les comptes individuels, les comptes de réseau social et fédérés comptes d’entreprise. Azure AD B2C fournit également l’authentification multifacteur avec une configuration minimale.

Azure Active Directory (Azure AD) et Azure AD B2C sont des offres de produits distinctes. Un locataire Azure AD représente une organisation, alors qu’un locataire Azure AD B2C représente une collection d’identités qui doivent être utilisées avec les applications de confiance. Pour plus d’informations, consultez [Azure AD B2C : Forum aux questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Dans la mesure où les API web ont pas d’interface utilisateur, ils ne peuvent pas rediriger l’utilisateur vers un service de jeton sécurisé comme Azure AD B2C. Au lieu de cela, l’API est passé un jeton du porteur à partir de l’application appelante, qui a déjà authentifié l’utilisateur avec Azure AD B2C. L’API valide ensuite le jeton sans intervention de l’utilisateur directement.

Dans ce didacticiel, découvrez comment :

> [!div class="checklist"]
> * Créer un locataire Azure Active Directory B2C.
> * Inscrire une API Web dans Azure AD B2C.
> * Utiliser Visual Studio pour créer une API Web configuré pour utiliser le locataire Azure AD B2C pour l’authentification.
> * Configurer des stratégies de contrôle du comportement du locataire Azure AD B2C.
> * Utiliser Postman pour simuler une application web qui présente une boîte de dialogue de connexion, récupère un jeton et l’utilise pour effectuer une demande sur l’API web.

## <a name="prerequisites"></a>Prérequis

Les éléments suivants sont requis pour cette procédure pas à pas :

* [Abonnement Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (toute édition)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Créer le client Azure Active Directory B2C

Créer un locataire Azure AD B2C [comme décrit dans la documentation](/azure/active-directory-b2c/active-directory-b2c-get-started). Lorsque vous y êtes invité, associer le locataire à un abonnement Azure est facultative pour ce didacticiel.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Configurer une stratégie d’inscription ou connexion

Utilisez les étapes de la documentation d’Azure AD B2C pour [créer une stratégie d’inscription ou connexion](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Nommez la stratégie **SiUpIn**.  Utilisez les exemples de valeurs fournies dans la documentation pour **fournisseurs d’identité**, **attributs d’inscription**, et **des revendications d’Application**. À l’aide de la **exécuter maintenant** bouton pour tester la stratégie, comme décrit dans la documentation est facultatif.

## <a name="register-the-api-in-azure-ad-b2c"></a>Inscrire l’API dans Azure AD B2C

Dans le locataire Azure AD B2C nouvellement créé, inscrire votre API à l’aide [les étapes décrites dans la documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) sous le **inscrire une API web** section.

Utilisez les valeurs suivantes :

| Paramètre                       | Value               | Notes                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Name**                      | *{Nom de l’API}*        | Entrez un **nom** pour l’application qui décrit votre application aux consommateurs.                     |
| **Inclure l’application web / API web** | Oui                 |                                                                                        |
| **Autoriser un flux implicite**       | Oui                 |                                                                                        |
| **URL de réponse**                 | `https://localhost` | URL de réponse sont des points de terminaison auxquels Azure AD B2C retourne les jetons demandés par votre application. |
| **URI ID d’application**                | *api*               | L’URI n’a pas besoin résoudre une adresse physique. Il doit être unique.     |
| **Inclure le client natif**     | Non                  |                                                                                        |

Une fois que l’API est inscrite, la liste des applications et des API dans le client s’affiche. Sélectionnez l’API qui a déjà été inscrit. Sélectionnez le **copie** icône à droite de la **ID d’Application** champ pour le copier dans le Presse-papiers. Sélectionnez **étendues publiées** et vérifiez la valeur par défaut *user_impersonation* étendue est présente.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Créer une application ASP.NET Core dans Visual Studio 2017

Le modèle d’Application Web de Visual Studio peut être configuré pour utiliser le locataire Azure AD B2C pour l’authentification.

Dans Visual Studio :

1. Créez une application web ASP.NET Core. 
2. Sélectionnez **API Web** à partir de la liste des modèles.
3. Sélectionnez le **modifier l’authentification** bouton.

    ![Bouton de l’authentification de modification](./azure-ad-b2c-webapi/change-auth-button.png)

4. Dans le **modifier l’authentification** boîte de dialogue, sélectionnez **comptes d’utilisateur individuels** > **se connecter à un magasin d’utilisateurs existant dans le cloud**.

    ![Boîte de dialogue Modifier l’authentification](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Remplissez le formulaire avec les valeurs suivantes :

    | Paramètre                       | Value                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nom de domaine**               | *{le nom de domaine de votre client B2C}*                |
    | **ID d’application**            | *{Collez l’ID d’Application à partir du Presse-papiers}*       |
    | **Stratégie d’inscription ou connexion** | B2C_1_SiUpIn                                          |

    Sélectionnez **OK** pour fermer la **modifier l’authentification** boîte de dialogue. Sélectionnez **OK** pour créer l’application web.

Visual Studio crée l’API web avec un contrôleur nommé *ValuesController.cs* qui retourne des valeurs codées en dur pour les requêtes GET. La classe est décorée avec le [attribut Authorize](xref:security/authorization/simple), de sorte que toutes les demandes nécessitent l’authentification.

## <a name="run-the-web-api"></a>Exécuter l’API web

Dans Visual Studio, exécutez l’API. Visual Studio lance un navigateur pointé URL racine de l’API. Notez l’URL dans la barre d’adresses et laissez l’API en cours d’exécution en arrière-plan.

> [!NOTE]
> Puisqu’il n’existe aucun contrôleur défini pour l’URL racine, le navigateur peut afficher alors une erreur 404 (page introuvable). Ce comportement est normal.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Utiliser Postman pour obtenir un jeton et tester l’API

[Postman](https://getpostman.com/postman) est un outil de test des API web. Pour ce didacticiel, Postman simule une application web qui accède à l’API web part de l’utilisateur.

### <a name="register-postman-as-a-web-app"></a>Inscrire Postman comme une application web

Étant donné que Postman simule une application web qui obtient des jetons à partir du locataire Azure AD B2C, elle doit être inscrite dans le client comme une application web. S’inscrire à l’aide de Postman [les étapes décrites dans la documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sous le **inscrire une application web** section. Arrêter à la **créer un secret de client d’application web** section. Une clé secrète client n’est pas nécessaire pour ce didacticiel. 

Utilisez les valeurs suivantes :

| Paramètre                       | Value                            | Notes                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Name**                      | Postman                          |                                 |
| **Inclure l’application web / API web** | Oui                              |                                 |
| **Autoriser un flux implicite**       | Oui                              |                                 |
| **URL de réponse**                 | `https://getpostman.com/postman` |                                 |
| **URI ID d’application**                | *{Laissez vide}*                  | Non requis pour ce didacticiel. |
| **Inclure le client natif**     | Non                               |                                 |

L’application web qui vient d’être inscrite a besoin d’autorisation pour accéder à l’API web part de l’utilisateur.  

1. Sélectionnez **Postman** dans la liste des applications, puis sélectionnez **accès à l’API** dans le menu de gauche.
1. Sélectionnez **+ ajouter**.
1. Dans le **sélectionner une API** liste déroulante, sélectionnez le nom de l’API web.
1. Dans le **sélectionner les étendues** liste déroulante, assurez-vous que toutes les étendues sont sélectionnées.
1. Sélectionnez **Ok**.

Notez l’ID d’Application de l’application Postman, est nécessaire pour obtenir un jeton du porteur.

### <a name="create-a-postman-request"></a>Créer une demande Postman

Lancez Postman. Par défaut, Postman affiche la **créer un nouveau** boîte de dialogue lors du lancement. Si la boîte de dialogue n’apparaît pas, sélectionnez le **+ nouveau** bouton dans le coin supérieur gauche.

À partir de la **créer un nouveau** boîte de dialogue :

1. Sélectionnez **demande**.

    ![Bouton de demande](./azure-ad-b2c-webapi/postman-create-new.png)

2. Entrez *obtenir les valeurs* dans le **nom de la demande** boîte.
3. Sélectionnez **+ créer une Collection** pour créer une nouvelle collection pour stocker la demande. Nommer la collection *didacticiels ASP.NET Core* , puis sélectionnez la coche.

    ![Créer une nouvelle collection](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Sélectionnez le **enregistrer dans les didacticiels ASP.NET Core** bouton.

### <a name="test-the-web-api-without-authentication"></a>Tester l’API web sans authentification

Pour vérifier que l’API web nécessite une authentification, tout d’abord effectuer une demande sans authentification.

1. Dans le **Entrez l’URL de la demande** , entrez l’URL pour `ValuesController`. L’URL est identique à celui affiché dans le navigateur avec **api/values** ajouté. Par exemple, `https://localhost:44375/api/values`.
2. Sélectionnez le **envoyer** bouton.
3. Notez l’état de la réponse est *401 non autorisé*.

    ![réponse 401 non autorisé](./azure-ad-b2c-webapi/postman-401-status.png)

> [!IMPORTANT]
> Si vous recevez une erreur « Pas pu obtenir de réponse », vous devrez peut-être désactiver la vérification du certificat SSL dans le [les paramètres de Postman](https://learning.getpostman.com/docs/postman/launching_postman/settings).

### <a name="obtain-a-bearer-token"></a>Obtenir un jeton du porteur

Pour faire une demande authentifiée à l’API web, un jeton du porteur est requis. Postman permet de facilement se connecter au locataire Azure AD B2C et obtenir un jeton.

1. Sur le **autorisation** sous l’onglet le **TYPE** liste déroulante, sélectionnez **OAuth 2.0**. Dans le **ajouter des données d’autorisation** liste déroulante, sélectionnez **en-têtes de requête**. Sélectionnez **accéder nouveau jeton**.

    ![Onglet d’autorisation avec les paramètres](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Terminer la **obtenir nouveau jeton d’accès** boîte de dialogue comme suit :


   |                Paramètre                 |                                             Value                                             |                                                                                                                                    Notes                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      <strong>Nom du jeton</strong>       |                                          *{nom du jeton}*                                       |                                                                                                                   Entrez un nom descriptif pour le jeton.                                                                                                                    |
   |      <strong>Type d’octroi</strong>       |                                           Implicite                                            |                                                                                                                                                                                                                                                                              |
   |     <strong>URL de rappel</strong>      |                                 `https://getpostman.com/postman`                              |                                                                                                                                                                                                                                                                              |
   |       <strong>URL d’authentification</strong>        | `https://login.microsoftonline.com/{tenant domain name}/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |  Remplacez *{nom de domaine client}* avec le nom de domaine du locataire. **IMPORTANT**: Cette URL doit avoir le même nom de domaine que ce qui se trouve dans `AzureAdB2C.Instance` dans le site web de l’API *appsettings.json* fichier. Voir la Remarque&dagger;.                                                  |
   |       <strong>ID de client</strong>       |                *{entrent dans l’application Postman <b>ID d’Application</b>}*                              |                                                                                                                                                                                                                                                                              |
   |         <strong>Portée</strong>         |         `https://{tenant domain name}/{api}/user_impersonation openid offline_access`       | Remplacez *{nom de domaine client}* avec le nom de domaine du locataire. Remplacez *{api}* avec l’URI ID d’application vous avez donné à l’API web quand vous avez inscrit (dans ce cas, `api`). Le modèle de l’URL est : `https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope name}`.         |
   |         <strong>État</strong>         |                                      *{Laissez vide}*                                          |                                                                                                                                                                                                                                                                              |
   | <strong>Authentification du client</strong> |                                Envoyer des informations d’identification du client dans le corps                                |                                                                                                                                                                                                                                                                              |

    > [!NOTE]
    > &dagger; La boîte de dialogue de paramètres de stratégie dans le portail Azure Active Directory B2C affiche deux URL possibles : L’autre dans le format `https://login.microsoftonline.com/`{nom de domaine client} / {informations supplémentaires du chemin} et l’autre dans le format `https://{tenant name}.b2clogin.com/`{nom de domaine client} / {informations de chemin d’accès supplémentaire}. Il a **critique** trouvant dans le domaine dans `AzureAdB2C.Instance` dans le site web de l’API *appsettings.json* fichier correspond à celui utilisé dans l’application web *appsettings.json* fichier. Il s’agit du même domaine que celui utilisé pour le champ URL d’authentification dans Postman. Notez que Visual Studio utilise un format d’URL légèrement différent à ce qui est affiché dans le portail. À condition que les domaines correspondent, l’URL fonctionne.

3. Sélectionnez le **demander un jeton** bouton.

4. Postman ouvre une nouvelle fenêtre contenant-boîte du locataire Azure AD B2C de dialogue connexion. Connectez-vous avec un compte existant (si un a été créé les stratégies de test) ou sélectionnez **s’inscrire maintenant** pour créer un nouveau compte. Le **votre mot de passe oublié ?** lien est utilisé pour réinitialiser un mot de passe oublié.

5. Après vous être connecté avec succès, la fenêtre se ferme et le **gérer les jetons d’accès** boîte de dialogue apparaît. Faites défiler jusqu'à la bas, puis sélectionnez le **utilisez jeton** bouton.

    ![Où trouver le bouton « Token usage »](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Tester l’API web avec l’authentification

Sélectionnez le **envoyer** bouton pour envoyer la demande à nouveau. Cette fois, l’état de réponse est *200 OK* et la charge utile JSON est visible sur la réponse **corps** onglet.

![État de réussite et la charge utile](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Créer un locataire Azure Active Directory B2C.
> * Inscrire une API Web dans Azure AD B2C.
> * Utiliser Visual Studio pour créer une API Web configuré pour utiliser le locataire Azure AD B2C pour l’authentification.
> * Configurer des stratégies de contrôle du comportement du locataire Azure AD B2C.
> * Utiliser Postman pour simuler une application web qui présente une boîte de dialogue de connexion, récupère un jeton et l’utilise pour effectuer une demande sur l’API web.

Continuer à développer votre API en apprenant à :

* [Sécuriser un ASP.NET Core application web à l’aide d’Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* [Appeler une API web .NET à partir d’une application web de .NET à l’aide d’Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Personnaliser l’interface utilisateur de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurer les exigences de complexité de mot de passe](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Activer l’authentification multifacteur](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurer les fournisseurs d’identité supplémentaires, telles que [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)et d’autres.
* [Utiliser l’API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) pour récupérer des informations supplémentaires, telles que l’appartenance au groupe, à partir du locataire Azure AD B2C.
