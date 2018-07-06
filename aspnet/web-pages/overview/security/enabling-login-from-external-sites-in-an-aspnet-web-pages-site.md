---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Connexion à l’aide de Sites externes dans une application Web Pages (Razor) Site | Microsoft Docs
author: tfitzmac
description: Cet article explique comment se connecter à votre site ASP.NET Web Pages (Razor) à l’aide de Facebook, Google, Twitter, Yahoo et autres sites, autrement dit, la prise en charge...
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: e6c5b578b4c74fd04136d31751d51b59ba9197ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825435"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Connexion à l’aide de Sites externes dans un Site ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment se connecter à votre site ASP.NET Web Pages (Razor) à l’aide de Facebook, Google, Twitter, Yahoo et autres sites, autrement dit, la prise en charge OAuth et OpenID dans votre site.
> 
> Ce que vous allez apprendre :
> 
> - Comment activer la connexion à partir d’autres sites lorsque vous utilisez le modèle de WebMatrix Starter Site.
> 
> Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :
> 
> - Le `OAuthWebSecurity` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

Les Pages Web ASP.NET inclut la prise en charge de [OAuth](http://oauth.net/) et [OpenID](http://openid.net/) fournisseurs. À l’aide de ces fournisseurs, vous pouvez laisser les utilisateurs ouvrent votre site à l’aide de leurs informations d’identification existantes à partir de Facebook, Twitter, Microsoft et Google. Par exemple, pour vous connecter à l’aide d’un compte Facebook, les utilisateurs peuvent simplement choisir une icône de Facebook, qui redirige vers la page de connexion Facebook où ils entrent leurs informations de l’utilisateur. Ils peuvent ensuite associer la connexion Facebook avec leur compte sur votre site. Autre amélioration pour les fonctionnalités d’appartenance Web Pages est que vous pouvez associer plusieurs connexions (y compris les connexions à partir de sites de réseau social) avec un seul compte sur votre site Web.

Cette illustration montre la page de connexion à partir de la **Starter Site** modèle, où un utilisateur peut choisir une icône de Facebook, Twitter, Google ou Microsoft pour activer la journalisation avec un compte externe :

![fournisseurs externes](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Vous pouvez activer l’appartenance de OAuth et OpenID en supprimant commentaires quelques lignes de code dans le **Starter Site** modèle. Les méthodes et propriétés vous permet de travailler avec le OAuth et OpenID fournisseurs sont dans le `WebMatrix.Security.OAuthWebSecurity` classe. Le **Starter Site** modèle inclut une infrastructure d’appartenance complète, avec une page de connexion, une base de données d’appartenance et tout le code que vous avez besoin permettre aux utilisateurs de se votre site à l’aide des informations d’identification locales ou ceux à partir d’un autre site .

Cette section fournit un exemple montrant comment permettre aux utilisateurs de se connecter à partir de sites externes à un site qui est basé sur le **Starter Site** modèle. Après avoir créé un site de démarrage, vous effectuez cette (suivi de détails) :

- Pour les sites qui utilisent un fournisseur OAuth (Facebook, Twitter et Microsoft), vous créez une application sur le site externe. Cela vous donne les clés d’application dont vous avez besoin pour appeler la fonctionnalité de connexion pour ces sites.
- Pour les sites qui utilisent un fournisseur OpenID (Google), il est inutile de créer une application. Pour tous ces sites, vous devez disposer un compte pour se connecter et créer des applications de développeur.

    > [!NOTE]
    > Applications Microsoft acceptent uniquement une URL dynamique pour un site Web de travail, vous ne pouvez pas utiliser une URL de site Web local pour le test des connexions.
- Modifier quelques fichiers dans votre site Web afin de spécifier le fournisseur d’authentification approprié et soumettre une connexion vers le site que vous souhaitez utiliser.

Cet article fournit des instructions distinctes pour les tâches suivantes :

- [L’activation de connexions de Google](#To_enable_Google_logins)
- [L’activation de connexions de Facebook](#To_enable_Facebook_logins)
- [L’activation de connexions d’accès Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>L’activation de connexions de Google

1. Créez ou ouvrez un site ASP.NET Web Pages qui est basé sur le modèle de Site de démarrage de WebMatrix.
2. Ouvrez le  *\_AppStart.cshtml* page et supprimez les commentaires de la ligne de code suivante. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Test de connexion Google

1. Exécutez le *default.cshtml* page de votre site et choisissez le **connectez-vous** bouton.
2. Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez une le **Google** ou **Yahoo** bouton Envoyer. Cet exemple utilise la connexion de Google. 

    La page web redirige la requête vers la page de connexion de Google.

    ![Connexion Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Entrez les informations d’identification pour un compte Google existant.
4. Si Google vous demande si vous souhaitez autoriser *Localhost* pour utiliser les informations relatives au compte, cliquez sur **autoriser**.

    Le code utilise le jeton de Google pour authentifier l’utilisateur et retourne ensuite à cette page sur votre site Web. Cette page permet aux utilisateurs d’associer leur connexion Google avec un compte existant sur votre site Web, ou ils peuvent inscrire un nouveau compte sur votre site à associer la connexion externe.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Choisissez le **associer** bouton. Le navigateur revient à la page d’accueil de votre application.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>L’activation de connexions de Facebook

1. Accédez à la [site de développeurs Facebook](https://developers.facebook.com/apps) (se connecter que si vous n’êtes pas déjà connecté).
2. Choisissez le **créer une application** bouton, puis suivez les invites pour nommer et créer l’application.
3. Dans la section **sélectionner comment votre application s’intègre avec Facebook**, choisissez le **site Web** section.
4. Renseignez le **URL du Site** champ avec l’URL de votre site (par exemple, `http://www.example.com`). Le **domaine** champ est facultatif ; vous pouvez l’utiliser pour l’authentification d’un domaine entier (tel que *example.com*). 

    > [!NOTE]
    > Si vous exécutez un site sur votre ordinateur local avec une URL comme `http://localhost:12345` (où le nombre est un numéro de port local), vous pouvez ajouter cette valeur pour le **URL du Site** champ pour tester votre site. Toutefois, chaque fois que le numéro de port de vos modifications de site local, vous devez mettre à jour le **URL du Site** champ de votre application.
5. Choisissez le **enregistrer les modifications** bouton.
6. Choisissez le **applications** onglet à nouveau et afficher la page de démarrage pour votre application.
7. Copie le **ID d’application** et **Secret d’application** valeurs pour votre application et collez-les dans un fichier texte temporaire. Vous transmettez ces valeurs pour le fournisseur de Facebook dans votre code de site Web.
8. Quitter le site des développeurs Facebook.

Maintenant vous apportez des modifications à deux pages dans votre site Web afin que les utilisateurs seront en mesure de vous connecter au site à l’aide de leur compte Facebook.

1. Créez ou ouvrez un site ASP.NET Web Pages qui est basé sur le modèle de Site de démarrage de WebMatrix.
2. Ouvrez le  *\_AppStart.cshtml* page et les commentaires du code pour le fournisseur Facebook OAuth. Le bloc de code sans commentaire se présente comme suit : 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copie le **ID d’application** valeur à partir de l’application Facebook en tant que la valeur de la `appId` paramètre (à l’intérieur des guillemets).
4. Copie **Secret d’application** valeur à partir de l’application Facebook en tant que le `appSecret` valeur du paramètre.
5. Enregistrez et fermez le fichier.

### <a name="testing-facebook-login"></a>Test de connexion Facebook

1. Exécuter le site *default.cshtml* page et choisissez le **connexion** bouton.
2. Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez le **Facebook** icône. 

    La page web redirige la requête vers la page de connexion Facebook.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Connectez-vous à un compte Facebook. 

    Le code utilise le jeton Facebook afin de vous authentifier et renvoie à une page où vous pouvez associer votre connexion Facebook avec le compte de connexion de votre site. Votre utilisateur nom ou adresse e-mail est rempli dans le **E-mail** champ sur le formulaire.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Choisissez le **associer** bouton. 

    Le navigateur revient à la page d’accueil et que vous êtes connecté.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>L’activation de connexions d’accès Twitter

1. Accédez à la [site des développeurs Twitter](https://dev.twitter.com/).
2. Choisissez le **créer une application** lier et ouvrir une session sur le site.
3. Sur le **créer une Application** forment, renseignez le **nom** et **Description** champs.
4. Dans le **site Web** , entrez l’URL de votre site (par exemple, `http://www.example.com`). 

    > [!NOTE]
    > Si vous testez votre site localement (à l’aide d’une URL telle que `http://localhost:12345`), Twitter ne peut-être pas accepter l’URL. Toutefois, vous pourrez peut-être utiliser l’adresse IP de bouclage local (par exemple `http://127.0.0.1:12345`). Cela simplifie le processus de test de votre application localement. Toutefois, chaque fois que le numéro de port de votre site local change, vous devez mettre à jour le **site Web** champ de votre application.
5. Dans le **URL de rappel** , entrez une URL pour la page dans votre site Web que vous souhaitez que les utilisateurs pour revenir à une fois la connexion à Twitter. Par exemple, pour envoyer les utilisateurs à la page d’accueil du Site Starter (qui reconnaîtra leur état connecté), entrez la même URL que vous avez entré dans le **site Web** champ.
6. Acceptez les termes du contrat et choisissez le **créer votre application Twitter** bouton.
7. Sur le **Mes Applications** d’accueil de page, choisissez l’application que vous avez créé.
8. Sur le **détails** onglet, faites défiler vers le bas et choisissez le **créer mon jeton d’accès** bouton.
9. Sur le **détails** onglet, copiez la **clé de consommateur** et **Secret de consommateur** valeurs pour votre application et collez-les dans un fichier texte temporaire. Vous transmettrez ces valeurs pour le fournisseur Twitter dans votre code de site Web.
10. Quitter le site Twitter.

Maintenant vous apportez des modifications à deux pages dans votre site Web afin que les utilisateurs pourront se connecter au site à l’aide de leur compte Twitter.

1. Créez ou ouvrez un site ASP.NET Web Pages qui est basé sur le modèle de Site de démarrage de WebMatrix.
2. Ouvrez le  *\_AppStart.cshtml* page et les commentaires du code pour le fournisseur OAuth pour Twitter. Le bloc de code sans commentaire ressemble à ceci : 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copie le **clé de consommateur** valeur à partir de l’application Twitter en tant que la valeur de la `consumerKey` paramètre (à l’intérieur des guillemets).
4. Copie le **Secret de consommateur** valeur à partir de l’application Twitter en tant que la valeur de la `consumerSecret` paramètre.
5. Enregistrez et fermez le fichier.

### <a name="testing-twitter-login"></a>Test de connexion à Twitter

1. Exécutez le *default.cshtml* page de votre site et choisissez le **connexion** bouton.
2. Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez le **Twitter** icône. 

    La page web redirige la demande vers une page de connexion Twitter pour l’application que vous avez créé.

    ![Oauth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Connectez-vous à un compte Twitter.
4. Le code utilise le jeton Twitter pour authentifier l’utilisateur et puis vous redirige vers une page où vous pouvez associer votre connexion avec votre compte de site Web. Votre nom ou adresse de messagerie est renseigné dans le **E-mail** champ sur le formulaire.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Choisissez le **associer** bouton. 

    Le navigateur revient à la page d’accueil et que vous êtes connecté.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


- [Personnalisation du comportement à l’échelle du site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Ajout de sécurité et l’appartenance à un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202904)
