---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Configuration de l’Application Web de Production à utiliser la base de données de Production (c#) | Microsoft Docs
author: rick-anderson
description: Comme indiqué dans les didacticiels précédents, il n’est pas rare que les informations de configuration diffèrent entre les environnements de développement et de production. Il s’agit d’es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: e2560ae7a845c27f7fa5f420eaf1d89618397c60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838983"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Configuration de l’Application Web de Production à utiliser la base de données de Production (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Comme indiqué dans les didacticiels précédents, il n’est pas rare que les informations de configuration diffèrent entre les environnements de développement et de production. Cela est particulièrement vrai pour les applications web orientées données, comme les chaînes de connexion de base de données diffèrent entre les environnements de développement et de production. Ce didacticiel explore plusieurs moyens pour configurer l’environnement de production afin d’inclure la chaîne de connexion appropriée plus en détail.


## <a name="introduction"></a>Introduction

Les applications web orientées données utilisent généralement une autre base de données en cas de développement que lorsque en production. Pour les applications hébergées par un fournisseur d’hébergement web et développé localement, la base de données de développement se trouve généralement sur l’ordinateur de s développeur tandis que la base de données de production est hébergée sur un serveur de base de données sur le web d’entreprise s fournisseur d’hébergement. Déploiement d’une application web pilotée par des données implique la copie de la base de données de développement vers le serveur de base de données de production. Dans le didacticiel précédent, nous avons vu comment effectuer cette étape.

L’application web utilise les informations contenues dans un *chaîne de connexion* pour établir une connexion avec la base de données. La chaîne de connexion, qui est généralement stockée dans `Web.config`, spécifie le nom du serveur de base de données, le nom de la base de données, le contexte de sécurité et d’autres informations. Étant donné que la base de données utilisé par l’application web dépend de si l’application web est en cours d’exécution dans les environnements de développement ou de production, les chaînes de connexion doivent différer entre les deux environnements.

Il n’est pas rare que les informations de configuration diffèrent entre les environnements de développement et de production. Le *différences entre développement Configuration commun et Production* didacticiel a décrit les techniques pour gérer les informations de configuration distincts entre ces deux environnements, mais aussi une brève discussion sur chaînes de connexion de base de données. Ce didacticiel explore plusieurs moyens pour configurer l’environnement de production afin d’inclure la chaîne de connexion appropriée plus en détail.

## <a name="examining-the-connection-string-information"></a>Examiner les informations de chaîne de connexion

La chaîne de connexion utilisée par l’application web de critiques de livres est stockée dans le fichier de configuration application s `Web.config`. `Web.config` inclut une section spéciale pour le stockage des chaînes de connexion, bien nommés [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Le `Web.config` fichier pour le site Web de critiques de livres comporte une chaîne de connexion définie dans cette section nommée `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

La chaîne de connexion - Source de données =. \SQLEXPRESS ; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated Security = True ; User Instance = True - se compose d’un nombre d’options et valeurs, avec les paires de valeur de l’option/délimitées par un point-virgule et chaque option et valeur délimitée par un signe égal. Les quatre options utilisées dans cette chaîne de connexion sont :

- `Data Source` -Spécifie l’emplacement du serveur de base de données et le nom d’instance de serveur de base de données (le cas échéant). La valeur, `.\SQLEXPRESS`, est un exemple où il existe un serveur de base de données et un nom d’instance. La période de Spécifie que le serveur de base de données est sur le même ordinateur que l’application ; le nom d’instance est `SQLEXPRESS`.
- `AttachDbFilename` -Spécifie l’emplacement du fichier de base de données. La valeur contient l’espace réservé `|DataDirectory|`, qui est résolu sur le chemin d’accès complet de l’application s `App_Data` dossier lors de l’exécution.
- `Integrated Security` -une valeur booléenne qui indique s’il faut utiliser un nom d’utilisateur/mot de passe spécifié lors de la connexion à la base de données (false) ou le Windows actuel des informations d’identification de compte (true).
- `User Instance` -une option de configuration spécifique pour les éditions SQL Server Express qui indique s’il faut autoriser les utilisateurs non administratifs sur l’ordinateur local, attachement et se connecter à une base de données SQL Server Express Edition. Consultez [SQL Server Express User Instances](https://msdn.microsoft.com/library/ms254504.aspx) pour plus d’informations sur ce paramètre.
  

Les options de chaîne de connexion autorisés dépendent de la base de données que vous êtes connecté et le fournisseur de base de données ADO.NET utilisé. Par exemple, la chaîne de connexion pour la connexion à Microsoft SQL Server diffère de la base de données utilisé pour se connecter à une base de données Oracle. De même, la connexion à une base de données Microsoft SQL Server via le fournisseur SqlClient utilise une chaîne de connexion différente que quand vous utilisez le fournisseur OLE DB.

Vous pouvez générer la chaîne de connexion de base de données manuellement à l’aide d’un site tel que [ConnectionStrings.com](http://www.connectionstrings.com/) en tant que ressource de quelles options sont disponibles. Toutefois, une approche plus simple consiste à ajouter la base de données à l’Explorateur de serveurs dans Visual Studio, puis attrapez la chaîne de connexion à partir de la fenêtre Propriétés. Permettent d’utiliser cette technique pour construire la chaîne de connexion au serveur de base de données de production s.

Ouvrez Visual Studio, puis accédez à la fenêtre Explorateur de serveurs (dans Visual Web Developer, cette fenêtre est appelée l’Explorateur de base de données). Avec le bouton droit sur l’option des connexions de données et choisissez l’option Ajouter une connexion dans le menu contextuel. Ceci fait apparaître l’Assistant illustré à la Figure 1. Choisissez la source de données approprié, puis cliquez sur Continuer.


[![Choisissez d’ajouter une nouvelle base de données à l’Explorateur de serveurs](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Figure 1**: choisissez d’ajouter une nouvelle base de données à l’Explorateur de serveurs ([cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Ensuite, spécifiez les informations de connexion de la base de données (voir Figure 2). Lorsque vous avez souscrit avec votre entreprise d’hébergement web ils doivent avoir fourni des informations sur la façon de se connecter à la base de données - le nom du serveur de base de données, le nom de la base de données, le nom d’utilisateur et le mot de passe à utiliser pour se connecter à la base de données et ainsi de suite. Après avoir entré ces informations, cliquez sur OK pour terminer cet Assistant et ajouter la base de données à l’Explorateur de serveurs.


[![Spécifiez les informations de connexion de base de données](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Figure 2**: spécifiez les informations de connexion de base de données ([cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


La base de données des environnement de production doit figurer dans l’Explorateur de serveurs. Sélectionnez la base de données à partir de l’Explorateur de serveurs et accédez à la fenêtre Propriétés. Vous y trouverez une propriété de chaîne de connexion avec la chaîne de connexion de base de données s nommée. En supposant que vous utilisez une base de données Microsoft SQL Server sur la production et le fournisseur SqlClient votre chaîne de connexion doit ressembler à ce qui suit :

<strong>Source de données =<em>nom_serveur</em>; Catalogue initial =<em>databaseName</em>; Persist Security Info = True ; ID d’utilisateur =<em>nom d’utilisateur</em>; Mot de passe =*mot de passe</strong>*

Où *nom_serveur*, *databaseName*, *nom d’utilisateur*, et *mot de passe* sont avec les valeurs pour le nom du serveur de base de données, la base de données nom et le nom d’utilisateur et mot de passe fournie par votre entreprise d’hôte web.

## <a name="deploying-the-book-reviews-web-application"></a>Déploiement de l’Application Web de révisions livre

Le didacticiel précédent a présenté la copie de la base de données de développement à l’environnement de production, mais Explorer pas le déploiement de l’application orientée données. À ce stade, l’environnement de production contient la base de données mais est à l’aide de la version de l’application de livre passe en revue avec les révisions statiques. Nous avons besoin de déployer l’application de nouvelle, pilotés par les données sur le serveur de production, ainsi que les informations de configuration mis à jour.

Prenez un moment pour déployer l’application pilotée par les données à partir de l’environnement de développement en production. Ce processus a été traité en détail dans les didacticiels précédents. Si vous avez besoin d’un rappel, consultez la *déploiement de votre site Web à l’aide d’un FTP Client* ou *déploiement de votre site Web à l’aide de Visual Studio* didacticiels. Vous devrez vous assurer que la chaîne de connexion de base de données de production est celui qui est utilisé dans l’environnement de production, ce qui signifie qu’un autre `Web.config` fichier doit être déployé. Plus précisément, ce paramètre modifié `Web.config` fichier s `<connectionStrings>` élément doit contenir la chaîne de connexion de base de données de production et doit ressembler à ce qui suit :

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Notez que la chaîne de connexion dans le `<connectionStrings>` élément porte le même nom (`ReviewsConnectionString`), mais contient désormais la chaîne de connexion de base de données de production au lieu de la chaîne de connexion de base de données de développement.

Sauf si vous avez un workflow de déploiement plus formel, soit modifier manuellement la `Web.config` fichier à utiliser la chaîne de connexion de base de données de production avant de déployer (n’oubliez pas de revenir en arrière à l’aide de la chaîne de connexion de base de données de développement par la suite) ou de maintenir un distinct `Web.config` fichier avec les informations de configuration des environnement de production qui sont chargées dans l’environnement de production dans le cadre du processus de déploiement.

> [!NOTE]
> Si vous déployez accidentellement un `Web.config` fichier qui contient la chaîne de connexion de base de données de développement, puis il y aura une erreur lors de l’application de production tente de se connecter à la base de données. Cette erreur se manifeste comme un `SqlException` avec un message indiquant que le serveur est introuvable ou n’est pas accessible.


Une fois que le site a été déployé en production, visitez le site de production via votre navigateur. Vous devez voir et profitez de la même expérience utilisateur en tant que lors de l’exécution de l’application orientée données localement. Bien sûr lorsque vous visitez le site Web de production le site est alimenté par le serveur de base de données de production, tandis que sur le site Web dans l’environnement de développement utilise la base de données dans le développement. La figure 3 illustre le *enseigner vous-même ASP.NET 3.5 des dernières 24 heures* passez en revue la page depuis le site Web dans l’environnement de production (Notez l’URL dans la barre d’adresse de navigateur s).


[![L’Application piloté par les données est maintenant disponible sur Production !](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Figure 3**: The Data-Driven Application est maintenant disponible sur Production ! ([Cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Stockage de chaînes de connexion dans un fichier de Configuration distinct

Une technique courante pour gérer les informations de configuration distinct dans les environnements de développement et de production consiste à avoir deux versions de la `Web.config`: un pour l’environnement de développement et un pour la production. Au moment du déploiement approprié `Web.config` version peut être copiée vers l’environnement de production. Dans l’idéal, ce processus est automatisé dans le cadre du flux de travail de déploiement.

Au lieu de maintenir deux distinct `Web.config` fichiers si vous le souhaitez, vous pouvez fournissent des différences plus granulaires. Les éléments qui composent le `Web.config` fichier peut être défini dans un fichiers de configuration externes qui sont ensuite référencés dans le `Web.config` fichier. En bref, vous pouvez avoir un `Web.config` de fichier pour les deux environnements qui fait référence à un fichier databaseConnectionStrings.config, qui contiendrait les chaînes de connexion utilisées par l’application et qu’il seraient uniques pour chaque environnement. Je trouve que, en séparant les différentes informations de configuration dans des fichiers séparés fournit une plus propre et plus simple `Web.config` fichier et bien plus encore clairement décrit les différences de configuration entre les environnements de développement et de production.

Pour utiliser cette technique, commencez par créer un nouveau dossier dans l’application web nommée `ConfigSections`. Ensuite, ajoutez deux fichiers dans ce nouveau dossier nommé databaseConnectionStrings.dev.config et databaseConnectionStrings.production.config. Ensuite, copiez la `<connectionStrings>` élément à partir de `Web.config` dans les fichiers databaseConnectionStrings.dev.config et databaseConnectionStrings.production.config, puis modifiez la chaîne de connexion dans le databaseConnectionStrings.production.config fichier afin qu’elle spécifie la chaîne de connexion de base de données de production. Par exemple, le fichier databaseConnectionStrings.dev.config doit contenir uniquement le `<connectionStrings>` élément avec une chaîne de connexion qui fait référence à la base de données de développement :

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

De même, le fichier databaseConnectionStrings.production.config doit contenir uniquement un `<connectionStrings>` élément, mais celui qui possède la chaîne de connexion de base de données de production.

Faites une copie du fichier databaseConnectionStrings.dev.config et nommez-le databaseConnectionStrings.config.

> [!NOTE]
> Vous pouvez nommer le fichier de configuration autre chose que databaseConnectionStrings.config, si d voulue, tel que `connectionStrings.config` ou `dbInfo.config`. Toutefois, veillez à nommer le fichier avec un `.config` extension comme `.config` fichiers sont, par défaut, pas pris en charge par le moteur ASP.NET. Si vous nommez le fichier quelque chose d’autre, tel que `connectionStrings.txt`, un utilisateur peut indiquer son navigateur pour [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) et afficher le contenu du fichier !


À ce stade le `ConfigSections` dossier doit contenir trois fichiers (voir Figure 4). Les fichiers databaseConnectionStrings.dev.config et databaseConnectionStrings.production.config contiennent les chaînes de connexion pour les environnements de développement et de production, respectivement. Le fichier databaseConnectionStrings.config contient les informations de chaîne de connexion qui seront utilisées par l’application web lors de l’exécution. Par conséquent, le fichier databaseConnectionStrings.config doit être identique au fichier databaseConnectionStrings.dev.config dans l’environnement de développement, alors que sur la production, le fichier databaseConnectionStrings.config doit être identique à databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Figure 4**: ConfigSections ([cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


Nous devons maintenant demander à `Web.config` à utiliser le fichier databaseConnectionStrings.config pour son magasin de chaînes de connexion. Ouvrez `Web.config` et remplacez l’élément `<connectionStrings>` existant par l’élément suivant :

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

Le `configSource` attribut spécifie un chemin d’accès physique relatif à la `Web.config` fichier. Si l’externe `.config` fichier se trouve dans le même répertoire que `Web.config` puis définissez cet attribut sur le nom de fichier de la `.config` fichier. Si elle s dans un sous-répertoire, comme c’est le cas avec databaseConnectionStrings.config, spécifiez le sous-dossier à l’aide d’une barre oblique inverse pour délimiter les noms de dossiers et de fichiers, tels que ConfigSections\databaseConnectionStrings.config.

Avec cette modification, les environnements de développement et de production contiennent les mêmes `Web.config` fichier. La seule différence est maintenant le fichier databaseConnectionStrings.config. Copiez le fichier databaseConnectionStrings.production.config en production et renommez-le databaseConnectionStrings.config. Si, à l’avenir, il existe des modifications apportées à la chaîne de connexion de base de données de production vous devrez rendre pour le fichier databaseConnectionStrings.production.config, puis chargez ce fichier en production, en le renommant databaseConnectionStrings.config.

> [!NOTE]
> Vous pouvez spécifier les informations pour toute `Web.config` élément dans un fichier distinct et utiliser le `configSource` attribut pour référencer ce fichier depuis `Web.config`.


## <a name="summary"></a>Récapitulatif

Applications orientées données utilisent généralement des bases de données différents dans les environnements de développement et de production. Par conséquent, les chaînes de connexion de base de données stockées dans la configuration de s d’application web doivent être uniques pour chaque environnement. Dans ce didacticiel, nous avons vu comment déterminer la chaîne de connexion de base de données de production et les façons de mettre à jour les informations de chaîne de connexion unique dans les deux environnements.

Bonne programmation !

#### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Chaînes de connexion et fichiers de configuration](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informations @ ConnectionStrings.com des chaînes de Configuration de la base de données](http://www.connectionstrings.com/)
- [Déplacer les paramètres du fichier Web.config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentation technique pour le &lt;connectionStrings&gt; élément](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Précédent](deploying-a-database-cs.md)
> [Suivant](configuring-a-website-that-uses-application-services-cs.md)
