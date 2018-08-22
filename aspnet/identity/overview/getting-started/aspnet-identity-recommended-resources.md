---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: Ressources recommandées pour ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique fournit des liens vers des ressources de documentation sur l’utilisation d’ASP.NET Identity. Si vous connaissez un excellent billet de blog, thread Stack Overflow ou n’importe quel autre lin...
ms.author: riande
ms.date: 04/09/2015
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: 9937003a23d018ffd7fb4a837fca9d39dcc2a89f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835003"
---
<a name="aspnet-identity-recommended-resources"></a>Ressources recommandées pour ASP.NET Identity
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Cette rubrique fournit des liens vers des ressources de documentation sur l’utilisation d’ASP.NET Identity.
> 
> Si vous connaissez un excellent blog valider, [stackoverflow](http://stackoverflow.com) thread ou un autre lien qui serait utile, [nous envoyer un e-mail](mailto:aspnetue@microsoft.com?subject=Identity recommended resources) avec le lien ou simplement laisser un message au bas de cette page.


- [Bien démarrer avec ASP.NET Identity](#gettingstarted)
- [Nouveaux articles de la lecture doit proposées](#feat)
- [Intermédiaire ASP.NET Identity](#adv)
- [Vidéos](#video)
- [Où poser des questions, de fonctionnalités de requête, de signaler un bogue et les builds nocturnes](#samp)
- [Billets de blog sur l’identité](#blog)
- [Fournisseurs de stockage personnalisés pour ASP.NET Identity](#cust)
- [Ressources d’identité supplémentaires](#additional)
- [Q &amp; un (questions/réponses)](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>Mise en route avec ASP.NET Identity

- [Application MVC 5 avec connexion Facebook, Twitter, LinkedIn et Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce tutoriel vous montre comment écrire une application ASP.NET MVC 5 avec une autorisation Facebook et Google OAuth2. Il montre également comment ajouter des données supplémentaires à la base de données Identity.
- [Déployer une application ASP.NET MVC sécurisée avec appartenance, OAuth et base de données SQL dans un Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ce tutoriel ajoute le déploiement Azure, la sécurisation de votre application avec des rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles, et des fonctionnalités de sécurité supplémentaires.
- [Introduction à ASP.NET Identity](introduction-to-aspnet-identity.md)
- [Créer une application web de ASP.NET MVC 5 sécurisée avec connexion, réinitialisation de confirmation et le mot de passe de messagerie](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [Application ASP.NET MVC 5 avec authentification à deux facteurs par SMS et e-mail](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>Nouveaux articles de la lecture doit proposées

- [Procédure pas à pas : ASP.NET MVC identité avec l’authentification de compte Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/) par [Benjamin Day](http://www.benday.com/about/)
- [Extension de modèles d’identité ASP.NET Identity 2.0 et à l’aide des clés de type entier au lieu de chaînes](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [Authentification par jeton AngularJS à l’aide d’ASP.NET Web API 2, Owin et l’identité](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Thinktecture.IdentityManager comme un remplacement pour le WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Identité ASP.NET 2.0 : Personnalisation des utilisateurs et des rôles](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>Intermédiaire ASP.NET Identity

- [Confirmation de compte et de récupération de mot de passe avec ASP.NET Identity](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Authentification à deux facteurs par SMS et e-mail avec ASP.NET Identity](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Migration d’un site web existant de l’appartenance SQL vers ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Ajout d’ASP.NET Identity à un projet Web Forms vide ou existant](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- MSDN Magazine [d’authentification externe avec ASP.NET Identity](https://msdn.microsoft.com/magazine/dn745860.aspx) par Dino Esposito
- MSDN Magazine[un premier aperçu de ASP.NET Identity](https://msdn.microsoft.com/magazine/dn605872.aspx) par Dino Esposito
- [ASP.NET Identity : verrouillage de l’utilisateur](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>Où poser des questions, de fonctionnalités de requête, de signaler un bogue et les builds nocturnes

- Pour StackOverflow, utilisez la balise [aspnet-identité](http://stackoverflow.com/questions/tagged/asp.net-identity)
- Pour les forums ASP.NET, publiez sur le [Forum sur la sécurité](https://forums.asp.net/25.aspx) et ajoutez **ASP.NET Identity** au titre.
- [ASP.NET Identity sur GitHub](https://github.com/aspnet/AspNetIdentity) Get les builds nocturnes, demander des fonctionnalités, ouvrir des bogues.

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>Billets de blog sur l’identité

- [Qu’est un SecurityStamp dans ASP.NET Identity ?](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- Par [John Atten](https://twitter.com/xivSolutions)

    - [Extension de modèles d’identité ASP.NET Identity 2.0 et à l’aide des clés de type entier au lieu de chaînes](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [Identité ASP.NET 2.0 : Personnalisation des utilisateurs et des rôles](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC et identité 2.0 : comprendre les principes de base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [Configuration de la Validation du compte et l’autorisation de deux facteurs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [Configuration de connexion de base de données et de Migration de Code First pour les comptes d’identité dans ASP.NET MVC 5 et Visual Studio 2013](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- Par [Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [Jeton l’authentification à l’aide d’ASP.NET Web API 2 et intergiciel (middleware) Owin ASP.NET Identity](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [Authentification par jeton AngularJS à l’aide d’ASP.NET Web API 2, Owin et l’identité](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [Activer les jetons d’actualisation OAuth dans l’application AngularJS à l’aide des API Web ASP .NET 2 et Owin – partie 3.](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- Par [Anders Abel](https://twitter.com/anders_abel)

    - [Compréhension du Pipeline d’authentification Owin externe](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [ASP.NET Identity et vue d’ensemble de Owin](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

  Par [K. Scott Allen](https://twitter.com/OdeToCode) sur Ode au Code

    - [ASP.NET Core Identity](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx) ce billet de blog examine les abstractions principales, y compris IUser, IUserStore et le\*Store interfaces.
    - [ASP.NET Identity avec Entity Framework](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) des comptes d’utilisateur individuels dans les applications MVC 5, Web API et SPA, chaînes de connexion et la gestion des contextes
    - [Options de personnalisation avec ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [Implémentation d’ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Benjamin Day](http://www.benday.com/about/)[procédure pas à pas : identité ASP.NET MVC avec authentification du compte Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Brock Allen](https://twitter.com/BrockLAllen)

    - [Notions fondamentales sur les fournisseurs de connexion externes (connexions sociales) avec l’intergiciel d’authentification OWIN/Katana](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [Présentation de IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/): un ensemble d’extensions vers ASP.NET Identity qui implémentent les principales fonctionnalités manquantes que j’ai jugeaient beaucoup trop.
- [Pranav Rastogi](https://twitter.com/rustd)

    - [Obtenir plus d’informations à partir de réseaux sociaux fournisseurs](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar) (Jerrie Pelser)

    - [Authentification à 2 facteurs](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [À l’aide de l’authentificateur de Google avec ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [Guides de l’authentification ASP.NET MVC 5](http://www.beabigrockstar.com/)
- [Obtenir plus d’informations provenant de fournisseurs sociaux utilisés dans les modèles de projet Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [Création d’une simple application ToDo avec ASP.NET Identity et l’association d’utilisateurs avec ToDoes](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Problèmes d’intégration de Google OpenId avec ASP.NET Identity](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/) si vous obtenez l’erreur : HTTP erreur 404.15 – introuvable sur le module de filtrage des demandes est configuré pour refuser une demande où la chaîne de requête est trop longue
- [Thinktecture.IdentityManager comme un remplacement pour le WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Authentification par jeton AngularJS à l’aide d’ASP.NET Web API 2, Owin et l’identité](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Core d’identité Asp.net simple sans Entity Framework](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [Utilisation des rôles dans ASP.NET Identity pour MVC](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc) par [Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [Déplacement vers ASP.NET Identity à partir de l’appartenance ASP.NET](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) par Alistair Matthews

<a id="video"></a>
## <a name="videos"></a>Vidéos

- Channel 9 [sécurisation des Applications ASP.NET et Services : restructuration de sécurité pour les Applications modernes](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid) par Ido Flatow
- Channel 9 [ASP.NET Identity Intro](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security) Pranav rastogi
- Channel 9 [authentification ASP.NET à l’aide d’ASP.NET Identity](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) par Cory Fowler
- Channel 9 [création d’applications Web modernes : ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03) par Jeff Koch
- Channel 9 [sécurisation de votre site Web avec ASP.NET Identity](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity) par Alex Thissen
- [Utiliser ASP.NET Identity sur un modèle de base de données existant](https://www.youtube.com/watch?v=elfqejow5hM) par Alexander Schmidt
- [Identité ASP.NET un](https://www.youtube.com/watch?v=w8GD-QIusKk) par Ivaylo Kenov de Telerik
- [Tchèque ASP.NET Identity](https://www.youtube.com/watch?v=tVbZp5brcpY) dans ce cours, nous allons montrer comment déployer l’authentification de base, comment ajouter la prise en charge des fournisseurs d’identité externes tels que Twitter ou Facebook et comment utiliser les mots de passe à usage unique (OTP). [ASP.NET Identity je nástupce un fournisseur de rôles d’appartenance&#367; v ASP.NET, tedy knihovna zajišt pro&#283;ní autentizace uživatel&#367;. V této p&#345;ednášce si ukážeme, jak nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>Fournisseurs de stockage personnalisés pour ASP.NET Identity

Si vous souhaitez écrire votre propre fournisseur, consultez [vue d’ensemble de fournisseurs de stockage personnalisés pour ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) et [implémentation ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) , puis examiner la source d’un des projets OSS répertoriés ci-dessous.

- Didacticiel : [vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) par Tom FitzMacken
- Blog : [implémentation ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Didacticiel :[configurer les comptes d’identité base et les vers une base de données externe](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Par [ @xivSolutions ](https://twitter.com/xivSolutions).
- Didacticiel[: implémentation d’un fournisseur de stockage ASP.NET Identity MySQL personnalisé](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Les entités CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) par [SoftFluent](http://www.softfluent.com/)
- [Stockage Table Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) de James Randall.
- Stockage Table Azure : [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) par [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant par Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Rec élastique[h: identité élastique](https://github.com/bmbsqd/elastic-identity) par AB. Bombsquad
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) par Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) par Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) par [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) par [ILMServices](http://www.ilmservice.com/).
- Redis : [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modèles T4 pour EF de générer du code pour un magasin d’utilisateurs « base de données tout d’abord » : [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>Ressources supplémentaires ASP.NET Identity

- [Présentation des fournisseurs de sécurité Yahoo et LinkedIn OAuth pour OWIN](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) par Jerrie Pelser pour obtenir des instructions Yahoo et LinkedIn.

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>Q&amp;un (questions/réponses)

- Verrouillé q : les utilisateurs qui ont activé « mémoriser » (afin qu’ils n’aient à passer par 2FA sur cet ordinateur/navigateur) ne sont pas verrouillés. Pourquoi et comment empêcher que ? Réponse [ici](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie).
- **Q**: Comment puis-je stocker des revendications personnalisées, telles que le nom d’utilisateur réel, dans le cookie d’identité ASP.NET afin d’éviter les requêtes inutiles de la base de données à chaque demande. Réponse [ici](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time).
- **La mise à jour le hachage de mot de passe d’AspNetUser q :**: j’ai 2 projets. Un d’eux est à l’aide de l’authentification ASP.NET, l’autre utilise l’authentification Windows, qui est le côté de l’administration. Je veux le projet de l’administrateur pour pouvoir gérer les utilisateurs de l’autre. Je peux modifier tout sauf le mot de passe. [Répondre ici](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash).
- **Q**: Comment puis-je réinitialiser mot de passe en tant qu’administrateur pour d’autres utilisateurs ? Réponse [ici](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766).
- **Q**: puis-je modifier le nom affiché du champ de nom d’utilisateur dans ASP.NET MVC IdentityUser ? Réponse [ici](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse).
- **Q**: comment pouvez contrat aux utilisateurs des autorisations pour ajouter d’autres utilisateurs à certains rôles ? Réponse [ici](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide).
- **Q**: stockage des informations de profil dans la table AspNetUsers et la table AspNetUserClaims. Réponse [ici](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne).
- **Q**: Mémoriser mes informations lors de l’utilisation d’un fournisseur d’authentification externe. Réponse [ici](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used).
- **Q**: pourquoi chaque demande requiert un ApplicationDBContext, n’est pas qui surcharge trop importante ?. La réponse, non, la surcharge est faible.
- Q : comment obtenir la liste des utilisateurs connectés ? Réponse [ici](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/).
- Q : Comment puis-je détecter quand un utilisateur se connecte avec Microsoft.AspNet.Identity ? Réponse [ici](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- Q : comment obtenir des messages d’erreur localisés pour l’identité ? Réponse [ici](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864).
- Q : comment pour configurer le CookieMiddleware pour obtenir des revendications actualisées toutes les 30 minutes ? Réponse [ici](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932).
- Q : comment modifier les revendications pour l’utilisateur une fois qu’ils se sont connectés ? Réponse [ici](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963).
- Q : comment invalider les jetons de sécurité ? Réponse [ici](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286).
- Q : comment... : magasin les revendications dans le middleware de cookie ? Réponse [ici](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856).
- Q : J’aimerais ont un code confidentiel ou la vérification sur chaque méthode d’action dans mon application MVC de la sécurité, mais j’aimerais stocker la réussite des utilisateurs afin qu’ils n’aient à entrer le code PIN à chaque demande à cette méthode d’action. Réponse [ici](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075).
- Q : J’aimerais pour enregistrer l’adresse de messagerie retourné à partir d’un fournisseur de réseau social à la base de données, comment faire ? Réponse [ici](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- Q : Comment puis-je détecter lorsqu’un utilisateur connecte dans les deux avec/sans en un cookie « mémoriser » ? Réponse [ici](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- Q : puis-je modifier les revendications dans ASP.NET Identity avec OWIN après l’appel de connexion ? Réponse : Connexion de l’appel est exactement ce que vous êtes censé pour faire lorsque vous souhaitez modifier les revendications pour l’utilisateur. En fait, elle force le ClaimsIdentity à sérialiser dans le cookie, c’est pourquoi vous voyez la nouvelle revendication n’apparaissent sur les demandes suivantes.
