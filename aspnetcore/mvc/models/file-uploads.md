---
title: Charger des fichiers dans ASP.NET Core
author: guardrex
description: Comment utiliser la liaison de modèle et le streaming pour charger des fichiers dans ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 04e7533aa190a4875d3f66e8665fec16abec48b3
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73462942"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="77bc9-103">Charger des fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77bc9-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="77bc9-104">Par [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)et [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="77bc9-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="77bc9-105">ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux</span><span class="sxs-lookup"><span data-stu-id="77bc9-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="77bc9-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="77bc9-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="77bc9-107">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="77bc9-107">Security considerations</span></span>

<span data-ttu-id="77bc9-108">Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="77bc9-109">Les attaquants peuvent tenter d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="77bc9-110">Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="77bc9-111">Télécharger des virus ou des programmes malveillants.</span><span class="sxs-lookup"><span data-stu-id="77bc9-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="77bc9-112">Compromettre les réseaux et les serveurs d’une autre manière.</span><span class="sxs-lookup"><span data-stu-id="77bc9-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="77bc9-113">Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="77bc9-114">Chargez les fichiers dans une zone de chargement de fichier dédiée, de préférence sur un lecteur non-système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="77bc9-115">Un emplacement dédié permet d’imposer des restrictions de sécurité plus faciles sur les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="77bc9-116">Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="77bc9-117">Ne conservez **pas** les fichiers téléchargés dans la même arborescence de répertoires que l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="77bc9-118">Utilisez un nom de fichier sécurisé déterminé par l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="77bc9-119">N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé.&dagger; code HTML encode le nom de fichier non fiable lors de son affichage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="77bc9-120">Par exemple, la journalisation du nom de fichier ou l’affichage de l’interface utilisateur (Razor génère automatiquement le code HTML).</span><span class="sxs-lookup"><span data-stu-id="77bc9-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="77bc9-121">Autorisez uniquement les extensions de fichier approuvées pour la spécification de conception de l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="77bc9-122">Vérifiez que les vérifications côté client sont effectuées sur le serveur.&dagger; contrôles côté client sont faciles à contourner.</span><span class="sxs-lookup"><span data-stu-id="77bc9-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="77bc9-123">Vérifiez la taille d’un fichier téléchargé.</span><span class="sxs-lookup"><span data-stu-id="77bc9-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="77bc9-124">Définissez une limite de taille maximale pour empêcher les chargements volumineux.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="77bc9-125">Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="77bc9-126">**Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="77bc9-127">&dagger;l’exemple d’application illustre une approche qui répond aux critères.</span><span class="sxs-lookup"><span data-stu-id="77bc9-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-128">Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :</span><span class="sxs-lookup"><span data-stu-id="77bc9-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="77bc9-129">Maîtrisez complètement un système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="77bc9-130">Surchargez un système avec le résultat du blocage du système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="77bc9-131">Compromettre les données utilisateur ou système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="77bc9-132">Appliquez Graffiti à une interface utilisateur publique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="77bc9-133">Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="77bc9-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="77bc9-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="77bc9-135">Sécurité Azure : Vérifier que les contrôles appropriés sont en place quand vous acceptez des fichiers d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="77bc9-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="77bc9-136">Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="77bc9-137">Scénarios de stockage</span><span class="sxs-lookup"><span data-stu-id="77bc9-137">Storage scenarios</span></span>

<span data-ttu-id="77bc9-138">Les options de stockage courantes pour les fichiers sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-138">Common storage options for files include:</span></span>

* <span data-ttu-id="77bc9-139">Base de données</span><span class="sxs-lookup"><span data-stu-id="77bc9-139">Database</span></span>

  * <span data-ttu-id="77bc9-140">Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).</span><span class="sxs-lookup"><span data-stu-id="77bc9-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="77bc9-141">Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).</span><span class="sxs-lookup"><span data-stu-id="77bc9-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="77bc9-142">Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="77bc9-143">Stockage physique (système de fichiers ou partage réseau)</span><span class="sxs-lookup"><span data-stu-id="77bc9-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="77bc9-144">Pour les chargements de fichiers volumineux :</span><span class="sxs-lookup"><span data-stu-id="77bc9-144">For large file uploads:</span></span>
    * <span data-ttu-id="77bc9-145">Les limites de base de données peuvent limiter la taille du téléchargement.</span><span class="sxs-lookup"><span data-stu-id="77bc9-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="77bc9-146">Le stockage physique est souvent moins économique que le stockage dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="77bc9-147">Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="77bc9-148">Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="77bc9-149">**N’accordez jamais l’autorisation EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="77bc9-150">Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="77bc9-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="77bc9-151">Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.</span><span class="sxs-lookup"><span data-stu-id="77bc9-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="77bc9-152">Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.</span><span class="sxs-lookup"><span data-stu-id="77bc9-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="77bc9-153">Pour plus d’informations, consultez [démarrage rapide : utiliser .net pour créer un objet BLOB dans le stockage d’objets](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="77bc9-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="77bc9-154">La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisée pour enregistrer un <xref:System.IO.FileStream> dans le stockage d’objets BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="77bc9-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="77bc9-155">Scénarios de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="77bc9-155">File upload scenarios</span></span>

<span data-ttu-id="77bc9-156">La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="77bc9-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="77bc9-157">**Mise en tampon**</span><span class="sxs-lookup"><span data-stu-id="77bc9-157">**Buffering**</span></span>

<span data-ttu-id="77bc9-158">La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="77bc9-159">Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="77bc9-160">Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque.</span><span class="sxs-lookup"><span data-stu-id="77bc9-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="77bc9-161">Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="77bc9-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="77bc9-162">Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.</span><span class="sxs-lookup"><span data-stu-id="77bc9-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="77bc9-163">La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="77bc9-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="77bc9-164">Stockage physique</span><span class="sxs-lookup"><span data-stu-id="77bc9-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="77bc9-165">Base de données</span><span class="sxs-lookup"><span data-stu-id="77bc9-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="77bc9-166">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="77bc9-166">**Streaming**</span></span>

<span data-ttu-id="77bc9-167">Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="77bc9-168">La diffusion en continu n’améliore pas les performances de manière significative.</span><span class="sxs-lookup"><span data-stu-id="77bc9-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="77bc9-169">La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="77bc9-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="77bc9-170">La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="77bc9-171">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique</span><span class="sxs-lookup"><span data-stu-id="77bc9-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="77bc9-172">Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="77bc9-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="77bc9-173">L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :</span><span class="sxs-lookup"><span data-stu-id="77bc9-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="77bc9-174">L’exemple suivant est similaire à l’exemple précédent, à l’exception de :</span><span class="sxs-lookup"><span data-stu-id="77bc9-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="77bc9-175">JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.</span><span class="sxs-lookup"><span data-stu-id="77bc9-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="77bc9-176">Il n’y a aucune validation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-176">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="77bc9-177">Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="77bc9-178">Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="77bc9-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="77bc9-179">Utilisez `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="77bc9-180">Exemple :</span><span class="sxs-lookup"><span data-stu-id="77bc9-180">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="77bc9-181">Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type de codage (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="77bc9-182">Pour qu’un élément `files` Input prenne en charge le téléchargement de plusieurs fichiers, indiquez l’attribut `multiple` sur l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="77bc9-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="77bc9-183">Les fichiers individuels téléchargés sur le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="77bc9-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="77bc9-184">L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="77bc9-185">N’utilisez **pas** la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> autre que pour l’affichage et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="77bc9-186">Lors de l’affichage ou de la journalisation, le nom du fichier est encodé au format HTML.</span><span class="sxs-lookup"><span data-stu-id="77bc9-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="77bc9-187">Une personne malveillante peut fournir un nom de fichier malveillant, y compris les chemins d’accès complets ou les chemins d’accès relatifs.</span><span class="sxs-lookup"><span data-stu-id="77bc9-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="77bc9-188">Les applications doivent :</span><span class="sxs-lookup"><span data-stu-id="77bc9-188">Applications should:</span></span>
>
> * <span data-ttu-id="77bc9-189">Supprimez le chemin d’accès du nom de fichier fourni par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="77bc9-190">Enregistrez le nom de fichier encodé au format HTML, avec chemin d’accès supprimé pour l’interface utilisateur ou la journalisation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="77bc9-191">Générez un nouveau nom de fichier aléatoire pour le stockage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="77bc9-192">Le code suivant supprime le chemin d’accès du nom de fichier :</span><span class="sxs-lookup"><span data-stu-id="77bc9-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="77bc9-193">Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="77bc9-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="77bc9-194">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="77bc9-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="77bc9-195">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="77bc9-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="77bc9-196">Validation</span><span class="sxs-lookup"><span data-stu-id="77bc9-196">Validation</span></span>](#validation)

<span data-ttu-id="77bc9-197">Lors du chargement de fichiers à l’aide d’une liaison de modèle et <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :</span><span class="sxs-lookup"><span data-stu-id="77bc9-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="77bc9-198"><xref:Microsoft.AspNetCore.Http.IFormFile>unique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="77bc9-199">L’un des regroupements suivants qui représentent plusieurs fichiers :</span><span class="sxs-lookup"><span data-stu-id="77bc9-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="77bc9-200">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="77bc9-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="77bc9-201">La liaison correspond aux fichiers de formulaire par nom.</span><span class="sxs-lookup"><span data-stu-id="77bc9-201">Binding matches form files by name.</span></span> <span data-ttu-id="77bc9-202">Par exemple, la valeur de `name` HTML dans `<input type="file" name="formFile">` doit correspondre C# au paramètre/à la propriété lié (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="77bc9-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="77bc9-203">Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="77bc9-204">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="77bc9-204">The following example:</span></span>

* <span data-ttu-id="77bc9-205">Effectue une boucle sur un ou plusieurs fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="77bc9-206">Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="77bc9-207">Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="77bc9-208">Retourne le nombre total et la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-208">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="77bc9-209">Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="77bc9-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="77bc9-210">Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :</span><span class="sxs-lookup"><span data-stu-id="77bc9-210">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="77bc9-211">Le chemin d’accès passé au <xref:System.IO.FileStream> *doit* inclure le nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="77bc9-212">Si le nom de fichier n’est pas fourni, une <xref:System.UnauthorizedAccessException> est levée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="77bc9-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="77bc9-213">Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement.</span><span class="sxs-lookup"><span data-stu-id="77bc9-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="77bc9-214">À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="77bc9-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="77bc9-215">Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="77bc9-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="77bc9-216">Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-217">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents.</span><span class="sxs-lookup"><span data-stu-id="77bc9-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="77bc9-218">La limite de 65 535 fichiers est une limite par serveur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="77bc9-219">Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="77bc9-220">GetTempFileNameA fonction)</span><span class="sxs-lookup"><span data-stu-id="77bc9-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="77bc9-221">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données</span><span class="sxs-lookup"><span data-stu-id="77bc9-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="77bc9-222">Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de groupe <xref:System.Byte> sur l’entité :</span><span class="sxs-lookup"><span data-stu-id="77bc9-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="77bc9-223">Spécifiez une propriété de modèle de page pour la classe qui comprend une <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="77bc9-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="77bc9-224"><xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="77bc9-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="77bc9-225">L’exemple précédent utilise une propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="77bc9-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="77bc9-226">Le `FileUpload` est utilisé dans le formulaire de Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="77bc9-226">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="77bc9-227">Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="77bc9-228">Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :</span><span class="sxs-lookup"><span data-stu-id="77bc9-228">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="77bc9-229">L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="77bc9-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="77bc9-230">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="77bc9-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="77bc9-231">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="77bc9-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-232">Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.</span><span class="sxs-lookup"><span data-stu-id="77bc9-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="77bc9-233">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="77bc9-234">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.</span><span class="sxs-lookup"><span data-stu-id="77bc9-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="77bc9-235">Les exemples fournis ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="77bc9-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="77bc9-236">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="77bc9-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="77bc9-237">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="77bc9-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="77bc9-238">Validation</span><span class="sxs-lookup"><span data-stu-id="77bc9-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="77bc9-239">Charger des fichiers volumineux avec streaming</span><span class="sxs-lookup"><span data-stu-id="77bc9-239">Upload large files with streaming</span></span>

<span data-ttu-id="77bc9-240">L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="77bc9-241">Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="77bc9-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="77bc9-242">Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="77bc9-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="77bc9-243">Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié.</span><span class="sxs-lookup"><span data-stu-id="77bc9-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="77bc9-244">Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="77bc9-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="77bc9-245">La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="77bc9-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="77bc9-246">L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :</span><span class="sxs-lookup"><span data-stu-id="77bc9-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="77bc9-247">Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="77bc9-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="77bc9-248">Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page de `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="77bc9-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="77bc9-249">Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner).</span><span class="sxs-lookup"><span data-stu-id="77bc9-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="77bc9-250">La méthode d’action fonctionne directement avec la propriété `Request`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="77bc9-251">Un `MultipartReader` est utilisé pour lire chaque section.</span><span class="sxs-lookup"><span data-stu-id="77bc9-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="77bc9-252">Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="77bc9-253">Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.</span><span class="sxs-lookup"><span data-stu-id="77bc9-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="77bc9-254">La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :</span><span class="sxs-lookup"><span data-stu-id="77bc9-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="77bc9-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :</span><span class="sxs-lookup"><span data-stu-id="77bc9-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="77bc9-256">La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :</span><span class="sxs-lookup"><span data-stu-id="77bc9-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="77bc9-257">Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="77bc9-258">Validation</span><span class="sxs-lookup"><span data-stu-id="77bc9-258">Validation</span></span>

<span data-ttu-id="77bc9-259">La classe de `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu.</span><span class="sxs-lookup"><span data-stu-id="77bc9-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="77bc9-260">Pour traiter <xref:Microsoft.AspNetCore.Http.IFormFile> les chargements de fichiers mis en mémoire tampon dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="77bc9-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="77bc9-261">Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-262">Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="77bc9-263">Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.</span><span class="sxs-lookup"><span data-stu-id="77bc9-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="77bc9-264">Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :</span><span class="sxs-lookup"><span data-stu-id="77bc9-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="77bc9-265">Comprenez pleinement l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="77bc9-266">Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="77bc9-267">**N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="77bc9-268">Validation du contenu</span><span class="sxs-lookup"><span data-stu-id="77bc9-268">Content validation</span></span>

<span data-ttu-id="77bc9-269">**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="77bc9-270">L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé.</span><span class="sxs-lookup"><span data-stu-id="77bc9-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="77bc9-271">Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="77bc9-272">En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie.</span><span class="sxs-lookup"><span data-stu-id="77bc9-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="77bc9-273">Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal.</span><span class="sxs-lookup"><span data-stu-id="77bc9-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="77bc9-274">Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="77bc9-275">À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="77bc9-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="77bc9-276">Validation de l’extension de fichier</span><span class="sxs-lookup"><span data-stu-id="77bc9-276">File extension validation</span></span>

<span data-ttu-id="77bc9-277">L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées.</span><span class="sxs-lookup"><span data-stu-id="77bc9-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="77bc9-278">Exemple :</span><span class="sxs-lookup"><span data-stu-id="77bc9-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="77bc9-279">Validation de la signature de fichier</span><span class="sxs-lookup"><span data-stu-id="77bc9-279">File signature validation</span></span>

<span data-ttu-id="77bc9-280">La signature d’un fichier est déterminée par les premiers octets au début d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="77bc9-281">Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="77bc9-282">L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants.</span><span class="sxs-lookup"><span data-stu-id="77bc9-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="77bc9-283">Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :</span><span class="sxs-lookup"><span data-stu-id="77bc9-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="77bc9-284">Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.</span><span class="sxs-lookup"><span data-stu-id="77bc9-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="77bc9-285">Sécurité des noms de fichiers</span><span class="sxs-lookup"><span data-stu-id="77bc9-285">File name security</span></span>

<span data-ttu-id="77bc9-286">N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="77bc9-287">Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.</span><span class="sxs-lookup"><span data-stu-id="77bc9-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="77bc9-288">Razor code automatiquement les valeurs de propriété pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="77bc9-289">Le code suivant peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="77bc9-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="77bc9-290">En dehors de Razor, <xref:System.Net.WebUtility.HtmlEncode*> toujours le contenu du nom de fichier à partir de la demande d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="77bc9-291">De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom.</span><span class="sxs-lookup"><span data-stu-id="77bc9-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="77bc9-292">Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="77bc9-293">Validation de la taille</span><span class="sxs-lookup"><span data-stu-id="77bc9-293">Size validation</span></span>

<span data-ttu-id="77bc9-294">Limitez la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="77bc9-295">Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets).</span><span class="sxs-lookup"><span data-stu-id="77bc9-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="77bc9-296">La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="77bc9-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="77bc9-297">Le `FileSizeLimit` est injecté dans des classes `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="77bc9-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="77bc9-298">Quand une taille de fichier dépasse la limite, le fichier est rejeté :</span><span class="sxs-lookup"><span data-stu-id="77bc9-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="77bc9-299">Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication</span><span class="sxs-lookup"><span data-stu-id="77bc9-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="77bc9-300">Dans les formulaires non Razor qui PUBLIEnt des données de formulaire ou utilisent directement les `FormData` JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="77bc9-301">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="77bc9-301">In the following example:</span></span>

* <span data-ttu-id="77bc9-302">Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="77bc9-303">Lorsque vous utilisez `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="77bc9-304">Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :</span><span class="sxs-lookup"><span data-stu-id="77bc9-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="77bc9-305">Pour une méthode de gestionnaire de page Razor Pages nommée `Upload`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="77bc9-306">Pour une méthode d’action du billet de contrôleur MVC :</span><span class="sxs-lookup"><span data-stu-id="77bc9-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="77bc9-307">Configuration du serveur et de l’application</span><span class="sxs-lookup"><span data-stu-id="77bc9-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="77bc9-308">Longueur limite du corps en plusieurs parties</span><span class="sxs-lookup"><span data-stu-id="77bc9-308">Multipart body length limit</span></span>

<span data-ttu-id="77bc9-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="77bc9-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="77bc9-310">Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lors de l’analyse.</span><span class="sxs-lookup"><span data-stu-id="77bc9-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="77bc9-311">La valeur par défaut est 134 217 728 (128 Mo).</span><span class="sxs-lookup"><span data-stu-id="77bc9-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="77bc9-312">Personnalisez la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="77bc9-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="77bc9-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="77bc9-314">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    });
```

<span data-ttu-id="77bc9-315">Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="77bc9-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="77bc9-316">Taille maximale du corps de la demande Kestrel</span><span class="sxs-lookup"><span data-stu-id="77bc9-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="77bc9-317">Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="77bc9-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="77bc9-318">Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :</span><span class="sxs-lookup"><span data-stu-id="77bc9-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="77bc9-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="77bc9-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="77bc9-320">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    });
```

<span data-ttu-id="77bc9-321">Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="77bc9-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="77bc9-322">La `RequestSizeLimitAttribute` peut également être appliquée à l’aide de la directive Razor [@attribute](xref:mvc/views/razor#attribute) :</span><span class="sxs-lookup"><span data-stu-id="77bc9-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="77bc9-323">Autres limites Kestrel</span><span class="sxs-lookup"><span data-stu-id="77bc9-323">Other Kestrel limits</span></span>

<span data-ttu-id="77bc9-324">D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="77bc9-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="77bc9-325">Nombre maximal de connexions client</span><span class="sxs-lookup"><span data-stu-id="77bc9-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="77bc9-326">Taux de données de demande et de réponse</span><span class="sxs-lookup"><span data-stu-id="77bc9-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="77bc9-327">Limite de longueur du contenu IIS</span><span class="sxs-lookup"><span data-stu-id="77bc9-327">IIS content length limit</span></span>

<span data-ttu-id="77bc9-328">La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="77bc9-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="77bc9-329">Personnaliser la limite dans le fichier *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="77bc9-329">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="77bc9-330">Ce paramètre s’applique seulement à IIS.</span><span class="sxs-lookup"><span data-stu-id="77bc9-330">This setting only applies to IIS.</span></span> <span data-ttu-id="77bc9-331">Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="77bc9-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="77bc9-332">Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="77bc9-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="77bc9-333">Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go.</span><span class="sxs-lookup"><span data-stu-id="77bc9-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="77bc9-334">Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="77bc9-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="77bc9-335">Résoudre les problèmes</span><span class="sxs-lookup"><span data-stu-id="77bc9-335">Troubleshoot</span></span>

<span data-ttu-id="77bc9-336">Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.</span><span class="sxs-lookup"><span data-stu-id="77bc9-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="77bc9-337">Erreur introuvable lors du déploiement sur un serveur IIS</span><span class="sxs-lookup"><span data-stu-id="77bc9-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="77bc9-338">L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :</span><span class="sxs-lookup"><span data-stu-id="77bc9-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="77bc9-339">Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="77bc9-340">Échec de la connexion</span><span class="sxs-lookup"><span data-stu-id="77bc9-340">Connection failure</span></span>

<span data-ttu-id="77bc9-341">Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="77bc9-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="77bc9-342">Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="77bc9-343">Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.</span><span class="sxs-lookup"><span data-stu-id="77bc9-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="77bc9-344">Exception de référence null avec IFormFile</span><span class="sxs-lookup"><span data-stu-id="77bc9-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="77bc9-345">Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile> mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="77bc9-346">Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments de <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="77bc9-347">Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="77bc9-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="77bc9-348">ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux</span><span class="sxs-lookup"><span data-stu-id="77bc9-348">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="77bc9-349">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="77bc9-349">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="77bc9-350">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="77bc9-350">Security considerations</span></span>

<span data-ttu-id="77bc9-351">Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-351">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="77bc9-352">Les attaquants peuvent tenter d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-352">Attackers may attempt to:</span></span>

* <span data-ttu-id="77bc9-353">Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-353">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="77bc9-354">Télécharger des virus ou des programmes malveillants.</span><span class="sxs-lookup"><span data-stu-id="77bc9-354">Upload viruses or malware.</span></span>
* <span data-ttu-id="77bc9-355">Compromettre les réseaux et les serveurs d’une autre manière.</span><span class="sxs-lookup"><span data-stu-id="77bc9-355">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="77bc9-356">Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-356">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="77bc9-357">Chargez les fichiers dans une zone de chargement de fichier dédiée, de préférence sur un lecteur non-système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-357">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="77bc9-358">Un emplacement dédié permet d’imposer des restrictions de sécurité plus faciles sur les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-358">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="77bc9-359">Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-359">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="77bc9-360">Ne conservez **pas** les fichiers téléchargés dans la même arborescence de répertoires que l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-360">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="77bc9-361">Utilisez un nom de fichier sécurisé déterminé par l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-361">Use a safe file name determined by the app.</span></span> <span data-ttu-id="77bc9-362">N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé.&dagger; code HTML encode le nom de fichier non fiable lors de son affichage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-362">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="77bc9-363">Par exemple, la journalisation du nom de fichier ou l’affichage de l’interface utilisateur (Razor génère automatiquement le code HTML).</span><span class="sxs-lookup"><span data-stu-id="77bc9-363">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="77bc9-364">Autorisez uniquement les extensions de fichier approuvées pour la spécification de conception de l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-364">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="77bc9-365">Vérifiez que les vérifications côté client sont effectuées sur le serveur.&dagger; contrôles côté client sont faciles à contourner.</span><span class="sxs-lookup"><span data-stu-id="77bc9-365">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="77bc9-366">Vérifiez la taille d’un fichier téléchargé.</span><span class="sxs-lookup"><span data-stu-id="77bc9-366">Check the size of an uploaded file.</span></span> <span data-ttu-id="77bc9-367">Définissez une limite de taille maximale pour empêcher les chargements volumineux.&dagger;</span><span class="sxs-lookup"><span data-stu-id="77bc9-367">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="77bc9-368">Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-368">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="77bc9-369">**Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-369">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="77bc9-370">&dagger;l’exemple d’application illustre une approche qui répond aux critères.</span><span class="sxs-lookup"><span data-stu-id="77bc9-370">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-371">Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :</span><span class="sxs-lookup"><span data-stu-id="77bc9-371">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="77bc9-372">Maîtrisez complètement un système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-372">Completely gain control of a system.</span></span>
> * <span data-ttu-id="77bc9-373">Surchargez un système avec le résultat du blocage du système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-373">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="77bc9-374">Compromettre les données utilisateur ou système.</span><span class="sxs-lookup"><span data-stu-id="77bc9-374">Compromise user or system data.</span></span>
> * <span data-ttu-id="77bc9-375">Appliquez Graffiti à une interface utilisateur publique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-375">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="77bc9-376">Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-376">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="77bc9-377">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="77bc9-377">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="77bc9-378">Sécurité Azure : Vérifier que les contrôles appropriés sont en place quand vous acceptez des fichiers d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="77bc9-378">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="77bc9-379">Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-379">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="77bc9-380">Scénarios de stockage</span><span class="sxs-lookup"><span data-stu-id="77bc9-380">Storage scenarios</span></span>

<span data-ttu-id="77bc9-381">Les options de stockage courantes pour les fichiers sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-381">Common storage options for files include:</span></span>

* <span data-ttu-id="77bc9-382">Base de données</span><span class="sxs-lookup"><span data-stu-id="77bc9-382">Database</span></span>

  * <span data-ttu-id="77bc9-383">Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).</span><span class="sxs-lookup"><span data-stu-id="77bc9-383">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="77bc9-384">Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).</span><span class="sxs-lookup"><span data-stu-id="77bc9-384">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="77bc9-385">Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-385">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="77bc9-386">Stockage physique (système de fichiers ou partage réseau)</span><span class="sxs-lookup"><span data-stu-id="77bc9-386">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="77bc9-387">Pour les chargements de fichiers volumineux :</span><span class="sxs-lookup"><span data-stu-id="77bc9-387">For large file uploads:</span></span>
    * <span data-ttu-id="77bc9-388">Les limites de base de données peuvent limiter la taille du téléchargement.</span><span class="sxs-lookup"><span data-stu-id="77bc9-388">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="77bc9-389">Le stockage physique est souvent moins économique que le stockage dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-389">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="77bc9-390">Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-390">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="77bc9-391">Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-391">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="77bc9-392">**N’accordez jamais l’autorisation EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-392">**Never grant execute permission.**</span></span>

* <span data-ttu-id="77bc9-393">Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="77bc9-393">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="77bc9-394">Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.</span><span class="sxs-lookup"><span data-stu-id="77bc9-394">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="77bc9-395">Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.</span><span class="sxs-lookup"><span data-stu-id="77bc9-395">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="77bc9-396">Pour plus d’informations, consultez [démarrage rapide : utiliser .net pour créer un objet BLOB dans le stockage d’objets](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="77bc9-396">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="77bc9-397">La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisée pour enregistrer un <xref:System.IO.FileStream> dans le stockage d’objets BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="77bc9-397">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="77bc9-398">Scénarios de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="77bc9-398">File upload scenarios</span></span>

<span data-ttu-id="77bc9-399">La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="77bc9-399">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="77bc9-400">**Mise en tampon**</span><span class="sxs-lookup"><span data-stu-id="77bc9-400">**Buffering**</span></span>

<span data-ttu-id="77bc9-401">La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-401">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="77bc9-402">Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-402">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="77bc9-403">Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque.</span><span class="sxs-lookup"><span data-stu-id="77bc9-403">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="77bc9-404">Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="77bc9-404">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="77bc9-405">Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.</span><span class="sxs-lookup"><span data-stu-id="77bc9-405">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="77bc9-406">La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="77bc9-406">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="77bc9-407">Stockage physique</span><span class="sxs-lookup"><span data-stu-id="77bc9-407">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="77bc9-408">Base de données</span><span class="sxs-lookup"><span data-stu-id="77bc9-408">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="77bc9-409">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="77bc9-409">**Streaming**</span></span>

<span data-ttu-id="77bc9-410">Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-410">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="77bc9-411">La diffusion en continu n’améliore pas les performances de manière significative.</span><span class="sxs-lookup"><span data-stu-id="77bc9-411">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="77bc9-412">La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="77bc9-412">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="77bc9-413">La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-413">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="77bc9-414">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique</span><span class="sxs-lookup"><span data-stu-id="77bc9-414">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="77bc9-415">Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="77bc9-415">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="77bc9-416">L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :</span><span class="sxs-lookup"><span data-stu-id="77bc9-416">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="77bc9-417">L’exemple suivant est similaire à l’exemple précédent, à l’exception de :</span><span class="sxs-lookup"><span data-stu-id="77bc9-417">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="77bc9-418">JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.</span><span class="sxs-lookup"><span data-stu-id="77bc9-418">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="77bc9-419">Il n’y a aucune validation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-419">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="77bc9-420">Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-420">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="77bc9-421">Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="77bc9-421">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="77bc9-422">Utilisez `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-422">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="77bc9-423">Exemple :</span><span class="sxs-lookup"><span data-stu-id="77bc9-423">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="77bc9-424">Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type de codage (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-424">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="77bc9-425">Pour qu’un élément `files` Input prenne en charge le téléchargement de plusieurs fichiers, indiquez l’attribut `multiple` sur l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="77bc9-425">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="77bc9-426">Les fichiers individuels téléchargés sur le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="77bc9-426">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="77bc9-427">L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-427">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="77bc9-428">N’utilisez **pas** la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> autre que pour l’affichage et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-428">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="77bc9-429">Lors de l’affichage ou de la journalisation, le nom du fichier est encodé au format HTML.</span><span class="sxs-lookup"><span data-stu-id="77bc9-429">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="77bc9-430">Une personne malveillante peut fournir un nom de fichier malveillant, y compris les chemins d’accès complets ou les chemins d’accès relatifs.</span><span class="sxs-lookup"><span data-stu-id="77bc9-430">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="77bc9-431">Les applications doivent :</span><span class="sxs-lookup"><span data-stu-id="77bc9-431">Applications should:</span></span>
>
> * <span data-ttu-id="77bc9-432">Supprimez le chemin d’accès du nom de fichier fourni par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-432">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="77bc9-433">Enregistrez le nom de fichier encodé au format HTML, avec chemin d’accès supprimé pour l’interface utilisateur ou la journalisation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-433">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="77bc9-434">Générez un nouveau nom de fichier aléatoire pour le stockage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-434">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="77bc9-435">Le code suivant supprime le chemin d’accès du nom de fichier :</span><span class="sxs-lookup"><span data-stu-id="77bc9-435">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="77bc9-436">Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="77bc9-436">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="77bc9-437">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="77bc9-437">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="77bc9-438">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="77bc9-438">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="77bc9-439">Validation</span><span class="sxs-lookup"><span data-stu-id="77bc9-439">Validation</span></span>](#validation)

<span data-ttu-id="77bc9-440">Lors du chargement de fichiers à l’aide d’une liaison de modèle et <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :</span><span class="sxs-lookup"><span data-stu-id="77bc9-440">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="77bc9-441"><xref:Microsoft.AspNetCore.Http.IFormFile>unique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-441">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="77bc9-442">L’un des regroupements suivants qui représentent plusieurs fichiers :</span><span class="sxs-lookup"><span data-stu-id="77bc9-442">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="77bc9-443">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="77bc9-443">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="77bc9-444">La liaison correspond aux fichiers de formulaire par nom.</span><span class="sxs-lookup"><span data-stu-id="77bc9-444">Binding matches form files by name.</span></span> <span data-ttu-id="77bc9-445">Par exemple, la valeur de `name` HTML dans `<input type="file" name="formFile">` doit correspondre C# au paramètre/à la propriété lié (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="77bc9-445">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="77bc9-446">Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-446">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="77bc9-447">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="77bc9-447">The following example:</span></span>

* <span data-ttu-id="77bc9-448">Effectue une boucle sur un ou plusieurs fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-448">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="77bc9-449">Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-449">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="77bc9-450">Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-450">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="77bc9-451">Retourne le nombre total et la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-451">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="77bc9-452">Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="77bc9-452">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="77bc9-453">Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :</span><span class="sxs-lookup"><span data-stu-id="77bc9-453">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="77bc9-454">Le chemin d’accès passé au <xref:System.IO.FileStream> *doit* inclure le nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-454">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="77bc9-455">Si le nom de fichier n’est pas fourni, une <xref:System.UnauthorizedAccessException> est levée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="77bc9-455">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="77bc9-456">Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement.</span><span class="sxs-lookup"><span data-stu-id="77bc9-456">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="77bc9-457">À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="77bc9-457">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="77bc9-458">Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="77bc9-458">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="77bc9-459">Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-459">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-460">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents.</span><span class="sxs-lookup"><span data-stu-id="77bc9-460">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="77bc9-461">La limite de 65 535 fichiers est une limite par serveur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-461">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="77bc9-462">Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="77bc9-462">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="77bc9-463">GetTempFileNameA fonction)</span><span class="sxs-lookup"><span data-stu-id="77bc9-463">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="77bc9-464">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données</span><span class="sxs-lookup"><span data-stu-id="77bc9-464">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="77bc9-465">Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de groupe <xref:System.Byte> sur l’entité :</span><span class="sxs-lookup"><span data-stu-id="77bc9-465">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="77bc9-466">Spécifiez une propriété de modèle de page pour la classe qui comprend une <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="77bc9-466">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="77bc9-467"><xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="77bc9-467"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="77bc9-468">L’exemple précédent utilise une propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="77bc9-468">The prior example uses a bound model property.</span></span>

<span data-ttu-id="77bc9-469">Le `FileUpload` est utilisé dans le formulaire de Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="77bc9-469">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="77bc9-470">Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="77bc9-470">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="77bc9-471">Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :</span><span class="sxs-lookup"><span data-stu-id="77bc9-471">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="77bc9-472">L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="77bc9-472">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="77bc9-473">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="77bc9-473">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="77bc9-474">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="77bc9-474">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-475">Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.</span><span class="sxs-lookup"><span data-stu-id="77bc9-475">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="77bc9-476">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-476">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="77bc9-477">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.</span><span class="sxs-lookup"><span data-stu-id="77bc9-477">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="77bc9-478">Les exemples fournis ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="77bc9-478">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="77bc9-479">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="77bc9-479">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="77bc9-480">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="77bc9-480">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="77bc9-481">Validation</span><span class="sxs-lookup"><span data-stu-id="77bc9-481">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="77bc9-482">Charger des fichiers volumineux avec streaming</span><span class="sxs-lookup"><span data-stu-id="77bc9-482">Upload large files with streaming</span></span>

<span data-ttu-id="77bc9-483">L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-483">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="77bc9-484">Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="77bc9-484">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="77bc9-485">Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="77bc9-485">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="77bc9-486">Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié.</span><span class="sxs-lookup"><span data-stu-id="77bc9-486">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="77bc9-487">Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="77bc9-487">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="77bc9-488">La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="77bc9-488">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="77bc9-489">L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :</span><span class="sxs-lookup"><span data-stu-id="77bc9-489">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="77bc9-490">Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="77bc9-490">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="77bc9-491">Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page de `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="77bc9-491">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="77bc9-492">Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner).</span><span class="sxs-lookup"><span data-stu-id="77bc9-492">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="77bc9-493">La méthode d’action fonctionne directement avec la propriété `Request`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-493">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="77bc9-494">Un `MultipartReader` est utilisé pour lire chaque section.</span><span class="sxs-lookup"><span data-stu-id="77bc9-494">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="77bc9-495">Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-495">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="77bc9-496">Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.</span><span class="sxs-lookup"><span data-stu-id="77bc9-496">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="77bc9-497">La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :</span><span class="sxs-lookup"><span data-stu-id="77bc9-497">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="77bc9-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :</span><span class="sxs-lookup"><span data-stu-id="77bc9-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="77bc9-499">La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :</span><span class="sxs-lookup"><span data-stu-id="77bc9-499">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="77bc9-500">Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-500">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="77bc9-501">Validation</span><span class="sxs-lookup"><span data-stu-id="77bc9-501">Validation</span></span>

<span data-ttu-id="77bc9-502">La classe de `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu.</span><span class="sxs-lookup"><span data-stu-id="77bc9-502">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="77bc9-503">Pour traiter <xref:Microsoft.AspNetCore.Http.IFormFile> les chargements de fichiers mis en mémoire tampon dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="77bc9-503">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="77bc9-504">Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-504">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="77bc9-505">Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-505">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="77bc9-506">Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.</span><span class="sxs-lookup"><span data-stu-id="77bc9-506">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="77bc9-507">Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :</span><span class="sxs-lookup"><span data-stu-id="77bc9-507">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="77bc9-508">Comprenez pleinement l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="77bc9-508">Fully understand the implementation.</span></span>
> * <span data-ttu-id="77bc9-509">Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-509">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="77bc9-510">**N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-510">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="77bc9-511">Validation du contenu</span><span class="sxs-lookup"><span data-stu-id="77bc9-511">Content validation</span></span>

<span data-ttu-id="77bc9-512">**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**</span><span class="sxs-lookup"><span data-stu-id="77bc9-512">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="77bc9-513">L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé.</span><span class="sxs-lookup"><span data-stu-id="77bc9-513">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="77bc9-514">Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-514">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="77bc9-515">En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie.</span><span class="sxs-lookup"><span data-stu-id="77bc9-515">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="77bc9-516">Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal.</span><span class="sxs-lookup"><span data-stu-id="77bc9-516">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="77bc9-517">Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-517">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="77bc9-518">À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="77bc9-518">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="77bc9-519">Validation de l’extension de fichier</span><span class="sxs-lookup"><span data-stu-id="77bc9-519">File extension validation</span></span>

<span data-ttu-id="77bc9-520">L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées.</span><span class="sxs-lookup"><span data-stu-id="77bc9-520">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="77bc9-521">Exemple :</span><span class="sxs-lookup"><span data-stu-id="77bc9-521">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="77bc9-522">Validation de la signature de fichier</span><span class="sxs-lookup"><span data-stu-id="77bc9-522">File signature validation</span></span>

<span data-ttu-id="77bc9-523">La signature d’un fichier est déterminée par les premiers octets au début d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-523">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="77bc9-524">Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="77bc9-524">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="77bc9-525">L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants.</span><span class="sxs-lookup"><span data-stu-id="77bc9-525">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="77bc9-526">Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :</span><span class="sxs-lookup"><span data-stu-id="77bc9-526">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="77bc9-527">Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.</span><span class="sxs-lookup"><span data-stu-id="77bc9-527">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="77bc9-528">Sécurité des noms de fichiers</span><span class="sxs-lookup"><span data-stu-id="77bc9-528">File name security</span></span>

<span data-ttu-id="77bc9-529">N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique.</span><span class="sxs-lookup"><span data-stu-id="77bc9-529">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="77bc9-530">Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.</span><span class="sxs-lookup"><span data-stu-id="77bc9-530">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="77bc9-531">Razor code automatiquement les valeurs de propriété pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="77bc9-531">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="77bc9-532">Le code suivant peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="77bc9-532">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="77bc9-533">En dehors de Razor, <xref:System.Net.WebUtility.HtmlEncode*> toujours le contenu du nom de fichier à partir de la demande d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-533">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="77bc9-534">De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom.</span><span class="sxs-lookup"><span data-stu-id="77bc9-534">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="77bc9-535">Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.</span><span class="sxs-lookup"><span data-stu-id="77bc9-535">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="77bc9-536">Validation de la taille</span><span class="sxs-lookup"><span data-stu-id="77bc9-536">Size validation</span></span>

<span data-ttu-id="77bc9-537">Limitez la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="77bc9-537">Limit the size of uploaded files.</span></span>

<span data-ttu-id="77bc9-538">Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets).</span><span class="sxs-lookup"><span data-stu-id="77bc9-538">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="77bc9-539">La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="77bc9-539">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="77bc9-540">Le `FileSizeLimit` est injecté dans des classes `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="77bc9-540">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="77bc9-541">Quand une taille de fichier dépasse la limite, le fichier est rejeté :</span><span class="sxs-lookup"><span data-stu-id="77bc9-541">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="77bc9-542">Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication</span><span class="sxs-lookup"><span data-stu-id="77bc9-542">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="77bc9-543">Dans les formulaires non Razor qui PUBLIEnt des données de formulaire ou utilisent directement les `FormData` JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="77bc9-543">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="77bc9-544">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="77bc9-544">In the following example:</span></span>

* <span data-ttu-id="77bc9-545">Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-545">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="77bc9-546">Lorsque vous utilisez `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-546">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="77bc9-547">Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :</span><span class="sxs-lookup"><span data-stu-id="77bc9-547">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="77bc9-548">Pour une méthode de gestionnaire de page Razor Pages nommée `Upload`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-548">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="77bc9-549">Pour une méthode d’action du billet de contrôleur MVC :</span><span class="sxs-lookup"><span data-stu-id="77bc9-549">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="77bc9-550">Configuration du serveur et de l’application</span><span class="sxs-lookup"><span data-stu-id="77bc9-550">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="77bc9-551">Longueur limite du corps en plusieurs parties</span><span class="sxs-lookup"><span data-stu-id="77bc9-551">Multipart body length limit</span></span>

<span data-ttu-id="77bc9-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="77bc9-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="77bc9-553">Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lors de l’analyse.</span><span class="sxs-lookup"><span data-stu-id="77bc9-553">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="77bc9-554">La valeur par défaut est 134 217 728 (128 Mo).</span><span class="sxs-lookup"><span data-stu-id="77bc9-554">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="77bc9-555">Personnalisez la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-555">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="77bc9-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="77bc9-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="77bc9-557">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-557">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="77bc9-558">Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="77bc9-558">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="77bc9-559">Taille maximale du corps de la demande Kestrel</span><span class="sxs-lookup"><span data-stu-id="77bc9-559">Kestrel maximum request body size</span></span>

<span data-ttu-id="77bc9-560">Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="77bc9-560">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="77bc9-561">Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :</span><span class="sxs-lookup"><span data-stu-id="77bc9-561">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

<span data-ttu-id="77bc9-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="77bc9-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="77bc9-563">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="77bc9-563">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="77bc9-564">Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="77bc9-564">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="77bc9-565">Autres limites Kestrel</span><span class="sxs-lookup"><span data-stu-id="77bc9-565">Other Kestrel limits</span></span>

<span data-ttu-id="77bc9-566">D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="77bc9-566">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="77bc9-567">Nombre maximal de connexions client</span><span class="sxs-lookup"><span data-stu-id="77bc9-567">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="77bc9-568">Taux de données de demande et de réponse</span><span class="sxs-lookup"><span data-stu-id="77bc9-568">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="77bc9-569">Limite de longueur du contenu IIS</span><span class="sxs-lookup"><span data-stu-id="77bc9-569">IIS content length limit</span></span>

<span data-ttu-id="77bc9-570">La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="77bc9-570">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="77bc9-571">Personnaliser la limite dans le fichier *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="77bc9-571">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="77bc9-572">Ce paramètre s’applique seulement à IIS.</span><span class="sxs-lookup"><span data-stu-id="77bc9-572">This setting only applies to IIS.</span></span> <span data-ttu-id="77bc9-573">Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="77bc9-573">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="77bc9-574">Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="77bc9-574">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="77bc9-575">Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go.</span><span class="sxs-lookup"><span data-stu-id="77bc9-575">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="77bc9-576">Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="77bc9-576">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="77bc9-577">Résoudre les problèmes</span><span class="sxs-lookup"><span data-stu-id="77bc9-577">Troubleshoot</span></span>

<span data-ttu-id="77bc9-578">Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.</span><span class="sxs-lookup"><span data-stu-id="77bc9-578">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="77bc9-579">Erreur introuvable lors du déploiement sur un serveur IIS</span><span class="sxs-lookup"><span data-stu-id="77bc9-579">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="77bc9-580">L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :</span><span class="sxs-lookup"><span data-stu-id="77bc9-580">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="77bc9-581">Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-581">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="77bc9-582">Échec de la connexion</span><span class="sxs-lookup"><span data-stu-id="77bc9-582">Connection failure</span></span>

<span data-ttu-id="77bc9-583">Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="77bc9-583">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="77bc9-584">Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="77bc9-584">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="77bc9-585">Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.</span><span class="sxs-lookup"><span data-stu-id="77bc9-585">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="77bc9-586">Exception de référence null avec IFormFile</span><span class="sxs-lookup"><span data-stu-id="77bc9-586">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="77bc9-587">Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile> mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-587">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="77bc9-588">Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments de <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="77bc9-588">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="77bc9-589">Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="77bc9-589">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="77bc9-590">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="77bc9-590">Additional resources</span></span>

* <span data-ttu-id="77bc9-591">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="77bc9-591">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
* [<span data-ttu-id="77bc9-592">Sécurité Azure : frame de sécurité : validation des entrées | Atténuations</span><span class="sxs-lookup"><span data-stu-id="77bc9-592">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="77bc9-593">Modèles de conception de Cloud Azure : modèle de clé valet</span><span class="sxs-lookup"><span data-stu-id="77bc9-593">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
