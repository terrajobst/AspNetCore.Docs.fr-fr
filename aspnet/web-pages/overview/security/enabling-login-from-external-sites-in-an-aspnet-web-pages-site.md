---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Connexion à l’aide de Sites externes dans une application Web Pages (Razor) Site | Microsoft Docs
author: tfitzmac
description: Cet article explique comment se connecter à votre site ASP.NET Web Pages (Razor) à l’aide de Facebook, Google, Twitter, Yahoo et autres sites, autrement dit, la prise en charge...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: b161a890d11b1c0bf86669b819b97255c075075f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396888"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="ccc25-103">Connexion à l’aide de Sites externes dans un Site ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="ccc25-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="ccc25-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ccc25-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ccc25-105">Cet article explique comment se connecter à votre site ASP.NET Web Pages (Razor) à l’aide de Facebook, Google, Twitter, Yahoo et autres sites, autrement dit, la prise en charge OAuth et OpenID dans votre site.</span><span class="sxs-lookup"><span data-stu-id="ccc25-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="ccc25-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="ccc25-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ccc25-107">Comment activer la connexion à partir d’autres sites lorsque vous utilisez le modèle de WebMatrix Starter Site.</span><span class="sxs-lookup"><span data-stu-id="ccc25-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="ccc25-108">Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :</span><span class="sxs-lookup"><span data-stu-id="ccc25-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="ccc25-109">Le `OAuthWebSecurity` helper.</span><span class="sxs-lookup"><span data-stu-id="ccc25-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ccc25-110">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="ccc25-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ccc25-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="ccc25-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="ccc25-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="ccc25-112">WebMatrix 3</span></span>

<span data-ttu-id="ccc25-113">Les Pages Web ASP.NET inclut la prise en charge de [OAuth](http://oauth.net/) et [OpenID](http://openid.net/) fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="ccc25-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="ccc25-114">À l’aide de ces fournisseurs, vous pouvez laisser les utilisateurs ouvrent votre site à l’aide de leurs informations d’identification existantes à partir de Facebook, Twitter, Microsoft et Google.</span><span class="sxs-lookup"><span data-stu-id="ccc25-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="ccc25-115">Par exemple, pour vous connecter à l’aide d’un compte Facebook, les utilisateurs peuvent simplement choisir une icône de Facebook, qui redirige vers la page de connexion Facebook où ils entrent leurs informations de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ccc25-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="ccc25-116">Ils peuvent ensuite associer la connexion Facebook avec leur compte sur votre site.</span><span class="sxs-lookup"><span data-stu-id="ccc25-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="ccc25-117">Autre amélioration pour les fonctionnalités d’appartenance Web Pages est que vous pouvez associer plusieurs connexions (y compris les connexions à partir de sites de réseau social) avec un seul compte sur votre site Web.</span><span class="sxs-lookup"><span data-stu-id="ccc25-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="ccc25-118">Cette illustration montre la page de connexion à partir de la **Starter Site** modèle, où un utilisateur peut choisir une icône de Facebook, Twitter, Google ou Microsoft pour activer la journalisation avec un compte externe :</span><span class="sxs-lookup"><span data-stu-id="ccc25-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![fournisseurs externes](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="ccc25-120">Vous pouvez activer l’appartenance de OAuth et OpenID en supprimant commentaires quelques lignes de code dans le **Starter Site** modèle.</span><span class="sxs-lookup"><span data-stu-id="ccc25-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="ccc25-121">Les méthodes et propriétés vous permet de travailler avec le OAuth et OpenID fournisseurs sont dans le `WebMatrix.Security.OAuthWebSecurity` classe.</span><span class="sxs-lookup"><span data-stu-id="ccc25-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="ccc25-122">Le **Starter Site** modèle inclut une infrastructure d’appartenance complète, avec une page de connexion, une base de données d’appartenance et tout le code que vous avez besoin permettre aux utilisateurs de se votre site à l’aide des informations d’identification locales ou ceux à partir d’un autre site .</span><span class="sxs-lookup"><span data-stu-id="ccc25-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="ccc25-123">Cette section fournit un exemple montrant comment permettre aux utilisateurs de se connecter à partir de sites externes à un site qui est basé sur le **Starter Site** modèle.</span><span class="sxs-lookup"><span data-stu-id="ccc25-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="ccc25-124">Après avoir créé un site de démarrage, vous effectuez cette (suivi de détails) :</span><span class="sxs-lookup"><span data-stu-id="ccc25-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="ccc25-125">Pour les sites qui utilisent un fournisseur OAuth (Facebook, Twitter et Microsoft), vous créez une application sur le site externe.</span><span class="sxs-lookup"><span data-stu-id="ccc25-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="ccc25-126">Cela vous donne les clés d’application dont vous avez besoin pour appeler la fonctionnalité de connexion pour ces sites.</span><span class="sxs-lookup"><span data-stu-id="ccc25-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="ccc25-127">Pour les sites qui utilisent un fournisseur OpenID (Google), il est inutile de créer une application.</span><span class="sxs-lookup"><span data-stu-id="ccc25-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="ccc25-128">Pour tous ces sites, vous devez disposer un compte pour se connecter et créer des applications de développeur.</span><span class="sxs-lookup"><span data-stu-id="ccc25-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ccc25-129">Applications Microsoft acceptent uniquement une URL dynamique pour un site Web de travail, vous ne pouvez pas utiliser une URL de site Web local pour le test des connexions.</span><span class="sxs-lookup"><span data-stu-id="ccc25-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="ccc25-130">Modifier quelques fichiers dans votre site Web afin de spécifier le fournisseur d’authentification approprié et soumettre une connexion vers le site que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="ccc25-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="ccc25-131">Cet article fournit des instructions distinctes pour les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="ccc25-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="ccc25-132">L’activation de connexions de Google</span><span class="sxs-lookup"><span data-stu-id="ccc25-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="ccc25-133">L’activation de connexions de Facebook</span><span class="sxs-lookup"><span data-stu-id="ccc25-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="ccc25-134">L’activation de connexions d’accès Twitter</span><span class="sxs-lookup"><span data-stu-id="ccc25-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="ccc25-135">L’activation de connexions de Google</span><span class="sxs-lookup"><span data-stu-id="ccc25-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="ccc25-136">Créez ou ouvrez un site ASP.NET Web Pages qui est basé sur le modèle de Site de démarrage de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ccc25-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ccc25-137">Ouvrez le  *\_AppStart.cshtml* page et supprimez les commentaires de la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="ccc25-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="ccc25-138">Test de connexion Google</span><span class="sxs-lookup"><span data-stu-id="ccc25-138">Testing Google login</span></span>

1. <span data-ttu-id="ccc25-139">Exécutez le *default.cshtml* page de votre site et choisissez le **connectez-vous** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="ccc25-140">Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez une le **Google** ou **Yahoo** bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="ccc25-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="ccc25-141">Cet exemple utilise la connexion de Google.</span><span class="sxs-lookup"><span data-stu-id="ccc25-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="ccc25-142">La page web redirige la requête vers la page de connexion de Google.</span><span class="sxs-lookup"><span data-stu-id="ccc25-142">The web page redirects the request to the Google login page.</span></span>

    ![Connexion Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="ccc25-144">Entrez les informations d’identification pour un compte Google existant.</span><span class="sxs-lookup"><span data-stu-id="ccc25-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="ccc25-145">Si Google vous demande si vous souhaitez autoriser *Localhost* pour utiliser les informations relatives au compte, cliquez sur **autoriser**.</span><span class="sxs-lookup"><span data-stu-id="ccc25-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="ccc25-146">Le code utilise le jeton de Google pour authentifier l’utilisateur et retourne ensuite à cette page sur votre site Web.</span><span class="sxs-lookup"><span data-stu-id="ccc25-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="ccc25-147">Cette page permet aux utilisateurs d’associer leur connexion Google avec un compte existant sur votre site Web, ou ils peuvent inscrire un nouveau compte sur votre site à associer la connexion externe.</span><span class="sxs-lookup"><span data-stu-id="ccc25-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="ccc25-149">Choisissez le **associer** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-149">Choose the **Associate** button.</span></span> <span data-ttu-id="ccc25-150">Le navigateur revient à la page d’accueil de votre application.</span><span class="sxs-lookup"><span data-stu-id="ccc25-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="ccc25-151">L’activation de connexions de Facebook</span><span class="sxs-lookup"><span data-stu-id="ccc25-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="ccc25-152">Accédez à la [site de développeurs Facebook](https://developers.facebook.com/apps) (se connecter que si vous n’êtes pas déjà connecté).</span><span class="sxs-lookup"><span data-stu-id="ccc25-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="ccc25-153">Choisissez le **créer une application** bouton, puis suivez les invites pour nommer et créer l’application.</span><span class="sxs-lookup"><span data-stu-id="ccc25-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="ccc25-154">Dans la section **sélectionner comment votre application s’intègre avec Facebook**, choisissez le **site Web** section.</span><span class="sxs-lookup"><span data-stu-id="ccc25-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="ccc25-155">Renseignez le **URL du Site** champ avec l’URL de votre site (par exemple, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="ccc25-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="ccc25-156">Le **domaine** champ est facultatif ; vous pouvez l’utiliser pour l’authentification d’un domaine entier (tel que *example.com*).</span><span class="sxs-lookup"><span data-stu-id="ccc25-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="ccc25-157">Si vous exécutez un site sur votre ordinateur local avec une URL comme `http://localhost:12345` (où le nombre est un numéro de port local), vous pouvez ajouter cette valeur pour le **URL du Site** champ pour tester votre site.</span><span class="sxs-lookup"><span data-stu-id="ccc25-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="ccc25-158">Toutefois, chaque fois que le numéro de port de vos modifications de site local, vous devez mettre à jour le **URL du Site** champ de votre application.</span><span class="sxs-lookup"><span data-stu-id="ccc25-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="ccc25-159">Choisissez le **enregistrer les modifications** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="ccc25-160">Choisissez le **applications** onglet à nouveau et afficher la page de démarrage pour votre application.</span><span class="sxs-lookup"><span data-stu-id="ccc25-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="ccc25-161">Copie le **ID d’application** et **Secret d’application** valeurs pour votre application et collez-les dans un fichier texte temporaire.</span><span class="sxs-lookup"><span data-stu-id="ccc25-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="ccc25-162">Vous transmettez ces valeurs pour le fournisseur de Facebook dans votre code de site Web.</span><span class="sxs-lookup"><span data-stu-id="ccc25-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="ccc25-163">Quitter le site des développeurs Facebook.</span><span class="sxs-lookup"><span data-stu-id="ccc25-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="ccc25-164">Maintenant vous apportez des modifications à deux pages dans votre site Web afin que les utilisateurs seront en mesure de vous connecter au site à l’aide de leur compte Facebook.</span><span class="sxs-lookup"><span data-stu-id="ccc25-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="ccc25-165">Créez ou ouvrez un site ASP.NET Web Pages qui est basé sur le modèle de Site de démarrage de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ccc25-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ccc25-166">Ouvrez le  *\_AppStart.cshtml* page et les commentaires du code pour le fournisseur Facebook OAuth.</span><span class="sxs-lookup"><span data-stu-id="ccc25-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="ccc25-167">Le bloc de code sans commentaire se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="ccc25-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="ccc25-168">Copie le **ID d’application** valeur à partir de l’application Facebook en tant que la valeur de la `appId` paramètre (à l’intérieur des guillemets).</span><span class="sxs-lookup"><span data-stu-id="ccc25-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="ccc25-169">Copie **Secret d’application** valeur à partir de l’application Facebook en tant que le `appSecret` valeur du paramètre.</span><span class="sxs-lookup"><span data-stu-id="ccc25-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="ccc25-170">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="ccc25-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="ccc25-171">Test de connexion Facebook</span><span class="sxs-lookup"><span data-stu-id="ccc25-171">Testing Facebook login</span></span>

1. <span data-ttu-id="ccc25-172">Exécuter le site *default.cshtml* page et choisissez le **connexion** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="ccc25-173">Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez le **Facebook** icône.</span><span class="sxs-lookup"><span data-stu-id="ccc25-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="ccc25-174">La page web redirige la requête vers la page de connexion Facebook.</span><span class="sxs-lookup"><span data-stu-id="ccc25-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="ccc25-176">Connectez-vous à un compte Facebook.</span><span class="sxs-lookup"><span data-stu-id="ccc25-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="ccc25-177">Le code utilise le jeton Facebook afin de vous authentifier et renvoie à une page où vous pouvez associer votre connexion Facebook avec le compte de connexion de votre site.</span><span class="sxs-lookup"><span data-stu-id="ccc25-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="ccc25-178">Votre utilisateur nom ou adresse e-mail est rempli dans le **E-mail** champ sur le formulaire.</span><span class="sxs-lookup"><span data-stu-id="ccc25-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="ccc25-180">Choisissez le **associer** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="ccc25-181">Le navigateur revient à la page d’accueil et que vous êtes connecté.</span><span class="sxs-lookup"><span data-stu-id="ccc25-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="ccc25-182">L’activation de connexions d’accès Twitter</span><span class="sxs-lookup"><span data-stu-id="ccc25-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="ccc25-183">Accédez à la [site des développeurs Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="ccc25-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="ccc25-184">Choisissez le **créer une application** lier et ouvrir une session sur le site.</span><span class="sxs-lookup"><span data-stu-id="ccc25-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="ccc25-185">Sur le **créer une Application** forment, renseignez le **nom** et **Description** champs.</span><span class="sxs-lookup"><span data-stu-id="ccc25-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="ccc25-186">Dans le **site Web** , entrez l’URL de votre site (par exemple, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="ccc25-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="ccc25-187">Si vous testez votre site localement (à l’aide d’une URL telle que `http://localhost:12345`), Twitter ne peut-être pas accepter l’URL.</span><span class="sxs-lookup"><span data-stu-id="ccc25-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="ccc25-188">Toutefois, vous pourrez peut-être utiliser l’adresse IP de bouclage local (par exemple `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="ccc25-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="ccc25-189">Cela simplifie le processus de test de votre application localement.</span><span class="sxs-lookup"><span data-stu-id="ccc25-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="ccc25-190">Toutefois, chaque fois que le numéro de port de votre site local change, vous devez mettre à jour le **site Web** champ de votre application.</span><span class="sxs-lookup"><span data-stu-id="ccc25-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="ccc25-191">Dans le **URL de rappel** , entrez une URL pour la page dans votre site Web que vous souhaitez que les utilisateurs pour revenir à une fois la connexion à Twitter.</span><span class="sxs-lookup"><span data-stu-id="ccc25-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="ccc25-192">Par exemple, pour envoyer les utilisateurs à la page d’accueil du Site Starter (qui reconnaîtra leur état connecté), entrez la même URL que vous avez entré dans le **site Web** champ.</span><span class="sxs-lookup"><span data-stu-id="ccc25-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="ccc25-193">Acceptez les termes du contrat et choisissez le **créer votre application Twitter** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="ccc25-194">Sur le **Mes Applications** d’accueil de page, choisissez l’application que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="ccc25-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="ccc25-195">Sur le **détails** onglet, faites défiler vers le bas et choisissez le **créer mon jeton d’accès** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="ccc25-196">Sur le **détails** onglet, copiez la **clé de consommateur** et **Secret de consommateur** valeurs pour votre application et collez-les dans un fichier texte temporaire.</span><span class="sxs-lookup"><span data-stu-id="ccc25-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="ccc25-197">Vous transmettrez ces valeurs pour le fournisseur Twitter dans votre code de site Web.</span><span class="sxs-lookup"><span data-stu-id="ccc25-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="ccc25-198">Quitter le site Twitter.</span><span class="sxs-lookup"><span data-stu-id="ccc25-198">Exit the Twitter site.</span></span>

<span data-ttu-id="ccc25-199">Maintenant vous apportez des modifications à deux pages dans votre site Web afin que les utilisateurs pourront se connecter au site à l’aide de leur compte Twitter.</span><span class="sxs-lookup"><span data-stu-id="ccc25-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="ccc25-200">Créez ou ouvrez un site ASP.NET Web Pages qui est basé sur le modèle de Site de démarrage de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ccc25-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ccc25-201">Ouvrez le  *\_AppStart.cshtml* page et les commentaires du code pour le fournisseur OAuth pour Twitter.</span><span class="sxs-lookup"><span data-stu-id="ccc25-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="ccc25-202">Le bloc de code sans commentaire ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="ccc25-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="ccc25-203">Copie le **clé de consommateur** valeur à partir de l’application Twitter en tant que la valeur de la `consumerKey` paramètre (à l’intérieur des guillemets).</span><span class="sxs-lookup"><span data-stu-id="ccc25-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="ccc25-204">Copie le **Secret de consommateur** valeur à partir de l’application Twitter en tant que la valeur de la `consumerSecret` paramètre.</span><span class="sxs-lookup"><span data-stu-id="ccc25-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="ccc25-205">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="ccc25-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="ccc25-206">Test de connexion à Twitter</span><span class="sxs-lookup"><span data-stu-id="ccc25-206">Testing Twitter login</span></span>

1. <span data-ttu-id="ccc25-207">Exécutez le *default.cshtml* page de votre site et choisissez le **connexion** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="ccc25-208">Sur le *connexion* page, dans le **utiliser un autre service pour vous connecter** , choisissez le **Twitter** icône.</span><span class="sxs-lookup"><span data-stu-id="ccc25-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="ccc25-209">La page web redirige la demande vers une page de connexion Twitter pour l’application que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="ccc25-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![Oauth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="ccc25-211">Connectez-vous à un compte Twitter.</span><span class="sxs-lookup"><span data-stu-id="ccc25-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="ccc25-212">Le code utilise le jeton Twitter pour authentifier l’utilisateur et puis vous redirige vers une page où vous pouvez associer votre connexion avec votre compte de site Web.</span><span class="sxs-lookup"><span data-stu-id="ccc25-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="ccc25-213">Votre nom ou adresse de messagerie est renseigné dans le **E-mail** champ sur le formulaire.</span><span class="sxs-lookup"><span data-stu-id="ccc25-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="ccc25-215">Choisissez le **associer** bouton.</span><span class="sxs-lookup"><span data-stu-id="ccc25-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="ccc25-216">Le navigateur revient à la page d’accueil et que vous êtes connecté.</span><span class="sxs-lookup"><span data-stu-id="ccc25-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ccc25-217">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ccc25-217">Additional Resources</span></span>


- [<span data-ttu-id="ccc25-218">Personnalisation du comportement à l’échelle du site</span><span class="sxs-lookup"><span data-stu-id="ccc25-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="ccc25-219">Ajout de sécurité et l’appartenance à un Site ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="ccc25-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
