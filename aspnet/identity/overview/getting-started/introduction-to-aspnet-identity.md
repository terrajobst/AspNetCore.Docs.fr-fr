---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introduction à ASP.NET Identity | Microsoft Docs
author: jongalloway
description: Le système d’appartenance ASP.NET a été introduit avec le retour de ASP.NET 2.0 en 2005 et depuis puis de nombreuses modifications ont été dans le typicall d’applications façons web...
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 3cefefc85857c3e3e295789dfa9d9f4789de4602
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811579"
---
<a name="introduction-to-aspnet-identity"></a>Introduction à ASP.NET Identity
====================
par [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

> Le système d’appartenance ASP.NET a été introduit avec ASP.NET 2.0 retour depuis 2005 et puis de nombreuses modifications ont été comme les applications web gèrent généralement l’authentification et l’autorisation. ASP.NET Identity est un regard neuf sur ce que le système d’appartenance doit être lorsque vous générez des applications modernes pour le web, un téléphone ou une tablette.
> 
> Cet article a été écrit par Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra et Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>D’arrière-plan : L’appartenance dans ASP.NET

### <a name="aspnet-membership"></a>Appartenance ASP.NET

[L’appartenance ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) a été conçu pour répondre aux exigences l’appartenance au site qui étaient courantes en 2005, impliquant l’authentification par formulaire et une base de données SQL Server pour les noms d’utilisateur, les mots de passe et les données de profil. Aujourd'hui, il existe un large éventail d’options de stockage de données pour les applications web, la plupart des développeurs et activer ou leurs sites pour utiliser des fournisseurs d’identité sociale pour les fonctionnalités d’authentification et d’autorisation. Les limitations de conception de l’appartenance d’ASP.NET compliquent cette transition :

- Le schéma de base de données a été conçu pour SQL Server et vous ne pouvez pas le modifier. Vous pouvez ajouter des informations de profil, mais les données supplémentaires sont empaquetées dans une autre table, ce qui rend difficile pour accéder à toute autre manière, sauf via l’API de fournisseur de profil.
- Le système du fournisseur permet de modifier le magasin de données de stockage, mais le système est conçu autour des hypothèses appropriés pour une base de données relationnelle. Vous pouvez écrire un fournisseur pour stocker les informations d’appartenance dans un mécanisme de stockage non relationnelles, telles que les Tables de stockage Azure, mais vous devez contourner la conception relationnelle en écrivant une grande quantité de code et un grand nombre de `System.NotImplementedException` exceptions pour les méthodes qui ne s’applique aux bases de données NoSQL.
- Étant donné que la fonctionnalité de déconnexion/journal est basée sur l’authentification par formulaire, le système d’appartenance ne peut pas utiliser [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN inclut les composants d’intergiciel (middleware) d’authentification, notamment la prise en charge des ouvertures de session à l’aide de fournisseurs d’identité externes (par exemple, Accounts Microsoft, Facebook, Google, Twitter) et les ouvertures de session à l’aide de comptes de société à partir d’Active Directory local ou Azure Active Directory. OWIN prend également en charge OAuth 2.0, JSON et CORS.

### <a name="aspnet-simple-membership"></a>Appartenance ASP.NET simples

[L’appartenance d’ASP.NET simple](../../../web-pages/overview/security/16-adding-security-and-membership.md) a été développé comme un système d’appartenance pour ASP.NET Web Pages. Il a été publié avec WebMatrix et Visual Studio 2010 SP1. L’objectif de l’appartenance Simple a été pour le rendre facile d’ajouter des fonctionnalités d’appartenance à une application Web Pages.

L’appartenance simple rendre plus facile à personnaliser les informations de profil utilisateur, mais il partage toujours les autres problèmes avec l’appartenance d’ASP.NET, et il présente certaines limitations :

- Il était difficile de conserver les données de système d’appartenance dans un magasin non relationnelles.
- Vous ne pouvez pas l’utiliser avec OWIN.
- Il ne fonctionne pas correctement avec les fournisseurs d’appartenances ASP.NET existants, et il n’est pas extensible.

### <a name="aspnet-universal-providers"></a>Fournisseurs universels ASP.NET

[Fournisseurs universels ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) ont été développées pour le rendre possible de conserver les informations d’appartenance dans Microsoft Azure SQL Database et ils fonctionnent également avec SQL Server Compact. Les fournisseurs universels ont été créés sur Entity Framework Code First, ce qui signifie que les fournisseurs universels peuvent être utilisés pour conserver les données dans n’importe quel magasin pris en charge par Entity Framework. Avec les fournisseurs universels, le schéma de base de données a été nettoyé beaucoup également.

Les fournisseurs universels sont basées sur l’infrastructure de l’appartenance ASP.NET, afin qu’ils exécutent toujours les mêmes limitations que le fournisseur SqlMembership. Autrement dit, ils ont été conçus pour les bases de données relationnelles, et il est difficile de personnaliser le profil et les informations utilisateur. Ces fournisseurs utilisent toujours l’authentification par formulaire pour la fonctionnalité de connexion et de déconnexion.

## <a name="aspnet-identity"></a>ASP.NET Identity

Comme l’appartenance au récit dans ASP.NET a évolué au fil des années, l’équipe ASP.NET a beaucoup appris à partir des commentaires des clients.

Partant du principe que les utilisateurs seront connecteront en entrant un nom d’utilisateur et le mot de passe qu’ils ont enregistrées dans votre propre application n’est plus valide. Le web est devenu plus de réseaux sociaux. Les utilisateurs interagissent avec eux en temps réel par le biais des canaux sociaux tels que Facebook, Twitter et autres sites web sociaux. Les développeurs veulent les utilisateurs puissent se connecter avec leurs identités sociales afin qu’ils peuvent avoir une expérience riche sur leurs sites web. Un système d’appartenance moderne doit activer en fonction de la redirection ouvertures de session aux fournisseurs d’authentification tels que Facebook, Twitter et d’autres utilisateurs.

Comme le développement web a évolué, a les modèles de développement web. Unité de code de l’application de test est devenue une préoccupation pour les développeurs d’applications. ASP.NET a ajouté une nouvelle infrastructure basée sur le modèle Model-View-Controller (MVC), en partie pour aider les développeurs à créer des applications ASP.NET testables unité en 2008. Les développeurs qui souhaitent unité tester leur logique d’application que vous voulez également être en mesure de le faire avec le système d’appartenance.

Compte tenu de ces modifications dans le développement d’applications web, ASP.NET Identity a été développé avec les objectifs suivants :

- **Un système d’identité ASP.NET**

    - ASP.NET Identity peut être utilisé avec toutes les infrastructures ASP.NET, tels que ASP.NET MVC, les Web Forms, les Pages Web, les API Web et SignalR.
    - Identité de ASP.NET peut être utilisée lorsque vous concevez des applications web, téléphone, magasin ou hybride.
- **Facilité de branchement dans les données de profil sur l’utilisateur**

    - Vous contrôlez le schéma d’utilisateur et les informations de profil. Par exemple, vous pouvez facilement activer le système stocker les dates de naissance entrées par les utilisateurs lorsqu’ils s’inscrivent à un compte dans votre application.

- **Contrôle de persistance**

    - Par défaut, le système d’identité ASP.NET stocke toutes les informations utilisateur dans une base de données. ASP.NET Identity utilise Entity Framework Code First pour implémenter l’ensemble de son mécanisme de persistance.
    - Étant donné que vous contrôlez le schéma de base de données, les tâches courantes telles que la modification des noms de table ou la modification du type de données de clés primaires est simple à réaliser.
    - Il est facile d’intégrer dans les différents mécanismes de stockage telles que SharePoint, Service de Table de stockage Azure, les bases de données NoSQL, etc., sans avoir à lever `System.NotImplementedExceptions` exceptions.
- **Testabilité unitaire**

    - ASP.NET Identity l’application web plus d’unité est testable. Vous pouvez écrire des tests unitaires pour les parties de votre application qui utilisent l’identité d’ASP.NET.
- **Fournisseur de rôles**

    - Il existe un fournisseur de rôles qui vous permet de restreindre l’accès aux parties de votre application par les rôles. Vous pouvez facilement créer des rôles, tels que « Admin » et ajouter des utilisateurs aux rôles.
- **Basée sur les revendications**

    - ASP.NET Identity prend en charge l’authentification basée sur les revendications, où l’identité de l’utilisateur est représentée comme un ensemble de revendications. Revendications permet aux développeurs d’être beaucoup plus expressif dans décrivant l’identité d’un utilisateur à autorisent les rôles. Alors que l’appartenance au rôle est simplement une valeur booléenne (membre ou non membre), une revendication peut inclure des informations détaillées sur l’identité et l’appartenance de l’utilisateur.
- **Fournisseurs de connexion de réseau social**

    - Vous pouvez facilement ajouter des ouvertures de session sociaux tels que Account Microsoft, Facebook, Twitter, Google et d’autres utilisateurs à votre application et stocker les données spécifiques à l’utilisateur dans votre application.
- **Azure Active Directory**

    - Vous pouvez également ajouter des fonctionnalités d’ouverture de session à l’aide d’Azure Active Directory et stocker les données spécifiques à l’utilisateur dans votre application. Pour plus d’informations, consultez [comptes professionnels](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) dans Creating ASP.NET Web Projects in Visual Studio 2013
- **Intégration OWIN**

    - Authentification ASP.NET est désormais basée sur l’intergiciel OWIN qui peut être utilisé sur n’importe quel hôte OWIN. ASP.NET Identity n’a pas de dépendances sur System.Web. Il est une infrastructure OWIN entièrement compatible et peut être utilisé dans n’importe quelle application OWIN hébergé.
    - ASP.NET Identity utilise l’authentification OWIN de journal/journal sorti d’utilisateurs dans le site web. Cela signifie qu’au lieu d’utiliser FormsAuthentication pour générer le cookie, l’application utilise OWIN CookieAuthentication pour ce faire.
- **Package NuGet**

    - ASP.NET Identity est redistribué sous forme de package NuGet qui est installé dans les modèles ASP.NET MVC, Web Forms et Web API fournis avec Visual Studio 2013. Vous pouvez télécharger ce package NuGet à partir de la galerie NuGet.
    - Libération d’identité ASP.NET comme un package NuGet package facilite l’équipe ASP.NET effectuer une itération sur les nouvelles fonctionnalités et correctifs de bogues et fournir aux développeurs de manière agile.

## <a name="getting-started-with-aspnet-identity"></a>Mise en route avec ASP.NET Identity

ASP.NET Identity est utilisé dans les modèles de projet Visual Studio 2013 pour ASP.NET MVC, Web Forms, Web API et SPA. Dans cette procédure pas à pas, nous allons illustrent la façon dont les modèles de projet utilisent ASP.NET Identity pour ajouter des fonctionnalités pour vous inscrire, connectez-vous et déconnecter un utilisateur.

ASP.NET Identity est implémenté à l’aide de la procédure suivante. L’objectif de cet article est de vous donner une vue d’ensemble détaillée de l’identité de ASP.NET ; Vous pouvez la suivre, étape par étape ou simplement de lire les détails. Pour obtenir des instructions plus détaillées sur la création d’applications à l’aide d’ASP.NET Identity, y compris à l’aide de la nouvelle API pour ajouter des utilisateurs, des rôles et des informations de profil, consultez la section étapes suivantes à la fin de cet article.

1. Créer une application ASP.NET MVC avec des comptes individuels. Vous pouvez utiliser ASP.NET Identity dans ASP.NET MVC, Web Forms, Web API, etc. de SignalR. Dans cet article, nous allons commencer avec une application ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Le projet créé contient les trois packages suivants pour ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Ce package a l’implémentation d’Entity Framework d’identité ASP.NET qui conserve les données d’identité ASP.NET et le schéma de SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Ce package contient des interfaces essentielles pour ASP.NET Identity. Ce package peut être utilisé pour écrire une implémentation pour ASP.NET Identity persistance différentes cibles stocke tels que Azure Table Storage, NoSQL, bases de données etc.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Ce package contient des fonctionnalités qui sont utilisée pour incorporer l’authentification OWIN avec ASP.NET Identity dans les applications ASP.NET. Cela est utilisé lorsque vous ajoutez un journal de fonctionnalités à votre application et appeler dans le middleware d’authentification de Cookie OWIN pour générer un cookie.
3. Création d’un utilisateur.  
   Lancer l’application, puis cliquez sur le **inscrire** lien pour créer un utilisateur. L’illustration suivante montre la page d’inscription qui collecte le nom d’utilisateur et le mot de passe.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Lorsque l’utilisateur clique sur le **inscrire** bouton, le `Register` action du contrôleur de compte crée l’utilisateur en appelant l’API d’identité ASP.NET, comme indiqué ci-dessous :

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Connectez-vous.  
   Si l’utilisateur a été créé avec succès, elle est connectée par le `SignInAsync` (méthode).  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   Le code en surbrillance ci-dessus dans le `SignInAsync` méthode génère une [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Dans la mesure où ASP.NET Identity et l’authentification des cookies OWIN sont système basé sur les revendications, l’infrastructure requiert l’application pour générer un ClaimsIdentity pour l’utilisateur. ClaimsIdentity comporte des informations sur toutes les revendications pour l’utilisateur, telles que les rôles de l’utilisateur appartient. Vous pouvez également ajouter plusieurs revendications de l’utilisateur à ce stade.  
  
   Le code en surbrillance ci-dessous dans le `SignInAsync` méthode connecte l’utilisateur à l’aide de la classe AuthenticationManager d’OWIN et en appelant `SignIn` et en transmettant le ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Fermez la session.  
   En cliquant sur le **déconnecter** lien appelle l’action de fermeture de session dans le contrôleur de compte. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Code en surbrillance ci-dessus montre OWIN `AuthenticationManager.SignOut` (méthode). Ce comportement est analogue à [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) méthode utilisée par le [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) module dans Web Forms.

## <a name="components-of-aspnet-identity"></a>Composants d’identité ASP.NET

Le diagramme ci-dessous illustre les composants du système d’identité ASP.NET (cliquez sur [cela](introduction-to-aspnet-identity/_static/image3.png) ou sur le diagramme pour l’agrandir). Les packages en vert constituent le système d’identité ASP.NET. Tous les autres packages sont des dépendances qui sont nécessaires pour utiliser le système d’identité ASP.NET dans les applications ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Voici une brève description des packages NuGet ne pas mentionné précédemment :

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Intergiciel (middleware) qui permet à une application utiliser un cookie en fonction de l’authentification, revient à ASP. Authentification de formulaires du NET.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework est la technologie d’accès aux données recommandée de Microsoft pour les bases de données relationnelles.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migration de l’appartenance à ASP.NET Identity

Nous espérons bientôt fournissent des conseils sur la migration de vos applications existantes qui utilisent l’appartenance d’ASP.NET ou l’appartenance Simple pour le nouveau système d’identité ASP.NET.

## <a name="next-steps"></a>Étapes suivantes

- [Créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Le didacticiel utilise l’API d’identité ASP.NET pour ajouter des informations de profil pour la base de données utilisateur et comment s’authentifier avec Google et Facebook.
- [Créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer sur Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Ce didacticiel montre comment utiliser l’API d’identité pour ajouter des utilisateurs et des rôles.
- [Comptes d’utilisateur individuels](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) lors de la création des projets Web ASP.NET dans Visual Studio 2013
- [Comptes de société](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) lors de la création des projets Web ASP.NET dans Visual Studio 2013
- [Personnalisation des informations de profil dans ASP.NET Identity dans les modèles de VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Obtenir plus d’informations provenant de fournisseurs sociaux utilisés dans les modèles de projet Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Exemple d’application qui montre comment ajouter des rôles de base et prise en charge de l’utilisateur et comment effectuer la gestion des utilisateurs et des rôles.
