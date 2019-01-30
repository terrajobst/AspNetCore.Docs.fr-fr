---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Ajout d’ASP.NET Identity à un site Web vide ou existant constitue le projet | Microsoft Docs
author: raquelsa
description: Ce didacticiel vous montre comment ajouter ASP.NET Identity (le nouveau système d’appartenance pour ASP.NET) à une application ASP.NET. Lorsque vous créez un nouveau Web Forms ou MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: cd28cc68db96b52eb205b8764aa2af014ffad9c3
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236508"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Ajout d’ASP.NET Identity à un site Web vide ou existant constitue le projet


> Ce didacticiel vous montre comment ajouter [ASP.NET Identity](introduction-to-aspnet-identity.md) (le nouveau système d’appartenance pour ASP.NET) à une application ASP.NET.
> 
> Lorsque vous créez un nouveau projet Web Forms ou MVC dans Visual Studio 2017 RTM avec des comptes individuels, Visual Studio installera tous les packages requis et ajouter toutes les classes nécessaires pour vous. Ce didacticiel illustre les étapes pour ajouter la prise en charge ASP.NET Identity à votre projet Web Forms existant ou un nouveau projet vide. J’ai décrit tous les packages NuGet que vous devez installer et vous devez ajouter des classes. Je passe en revue les exemples de formulaires Web pour l’inscription de nouveaux utilisateurs et la journalisation lors de la mise en surbrillance toutes les API de point d’entrée principal pour l’authentification et de gestion des utilisateurs. Cet exemple utilise l’implémentation par défaut de ASP.NET Identity pour le stockage de données SQL qui est basé sur Entity Framework. Ce didacticiel, nous allons utiliser la base de données locale pour la base de données SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Bien démarrer avec ASP.NET Identity

1. Démarrez en installant et en cours d’exécution [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Sélectionnez **nouveau projet** à partir du début page, ou vous pouvez utiliser le menu et sélectionnez **fichier**, puis **nouveau projet**.
3. Dans le volet gauche, développez **Visual C#** , puis sélectionnez **Web**, puis **Application Web ASP.NET (.Net Framework)**. Nommez votre projet « WebFormsIdentity » et sélectionnez **OK**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Notez que le **modifier l’authentification** bouton est désactivé et aucune prise en charge l’authentification n’est fourni dans ce modèle. Les modèles Web Forms, MVC et API Web permettent de sélectionner l’approche de l’authentification.

## <a name="add-identity-packages-to-your-app"></a>Ajouter des packages de l’identité à votre application

Dans l’Explorateur de solutions, cliquez sur votre projet et sélectionnez **gérer les Packages NuGet**. Recherchez et installez le **Microsoft.AspNet.Identity.EntityFramework** package. 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Notez que ce package installe les packages de dépendance : **EntityFramework** et **identité Microsoft ASP.NET Core**.

## <a name="add-a-web-form-to-register-users"></a>Ajoutez un formulaire web pour inscrire des utilisateurs

1. Dans **l’Explorateur de solutions**, cliquez sur votre projet et sélectionnez **ajouter**, puis **Web Form**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. Dans le **spécifier le nom de l’élément** boîte de dialogue, le nom du nouveau formulaire web **inscrire**, puis sélectionnez **OK**
3. Remplacez le balisage dans le texte généré *Register.aspx* fichier par le code ci-dessous. Les modifications du code apparaissent en surbrillance. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Il s’agit simplement d’une version simplifiée de la *Register.aspx* fichier est créé lorsque vous créez un nouveau projet ASP.NET Web Forms. Le balisage ci-dessus ajoute des champs de formulaire et un bouton pour inscrire un nouvel utilisateur.
4. Ouvrez le *Register.aspx.cs* fichier et remplacez le contenu du fichier par le code suivant :

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Le code ci-dessus est une version simplifiée de la *Register.aspx.cs* fichier est créé lorsque vous créez un nouveau projet ASP.NET Web Forms.
    > 2. Le *IdentityUser* classe est l’implémentation EntityFramework par défaut de la *IUser* interface. *IUser* interface est l’interface minimale pour un utilisateur sur l’identité de ASP.NET Core.
    > 3. Le *UserStore* classe est l’implémentation d’Entity Framework par défaut d’un magasin d’utilisateurs. Cette classe implémente les interfaces minimales de l’identité d’ASP.NET Core : *IUserStore*, *IUserLoginStore*, *IUserClaimStore* et *IUserRoleStore*.
    > 4. Le *UserManager* classe expose l’API qui enregistrement automatiquement les modifications associées à la *UserStore*.
    > 5. Le *IdentityResult* classe représente le résultat d’une opération d’identité.
5. Dans **l’Explorateur de solutions**, cliquez sur votre projet et sélectionnez **ajouter**, **ajouter le dossier ASP.NET** , puis **application\_données**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Ouvrez le *Web.config* fichier, puis ajoutez une entrée de chaîne de connexion pour la base de données que nous utiliserons pour stocker les informations utilisateur. La base de données est créé lors de l’exécution par Entity Framework pour les entités d’identité. La chaîne de connexion est similaire à celui créé pour vous lorsque vous créez un nouveau projet Web Forms. Le code en surbrillance montre le balisage que vous devez ajouter :

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Pour Visual Studio 2015 ou version ultérieure, remplacez `(localdb)\v11.0` avec `(localdb)\MSSQLLocalDB` dans votre chaîne de connexion.
    
7. Cliquez avec le bouton droit sur fichier *Register.aspx* dans votre projet, puis sélectionnez **définir comme Page de démarrage**. Appuyez sur Ctrl + F5 pour générer et exécuter l’application web. Entrez un nouveau nom d’utilisateur et le mot de passe, puis sélectionnez **inscrire**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity a prise en charge pour la validation et dans cet exemple, vous pouvez vérifier le comportement par défaut sur l’utilisateur et mot de passe validateurs sont fournis à partir du package de base de l’identité. Le validateur par défaut pour l’utilisateur (`UserValidator`) possède une propriété `AllowOnlyAlphanumericUserNames` qui a la valeur par défaut est définie sur `true`. Le validateur par défaut pour le mot de passe (`MinimumLengthValidator`) garantit que ce mot de passe a au moins 6 caractères. Ces validateurs sont des propriétés sur `UserManager` qui peut être substituée si vous souhaitez que la validation personnalisée,

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Vérifiez que la base de données de l’identité de la base de données locale et les tables générées par Entity Framework

1. Dans le **vue** menu, sélectionnez **Explorateur de serveurs**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Développez **DefaultConnection (WebFormsIdentity)**, développez **Tables**, avec le bouton droit **AspNetUsers** , puis sélectionnez **afficher les données de Table**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Configurer l’application pour l’authentification OWIN

À ce stade nous avons seulement ajouté la prise en charge pour la création d’utilisateurs. Nous allons maintenant, pour illustrer la façon dont nous pouvons ajouter l’authentification pour vous connecter un utilisateur. ASP.NET Identity utilise l’intergiciel d’authentification OWIN de Microsoft pour l’authentification par formulaire. L’authentification de Cookie OWIN est un cookie et le mécanisme d’authentification basé sur qui peut être utilisée par n’importe quelle infrastructure hébergée sur des revendications [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ou IIS. Avec ce modèle, les packages de l’authentification même peuvent être utilisés sur plusieurs infrastructures, notamment ASP.NET MVC et Web Forms. Pour plus d’informations sur le projet Katana et comment exécuter l’intergiciel (middleware) dans un consultez agnostique hôte [mise en route avec le projet Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Installer les packages d’authentification à votre application

1. Dans l’Explorateur de solutions, cliquez sur votre projet et sélectionnez **gérer les Packages NuGet**. Recherchez et installez le ***Microsoft.AspNet.Identity.Owin*** package. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Recherchez et installez le ***Microsoft.Owin.Host.SystemWeb*** package.

    > [!NOTE]
    > Le **Microsoft.Aspnet.Identity.Owin** package contient un ensemble de classes d’extension OWIN pour gérer et configurer l’intergiciel (middleware) OWIN d’authentification pour être consommés par les packages de base d’identité ASP.NET.
    > Le **Microsoft.Owin.Host.SystemWeb** package contient un serveur OWIN qui permet aux applications basées sur OWIN s’exécutent sur IIS à l’aide du pipeline de demande ASP.NET. Pour plus d’informations, consultez [intergiciel (middleware) OWIN dans IIS intégré pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Ajouter des classes de configuration de démarrage et d’authentification OWIN

1. Dans **l’Explorateur de solutions**, cliquez sur votre projet, sélectionnez **ajouter**, puis **ajouter un nouvel élément**. Dans la boîte de dialogue zone de texte de recherche, tapez «*owin*». Nommez la classe «*démarrage*» et sélectionnez **ajouter**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Dans le fichier Startup.cs, ajoutez le code en surbrillance ci-dessous pour configurer l’authentification des cookies OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Cette classe contient le `OwinStartup` attribut pour spécifier la classe de démarrage OWIN. Chaque application OWIN a une classe de démarrage où vous spécifiez des composants pour le pipeline d’application. Consultez [détection de classe de démarrage OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) pour plus d’informations sur ce modèle.

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Ajouter des formulaires web pour l’inscription et la signature des utilisateurs

1. Ouvrez le *Register.aspx.cs* fichier, puis ajoutez le code suivant qui connecte l’utilisateur lors de l’inscription réussit.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Dans la mesure où ASP.NET Identity et l’authentification des cookies OWIN sont du système basée sur les revendications, le framework exige que le développeur d’application générer un [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) pour l’utilisateur. ClaimsIdentity comporte des informations sur toutes les revendications pour l’utilisateur telles que les rôles de l’utilisateur appartient. Vous pouvez également ajouter plusieurs revendications de l’utilisateur à ce stade.
    > - Vous pouvez connecter l’utilisateur à l’aide de la classe AuthenticationManager d’OWIN et en appelant `SignIn` et en transmettant le ClaimsIdentity comme indiqué ci-dessus. Ce code connecte l’utilisateur et générer ainsi un cookie. Cet appel est analogue à [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilisé par le [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) module.
2. Dans **l’Explorateur de solutions**, cliquez sur votre projet, sélectionnez **ajouter**, puis **Web Form**. Nommez le formulaire web **connexion**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Remplacez le contenu de la *Login.aspx* fichier par le code suivant :

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Remplacez le contenu de la *Login.aspx.cs* fichier par le code suivant :

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Le `Page_Load` maintenant vérifie l’état de l’utilisateur actuel et prend des mesures en fonction de son `Context.User.Identity.IsAuthenticated` état.
    >     **Affichage enregistré dans le nom d’utilisateur** : L’infrastructure d’identité Microsoft ASP.NET a ajouté des méthodes d’extension sur [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) qui permet d’obtenir le `UserName` et `UserId` pour l’utilisateur connecté. Ces méthodes d’extension sont définies dans le `Microsoft.AspNet.Identity.Core` assembly. Ces méthodes d’extension sont le remplacement pour [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Méthode de connexion : `This` méthode remplace la précédente `CreateUser_Click` méthode dans cet exemple et cette maintenant se connecte l’utilisateur après avoir créé l’utilisateur.   
    >   L’infrastructure OWIN de Microsoft a ajouté des méthodes d’extension sur `System.Web.HttpContext` qui vous permet d’obtenir une référence à un `IOwinContext`. Ces méthodes d’extension sont définies dans `Microsoft.Owin.Host.SystemWeb` assembly. Le `OwinContext` classe expose un `IAuthenticationManager` propriété qui représente les fonctionnalités d’intergiciel (middleware) d’authentification disponibles sur la requête actuelle. Vous pouvez connecter l’utilisateur à l’aide de la `AuthenticationManager` de OWIN et l’appel `SignIn` et en passant le `ClaimsIdentity` comme indiqué ci-dessus. Identité ASP.NET et l’authentification des cookies OWIN étant système basé sur les revendications, l’infrastructure requiert l’application pour générer un `ClaimsIdentity` pour l’utilisateur. Le `ClaimsIdentity` contient des informations sur toutes les revendications pour l’utilisateur, telles que les rôles de l’utilisateur appartient. Vous pouvez également ajouter plusieurs revendications de l’utilisateur à ce stade, ce code connecte l’utilisateur et générer ainsi un cookie. Cet appel est analogue à [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilisé par le [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) module.
    > - `SignOut` méthode : Obtient une référence à la `AuthenticationManager` de OWIN et appelle `SignOut`. Ce comportement est analogue à [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) méthode utilisée par le [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) module.
5. Appuyez sur **Ctrl + F5** pour générer et exécuter l’application web. Entrez un nouveau nom d’utilisateur et le mot de passe, puis sélectionnez **inscrire**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Remarque : À ce stade, le nouvel utilisateur est créé et connecté.
6. Sélectionnez le **déconnecter** bouton. Vous êtes redirigé vers le journal dans le formulaire.
7. Entrez un nom d’utilisateur non valide ou le mot de passe, puis sélectionnez le **connectez-vous** bouton. 
   Le `UserManager.Find` méthode retournera null et le message d’erreur : " *Nom d’utilisateur non valide ou le mot de passe* « s’affichera.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
