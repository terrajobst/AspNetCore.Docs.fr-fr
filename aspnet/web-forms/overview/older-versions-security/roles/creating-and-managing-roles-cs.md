---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Créer et gérer des rôles (c#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel examine les étapes nécessaires à la configuration de l’infrastructure de rôles. Ensuite, nous allons créer des pages web pour créer et supprimer des rôles.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: af7f3a1c45f0e87f146e8fb1f3aff37a13847ba8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390787"
---
<a name="creating-and-managing-roles-c"></a>Créer et gérer des rôles (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Ce didacticiel examine les étapes nécessaires à la configuration de l’infrastructure de rôles. Ensuite, nous allons créer des pages web pour créer et supprimer des rôles.


## <a name="introduction"></a>Introduction

Dans le <a id="_msoanchor_1"> </a> [ *autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-cs.md) didacticiel, nous avons examiné à l’aide de l’autorisation d’URL pour restreindre certains utilisateurs à partir d’un ensemble de pages et exploré déclarative et techniques par programme pour ajuster les fonctionnalités d’une page ASP.NET en fonction de l’utilisateur visite. Une autorisation d’accès à la page ou d’une fonctionnalité utilisateur par utilisateur, cependant, peut devenir un cauchemar en termes de maintenance dans les scénarios où il existe plusieurs comptes d’utilisateur ou lorsque les privilèges des utilisateurs changent souvent. Chaque fois qu’un utilisateur gagne ou perd l’autorisation d’effectuer une tâche particulière, l’administrateur doit mettre à jour les règles d’autorisation URL appropriées, le balisage déclaratif et le code.

Il est généralement utile de classer les utilisateurs en groupes ou *rôles* , puis pour appliquer des autorisations sur une base de rôle par rôle. Par exemple, la plupart des applications web ont un certain ensemble de pages ou des tâches qui sont réservés uniquement pour les utilisateurs administratifs. En utilisant les techniques apprises dans le *autorisation basée sur l’utilisateur* didacticiel, nous devons ajouter les règles d’autorisation URL appropriées, le balisage déclaratif et le code pour autoriser les comptes d’utilisateur spécifié effectuer des tâches d’administration. Mais si un nouvel administrateur a été ajouté ou si un administrateur existant est nécessaire pour que ses droits d’administration révoqués, nous devrions retourner et mettre à jour les fichiers de configuration et les pages web. Toutefois, avec les rôles, nous pourrions créer un rôle appelé Administrateurs et affecter ces utilisateurs approuvés au rôle Administrateurs. Ensuite, nous ajoutons la règles d’autorisation URL appropriées, le balisage déclaratif et le code pour autoriser le rôle Administrateurs effectuer les différentes tâches d’administration. Avec cette infrastructure en place, l’ajout de nouveaux administrateurs au site ou la suppression d’existants est aussi simple en incluant ou en supprimant l’utilisateur du rôle Administrateurs. Aucune configuration, balisage déclaratif ou des modifications de code ne sont nécessaires.

ASP.NET offre une infrastructure de rôles pour la définition des rôles et leur association avec les comptes d’utilisateur. Avec l’infrastructure des rôles, nous pouvons créer et supprimer des rôles, ajouter des utilisateurs ou supprimer des utilisateurs à partir d’un rôle, déterminer l’ensemble des utilisateurs qui appartiennent à un rôle particulier et indiquer si un utilisateur appartient à un rôle particulier. Une fois que l’infrastructure de rôles a été configuré, nous pouvons limiter l’accès aux pages sur une base de rôle par rôle via des règles d’autorisation d’URL et afficher ou masquer des informations supplémentaires ou des fonctionnalités sur une page basée sur les rôles de l’utilisateur actuellement connecté.

Ce didacticiel examine les étapes nécessaires à la configuration de l’infrastructure de rôles. Ensuite, nous allons créer des pages web pour créer et supprimer des rôles. Dans le <a id="_msoanchor_2"> </a> [ *affectation de rôles aux utilisateurs* ](assigning-roles-to-users-cs.md) didacticiel, nous allons examiner comment ajouter et supprimer des utilisateurs des rôles. Puis, dans le <a id="_msoanchor_3"> </a> [ *autorisation basée sur le rôle* ](role-based-authorization-cs.md) didacticiel, nous verrons comment limiter l’accès aux pages sur une base de rôle par rôle, ainsi que comment ajuster en fonction de la fonctionnalité de page la visite rôle de l’utilisateur. C’est parti !

## <a name="step-1-adding-new-aspnet-pages"></a>Étape 1 : Ajout de nouvelles Pages ASP.NET

Dans ce didacticiel et les deux suivants, nous étudierons diverses fonctions liées aux rôles et fonctionnalités. Nous aurons besoin d’une série de pages ASP.NET pour implémenter les rubriques examinés tout au long de ces didacticiels. Nous allons créer ces pages et mettre à jour le plan du site.

Commencez par créer un nouveau dossier dans le projet nommé `Roles`. Ensuite, ajoutez quatre nouvelles pages ASP.NET à le `Roles` dossier, lier chaque page avec le `Site.master` page maître. Nommez les pages :

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

À ce stade l’Explorateur de solutions de votre projet doit ressembler à l’écran illustré à la Figure 1.


[![Quatre nouvelles Pages ont été ajoutés dans le dossier rôles](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figure 1**: quatre nouvelles Pages ont été ajoutées à la `Roles` dossier ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image3.png))


Chaque page doit avoir à ce stade, les deux contrôles de contenu, une pour chaque ContentPlaceHolders de la page maître : `MainContent` et `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

N’oubliez pas que le `LoginContent` balisage par défaut de ContentPlaceHolder affiche un lien pour se connecter ou se déconnecter du site, selon si l’utilisateur est authentifié. La présence de la `Content2` contenu de contrôle dans la page ASP.NET, substitue toutefois, le balisage de la page maître par défaut. Comme expliqué dans <a id="_msoanchor_4"> </a> [ *une vue d’ensemble de l’authentification par formulaire* ](../introduction/an-overview-of-forms-authentication-cs.md) didacticiel, en remplaçant le balisage par défaut est utile dans les pages où nous ne souhaitez pas afficher associées à la connexion Options de la colonne de gauche.

Pour ces quatre pages, toutefois, nous souhaitons afficher le balisage par défaut de la page maître pour la `LoginContent` ContentPlaceHolder. Par conséquent, supprimez le balisage déclaratif pour la `Content2` contrôle de contenu. Après cela, chacun de balisage de la page quatre doit contenir qu’un seul contrôle de contenu.

Enfin, mettons à jour le plan du site (`Web.sitemap`) à inclure ces nouvelles pages web. Ajoutez le code XML suivant après le `<siteMapNode>` nous avons ajouté pour les didacticiels de l’appartenance.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Avec le plan de site mis à jour, visitez le site via un navigateur. Comme le montre la Figure 2, le volet de navigation de gauche maintenant inclut des éléments pour les didacticiels de rôles.


[![Quatre nouvelles Pages ont été ajoutés dans le dossier rôles](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figure 2**: quatre nouvelles Pages ont été ajoutées à la `Roles` dossier ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Étape 2 : Spécification et configuration du fournisseur de Framework de rôles

Comme l’infrastructure de l’appartenance, l’infrastructure de rôles est construit sur le modèle de fournisseur. Comme indiqué dans le <a id="_msoanchor_5"> </a> [ *principes élémentaires de sécurité et la prise en charge ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) didacticiel, le .NET Framework est livré avec trois fournisseurs de rôles intégrés : [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), et [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Cette série de didacticiels se concentre sur la `SqlRoleProvider`, qui utilise une base de données Microsoft SQL Server comme magasin de rôles.

Dans les coulisses du framework de rôles et `SqlRoleProvider` fonctionnent comme l’infrastructure de l’appartenance et `SqlMembershipProvider`. Le .NET Framework contient un `Roles` classe qui sert de l’API du Framework de rôles. Le `Roles` classe a des méthodes statiques telles que `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, et ainsi de suite. Lorsqu’une de ces méthodes est appelée, la `Roles` classe délègue l’appel au fournisseur configuré. Le `SqlRoleProvider` fonctionne avec les tables spécifiques au rôle (`aspnet_Roles` et `aspnet_UsersInRoles`) dans la réponse.

Pour pouvoir utiliser le `SqlRoleProvider` fournisseur dans notre application, nous devons spécifier ce que la base de données à utiliser comme magasin. Le `SqlRoleProvider` attend le magasin de rôle spécifié pour que certaines tables de base de données, les vues et les procédures stockées. Ces objets de base de données requis peuvent être ajoutés à l’aide de la [ `aspnet_regsql.exe` outil](https://msdn.microsoft.com/library/ms229862.aspx). À ce stade nous avons déjà une base de données avec le schéma nécessaire pour le `SqlRoleProvider`. Dans le <a id="_msoanchor_6"> </a> [ *création du schéma d’appartenance dans SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) didacticiel, nous avons créé une base de données nommée `SecurityTutorials.mdf` et utilisé `aspnet_regsql.exe` pour ajouter l’application services incluant les objets de base de données requis par le `SqlRoleProvider`. Par conséquent, nous devons indiquer à l’infrastructure de rôles pour activer la prise en charge du rôle et pour utiliser le `SqlRoleProvider` avec la `SecurityTutorials.mdf` base de données en tant que magasin de rôles.

Le framework de rôles est configuré via le &lt; `roleManager` &gt; élément dans l’application `Web.config` fichier. Par défaut, la prise en charge du rôle est désactivé. Pour l’activer, vous devez définir le [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) l’élément `enabled` attribut `true` comme suit :

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Par défaut, toutes les applications web ont un fournisseur de rôles nommé `AspNetSqlRoleProvider` de type `SqlRoleProvider`. Ce fournisseur par défaut est inscrit dans `machine.config` (situé dans `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`) :

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Le fournisseur `connectionStringName` attribut spécifie le magasin de rôle qui est utilisé. Le `AspNetSqlRoleProvider` fournisseur définit cet attribut `LocalSqlServer`, qui est également défini dans `machine.config` points, par défaut, à une base de données SQL Server 2005 Express Edition et la `App_Data` dossier nommé `aspnet.mdf`.

Par conséquent, si nous avons simplement permettre à l’infrastructure de rôles sans spécifier des informations sur le fournisseur de notre application `Web.config` fichier, l’application utilise le fournisseur de rôles par défaut inscrit, `AspNetSqlRoleProvider`. Si le `~/App_Data/aspnet.mdf` base de données n’existe pas, le runtime ASP.NET crée automatiquement et ajoutez le schéma de services d’application. Toutefois, nous ne souhaitez pas utiliser le `aspnet.mdf` base de données ; au lieu de cela, nous souhaitons utiliser la `SecurityTutorials.mdf` que nous avons déjà créé et ajouté le schéma de services d’application à base de données. Cette modification peut être accomplie de deux manières :

- <strong>Spécifiez une valeur pour le</strong><strong>`LocalSqlServer`</strong><strong>nom de chaîne de connexion dans</strong><strong>`Web.config`</strong><strong>.</strong> En remplaçant le `LocalSqlServer` valeur du nom de chaîne de connexion `Web.config`, nous pouvons utiliser le fournisseur de rôles par défaut inscrit (`AspNetSqlRoleProvider`) et que cela fonctionne correctement avec le `SecurityTutorials.mdf` base de données. Pour plus d’informations sur cette technique, consultez [Scott Guthrie](https://weblogs.asp.net/scottgu/)du billet de blog, [configuration des Services ASP.NET 2.0 pour utiliser SQL Server 2000 ou SQL Server 2005 Application](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Ajouter un nouveau fournisseur inscrit du type</strong><strong>`SqlRoleProvider`</strong><strong>et configurer ses</strong><strong>`connectionStringName`</strong><strong>paramètre pour pointer vers le</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>base de données.</strong> C’est l’approche que j’ai recommandé et utilisé dans le <a id="_msoanchor_7"> </a> [ *création du schéma d’appartenance dans SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) didacticiel, qui est l’approche que j’utiliserai dans ce didacticiel.

Ajoutez le balisage suivant de configuration des rôles pour le `Web.config` fichier. Ce balisage inscrit un nouveau fournisseur nommé `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Le balisage ci-dessus définit le `SecurityTutorialsSqlRoleProvider` en tant que le fournisseur par défaut (via le `defaultProvider` d’attribut dans le `<roleManager>` élément). Il définit également la `SecurityTutorialsSqlRoleProvider`de `applicationName` à `SecurityTutorials`, qui est le même `applicationName` paramètre utilisé par le fournisseur d’appartenances (`SecurityTutorialsSqlMembershipProvider`). Bien que non illustrée ici, le [ `<add>` élément](https://msdn.microsoft.com/library/ms164662.aspx) pour le `SqlRoleProvider` peut également contenir un `commandTimeout` attribut pour spécifier la durée d’expiration de la base de données, en secondes. La valeur par défaut est 30.

Ce balisage de configuration en place, nous sommes prêts à commencer à utiliser les fonctionnalités de rôle au sein de notre application.

> [!NOTE]
> Le balisage de configuration ci-dessus illustre l’utilisation de la &lt; `roleManager` &gt; l’élément `enabled` et `defaultProvider` attributs. Il existe plusieurs autres attributs qui affectent comment le framework de rôles associe les informations de rôle utilisateur par utilisateur. Nous allons examiner ces paramètres dans le <a id="_msoanchor_8"> </a> [ *autorisation basée sur le rôle* ](role-based-authorization-cs.md) didacticiel.


## <a name="step-3-examining-the-roles-api"></a>Étape 3 : Examen de l’API de rôles

Les fonctionnalités de l’infrastructure de rôles sont exposées via la [ `Roles` classe](https://msdn.microsoft.com/library/system.web.security.roles.aspx), qui contient des méthodes statiques treize pour effectuer des opérations en fonction du rôle. Lorsque nous examinons la création et la suppression de rôles dans les étapes 4 et 6, nous allons utiliser le [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) et [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) méthodes, ajouter ou supprimer un rôle dans le système.

Pour obtenir une liste de tous les rôles dans le système, utilisez le [ `GetAllRoles` méthode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (voir l’étape 5). Le [ `RoleExists` méthode](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) retourne une valeur booléenne indiquant si un rôle spécifié existe.

Dans le didacticiel suivant, nous allons examiner comment associer des utilisateurs aux rôles. Le `Roles` la classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), et [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) méthodes d’ajoutent un ou plusieurs utilisateurs à un ou plusieurs rôles. Pour supprimer les utilisateurs des rôles, utilisez le [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), ou [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) méthodes.

Dans le <a id="_msoanchor_9"> </a> [ *autorisation basée sur le rôle* ](role-based-authorization-cs.md) didacticiel, nous allons examiner les façons d’afficher par programme ou de masquer les fonctionnalités en fonction du rôle de l’utilisateur actuellement connecté. Pour ce faire, nous pouvons utiliser le `Role` la classe [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), ou [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) méthodes.

> [!NOTE]
> N’oubliez pas que n’importe quel moment une de ces méthodes est appelée, la `Roles` classe délègue l’appel au fournisseur configuré. Dans notre cas, cela signifie que l’appel est envoyé à la `SqlRoleProvider`. Le `SqlRoleProvider` puis effectue l’opération de base de données appropriée en fonction de la méthode appelée. Par exemple, le code `Roles.CreateRole("Administrators")` entraîne la `SqlRoleProvider` l’exécution de la `aspnet_Roles_CreateRole` procédure stockée, qui insère un nouvel enregistrement dans le `aspnet_Roles` table nommée d’administrateurs.


Le reste de ce didacticiel examine à l’aide de la `Roles` la classe `CreateRole`, `GetAllRoles`, et `DeleteRole` méthodes pour gérer les rôles dans le système.

## <a name="step-4-creating-new-roles"></a>Étape 4 : Création de nouveaux rôles

Rôles offrent un moyen de regrouper arbitrairement des utilisateurs, et plus souvent ce regroupement est utilisé pour un moyen plus pratique appliquer des règles d’autorisation. Mais pour pouvoir utiliser les rôles comme mécanisme d’autorisation, que nous devons tout d’abord définir les rôles qui existent dans l’application. Malheureusement, ASP.NET n’inclut pas d’un contrôle CreateRoleWizard. Pour ajouter de nouveaux rôles que nous devons créer une interface utilisateur appropriée et appeler l’API de rôles nous-mêmes. La bonne nouvelle est qu’il s’agit très facile à accomplir.

> [!NOTE]
> Il n’existe aucun contrôle CreateRoleWizard Web, il est le [outil Administration de Site Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), qui est une application ASP.NET locale, conçue pour aider à afficher et gérer la configuration de votre application web. Toutefois, je ne suis pas un grand fan de l’outil d’Administration de Site Web ASP.NET pour deux raisons. Tout d’abord, il est un peu bogué et l’expérience utilisateur laisse beaucoup à désirer. Deuxièmement, l’outil d’Administration de Site Web ASP.NET est conçu pour fonctionner uniquement localement, ce qui signifie que vous devrez créer des pages web de gestion de vos propres rôles si vous avez besoin gérer à distance les rôles sur un site de production. Ces deux raisons, ce didacticiel et la prochaine allons nous concentrer sur la création du rôle nécessaire des outils de gestion dans une page web plutôt que de compter sur l’outil d’Administration de Site Web ASP.NET.


Ouvrez le `ManageRoles.aspx` page dans le `Roles` dossier et ajoutez une zone de texte et un contrôle bouton Web à la page. Définir le contrôle de zone de texte `ID` propriété `RoleName` et du bouton `ID` et `Text` propriétés à `CreateRoleButton` et créer un rôle, respectivement. À ce stade, balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Ensuite, double-cliquez sur le `CreateRoleButton` bouton de contrôle dans le concepteur pour créer un `Click` Gestionnaire d’événements, puis ajoutez le code suivant :

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Le code ci-dessus commence, affectez le nom de rôle rognées entré dans le `RoleName` zone de texte pour le `newRoleName` variable. Ensuite, le `Roles` la classe `RoleExists` méthode est appelée pour déterminer si le rôle `newRoleName` existe déjà dans le système. Si le rôle n’existe pas, il est créé via un appel à la `CreateRole` (méthode). Si le `CreateRole` un nom de rôle existe déjà dans le système, est passé à la méthode un `ProviderException` exception est levée. C’est pourquoi le code vérifie d’abord pour vous assurer que le rôle n’existe pas déjà dans le système avant d’appeler `CreateRole`. Le `Click` Gestionnaire d’événements se termine en effaçant le `RoleName` la zone de texte `Text` propriété.

> [!NOTE]
> Vous vous demandez peut-être ce qui se produira si l’utilisateur n’entre pas n’importe quelle valeur dans la `RoleName` zone de texte. Si la valeur passée dans le `CreateRole` méthode est `null` ou une chaîne vide, une exception est levée. De même, si le nom de rôle contient une virgule, une exception est levée. Par conséquent, la page doit contenir des contrôles de validation pour vous assurer que l’utilisateur entre un rôle et qu’il ne contient pas de virgules. Je laisse en guise d’exercice pour le lecteur.


Nous allons créer un rôle nommé administrateurs. Visitez le `ManageRoles.aspx` page via un navigateur, tapez les administrateurs dans la zone de texte (voir Figure 3), puis cliquez sur le bouton Créer un rôle.


[![Créer un rôle d’administrateurs](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figure 3**: créer un rôle d’administrateurs ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image9.png))


Que se passe-t-il ? Une publication (postback) se produit, mais il n’existe aucun indice visuel le rôle a bien été ajouté au système. Nous mettrons à jour cette page à l’étape 5 pour inclure des commentaires visuels. Pour l’instant, toutefois, vous pouvez vérifier que le rôle a été créé en accédant à la `SecurityTutorials.mdf` base de données et afficher les données à partir de la `aspnet_Roles` table. Comme le montre la Figure 4, le `aspnet_Roles` table contient un enregistrement pour les rôles d’administrateurs juste-ajouté.


[![La Table aspnet_Roles comporte une ligne pour les administrateurs](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figure 4**: le `aspnet_Roles` Table contient une ligne pour les administrateurs ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Étape 5 : Afficher les rôles dans le système

Nous allons augmenter la `ManageRoles.aspx` page à inclure une liste des rôles actuels dans le système. Pour ce faire, ajoutez un contrôle GridView à la page et définissez son `ID` propriété `RoleList`. Ensuite, ajoutez une méthode à la classe de code-behind de la page nommée `DisplayRolesInGrid` utilisant le code suivant :

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

Le `Roles` la classe `GetAllRoles` méthode retourne tous les rôles du système sous forme de tableau de chaînes. Ce tableau de chaînes est ensuite lié au GridView. Pour lier la liste des rôles pour le contrôle GridView lors du premier chargement de la page, nous devons appeler le `DisplayRolesInGrid` méthode à partir de la page `Page_Load` Gestionnaire d’événements. Le code suivant appelle cette méthode lorsque la page est visitée en premier, mais pas sur les publications ultérieures.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Avec ce code en place, visitez la page via un navigateur. Comme le montre la Figure 5, vous devez voir une grille avec une seule colonne intitulée élément. La grille inclut une ligne pour le rôle d’administrateurs que nous avons ajouté à l’étape 4.


[![Le contrôle GridView affiche les rôles dans une seule colonne](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figure 5**: le contrôle GridView affiche les rôles dans une seule colonne ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image15.png))


Le contrôle GridView affiche une colonne de Solitaire étiquetée élément, car le GridView `AutoGenerateColumns` propriété est définie sur True (valeur par défaut), ce qui entraîne le contrôle GridView à créer automatiquement une colonne pour chaque propriété dans son `DataSource`. Un tableau a une propriété unique qui représente les éléments dans le tableau, par conséquent, la seule colonne dans le contrôle GridView.

Lors de l’affichage des données avec un GridView, je préfère explicitement définir mes articles au lieu des générer implicitement par le contrôle GridView. En définissant explicitement les colonnes, il est beaucoup plus facile à mettre en forme les données, réorganiser les colonnes et effectuer d’autres tâches courantes. Par conséquent, nous allons mettre à jour balisage déclaratif de GridView afin que ses colonnes sont explicitement définies.

Commencez par définir le GridView `AutoGenerateColumns` False à la propriété. Ajouter un TemplateField dans la grille, définissez ensuite sa `HeaderText` propriété aux rôles et configurer ses `ItemTemplate` afin qu’il affiche le contenu du tableau. Pour ce faire, ajoutez un contrôle Web Label nommé `RoleNameLabel` à la `ItemTemplate` et lier sa `Text` propriété `Container.DataItem`.

Ces propriétés et le `ItemTemplate`du contenu peut être défini de manière déclarative ou par le biais champs boîte de dialogue du contrôle GridView et l’interface de modifier les modèles. Pour atteindre les champs de la boîte de dialogue, cliquez sur le lien Modifier les colonnes dans la balise active le contrôle GridView. Ensuite, désactivez les générer automatiquement des champs pour définir la `AutoGenerateColumns` propriété sur False et en ajouter un TemplateField au GridView, définissant son `HeaderText` propriété au rôle. Pour définir le `ItemTemplate`du contenu, choisissez l’option Modifier les modèles à partir de la balise active le contrôle GridView. Faites glisser un contrôle Web Label dans le `ItemTemplate`, définissez son `ID` propriété `RoleNameLabel`et configurer ses paramètres de liaison de données afin que son `Text` propriété est liée à `Container.DataItem`.

Quelle que soit quelle approche que vous utilisez, balisage déclaratif qui en résulte le contrôle GridView doit ressembler à ce qui suit lorsque vous avez terminé.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Le contenu du tableau est affiché à l’aide de la syntaxe de liaison de données `<%# Container.DataItem %>`. Une description détaillée de la raison pour laquelle cette syntaxe est utilisée lors de l’affichage du contenu d’un tableau lié au GridView est dépasse le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, reportez-vous à [un tableau scalaire de liaison à un contrôle Web de données](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Actuellement, le `RoleList` GridView est uniquement lié à la liste des rôles lorsque la page est visitée en premier. Nous devons actualiser la grille chaque fois qu’un nouveau rôle est ajouté. Pour ce faire, mettez à jour le `CreateRoleButton` du bouton `Click` Gestionnaire d’événements afin qu’il appelle le `DisplayRolesInGrid` méthode si un nouveau rôle est créé.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Maintenant lorsque l’utilisateur ajoute un nouveau rôle le `RoleList` GridView affiche le rôle ajouté juste sur la publication (postback), fournissant une rétroaction visuelle que le rôle a été créé avec succès. Pour illustrer ceci, visitez le `ManageRoles.aspx` page via un navigateur et ajoutez un rôle nommé superviseurs. Lorsque vous cliquez sur le bouton Créer un rôle, résulte d’une publication (postback) et la grille mettra à jour pour inclure les administrateurs, ainsi que le nouveau rôle, les superviseurs.


[![Le rôle superviseurs a été ajouté](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figure 6**: rôle superviseurs a été ajouté ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Étape 6 : Suppression des rôles

À ce stade un utilisateur peut créer un nouveau rôle et afficher tous les rôles existants à partir de la `ManageRoles.aspx` page. Nous allons permettre aux utilisateurs de supprimer des rôles. Le `Roles.DeleteRole` méthode a deux surcharges :

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Supprime le rôle *roleName*. Une exception est levée si le rôle contient un ou plusieurs membres.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Supprime le rôle *roleName*. Si *throwOnPopulateRole* est `true`, une exception est levée si le rôle contient un ou plusieurs membres. Si *throwOnPopulateRole* est `false`, puis le rôle est supprimé si elle contient des membres ou non. En interne, le `DeleteRole(roleName)` les appels de méthode `DeleteRole(roleName, true)`.

Le `DeleteRole` méthode lève également une exception si *roleName* est `null` ou une chaîne vide ou si *roleName* contient une virgule. Si *roleName* n’existe pas dans le système, `DeleteRole` échoue en mode silencieux, sans lever une exception.

Nous allons augmenter le contrôle GridView dans `ManageRoles.aspx` pour inclure un bouton Supprimer qui, lorsque vous cliquez dessus, supprime le rôle sélectionné. Commencez par ajouter un bouton Supprimer pour le contrôle GridView en accédant à la boîte de dialogue champs et ajout d’un bouton de suppression, ce qui se trouve sous l’option CommandField. Rendre la suppression de bouton de la colonne la plus à gauche et définissez son `DeleteText` propriété à supprimer un rôle.


[![Ajouter un bouton Supprimer pour le contrôle RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figure 7**: ajouter un bouton Supprimer pour le `RoleList` GridView ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image21.png))


Après avoir ajouté le bouton Supprimer, balisage déclaratif de votre contrôle GridView doit ressembler à ce qui suit :

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Ensuite, créez un gestionnaire d’événements pour le contrôle GridView `RowDeleting` événement. Il s’agit de l’événement est déclenché lors de la publication lorsque l’utilisateur clique sur le bouton Supprimer le rôle. Ajoutez le code suivant au gestionnaire d’événements.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Le code commence par référencer par programme le `RoleNameLabel` contrôle dans la ligne dont bouton Supprimer le rôle Web. Le `Roles.DeleteRole` méthode est ensuite appelée, en passant le `Text` de la `RoleNameLabel` et `false`, d'où supprimer le rôle ait ou non des utilisateurs associés au rôle sont. Enfin, le `RoleList` GridView est actualisé afin que le rôle juste de supprimé n’apparaît plus dans la grille.

> [!NOTE]
> Le bouton Supprimer le rôle ne nécessite pas une forme quelconque de confirmation de l’utilisateur avant de supprimer le rôle. Une des méthodes plus simples pour confirmer une action est via une boîte de dialogue de confirmation du côté client. Pour plus d’informations sur cette technique, consultez [Ajout côté Client Confirmation lors de la suppression](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Récapitulatif

De nombreuses applications web ont certaines règles d’autorisation ou la fonctionnalité de niveau de la page qui est uniquement disponible pour certaines classes d’utilisateurs. Par exemple, il peut être un ensemble de pages web que seuls les administrateurs peuvent accéder. Au lieu de la définition de ces règles d’autorisation sur une base par utilisateur, souvent, il est plus utile définir les règles basées sur un rôle. Autrement dit, au lieu de laisser explicitement les utilisateurs Scott et Jisun pour accéder aux pages web d’administration, une approche plus facile à gérer est d’autoriser des membres du rôle Administrateurs d’accéder à ces pages, puis pour dénoter Scott et Jisun en tant qu’utilisateurs appartenant à la Rôle des administrateurs.

Le framework de rôles rend plus facile créer et gérer des rôles. Dans ce didacticiel, nous avons examiné comment configurer l’infrastructure de rôles à utiliser le `SqlRoleProvider`, qui utilise une base de données Microsoft SQL Server comme magasin de rôles. Nous avons créé également une page web qui répertorie les rôles existants dans le système, tout en permettant la création de nouveaux rôles et existants à supprimer. Dans les didacticiels suivants, nous verrons comment affecter des utilisateurs aux rôles et comment appliquer l’autorisation en fonction du rôle.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Examen d’ASP.NET de 2.0 l’appartenance, rôles et profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Guide pratique pour utiliser le Gestionnaire de rôles dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Fournisseurs de rôles](https://msdn.microsoft.com/library/aa478950.aspx)
- [Déploiement de votre propre outil d’Administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentation technique pour le `<roleManager>` élément](https://msdn.microsoft.com/library/ms164660.aspx)
- [À l’aide de l’appartenance et les API du Gestionnaire de rôle](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel incluent Alicja Maziarz, Suchi Banerjee et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](assigning-roles-to-users-cs.md)
