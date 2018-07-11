---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Création d’une chaîne de connexion et l’utilisation de SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 8c8db3fc121b2ff263ab033404211e167e19cd71
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38168374"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Création d’une chaîne de connexion et l’utilisation de SQL Server LocalDB
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Création d’une chaîne de connexion et l’utilisation de SQL Server LocalDB

Le `MovieDBContext` classe que vous avez créé gère la tâche de connexion à la base de données et de mappage `Movie` objets aux enregistrements de base de données. Une question que vous vous demandez peut-être, cependant, consiste à spécifier quelle base de données, il se connecte à. Vous n’avez pas réellement spécifier la base de données à utiliser, Entity Framework utilisera par défaut [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). Dans cette section, nous ajouterons explicitement une chaîne de connexion dans le *Web.config* fichier de l’application.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) est une version légère de SQL Server Express Database Engine qui commence à la demande et s’exécute en mode utilisateur. Base de données locale s’exécute dans un mode spécial de l’exécution de SQL Server Express qui vous permet de travailler avec les bases de données en tant que *.mdf* fichiers. En règle générale, les fichiers de base de données de base de données locale sont conservés dans le *application\_données* dossier d’un projet web.

SQL Server Express n’est pas recommandée pour une utilisation dans les applications web de production. Base de données locale en particulier ne doit pas être utilisé pour la production avec une application web, car il n’est pas conçu pour fonctionner avec IIS. Toutefois, une base de données de base de données locale peut être facilement migrée vers SQL Server ou SQL Azure.

Dans Visual Studio 2017, LocalDB est installé par défaut avec Visual Studio.

Par défaut, Entity Framework recherche une chaîne de connexion que le même nom que la classe de contexte d’objet (`MovieDBContext` pour ce projet). Pour plus d’informations, consultez [chaînes de connexion SQL Server pour les Applications Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Ouvrez la racine de l’application *Web.config* fichier indiqué ci-dessous. (Pas le *Web.config* de fichiers dans le *vues* dossier.)

![](creating-a-connection-string/_static/image1.png)

Rechercher le `<connectionStrings>` élément :

![](creating-a-connection-string/_static/image2.png)

Ajoutez la chaîne de connexion suivante à la `<connectionStrings>` élément dans le *Web.config* fichier.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

L’exemple suivant montre une partie de la *Web.config* fichier avec la nouvelle chaîne de connexion ajoutée :

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Les deux chaînes de connexion sont très similaires. La première chaîne de connexion est nommée `DefaultConnection` et est utilisé pour la base de données d’appartenance pour contrôler qui peut accéder à l’application. Vous avez ajouté la chaîne de connexion spécifie une base de données de base de données locale nommée *Movie.mdf* situé dans le *application\_données* dossier. Nous ne sont pas dans ce didacticiel, pour plus d’informations sur l’appartenance, l’authentification et la sécurité, utilisez la base de données d’appartenance, consultez mon didacticiel [créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer sur Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Le nom de la chaîne de connexion doit correspondre au nom de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Vous n’avez pas besoin ajouter le `MovieDBContext` chaîne de connexion. Si vous ne spécifiez pas une chaîne de connexion, Entity Framework crée une base de données de base de données locale dans le répertoire d’utilisateurs avec le nom qualifié complet de le [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (dans ce cas `MvcMovie.Models.MovieDBContext`). Vous pouvez nommer la base de données comme vous le souhaitez, tant qu’il a le *. MDF* suffixe. Par exemple, nous pourrions nommer la base de données *MyFilms.mdf*.

Ensuite, vous allez générer une nouvelle `MoviesController` classe que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer de nouvelles listes de film.

> [!div class="step-by-step"]
> [Précédent](adding-a-model.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)
