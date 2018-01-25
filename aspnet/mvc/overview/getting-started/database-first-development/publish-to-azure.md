---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publier un site MVC Database First sur Azure | Documents Microsoft
author: tfitzmac
description: "À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: eadc0f2b08df29f80fe53d03cf88cd3cdcecfb12
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="872e8-104">Publier un site MVC Database First dans Windows Azure</span><span class="sxs-lookup"><span data-stu-id="872e8-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="872e8-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="872e8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="872e8-106">À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="872e8-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="872e8-107">Cette série de didacticiels vous montre comment automatiquement générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer des données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="872e8-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="872e8-109">Cette partie de la série se concentre sur l’application web et la base de données de publication vers Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="872e8-110">Vous pouvez lire cette rubrique pour en savoir plus sur la publication d’une application web et la base de données, mais pour effectuer les étapes que vous devez commencer au début du didacticiel.</span><span class="sxs-lookup"><span data-stu-id="872e8-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="872e8-111">Consultez [mise en route](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="872e8-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="872e8-112">Déployer votre application web sur Azure</span><span class="sxs-lookup"><span data-stu-id="872e8-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="872e8-113">Vous avez besoin d’un compte Azure pour effectuer ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="872e8-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="872e8-114">Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -vous obtenez des crédits que vous pouvez utiliser pour tester les services Azure payants et même après qu’ils soient utilisés jusqu'à, vous pouvez conserver le compte et libérer de l’utilisation des services Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="872e8-115">Vous pouvez [activer les abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="872e8-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="872e8-116">Pour publier votre application web, cliquez sur le projet et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="872e8-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![publication du site](publish-to-azure/_static/image1.png)

<span data-ttu-id="872e8-118">Sélectionnez les sites Web Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-118">Select Microsoft Azure Websites.</span></span>

![Sélectionnez Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="872e8-120">Si vous n’êtes pas connecté Azure, fournissez vos informations d’identification de compte Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="872e8-121">Ensuite, sélectionnez Nouveau pour créer une application web.</span><span class="sxs-lookup"><span data-stu-id="872e8-121">Then, select New to create a new web app.</span></span>

![nouveau site](publish-to-azure/_static/image3.png)

<span data-ttu-id="872e8-123">Créer un nom unique pour votre application web.</span><span class="sxs-lookup"><span data-stu-id="872e8-123">Create a unique name for your web app.</span></span> <span data-ttu-id="872e8-124">Vous savez que le nom est unique si vous voyez une coche verte à droite du champ name.</span><span class="sxs-lookup"><span data-stu-id="872e8-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="872e8-125">Sélectionnez une région pour votre application web.</span><span class="sxs-lookup"><span data-stu-id="872e8-125">Select a region for your web app.</span></span> <span data-ttu-id="872e8-126">Sélectionnez **créer un nouveau serveur** pour la base de données et fournir un nom d’utilisateur et un mot de passe pour ce nouveau serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="872e8-127">Lorsque vous avez terminé, cliquez sur **créer**.</span><span class="sxs-lookup"><span data-stu-id="872e8-127">When finished, click **Create**.</span></span>

![créer un site](publish-to-azure/_static/image4.png)

<span data-ttu-id="872e8-129">Les valeurs de connexion sont maintenant tous définis.</span><span class="sxs-lookup"><span data-stu-id="872e8-129">Your connection values are now all set.</span></span> <span data-ttu-id="872e8-130">Vous pouvez laisser ces valeurs inchangées.</span><span class="sxs-lookup"><span data-stu-id="872e8-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="872e8-132">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="872e8-132">Click **Next**.</span></span>

<span data-ttu-id="872e8-133">Pour les paramètres, notez que deux connexions de base de données sont spécifiés - ApplicationDBContext et ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="872e8-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="872e8-134">ApplicationDBContext correspond à la connexion pour les tables de compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="872e8-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="872e8-135">Ces valeurs affichent uniquement les chaînes de connexion pour les bases de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="872e8-136">Cela ne signifie pas que ces bases de données seront publiées lorsque vous publiez votre site.</span><span class="sxs-lookup"><span data-stu-id="872e8-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="872e8-137">Vous allez publier votre projet de base de données après avoir fini de l’application web de publication.</span><span class="sxs-lookup"><span data-stu-id="872e8-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="872e8-138">Les points de suspension (...) en regard de la connexion de base de données affiche les détails de la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="872e8-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="872e8-139">Cliquez sur le bouton de sélection en regard de ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="872e8-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="872e8-140">Notez le nom du serveur de base de données et la base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="872e8-141">Le nom du serveur est généré aléatoirement.</span><span class="sxs-lookup"><span data-stu-id="872e8-141">The server name is randomly generated.</span></span> <span data-ttu-id="872e8-142">Le nom de la base de données est simple le nom de votre site avec  **\_db** ajouté.</span><span class="sxs-lookup"><span data-stu-id="872e8-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="872e8-143">Vous aurez besoin des noms ultérieurement lorsque vous publiez votre base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="872e8-144">Cliquez sur **OK** pour fermer la fenêtre de chaîne de connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="872e8-145">Dans la fenêtre de publier le site Web, cliquez sur **suivant** permet d’afficher l’aperçu.</span><span class="sxs-lookup"><span data-stu-id="872e8-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="872e8-146">Vous pouvez cliquer sur Démarrer l’aperçu pour afficher la liste des fichiers à publier.</span><span class="sxs-lookup"><span data-stu-id="872e8-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="872e8-147">Dans la mesure où il s’agit de la première fois que vous avez publié ce site, la liste est tous les fichiers appropriés dans le projet.</span><span class="sxs-lookup"><span data-stu-id="872e8-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="872e8-148">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="872e8-148">Click **Publish**.</span></span>

<span data-ttu-id="872e8-149">Le volet de résultats affiche les résultats de votre publication.</span><span class="sxs-lookup"><span data-stu-id="872e8-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="872e8-150">Après la publication, le site est immedialely lancée dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="872e8-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="872e8-151">Votre site a été déployée et vous pouvez inscrire un nouvel utilisateur sur le site ; Toutefois, les tables dans le projet ContosoUniversityData n’ont pas encore été publiées.</span><span class="sxs-lookup"><span data-stu-id="872e8-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="872e8-152">Si vous cliquez sur la liste des étudiants lien, vous recevrez une erreur.</span><span class="sxs-lookup"><span data-stu-id="872e8-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="872e8-153">Publier la base de données vers SQL Azure</span><span class="sxs-lookup"><span data-stu-id="872e8-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="872e8-154">Avant de publier votre base de données, il se peut que vous devez vous assurer que votre ordinateur local peut se connecter au serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="872e8-155">Le pare-feu pour votre serveur de base de données restreint qui machines peuvent se connecter à la base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="872e8-156">Vous devez ajouter l’adresse IP de votre ordinateur aux adresses IP autorisées pour le pare-feu.</span><span class="sxs-lookup"><span data-stu-id="872e8-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="872e8-157">Connectez-vous à votre compte Azure via le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="872e8-158">Sélectionnez votre nouvelle base de données et sélectionnez **gérer**.</span><span class="sxs-lookup"><span data-stu-id="872e8-158">Select your new database and select **Manage**.</span></span>

![Gérer](publish-to-azure/_static/image10.png)

<span data-ttu-id="872e8-160">Vous devez configurer votre serveur de base de données pour autoriser les connexions à partir de votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="872e8-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="872e8-161">Lorsque vous sélectionnez Gérer, vous êtes invité à ajouter l’adresse IP actuelle comme autorisées sur le serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="872e8-162">Sélectionnez Oui.</span><span class="sxs-lookup"><span data-stu-id="872e8-162">Select Yes.</span></span>

![Ajouter une adresse ip](publish-to-azure/_static/image11.png)

<span data-ttu-id="872e8-164">Il est probable que l’adresse IP que vous avez ajouté à l’étape précédente n’est pas la seule adresse IP, que vous devez configurer pour les connexions.</span><span class="sxs-lookup"><span data-stu-id="872e8-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="872e8-165">Vous pouvez tenter de se connecter à la base de données afin de déterminer si les connexions ont été correctement configurées.</span><span class="sxs-lookup"><span data-stu-id="872e8-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="872e8-166">Fournir l’utilisateur et le mot de passe que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="872e8-166">Provide the user and password you created earlier.</span></span>

![connexion](publish-to-azure/_static/image12.png)

<span data-ttu-id="872e8-168">Si vous recevez un message d’erreur, vous devez ajouter une autre adresse IP.</span><span class="sxs-lookup"><span data-stu-id="872e8-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="872e8-169">Cliquez sur le message d’erreur pour plus d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="872e8-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="872e8-170">Dans les détails, vous verrez l’adresse IP que vous souhaitez ajouter.</span><span class="sxs-lookup"><span data-stu-id="872e8-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="872e8-171">Notez cette adresse IP.</span><span class="sxs-lookup"><span data-stu-id="872e8-171">Note this IP address.</span></span>

![non autorisé](publish-to-azure/_static/image13.png)

<span data-ttu-id="872e8-173">Fermez cette fenêtre de connexion et retourner au portail Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="872e8-174">Accédez au tableau de bord pour votre base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="872e8-175">Cliquez sur **gérer les adresses IP autorisée**.</span><span class="sxs-lookup"><span data-stu-id="872e8-175">Click **Manage allowed IP addresses**.</span></span>

![gérer les adresses ip](publish-to-azure/_static/image14.png)

<span data-ttu-id="872e8-177">Vous devez à présent ajouter l’adresse IP à partir du message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="872e8-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="872e8-178">Modifier la plage d’adresses IP autorisées à inclure de celui du message d’erreur, ou ajouter cette adresse IP en tant qu’une entrée distincte.</span><span class="sxs-lookup"><span data-stu-id="872e8-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Ajouter une nouvelle adresse](publish-to-azure/_static/image15.png)

<span data-ttu-id="872e8-180">Enregistrez la modification des adresses IP autorisées.</span><span class="sxs-lookup"><span data-stu-id="872e8-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="872e8-181">Cliquez sur Gérer et essayez de vous connecter à nouveau à la base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="872e8-182">Vous devrez peut-être attendre quelques minutes avant que les adresses IP autorisées sont correctement configurés pour le pare-feu.</span><span class="sxs-lookup"><span data-stu-id="872e8-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="872e8-183">Lorsque vous pouvez vous connecter avec succès dans la base de données, vous avez défini votre connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="872e8-184">Vous pouvez laisser cette fenêtre d’administration ouvert, car vous allez vérifier le résultat de votre déploiement de base de données peu de temps.</span><span class="sxs-lookup"><span data-stu-id="872e8-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="872e8-185">Revenir à votre projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-185">Return to your database project.</span></span> <span data-ttu-id="872e8-186">Cliquez sur le projet et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="872e8-186">Right-click the project and select **Publish**.</span></span>

![publier la base de données](publish-to-azure/_static/image16.png)

<span data-ttu-id="872e8-188">Dans la fenêtre de la base de données de publication, sélectionnez **modifier**.</span><span class="sxs-lookup"><span data-stu-id="872e8-188">In the Publish Database window, select **Edit**.</span></span>

![modifier](publish-to-azure/_static/image17.png)

<span data-ttu-id="872e8-190">Indiquez le nom du serveur de base de données et vos informations d’identification d’authentification pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="872e8-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="872e8-191">Après avoir entré les informations d’identification, sélectionnez la base de données que vous avez créé à partir de la liste des bases de données disponibles.</span><span class="sxs-lookup"><span data-stu-id="872e8-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="872e8-192">Par défaut, Visual Studio définit le nom du champ de base de données sur le nom de votre projet qui ne peut pas être identique à la base de données que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="872e8-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="872e8-193">Cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="872e8-193">Click OK.</span></span>

<span data-ttu-id="872e8-194">Vous souhaiterez probablement enregistrer ce profil, vous pouvez publier des mises à jour à l’avenir sans entrer à nouveau toutes les informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="872e8-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="872e8-195">Sélectionnez **Créer un profil**.</span><span class="sxs-lookup"><span data-stu-id="872e8-195">Select **Create Profile**.</span></span>

![enregistrer le profil](publish-to-azure/_static/image19.png)

<span data-ttu-id="872e8-197">Il crée un fichier dans votre projet nommé **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="872e8-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="872e8-198">La prochaine fois que vous souhaitez publier la base de données vers Azure, il suffit de recharger ce profil.</span><span class="sxs-lookup"><span data-stu-id="872e8-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="872e8-199">Maintenant, cliquez sur **publier** pour créer la base de données sur Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-199">Now, click **Publish** to create the database on Azure.</span></span>

![publish](publish-to-azure/_static/image20.png)

<span data-ttu-id="872e8-201">Après l’exécution d’un certain temps, les publication des résultats sont affichés.</span><span class="sxs-lookup"><span data-stu-id="872e8-201">After running for a while, the publishing results are displayed.</span></span>

![résultats](publish-to-azure/_static/image21.png)

<span data-ttu-id="872e8-203">Maintenant, vous pouvez revenir au portail de gestion pour votre base de données.</span><span class="sxs-lookup"><span data-stu-id="872e8-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="872e8-204">Actualiser la vue de conception et notez que les 3 tables avec données préremplies ont été déployés.</span><span class="sxs-lookup"><span data-stu-id="872e8-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![nouvelles tables](publish-to-azure/_static/image22.png)

<span data-ttu-id="872e8-206">Vous êtes maintenant prêt à tester l’application web qui est déployée sur Azure.</span><span class="sxs-lookup"><span data-stu-id="872e8-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="872e8-207">Accédez à l’application web sur Azure (par exemple, http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="872e8-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="872e8-208">Cliquez sur le lien pour la liste d’étudiants et vous devez voir l’affichage de l’index pour les étudiants.</span><span class="sxs-lookup"><span data-stu-id="872e8-208">Click the link for List of students and you should see the index view for students.</span></span>

![vue](publish-to-azure/_static/image23.png)

<span data-ttu-id="872e8-210">Parfois, la base de données et la connexion doivent un peu de temps pour être correctement configurés.</span><span class="sxs-lookup"><span data-stu-id="872e8-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="872e8-211">Si vous recevez une erreur, attendez quelques minutes et réessayez.</span><span class="sxs-lookup"><span data-stu-id="872e8-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="872e8-212">Conclusion</span><span class="sxs-lookup"><span data-stu-id="872e8-212">Conclusion</span></span>

<span data-ttu-id="872e8-213">Cette série fourni un exemple simple de génération de code à partir d’une base de données existante qui permet aux utilisateurs de modifier, mettre à jour, créer et supprimer des données.</span><span class="sxs-lookup"><span data-stu-id="872e8-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="872e8-214">Il utilisé ASP.NET MVC 5, Entity Framework et génération de modèles automatique ASP.NET pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="872e8-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="872e8-215">Pour obtenir un exemple d’introduction du développement Code First, consultez [mise en route avec ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="872e8-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="872e8-216">Pour obtenir un exemple plus avancé, consultez [création d’un modèle de données Entity Framework pour une application ASP.NET MVC est 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="872e8-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="872e8-217">Notez que l’API DbContext que vous utilisez pour l’utilisation des données dans la première base de données est identique à l’API que vous utilisez pour travailler avec des données dans le Code First.</span><span class="sxs-lookup"><span data-stu-id="872e8-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="872e8-218">Même si vous envisagez d’utiliser la première base de données, vous pouvez apprendre à gérer des scénarios plus complexes telles que la lecture et mise à jour des données associées, la gestion des conflits d’accès concurrentiel, et ainsi de suite à partir d’un didacticiel de Code First.</span><span class="sxs-lookup"><span data-stu-id="872e8-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="872e8-219">La seule différence est dans la façon dont la base de données, classe de contexte et des classes d’entité sont créés.</span><span class="sxs-lookup"><span data-stu-id="872e8-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="872e8-220">Précédent</span><span class="sxs-lookup"><span data-stu-id="872e8-220">Previous</span></span>](enhancing-data-validation.md)
