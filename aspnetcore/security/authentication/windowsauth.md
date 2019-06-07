---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core pour IIS et HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/05/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 900bbf5f14b1876ad537b2b77e4ba07d7aa168f2
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750161"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="e8fd4-103">Configurer l’authentification Windows dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8fd4-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="e8fd4-104">Par [Scott Addie](https://twitter.com/Scott_Addie) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e8fd4-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e8fd4-105">[L’authentification Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) peut être configuré pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="e8fd4-106">L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="e8fd4-107">Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou les comptes Windows pour identifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="e8fd4-108">L’authentification Windows est mieux adaptée aux environnements intranet où les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="e8fd4-109">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="e8fd4-109">IIS/IIS Express</span></span>

<span data-ttu-id="e8fd4-110">Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> espace de noms) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e8fd4-110">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="e8fd4-111">Lancer des paramètres (débogueur)</span><span class="sxs-lookup"><span data-stu-id="e8fd4-111">Launch settings (debugger)</span></span>

<span data-ttu-id="e8fd4-112">Configuration des paramètres de lancement affecte uniquement le *Properties/launchSettings.json* de fichiers pour IIS Express et ne configure IIS pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-112">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="e8fd4-113">Configuration du serveur est expliquée dans le [IIS](#iis) section.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-113">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="e8fd4-114">Le **Web Application** modèle disponible via Visual Studio ou l’interface CLI .NET Core peut être configuré pour prendre en charge l’authentification Windows, ce qui met à jour le *Properties/launchSettings.json* fichier automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-114">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8fd4-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8fd4-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e8fd4-116">**nouveau projet**</span><span class="sxs-lookup"><span data-stu-id="e8fd4-116">**New project**</span></span>

1. <span data-ttu-id="e8fd4-117">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-117">Create a new project.</span></span>
1. <span data-ttu-id="e8fd4-118">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e8fd4-119">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-119">Select **Next**.</span></span>
1. <span data-ttu-id="e8fd4-120">Fournissez un nom dans la **nom_projet** champ.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-120">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="e8fd4-121">Confirmer la **emplacement** entrée est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="e8fd4-122">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-122">Select **Create**.</span></span>
1. <span data-ttu-id="e8fd4-123">Sélectionnez **modification** sous **authentification**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-123">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="e8fd4-124">Dans le **modifier l’authentification** fenêtre, sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-124">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="e8fd4-125">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-125">Select **OK**.</span></span>
1. <span data-ttu-id="e8fd4-126">Sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-126">Select **Web Application**.</span></span>
1. <span data-ttu-id="e8fd4-127">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-127">Select **Create**.</span></span>

<span data-ttu-id="e8fd4-128">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-128">Run the app.</span></span> <span data-ttu-id="e8fd4-129">Le nom d’utilisateur s’affiche dans l’interface utilisateur de l’application rendue.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-129">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="e8fd4-130">**Projet existant**</span><span class="sxs-lookup"><span data-stu-id="e8fd4-130">**Existing project**</span></span>

<span data-ttu-id="e8fd4-131">Les propriétés du projet activer l’authentification Windows et désactivez l’authentification anonyme :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-131">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="e8fd4-132">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-132">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="e8fd4-133">Sélectionnez l’onglet **Débogage**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-133">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="e8fd4-134">Désactivez la case à cocher **activer l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-134">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="e8fd4-135">Sélectionnez la case à cocher **activer l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-135">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="e8fd4-136">Enregistrez et fermez la page de propriétés.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-136">Save and close the property page.</span></span>

<span data-ttu-id="e8fd4-137">Vous pouvez également les propriétés peuvent être configurées dans le `iisSettings` nœud de la *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-137">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="e8fd4-138">Visual Studio Code/.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e8fd4-138">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="e8fd4-139">**nouveau projet**</span><span class="sxs-lookup"><span data-stu-id="e8fd4-139">**New project**</span></span>

<span data-ttu-id="e8fd4-140">Exécuter le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande avec le `webapp` argument (application de Web ASP.NET Core) et `--auth Windows` commutateur :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-140">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="e8fd4-141">**Projet existant**</span><span class="sxs-lookup"><span data-stu-id="e8fd4-141">**Existing project**</span></span>

<span data-ttu-id="e8fd4-142">Mise à jour le `iisSettings` nœud de la *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-142">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="e8fd4-143">Lorsque vous modifiez un projet existant, vérifiez que le fichier projet contient une référence de package pour le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) **ou** le [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-143">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="e8fd4-144">IIS</span><span class="sxs-lookup"><span data-stu-id="e8fd4-144">IIS</span></span>

<span data-ttu-id="e8fd4-145">IIS utilise le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-145">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="e8fd4-146">L’authentification Windows est configurée pour IIS via le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-146">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="e8fd4-147">Les sections suivantes montrent comment :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-147">The following sections show how to:</span></span>

* <span data-ttu-id="e8fd4-148">Fournir une variable locale *web.config* fichier qui active l’authentification Windows sur le serveur quand l’application est déployée.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-148">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="e8fd4-149">Utilisez le Gestionnaire des services Internet pour configurer le *web.config* fichier d’une application ASP.NET Core qui a déjà été déployée sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-149">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="e8fd4-150">Si vous n’avez pas déjà fait, activez IIS pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-150">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="e8fd4-151">Pour plus d'informations, consultez <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-151">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="e8fd4-152">Activer le Service de rôle IIS pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-152">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="e8fd4-153">Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-153">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="e8fd4-154">[Intergiciel d’intégration IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) est configuré pour authentifier les demandes automatiquement par défaut.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-154">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="e8fd4-155">Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : Options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-155">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="e8fd4-156">Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-156">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="e8fd4-157">Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-157">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="e8fd4-158">Utilisez **soit** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-158">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="e8fd4-159">**Avant la publication et le déploiement du projet,** ajoutez le code suivant *web.config* fichier à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-159">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="e8fd4-160">Lorsque le projet est publié par le SDK .NET Core (sans le `<IsTransformWebConfigDisabled>` propriété définie sur `true` dans le fichier projet), publié *web.config* fichier inclut le `<location><system.webServer><security><authentication>` section.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-160">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="e8fd4-161">Pour plus d’informations sur la `<IsTransformWebConfigDisabled>` propriété, consultez <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-161">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="e8fd4-162">**Après la publication et le déploiement du projet,** effectuer la configuration côté serveur avec le Gestionnaire des services Internet :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-162">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="e8fd4-163">Dans le Gestionnaire des services Internet, sélectionnez le site IIS sous la **Sites** nœud de la **connexions** encadré.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-163">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="e8fd4-164">Double-cliquez sur **authentification** dans le **IIS** zone.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-164">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="e8fd4-165">Sélectionnez **l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-165">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="e8fd4-166">Sélectionnez **désactiver** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-166">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="e8fd4-167">Sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-167">Select **Windows Authentication**.</span></span> <span data-ttu-id="e8fd4-168">Sélectionnez **activer** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-168">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="e8fd4-169">Lorsque ces actions sont effectuées, le Gestionnaire des services Internet modifie l’application *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-169">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="e8fd4-170">Un `<system.webServer><security><authentication>` nœud est ajouté avec les paramètres mis à jour pour `anonymousAuthentication` et `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e8fd4-170">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="e8fd4-171">Le `<system.webServer>` section ajoutée à la *web.config* fichier par le Gestionnaire des services Internet se trouve en dehors de l’application `<location>` section ajoutée par le SDK .NET Core lors de l’application est publiée.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-171">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="e8fd4-172">Étant donné que la section est ajoutée à l’extérieur de la `<location>` nœud, les paramètres sont hérités par les [sous-applications](xref:host-and-deploy/iis/index#sub-applications) à l’application actuelle.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-172">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="e8fd4-173">Pour empêcher l’héritage, déplacer la `<security>` section à l’intérieur de la `<location><system.webServer>` section fourni le SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-173">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="e8fd4-174">Lorsque le Gestionnaire des services Internet est utilisé pour ajouter la configuration d’IIS, il affecte uniquement l’application *web.config* fichier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-174">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="e8fd4-175">Un autre déploiement de l’application peut remplacer les paramètres sur le serveur si la copie du serveur de *web.config* est remplacé par le projet *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-175">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="e8fd4-176">Utilisez **soit** des approches suivantes pour gérer les paramètres :</span><span class="sxs-lookup"><span data-stu-id="e8fd4-176">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="e8fd4-177">Utilisez le Gestionnaire IIS pour réinitialiser les paramètres dans le *web.config* une fois que le fichier est remplacé sur le déploiement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-177">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="e8fd4-178">Ajouter un *fichier web.config* à l’application localement avec les paramètres.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-178">Add a *web.config file* to the app locally with the settings.</span></span>

## <a name="httpsys"></a><span data-ttu-id="e8fd4-179">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="e8fd4-179">HTTP.sys</span></span>

<span data-ttu-id="e8fd4-180">Dans les scénarios auto-hébergés, [Kestrel](xref:fundamentals/servers/kestrel) ne prise en charge l’authentification Windows, mais vous pouvez utiliser [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-180">In self-hosted scenarios, [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, but you can use [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="e8fd4-181">Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espace de noms) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e8fd4-181">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="e8fd4-182">Configurer l’hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-182">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="e8fd4-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> est dans le <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espace de noms.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="e8fd4-184">HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-184">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="e8fd4-185">L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-185">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="e8fd4-186">Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-186">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="e8fd4-187">Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-187">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="e8fd4-188">HTTP.sys n’est pas pris en charge sur Nano Server version 1709 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-188">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="e8fd4-189">Pour utiliser l’authentification Windows et HTTP.sys avec Nano Server, utilisez un [conteneur de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-189">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="e8fd4-190">Pour plus d’informations sur Server Core, consultez [quel est l’option d’installation Server Core de Windows Server ?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="e8fd4-190">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="e8fd4-191">Autoriser les utilisateurs</span><span class="sxs-lookup"><span data-stu-id="e8fd4-191">Authorize users</span></span>

<span data-ttu-id="e8fd4-192">L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-192">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="e8fd4-193">Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-193">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="e8fd4-194">Interdire l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="e8fd4-194">Disallow anonymous access</span></span>

<span data-ttu-id="e8fd4-195">Lorsque l’authentification Windows est activée et que l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-195">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="e8fd4-196">Si un site IIS est configuré pour interdire l’accès anonyme, la demande n’atteint jamais l’application.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-196">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="e8fd4-197">Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-197">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="e8fd4-198">Autoriser l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="e8fd4-198">Allow anonymous access</span></span>

<span data-ttu-id="e8fd4-199">Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-199">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="e8fd4-200">Le `[Authorize]` attribut vous permet de sécuriser les points de terminaison de l’application qui requièrent une authentification.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-200">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="e8fd4-201">Le `[AllowAnonymous]` substitutions d’attribut le `[Authorize]` attribut dans les applications qui autorisent l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-201">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="e8fd4-202">Pour plus d’informations de l’utilisation d’attribut, consultez <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-202">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="e8fd4-203">Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-203">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="e8fd4-204">Le [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».</span><span class="sxs-lookup"><span data-stu-id="e8fd4-204">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="e8fd4-205">emprunt d'identité</span><span class="sxs-lookup"><span data-stu-id="e8fd4-205">Impersonation</span></span>

<span data-ttu-id="e8fd4-206">ASP.NET Core n’implémente pas l’emprunt d’identité.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-206">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="e8fd4-207">Applications s’exécutent avec l’identité d’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-207">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="e8fd4-208">Si l’application doit effectuer une action de la part d’un utilisateur, utilisez [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) dans un [intergiciel (middleware) inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-208">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="e8fd4-209">Exécuter une action unique dans ce contexte, puis fermez le contexte.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-209">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="e8fd4-210">`RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-210">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="e8fd4-211">Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-211">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="e8fd4-212">Transformations de revendications</span><span class="sxs-lookup"><span data-stu-id="e8fd4-212">Claims transformations</span></span>

<span data-ttu-id="e8fd4-213">Lorsque vous hébergez avec mode in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelée en interne pour initialiser un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-213">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="e8fd4-214">Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-214">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="e8fd4-215">Pour plus d’informations et un exemple de code qui active les transformations de revendications lors de l’hébergement intra-processus, consultez <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="e8fd4-215">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8fd4-216">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e8fd4-216">Additional resources</span></span>

* [<span data-ttu-id="e8fd4-217">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="e8fd4-217">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
