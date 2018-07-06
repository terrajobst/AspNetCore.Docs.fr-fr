---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Commande et paiement avec PayPal | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 3299da33a68f02ac1b3ffe7c037d06d8ece9455e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835629"
---
<a name="checkout-and-payment-with-paypal"></a>Commande et paiement avec PayPal
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. Un Visual Studio 2013 [projet avec du code source c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.


Ce didacticiel décrit comment modifier l’exemple d’application Wingtip Toys à inclure l’autorisation de l’utilisateur, inscription et paiement à l’aide de PayPal. Uniquement les utilisateurs qui sont connectés dans aura l’autorisation d’acheter des produits. Fonctionnalités de l’inscription utilisateur intégrés du modèle de projet Web Forms ASP.NET 4.5 incluent déjà une grande partie de ce dont vous avez besoin. Vous allez ajouter la fonctionnalité de validation d’Express PayPal. Dans ce didacticiel vous utiliser le développeur de PayPal environnement, de test et aucun fonds réels ne seront transférées. À la fin du didacticiel, vous allez tester l’application en sélectionnant les produits à ajouter au panier d’achat, en cliquant sur le bouton de validation et transfert de données vers le site web de test PayPal. Sur le site web de test PayPal, vous confirmez vos informations d’expédition et de paiement et puis revenez à l’exemple d’application Wingtip Toys local pour confirmer et terminer l’achat.

Il existe plusieurs processeurs expérimentés paiement tiers spécialisés dans les achats en ligne évolutivité de l’adresse et de la sécurité. Les développeurs ASP.NET devraient peser les avantages d’utilisation d’une solution de paiement tiers avant la solution d’achat et d’implémentation d’un achat.

> [!NOTE] 
> 
> L’exemple d’application Wingtip Toys a été conçu pour indiqué décrit certains concepts spécifiques ASP.NET et les fonctionnalités disponibles pour les développeurs web ASP.NET. Cet exemple d’application n’était pas optimisée pour tous les cas possibles en ce qui concerne l’évolutivité et de sécurité.


## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment restreindre l’accès à des pages spécifiques dans un dossier.
- Comment créer un panier d’achat connu à partir d’un panier d’achat anonyme.
- Comment activer SSL pour le projet.
- Comment ajouter un fournisseur OAuth au projet.
- Comment utiliser PayPal pour acheter des produits à l’aide de l’environnement de test de PayPal.
- Comment afficher les détails de PayPal dans un **DetailsView** contrôle.
- Comment mettre à jour de la base de données de l’application Wingtip Toys avec les informations obtenues à partir de PayPal.

## <a name="adding-order-tracking"></a>Ajout de chaînage

Dans ce didacticiel, vous allez créer deux nouvelles classes pour effectuer le suivi des données à partir de l’ordre de qu'un utilisateur a créé. Les classes effectue le suivi des données concernant les informations d’expédition total d’achat et confirmation de paiement.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Ajouter l’ordre et les Classes de modèle de OrderDetail

Plus haut dans cette série de didacticiels, vous avez défini le schéma pour les catégories, produits, et les éléments de panier d’achat en créant le `Category`, `Product`, et `CartItem` classes dans le *modèles* dossier. Maintenant, vous allez ajouter deux nouvelles classes pour définir le schéma pour l’ordre de produit et les détails de la commande.

1. Dans le **modèles** dossier, ajoutez une nouvelle classe nommée *Order.cs*.   
   Le nouveau fichier de classe s’affiche dans l’éditeur.
2. Remplacez le code par défaut avec les éléments suivants :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Ajouter un *OrderDetail.cs* classe à la *modèles* dossier.
4. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Le `Order` et `OrderDetail` classes contient le schéma pour définir les informations de commande utilisées pour l’achat et de livraison.

En outre, vous devez mettre à jour la classe de contexte de base de données qui gère les classes d’entité et qui fournit l’accès à la base de données. Pour ce faire, vous allez ajouter la commande qui vient d’être créée et `OrderDetail` classes de modèle `ProductContext` classe.

1. Dans **l’Explorateur de solutions**, recherchez et ouvrez le *ProductContext.cs* fichier.
2. Ajoutez le code en surbrillance à la *ProductContext.cs* fichier comme indiqué ci-dessous :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Comme mentionné précédemment dans cette série de didacticiels, le code dans le *ProductContext.cs* fichier ajoute le `System.Data.Entity` espace de noms afin que vous avez accès à toutes les fonctionnalités principales d’Entity Framework. Cette fonctionnalité inclut la possibilité d’interroger, insérer, mettre à jour et supprimer des données en travaillant avec des objets fortement typés. Le code ci-dessus dans le `ProductContext` classe ajoute l’accès de l’Entity Framework vers la nouvelle `Order` et `OrderDetail` classes.

## <a name="adding-checkout-access"></a>Ajout d’accès d’extraction

L’exemple d’application Wingtip Toys permet aux utilisateurs anonymes d’examiner et ajouter des produits à un panier d’achat. Toutefois, lorsque les utilisateurs anonymes choisissent les produits qu’ils ajoutés au panier d’achat, ils doivent vous connecter au site. Une fois qu’ils ont ouvert une session, ils peuvent accéder aux pages restreints de l’application Web qui gèrent l’extraction et de processus d’achat. Ces pages à accès restreint sont contenus dans le *extraction* dossier de l’application.

### <a name="add-a-checkout-folder-and-pages"></a>Ajouter un dossier d’extraction et de Pages

Vous allez maintenant créer le *extraction* dossier et les pages qu’il contient le client qui s’affiche que pendant le processus de validation. Vous mettrez à jour ces pages plus loin dans ce didacticiel.

1. Cliquez sur le nom de projet (**Wingtip Toys**) dans **l’Explorateur de solutions** et sélectionnez **ajouter un nouveau dossier**. 

    ![Commande et paiement avec PayPal - nouveau dossier](checkout-and-payment-with-paypal/_static/image1.png)
2. Nommez le nouveau dossier *extraction*.
3. Avec le bouton droit le *extraction* dossier, puis sélectionnez **ajouter**-&gt;**un nouvel élément**. 

    ![Commande et paiement avec PayPal - nouvel élément](checkout-and-payment-with-paypal/_static/image2.png)
4. La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
5. Sélectionnez le **Visual C#**  - &gt; **Web** groupe de modèles sur la gauche. Ensuite, dans le volet central, sélectionnez **Web Form avec Page maître**et nommez-le *CheckoutStart.aspx*. 

    ![Commande et paiement avec PayPal - ajouter la boîte de dialogue Nouvel élément](checkout-and-payment-with-paypal/_static/image3.png)
6. Comme précédemment, sélectionnez le *Site.Master* fichier en tant que la page maître.
7. Ajouter des pages supplémentaires pour le *extraction* dossier à l’aide de la procédure ci-dessus :   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Ajouter un fichier Web.config

En ajoutant une nouvelle *Web.config* de fichiers à la *extraction* dossier, vous pourrez restreindre l’accès à toutes les pages contenues dans le dossier.

1. Avec le bouton droit le *extraction* dossier et sélectionnez **ajouter**  - &gt; **un nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le **Visual C#**  - &gt; **Web** groupe de modèles sur la gauche. Ensuite, dans le volet central, sélectionnez **fichier de Configuration Web**, acceptez le nom par défaut *Web.config*, puis sélectionnez **ajouter**.
3. Remplacez le contenu XML existant dans le *Web.config* fichier par le code suivant :  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Enregistrer le *Web.config* fichier.

Le *Web.config* fichier Spécifie que tous les utilisateurs inconnus de l’application Web doivent être interdit l’accès aux pages contenues dans le *extraction* dossier. Cependant, si l’utilisateur a inscrit un compte et a ouvert une session, ils seront un utilisateur connu et auront accès aux pages dans le *extraction* dossier.

Il est important de noter que configuration ASP.NET suit une hiérarchie, où chaque *Web.config* fichier applique les paramètres de configuration vers le dossier qu’il est et à tous les répertoires enfants situés en dessous.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Activer SSL pour le projet

 Couche de Sockets sécurisée (SSL) est un protocole défini pour autoriser les serveurs Web et Web aux clients de communiquer en toute sécurité via l’utilisation du chiffrement. Lorsque SSL n’est pas utilisé, les données envoyées entre le client et le serveur sont ouverts pour la détection des paquets par toute personne ayant un accès physique au réseau. En outre, plusieurs schémas d’authentification ne sont pas sécurisés sur le HTTP standard. En particulier, l’authentification de base et l’authentification par formulaire envoient les informations d’identification non chiffrées. Pour être sécurisés, ces schémas d’authentification doivent utiliser SSL. 

1. Dans **l’Explorateur de solutions**, cliquez sur le **WingtipToys** de projet, puis appuyez sur **F4** pour afficher le **propriétés** fenêtre.
2. Modification **SSL activé** à `true`.
3. Copie le **URL SSL** afin de pouvoir l’utiliser ultérieurement.   
 L’URL SSL sera `https://localhost:44300/` sauf si vous avez déjà créé des Sites Web SSL (comme indiqué ci-dessous).   
    ![Propriétés de projet](checkout-and-payment-with-paypal/_static/image4.png)
4. Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le **WingtipToys** projet puis cliquez sur **propriétés**.
5. Dans l’onglet gauche, cliquez sur **Web**.
6. Modifier le **Url du projet** à utiliser le **URL SSL** que vous avez enregistré précédemment.   
    ![Propriétés du projet Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Enregistrez la page en appuyant sur **CTRL + S**.
8. Appuyez sur **Ctrl+F5** pour exécuter l’application. Visual Studio affiche une option vous permet d’éviter les avertissements SSL.
9. Cliquez sur **Oui** pour approuver le certificat IIS Express SSL et continuer.   
    ![Détails du certificat SSL d’IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Un avertissement de sécurité s’affiche.
10. Cliquez sur **Oui** pour installer le certificat à votre localhost.   
    ![Boîte de dialogue Avertissement de sécurité](checkout-and-payment-with-paypal/_static/image7.png)  
 La fenêtre du navigateur s’affichera.

Vous pouvez désormais facilement tester votre application Web localement à l’aide de SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Ajouter un fournisseur OAuth 2.0

ASP.NET Web Forms fournit des options améliorées pour l’appartenance et l’authentification. Ces améliorations incluent OAuth. OAuth est un protocole ouvert permettant d’autorisation sécurisée dans une méthode simple et standard à partir d’applications web, mobiles et de bureau. Le modèle Web Forms ASP.NET utilise OAuth pour exposer Facebook, Twitter, Google et Microsoft comme fournisseurs d’authentification. Bien que ce didacticiel utilise uniquement Google comme fournisseur d’authentification, vous pouvez facilement modifier le code pour utiliser un des fournisseurs. Les étapes pour implémenter d’autres fournisseurs sont très similaires à celles que vous voyez dans ce didacticiel.

Outre l’authentification, le didacticiel utilisera également des rôles pour implémenter l’autorisation. Seuls les utilisateurs que vous ajoutez à la `canEdit` rôle sera en mesure de modifier des données (créer, modifier ou supprimer des contacts).

> [!NOTE] 
> 
> Applications Windows Live acceptent uniquement une URL dynamique pour un site Web de travail, vous ne pouvez pas utiliser une URL de site Web local pour le test des connexions.


Les étapes suivantes vous permettra à ajouter un fournisseur d’authentification Google.

1. Ouvrez le *application\_Start\Startup.Auth.cs* fichier.
2. Supprimez les caractères de commentaire de la `app.UseGoogleAuthentication()` méthode afin que la méthode apparaît comme suit : 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Accédez à la [Google Developers Console](https://console.developers.google.com/). Vous devez également vous connecter à votre compte de messagerie de développeur Google (gmail.com). Si vous n’avez pas un compte Google, sélectionnez le **créer un compte** lien.   
   Ensuite, vous verrez la **Google Developers Console**.   
    ![Console développeur de Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Cliquez sur le **créer un projet** bouton et entrez un nom de projet et un ID (vous pouvez utiliser les valeurs par défaut). Ensuite, cliquez sur le **case à cocher de l’accord** et **créer** bouton.  

    ![Google - nouveau projet](checkout-and-payment-with-paypal/_static/image9.png)

   En quelques secondes le nouveau projet sera créé et votre navigateur affiche la nouvelle page de projets.
5. Dans l’onglet gauche, cliquez sur **API &amp; auth**, puis cliquez sur **informations d’identification**.
6. Cliquez sur le **créer un ID de Client** sous **OAuth**.   
   Le **créer un identifiant Client** boîte de dialogue s’affiche.   
    ![Google - créer un identifiant Client](checkout-and-payment-with-paypal/_static/image10.png)
7. Dans le **créer un identifiant Client** boîte de dialogue, conservez la valeur par défaut **application Web** pour le type d’application.
8. Définir le **origines JavaScript** à l’URL SSL que vous avez utilisé précédemment dans ce didacticiel (`https://localhost:44300/` , sauf si vous avez créé d’autres projets SSL).   
   Cette URL est l’origine de votre application. Pour cet exemple, vous entrerez uniquement l’URL de test de localhost. Toutefois, vous pouvez entrer plusieurs URL pour prendre en compte pour localhost et la production.
9. Définir le **URI de redirection autorisés** à ce qui suit : 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Cette valeur est l’URI que OAuth ASP.NET aux utilisateurs de communiquer avec le serveur OAuth google. N’oubliez pas de l’URL SSL que vous avez utilisé ci-dessus ( `https://localhost:44300/` , sauf si vous avez créé d’autres projets SSL).
10. Cliquez sur le **créer un identifiant Client** bouton.
11. Dans le menu de gauche de la Console de développeur de Google, cliquez sur le **écran de consentement** élément de menu, puis définissez votre nom de produit et l’adresse de messagerie. Lorsque vous avez rempli le formulaire, cliquez sur **enregistrer**.
12. Cliquez sur le **API** élément de menu, faites défiler vers le bas, cliquez sur le **hors** situé en regard **API Google +**.   
    Acceptation de cette option permettra l’API Google +.
13. Vous devez également mettre à jour le **Microsoft.Owin** package NuGet à la version 3.0.0.   
    À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet** , puis sélectionnez **gérer les Packages NuGet pour la Solution**.  
    À partir de la **gérer les Packages NuGet** fenêtre, recherchez et mise à jour le **Microsoft.Owin** package à la version 3.0.0.
14. Dans Visual Studio, mettez à jour le `UseGoogleAuthentication` méthode de la *Startup.Auth.cs* page en copiant et collant la **ID Client** et **clé secrète Client** dans la méthode. Le **ID Client** et **clé secrète Client** valeurs indiquées ci-dessous sont des exemples et ne fonctionnera pas. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Appuyez sur **CTRL + F5** pour générer et exécuter l’application. Cliquez sur le **connectez-vous** lien.
16. Sous **utiliser un autre service pour vous connecter**, cliquez sur **Google**.  
    ![Connectez-vous](checkout-and-payment-with-paypal/_static/image11.png)
17. Si vous avez besoin d’entrer vos informations d’identification, vous serez redirigé vers le site google où vous entrez vos informations d’identification.  
    ![Google - se connecter](checkout-and-payment-with-paypal/_static/image12.png)
18. Après avoir entré vos informations d’identification, vous devrez accorder des autorisations à l’application web que vous venez de créer.  
    ![Compte de Service de projet par défaut](checkout-and-payment-with-paypal/_static/image13.png)
19. Cliquez sur **accepter**. Vous allez maintenant être redirigé vers le **inscrire** page de la **WingtipToys** application où vous pouvez inscrire votre compte Google.  
    ![Inscrire avec votre compte Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Vous avez la possibilité de modifier le nom d’inscription local utilisé pour votre compte Gmail, mais il est généralement conseillé de conserver l’alias de messagerie électronique par défaut (autrement dit, celui utilisé pour l’authentification). Cliquez sur **connectez-vous** comme indiqué ci-dessus.

### <a name="modifying-login-functionality"></a>Modification des fonctionnalités de connexion

Comme mentionné précédemment dans cette série de didacticiels, une grande partie des fonctionnalités de l’inscription de l’utilisateur a été incluse dans le modèle Web Forms ASP.NET par défaut. Vous allez maintenant modifier la valeur par défaut *Login.aspx* et *Register.aspx* pages pour appeler le `MigrateCart` (méthode). Le `MigrateCart` méthode associe un utilisateur qui vient d’être connecté à un panier d’achat anonyme. En associant l’utilisateur et le panier d’achat, l’exemple d’application Wingtip Toys sera en mesure de maintenir le panier d’achat de l’utilisateur entre les visites.

1. Dans **l’Explorateur de solutions**, recherchez et ouvrez le *compte* dossier.
2. Modifier la page code-behind nommée *Login.aspx.cs* pour inclure le code mis en surbrillance en jaune, afin qu’il apparaisse comme suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Enregistrer le *Login.aspx.cs* fichier.

Pour l’instant, vous pouvez ignorer l’avertissement qu’il n’existe aucune définition pour le `MigrateCart` (méthode). Vous allez ajouter cela un peu plus loin dans ce didacticiel.

Le *Login.aspx.cs* fichier code-behind prend en charge une méthode de connexion. En examinant la page Login.aspx, vous verrez que cette page inclut un bouton « Se connecter » que quand cliquez sur les déclencheurs le `LogIn` gestionnaire sur le code-behind.

Lorsque le `Login` méthode sur le *Login.aspx.cs* est appelée, une nouvelle instance de panier d’achat nommé `usersShoppingCart` est créé. L’ID du panier d’achat (GUID) est récupéré et défini sur le `cartId` variable. Ensuite, le `MigrateCart` méthode est appelée, en passant à la fois le `cartId` et le nom de l’utilisateur connecté à cette méthode. Lorsque le panier d’achat est migré, le GUID utilisé pour identifier le panier d’achat anonyme est remplacé par le nom d’utilisateur.

En plus de modifier le *Login.aspx.cs* fichier code-behind pour migrer le panier d’achat lorsque l’utilisateur se connecte, vous devez également modifier le *fichier code-behind de Register.aspx.cs* pour migrer le panier d’achat Lorsque l’utilisateur crée un nouveau compte et se connecte.

1. Dans le *compte* dossier, ouvrez le fichier code-behind nommé *Register.aspx.cs*.
2. Modifiez le fichier code-behind en incluant le code en jaune, afin qu’il apparaisse comme suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Enregistrer le *Register.aspx.cs* fichier. Une fois encore, ignorer l’avertissement sur la `MigrateCart` (méthode).

Notez que le code que vous avez utilisé dans le `CreateUser_Click` Gestionnaire d’événements est très similaire au code que vous avez utilisé dans le `LogIn` (méthode). Lorsque l’utilisateur enregistre ou ouvre une session le site, un appel à la `MigrateCart` méthode sera établie.

## <a name="migrating-the-shopping-cart"></a>Migrer le panier d’achat

Maintenant que vous avez le processus d’ouverture de session et d’inscription mis à jour, vous pouvez ajouter le code pour migrer le panier d’achat à l’aide du `MigrateCart` (méthode).

1. Dans **l’Explorateur de solutions**, recherchez le *logique* dossier et ouvrez le *ShoppingCartActions.cs* fichier de classe.
2. Ajoutez le code mis en surbrillance en jaune pour le code existant dans le *ShoppingCartActions.cs* fichier, afin que le code dans le *ShoppingCartActions.cs* fichier s’affiche comme suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Le `MigrateCart` méthode utilise le cartId existant pour trouver le panier d’achat de l’utilisateur. Ensuite, le code effectue une itération sur tous les éléments du panier d’achat et remplace le `CartId` propriété (comme spécifié par le `CartItem` schéma) avec le nom de l’utilisateur connecté.

### <a name="updating-the-database-connection"></a>La mise à jour de la connexion de base de données

Si vous suivez ce didacticiel en utilisant le **prédéfinis** Wingtip Toys exemple d’application, vous devez recréer la base de données d’appartenance par défaut. En modifiant la chaîne de connexion par défaut, la base de données d’appartenance s’affichera lors de la prochaine exécution de l’application.

1. Ouvrez le *Web.config* fichier à la racine du projet.
2. Mettre à jour la chaîne de connexion par défaut afin qu’il apparaisse comme suit :   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>L’intégration PayPal

PayPal est une plateforme de facturation basée sur le web qui accepte les paiements effectués par les commerçants en ligne. Ce didacticiel explique ensuite comment intégrer la fonctionnalité de validation Express de PayPal dans votre application. Extraction expresse permet à vos clients à utiliser PayPal pour payer les éléments qu’ils ont ajoutés à leur panier d’achat.

### <a name="create-paylpal-test-accounts"></a>Créer des comptes de Test de PaylPal

Pour utiliser l’environnement de test de PayPal, vous devez créer et vérifier un compte de test de développeur. Vous allez utiliser le compte de test de développeur pour créer un acheteur potentiel de compte de test et un compte de test de vendeur. Les informations d’identification du compte de test développeur permettent également l’exemple d’application Wingtip Toys pour accéder à l’environnement de test PayPal.

1. Dans un navigateur, naviguez vers le site de test pour les développeurs PayPal :   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Si vous n’avez pas un compte de développeur de PayPal, créez un nouveau compte en cliquant sur **s’inscrire**et en suivant la procédure d’inscription. Si vous disposez d’un compte développeur PayPal, connectez-vous en cliquant sur **Log In**. Vous aurez besoin de votre compte de développeur de PayPal pour tester l’exemple d’application Wingtip Toys plus loin dans ce didacticiel.
3. Si vous êtes simplement inscrit pour votre compte de développeur de PayPal, vous devrez peut-être vérifier votre compte de développeur de PayPal avec PayPal. Vous pouvez vérifier votre compte en suivant les étapes PayPal envoyé à votre compte de messagerie. Une fois que vous avez vérifié votre compte de développeur de PayPal, ouvrez une session dans le site de test pour les développeurs PayPal.
4. Une fois que vous êtes connecté au site développeur PayPal avec votre compte de développeur PayPal que vous devez créer un compte de test de l’acheteur PayPal si vous n’avez pas déjà posséder un. Pour créer un compte de test de l’acheteur, sur le site PayPal, cliquez sur le **Applications** onglet, puis cliquez sur **des comptes de bac à sable**.   
 Le **les comptes de test de bac à sable** page est affichée.   

    > [!NOTE] 
    > 
    > Le site de développement de PayPal fournit déjà un compte de marchand de test.

    ![Commande et paiement avec PayPal - comptes de test de bac à sable](checkout-and-payment-with-paypal/_static/image15.png)
5. Dans la page comptes de test de bac à sable, cliquez sur **créer un compte**.
6. Sur le **créer un compte test** page Choisir un acheteur potentiel de courrier électronique de test compte et mot de passe de votre choix.   

    > [!NOTE] 
    > 
    > Vous devez les adresses de messagerie de l’acheteur et un mot de passe pour tester l’exemple d’application Wingtip Toys à la fin de ce didacticiel.

    ![Commande et paiement avec PayPal - comptes de test de bac à sable](checkout-and-payment-with-paypal/_static/image16.png)
7. Créer le compte de test de l’acheteur en cliquant sur le **créer un compte** bouton.  
 Le **comptes de bac à sable Test** page s’affiche. 

    ![Commande et paiement avec PayPal - PaylPal comptes](checkout-and-payment-with-paypal/_static/image17.png)
8. Sur le **les comptes de test de bac à sable** , cliquez sur le **l’animateur** compte de messagerie.  
    **Profil** et **Notification** options s’affichent.
9. Sélectionnez le **profil** option, puis cliquez sur **informations d’identification de l’API** pour afficher vos informations d’identification de l’API pour le compte de marchand de test.
10. Copiez les informations d’identification de l’API de TEST dans le bloc-notes.

Vous devez vos informations d’identification Classic API de TEST affichées (nom d’utilisateur, mot de passe et Signature) pour effectuer des appels d’API à partir de l’exemple d’application Wingtip Toys à l’environnement de test de PayPal. Vous allez ajouter les informations d’identification à l’étape suivante.

### <a name="add-paypal-class-and-api-credentials"></a>Ajouter des informations d’identification de l’API et de classe de PayPal

Vous devez placer la majorité du code PayPal en une seule classe. Cette classe contient les méthodes utilisées pour communiquer avec PayPal. En outre, vous allez ajouter vos informations d’identification PayPal à cette classe.

1. Dans l’application exemple Wingtip Toys, au sein de Visual Studio, cliquez sur le **logique** dossier, puis sélectionnez **ajouter**  - &gt; **un nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sous **Visual C#** à partir de la **installé** volet de gauche, sélectionnez **Code**.
3. Dans le volet central, sélectionnez **classe**. Nommez cette nouvelle classe **PayPalFunctions.cs**.
4. Cliquez sur **Ajouter**.  
   Le nouveau fichier de classe s’affiche dans l’éditeur.
5. Remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Ajouter les API de marchand informations d’identification (nom d’utilisateur, mot de passe et Signature) que vous avez affiché précédemment dans ce didacticiel afin que vous pouvez effectuer des appels de fonction dans l’environnement de test de PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Dans cet exemple d’application vous ajoutez simplement informations d’identification dans un fichier c# (.cs). Toutefois, dans une solution implémentée, vous devez envisager de chiffrer vos informations d’identification dans un fichier de configuration.


La classe NVPAPICaller contient la majorité des fonctionnalités de PayPal. Le code dans la classe fournit les méthodes nécessaires pour effectuer un test d’achat à partir de l’environnement de test de PayPal. Les fonctions de PayPal trois suivantes sont utilisées pour effectuer des achats :

- `SetExpressCheckout` (Fonction)
- `GetExpressCheckoutDetails` (Fonction)
- `DoExpressCheckoutPayment` (Fonction)

Le `ShortcutExpressCheckout` méthode collecte les détails de produit et les informations de fournisseur test à partir du panier d’achat et les appels le `SetExpressCheckout` PayPal (fonction). Le `GetCheckoutDetails` méthode confirme les détails de l’achat et appelle le `GetExpressCheckoutDetails` PayPal (fonction) avant d’effectuer l’achat de test. Le `DoCheckoutPayment` méthode termine à l’achat de test à partir de l’environnement de test en appelant le `DoExpressCheckoutPayment` PayPal (fonction). Le code restant prend en charge les méthodes de PayPal et les processus, tels que l’encodage des chaînes, décodage des chaînes, tableaux de traitement et déterminer les informations d’identification.

> [!NOTE] 
> 
> PayPal permet de vous permettent d’inclure les détails de l’achat facultatif selon [spécification de l’API de PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). En étendant le code dans l’exemple d’application Wingtip Toys, vous pouvez inclure des détails de la localisation, descriptions de produit, taxe, un numéro de service client, ainsi que plusieurs autres champs facultatifs.


Notez que les URL de retour et d’annulation qui sont spécifiés dans le **ShortcutExpressCheckout** méthode utiliser un numéro de port.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Lorsque Visual Web Developer exécute un projet web à l’aide de SSL, le port 44300 est couramment utilisé pour le serveur web. Comme indiqué ci-dessus, le numéro de port est 44300. Lorsque vous exécutez l’application, vous pouvez voir un numéro de port différent. Vos besoins de numéro de port doit être correctement défini dans le code afin que vous puissiez réussie exécuter l’exemple d’application Wingtip Toys à la fin de ce didacticiel. La section suivante de ce didacticiel explique comment récupérer le numéro de port d’hôte local et mettre à jour de la classe de PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Mettre à jour le numéro de Port de LocalHost dans la classe PayPal

L’exemple d’application Wingtip Toys vos achats de produits en accédant au site de test PayPal et en retournant à votre instance locale de l’exemple d’application Wingtip Toys. Pour revenir à l’URL correcte PayPal, vous devez spécifier le numéro de port de l’exécution localement exemple d’application dans le code de PayPal mentionné ci-dessus.

1. Cliquez sur le nom de projet (**WingtipToys**) dans **l’Explorateur de solutions** et sélectionnez **propriétés**.
2. Dans la colonne de gauche, sélectionnez le **Web** onglet.
3. Récupérer le numéro de port à partir de la **Url du projet** boîte.
4. Si nécessaire, mettez à jour le `returnURL` et `cancelURL` dans la classe PayPal (`NVPAPICaller`) dans le *PayPalFunctions.cs* fichier à utiliser le numéro de port de votre application web :   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Maintenant le code que vous avez ajoutée sera correspond au port attendu pour votre application Web locale. PayPal sera en mesure de retourner à l’URL correcte sur votre ordinateur local.

### <a name="add-the-paypal-checkout-button"></a>Ajouter le bouton de retrait de PayPal

Maintenant que les principales fonctions de PayPal ont été ajoutées à l’exemple d’application, vous pouvez commencer à ajouter le balisage et le code nécessaire pour appeler ces fonctions. Tout d’abord, vous devez ajouter le bouton de passer l’utilisateur s’affiche sur la page du panier.

1. Ouvrez le *ShoppingCart.aspx* fichier.
2. Faites défiler vers le bas du fichier et recherchez le `<!--Checkout Placeholder -->` commentaire.
3. Remplacez le commentaire avec un `ImageButton` contrôler afin que le balisage est remplacé comme suit :  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Dans le *ShoppingCart.aspx.cs* de fichiers, après le `UpdateBtn_Click` Gestionnaire d’événements vers la fin du fichier, ajoutez le `CheckOutBtn_Click` Gestionnaire d’événements :  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Également dans le *ShoppingCart.aspx.cs* , ajoutez une référence à la `CheckoutBtn`, de sorte que le nouveau bouton d’image est référencé comme suit :  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Enregistrez vos modifications dans les deux le *ShoppingCart.aspx* fichier et le *ShoppingCart.aspx.cs* fichier.
7. Dans le menu, sélectionnez **déboguer**-&gt;**WingtipToys Build**.  
   Le projet va être régénéré avec récemment ajouté **ImageButton** contrôle.

### <a name="send-purchase-details-to-paypal"></a>Envoyer les détails de l’achat à PayPal

Lorsque l’utilisateur clique sur le **extraction** bouton sur la page du panier (*ShoppingCart.aspx*), ils commenceront le processus d’achat. Le code suivant appelle la première fonction PayPal nécessitée pour acheter des produits.

1. À partir de la *extraction* dossier, ouvrez le fichier code-behind nommé *CheckoutStart.aspx.cs*.   
   Veillez à ouvrir le fichier code-behind.
2. Remplacez le code existant par le code ci-dessous :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Lorsque l’utilisateur de l’application clique sur le **extraction** bouton sur la page du panier, le navigateur permet d’accéder à la *CheckoutStart.aspx* page. Lorsque le *CheckoutStart.aspx* page charges, le `ShortcutExpressCheckout` méthode est appelée. À ce stade, l’utilisateur est transféré vers le site web de test PayPal. Sur le site de PayPal, l’utilisateur entre ses informations d’identification de PayPal, passe en revue les détails de l’achat, encore accepte le contrat de PayPal et retourne à l’exemple d’application Wingtip Toys où le `ShortcutExpressCheckout` méthode se termine. Lorsque le `ShortcutExpressCheckout` méthode est terminée, il redirige l’utilisateur vers le *CheckoutReview.aspx* page spécifiée dans le `ShortcutExpressCheckout` (méthode). Cela permet à l’utilisateur passer en revue les détails de commande à partir de l’exemple d’application Wingtip Toys.

### <a name="review-order-details"></a>Détails de la commande

Après le renvoi de PayPal, le *CheckoutReview.aspx* page de l’exemple d’application Wingtip Toys affiche les détails de commande. Cette page permet à l’utilisateur passer en revue les détails de commande avant d’acheter les produits. Le *CheckoutReview.aspx* page doit être créée comme suit :

1. Dans le *extraction* dossier, ouvrez la page nommée *CheckoutReview.aspx*.
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Ouvrez la page code-behind nommée *CheckoutReview.aspx.cs* et remplacez le code existant par le code suivant :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Le **DetailsView** contrôle est utilisé pour afficher les détails des commandes qui ont été retournés à partir de PayPal. En outre, le code ci-dessus enregistre les détails de commande à la base de données Wingtip Toys en tant qu’un `OrderDetail` objet. Lorsque l’utilisateur clique sur le **Order complète** bouton, ils sont redirigés vers le *CheckoutComplete.aspx* page.

> [!NOTE] 
> 
> **Info-bulle**
> 
> Dans le balisage de la *CheckoutReview.aspx* page, notez que le `<ItemStyle>` balise est utilisée pour modifier le style des éléments dans le **DetailsView** contrôle près du bas de la page. En affichant la page dans **mode Design** (en sélectionnant **conception** dans l’angle inférieur gauche de Visual Studio), puis en sélectionnant le **DetailsView** contrôler, puis en sélectionnant le  **Balise active** (l’icône de flèche en haut à droite du contrôle), vous serez en mesure de voir les **Tâches DetailsView**.
> 
> ![Commande et paiement avec PayPal - modifier les champs](checkout-and-payment-with-paypal/_static/image18.png)
> 
> En sélectionnant **modifier les champs**, le **champs** boîte de dialogue s’affiche. Dans cette boîte de dialogue vous pouvez facilement contrôler les propriétés visuelles, telles que **ItemStyle**, de la **DetailsView** contrôle.
> 
> ![Commande et paiement avec PayPal - boîte de dialogue champs](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Effectuer l’achat

*CheckoutComplete.aspx* page effectue l’achat de PayPal. Comme mentionné ci-dessus, l’utilisateur doit cliquer sur le **Order complète** bouton avant que l’application permet d’accéder à la *CheckoutComplete.aspx* page.

1. Dans le *extraction* dossier, ouvrez la page nommée *CheckoutComplete.aspx*.
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Ouvrez la page code-behind nommée *CheckoutComplete.aspx.cs* et remplacez le code existant par le code suivant :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Lorsque le *CheckoutComplete.aspx* page est chargée, le `DoCheckoutPayment` méthode est appelée. Comme mentionné précédemment, le `DoCheckoutPayment` méthode termine à l’achat à partir de l’environnement de test de PayPal. Une fois PayPal a terminé l’achat de la commande, le *CheckoutComplete.aspx* page affiche une transaction de paiement `ID` à l’acheteur.

### <a name="handle-cancel-purchase"></a>Handle d’annuler l’achat

Si l’utilisateur décide d’annuler l’achat, ils seront dirigés vers le *CheckoutCancel.aspx* page où ils verront que leur ordre a été annulée.

1. Ouvrez la page nommée *CheckoutCancel.aspx* dans le *extraction* dossier.
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Gérer les erreurs de fournisseur

Erreurs pendant le processus d’achat seront gérées par le *CheckoutError.aspx* page. Le code-behind de la *CheckoutStart.aspx* page, le *CheckoutReview.aspx* page et le *CheckoutComplete.aspx* page chacun redirige vers le  *CheckoutError.aspx* page si une erreur se produit.

1. Ouvrez la page nommée *CheckoutError.aspx* dans le *extraction* dossier.
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

Le *CheckoutError.aspx* page est affichée avec les détails d’erreur lorsqu’une erreur se produit pendant le processus de validation.

## <a name="running-the-application"></a>Exécution de l'application

Exécutez l’application pour voir comment acheter des produits. Notez que vous allez exécuter dans le PayPal environnement de test. Aucun argent n’est échangée.

1. Assurez-vous que tous vos fichiers sont enregistrés dans Visual Studio.
2. Ouvrez un navigateur Web et accédez à [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Connectez-vous avec votre compte de développeur PayPal que vous avez créé précédemment dans ce didacticiel.  
   Pour sandbox du développeur de PayPal, vous devez être connecté au [ https://developer.paypal.com ](https://developer.paypal.com/) pour tester la récupération rapide. Cela s’applique uniquement au bac à sable de PayPal test, pas à l’environnement de production de PayPal.
4. Dans Visual Studio, appuyez sur **F5** pour exécuter l’exemple d’application Wingtip Toys.  
   Une fois que la base de données est reconstruit, le navigateur s’ouvre et affiche le *Default.aspx* page.
5. Ajoutez les trois produits différents au panier en sélectionnant la catégorie de produits, tels que « Cars », puis sur **ajouter au panier** en regard de chaque produit.  
   Le panier d’achat affiche le produit que vous avez sélectionné.
6. Cliquez sur le **PayPal** bouton à extraire. 

    ![Commande et paiement avec PayPal - panier](checkout-and-payment-with-paypal/_static/image20.png)

   La récupération nécessite que vous disposez d’un compte d’utilisateur pour l’exemple d’application Wingtip Toys.
7. Cliquez sur le **Google** lien à droite de la page pour vous connecter avec un compte de messagerie gmail.com existant.  
   Si vous n’avez pas un compte gmail.com, vous pouvez en créer un à des fins de test [www.gmail.com](https://www.gmail.com/). Vous pouvez également utiliser un compte local standard en cliquant sur « Enregistrer ». 

    ![Commande et paiement avec PayPal - connectez-vous](checkout-and-payment-with-paypal/_static/image21.png)
8. Connectez-vous à votre compte gmail et le mot de passe. 

    ![Commande et paiement avec PayPal - connexion Gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Cliquez sur le **connectez-vous** bouton pour enregistrer votre compte gmail avec votre nom d’utilisateur application Wingtip Toys exemple. 

    ![Commande et paiement avec PayPal - compte d’inscription](checkout-and-payment-with-paypal/_static/image23.png)
10. Sur le site de test PayPal, ajoutez votre **buyer** adresse électronique et mot de passe que vous avez créé précédemment dans ce didacticiel, puis cliquez sur le **Log In** bouton. 

    ![Commande et paiement avec PayPal - PayPal de connexion](checkout-and-payment-with-paypal/_static/image24.png)
11. Acceptez la stratégie de PayPal, cliquez sur le **accepter et continuer** bouton.  
    Notez que cette page est uniquement affiché la première fois que vous utilisez ce compte PayPal. Notez à nouveau qu’il s’agit d’un compte de test, aucun money réel n’est échangée. 

    ![Commande et paiement avec PayPal - stratégie de PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Passez en revue les informations de commande sur le PayPal test page révision de l’environnement et cliquez sur **continuer**. 

    ![Commande et paiement avec PayPal - consulter les informations](checkout-and-payment-with-paypal/_static/image26.png)
13. Sur le *CheckoutReview.aspx* page, vérifiez que le montant de la commande et afficher l’adresse d’expédition généré. Ensuite, cliquez sur le **Order complète** bouton. 

    ![Commande et paiement avec PayPal - ordre révision](checkout-and-payment-with-paypal/_static/image27.png)
14. Le **CheckoutComplete.aspx** page est affichée avec un ID de transaction de paiement. 

    ![Commande et paiement avec PayPal - extraction terminée](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Examen de la base de données

En examinant les données mises à jour dans la base de données d’application Wingtip Toys exemple après l’exécution de l’application, vous pouvez voir que l’application enregistrée avec succès l’achat des produits.

Vous pouvez inspecter les données contenues dans le *Wingtiptoys.mdf* fichier de base de données à l’aide de la **Database Explorer** fenêtre (**Explorateur de serveurs** fenêtre dans Visual Studio) comme vous le faisiez plus haut dans cette série de didacticiels.

1. Fermez la fenêtre du navigateur si elle est toujours ouverte.
2. Dans Visual Studio, sélectionnez le **afficher tous les fichiers** icône en haut de **l’Explorateur de solutions** vous permet de développer le **application\_données** dossier.
3. Développez le **application\_données** dossier.  
 Vous devrez peut-être sélectionner la **afficher tous les fichiers** icône du dossier.
4. Cliquez sur le *Wingtiptoys.mdf* fichier de base de données, puis sélectionnez **Open**.  
    **Explorateur de serveurs** s’affiche.
5. Développez le **Tables** dossier.
6. Avec le bouton droit le **commandes**de table et sélectionnez **afficher les données de Table**.  
 Le **commandes** table s’affiche.
7. Examinez le **PaymentTransactionID** colonne pour confirmer les transactions réussies. 

    ![Commande et paiement avec PayPal - base de données de révision](checkout-and-payment-with-paypal/_static/image29.png)
8. Fermer le **commandes** fenêtre de la table.
9. Dans l’Explorateur de serveurs, cliquez sur le **OrderDetails** de table et sélectionnez **afficher les données de Table**.
10. Examinez le `OrderId` et `Username` des valeurs dans le **OrderDetails** table. Notez que ces valeurs correspondent à la `OrderId` et `Username` valeurs inclus dans le **commandes** table.
11. Fermer le **OrderDetails** fenêtre de la table.
12. Cliquez sur le fichier de base de données Wingtip Toys (*Wingtiptoys.mdf*) et sélectionnez **fermer la connexion**.
13. Si vous ne voyez pas le **l’Explorateur de solutions** fenêtre, cliquez sur **l’Explorateur de solutions** en bas de la **Explorateur de serveurs** fenêtre pour afficher le **l’Explorateur de solutions**  à nouveau.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez ajouté ordre et des schémas de détail de commande pour effectuer le suivi de l’achat de produits. Vous intégré également une fonctionnalité de PayPal dans l’exemple d’application Wingtip Toys.

## <a name="additional-resources"></a>Ressources supplémentaires

[Vue d’ensemble de la Configuration ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Déployer une application de formulaires Web ASP.NET sécurisée avec appartenance, OAuth et base de données SQL dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Exclusion de responsabilité

Ce didacticiel contient un exemple de code. Ces exemples de code est fourni « en l’état » sans garantie d’aucune sorte. En conséquence, Microsoft ne garantit pas la précision, d’intégrité ou qualité de l’exemple de code. Vous acceptez d’utiliser l’exemple de code à vos propres risques. En aucun cas Microsoft seront tenu de quelque manière pour les exemples de code, le contenu, y compris mais sans limitation, les erreurs ou d’omissions dans les exemples de code, contenu, aucune perte ni aucun dommage résultant de l’utilisation d’un exemple de code. Vous êtes informé par décident d’indemniser, enregistrer et défendre Microsoft contre toute perte, les revendications de perte, blessure ou dommage de n’importe quel type, y compris, sans limitation, ceux occasionnés par ou découlant de matériel que vous publiez, transmettre, utiliser ou s’appuient sur y compris, mais sans limitation, les vues exprimées dans celui-ci.

> [!div class="step-by-step"]
> [Précédent](shopping-cart.md)
> [Suivant](membership-and-administration.md)
