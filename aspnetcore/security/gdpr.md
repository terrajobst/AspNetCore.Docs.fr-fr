---
title: Règlement de Protection de données générales (RGPD) prend en charge dans ASP.NET Core
author: rick-anderson
description: Découvrez comment accéder aux points d’extension RGPD dans une application web ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/05/2019
uid: security/gdpr
ms.openlocfilehash: 1580187afef56e8e2f5be7a4bae32912e6305c5a
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152863"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="f4671-103">Prise en charge de l’Union européenne général Protection des données règlement (RGPD) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4671-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="f4671-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f4671-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f4671-105">ASP.NET Core fournit des API et des modèles pour répondre à certaines de la [règlement de Protection générale données (RGPD) d’Europe](https://www.eugdpr.org/) exigences :</span><span class="sxs-lookup"><span data-stu-id="f4671-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="f4671-106">Les modèles de projet incluent des points d’extension et de stub balisage que vous pouvez remplacer votre stratégie d’utilisation de cookies et de confidentialité.</span><span class="sxs-lookup"><span data-stu-id="f4671-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="f4671-107">Une fonctionnalité de consentement de cookie vous permet demandera (et le suivi) consentement à partir de vos utilisateurs pour le stockage des informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="f4671-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="f4671-108">Si un utilisateur n’a pas consenti à la collecte de données et de l’application a [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) défini sur `true`, les cookies non essentielles ne sont pas envoyés au navigateur.</span><span class="sxs-lookup"><span data-stu-id="f4671-108">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="f4671-109">Les cookies peuvent être marqués comme essentielles.</span><span class="sxs-lookup"><span data-stu-id="f4671-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="f4671-110">Les cookies essentielles sont envoyés au navigateur même lorsque l’utilisateur n’a pas donné son consentement et le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="f4671-110">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="f4671-111">[Les cookies TempData et Session](#tempdata) ne sont pas fonctionnelles lorsque le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="f4671-111">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="f4671-112">Le [gérer les identités](#pd) page fournit un lien pour télécharger et supprimer des données de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f4671-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="f4671-113">Le [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) permet de vous tester la plupart des points d’extension RGPD et API ajoutées aux modèles ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f4671-113">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="f4671-114">Consultez le [Lisez-moi](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) fichier d’instructions de test.</span><span class="sxs-lookup"><span data-stu-id="f4671-114">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="f4671-115">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4671-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="f4671-116">ASP.NET Core RGPD prennent en charge dans le code généré par le modèle</span><span class="sxs-lookup"><span data-stu-id="f4671-116">ASP.NET Core GDPR support in template-generated code</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f4671-117">Les Pages Razor et MVC les projets créés avec les modèles de projet n’ont aucune prise en charge de RGPD ou un cookie de consentement.</span><span class="sxs-lookup"><span data-stu-id="f4671-117">Razor Pages and MVC projects created with the project templates have no support for GDPR or cookie consent.</span></span> <span data-ttu-id="f4671-118">Pour ajouter le RGPD, copiez le code généré dans les modèles ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="f4671-118">To add GDPR, copy the code generated in the ASP.NET Core 2.2 templates.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f4671-119">Les Pages Razor et MVC projets créés avec les modèles de projet incluent la prise en charge de RGPD suivante :</span><span class="sxs-lookup"><span data-stu-id="f4671-119">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

::: moniker-end

* <span data-ttu-id="f4671-120">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) et [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) sont définies le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="f4671-120">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="f4671-121">Le  *\_CookieConsentPartial.cshtml* [vue partielle](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f4671-121">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="f4671-122">Un **Accept** bouton est inclus dans ce fichier.</span><span class="sxs-lookup"><span data-stu-id="f4671-122">An **Accept** button is included in this file.</span></span> <span data-ttu-id="f4671-123">Lorsque l’utilisateur clique sur le **Accept** bouton, de donner son consentement pour stocker des cookies est fourni.</span><span class="sxs-lookup"><span data-stu-id="f4671-123">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="f4671-124">Le *Pages/Privacy.cshtml* page ou *Views/Home/Privacy.cshtml* vue fournit une page qui décrit en détail la politique de confidentialité de votre site.</span><span class="sxs-lookup"><span data-stu-id="f4671-124">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="f4671-125">Le  *\_CookieConsentPartial.cshtml* fichier génère un lien vers la page de la confidentialité.</span><span class="sxs-lookup"><span data-stu-id="f4671-125">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="f4671-126">Pour les applications créées avec des comptes d’utilisateur individuels, la page de gestion fournit des liens pour télécharger et supprimer des [données personnelles utilisateur](#pd).</span><span class="sxs-lookup"><span data-stu-id="f4671-126">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="f4671-127">CookiePolicyOptions et UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="f4671-127">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="f4671-128">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) sont initialisées dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f4671-128">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="f4671-129">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) est appelée `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f4671-129">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="f4671-130">\_Vue partielle CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="f4671-130">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="f4671-131">Le  *\_CookieConsentPartial.cshtml* vue partielle :</span><span class="sxs-lookup"><span data-stu-id="f4671-131">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="f4671-132">Partielle :</span><span class="sxs-lookup"><span data-stu-id="f4671-132">This partial:</span></span>

* <span data-ttu-id="f4671-133">Obtient l’état de suivi pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f4671-133">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="f4671-134">Si l’application est configurée pour demander le consentement, l’utilisateur doit donner son consentement avant que les cookies peuvent être suivis.</span><span class="sxs-lookup"><span data-stu-id="f4671-134">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="f4671-135">Si un consentement est requis, le panneau de consentement de cookie est résolu en haut de la barre de navigation créée par le  *\_Layout.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="f4671-135">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="f4671-136">Fournit un élément HTML `<p>` élément permet de synthétiser votre confidentialité et le cookie utiliser la stratégie.</span><span class="sxs-lookup"><span data-stu-id="f4671-136">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="f4671-137">Fournit un lien vers la page de la confidentialité ou la vue dans laquelle vous pouvez décrit en détail la politique de confidentialité de votre site.</span><span class="sxs-lookup"><span data-stu-id="f4671-137">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="f4671-138">Cookies essentielles</span><span class="sxs-lookup"><span data-stu-id="f4671-138">Essential cookies</span></span>

<span data-ttu-id="f4671-139">Si donner son consentement pour stocker des cookies n’a pas été fourni, uniquement les cookies marqués essentielles sont envoyés au navigateur.</span><span class="sxs-lookup"><span data-stu-id="f4671-139">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="f4671-140">Le code suivant fait un cookie essentielles :</span><span class="sxs-lookup"><span data-stu-id="f4671-140">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="f4671-141">Cookies d’état de session et le fournisseur TempData ne sont pas essentiels</span><span class="sxs-lookup"><span data-stu-id="f4671-141">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="f4671-142">Le [fournisseur TempData](xref:fundamentals/app-state#tempdata) cookie n’est pas indispensable.</span><span class="sxs-lookup"><span data-stu-id="f4671-142">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="f4671-143">Si le suivi est désactivé, le fournisseur de TempData n’est pas fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="f4671-143">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="f4671-144">Pour activer le fournisseur TempData lorsque le suivi est désactivé, marquer le cookie de TempData comme essentielle dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f4671-144">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="f4671-145">[État de session](xref:fundamentals/app-state) les cookies ne sont pas indispensables.</span><span class="sxs-lookup"><span data-stu-id="f4671-145">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="f4671-146">État de session n’est pas fonctionnel lorsque le suivi est désactivé.</span><span class="sxs-lookup"><span data-stu-id="f4671-146">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="f4671-147">Le code suivant fait les cookies de session essentielles :</span><span class="sxs-lookup"><span data-stu-id="f4671-147">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="f4671-148">Données personnelles</span><span class="sxs-lookup"><span data-stu-id="f4671-148">Personal data</span></span>

<span data-ttu-id="f4671-149">Les applications ASP.NET Core créées avec les comptes d’utilisateur individuels incluent du code pour télécharger et supprimer des données personnelles.</span><span class="sxs-lookup"><span data-stu-id="f4671-149">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="f4671-150">Sélectionnez le nom d’utilisateur, puis **les données personnelles**:</span><span class="sxs-lookup"><span data-stu-id="f4671-150">Select the user name and then select **Personal data**:</span></span>

![Gérer les données personnelles](gdpr/_static/pd.png)

<span data-ttu-id="f4671-152">Remarques :</span><span class="sxs-lookup"><span data-stu-id="f4671-152">Notes:</span></span>

* <span data-ttu-id="f4671-153">Pour générer le `Account/Manage` de code, consultez [identité d’une structure](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="f4671-153">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="f4671-154">Le **supprimer** et **télécharger** liens agissent uniquement sur les données d’identité par défaut.</span><span class="sxs-lookup"><span data-stu-id="f4671-154">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="f4671-155">Les applications qui créent des données utilisateur personnalisé doivent être étendues pour delete/télécharger les données utilisateur personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f4671-155">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="f4671-156">Pour plus d’informations, consultez [ajouter, télécharger et supprimer les données d’utilisateur personnalisée pour identité](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="f4671-156">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="f4671-157">Enregistré des jetons pour l’utilisateur qui sont stockées dans la table de base de données d’identité `AspNetUserTokens` sont supprimés lorsque l’utilisateur est supprimé via le comportement de suppression en cascade en raison du [clé étrangère](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="f4671-157">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="f4671-158">[L’authentification du fournisseur externe](xref:security/authentication/social/index), tels que Facebook et Google, n’est pas disponible avant l’acceptation de la stratégie de cookie.</span><span class="sxs-lookup"><span data-stu-id="f4671-158">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="f4671-159">Chiffrement au repos</span><span class="sxs-lookup"><span data-stu-id="f4671-159">Encryption at rest</span></span>

<span data-ttu-id="f4671-160">Certaines bases de données et les mécanismes de stockage autorisent le chiffrement au repos.</span><span class="sxs-lookup"><span data-stu-id="f4671-160">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="f4671-161">Chiffrement au repos :</span><span class="sxs-lookup"><span data-stu-id="f4671-161">Encryption at rest:</span></span>

* <span data-ttu-id="f4671-162">Chiffre automatiquement les données stockées.</span><span class="sxs-lookup"><span data-stu-id="f4671-162">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="f4671-163">Chiffre sans configuration, de programmation ou d’autres tâches pour le logiciel qui accède aux données.</span><span class="sxs-lookup"><span data-stu-id="f4671-163">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="f4671-164">Option est la plus simple et plus sûre.</span><span class="sxs-lookup"><span data-stu-id="f4671-164">Is the easiest and safest option.</span></span>
* <span data-ttu-id="f4671-165">Permet de gérer les clés et chiffrement de la base de données.</span><span class="sxs-lookup"><span data-stu-id="f4671-165">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="f4671-166">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f4671-166">For example:</span></span>

* <span data-ttu-id="f4671-167">Microsoft SQL et SQL Azure fournissent [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="f4671-167">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="f4671-168">SQL Azure chiffre de la base de données par défaut</span><span class="sxs-lookup"><span data-stu-id="f4671-168">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="f4671-169">[Objets BLOB, de fichiers, de Table et d’un stockage file d’attente Azure sont chiffrées par défaut](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="f4671-169">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="f4671-170">Pour les bases de données qui ne fournissent pas un chiffrement intégré au repos, vous pourrez peut-être utiliser le chiffrement de disque pour fournir le même niveau de protection.</span><span class="sxs-lookup"><span data-stu-id="f4671-170">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="f4671-171">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f4671-171">For example:</span></span>

* [<span data-ttu-id="f4671-172">BitLocker pour Windows Server</span><span class="sxs-lookup"><span data-stu-id="f4671-172">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="f4671-173">Linux :</span><span class="sxs-lookup"><span data-stu-id="f4671-173">Linux:</span></span>
  * [<span data-ttu-id="f4671-174">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="f4671-174">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="f4671-175">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="f4671-175">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4671-176">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f4671-176">Additional resources</span></span>

* [<span data-ttu-id="f4671-177">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="f4671-177">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
