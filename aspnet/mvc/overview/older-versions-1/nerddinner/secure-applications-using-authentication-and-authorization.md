---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Sécuriser des Applications à l’aide de l’authentification et autorisation | Microsoft Docs
author: microsoft
description: Étape 9 montre comment ajouter l’authentification et l’autorisation pour sécuriser notre application NerdDinner, afin que les utilisateurs ont besoin inscrire et de la connexion au site pour créer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d28102c8b80433b58a42cadc70b26c9fb5bc4404
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369882"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Sécuriser des Applications à l’aide de l’authentification et autorisation
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 9 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 9 montre comment ajouter l’authentification et l’autorisation pour sécuriser notre application NerdDinner, afin que les utilisateurs ont besoin inscrire et de la connexion au site pour créer une nouvelle dîners et seul l’utilisateur qui héberge un dîner peut le modifier ultérieurement.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner étape 9 : Authentification et autorisation

Dès maintenant notre NerdDinner application accorde toute personne visitant le site la possibilité de créer et modifier les détails de n’importe quel dîner. Nous allons modifier cela afin que les utilisateurs ont besoin inscrire et de la connexion au site pour créer nouvelle dîners et ajouter une restriction afin que seul l’utilisateur qui héberge un dîner peut le modifier ultérieurement.

Pour ce faire, nous allons utiliser l’authentification et l’autorisation pour sécuriser notre application.

### <a name="understanding-authentication-and-authorization"></a>Autorisation et la compréhension de l’authentification

*Authentification* est le processus d’identification et de validation de l’identité d’un client de l’accès à une application. Plus simplement, il agit sur l’identification des « qui » est de l’utilisateur final lorsqu’ils visitent un site Web. ASP.NET prend en charge plusieurs méthodes d’authentification des utilisateurs du navigateur. Pour des applications web Internet, l’approche la plus courante d’authentification utilisée est appelée « Authentification de formulaires ». L’authentification par formulaire permet à un développeur créer un formulaire de connexion HTML dans leur application et puis valider le nom d’utilisateur/le mot de passe que l’utilisateur final soumet par rapport à une base de données ou un autre magasin d’informations d’identification de mot de passe. Si la combinaison nom d’utilisateur/mot de passe est correcte, le développeur peut demander puis ASP.NET pour émettre un cookie HTTP chiffré pour identifier l’utilisateur entre les demandes futures. Nous allons à l’aide de l’authentification par formulaire avec notre application NerdDinner.

*Autorisation* est le processus permettant de déterminer si un utilisateur authentifié est autorisé à accéder à une ressource/URL particulière ou d’effectuer une action. Par exemple, au sein de notre application NerdDinner nous devrons autoriser que seuls les utilisateurs qui sont connectés peuvent accéder à la */dîners/créer* URL et créer de nouveaux dîners. Nous allons également ajouter une logique d’autorisation afin que seul l’utilisateur qui héberge un dîner modification – et de refuser l’accès en modification à tous les autres utilisateurs.

### <a name="forms-authentication-and-the-accountcontroller"></a>L’authentification par formulaire et la nomme AccountController

Le modèle de projet Visual Studio par défaut pour ASP.NET MVC active automatiquement l’authentification par formulaire lors de la création de nouvelles applications ASP.NET MVC. Il ajoute également automatiquement une implémentation de page de connexion de compte prédéfini pour le projet, ce qui le rend très facile à intégrer la sécurité au sein d’un site.

La page maître Site.master de valeur par défaut affiche un lien « Ouverture de session » dans l’angle supérieur droit du site quand l’utilisateur qui y accède n’est pas authentifiée :

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Cliquant sur le lien « Ouverture de session » amène un utilisateur vers le */compte / d’ouverture de session* URL :

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Les visiteurs qui n’ont pas enregistré peuvent se faire en cliquant sur le lien « S’inscrire » – qui mène à la */compte/Register* URL et leur permettre d’entrer les détails du compte :

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

En cliquant sur le bouton « S’inscrire » crée un nouvel utilisateur dans le système d’appartenance d’ASP.NET et authentifier l’utilisateur sur le site à l’aide de l’authentification par formulaire.

Lorsqu’un utilisateur est connecté, la page Site.master change haut à droite de la page pour générer un « Bienvenue [username] ! » message et restitue un « fermer la session » lier au lieu d’une « session » un. Cliquant sur le lien « Fermer la session » déconnecte l’utilisateur :

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La fonctionnalité de connexion, déconnexion et d’inscription ci-dessus est implémentée dans la classe AccountController qui a été ajoutée à notre projet par Visual Studio lors de la création du projet. L’interface utilisateur pour le contrôle AccountController est implémentée à l’aide de modèles de vue dans le répertoire \Views\Account :

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La classe AccountController utilise le système d’authentification par formulaire ASP.NET pour émettre des cookies d’authentification chiffré et l’API d’appartenance ASP.NET pour stocker et de valider les noms d’utilisateur/mots de passe. L’API d’appartenance ASP.NET est extensible et permet de n’importe quel magasin d’informations d’identification de mot de passe à utiliser. ASP.NET est livré avec des implémentations de fournisseur d’appartenances intégré qui stockent le nom d’utilisateur/mots de passe dans une base de données SQL, ou dans Active Directory.

Nous pouvons configurer le fournisseur d’appartenances notre application NerdDinner doit utiliser en ouvrant le fichier « web.config » à la racine du projet et en recherchant le &lt;appartenance&gt; section qu’il contient. Inscrit le fournisseur d’appartenances SQL web.config par défaut ajoutée lorsque le projet a été créé et configure pour utiliser une chaîne de connexion nommée « ApplicationServices » pour spécifier l’emplacement de la base de données.

La chaîne de connexion par défaut « ApplicationServices » (ce qui est spécifié dans le &lt;connectionStrings&gt; section du fichier web.config) est configuré pour utiliser SQL Express. Il pointe vers une base de données SQL Express nommée « ASPNETDB. MDF » sous l’application « application\_données « directory. Si cette base de données n’existe pas la première fois que l’API d’appartenance est utilisée au sein de l’application, ASP.NET sera automatiquement créer la base de données et approvisionner du schéma de base de données d’appartenance appropriés qu’il contient :

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Si au lieu d’utiliser SQL Express que nous souhaitons utiliser une instance de SQL Server complète (ou se connecter à une base de données distante), tous les nous aurions besoin d’action consiste à mettre à jour la chaîne de connexion « ApplicationServices » dans le fichier web.config et assurez-vous que le schéma d’adhésion appropriées a été ajoutée pour qu’il pointe vers la base de données. Vous pouvez exécuter le « aspnet\_regsql.exe « utilitaire dans le répertoire \Windows\Microsoft.NET\Framework\v2.0.50727\ pour ajouter le schéma approprié pour l’appartenance et les autres services d’application ASP.NET à une base de données.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorisation de l’URL dîners/créer en utilisant le filtre [Authorize]

Nous n’avions pas à écrire du code pour activer une authentification sécurisée et l’implémentation de gestion de compte pour l’application NerdDinner. Les utilisateurs peuvent inscrire de nouveaux comptes avec notre application et se déconnecter du site.

Maintenant, nous pouvons ajouter la logique d’autorisation à l’application et permet de contrôler ce qu’ils peuvent et ne peut pas au sein du site l’état de l’authentification et le nom d’utilisateur de visiteurs. Commençons par l’ajout d’une logique d’autorisation aux méthodes d’action « Créer » de notre classe DinnersController. Plus précisément, nous avons besoin que les utilisateurs accédant à la */dîners/créer* URL doit être connectée. S’ils ne sont pas connectés nous allons les rediriger vers la page de connexion afin qu’ils peuvent se connecter à le.

L’implémentation de cette logique est assez facile. Il nous suffit d’action consiste à ajouter un attribut de filtre [Authorize] à nos méthodes d’action de créer comme suit :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC prend en charge la possibilité de créer des « filtres d’action » qui peuvent être utilisées pour implémenter une logique réutilisable qui peut être appliquée de manière déclarative aux méthodes d’action. Le filtre [Authorize] est un des filtres d’action intégrés fournis par ASP.NET MVC, et il permet à un développeur de façon déclarative appliquer des règles d’autorisation pour les méthodes d’action et les classes de contrôleur.

Lorsqu’il est appliqué sans paramètres (comme ci-dessus) le filtre [Authorize] impose que l’utilisateur qui effectue la requête de méthode d’action doit être connecté – et il automatiquement redirige le navigateur vers l’URL de connexion si elles ne sont pas. Cette redirection de l’URL demandée à l’origine est passée comme un argument de chaîne de requête lors de l’exécution (par exemple : / compte / d’ouverture de session ? ReturnUrl = % 2fDinners % 2fCreate). Contrôle AccountController puis redirigera l’utilisateur vers l’URL demandée à l’origine une fois qu’il se connecte.

Le filtre [Authorize] éventuellement prend en charge que la possibilité de spécifier une propriété « Utilisateurs » ou « Rôles » qui peut être utilisée pour exiger que l’utilisateur est connecté et dans une liste d’utilisateurs autorisés ou un membre d’un rôle de sécurité autorisé. Par exemple, le code ci-dessous autorise uniquement deux des utilisateurs spécifiques, « scottgu » et « billg », pour accéder à l’URL dîners/Create :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Incorporation des noms d’utilisateur spécifique dans du code tend à être relativement non gérable cependant. Une meilleure approche consiste à définir de niveau supérieurs « rôles » que le code vérifie par rapport à, puis de mapper les utilisateurs dans le rôle à l’aide d’une base de données ou le système active directory (l’activation de la liste de mappage utilisateur réelles à stocker en externe à partir du code). ASP.NET inclut une API de gestion de rôle intégré, mais aussi un ensemble intégré de fournisseurs de rôles (y compris celles pour SQL et Active Directory) qui peuvent aider à effectuer ce mappage de rôle d’utilisateur. Nous pourrions ensuite mettre à jour le code pour autoriser uniquement les utilisateurs au sein d’un rôle spécifique « admin » pour accéder à l’URL dîners/Create :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>À l’aide de la propriété User.Identity.Name lors de la création dîners

Nous pouvons récupérer le nom d’utilisateur de l’utilisateur actuellement connecté d’une demande à l’aide de la propriété User.Identity.Name exposée sur la classe de base du contrôleur.

Antérieures lorsque nous avons implémenté la version de HTTP-POST de notre méthode d’action Create() nous avions codée en dur la propriété « HostedBy » du dîner à une chaîne statique. Nous pouvons maintenant mettre à jour de ce code pour utiliser à la place de la propriété User.Identity.Name, mais aussi ajouter automatiquement une réponse pour l’hôte création le dîner :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Étant donné que nous avons ajouté un attribut [Authorize] à la méthode Create(), ASP.NET MVC permet de s’assurer que la méthode d’action s’exécute uniquement si l’utilisateur visitant l’URL dîners/créer est connecté sur le site. Par conséquent, la valeur de propriété User.Identity.Name contient toujours un nom d’utilisateur valide.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>À l’aide de la propriété User.Identity.Name lors de la modification dîners

Nous allons maintenant ajouter une logique d’autorisation qui limite les utilisateurs afin qu’ils peuvent uniquement modifier les propriétés de dîners qu'eux-mêmes qui héberge.

Pour cela, nous allons tout d’abord ajouter une méthode d’assistance de « IsHostedBy(username) » à notre objet dîner (au sein de la classe partielle Dinner.cs que nous avons créé précédemment). Cette méthode d’assistance retourne true ou false selon si un nom d’utilisateur fourni correspond à la propriété dîner HostedBy et encapsule la logique nécessaire pour effectuer une comparaison de chaînes respectant la casse d'entre eux :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Nous allons ensuite ajouter un attribut [Authorize] aux méthodes d’action Edit() au sein de notre classe DinnersController. Cela garantit que les utilisateurs doivent être connectés à la demande un */Dinners/modification / [id]* URL.

Nous pouvons ensuite ajouter le code à nos méthodes de modification qui utilise la méthode d’assistance Dinner.IsHostedBy(username) pour vérifier que l’utilisateur connecté correspond à l’hôte dîner. Si l’utilisateur n’est pas l’ordinateur hôte, nous afficher une vue « InvalidOwner » et mettre fin à la demande. Le code d’implémentation se présente comme suit :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Nous pouvons ensuite avec le bouton droit sur le répertoire \Views\Dinners et choisir Add -&gt;afficher la commande de menu pour créer une nouvelle vue de « InvalidOwner ». Nous allons renseignez-le avec le message d’erreur ci-dessous :

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Et maintenant quand un utilisateur tente de modifier un dîner qu’ils ne possèdent, ils obtiendront un message d’erreur :

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Nous pouvons répéter les mêmes étapes pour les méthodes d’action Delete() dans notre contrôleur pour bloquer l’autorisation de supprimer dîners ainsi et vous assurer que seul l’hôte d’un dîner pouvez le supprimer.

### <a name="showinghiding-edit-and-delete-links"></a>Modification de l’affichage/masquage et des liens de suppression

Nous sommes liaison à la méthode d’action Edit et Delete de notre classe DinnersController dans notre URL détails :

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Actuellement, nous expliquons les modifier et supprimer des liens d’action que le visiteur à l’URL de détails soit l’hôte du dîner. Nous allons modifier afin que les liens sont affichés uniquement si l’utilisateur visite est le propriétaire du dîner.

La méthode d’action Details() au sein de notre DinnersController récupère un objet dîner et transmet ensuite en tant que l’objet de modèle à notre modèle de vue :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Nous pouvons mettre à jour de notre modèle de vue pour conditionnellement afficher/masquer les liens Edit et Delete à l’aide de la Dinner.IsHostedBy() méthode d’assistance comme ci-dessous :

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Étapes suivantes

Voyons maintenant comment nous pouvons activer les utilisateurs authentifiés à RSVP pour préparés à l’aide d’AJAX.

> [!div class="step-by-step"]
> [Précédent](implement-efficient-data-paging.md)
> [Suivant](use-ajax-to-deliver-dynamic-updates.md)
