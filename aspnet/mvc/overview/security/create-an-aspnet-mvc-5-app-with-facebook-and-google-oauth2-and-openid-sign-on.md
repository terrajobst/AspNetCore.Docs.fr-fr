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
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="0ae60-103">Créer une application ASP.NET MVC 5 avec Facebook, Twitter, LinkedIn et Google OAuth2 Sign-on (c#)</span><span class="sxs-lookup"><span data-stu-id="0ae60-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="0ae60-104">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0ae60-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="0ae60-105">Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide de [OAuth 2.0](http://oauth.net/2/) avec informations d’identification à partir d’un fournisseur d’authentification externes, tels que Facebook, Twitter, LinkedIn, Microsoft ou Google.</span><span class="sxs-lookup"><span data-stu-id="0ae60-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="0ae60-106">Par souci de simplicité, ce didacticiel se concentre sur l’utilisation des informations d’identification à partir de Facebook et Google.</span><span class="sxs-lookup"><span data-stu-id="0ae60-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="0ae60-107">L’activation de ces informations d’identification dans vos sites web de fournit un avantage significatif, car des millions d’utilisateurs disposent déjà de comptes avec ces fournisseurs externes.</span><span class="sxs-lookup"><span data-stu-id="0ae60-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="0ae60-108">Ces utilisateurs peuvent être plus de chances de s’inscrire à votre site si elles n’ont pas à créer et à mémoriser un nouvel ensemble d’informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="0ae60-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="0ae60-109">Voir aussi [application ASP.NET MVC 5 avec SMS et e-mail d’authentification à deux facteurs](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="0ae60-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="0ae60-110">Le didacticiel montre également comment ajouter des données de profil pour l’utilisateur et comment utiliser l’API d’appartenance pour ajouter des rôles.</span><span class="sxs-lookup"><span data-stu-id="0ae60-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="0ae60-111">Ce didacticiel a été écrit par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Veuillez me suivre sur Twitter : [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="0ae60-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="0ae60-112">Prise en main</span><span class="sxs-lookup"><span data-stu-id="0ae60-112">Getting Started</span></span>

<span data-ttu-id="0ae60-113">Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="0ae60-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="0ae60-114">Installer Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="0ae60-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="0ae60-115">Pour l’aide auprès de Dropbox, GitHub, Linkedin, Instagram, mémoire tampon, Salesforce, vapeur, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! et bien plus encore, consultez ce [exemple de projet](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="0ae60-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="0ae60-116">Vous devez installer Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure pour utiliser Google OAuth 2 et déboguer localement sans avertissements de SSL.</span><span class="sxs-lookup"><span data-stu-id="0ae60-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="0ae60-117">Cliquez sur **nouveau projet** à partir de la **Démarrer** page, ou vous pouvez utiliser le menu et sélectionnez **fichier**, puis **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="0ae60-118">Créer votre première Application</span><span class="sxs-lookup"><span data-stu-id="0ae60-118">Creating Your First Application</span></span>

<span data-ttu-id="0ae60-119">Cliquez sur **nouveau projet**, puis sélectionnez **Visual C#** sur la gauche, puis **Web** , puis sélectionnez **Application Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="0ae60-120">Nommez votre projet « MvcAuth », puis **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="0ae60-121">Dans le **nouveau projet ASP.NET** boîte de dialogue, cliquez sur **MVC**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="0ae60-122">Si l’authentification n’est pas **comptes d’utilisateur individuels**, cliquez sur le **modifier l’authentification** bouton et sélectionnez **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="0ae60-123">En vérifiant **hôte dans le cloud**, l’application sera très facile à héberger dans Azure.</span><span class="sxs-lookup"><span data-stu-id="0ae60-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="0ae60-124">Si vous avez sélectionné **hôte dans le cloud**, renseignez la boîte de dialogue Configurer.</span><span class="sxs-lookup"><span data-stu-id="0ae60-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="0ae60-125">Utilisez NuGet pour mettre à jour vers la dernière intergiciel (middleware) OWIN</span><span class="sxs-lookup"><span data-stu-id="0ae60-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="0ae60-126">Utilisez le Gestionnaire de package NuGet pour mettre à jour le [intergiciel (middleware) OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="0ae60-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="0ae60-127">Sélectionnez **mises à jour** dans le menu de gauche.</span><span class="sxs-lookup"><span data-stu-id="0ae60-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="0ae60-128">Vous pouvez cliquer sur le **tout mettre à jour** bouton ou vous pouvez rechercher uniquement les packages OWIN (illustrés dans l’image suivante) :</span><span class="sxs-lookup"><span data-stu-id="0ae60-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="0ae60-129">Dans l’image ci-dessous, seuls les packages OWIN sont affichés :</span><span class="sxs-lookup"><span data-stu-id="0ae60-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="0ae60-130">À partir de la Manager Console (package), vous pouvez entrer la `Update-Package` commande qui met à jour tous les packages.</span><span class="sxs-lookup"><span data-stu-id="0ae60-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="0ae60-131">Appuyez sur **F5** ou **Ctrl + F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="0ae60-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="0ae60-132">Dans l’image ci-dessous, le numéro de port est 1234.</span><span class="sxs-lookup"><span data-stu-id="0ae60-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="0ae60-133">Lorsque vous exécutez l’application, vous verrez un numéro de port différent.</span><span class="sxs-lookup"><span data-stu-id="0ae60-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="0ae60-134">Selon la taille de la fenêtre du navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher le **accueil**, **sur**, **Contact**, **inscrire**et **connectez-vous** des liens.</span><span class="sxs-lookup"><span data-stu-id="0ae60-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="0ae60-135">Configuration de SSL dans le projet</span><span class="sxs-lookup"><span data-stu-id="0ae60-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="0ae60-136">Pour vous connecter aux fournisseurs d’authentification tels que Google et Facebook, vous devez configurer IIS Express pour utiliser SSL.</span><span class="sxs-lookup"><span data-stu-id="0ae60-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="0ae60-137">Il est important de garder à l’aide de SSL une fois la connexion et pas retomber sur HTTP, votre cookie de connexion est simplement en tant que secret en tant que votre nom d’utilisateur et le mot de passe et sans utiliser SSL que vous l’envoyez en texte clair sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="0ae60-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="0ae60-138">En outre, vous avez déjà pris le temps d’effectuer la négociation et de sécuriser le canal (c'est-à-dire la majeure partie de ce qui rend HTTPS plus lent que HTTP) avant l’exécution, le pipeline MVC donc redirection vers HTTP une fois que vous êtes connecté ne rendre la requête actuelle ou le futur demandes beaucoup plus rapides.</span><span class="sxs-lookup"><span data-stu-id="0ae60-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="0ae60-139">Dans **l’Explorateur de solutions**, cliquez sur le **MvcAuth** projet.</span><span class="sxs-lookup"><span data-stu-id="0ae60-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="0ae60-140">Appuyez sur la touche F4 pour afficher les propriétés du projet.</span><span class="sxs-lookup"><span data-stu-id="0ae60-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="0ae60-141">Vous pouvez également, à partir de la **vue** menu que vous pouvez sélectionner **fenêtre Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="0ae60-142">Modification **SSL activé** sur True.</span><span class="sxs-lookup"><span data-stu-id="0ae60-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="0ae60-143">Copiez l’URL SSL (qui sera `https://localhost:44300/` , sauf si vous avez créé d’autres projets SSL).</span><span class="sxs-lookup"><span data-stu-id="0ae60-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="0ae60-144">Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le **MvcAuth** de projet et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="0ae60-145">Sélectionnez le **Web** onglet et collez l’URL SSL dans le **Url du projet** boîte.</span><span class="sxs-lookup"><span data-stu-id="0ae60-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="0ae60-146">Enregistrez le fichier (CTRL + S).</span><span class="sxs-lookup"><span data-stu-id="0ae60-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="0ae60-147">Vous aurez besoin de cette URL à configurer des applications de l’authentification Facebook et Google.</span><span class="sxs-lookup"><span data-stu-id="0ae60-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="0ae60-148">Ajouter le [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribut le `Home` contrôleur pour toutes les demandes doit utiliser HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0ae60-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="0ae60-149">Une approche plus sécurisée consiste à ajouter le [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtre à l’application.</span><span class="sxs-lookup"><span data-stu-id="0ae60-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="0ae60-150">Consultez la section &quot;protéger l’Application avec SSL et l’attribut autoriser&quot; dans mon tutoral [créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer sur Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="0ae60-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="0ae60-151">Vous trouverez ci-dessous une partie du contrôleur Home.</span><span class="sxs-lookup"><span data-stu-id="0ae60-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="0ae60-152">Appuyez sur CTRL+F5 pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="0ae60-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="0ae60-153">Si vous avez installé le certificat dans le passé, vous pouvez ignorer le reste de cette section et passer à [création d’une application Google pour oauth2 et de connexion de l’application au projet](#goog), dans le cas contraire, suivez les instructions pour approuver l’auto-signé certificat IIS Express a généré.</span><span class="sxs-lookup"><span data-stu-id="0ae60-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="0ae60-154">Lire le **avertissement de sécurité** boîte de dialogue, puis cliquez sur **Oui** si vous souhaitez installer le certificat représentant localhost.</span><span class="sxs-lookup"><span data-stu-id="0ae60-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="0ae60-155">IE affiche la *accueil* page et aucun avertissement SSL.</span><span class="sxs-lookup"><span data-stu-id="0ae60-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="0ae60-156">En outre, Google Chrome accepte le certificat et affiche le contenu HTTPS sans avertissement.</span><span class="sxs-lookup"><span data-stu-id="0ae60-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="0ae60-157">Firefox utilise son propre magasin de certificats, par conséquent, il contient un avertissement.</span><span class="sxs-lookup"><span data-stu-id="0ae60-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="0ae60-158">Pour notre application vous pouvez cliquer en toute sécurité sur **conscient des risques de**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="0ae60-159">Création d’une application Google pour oauth2 et de connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="0ae60-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="0ae60-160">Pour obtenir des instructions de Google OAuth actuelles, consultez [Google de configuration de l’authentification dans ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="0ae60-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="0ae60-161">Accédez à la [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="0ae60-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="0ae60-162">Si vous n’avez pas créé un projet avant, sélectionnez **informations d’identification** dans l’onglet gauche, puis sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="0ae60-163">Dans l’onglet gauche, cliquez sur **informations d’identification**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="0ae60-164">Cliquez sur **créer les informations d’identification** puis **ID client OAuth**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="0ae60-165">Dans le **créer un identifiant Client** boîte de dialogue, conservez la valeur par défaut **application Web** pour le type d’application.</span><span class="sxs-lookup"><span data-stu-id="0ae60-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="0ae60-166">Définir le **JavaScript autorisé** origines à l’URL SSL que vous avez utilisé ci-dessus (`https://localhost:44300/` , sauf si vous avez créé d’autres projets SSL)</span><span class="sxs-lookup"><span data-stu-id="0ae60-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="0ae60-167">Définir le **URI de redirection autorisés** à :</span><span class="sxs-lookup"><span data-stu-id="0ae60-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="0ae60-168">Cliquez sur l’élément de menu de consentement OAuth écran, puis définissez votre nom de produit et l’adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="0ae60-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="0ae60-169">Lorsque vous avez complété le formulaire, cliquez **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="0ae60-170">Cliquez sur l’élément de menu de bibliothèque, rechercher **API Google +**, cliquez dessus, puis appuyez sur Activer.</span><span class="sxs-lookup"><span data-stu-id="0ae60-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="0ae60-171">L’image ci-dessous montre les API est activées.</span><span class="sxs-lookup"><span data-stu-id="0ae60-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="0ae60-172">À partir du Gestionnaire d’API Google API, visitez le **informations d’identification** tab pour obtenir le **ID Client**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="0ae60-173">Téléchargement pour enregistrer un fichier JSON avec des secrets d’application.</span><span class="sxs-lookup"><span data-stu-id="0ae60-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="0ae60-174">Copiez et collez le **ClientId** et **ClientSecret** dans le `UseGoogleAuthentication` méthode trouvée dans le *Startup.Auth.cs* de fichiers dans le *App_Start* dossier.</span><span class="sxs-lookup"><span data-stu-id="0ae60-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="0ae60-175">Le **ClientId** et **ClientSecret** valeurs indiquées ci-dessous sont des exemples et ne fonctionnent pas.</span><span class="sxs-lookup"><span data-stu-id="0ae60-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="0ae60-176">Sécurité - Ne jamais stocker de données sensibles dans votre code source.</span><span class="sxs-lookup"><span data-stu-id="0ae60-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="0ae60-177">Le compte et les informations d’identification sont ajoutées au code ci-dessus pour que l’exemple reste simple.</span><span class="sxs-lookup"><span data-stu-id="0ae60-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="0ae60-178">Consultez [meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="0ae60-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="0ae60-179">Appuyez sur **CTRL + F5** pour générer et exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="0ae60-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="0ae60-180">Cliquez sur le **connectez-vous** lien.</span><span class="sxs-lookup"><span data-stu-id="0ae60-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="0ae60-181">Sous **utiliser un autre service pour vous connecter**, cliquez sur **Google**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="0ae60-182">Si vous omettez une des étapes ci-dessus, vous obtiendrez une erreur HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="0ae60-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="0ae60-183">Vérifiez à nouveau les étapes ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="0ae60-183">Recheck your steps above.</span></span> <span data-ttu-id="0ae60-184">Si vous omettez un paramètre requis (par exemple **nom de produit**), ajoutez l’élément manquant et enregistrer ; peut prendre quelques minutes pour l’authentification fonctionne.</span><span class="sxs-lookup"><span data-stu-id="0ae60-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="0ae60-185">Vous êtes redirigé vers le site Google où vous entrez vos informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="0ae60-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="0ae60-186">Après avoir entré vos informations d’identification, vous devrez accorder des autorisations à l’application web que vous venez de créer :</span><span class="sxs-lookup"><span data-stu-id="0ae60-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="0ae60-187">Cliquez sur **accepter**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-187">Click **Accept**.</span></span> <span data-ttu-id="0ae60-188">Vous allez maintenant être redirigé vers le **inscrire** page de l’application MvcAuth où vous pouvez inscrire votre compte Google.</span><span class="sxs-lookup"><span data-stu-id="0ae60-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="0ae60-189">Vous avez la possibilité de modifier le nom d’inscription local utilisé pour votre compte Gmail, mais il est généralement conseillé de conserver l’alias de messagerie électronique par défaut (autrement dit, celui utilisé pour l’authentification).</span><span class="sxs-lookup"><span data-stu-id="0ae60-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="0ae60-190">Cliquez sur **Register**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="0ae60-191">Création de l’application dans Facebook et la connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="0ae60-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="0ae60-192">Pour obtenir des instructions de l’authentification Facebook OAuth2 actuelles, consultez [l’authentification Facebook de configuration](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="0ae60-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="0ae60-193">Examiner les données d’appartenance</span><span class="sxs-lookup"><span data-stu-id="0ae60-193">Examine the Membership Data</span></span>

<span data-ttu-id="0ae60-194">Dans le **vue** menu, cliquez sur **Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="0ae60-195">Développez **DefaultConnection (MvcAuth)**, développez **Tables**, avec le bouton droit cliquez sur **AspNetUsers** et cliquez sur **afficher les données de Table**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![données de la table aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="0ae60-197">Ajout de données de profil à la classe d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="0ae60-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="0ae60-198">Dans cette section vous allez ajouter une date de naissance et ville natale aux données utilisateur pendant l’inscription, comme illustré dans l’image suivante.</span><span class="sxs-lookup"><span data-stu-id="0ae60-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg avec ville natale et Anniv.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="0ae60-200">Ouvrez le *Models\IdentityModels.cs* fichier, puis ajoutez les propriétés de ville accueil et de la date de naissance :</span><span class="sxs-lookup"><span data-stu-id="0ae60-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="0ae60-201">Ouvrez le *Models\AccountViewModels.cs* fichier et l’ensemble de date et accueil des propriétés de ville dans naissance `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="0ae60-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="0ae60-202">Ouvrez le *controllers\accountcontroller.cs* fichier, puis ajoutez le code pour la ville de date et d’accueil de naissance dans la `ExternalLoginConfirmation` méthode d’action comme illustré :</span><span class="sxs-lookup"><span data-stu-id="0ae60-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="0ae60-203">Ajouter la date de naissance et ville natale à la *Views\Account\ExternalLoginConfirmation.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="0ae60-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="0ae60-204">Supprimer la base de données d’appartenance afin de pouvoir à nouveau enregistrer votre compte Facebook avec votre application et vérifiez que vous pouvez ajouter la nouvelle date de naissance et les informations de profil de ville natale.</span><span class="sxs-lookup"><span data-stu-id="0ae60-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="0ae60-205">À partir de **l’Explorateur de solutions**, cliquez sur le **afficher tous les fichiers** icône, puis clic droit *ajouter\_Data\aspnet-MvcAuth -&lt;date&gt;.mdf* et cliquez sur **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="0ae60-206">À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet**, puis cliquez sur **Console du Gestionnaire de Package** (PMC).</span><span class="sxs-lookup"><span data-stu-id="0ae60-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="0ae60-207">Entrez les commandes suivantes dans PMC.</span><span class="sxs-lookup"><span data-stu-id="0ae60-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="0ae60-208">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="0ae60-208">Enable-Migrations</span></span>
2. <span data-ttu-id="0ae60-209">Add-Migration Init</span><span class="sxs-lookup"><span data-stu-id="0ae60-209">Add-Migration Init</span></span>
3. <span data-ttu-id="0ae60-210">Mise à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="0ae60-210">Update-Database</span></span>

<span data-ttu-id="0ae60-211">Exécutez l’application et utiliser FaceBook et Google pour vous connecter et inscrire certains utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="0ae60-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="0ae60-212">Examiner les données d’appartenance</span><span class="sxs-lookup"><span data-stu-id="0ae60-212">Examine the Membership Data</span></span>

<span data-ttu-id="0ae60-213">Dans le **vue** menu, cliquez sur **Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="0ae60-214">Bouton droit sur **AspNetUsers** et cliquez sur **afficher les données de Table**.</span><span class="sxs-lookup"><span data-stu-id="0ae60-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="0ae60-215">Le `HomeTown` et `BirthDate` champs sont affichés ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0ae60-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="0ae60-216">Déconnecter votre application, puis connectez-vous avec un autre compte</span><span class="sxs-lookup"><span data-stu-id="0ae60-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="0ae60-217">Si vous ouvrez une session sur votre application avec Facebook et puis déconnectez-vous et réessayez de vous connecter à nouveau avec un autre compte Facebook (à l’aide du même navigateur), vous serez immédiatement connecté pour le compte Facebook précédent que vous avez utilisé.</span><span class="sxs-lookup"><span data-stu-id="0ae60-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="0ae60-218">Pour utiliser un autre compte, vous devez accéder à Facebook et se déconnecter à Facebook.</span><span class="sxs-lookup"><span data-stu-id="0ae60-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="0ae60-219">La même règle s’applique à n’importe quel autre 3ème partie fournisseur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="0ae60-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="0ae60-220">Vous pouvez également vous connecter avec un autre compte à l’aide d’un autre navigateur.</span><span class="sxs-lookup"><span data-stu-id="0ae60-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ae60-221">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="0ae60-221">Next Steps</span></span>

<span data-ttu-id="0ae60-222">Consultez [présentation les fournisseurs de sécurité Yahoo et LinkedIn OAuth pour OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) par Jerrie Pelser pour obtenir des instructions Yahoo et LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="0ae60-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="0ae60-223">Consultez de Jerrie presque les boutons de connexion de réseau social pour ASP.NET MVC 5 obtenir des boutons de connexion de réseau social enable.</span><span class="sxs-lookup"><span data-stu-id="0ae60-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="0ae60-224">Suivre me didacticiel [créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer sur Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), qui continue de ce didacticiel et affiche les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="0ae60-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="0ae60-225">Comment déployer votre application dans Azure.</span><span class="sxs-lookup"><span data-stu-id="0ae60-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="0ae60-226">Guide pratique pour sécuriser votre application avec des rôles.</span><span class="sxs-lookup"><span data-stu-id="0ae60-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="0ae60-227">Comment sécuriser votre application avec le [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) et [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtres.</span><span class="sxs-lookup"><span data-stu-id="0ae60-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="0ae60-228">Comment utiliser l’API d’appartenance pour ajouter des utilisateurs et des rôles.</span><span class="sxs-lookup"><span data-stu-id="0ae60-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="0ae60-229">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer.</span><span class="sxs-lookup"><span data-stu-id="0ae60-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="0ae60-230">Vous pouvez également demander de nouvelles rubriques à [afficher les leçons de Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="0ae60-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="0ae60-231">Vous pouvez même demander et voter sur les nouvelles fonctionnalités à ajouter à ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0ae60-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="0ae60-232">Par exemple, vous pouvez voter pour un outil à [créer et gérer des utilisateurs et des rôles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="0ae60-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="0ae60-233">Pour une bonne explication du fonctionnement des Services d’authentification externe ASP.NET, consultez de Robert McMurray [des Services d’authentification externe](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="0ae60-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="0ae60-234">L’article de Robert prendra également en détail dans l’activation de l’authentification de Microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="0ae60-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="0ae60-235">Tom Dykstra de [didacticiel EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) montre comment travailler avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ae60-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
