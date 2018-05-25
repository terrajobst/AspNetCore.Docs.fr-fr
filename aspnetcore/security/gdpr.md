---
title: Protection de données générales règlement (PIBR) prend en charge dans ASP.NET Core
author: rick-anderson
description: Montre comment accéder aux points d’extension PIBR dans un cœur d’ASP.NET de l’application web.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: dc1724e8a78c25d3697d14ad784ce853737681f2
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2018
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="24670-103">Prise en charge de l’Union européenne général données Protection règlement (PIBR) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24670-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="24670-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="24670-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="24670-105">ASP.NET Core fournit des API et des modèles pour répondre à certaines de la [UE général données Protection règlement (PIBR)](https://www.eugdpr.org/) configuration requise :</span><span class="sxs-lookup"><span data-stu-id="24670-105">ASP.NET Core provides APIs and templates to help meet some of the [UE General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="24670-106">Les modèles de projet incluent des points d’extension et de stub balisage que vous pouvez remplacer par votre confidentialité et de la stratégie d’utilisation de cookies.</span><span class="sxs-lookup"><span data-stu-id="24670-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="24670-107">Une fonctionnalité de consentement de cookie vous permet demandera (et le suivi) consentement à partir de vos utilisateurs pour le stockage des informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="24670-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="24670-108">Si un utilisateur n’a pas accepté de collecte de données et l’application est configurée avec [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) à `true`, les cookies non essentiels n’être envoyées au navigateur.</span><span class="sxs-lookup"><span data-stu-id="24670-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="24670-109">Les cookies peuvent être marqués comme essentielles.</span><span class="sxs-lookup"><span data-stu-id="24670-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="24670-110">Les cookies essentielles sont envoyés dans le navigateur même lorsque l’utilisateur n’a pas été accepté et le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="24670-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="24670-111">[Les cookies de Session et TempData](#tempdata) ne fonctionnent pas lorsque le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="24670-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="24670-112">Le [gérer les identités](#pd) page fournit un lien pour télécharger et supprimer des données de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="24670-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="24670-113">Le [exemple d’application](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) vous permet de tester la plupart des API ajouté aux modèles ASP.NET Core 2.1 et PIBR des points d’extension.</span><span class="sxs-lookup"><span data-stu-id="24670-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="24670-114">Consultez le [Lisez-moi](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) fichier pour tester les instructions.</span><span class="sxs-lookup"><span data-stu-id="24670-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="24670-115">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="24670-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="24670-116">Prise en charge d’ASP.NET Core PIBR dans le modèle de code généré</span><span class="sxs-lookup"><span data-stu-id="24670-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="24670-117">Pages Razor et MVC projets créés avec les modèles de projet incluent la prise en charge PIBR suivante :</span><span class="sxs-lookup"><span data-stu-id="24670-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="24670-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) et [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sont définies dans `Startup`.</span><span class="sxs-lookup"><span data-stu-id="24670-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="24670-119">Le *_CookieConsentPartial.cshtml* [vue partielle](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="24670-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="24670-120">Le *Pages/Privacy.cshtml* ou *Home/rivacy.cshtml* affichage fournit une page détaillé politique de confidentialité de votre site.</span><span class="sxs-lookup"><span data-stu-id="24670-120">The *Pages/Privacy.cshtml*  or *Home/rivacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="24670-121">Le *_CookieConsentPartial.cshtml* fichier génère un lien vers la page de la confidentialité.</span><span class="sxs-lookup"><span data-stu-id="24670-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="24670-122">Pour les applications créées avec des comptes d’utilisateur individuels, la page de gestion fournit des liens pour télécharger et supprimer [personnelle](#pd).</span><span class="sxs-lookup"><span data-stu-id="24670-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="24670-123">CookiePolicyOptions et UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="24670-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="24670-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) sont initialisés dans le `Startup` classe `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="24670-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="24670-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) est appelée dans le `Startup` classe `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="24670-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="24670-126">Vue partielle de _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="24670-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="24670-127">Le *_CookieConsentPartial.cshtml* vue partielle :</span><span class="sxs-lookup"><span data-stu-id="24670-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="24670-128">Partielle :</span><span class="sxs-lookup"><span data-stu-id="24670-128">This partial:</span></span>

* <span data-ttu-id="24670-129">Obtient l’état de suivi pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="24670-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="24670-130">Si l’application est configurée pour exiger le consentement de que l’utilisateur doit accepter que les cookies puissent être suivies.</span><span class="sxs-lookup"><span data-stu-id="24670-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="24670-131">Si un accord est requis, le chrome de consentement de cookie est fixe au-dessus de la barre de navigation créée dans le *Pages/Shared/_Layout.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="24670-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="24670-132">Fournit un élément HTML `<p>` élément permet de synthétiser confidentialité et cookies utiliser la stratégie.</span><span class="sxs-lookup"><span data-stu-id="24670-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="24670-133">Fournit un lien vers *Pages/Privacy.cshtml* où vous pouvez détailler la politique de confidentialité de votre site.</span><span class="sxs-lookup"><span data-stu-id="24670-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="24670-134">Cookies essentiels</span><span class="sxs-lookup"><span data-stu-id="24670-134">Essential cookies</span></span>

<span data-ttu-id="24670-135">Si le consentement n’ont pas été attribué, uniquement les cookies marqués essentielles sont envoyés au navigateur.</span><span class="sxs-lookup"><span data-stu-id="24670-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="24670-136">Le code suivant fait un cookie essentielles :</span><span class="sxs-lookup"><span data-stu-id="24670-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="24670-137">Cookies du état TempData fournisseur et la session ne sont pas essentiels</span><span class="sxs-lookup"><span data-stu-id="24670-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="24670-138">Le [Tempdata fournisseur](xref:fundamentals/app-state#tempdata) cookie n’est pas indispensable.</span><span class="sxs-lookup"><span data-stu-id="24670-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="24670-139">Si le suivi est désactivé, le fournisseur de Tempdata n’est pas fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="24670-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="24670-140">Pour activer le fournisseur Tempdata lorsque le suivi est désactivé, marquer le cookie TempData comme essentielle dans `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="24670-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="24670-141">[État de session](xref:fundamentals/app-state) les cookies ne sont pas indispensables.</span><span class="sxs-lookup"><span data-stu-id="24670-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="24670-142">État de session ne fonctionne pas lorsque le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="24670-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="24670-143">Données personnelles</span><span class="sxs-lookup"><span data-stu-id="24670-143">Personal data</span></span>

<span data-ttu-id="24670-144">Les applications ASP.NET Core créées avec des comptes d’utilisateur individuels incluent du code pour télécharger et supprimer des données personnelles.</span><span class="sxs-lookup"><span data-stu-id="24670-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="24670-145">Sélectionnez le nom d’utilisateur, puis sélectionnez **données personnelles**:</span><span class="sxs-lookup"><span data-stu-id="24670-145">Select the user name and then select **Personal data**:</span></span>

![Gérer les données personnelles](gdpr/_static/pd.png)

<span data-ttu-id="24670-147">Remarques :</span><span class="sxs-lookup"><span data-stu-id="24670-147">Notes:</span></span>

* <span data-ttu-id="24670-148">Pour générer le `Account/Manage` de code, consultez [Scaffold identité](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="24670-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="24670-149">Supprimer et télécharger uniquement impact sur les données d’identité par défaut.</span><span class="sxs-lookup"><span data-stu-id="24670-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="24670-150">Les données utilisateur personnalisées de création d’applications doivent être étendues pour supprimer/télécharger les données utilisateur personnalisées.</span><span class="sxs-lookup"><span data-stu-id="24670-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="24670-151">Problème GitHub [comment ajouter/supprimer des données utilisateur personnalisées pour l’identité](https://github.com/aspnet/Docs/issues/6226) effectue le suivi d’un article proposé sur la création personnalisée/suppression/téléchargement des données utilisateur personnalisées.</span><span class="sxs-lookup"><span data-stu-id="24670-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="24670-152">Si vous souhaitez voir cette rubrique hiérarchisée, laissez un pouce vers le haut la réaction dans le problème.</span><span class="sxs-lookup"><span data-stu-id="24670-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="24670-153">Enregistré des jetons pour l’utilisateur qui sont stockées dans la table de base de données d’identité `AspNetUserTokens` sont supprimés lorsque l’utilisateur est supprimé via le comportement de suppression en cascade due à la [clé étrangère](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="24670-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="24670-154">Chiffrement au repos</span><span class="sxs-lookup"><span data-stu-id="24670-154">Encryption at rest</span></span>

<span data-ttu-id="24670-155">Certaines bases de données et les mécanismes de stockage permettent un chiffrement au repos.</span><span class="sxs-lookup"><span data-stu-id="24670-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="24670-156">Chiffrement au repos :</span><span class="sxs-lookup"><span data-stu-id="24670-156">Encryption at rest:</span></span>

* <span data-ttu-id="24670-157">Chiffre les données stockées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="24670-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="24670-158">Chiffre sans configuration, programmation ou autres tâches du logiciel qui accède aux données.</span><span class="sxs-lookup"><span data-stu-id="24670-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="24670-159">Option est la plus simple et plus sûre.</span><span class="sxs-lookup"><span data-stu-id="24670-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="24670-160">Permet de gérer les clés et chiffrement de la base de données.</span><span class="sxs-lookup"><span data-stu-id="24670-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="24670-161">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="24670-161">For example:</span></span>

* <span data-ttu-id="24670-162">Microsoft SQL et SQL Azure fournissent [chiffrement Transparent des données](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span><span class="sxs-lookup"><span data-stu-id="24670-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="24670-163">SQL Azure chiffre de la base de données par défaut</span><span class="sxs-lookup"><span data-stu-id="24670-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="24670-164">[Objets BLOB, fichiers, Table Azure et stockage de file d’attente sont chiffrés par défaut](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="24670-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="24670-165">Pour les bases de données qui ne fournissent pas un chiffrement intégré au repos vous pourrez peut-être utiliser le chiffrement de disque pour fournir le même niveau de protection.</span><span class="sxs-lookup"><span data-stu-id="24670-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="24670-166">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="24670-166">For example:</span></span>

* [<span data-ttu-id="24670-167">BitLocker pour windows server</span><span class="sxs-lookup"><span data-stu-id="24670-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="24670-168">Linux :</span><span class="sxs-lookup"><span data-stu-id="24670-168">Linux:</span></span>
  * [<span data-ttu-id="24670-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="24670-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="24670-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="24670-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24670-171">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24670-171">Additional Resources</span></span>

* [<span data-ttu-id="24670-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="24670-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
