---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758191"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="79b00-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="79b00-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="79b00-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="79b00-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="79b00-105">Une application ASP.NET Core peut être hébergée sur Windows sans utiliser IIS en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="79b00-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="79b00-106">Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.</span><span class="sxs-lookup"><span data-stu-id="79b00-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="79b00-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="79b00-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="79b00-108">Convertir un projet en service Windows</span><span class="sxs-lookup"><span data-stu-id="79b00-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="79b00-109">Les modifications minimales suivantes sont nécessaires pour configurer un projet ASP.NET Core existant afin qu’il s’exécute en tant que service :</span><span class="sxs-lookup"><span data-stu-id="79b00-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="79b00-110">Dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="79b00-110">In the project file:</span></span>

   * <span data-ttu-id="79b00-111">Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le à `<PropertyGroup>` qui contient la version cible du .Net Framework :</span><span class="sxs-lookup"><span data-stu-id="79b00-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="79b00-112">Pour publier pour plusieurs RID :</span><span class="sxs-lookup"><span data-stu-id="79b00-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="79b00-113">Fournissez les RID dans une liste séparée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="79b00-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="79b00-114">Utilisez le nom de la propriété `<RuntimeIdentifiers>` (pluriel).</span><span class="sxs-lookup"><span data-stu-id="79b00-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="79b00-115">Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="79b00-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="79b00-116">Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="79b00-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="79b00-117">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="79b00-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="79b00-118">Appelez [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) au lieu de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="79b00-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="79b00-119">Appelez [UseContentRoot](xref:fundamentals/host/web-host#content-root) et utilisez un chemin vers l’emplacement publié de l’application au lieu de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="79b00-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="79b00-120">Publiez l’application avec [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="79b00-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="79b00-121">Si vous utilisez Visual Studio, sélectionnez **FolderProfile** et configurez **Emplacement cible** avant de sélectionner le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="79b00-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="79b00-122">Pour publier l’exemple d’application avec des outils de l’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) à une invite de commandes à partir du dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="79b00-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="79b00-123">Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="79b00-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="79b00-124">Dans l’exemple suivant, l’application est publiée dans la configuration Release pour le runtime `win7-x64` sur un dossier créé à l’emplacement *c:\\svc* :</span><span class="sxs-lookup"><span data-stu-id="79b00-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="79b00-125">Créez un compte d’utilisateur pour le service à l’aide de la commande `net user` :</span><span class="sxs-lookup"><span data-stu-id="79b00-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="79b00-126">Pour l’exemple d’application, créez un compte d’utilisateur avec le nom `ServiceUser` et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="79b00-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="79b00-127">Dans la commande suivante, remplacez `{PASSWORD}` par un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="79b00-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="79b00-128">Si vous devez ajouter l’utilisateur à un groupe, utilisez la commande `net localgroup`, où `{GROUP}` est le nom du groupe :</span><span class="sxs-lookup"><span data-stu-id="79b00-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="79b00-129">Pour plus d’informations, consultez [Comptes d’utilisateur de service](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="79b00-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="79b00-130">Accordez l’accès en écriture/lecture/exécution au dossier de l’application à l’aide de la commande [icacls](/windows-server/administration/windows-commands/icacls) :</span><span class="sxs-lookup"><span data-stu-id="79b00-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="79b00-131">`{PATH}` &ndash; Chemin au dossier de l’application.</span><span class="sxs-lookup"><span data-stu-id="79b00-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="79b00-132">`{USER ACCOUNT}` &ndash; Compte d’utilisateur (SID).</span><span class="sxs-lookup"><span data-stu-id="79b00-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="79b00-133">`(OI)` &ndash; L’indicateur Object Inherit propage les autorisations aux fichiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="79b00-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="79b00-134">`(CI)` &ndash; L’indicateur Container Inherit propage les autorisations aux dossiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="79b00-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="79b00-135">`{PERMISSION FLAGS}` &ndash; Définit les autorisations d’accès de l’application.</span><span class="sxs-lookup"><span data-stu-id="79b00-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="79b00-136">Écriture (`W`)</span><span class="sxs-lookup"><span data-stu-id="79b00-136">Write (`W`)</span></span>
     * <span data-ttu-id="79b00-137">Lecture (`R`)</span><span class="sxs-lookup"><span data-stu-id="79b00-137">Read (`R`)</span></span>
     * <span data-ttu-id="79b00-138">Exécution (`X`)</span><span class="sxs-lookup"><span data-stu-id="79b00-138">Execute (`X`)</span></span>
     * <span data-ttu-id="79b00-139">Complet (`F`)</span><span class="sxs-lookup"><span data-stu-id="79b00-139">Full (`F`)</span></span>
     * <span data-ttu-id="79b00-140">Modification (`M`)</span><span class="sxs-lookup"><span data-stu-id="79b00-140">Modify (`M`)</span></span>
   * <span data-ttu-id="79b00-141">`/t` &ndash; Appliquer de manière récursive aux dossiers et fichiers subordonnés existants.</span><span class="sxs-lookup"><span data-stu-id="79b00-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="79b00-142">Pour l’exemple d’application publiée dans le dossier *c:\\svc* et le compte `ServiceUser` avec des autorisations en écriture/lecture/exécution, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="79b00-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="79b00-143">Pour plus d’informations, consultez [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="79b00-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="79b00-144">Utilisez l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service.</span><span class="sxs-lookup"><span data-stu-id="79b00-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="79b00-145">La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="79b00-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="79b00-146">**L’espace entre le signe égal à la fin du paramètre et le guillemet au début de la valeur est obligatoire.**</span><span class="sxs-lookup"><span data-stu-id="79b00-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="79b00-147">`{SERVICE NAME}` &ndash; Nom à attribuer au service dans le [Gestionnaire de contrôle des services](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="79b00-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="79b00-148">`{PATH}` &ndash; Chemin à l’exécutable du service.</span><span class="sxs-lookup"><span data-stu-id="79b00-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="79b00-149">`{DOMAIN}` (ou, si l’ordinateur n’est pas joint à un domaine, nom de l’ordinateur local) et `{USER ACCOUNT}` &ndash; Domaine (ou nom de l’ordinateur local) et compte d’utilisateur sous lesquels le service s’exécute.</span><span class="sxs-lookup"><span data-stu-id="79b00-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="79b00-150">**N’oubliez pas** le paramètre `obj`.</span><span class="sxs-lookup"><span data-stu-id="79b00-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="79b00-151">`obj` a pour valeur par défaut le compte [LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="79b00-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="79b00-152">L’exécution d’un service sous le compte `LocalSystem` présente un risque de sécurité important.</span><span class="sxs-lookup"><span data-stu-id="79b00-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="79b00-153">Veillez à toujours exécuter un service sous un compte d’utilisateur disposant de privilèges limités sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="79b00-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="79b00-154">`{PASSWORD}` &ndash; Mot de passe du compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="79b00-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="79b00-155">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="79b00-155">In the following example:</span></span>

   * <span data-ttu-id="79b00-156">Le service s’appelle **MyService**.</span><span class="sxs-lookup"><span data-stu-id="79b00-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="79b00-157">Le service publié réside dans le dossier *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="79b00-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="79b00-158">L’exécutable de l’application s’appelle *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="79b00-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="79b00-159">La valeur `binPath` est placée entre des guillemets droits (").</span><span class="sxs-lookup"><span data-stu-id="79b00-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="79b00-160">Le service s’exécute sous le compte `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="79b00-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="79b00-161">Remplacez `{DOMAIN}` par le nom de l’ordinateur local ou le domaine du compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="79b00-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="79b00-162">Placez la valeur `obj` entre des guillemets droits (").</span><span class="sxs-lookup"><span data-stu-id="79b00-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="79b00-163">Exemple : Si le système d’hébergement est un ordinateur local nommé `MairaPC`, définissez `obj` avec la valeur `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="79b00-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="79b00-164">Remplacez `{PASSWORD}` par le mot de passe du compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="79b00-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="79b00-165">La valeur `password` est placée entre des guillemets droits (").</span><span class="sxs-lookup"><span data-stu-id="79b00-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="79b00-166">Vérifiez la présence d’espaces entre les signes égal et les valeurs des paramètres.</span><span class="sxs-lookup"><span data-stu-id="79b00-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="79b00-167">Démarrez le service avec la commande `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="79b00-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="79b00-168">Pour démarrer l’exemple de service d’application, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="79b00-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="79b00-169">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="79b00-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="79b00-170">Pour vérifier l’état du service, utilisez la commande `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="79b00-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="79b00-171">L’état est signalé comme étant l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="79b00-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="79b00-172">Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="79b00-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="79b00-173">Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).</span><span class="sxs-lookup"><span data-stu-id="79b00-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="79b00-174">Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="79b00-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="79b00-175">Arrêtez le service avec la commande `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="79b00-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="79b00-176">La commande suivante arrête l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="79b00-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="79b00-177">Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="79b00-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="79b00-178">Vérifiez l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="79b00-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="79b00-179">Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :</span><span class="sxs-lookup"><span data-stu-id="79b00-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="79b00-180">Exécuter l’application en dehors d’un service</span><span class="sxs-lookup"><span data-stu-id="79b00-180">Run the app outside of a service</span></span>

<span data-ttu-id="79b00-181">Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="79b00-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="79b00-182">Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :</span><span class="sxs-lookup"><span data-stu-id="79b00-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="79b00-183">Étant donné que la configuration d’ASP.NET Core nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé avant que les arguments ne soient passés à [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="79b00-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="79b00-184">`isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="79b00-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="79b00-185">Gérer les événements d’arrêt et de démarrage</span><span class="sxs-lookup"><span data-stu-id="79b00-185">Handle stopping and starting events</span></span>

<span data-ttu-id="79b00-186">Pour gérer les événements [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) et [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="79b00-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="79b00-187">Créez une classe qui dérive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) :</span><span class="sxs-lookup"><span data-stu-id="79b00-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="79b00-188">Créez une méthode d’extension pour [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) qui transmet le `WebHostService` personnalisé à [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) :</span><span class="sxs-lookup"><span data-stu-id="79b00-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="79b00-189">Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) :</span><span class="sxs-lookup"><span data-stu-id="79b00-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="79b00-190">`isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="79b00-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="79b00-191">Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) :</span><span class="sxs-lookup"><span data-stu-id="79b00-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="79b00-192">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="79b00-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="79b00-193">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="79b00-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="79b00-194">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="79b00-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="79b00-195">Configurer HTTPS</span><span class="sxs-lookup"><span data-stu-id="79b00-195">Configure HTTPS</span></span>

<span data-ttu-id="79b00-196">Pour configurer le service avec un point de terminaison sécurisé :</span><span class="sxs-lookup"><span data-stu-id="79b00-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="79b00-197">Créez un certificat X.509 pour le système d’hébergement à l’aide des mécanismes d’acquisition et de déploiement de certificat de votre plateforme.</span><span class="sxs-lookup"><span data-stu-id="79b00-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="79b00-198">Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) pour utiliser le certificat.</span><span class="sxs-lookup"><span data-stu-id="79b00-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="79b00-199">L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="79b00-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="79b00-200">Répertoire actif et racine du contenu</span><span class="sxs-lookup"><span data-stu-id="79b00-200">Current directory and content root</span></span>

<span data-ttu-id="79b00-201">Le répertoire de travail actif retourné par l’appel à `Directory.GetCurrentDirectory()` pour un service Windows est le dossier *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="79b00-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="79b00-202">Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres).</span><span class="sxs-lookup"><span data-stu-id="79b00-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="79b00-203">Adoptez l’une des approches suivantes pour tenir à jour et accéder aux ressources et aux fichiers de paramètres d’un service avec [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quand vous utilisez un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) :</span><span class="sxs-lookup"><span data-stu-id="79b00-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="79b00-204">Utilisez le chemin racine du contenu.</span><span class="sxs-lookup"><span data-stu-id="79b00-204">Use the content root path.</span></span> <span data-ttu-id="79b00-205">`IHostingEnvironment.ContentRootPath` est le même chemin que celui fourni à l’argument `binPath` quand le service est créé.</span><span class="sxs-lookup"><span data-stu-id="79b00-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="79b00-206">Au lieu d’utiliser `Directory.GetCurrentDirectory()` pour créer des chemins aux fichiers de paramètres, utilisez le chemin racine du contenu et tenez à jour les fichiers dans la racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="79b00-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="79b00-207">Stockez les fichiers à un emplacement approprié sur le disque.</span><span class="sxs-lookup"><span data-stu-id="79b00-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="79b00-208">Spécifiez un chemin absolu avec `SetBasePath` au dossier contenant les fichiers.</span><span class="sxs-lookup"><span data-stu-id="79b00-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79b00-209">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="79b00-209">Additional resources</span></span>

* <span data-ttu-id="79b00-210">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="79b00-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
