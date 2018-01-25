---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "Création d’une chaîne de connexion et l’utilisation de base de données SQL Server locale | Documents Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="8cfa9-102">Création d’une chaîne de connexion et l’utilisation de base de données SQL Server locale</span><span class="sxs-lookup"><span data-stu-id="8cfa9-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="8cfa9-103">Par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8cfa9-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="8cfa9-104">Création d’une chaîne de connexion et l’utilisation de base de données SQL Server locale</span><span class="sxs-lookup"><span data-stu-id="8cfa9-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="8cfa9-105">Le `MovieDBContext` classe créée gère la tâche de connexion à la base de données et de mappage `Movie` objets aux enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="8cfa9-106">Une question que vous vous demandez peut-être, cependant, est comment spécifier une base de données auquel il se connecte à.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="8cfa9-107">Vous n’avez pas réellement spécifier la base de données à utiliser, Entity Framework utilise par défaut [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="8cfa9-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="8cfa9-108">Dans cette section, nous ajouterons explicitement une chaîne de connexion dans le *Web.config* fichier de l’application.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="8cfa9-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="8cfa9-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="8cfa9-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) est une version allégée du moteur SQL Server Express de base de données qui démarre à la demande et s’exécute en mode utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="8cfa9-111">Base de données locale s’exécute dans un mode spécial d’exécution de SQL Server Express qui vous permet de travailler avec les bases de données en tant que *.mdf* fichiers.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="8cfa9-112">En règle générale, les fichiers de base de données de base de données locale sont conservés dans le *application\_données* dossier d’un projet web.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="8cfa9-113">SQL Server Express n’est pas recommandée pour une utilisation dans les applications web de production.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="8cfa9-114">LocalDB notamment ne doit pas être utilisé pour la production avec une application web, car il n’est pas conçu pour fonctionner avec les services IIS.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="8cfa9-115">Toutefois, une base de données de la base de données locale peut facilement être migré dans SQL Server ou SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="8cfa9-116">Dans Visual Studio 2017, base de données locale est installé par défaut avec Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="8cfa9-117">Par défaut, Entity Framework recherche une chaîne de connexion que le même nom que la classe de contexte d’objet (`MovieDBContext` pour ce projet).</span><span class="sxs-lookup"><span data-stu-id="8cfa9-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="8cfa9-118">Pour plus d’informations, consultez [chaînes de connexion SQL Server pour les Applications Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="8cfa9-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="8cfa9-119">Ouvrez la racine de l’application *Web.config* fichier ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="8cfa9-120">(Pas le *Web.config* de fichiers dans le *vues* dossier.)</span><span class="sxs-lookup"><span data-stu-id="8cfa9-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="8cfa9-121">Rechercher les `<connectionStrings>` élément :</span><span class="sxs-lookup"><span data-stu-id="8cfa9-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="8cfa9-122">Ajouter la chaîne de connexion à la `<connectionStrings>` élément dans le *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="8cfa9-123">L’exemple suivant montre une partie de la *Web.config* fichier avec la nouvelle chaîne de connexion ajoutée :</span><span class="sxs-lookup"><span data-stu-id="8cfa9-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="8cfa9-124">Les deux chaînes de connexion sont très similaires.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-124">The two connection strings are very similar.</span></span> <span data-ttu-id="8cfa9-125">La première chaîne de connexion est nommée `DefaultConnection` et est utilisé pour la base de données d’appartenance pour contrôler qui peut accéder à l’application.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="8cfa9-126">Vous avez ajouté la chaîne de connexion spécifie une base de données de la base de données locale nommée *Movie.mdf* situé dans le *application\_données* dossier.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="8cfa9-127">Nous ne sera pas utiliser la base de données d’appartenance dans ce didacticiel, pour plus d’informations sur l’appartenance, l’authentification et la sécurité, consultez mon didacticiel [créer une application ASP.NET MVC avec l’authentification et de la base de données SQL et le déployer vers Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="8cfa9-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="8cfa9-128">Le nom de la chaîne de connexion doit correspondre au nom de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="8cfa9-129">Vous n’avez pas réellement besoin ajouter le `MovieDBContext` chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="8cfa9-130">Si vous ne spécifiez pas une chaîne de connexion, Entity Framework crée une base de données de la base de données locale dans le répertoire des utilisateurs avec le nom qualifié complet de le [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (dans ce cas `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="8cfa9-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="8cfa9-131">Vous pouvez nommer la base de données comme vous le souhaitez, tant qu’il a le *. MDF* suffixe.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="8cfa9-132">Par exemple, nous avons nom de la base de données *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="8cfa9-133">Ensuite, vous allez générer un nouveau `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer de nouvelles listes de film.</span><span class="sxs-lookup"><span data-stu-id="8cfa9-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8cfa9-134">[Précédent](adding-a-model.md)
[Suivant](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="8cfa9-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
