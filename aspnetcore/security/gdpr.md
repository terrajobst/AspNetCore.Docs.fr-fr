---
title: Règlement de Protection de données générales (RGPD) prend en charge dans ASP.NET Core
author: rick-anderson
description: Découvrez comment accéder aux points d’extension RGPD dans une application web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 01d2f8943c0995c1400122b89c4ca7c459a85279
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724565"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Prise en charge de l’Union européenne général Protection des données règlement (RGPD) dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core fournit des API et des modèles pour répondre à certaines de la [règlement de Protection générale données (RGPD) d’Europe](https://www.eugdpr.org/) exigences :

::: moniker range=">= aspnetcore-3.0"

* Les modèles de projet incluent des points d’extension et de stub balisage que vous pouvez remplacer votre stratégie d’utilisation de cookies et de confidentialité.
* Le *Pages/Privacy.cshtml* page ou *Views/Home/Privacy.cshtml* vue fournit une page qui décrit en détail la politique de confidentialité de votre site.

Pour activer la fonctionnalité de consentement de cookie par défaut comme celui trouvé dans les modèles ASP.NET Core 2.2 dans une application de modèle générée d’ASP.NET Core 3.0 :

* Ajouter [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) trop `Startup.ConfigureServices` et [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) à `Startup.Configure`:

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* Ajouter le partielle de consentement de cookie pour le *_Layout.cshtml* fichier :

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* Ajouter le  *\_CookieConsentPartial.cshtml* fichier au projet :

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* Sélectionnez la version d’ASP.NET Core 2.2 de cet article pour en savoir plus sur la fonctionnalité de consentement du cookie.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* Les modèles de projet incluent des points d’extension et de stub balisage que vous pouvez remplacer votre stratégie d’utilisation de cookies et de confidentialité.
* Une fonctionnalité de consentement de cookie vous permet demandera (et le suivi) consentement à partir de vos utilisateurs pour le stockage des informations personnelles. Si un utilisateur n’a pas consenti à la collecte de données et de l’application a [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) défini sur `true`, les cookies non essentielles ne sont pas envoyés au navigateur.
* Les cookies peuvent être marqués comme essentielles. Les cookies essentielles sont envoyés au navigateur même lorsque l’utilisateur n’a pas donné son consentement et le suivi est désactivé.
* [Les cookies TempData et Session](#tempdata) ne sont pas fonctionnelles lorsque le suivi est désactivé.
* Le [gérer les identités](#pd) page fournit un lien pour télécharger et supprimer des données de l’utilisateur.

Le [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) permet de vous tester la plupart des points d’extension RGPD et API ajoutées aux modèles ASP.NET Core 2.1. Consultez le [Lisez-moi](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) fichier d’instructions de test.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>ASP.NET Core RGPD prennent en charge dans le code généré par le modèle

Les Pages Razor et MVC projets créés avec les modèles de projet incluent la prise en charge de RGPD suivante :

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) et [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) sont définies le `Startup` classe.
* Le  *\_CookieConsentPartial.cshtml* [vue partielle](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). Un **Accept** bouton est inclus dans ce fichier. Lorsque l’utilisateur clique sur le **Accept** bouton, de donner son consentement pour stocker des cookies est fourni.
* Le *Pages/Privacy.cshtml* page ou *Views/Home/Privacy.cshtml* vue fournit une page qui décrit en détail la politique de confidentialité de votre site. Le  *\_CookieConsentPartial.cshtml* fichier génère un lien vers la page de la confidentialité.
* Pour les applications créées avec des comptes d’utilisateur individuels, la page de gestion fournit des liens pour télécharger et supprimer des [données personnelles utilisateur](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions et UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) sont initialisées dans `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) est appelée `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>\_Vue partielle CookieConsentPartial.cshtml

Le  *\_CookieConsentPartial.cshtml* vue partielle :

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

Partielle :

* Obtient l’état de suivi pour l’utilisateur. Si l’application est configurée pour demander le consentement, l’utilisateur doit donner son consentement avant que les cookies peuvent être suivis. Si un consentement est requis, le panneau de consentement de cookie est résolu en haut de la barre de navigation créée par le  *\_Layout.cshtml* fichier.
* Fournit un élément HTML `<p>` élément permet de synthétiser votre confidentialité et le cookie utiliser la stratégie.
* Fournit un lien vers la page de la confidentialité ou la vue dans laquelle vous pouvez décrit en détail la politique de confidentialité de votre site.

## <a name="essential-cookies"></a>Cookies essentielles

Si donner son consentement pour stocker des cookies n’a pas été fourni, uniquement les cookies marqués essentielles sont envoyés au navigateur. Le code suivant fait un cookie essentielles :

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>Cookies d’état de session et le fournisseur TempData ne sont pas essentiels

Le [fournisseur TempData](xref:fundamentals/app-state#tempdata) cookie n’est pas indispensable. Si le suivi est désactivé, le fournisseur de TempData n’est pas fonctionnel. Pour activer le fournisseur TempData lorsque le suivi est désactivé, marquer le cookie de TempData comme essentielle dans `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

[État de session](xref:fundamentals/app-state) les cookies ne sont pas indispensables. État de session n’est pas fonctionnel lorsque le suivi est désactivé. Le code suivant fait les cookies de session essentielles :

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Données personnelles

Les applications ASP.NET Core créées avec les comptes d’utilisateur individuels incluent du code pour télécharger et supprimer des données personnelles.

Sélectionnez le nom d’utilisateur, puis **les données personnelles**:

![Gérer les données personnelles](gdpr/_static/pd.png)

Remarques :

* Pour générer le `Account/Manage` de code, consultez [identité d’une structure](xref:security/authentication/scaffold-identity).
* Le **supprimer** et **télécharger** liens agissent uniquement sur les données d’identité par défaut. Les applications qui créent des données utilisateur personnalisé doivent être étendues pour delete/télécharger les données utilisateur personnalisées. Pour plus d’informations, consultez [ajouter, télécharger et supprimer les données d’utilisateur personnalisée pour identité](xref:security/authentication/add-user-data).
* Enregistré des jetons pour l’utilisateur qui sont stockées dans la table de base de données d’identité `AspNetUserTokens` sont supprimés lorsque l’utilisateur est supprimé via le comportement de suppression en cascade en raison du [clé étrangère](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).
* [L’authentification du fournisseur externe](xref:security/authentication/social/index), tels que Facebook et Google, n’est pas disponible avant l’acceptation de la stratégie de cookie.

::: moniker-end

## <a name="encryption-at-rest"></a>Chiffrement au repos

Certaines bases de données et les mécanismes de stockage autorisent le chiffrement au repos. Chiffrement au repos :

* Chiffre automatiquement les données stockées.
* Chiffre sans configuration, de programmation ou d’autres tâches pour le logiciel qui accède aux données.
* Option est la plus simple et plus sûre.
* Permet de gérer les clés et chiffrement de la base de données.

Par exemple :

* Microsoft SQL et SQL Azure fournissent [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure chiffre de la base de données par défaut](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Objets BLOB, de fichiers, de Table et d’un stockage file d’attente Azure sont chiffrées par défaut](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Pour les bases de données qui ne fournissent pas un chiffrement intégré au repos, vous pourrez peut-être utiliser le chiffrement de disque pour fournir le même niveau de protection. Par exemple :

* [BitLocker pour Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux :
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
