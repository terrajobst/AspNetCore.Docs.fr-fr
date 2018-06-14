---
title: Protection de données générales règlement (PIBR) prend en charge dans ASP.NET Core
author: rick-anderson
description: Découvrez comment accéder aux points d’extension PIBR dans une application de web ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: c3c8a3fcd4a303aea65c57ff6be2ff0434383f33
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341923"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Prise en charge de l’Union européenne général données Protection règlement (PIBR) dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core fournit des API et des modèles pour répondre à certaines de la [Europe général données Protection règlement (PIBR)](https://www.eugdpr.org/) configuration requise :

* Les modèles de projet incluent des points d’extension et de stub balisage que vous pouvez remplacer par votre confidentialité et de la stratégie d’utilisation de cookies.
* Une fonctionnalité de consentement de cookie vous permet demandera (et le suivi) consentement à partir de vos utilisateurs pour le stockage des informations personnelles. Si un utilisateur n’a pas accepté de collecte de données et l’application est configurée avec [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) à `true`, les cookies non essentiels n’être envoyées au navigateur.
* Les cookies peuvent être marqués comme essentielles. Les cookies essentielles sont envoyés dans le navigateur même lorsque l’utilisateur n’a pas été accepté et le suivi est désactivé.
* [Les cookies de Session et TempData](#tempdata) ne fonctionnent pas lorsque le suivi est désactivé.
* Le [gérer les identités](#pd) page fournit un lien pour télécharger et supprimer des données de l’utilisateur.

Le [exemple d’application](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) vous permet de tester la plupart des API ajouté aux modèles ASP.NET Core 2.1 et PIBR des points d’extension. Consultez le [Lisez-moi](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) fichier pour tester les instructions.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Prise en charge d’ASP.NET Core PIBR dans le modèle de code généré

Pages Razor et MVC projets créés avec les modèles de projet incluent la prise en charge PIBR suivante :

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) et [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) sont définies dans `Startup`.
* Le *_CookieConsentPartial.cshtml* [vue partielle](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* Le *Pages/Privacy.cshtml* ou *Home/Privacy.cshtml* affichage fournit une page détaillé politique de confidentialité de votre site. Le *_CookieConsentPartial.cshtml* fichier génère un lien vers la page de la confidentialité.
* Pour les applications créées avec des comptes d’utilisateur individuels, la page de gestion fournit des liens pour télécharger et supprimer [personnelle](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions et UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) sont initialisés dans le `Startup` classe `ConfigureServices` méthode :

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) est appelée dans le `Startup` classe `Configure` méthode :

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>Vue partielle de _CookieConsentPartial.cshtml

Le *_CookieConsentPartial.cshtml* vue partielle :

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Partielle :

* Obtient l’état de suivi pour l’utilisateur. Si l’application est configurée pour exiger le consentement de que l’utilisateur doit accepter que les cookies puissent être suivies. Si un accord est requis, le chrome de consentement de cookie est fixe au-dessus de la barre de navigation créée dans le *Pages/Shared/_Layout.cshtml* fichier.
* Fournit un élément HTML `<p>` élément permet de synthétiser confidentialité et cookies utiliser la stratégie.
* Fournit un lien vers *Pages/Privacy.cshtml* où vous pouvez détailler la politique de confidentialité de votre site.

## <a name="essential-cookies"></a>Cookies essentiels

Si le consentement n’ont pas été attribué, uniquement les cookies marqués essentielles sont envoyés au navigateur. Le code suivant fait un cookie essentielles :

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Cookies du état TempData fournisseur et la session ne sont pas essentiels

Le [Tempdata fournisseur](xref:fundamentals/app-state#tempdata) cookie n’est pas indispensable. Si le suivi est désactivé, le fournisseur de Tempdata n’est pas fonctionnel. Pour activer le fournisseur Tempdata lorsque le suivi est désactivé, marquer le cookie TempData comme essentielle dans `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[État de session](xref:fundamentals/app-state) les cookies ne sont pas indispensables. État de session ne fonctionne pas lorsque le suivi est désactivé.

<a name="pd"></a>

## <a name="personal-data"></a>Données personnelles

Les applications ASP.NET Core créées avec des comptes d’utilisateur individuels incluent du code pour télécharger et supprimer des données personnelles.

Sélectionnez le nom d’utilisateur, puis sélectionnez **données personnelles**:

![Gérer les données personnelles](gdpr/_static/pd.png)

Remarques :

* Pour générer le `Account/Manage` de code, consultez [Scaffold identité](xref:security/authentication/scaffold-identity).
* Supprimer et télécharger uniquement impact sur les données d’identité par défaut. Les données utilisateur personnalisées de création d’applications doivent être étendues pour supprimer/télécharger les données utilisateur personnalisées. Problème GitHub [comment ajouter/supprimer des données utilisateur personnalisées pour l’identité](https://github.com/aspnet/Docs/issues/6226) effectue le suivi d’un article proposé sur la création personnalisée/suppression/téléchargement des données utilisateur personnalisées. Si vous souhaitez voir cette rubrique hiérarchisée, laissez un pouce vers le haut la réaction dans le problème.
* Enregistré des jetons pour l’utilisateur qui sont stockées dans la table de base de données d’identité `AspNetUserTokens` sont supprimés lorsque l’utilisateur est supprimé via le comportement de suppression en cascade due à la [clé étrangère](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Chiffrement au repos

Certaines bases de données et les mécanismes de stockage permettent un chiffrement au repos. Chiffrement au repos :

* Chiffre les données stockées automatiquement.
* Chiffre sans configuration, programmation ou autres tâches du logiciel qui accède aux données.
* Option est la plus simple et plus sûre.
* Permet de gérer les clés et chiffrement de la base de données.

Exemple :

* Microsoft SQL et SQL Azure fournissent [chiffrement Transparent des données](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure chiffre de la base de données par défaut](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Objets BLOB, fichiers, Table Azure et stockage de file d’attente sont chiffrés par défaut](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Pour les bases de données qui ne fournissent pas un chiffrement intégré au repos vous pourrez peut-être utiliser le chiffrement de disque pour fournir le même niveau de protection. Exemple :

* [BitLocker pour Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux :
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
