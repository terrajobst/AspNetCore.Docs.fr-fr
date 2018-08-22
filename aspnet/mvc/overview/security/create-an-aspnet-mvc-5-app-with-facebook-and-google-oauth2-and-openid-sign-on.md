---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Créer de MVC 5 application avec Facebook, Twitter, LinkedIn et Google OAuth2 Sign-on (c#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec les informations d’identification à partir d’un authentifier externe...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 330cb290668ae951e822b95990ed92100b790cd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834626"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Créer une application ASP.NET MVC 5 avec Facebook, Twitter, LinkedIn et Google OAuth2 Sign-on (c#)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide de [OAuth 2.0](http://oauth.net/2/) avec informations d’identification à partir d’un fournisseur d’authentification externes, tels que Facebook, Twitter, LinkedIn, Microsoft ou Google. Par souci de simplicité, ce didacticiel se concentre sur l’utilisation des informations d’identification à partir de Facebook et Google.
> 
> L’activation de ces informations d’identification dans vos sites web de fournit un avantage significatif, car des millions d’utilisateurs disposent déjà de comptes avec ces fournisseurs externes. Ces utilisateurs peuvent être plus de chances de s’inscrire à votre site si elles n’ont pas à créer et à mémoriser un nouvel ensemble d’informations d’identification.
> 
> Voir aussi [application ASP.NET MVC 5 avec SMS et e-mail d’authentification à deux facteurs](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Le didacticiel montre également comment ajouter des données de profil pour l’utilisateur et comment utiliser l’API d’appartenance pour ajouter des rôles. Ce didacticiel a été écrit par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Veuillez me suivre sur Twitter : [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Prise en main

Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installer Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure. Pour l’aide auprès de Dropbox, GitHub, Linkedin, Instagram, mémoire tampon, Salesforce, vapeur, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! et bien plus encore, consultez ce [exemple de projet](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Vous devez installer Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure pour utiliser Google OAuth 2 et déboguer localement sans avertissements de SSL.


Cliquez sur **nouveau projet** à partir de la **Démarrer** page, ou vous pouvez utiliser le menu et sélectionnez **fichier**, puis **nouveau projet**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Créer votre première Application

Cliquez sur **nouveau projet**, puis sélectionnez **Visual C#** sur la gauche, puis **Web** , puis sélectionnez **Application Web ASP.NET**. Nommez votre projet « MvcAuth », puis **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Dans le **nouveau projet ASP.NET** boîte de dialogue, cliquez sur **MVC**. Si l’authentification n’est pas **comptes d’utilisateur individuels**, cliquez sur le **modifier l’authentification** bouton et sélectionnez **comptes d’utilisateur individuels**. En vérifiant **hôte dans le cloud**, l’application sera très facile à héberger dans Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Si vous avez sélectionné **hôte dans le cloud**, renseignez la boîte de dialogue Configurer.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Utilisez NuGet pour mettre à jour vers la dernière intergiciel (middleware) OWIN

Utilisez le Gestionnaire de package NuGet pour mettre à jour le [intergiciel (middleware) OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Sélectionnez **mises à jour** dans le menu de gauche. Vous pouvez cliquer sur le **tout mettre à jour** bouton ou vous pouvez rechercher uniquement les packages OWIN (illustrés dans l’image suivante) :

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Dans l’image ci-dessous, seuls les packages OWIN sont affichés :

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

À partir de la Manager Console (package), vous pouvez entrer la `Update-Package` commande qui met à jour tous les packages.

Appuyez sur **F5** ou **Ctrl + F5** pour exécuter l’application. Dans l’image ci-dessous, le numéro de port est 1234. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

Selon la taille de la fenêtre du navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher le **accueil**, **sur**, **Contact**, **inscrire**et **connectez-vous** des liens.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configuration de SSL dans le projet

Pour vous connecter aux fournisseurs d’authentification tels que Google et Facebook, vous devez configurer IIS Express pour utiliser SSL. Il est important de garder à l’aide de SSL une fois la connexion et pas retomber sur HTTP, votre cookie de connexion est simplement en tant que secret en tant que votre nom d’utilisateur et le mot de passe et sans utiliser SSL que vous l’envoyez en texte clair sur le réseau. En outre, vous avez déjà pris le temps d’effectuer la négociation et de sécuriser le canal (c'est-à-dire la majeure partie de ce qui rend HTTPS plus lent que HTTP) avant l’exécution, le pipeline MVC donc redirection vers HTTP une fois que vous êtes connecté ne rendre la requête actuelle ou le futur demandes beaucoup plus rapides.

1. Dans **l’Explorateur de solutions**, cliquez sur le **MvcAuth** projet.
2. Appuyez sur la touche F4 pour afficher les propriétés du projet. Vous pouvez également, à partir de la **vue** menu que vous pouvez sélectionner **fenêtre Propriétés**.
3. Modification **SSL activé** sur True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copiez l’URL SSL (qui sera `https://localhost:44300/` , sauf si vous avez créé d’autres projets SSL).
5. Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le **MvcAuth** de projet et sélectionnez **propriétés**.
6. Sélectionnez le **Web** onglet et collez l’URL SSL dans le **Url du projet** boîte. Enregistrez le fichier (CTRL + S). Vous aurez besoin de cette URL à configurer des applications de l’authentification Facebook et Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Ajouter le [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribut le `Home` contrôleur pour toutes les demandes doit utiliser HTTPS. Une approche plus sécurisée consiste à ajouter le [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtre à l’application. Consultez la section &quot;protéger l’Application avec SSL et l’attribut autoriser&quot; dans mon tutoral [créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer sur Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Vous trouverez ci-dessous une partie du contrôleur Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Appuyez sur CTRL+F5 pour exécuter l'application. Si vous avez installé le certificat dans le passé, vous pouvez ignorer le reste de cette section et passer à [création d’une application Google pour oauth2 et de connexion de l’application au projet](#goog), dans le cas contraire, suivez les instructions pour approuver l’auto-signé certificat IIS Express a généré.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lire le **avertissement de sécurité** boîte de dialogue, puis cliquez sur **Oui** si vous souhaitez installer le certificat représentant localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE affiche la *accueil* page et aucun avertissement SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. En outre, Google Chrome accepte le certificat et affiche le contenu HTTPS sans avertissement. Firefox utilise son propre magasin de certificats, par conséquent, il contient un avertissement. Pour notre application vous pouvez cliquer en toute sécurité sur **conscient des risques de**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Création d’une application Google pour oauth2 et de connexion de l’application au projet

> [!WARNING]
> Pour obtenir des instructions de Google OAuth actuelles, consultez [Google de configuration de l’authentification dans ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Accédez à la [Google Developers Console](https://console.developers.google.com/).
2. Si vous n’avez pas créé un projet avant, sélectionnez **informations d’identification** dans l’onglet gauche, puis sélectionnez **créer**.
3. Dans l’onglet gauche, cliquez sur **informations d’identification**.
4. Cliquez sur **créer les informations d’identification** puis **ID client OAuth**. 

    1. Dans le **créer un identifiant Client** boîte de dialogue, conservez la valeur par défaut **application Web** pour le type d’application.
    2. Définir le **JavaScript autorisé** origines à l’URL SSL que vous avez utilisé ci-dessus (`https://localhost:44300/` , sauf si vous avez créé d’autres projets SSL)
    3. Définir le **URI de redirection autorisés** à :  
         `https://localhost:44300/signin-google`
5. Cliquez sur l’élément de menu de consentement OAuth écran, puis définissez votre nom de produit et l’adresse de messagerie. Lorsque vous avez complété le formulaire, cliquez **enregistrer**.
6. Cliquez sur l’élément de menu de bibliothèque, rechercher **API Google +**, cliquez dessus, puis appuyez sur Activer.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   L’image ci-dessous montre les API est activées.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. À partir du Gestionnaire d’API Google API, visitez le **informations d’identification** tab pour obtenir le **ID Client**. Téléchargement pour enregistrer un fichier JSON avec des secrets d’application. Copiez et collez le **ClientId** et **ClientSecret** dans le `UseGoogleAuthentication` méthode trouvée dans le *Startup.Auth.cs* de fichiers dans le *App_Start* dossier. Le **ClientId** et **ClientSecret** valeurs indiquées ci-dessous sont des exemples et ne fonctionnent pas.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont ajoutées au code ci-dessus pour que l’exemple reste simple. Consultez [meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Appuyez sur **CTRL + F5** pour générer et exécuter l’application. Cliquez sur le **connectez-vous** lien.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Sous **utiliser un autre service pour vous connecter**, cliquez sur **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Si vous omettez une des étapes ci-dessus, vous obtiendrez une erreur HTTP 401. Vérifiez à nouveau les étapes ci-dessus. Si vous omettez un paramètre requis (par exemple **nom de produit**), ajoutez l’élément manquant et enregistrer ; peut prendre quelques minutes pour l’authentification fonctionne.
10. Vous êtes redirigé vers le site Google où vous entrez vos informations d’identification.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Après avoir entré vos informations d’identification, vous devrez accorder des autorisations à l’application web que vous venez de créer :
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Cliquez sur **accepter**. Vous allez maintenant être redirigé vers le **inscrire** page de l’application MvcAuth où vous pouvez inscrire votre compte Google. Vous avez la possibilité de modifier le nom d’inscription local utilisé pour votre compte Gmail, mais il est généralement conseillé de conserver l’alias de messagerie électronique par défaut (autrement dit, celui utilisé pour l’authentification). Cliquez sur **Register**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Création de l’application dans Facebook et la connexion de l’application au projet

> [!WARNING]
> Pour obtenir des instructions de l’authentification Facebook OAuth2 actuelles, consultez [l’authentification Facebook de configuration](/aspnet/core/security/authentication/social/facebook-logins)


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examiner les données d’appartenance

Dans le **vue** menu, cliquez sur **Explorateur de serveurs**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Développez **DefaultConnection (MvcAuth)**, développez **Tables**, avec le bouton droit cliquez sur **AspNetUsers** et cliquez sur **afficher les données de Table**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![données de la table aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Ajout de données de profil à la classe d’utilisateur

Dans cette section vous allez ajouter une date de naissance et ville natale aux données utilisateur pendant l’inscription, comme illustré dans l’image suivante.

![reg avec ville natale et Anniv.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Ouvrez le *Models\IdentityModels.cs* fichier, puis ajoutez les propriétés de ville accueil et de la date de naissance :

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Ouvrez le *Models\AccountViewModels.cs* fichier et l’ensemble de date et accueil des propriétés de ville dans naissance `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Ouvrez le *controllers\accountcontroller.cs* fichier, puis ajoutez le code pour la ville de date et d’accueil de naissance dans la `ExternalLoginConfirmation` méthode d’action comme illustré :

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Ajouter la date de naissance et ville natale à la *Views\Account\ExternalLoginConfirmation.cshtml* fichier :

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Supprimer la base de données d’appartenance afin de pouvoir à nouveau enregistrer votre compte Facebook avec votre application et vérifiez que vous pouvez ajouter la nouvelle date de naissance et les informations de profil de ville natale.

À partir de **l’Explorateur de solutions**, cliquez sur le **afficher tous les fichiers** icône, puis clic droit *ajouter\_Data\aspnet-MvcAuth -&lt;date&gt;.mdf* et cliquez sur **supprimer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet**, puis cliquez sur **Console du Gestionnaire de Package** (PMC). Entrez les commandes suivantes dans PMC.

1. Enable-Migrations
2. Add-Migration Init
3. Mise à jour la base de données

Exécutez l’application et utiliser FaceBook et Google pour vous connecter et inscrire certains utilisateurs.

## <a name="examine-the-membership-data"></a>Examiner les données d’appartenance

Dans le **vue** menu, cliquez sur **Explorateur de serveurs**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Bouton droit sur **AspNetUsers** et cliquez sur **afficher les données de Table**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Le `HomeTown` et `BirthDate` champs sont affichés ci-dessous.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Déconnecter votre application, puis connectez-vous avec un autre compte

Si vous ouvrez une session sur votre application avec Facebook et puis déconnectez-vous et réessayez de vous connecter à nouveau avec un autre compte Facebook (à l’aide du même navigateur), vous serez immédiatement connecté pour le compte Facebook précédent que vous avez utilisé. Pour utiliser un autre compte, vous devez accéder à Facebook et se déconnecter à Facebook. La même règle s’applique à n’importe quel autre 3ème partie fournisseur d’authentification. Vous pouvez également vous connecter avec un autre compte à l’aide d’un autre navigateur.

## <a name="next-steps"></a>Étapes suivantes

Consultez [présentation les fournisseurs de sécurité Yahoo et LinkedIn OAuth pour OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) par Jerrie Pelser pour obtenir des instructions Yahoo et LinkedIn. Consultez de Jerrie presque les boutons de connexion de réseau social pour ASP.NET MVC 5 obtenir des boutons de connexion de réseau social enable.

Suivre me didacticiel [créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer sur Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), qui continue de ce didacticiel et affiche les informations suivantes :

1. Comment déployer votre application dans Azure.
2. Guide pratique pour sécuriser votre application avec des rôles.
3. Comment sécuriser votre application avec le [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) et [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtres.
4. Comment utiliser l’API d’appartenance pour ajouter des utilisateurs et des rôles.

Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer. Vous pouvez également demander de nouvelles rubriques à [afficher les leçons de Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Vous pouvez même demander et voter sur les nouvelles fonctionnalités à ajouter à ASP.NET. Par exemple, vous pouvez voter pour un outil à [créer et gérer des utilisateurs et des rôles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Pour une bonne explication du fonctionnement des Services d’authentification externe ASP.NET, consultez de Robert McMurray [des Services d’authentification externe](https://asp.net/web-api/overview/security/external-authentication-services). L’article de Robert prendra également en détail dans l’activation de l’authentification de Microsoft et Twitter. Tom Dykstra de [didacticiel EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) montre comment travailler avec Entity Framework.
