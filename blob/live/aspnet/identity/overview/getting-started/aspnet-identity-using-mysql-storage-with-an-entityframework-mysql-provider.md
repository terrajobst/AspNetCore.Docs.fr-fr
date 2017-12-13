---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "Identité ASP.NET : L’utilisation du stockage de MySQL avec un fournisseur de MySQL EntityFramework (c#) | Documents Microsoft"
author: maumar
description: "Ce didacticiel vous montre comment remplacer le mécanisme de stockage de données par défaut pour ASP.NET Identity EntityFramework (fournisseur SQL client) avec un fournir MySQL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: ac254abcb756d048d159a9b67967a581f35ac871
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="4f827-103">Identité ASP.NET : L’utilisation du stockage de MySQL avec un fournisseur EntityFramework MySQL (c#)</span><span class="sxs-lookup"><span data-stu-id="4f827-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="4f827-104">par [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="4f827-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="4f827-105">Ce didacticiel vous montre comment remplacer le mécanisme de stockage de données par défaut pour [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) avec EntityFramework (fournisseur SQL client) avec un fournisseur MySQL.</span><span class="sxs-lookup"><span data-stu-id="4f827-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="4f827-106">Les rubriques suivantes sont traitées dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="4f827-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="4f827-107">Création d’une base de données MySQL sur Azure</span><span class="sxs-lookup"><span data-stu-id="4f827-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="4f827-108">Création d’une application MVC à l’aide du modèle MVC de 2013 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f827-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="4f827-109">Configuration EntityFramework pour travailler avec un fournisseur de base de données MySQL</span><span class="sxs-lookup"><span data-stu-id="4f827-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="4f827-110">Exécution de l’application pour vérifier les résultats</span><span class="sxs-lookup"><span data-stu-id="4f827-110">Running the application to verify the results</span></span>

<span data-ttu-id="4f827-111">À la fin de ce didacticiel, vous disposerez une application MVC avec l’identité de ASP.NET stocker qui utilise une base de données MySQL qui est hébergé dans Azure.</span><span class="sxs-lookup"><span data-stu-id="4f827-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="4f827-112">Création d’une instance de la base de données MySQL sur Azure</span><span class="sxs-lookup"><span data-stu-id="4f827-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="4f827-113">Se connecter à la [portail Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="4f827-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="4f827-114">Cliquez sur **nouveau** au bas de la page, puis sélectionnez **magasin**:</span><span class="sxs-lookup"><span data-stu-id="4f827-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="4f827-115">Dans le **choisir et des modules complémentaires** Assistant, sélectionnez **base de données MySQL de ClearDB**, puis cliquez sur le **suivant** flèche en bas de l’image :</span><span class="sxs-lookup"><span data-stu-id="4f827-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="4f827-116">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-116">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-117">]</span><span class="sxs-lookup"><span data-stu-id="4f827-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="4f827-118">Conservez la valeur par défaut **libre** planifier, de modifier le **nom** à **IdentityMySQLDatabase**, sélectionnez la région qui est plus proche de vous, puis cliquez sur le **suivant** flèche en bas de l’image :</span><span class="sxs-lookup"><span data-stu-id="4f827-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="4f827-119">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-119">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-120">]</span><span class="sxs-lookup"><span data-stu-id="4f827-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="4f827-121">Cliquez sur le **achat** coche pour terminer la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="4f827-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
 <span data-ttu-id="4f827-122">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-122">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-123">]</span><span class="sxs-lookup"><span data-stu-id="4f827-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="4f827-124">Une fois que votre base de données a été créé, vous pouvez le gérer à partir de la **modules complémentaires** onglet dans le portail de gestion.</span><span class="sxs-lookup"><span data-stu-id="4f827-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="4f827-125">Pour récupérer les informations de connexion pour votre base de données, cliquez sur **informations de connexion** au bas de la page :</span><span class="sxs-lookup"><span data-stu-id="4f827-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
 <span data-ttu-id="4f827-126">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-126">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-127">]</span><span class="sxs-lookup"><span data-stu-id="4f827-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="4f827-128">Copier la chaîne de connexion en cliquant sur le bouton Copier par le **CONNECTIONSTRING** champ et l’enregistrer, vous allez utiliser ces informations ultérieurement dans ce didacticiel pour votre application MVC :</span><span class="sxs-lookup"><span data-stu-id="4f827-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
 <span data-ttu-id="4f827-129">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-129">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-130">]</span><span class="sxs-lookup"><span data-stu-id="4f827-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="4f827-131">Création d’un projet d’application MVC</span><span class="sxs-lookup"><span data-stu-id="4f827-131">Creating an MVC application project</span></span>

<span data-ttu-id="4f827-132">Pour effectuer les étapes décrites dans cette section du didacticiel, vous devez d’abord installer [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="4f827-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="4f827-133">Une fois que Visual Studio a été installé, procédez comme suit pour créer un nouveau projet d’application MVC :</span><span class="sxs-lookup"><span data-stu-id="4f827-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="4f827-134">Ouvrez Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="4f827-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="4f827-135">Cliquez sur **nouveau projet** à partir de la **Démarrer** page, vous pouvez également cliquer sur le **fichier** menu, puis **nouveau projet**:</span><span class="sxs-lookup"><span data-stu-id="4f827-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
 <span data-ttu-id="4f827-136">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-136">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-137">]</span><span class="sxs-lookup"><span data-stu-id="4f827-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="4f827-138">Lorsque le **nouveau projet** boîte de dialogue, développez **Visual C#** dans la liste des modèles, puis cliquez sur **Web**, puis sélectionnez **ASP.NET Web Application**.</span><span class="sxs-lookup"><span data-stu-id="4f827-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="4f827-139">Nommez votre projet **IdentityMySQLDemo** puis cliquez sur **OK**:</span><span class="sxs-lookup"><span data-stu-id="4f827-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
 <span data-ttu-id="4f827-140">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-140">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-141">]</span><span class="sxs-lookup"><span data-stu-id="4f827-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="4f827-142">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **MVC** templatewith les options par défaut ; cette va configurer **comptes d’utilisateur individuels** comme méthode d’authentification.</span><span class="sxs-lookup"><span data-stu-id="4f827-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="4f827-143">Cliquez sur **OK**:</span><span class="sxs-lookup"><span data-stu-id="4f827-143">Click **OK**:</span></span>  
  
 <span data-ttu-id="4f827-144">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-144">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-145">]</span><span class="sxs-lookup"><span data-stu-id="4f827-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="4f827-146">Configurer EntityFramework pour travailler avec une base de données MySQL</span><span class="sxs-lookup"><span data-stu-id="4f827-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="4f827-147">Mise à jour de l’assembly d’Entity Framework pour votre projet</span><span class="sxs-lookup"><span data-stu-id="4f827-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="4f827-148">L’application MVC qui a été créée à partir du modèle Visual Studio 2013 contienne une référence à la [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) du package, mais il ont été mises à jour à cet assembly depuis sa version qui contiennent améliorations des performances.</span><span class="sxs-lookup"><span data-stu-id="4f827-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="4f827-149">Pour utiliser ces dernières mises à jour dans votre application, procédez comme suit.</span><span class="sxs-lookup"><span data-stu-id="4f827-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="4f827-150">Ouvrez votre projet MVC dans Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4f827-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="4f827-151">Cliquez sur **outils**, puis cliquez sur **Gestionnaire de Package de bibliothèque**, puis cliquez sur **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="4f827-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
 <span data-ttu-id="4f827-152">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-152">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-153">]</span><span class="sxs-lookup"><span data-stu-id="4f827-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="4f827-154">Le **Package Manager Console** apparaîtra dans la section inférieure de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f827-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="4f827-155">Type &quot; **mise à jour-Package EntityFramework** &quot; et appuyez sur ENTRÉE :</span><span class="sxs-lookup"><span data-stu-id="4f827-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
 <span data-ttu-id="4f827-156">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-156">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-157">]</span><span class="sxs-lookup"><span data-stu-id="4f827-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="4f827-158">Installez le fournisseur MySQL pour EntityFramework</span><span class="sxs-lookup"><span data-stu-id="4f827-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="4f827-159">Dans l’ordre pour EntityFramework pour se connecter à la base de données MySQL, vous devez installer un fournisseur MySQL.</span><span class="sxs-lookup"><span data-stu-id="4f827-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="4f827-160">Pour ce faire, ouvrez le **Package Manager Console** et type &quot; **Pre - Install-Package MySql.Data.Entity**&quot;, puis appuyez sur ENTRÉE.</span><span class="sxs-lookup"><span data-stu-id="4f827-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="4f827-161">Il s’agit d’une version préliminaire de l’assembly, et par conséquent il peut contenir des bogues.</span><span class="sxs-lookup"><span data-stu-id="4f827-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="4f827-162">Vous ne devez pas utiliser une version préliminaire du fournisseur en production.</span><span class="sxs-lookup"><span data-stu-id="4f827-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="4f827-163">[Cliquez sur l’image suivante pour la développer.]</span><span class="sxs-lookup"><span data-stu-id="4f827-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="4f827-164">Apporter des modifications de configuration de projet dans le fichier Web.config pour votre application</span><span class="sxs-lookup"><span data-stu-id="4f827-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="4f827-165">Dans cette section, que vous allez configurer l’Entity Framework pour utiliser le fournisseur MySQL que vous venez d’installer, inscrire la fabrique de fournisseur MySQL et ajoutez votre chaîne de connexion à partir d’Azure.</span><span class="sxs-lookup"><span data-stu-id="4f827-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="4f827-166">Les exemples suivants contiennent une version d’assembly spécifique pour MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="4f827-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="4f827-167">Si la version d’assembly change, vous devez modifier les paramètres de configuration approprié avec la version appropriée.</span><span class="sxs-lookup"><span data-stu-id="4f827-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="4f827-168">Ouvrez le fichier Web.config de votre projet dans Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4f827-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="4f827-169">Recherchez les paramètres de configuration suivants, qui définissent le fournisseur de base de données par défaut et la fabrique pour Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="4f827-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="4f827-170">Remplacer les paramètres de configuration avec la commande suivante, qui configure l’Entity Framework pour utiliser le fournisseur MySQL :</span><span class="sxs-lookup"><span data-stu-id="4f827-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="4f827-171">Recherchez le &lt;connectionStrings&gt; section et remplacez-le par le code suivant, qui définit la chaîne de connexion pour votre base de données MySQL qui est hébergé sur Azure (Notez que valeur providerName a également été modifié à partir de la d’origine) :</span><span class="sxs-lookup"><span data-stu-id="4f827-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="4f827-172">Ajout de contexte MigrationHistory personnalisé</span><span class="sxs-lookup"><span data-stu-id="4f827-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="4f827-173">Utilise Entity Framework Code First un **MigrationHistory** table pour effectuer le suivi des modifications apportées au modèle et pour assurer la cohérence entre le schéma de base de données et le schéma conceptuel.</span><span class="sxs-lookup"><span data-stu-id="4f827-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="4f827-174">Toutefois, cette table ne fonctionne pas les pour MySQL par défaut, car la clé primaire est trop grande.</span><span class="sxs-lookup"><span data-stu-id="4f827-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="4f827-175">Pour remédier à cette situation, vous devez réduire la taille de clé pour cette table.</span><span class="sxs-lookup"><span data-stu-id="4f827-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="4f827-176">Pour ce faire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="4f827-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="4f827-177">Les informations de schéma pour cette table sont capturées dans une **HistoryContext**, ce qui peut être modifié comme n’importe quel autre **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="4f827-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="4f827-178">Pour ce faire, ajoutez un nouveau fichier de classe nommé **MySqlHistoryContext.cs** au projet et remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4f827-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="4f827-179">Ensuite, vous devrez configurer Entity Framework pour utiliser modifié **HistoryContext**, plutôt que par défaut.</span><span class="sxs-lookup"><span data-stu-id="4f827-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="4f827-180">Cela est possible en tirant parti des fonctionnalités de configuration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="4f827-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="4f827-181">Pour ce faire, ajoutez le nouveau fichier de classe nommé **MySqlConfiguration.cs** à votre projet et remplacez son contenu avec :</span><span class="sxs-lookup"><span data-stu-id="4f827-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="4f827-182">Création d’un initialiseur EntityFramework personnalisé pour ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="4f827-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="4f827-183">Le fournisseur MySQL qui est inclu dans ce didacticiel ne prend pas en charge les migrations d’Entity Framework, vous devez utiliser des initialiseurs de modèle pour vous connecter à la base de données.</span><span class="sxs-lookup"><span data-stu-id="4f827-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="4f827-184">Étant donné que ce didacticiel utilise une instance de MySQL sur Azure, vous devrez peut-être créer un initialiseur d’Entity Framework personnalisé.</span><span class="sxs-lookup"><span data-stu-id="4f827-184">Because this tutorial is using a MySQL instance on Azure, you will need need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="4f827-185">Cette étape n’est pas nécessaire si vous vous connectez à une instance de SQL Server sur Azure ou si vous utilisez une base de données est hébergée sur site.</span><span class="sxs-lookup"><span data-stu-id="4f827-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="4f827-186">Pour créer un initialiseur d’Entity Framework personnalisé pour MySQL, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="4f827-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="4f827-187">Ajouter un nouveau fichier de classe nommé **MySqlInitializer.cs** pour le projet, puis remplacez, il est contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4f827-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="4f827-188">Ouvrez le **IdentityModels.cs** fichier de votre projet, ce qui se trouve dans le **modèles** active et remplacer le contenu avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4f827-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="4f827-189">Exécution de l’application et la vérification de la base de données</span><span class="sxs-lookup"><span data-stu-id="4f827-189">Running the application and verifying the database</span></span>

<span data-ttu-id="4f827-190">Une fois que vous avez terminé les étapes décrites dans les sections précédentes, vous devez tester votre base de données.</span><span class="sxs-lookup"><span data-stu-id="4f827-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="4f827-191">Pour ce faire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="4f827-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="4f827-192">Appuyez sur **Ctrl + F5** pour générer et exécuter l’application web.</span><span class="sxs-lookup"><span data-stu-id="4f827-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="4f827-193">Cliquez sur le **inscrire** onglet en haut de la page :</span><span class="sxs-lookup"><span data-stu-id="4f827-193">Click the **Register** tab on the top of the page:</span></span>  
  
 <span data-ttu-id="4f827-194">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-194">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-195">]</span><span class="sxs-lookup"><span data-stu-id="4f827-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="4f827-196">Entrez un nom d’utilisateur et un mot de passe, puis cliquez sur **inscrire**:</span><span class="sxs-lookup"><span data-stu-id="4f827-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
 <span data-ttu-id="4f827-197">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-197">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-198">]</span><span class="sxs-lookup"><span data-stu-id="4f827-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="4f827-199">À ce stade, les tables d’identité ASP.NET sont créés sur la base de données MySQL, et l’utilisateur est enregistré et connecté à l’application :</span><span class="sxs-lookup"><span data-stu-id="4f827-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
 <span data-ttu-id="4f827-200">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-200">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-201">]</span><span class="sxs-lookup"><span data-stu-id="4f827-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="4f827-202">Installation de l’outil de banc d’essai MySQL pour vérifier les données</span><span class="sxs-lookup"><span data-stu-id="4f827-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="4f827-203">Installer le **banc d’essai MySQL** outil à partir de la [page Téléchargements de MySQL](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="4f827-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="4f827-204">Dans l’Assistant installation : **sélection des fonctionnalités** onglet, sélectionnez **banc d’essai MySQL** sous **applications** section.</span><span class="sxs-lookup"><span data-stu-id="4f827-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="4f827-205">Lancez l’application et ajouter une nouvelle connexion en utilisant les données de chaîne de connexion à partir de la base de données MySQL de Azure que vous avez créé à la lacune de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="4f827-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="4f827-206">Après avoir établi la connexion, vérifiez que le **ASP.NET Identity** tables créées sur le **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="4f827-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="4f827-207">Vous verrez que tous les ASP.NET Identity requis tables sont créées comme indiqué dans l’image ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="4f827-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
 <span data-ttu-id="4f827-208">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-208">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-209">]</span><span class="sxs-lookup"><span data-stu-id="4f827-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="4f827-210">Inspecter le **aspnetusers** de table pour l’instance dont vous recherchez les entrées que vous inscrivez des nouveaux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="4f827-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
 <span data-ttu-id="4f827-211">[Cliquez sur l’image suivante pour la développer.</span><span class="sxs-lookup"><span data-stu-id="4f827-211">[Click the following image to expand it.</span></span> <span data-ttu-id="4f827-212">]</span><span class="sxs-lookup"><span data-stu-id="4f827-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
