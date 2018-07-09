---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Contrôles de Source de données | Microsoft Docs
author: microsoft
description: Le contrôle DataGrid dans ASP.NET 1.x a marqué une amélioration considérable de l’accès aux données dans les applications Web. Toutefois, il n’était pas aussi conviviale qu’il aurait pu être...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 15a09e8ac7da6d23216a92863ae7ce6db7ecd57a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809351"
---
<a name="data-source-controls"></a>Contrôles de Source de données
====================
par [Microsoft](https://github.com/microsoft)

> Le contrôle DataGrid dans ASP.NET 1.x a marqué une amélioration considérable de l’accès aux données dans les applications Web. Toutefois, il n’était pas aussi conviviale qu’il aurait pu être. Il exigeait une quantité considérable de code pour obtenir un grand nombre de fonctionnalités utile à partir de celui-ci. C’est le modèle dans tous les domaines d’accès aux données dans la version 1.x.


Le contrôle DataGrid dans ASP.NET 1.x a marqué une amélioration considérable de l’accès aux données dans les applications Web. Toutefois, il n’était pas aussi conviviale qu’il aurait pu être. Il exigeait une quantité considérable de code pour obtenir un grand nombre de fonctionnalités utile à partir de celui-ci. C’est le modèle dans tous les domaines d’accès aux données dans la version 1.x.

ASP.NET 2.0 avec répond en partie avec les contrôles de source de données. Les contrôles de source de données dans ASP.NET 2.0 offrent aux développeurs un modèle déclaratif pour la récupération des données, l’affichage des données et la modification des données. L’objectif de contrôles de source de données consiste à fournir une représentation cohérente des données aux contrôles liés aux données, quel que soit la source de ces données. Au cœur des contrôles de source de données dans ASP.NET 2.0 est la classe abstraite DataSourceControl. La classe DataSourceControl fournit une implémentation de base de l’interface de IDataSource et de l’interface IListSource, ce dernier qui vous autorise à affecter le contrôle de source de données en tant que la source de données d’un contrôle lié aux données (par le biais de la nouvelle propriété DataSourceId abordée plus tard) et exposer les données qui y sont sous forme de liste. Chaque liste de données à partir d’un contrôle de source de données est exposée en tant qu’objet DataSourceView. Accès aux instances DataSourceView est fourni par l’interface IDataSource. Par exemple, la méthode GetViewNames retourne une ICollection qui vous permet d’énumérer les DataSourceViews associé à un contrôle de source de données particulière, et la méthode GetView vous permet d’accéder à une instance de DataSourceView particulière en nom.

Contrôles de source de données n’ont aucune interface utilisateur. Ils sont implémentés en tant que contrôles de serveur afin qu’ils peuvent prendre en charge la syntaxe déclarative et afin qu’ils aient accès à l’état de la page si vous le souhaitez. Contrôles de source de données ne s’affichent pas toutes les balises HTML au client.

> [!NOTE]
> Comme vous le verrez plus tard, il sont également la mise en cache les avantages obtenus à l’aide de contrôles de source de données.


## <a name="storing-connection-strings"></a>Stockage de chaînes de connexion

Avant de passer à regarder comment configurer des contrôles de source de données, nous devons couvrent une nouveauté dans ASP.NET 2.0 concernant les chaînes de connexion. ASP.NET 2.0 introduit une nouvelle section dans le fichier de configuration qui vous permet de stocker aisément des chaînes de connexion qui peuvent être lues de manière dynamique lors de l’exécution. Le &lt;connectionStrings&gt; section vous permet de stocker des chaînes de connexion.

L’extrait de code ci-dessous ajoute une nouvelle chaîne de connexion.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Comme avec la &lt;appSettings&gt; section, le &lt;connectionStrings&gt; section apparaît en dehors de la &lt;system.web&gt; section dans le fichier de configuration.


Pour utiliser cette chaîne de connexion, vous pouvez utiliser la syntaxe suivante lors de la définition de l’attribut ConnectionString d’un contrôle serveur.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

Le &lt;connectionStrings&gt; section peut également être chiffrée afin que des informations sensibles ne sont pas exposées. Cette capacité est abordée dans un autre module.

## <a name="caching-data-sources"></a>La mise en cache des Sources de données

Chaque DataSourceControl fournit quatre propriétés de configuration de la mise en cache ; EnableCaching CacheDuration, CacheExpirationPolicy et CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching est une propriété booléenne qui détermine si la mise en cache est activée pour le contrôle de source de données.

## <a name="cacheduration-property"></a>Propriété CacheDuration

La propriété CacheDuration définit le nombre de secondes pendant lesquelles le cache reste valid. Si cette propriété **0** , le cache reste valide jusqu'à ce qu’explicitement.

## <a name="cacheexpirationpolicy-property"></a>Propriété de CacheExpirationPolicy

La propriété CacheExpirationPolicy peut être définie sur **absolu** ou **coulissante**. La valeur absolue signifie que la quantité maximale de temps les données seront mise en cache est le nombre de secondes spécifié par la propriété CacheDuration. En lui affectant fenêtre glissante, le délai d’expiration est réinitialisé lors de chaque opération est effectuée.

## <a name="cachekeydependency-property"></a>Propriété CacheKeyDependency

Si une valeur de chaîne est spécifiée pour la propriété CacheKeyDependency, ASP.NET définit une nouvelle dépendance de cache basé sur cette chaîne. Cela vous permet à explicitement invalider le cache en modifiant ou en supprimant le CacheKeyDependency simplement.

**Important**: si l’emprunt d’identité est activé et l’accès à la source de données et/ou le contenu de données est basée sur l’identité du client, il est recommandé que la mise en cache désactivée en définissant EnableCaching sur False. Si la mise en cache est activée dans ce scénario et un utilisateur autre que l’utilisateur qui a demandé les données émet une demande, l’autorisation à la source de données n’est pas appliquée. Les données seront traitées simplement à partir du cache.

## <a name="the-sqldatasource-control"></a>Le contrôle SqlDataSource

Le contrôle SqlDataSource permet au développeur d’accéder aux données stockées dans une base de données relationnelle qui prend en charge ADO.NET. Il peut utiliser le fournisseur System.Data.SqlClient pour accéder à une base de données SQL Server, le fournisseur System.Data.OleDb, le fournisseur System.Data.Odbc ou le fournisseur System.Data.OracleClient pour accéder à Oracle. Par conséquent, SqlDataSource est certainement pas uniquement utilisé pour accéder aux données dans une base de données SQL Server.

Pour pouvoir utiliser SqlDataSource, vous simplement fournissez une valeur pour la propriété ConnectionString et spécifier une commande SQL ou procédure stockée. Le contrôle SqlDataSource s’occupe de l’utilisation de l’architecture ADO.NET sous-jacent. Il ouvre la connexion, interroge la source de données ou exécute la procédure stockée, retourne les données, puis ferme la connexion pour vous.

> [!NOTE]
> Étant donné que la classe DataSourceControl ferme automatiquement la connexion pour vous, il doit réduire le nombre d’appels de client généré par la fuite des connexions de base de données.


L’extrait de code suivant lie un contrôle DropDownList à un contrôle SqlDataSource à l’aide de la chaîne de connexion qui est stockée dans le fichier de configuration, comme indiqué ci-dessus.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Comme illustré ci-dessus, la propriété DataSourceMode de SqlDataSource Spécifie le mode pour la source de données. Dans l’exemple ci-dessus, le DataSourceMode est défini à DataReader. Dans ce cas, SqlDataSource retournera un objet IDataReader à l’aide d’un curseur avant uniquement et en lecture seule. Le type spécifié de l’objet retourné est contrôlé par le fournisseur qui est utilisé. Dans ce cas, j’utilise le fournisseur System.Data.SqlClient tel que spécifié dans le &lt;connectionStrings&gt; section du fichier web.config. Par conséquent, l’objet retourné sera de type SqlDataReader. En spécifiant une valeur DataSourceMode du jeu de données, les données peuvent être stockées dans un jeu de données sur le serveur. Ce mode vous permet d’ajouter des fonctionnalités telles que le tri, la pagination, etc. Si j’avais été la liaison de données SqlDataSource à un contrôle GridView, j’ai choisissions le mode de jeu de données. Toutefois, dans le cas d’un contrôle DropDownList, le mode de DataReader est le bon choix.

> [!NOTE]
> Lors de la mise en cache un SqlDataSource ou un AccessDataSource, vous devez définir la propriété DataSourceMode au jeu de données. Une exception se produit si vous activez la mise en cache avec un DataSourceMode de DataReader.


## <a name="sqldatasource-properties"></a>Propriétés de SqlDataSource

Voici certaines des propriétés du contrôle SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valeur booléenne qui spécifie si une commande select est annulée si l’un des paramètres a la valeur null. True par défaut.

### <a name="conflictdetection"></a>ConflictDetection

Dans une situation où plusieurs utilisateurs peuvent être mise à jour une source de données en même temps, la propriété ConflictDetection détermine le comportement du contrôle SqlDataSource. Cette propriété correspond à une des valeurs de l’énumération ConflictOptions. Ces valeurs sont **CompareAllValues** et **OverwriteChanges**. Si set OverwriteChanges, la dernière personne à écrire des données dans la source de données remplace toutes les modifications précédentes. Toutefois, si la propriété ConflictDetection est définie à CompareAllValues, paramètres sont créés pour les colonnes retournées par SelectCommand et paramètres sont également créées pour stocker les valeurs d’origine dans chacune de ces colonnes autorisant SqlDataSource à déterminer si les valeurs ont changé dans la mesure où SelectCommand a été exécutée.

### <a name="deletecommand"></a>DeleteCommand

Définit ou obtient la chaîne SQL utilisée lors de la suppression de lignes à partir de la base de données. Il peut s’agir d’une requête SQL ou un nom de procédure stockée.

### <a name="deletecommandtype"></a>DeleteCommandType

Définit ou obtient le type de commande de suppression, soit une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Retourne les paramètres utilisés par la propriété DeleteCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Cette propriété est utilisée pour spécifier le format des paramètres des valeurs d’origine dans les cas où la propriété ConflictDetection a CompareAllValues. La valeur par défaut est {0} ce qui signifie que les paramètres de valeur d’origine prend le même nom que le paramètre d’origine. En d’autres termes, si le nom du champ est EmployeeID, le paramètre de valeur d’origine serait @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Définit ou obtient la chaîne SQL qui est utilisée pour récupérer des données à partir de la base de données. Il peut s’agir d’une requête SQL ou un nom de procédure stockée.

### <a name="selectcommandtype"></a>SelectCommandType

Définit ou obtient le type de commande select, soit une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Retourne les paramètres qui sont utilisés par SelectCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Obtient ou définit le nom d’un paramètre de procédure stockée qui est utilisé lorsque le tri des données récupérées par le contrôle de source de données. Valide uniquement lorsque SelectCommandType est défini sur StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Chaîne spécifiant les bases de données et les tables utilisées dans une dépendance de cache SQL Server, séparés par un point-virgule. (Les dépendances de cache SQL seront abordées dans un autre module.)

### <a name="updatecommand"></a>UpdateCommand

Définit ou obtient la chaîne SQL qui est utilisée lors de la mise à jour des données dans la base de données. Il peut s’agir d’une requête SQL ou un nom de procédure stockée.

### <a name="updatecommandtype"></a>UpdateCommandType

Définit ou obtient le type de commande de mise à jour, soit une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Retourne les paramètres utilisés par la propriété UpdateCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

## <a name="the-accessdatasource-control"></a>Le contrôle AccessDataSource

Le contrôle AccessDataSource dérive de la classe SqlDataSource et est utilisé pour lier les données à une base de données Microsoft Access. La propriété ConnectionString pour le contrôle AccessDataSource est une propriété en lecture seule. Au lieu d’utiliser la propriété ConnectionString, la propriété de fichier de données est utilisée pour pointer vers la base de données Access comme indiqué ci-dessous.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

Le AccessDataSource sera toujours la valeur de base SqlDataSource ProviderName System.Data.OleDb et se connecte à la base de données à l’aide du fournisseur OLE DB Microsoft.Jet.OLEDB.4.0. Vous ne pouvez pas utiliser le contrôle AccessDataSource pour se connecter à une base de données Access protégé par mot de passe. Si vous devez vous connecter à une base de données protégé par mot de passe, vous devez utiliser le contrôle SqlDataSource.

> [!NOTE]
> Bases de données Access stockées dans le site Web doivent être placés dans l’application\_répertoire de données. ASP.NET n’autorise pas les fichiers dans ce répertoire à Explorer. Vous devez accorder des autorisations en lecture et en écriture à l’application le compte de processus\_répertoire de données lors de l’utilisation de bases de données Access.


## <a name="the-xmldatasource-control"></a>Le contrôle XmlDataSource

XmlDataSource est utilisé pour lier des données XML à des contrôles liés aux données. Vous pouvez lier à un fichier XML à l’aide de la propriété de fichier de données, ou vous pouvez lier à une chaîne XML à l’aide de la propriété de données. XmlDataSource expose des attributs XML en tant que champs pouvant être liées. Dans le cas où vous devez lier à des valeurs qui ne sont pas représentés en tant qu’attributs, vous devez utiliser une transformation XSL. Vous pouvez également utiliser des expressions XPath pour filtrer les données XML.

Prenez en compte le fichier XML suivant :

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Notez que XmlDataSource utilise une propriété XPath de *personnes ou d’une personne* afin de filtrer sur uniquement le &lt;personne&gt; nœuds. L’objet DropDownList puis données établit une liaison à l’attribut LastName à l’aide de la propriété DataTextField.

Tandis que le contrôle XmlDataSource est principalement utilisé pour lier les données à des données XML en lecture seule, il est possible de modifier le fichier de données XML. Notez que dans ce cas, l’insertion automatique, la mise à jour et suppression d’informations dans le fichier XML n’est pas automatique comme il le fait avec d’autres contrôles de source de données. Au lieu de cela, vous devrez écrire du code pour modifier manuellement les données à l’aide des méthodes suivantes du contrôle XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Récupère un objet XmlDocument qui contient le code XML récupéré par le XmlDataSource.

### <a name="save"></a>Enregistrer

Enregistre le XmlDocument en mémoire à la source de données.

Il est important de savoir que la méthode Save fonctionne uniquement lorsque les deux conditions suivantes sont remplies :

1. XmlDataSource est à l’aide de la propriété de fichier de données à lier à un fichier XML au lieu de la propriété de données à lier aux données XML en mémoire.
2. Aucune transformation n’est spécifiée via la propriété de transformation ou TransformFile.

Notez également que la méthode Save peut produire des résultats inattendus lorsqu’elle est appelée par plusieurs utilisateurs simultanément.

## <a name="the-objectdatasource-control"></a>Le contrôle ObjectDataSource

Les contrôles de source de données que nous avons décrites jusqu’ici sont un choix excellent pour les applications à deux niveaux où le contrôle de source de données communique directement avec le magasin de données. Toutefois, de nombreuses applications du monde réel sont des applications multiniveau où un contrôle de source de données peut-être avoir besoin communiquer avec un objet métier qui, à son tour, communique avec la couche données. Dans ces situations, ObjectDataSource remplit la facture parfaitement. ObjectDataSource fonctionne conjointement avec un objet source. Le contrôle ObjectDataSource crée une instance de l’objet source, appelez la méthode spécifiée et supprime l’instance d’objet tout dans l’étendue d’une demande unique, si votre objet possède des méthodes d’instance au lieu de méthodes statiques (Shared en Visual Basic). Par conséquent, votre objet doit être sans état. Autrement dit, votre objet doit acquérir et libérer toutes les ressources nécessaires au sein de l’étendue d’une demande unique. Vous pouvez contrôler la façon dont l’objet source est créé en gérant l’événement ObjectCreating du contrôle ObjectDataSource. Vous pouvez créer une instance de l’objet source et puis définissez la propriété ObjectInstance de la classe ObjectDataSourceEventArgs à cette instance. Le contrôle ObjectDataSource utilisera l’instance qui est créé dans l’événement ObjectCreating au lieu de créer une instance sur son propre.

Si l’objet source pour un contrôle ObjectDataSource expose des méthodes statiques publiques (Shared en Visual Basic) qui peuvent être appelés pour extraire et modifier des données, un contrôle ObjectDataSource appelle ces méthodes directement. Si un contrôle ObjectDataSource doit créer une instance de l’objet source afin d’effectuer des appels de méthode, l’objet doit inclure un constructeur public qui n’accepte aucun paramètre. Le contrôle ObjectDataSource appelle ce constructeur lorsqu’il crée une nouvelle instance de l’objet source.

Si l’objet source ne contient pas de constructeur public sans paramètres, vous pouvez créer une instance de l’objet source qui sera utilisé par le contrôle ObjectDataSource dans l’événement ObjectCreating.

## <a name="specifying-object-methods"></a>Spécification des méthodes d’objet

L’objet source pour un contrôle ObjectDataSource peut contenir un nombre illimité de méthodes qui sont utilisées pour sélectionner, insérer, mettre à jour ou supprimer des données. Ces méthodes sont appelées par le contrôle ObjectDataSource basé sur le nom de la méthode, tel qu’identifié à l’aide de la méthode SelectMethod, InsertMethod, UpdateMethod, DeleteMethod propriété ou du contrôle ObjectDataSource. L’objet source peut également inclure une méthode SelectCount facultative, ce qui est identifiée par le contrôle ObjectDataSource à l’aide de la propriété SelectCountMethod, qui retourne le nombre du nombre total d’objets à la source de données. Le contrôle ObjectDataSource appelle la méthode SelectCount après qu’une méthode Select a été appelée pour récupérer le nombre total d’enregistrements à la source de données pour une utilisation lors de la pagination.

## <a name="lab-using-data-source-controls"></a>Laboratoire à l’aide de contrôles de Source de données

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Exercice 1 : affichage des données avec le contrôle SqlDataSource

L’exercice suivant utilise le contrôle SqlDataSource pour se connecter à la base de données Northwind. Il suppose que vous avez accès à la base de données Northwind sur une instance de SQL Server 2000.

1. Créez un site web ASP.NET.
2. Ajouter un nouveau fichier web.config.

    1. Avec le bouton droit sur le projet dans l’Explorateur de solutions, puis cliquez sur Ajouter un nouvel élément.
    2. Choisissez le fichier de Configuration Web à partir de la liste des modèles et cliquez sur Ajouter.
3. Modifier le &lt;connectionStrings&gt; section comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Basculez en mode Code et ajoutez un attribut ConnectionString et un attribut de SelectCommand à la &lt;asp : SqlDataSource&gt; contrôler comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. À partir de la vue de conception, ajoutez un nouveau contrôle GridView.
6. Dans la liste déroulante Choisir la Source de données dans le menu Tâches GridView, choisissez SqlDataSource1.
7. Avec le bouton droit sur Default.aspx et choisissez Afficher dans le navigateur à partir du menu. Cliquez sur Oui lorsque vous êtes invité à enregistrer.
8. Le contrôle GridView affiche les données à partir de la table Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Exercice 2 : modification des données avec le contrôle SqlDataSource

L’exercice suivant montre comment lier des données DropDownList contrôler à l’aide de la syntaxe déclarative et permet de modifier les données présentées dans le contrôle DropDownList.

1. En mode conception, supprimez le contrôle GridView Default.aspx. 

    **Important**: laissez ce contrôle SqlDataSource sur la page.
2. Ajouter un contrôle DropDownList à Default.aspx.
3. Basculer en mode Source.
4. Ajoutez un attribut DataSourceId, DataTextField et DataValueField à la &lt;asp : DropDownList&gt; contrôler comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Enregistrez Default.aspx et affichez-le dans le navigateur. Notez que l’objet DropDownList contient tous les produits à partir de la base de données Northwind.
6. Fermez le navigateur.
7. Dans la vue de Source de Default.aspx, ajoutez un nouveau contrôle de zone de texte sous le contrôle DropDownList. Modifier la propriété ID de la zone de texte à txtProductName.
8. Sous le contrôle de zone de texte, ajoutez un nouveau contrôle de bouton. Modifier la propriété ID du bouton btnUpdate et la propriété de texte à **nom de produit de mise à jour**.
9. Dans la vue de Source de Default.aspx, ajoutez une propriété UpdateCommand et deux UpdateParameters nouveau comme suit à la balise SqlDataSource : 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Notez qu’il existe que deux paramètres (ProductName et ProductID) ajoutés dans ce code de mise à jour. Ces paramètres sont mappés à la propriété de texte de la zone de texte txtProductName et la propriété SelectedValue de la ddlProducts DropDownList.
10. Basculez en mode Design et double-cliquez sur le contrôle bouton pour ajouter un gestionnaire d’événements.
11. Ajoutez le code suivant à la btnUpdate\_cliquez sur le code : 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Avec le bouton droit sur Default.aspx, puis choisissez et les afficher dans le navigateur. Lorsque vous êtes invité à enregistrer toutes les modifications, cliquez sur Oui.
13. ASP.NET 2.0 les classes partielles permettent de compilation lors de l’exécution. Il n’est pas nécessaire générer une application afin de voir les modifications de code prendre effet.
14. Sélectionnez un produit dans la liste DropDownList.
15. Entrez un nouveau nom pour le produit sélectionné dans la zone de texte, puis sur le bouton de mise à jour.
16. Le nom du produit est mis à jour dans la base de données.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Exercice 3 en utilisant le contrôle ObjectDataSource

Cet exercice vous montrer comment utiliser le contrôle ObjectDataSource et un objet source pour interagir avec la base de données Northwind.

1. Avec le bouton droit sur le projet dans l’Explorateur de solutions, puis cliquez sur Ajouter un nouvel élément.
2. Sélectionnez le formulaire Web dans la liste des modèles. Remplacez le nom object.aspx et cliquez sur Ajouter.
3. Avec le bouton droit sur le projet dans l’Explorateur de solutions, puis cliquez sur Ajouter un nouvel élément.
4. Sélectionnez la classe dans la liste des modèles. Remplacez le nom de la classe NorthwindData.cs et cliquez sur Ajouter.
5. Cliquez sur Oui lorsque vous êtes invité à ajouter la classe à l’application\_dossier de Code.
6. Ajoutez le code suivant au fichier NorthwindData.cs : 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Ajoutez le code suivant à la vue de Source de object.aspx : 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Enregistrez tous les fichiers et parcourir object.aspx.
9. Interagir avec l’interface en affichant les détails, modification des employés, l’ajout des employés et suppression des employés.
