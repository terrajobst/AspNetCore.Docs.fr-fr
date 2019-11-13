---
title: Charger des fichiers dans ASP.NET Core
author: guardrex
description: Comment utiliser la liaison de modèle et le streaming pour charger des fichiers dans ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: b57ad4fe62de38085c11d7026d278cc6e0c565ce
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963158"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="065e4-103">Charger des fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="065e4-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="065e4-104">Par [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)et [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="065e4-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="065e4-105">ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux</span><span class="sxs-lookup"><span data-stu-id="065e4-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="065e4-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="065e4-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="065e4-107">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="065e4-107">Security considerations</span></span>

<span data-ttu-id="065e4-108">Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="065e4-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="065e4-109">Les attaquants peuvent tenter d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="065e4-110">Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="065e4-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="065e4-111">Télécharger des virus ou des programmes malveillants.</span><span class="sxs-lookup"><span data-stu-id="065e4-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="065e4-112">Compromettre les réseaux et les serveurs d’une autre manière.</span><span class="sxs-lookup"><span data-stu-id="065e4-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="065e4-113">Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="065e4-114">Chargez les fichiers dans une zone de chargement de fichier dédiée, de préférence sur un lecteur non-système.</span><span class="sxs-lookup"><span data-stu-id="065e4-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="065e4-115">Un emplacement dédié permet d’imposer des restrictions de sécurité plus faciles sur les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="065e4-116">Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="065e4-117">Ne conservez **pas** les fichiers téléchargés dans la même arborescence de répertoires que l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="065e4-118">Utilisez un nom de fichier sécurisé déterminé par l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="065e4-119">N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé.&dagger; code HTML encode le nom de fichier non fiable lors de son affichage.</span><span class="sxs-lookup"><span data-stu-id="065e4-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="065e4-120">Par exemple, la journalisation du nom de fichier ou l’affichage de l’interface utilisateur (Razor génère automatiquement le code HTML).</span><span class="sxs-lookup"><span data-stu-id="065e4-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="065e4-121">Autorisez uniquement les extensions de fichier approuvées pour la spécification de conception de l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="065e4-122">Vérifiez que les vérifications côté client sont effectuées sur le serveur.&dagger; contrôles côté client sont faciles à contourner.</span><span class="sxs-lookup"><span data-stu-id="065e4-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="065e4-123">Vérifiez la taille d’un fichier téléchargé.</span><span class="sxs-lookup"><span data-stu-id="065e4-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="065e4-124">Définissez une limite de taille maximale pour empêcher les chargements volumineux.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="065e4-125">Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="065e4-126">**Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**</span><span class="sxs-lookup"><span data-stu-id="065e4-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="065e4-127">&dagger;l’exemple d’application illustre une approche qui répond aux critères.</span><span class="sxs-lookup"><span data-stu-id="065e4-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-128">Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :</span><span class="sxs-lookup"><span data-stu-id="065e4-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="065e4-129">Maîtrisez complètement un système.</span><span class="sxs-lookup"><span data-stu-id="065e4-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="065e4-130">Surchargez un système avec le résultat du blocage du système.</span><span class="sxs-lookup"><span data-stu-id="065e4-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="065e4-131">Compromettre les données utilisateur ou système.</span><span class="sxs-lookup"><span data-stu-id="065e4-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="065e4-132">Appliquez Graffiti à une interface utilisateur publique.</span><span class="sxs-lookup"><span data-stu-id="065e4-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="065e4-133">Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="065e4-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="065e4-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="065e4-135">Sécurité Azure : Vérifier que les contrôles appropriés sont en place quand vous acceptez des fichiers d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="065e4-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="065e4-136">Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .</span><span class="sxs-lookup"><span data-stu-id="065e4-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="065e4-137">Scénarios de stockage</span><span class="sxs-lookup"><span data-stu-id="065e4-137">Storage scenarios</span></span>

<span data-ttu-id="065e4-138">Les options de stockage courantes pour les fichiers sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-138">Common storage options for files include:</span></span>

* <span data-ttu-id="065e4-139">Base de données</span><span class="sxs-lookup"><span data-stu-id="065e4-139">Database</span></span>

  * <span data-ttu-id="065e4-140">Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).</span><span class="sxs-lookup"><span data-stu-id="065e4-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="065e4-141">Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).</span><span class="sxs-lookup"><span data-stu-id="065e4-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="065e4-142">Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="065e4-143">Stockage physique (système de fichiers ou partage réseau)</span><span class="sxs-lookup"><span data-stu-id="065e4-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="065e4-144">Pour les chargements de fichiers volumineux :</span><span class="sxs-lookup"><span data-stu-id="065e4-144">For large file uploads:</span></span>
    * <span data-ttu-id="065e4-145">Les limites de base de données peuvent limiter la taille du téléchargement.</span><span class="sxs-lookup"><span data-stu-id="065e4-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="065e4-146">Le stockage physique est souvent moins économique que le stockage dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="065e4-147">Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="065e4-148">Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage.</span><span class="sxs-lookup"><span data-stu-id="065e4-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="065e4-149">**N’accordez jamais l’autorisation EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="065e4-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="065e4-150">Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="065e4-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="065e4-151">Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.</span><span class="sxs-lookup"><span data-stu-id="065e4-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="065e4-152">Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.</span><span class="sxs-lookup"><span data-stu-id="065e4-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="065e4-153">Pour plus d’informations, consultez [démarrage rapide : utiliser .net pour créer un objet BLOB dans le stockage d’objets](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="065e4-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="065e4-154">La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisée pour enregistrer un <xref:System.IO.FileStream> dans le stockage d’objets BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="065e4-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="065e4-155">Scénarios de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="065e4-155">File upload scenarios</span></span>

<span data-ttu-id="065e4-156">La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="065e4-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="065e4-157">**Mise en tampon**</span><span class="sxs-lookup"><span data-stu-id="065e4-157">**Buffering**</span></span>

<span data-ttu-id="065e4-158">La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="065e4-159">Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés.</span><span class="sxs-lookup"><span data-stu-id="065e4-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="065e4-160">Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque.</span><span class="sxs-lookup"><span data-stu-id="065e4-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="065e4-161">Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="065e4-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="065e4-162">Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.</span><span class="sxs-lookup"><span data-stu-id="065e4-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="065e4-163">La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="065e4-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="065e4-164">Stockage physique</span><span class="sxs-lookup"><span data-stu-id="065e4-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="065e4-165">Base de données</span><span class="sxs-lookup"><span data-stu-id="065e4-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="065e4-166">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="065e4-166">**Streaming**</span></span>

<span data-ttu-id="065e4-167">Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="065e4-168">La diffusion en continu n’améliore pas les performances de manière significative.</span><span class="sxs-lookup"><span data-stu-id="065e4-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="065e4-169">La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="065e4-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="065e4-170">La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="065e4-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="065e4-171">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique</span><span class="sxs-lookup"><span data-stu-id="065e4-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="065e4-172">Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="065e4-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="065e4-173">L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :</span><span class="sxs-lookup"><span data-stu-id="065e4-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="065e4-174">L’exemple suivant est similaire à l’exemple précédent, à l’exception de :</span><span class="sxs-lookup"><span data-stu-id="065e4-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="065e4-175">JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.</span><span class="sxs-lookup"><span data-stu-id="065e4-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="065e4-176">Il n’y a aucune validation.</span><span class="sxs-lookup"><span data-stu-id="065e4-176">There's no validation.</span></span>

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

<span data-ttu-id="065e4-177">Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="065e4-178">Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="065e4-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="065e4-179">Utilisez `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="065e4-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="065e4-180">Exemple :</span><span class="sxs-lookup"><span data-stu-id="065e4-180">For example:</span></span>

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

<span data-ttu-id="065e4-181">Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type de codage (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="065e4-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="065e4-182">Pour qu’un élément `files` Input prenne en charge le téléchargement de plusieurs fichiers, indiquez l’attribut `multiple` sur l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="065e4-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="065e4-183">Les fichiers individuels téléchargés sur le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="065e4-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="065e4-184">L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.</span><span class="sxs-lookup"><span data-stu-id="065e4-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="065e4-185">N’utilisez **pas** la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> autre que pour l’affichage et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="065e4-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="065e4-186">Lors de l’affichage ou de la journalisation, le nom du fichier est encodé au format HTML.</span><span class="sxs-lookup"><span data-stu-id="065e4-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="065e4-187">Une personne malveillante peut fournir un nom de fichier malveillant, y compris les chemins d’accès complets ou les chemins d’accès relatifs.</span><span class="sxs-lookup"><span data-stu-id="065e4-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="065e4-188">Les applications doivent :</span><span class="sxs-lookup"><span data-stu-id="065e4-188">Applications should:</span></span>
>
> * <span data-ttu-id="065e4-189">Supprimez le chemin d’accès du nom de fichier fourni par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="065e4-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="065e4-190">Enregistrez le nom de fichier encodé au format HTML, avec chemin d’accès supprimé pour l’interface utilisateur ou la journalisation.</span><span class="sxs-lookup"><span data-stu-id="065e4-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="065e4-191">Générez un nouveau nom de fichier aléatoire pour le stockage.</span><span class="sxs-lookup"><span data-stu-id="065e4-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="065e4-192">Le code suivant supprime le chemin d’accès du nom de fichier :</span><span class="sxs-lookup"><span data-stu-id="065e4-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="065e4-193">Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="065e4-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="065e4-194">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="065e4-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="065e4-195">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="065e4-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="065e4-196">Validation</span><span class="sxs-lookup"><span data-stu-id="065e4-196">Validation</span></span>](#validation)

<span data-ttu-id="065e4-197">Lors du chargement de fichiers à l’aide d’une liaison de modèle et <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :</span><span class="sxs-lookup"><span data-stu-id="065e4-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="065e4-198"><xref:Microsoft.AspNetCore.Http.IFormFile>unique.</span><span class="sxs-lookup"><span data-stu-id="065e4-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="065e4-199">L’un des regroupements suivants qui représentent plusieurs fichiers :</span><span class="sxs-lookup"><span data-stu-id="065e4-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="065e4-200">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="065e4-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="065e4-201">La liaison correspond aux fichiers de formulaire par nom.</span><span class="sxs-lookup"><span data-stu-id="065e4-201">Binding matches form files by name.</span></span> <span data-ttu-id="065e4-202">Par exemple, la valeur de `name` HTML dans `<input type="file" name="formFile">` doit correspondre C# au paramètre/à la propriété lié (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="065e4-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="065e4-203">Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="065e4-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="065e4-204">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="065e4-204">The following example:</span></span>

* <span data-ttu-id="065e4-205">Effectue une boucle sur un ou plusieurs fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="065e4-206">Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="065e4-207">Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="065e4-208">Retourne le nombre total et la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-208">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="065e4-209">Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="065e4-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="065e4-210">Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :</span><span class="sxs-lookup"><span data-stu-id="065e4-210">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="065e4-211">Le chemin d’accès passé au <xref:System.IO.FileStream> *doit* inclure le nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="065e4-212">Si le nom de fichier n’est pas fourni, une <xref:System.UnauthorizedAccessException> est levée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="065e4-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="065e4-213">Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement.</span><span class="sxs-lookup"><span data-stu-id="065e4-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="065e4-214">À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="065e4-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="065e4-215">Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="065e4-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="065e4-216">Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-217">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents.</span><span class="sxs-lookup"><span data-stu-id="065e4-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="065e4-218">La limite de 65 535 fichiers est une limite par serveur.</span><span class="sxs-lookup"><span data-stu-id="065e4-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="065e4-219">Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="065e4-220">GetTempFileNameA fonction)</span><span class="sxs-lookup"><span data-stu-id="065e4-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="065e4-221">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données</span><span class="sxs-lookup"><span data-stu-id="065e4-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="065e4-222">Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de groupe <xref:System.Byte> sur l’entité :</span><span class="sxs-lookup"><span data-stu-id="065e4-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="065e4-223">Spécifiez une propriété de modèle de page pour la classe qui comprend une <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="065e4-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="065e4-224"><xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="065e4-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="065e4-225">L’exemple précédent utilise une propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="065e4-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="065e4-226">Le `FileUpload` est utilisé dans le formulaire de Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="065e4-226">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="065e4-227">Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="065e4-228">Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :</span><span class="sxs-lookup"><span data-stu-id="065e4-228">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="065e4-229">L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="065e4-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="065e4-230">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="065e4-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="065e4-231">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="065e4-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-232">Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.</span><span class="sxs-lookup"><span data-stu-id="065e4-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="065e4-233">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="065e4-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="065e4-234">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.</span><span class="sxs-lookup"><span data-stu-id="065e4-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="065e4-235">Les exemples fournis ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="065e4-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="065e4-236">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="065e4-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="065e4-237">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="065e4-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="065e4-238">Validation</span><span class="sxs-lookup"><span data-stu-id="065e4-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="065e4-239">Charger des fichiers volumineux avec streaming</span><span class="sxs-lookup"><span data-stu-id="065e4-239">Upload large files with streaming</span></span>

<span data-ttu-id="065e4-240">L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="065e4-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="065e4-241">Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="065e4-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="065e4-242">Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="065e4-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="065e4-243">Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié.</span><span class="sxs-lookup"><span data-stu-id="065e4-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="065e4-244">Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="065e4-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="065e4-245">La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="065e4-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="065e4-246">L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :</span><span class="sxs-lookup"><span data-stu-id="065e4-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="065e4-247">Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="065e4-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="065e4-248">Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page de `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="065e4-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="065e4-249">Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner).</span><span class="sxs-lookup"><span data-stu-id="065e4-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="065e4-250">La méthode d’action fonctionne directement avec la propriété `Request`.</span><span class="sxs-lookup"><span data-stu-id="065e4-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="065e4-251">Un `MultipartReader` est utilisé pour lire chaque section.</span><span class="sxs-lookup"><span data-stu-id="065e4-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="065e4-252">Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="065e4-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="065e4-253">Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.</span><span class="sxs-lookup"><span data-stu-id="065e4-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="065e4-254">La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :</span><span class="sxs-lookup"><span data-stu-id="065e4-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="065e4-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :</span><span class="sxs-lookup"><span data-stu-id="065e4-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="065e4-256">La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :</span><span class="sxs-lookup"><span data-stu-id="065e4-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="065e4-257">Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="065e4-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="065e4-258">Validation</span><span class="sxs-lookup"><span data-stu-id="065e4-258">Validation</span></span>

<span data-ttu-id="065e4-259">La classe de `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu.</span><span class="sxs-lookup"><span data-stu-id="065e4-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="065e4-260">Pour traiter <xref:Microsoft.AspNetCore.Http.IFormFile> les chargements de fichiers mis en mémoire tampon dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="065e4-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="065e4-261">Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-262">Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="065e4-263">Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.</span><span class="sxs-lookup"><span data-stu-id="065e4-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="065e4-264">Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :</span><span class="sxs-lookup"><span data-stu-id="065e4-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="065e4-265">Comprenez pleinement l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="065e4-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="065e4-266">Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="065e4-267">**N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**</span><span class="sxs-lookup"><span data-stu-id="065e4-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="065e4-268">Validation du contenu</span><span class="sxs-lookup"><span data-stu-id="065e4-268">Content validation</span></span>

<span data-ttu-id="065e4-269">**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**</span><span class="sxs-lookup"><span data-stu-id="065e4-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="065e4-270">L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé.</span><span class="sxs-lookup"><span data-stu-id="065e4-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="065e4-271">Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="065e4-272">En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie.</span><span class="sxs-lookup"><span data-stu-id="065e4-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="065e4-273">Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal.</span><span class="sxs-lookup"><span data-stu-id="065e4-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="065e4-274">Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="065e4-275">À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="065e4-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="065e4-276">Validation de l’extension de fichier</span><span class="sxs-lookup"><span data-stu-id="065e4-276">File extension validation</span></span>

<span data-ttu-id="065e4-277">L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées.</span><span class="sxs-lookup"><span data-stu-id="065e4-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="065e4-278">Exemple :</span><span class="sxs-lookup"><span data-stu-id="065e4-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="065e4-279">Validation de la signature de fichier</span><span class="sxs-lookup"><span data-stu-id="065e4-279">File signature validation</span></span>

<span data-ttu-id="065e4-280">La signature d’un fichier est déterminée par les premiers octets au début d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="065e4-281">Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="065e4-282">L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants.</span><span class="sxs-lookup"><span data-stu-id="065e4-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="065e4-283">Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :</span><span class="sxs-lookup"><span data-stu-id="065e4-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="065e4-284">Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.</span><span class="sxs-lookup"><span data-stu-id="065e4-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="065e4-285">Sécurité des noms de fichiers</span><span class="sxs-lookup"><span data-stu-id="065e4-285">File name security</span></span>

<span data-ttu-id="065e4-286">N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique.</span><span class="sxs-lookup"><span data-stu-id="065e4-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="065e4-287">Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.</span><span class="sxs-lookup"><span data-stu-id="065e4-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="065e4-288">Razor code automatiquement les valeurs de propriété pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="065e4-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="065e4-289">Le code suivant peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="065e4-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="065e4-290">En dehors de Razor, <xref:System.Net.WebUtility.HtmlEncode*> toujours le contenu du nom de fichier à partir de la demande d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="065e4-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="065e4-291">De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom.</span><span class="sxs-lookup"><span data-stu-id="065e4-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="065e4-292">Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.</span><span class="sxs-lookup"><span data-stu-id="065e4-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="065e4-293">Validation de la taille</span><span class="sxs-lookup"><span data-stu-id="065e4-293">Size validation</span></span>

<span data-ttu-id="065e4-294">Limitez la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="065e4-295">Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets).</span><span class="sxs-lookup"><span data-stu-id="065e4-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="065e4-296">La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="065e4-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="065e4-297">Le `FileSizeLimit` est injecté dans des classes `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="065e4-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="065e4-298">Quand une taille de fichier dépasse la limite, le fichier est rejeté :</span><span class="sxs-lookup"><span data-stu-id="065e4-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="065e4-299">Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication</span><span class="sxs-lookup"><span data-stu-id="065e4-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="065e4-300">Dans les formulaires non Razor qui PUBLIEnt des données de formulaire ou utilisent directement les `FormData` JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="065e4-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="065e4-301">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="065e4-301">In the following example:</span></span>

* <span data-ttu-id="065e4-302">Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="065e4-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="065e4-303">Lorsque vous utilisez `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="065e4-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="065e4-304">Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :</span><span class="sxs-lookup"><span data-stu-id="065e4-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="065e4-305">Pour une méthode de gestionnaire de page Razor Pages nommée `Upload`:</span><span class="sxs-lookup"><span data-stu-id="065e4-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="065e4-306">Pour une méthode d’action du billet de contrôleur MVC :</span><span class="sxs-lookup"><span data-stu-id="065e4-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="065e4-307">Configuration du serveur et de l’application</span><span class="sxs-lookup"><span data-stu-id="065e4-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="065e4-308">Longueur limite du corps en plusieurs parties</span><span class="sxs-lookup"><span data-stu-id="065e4-308">Multipart body length limit</span></span>

<span data-ttu-id="065e4-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="065e4-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="065e4-310">Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lors de l’analyse.</span><span class="sxs-lookup"><span data-stu-id="065e4-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="065e4-311">La valeur par défaut est 134 217 728 (128 Mo).</span><span class="sxs-lookup"><span data-stu-id="065e4-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="065e4-312">Personnalisez la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="065e4-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="065e4-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="065e4-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="065e4-314">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="065e4-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="065e4-315">Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="065e4-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="065e4-316">Taille maximale du corps de la demande Kestrel</span><span class="sxs-lookup"><span data-stu-id="065e4-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="065e4-317">Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="065e4-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="065e4-318">Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :</span><span class="sxs-lookup"><span data-stu-id="065e4-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="065e4-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="065e4-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="065e4-320">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="065e4-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="065e4-321">Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="065e4-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="065e4-322">La `RequestSizeLimitAttribute` peut également être appliquée à l’aide de la directive Razor [@attribute](xref:mvc/views/razor#attribute) :</span><span class="sxs-lookup"><span data-stu-id="065e4-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="065e4-323">Autres limites Kestrel</span><span class="sxs-lookup"><span data-stu-id="065e4-323">Other Kestrel limits</span></span>

<span data-ttu-id="065e4-324">D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="065e4-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="065e4-325">Nombre maximal de connexions client</span><span class="sxs-lookup"><span data-stu-id="065e4-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="065e4-326">Taux de données de demande et de réponse</span><span class="sxs-lookup"><span data-stu-id="065e4-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="065e4-327">Limite de longueur du contenu IIS</span><span class="sxs-lookup"><span data-stu-id="065e4-327">IIS content length limit</span></span>

<span data-ttu-id="065e4-328">La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="065e4-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="065e4-329">Personnaliser la limite dans le fichier *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="065e4-329">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="065e4-330">Ce paramètre s’applique seulement à IIS.</span><span class="sxs-lookup"><span data-stu-id="065e4-330">This setting only applies to IIS.</span></span> <span data-ttu-id="065e4-331">Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="065e4-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="065e4-332">Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="065e4-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="065e4-333">Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go.</span><span class="sxs-lookup"><span data-stu-id="065e4-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="065e4-334">Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="065e4-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="065e4-335">Résoudre les problèmes</span><span class="sxs-lookup"><span data-stu-id="065e4-335">Troubleshoot</span></span>

<span data-ttu-id="065e4-336">Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.</span><span class="sxs-lookup"><span data-stu-id="065e4-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="065e4-337">Erreur introuvable lors du déploiement sur un serveur IIS</span><span class="sxs-lookup"><span data-stu-id="065e4-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="065e4-338">L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :</span><span class="sxs-lookup"><span data-stu-id="065e4-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="065e4-339">Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="065e4-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="065e4-340">Échec de la connexion</span><span class="sxs-lookup"><span data-stu-id="065e4-340">Connection failure</span></span>

<span data-ttu-id="065e4-341">Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="065e4-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="065e4-342">Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="065e4-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="065e4-343">Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.</span><span class="sxs-lookup"><span data-stu-id="065e4-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="065e4-344">Exception de référence null avec IFormFile</span><span class="sxs-lookup"><span data-stu-id="065e4-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="065e4-345">Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile> mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="065e4-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="065e4-346">Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments de <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="065e4-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="065e4-347">Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="065e4-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="065e4-348">Le flux est trop long</span><span class="sxs-lookup"><span data-stu-id="065e4-348">Stream was too long</span></span>

<span data-ttu-id="065e4-349">Les exemples de cette rubrique reposent sur <xref:System.IO.MemoryStream> pour stocker le contenu du fichier chargé.</span><span class="sxs-lookup"><span data-stu-id="065e4-349">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="065e4-350">La limite de taille d’un `MemoryStream` est `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="065e4-350">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="065e4-351">Si le scénario de chargement de fichiers de l’application nécessite le maintien d’un contenu de fichier supérieur à 50 Mo, utilisez une autre approche qui ne repose pas sur une seule `MemoryStream` pour conserver le contenu d’un fichier chargé.</span><span class="sxs-lookup"><span data-stu-id="065e4-351">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="065e4-352">ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux</span><span class="sxs-lookup"><span data-stu-id="065e4-352">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="065e4-353">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="065e4-353">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="065e4-354">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="065e4-354">Security considerations</span></span>

<span data-ttu-id="065e4-355">Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="065e4-355">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="065e4-356">Les attaquants peuvent tenter d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-356">Attackers may attempt to:</span></span>

* <span data-ttu-id="065e4-357">Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="065e4-357">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="065e4-358">Télécharger des virus ou des programmes malveillants.</span><span class="sxs-lookup"><span data-stu-id="065e4-358">Upload viruses or malware.</span></span>
* <span data-ttu-id="065e4-359">Compromettre les réseaux et les serveurs d’une autre manière.</span><span class="sxs-lookup"><span data-stu-id="065e4-359">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="065e4-360">Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-360">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="065e4-361">Chargez les fichiers dans une zone de chargement de fichier dédiée, de préférence sur un lecteur non-système.</span><span class="sxs-lookup"><span data-stu-id="065e4-361">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="065e4-362">Un emplacement dédié permet d’imposer des restrictions de sécurité plus faciles sur les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-362">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="065e4-363">Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-363">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="065e4-364">Ne conservez **pas** les fichiers téléchargés dans la même arborescence de répertoires que l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-364">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="065e4-365">Utilisez un nom de fichier sécurisé déterminé par l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-365">Use a safe file name determined by the app.</span></span> <span data-ttu-id="065e4-366">N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé.&dagger; code HTML encode le nom de fichier non fiable lors de son affichage.</span><span class="sxs-lookup"><span data-stu-id="065e4-366">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="065e4-367">Par exemple, la journalisation du nom de fichier ou l’affichage de l’interface utilisateur (Razor génère automatiquement le code HTML).</span><span class="sxs-lookup"><span data-stu-id="065e4-367">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="065e4-368">Autorisez uniquement les extensions de fichier approuvées pour la spécification de conception de l’application.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-368">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="065e4-369">Vérifiez que les vérifications côté client sont effectuées sur le serveur.&dagger; contrôles côté client sont faciles à contourner.</span><span class="sxs-lookup"><span data-stu-id="065e4-369">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="065e4-370">Vérifiez la taille d’un fichier téléchargé.</span><span class="sxs-lookup"><span data-stu-id="065e4-370">Check the size of an uploaded file.</span></span> <span data-ttu-id="065e4-371">Définissez une limite de taille maximale pour empêcher les chargements volumineux.&dagger;</span><span class="sxs-lookup"><span data-stu-id="065e4-371">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="065e4-372">Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-372">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="065e4-373">**Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**</span><span class="sxs-lookup"><span data-stu-id="065e4-373">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="065e4-374">&dagger;l’exemple d’application illustre une approche qui répond aux critères.</span><span class="sxs-lookup"><span data-stu-id="065e4-374">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-375">Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :</span><span class="sxs-lookup"><span data-stu-id="065e4-375">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="065e4-376">Maîtrisez complètement un système.</span><span class="sxs-lookup"><span data-stu-id="065e4-376">Completely gain control of a system.</span></span>
> * <span data-ttu-id="065e4-377">Surchargez un système avec le résultat du blocage du système.</span><span class="sxs-lookup"><span data-stu-id="065e4-377">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="065e4-378">Compromettre les données utilisateur ou système.</span><span class="sxs-lookup"><span data-stu-id="065e4-378">Compromise user or system data.</span></span>
> * <span data-ttu-id="065e4-379">Appliquez Graffiti à une interface utilisateur publique.</span><span class="sxs-lookup"><span data-stu-id="065e4-379">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="065e4-380">Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-380">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="065e4-381">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="065e4-381">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="065e4-382">Sécurité Azure : Vérifier que les contrôles appropriés sont en place quand vous acceptez des fichiers d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="065e4-382">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="065e4-383">Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .</span><span class="sxs-lookup"><span data-stu-id="065e4-383">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="065e4-384">Scénarios de stockage</span><span class="sxs-lookup"><span data-stu-id="065e4-384">Storage scenarios</span></span>

<span data-ttu-id="065e4-385">Les options de stockage courantes pour les fichiers sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-385">Common storage options for files include:</span></span>

* <span data-ttu-id="065e4-386">Base de données</span><span class="sxs-lookup"><span data-stu-id="065e4-386">Database</span></span>

  * <span data-ttu-id="065e4-387">Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).</span><span class="sxs-lookup"><span data-stu-id="065e4-387">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="065e4-388">Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).</span><span class="sxs-lookup"><span data-stu-id="065e4-388">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="065e4-389">Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-389">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="065e4-390">Stockage physique (système de fichiers ou partage réseau)</span><span class="sxs-lookup"><span data-stu-id="065e4-390">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="065e4-391">Pour les chargements de fichiers volumineux :</span><span class="sxs-lookup"><span data-stu-id="065e4-391">For large file uploads:</span></span>
    * <span data-ttu-id="065e4-392">Les limites de base de données peuvent limiter la taille du téléchargement.</span><span class="sxs-lookup"><span data-stu-id="065e4-392">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="065e4-393">Le stockage physique est souvent moins économique que le stockage dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-393">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="065e4-394">Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-394">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="065e4-395">Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage.</span><span class="sxs-lookup"><span data-stu-id="065e4-395">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="065e4-396">**N’accordez jamais l’autorisation EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="065e4-396">**Never grant execute permission.**</span></span>

* <span data-ttu-id="065e4-397">Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="065e4-397">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="065e4-398">Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.</span><span class="sxs-lookup"><span data-stu-id="065e4-398">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="065e4-399">Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.</span><span class="sxs-lookup"><span data-stu-id="065e4-399">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="065e4-400">Pour plus d’informations, consultez [démarrage rapide : utiliser .net pour créer un objet BLOB dans le stockage d’objets](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="065e4-400">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="065e4-401">La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisée pour enregistrer un <xref:System.IO.FileStream> dans le stockage d’objets BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="065e4-401">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="065e4-402">Scénarios de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="065e4-402">File upload scenarios</span></span>

<span data-ttu-id="065e4-403">La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="065e4-403">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="065e4-404">**Mise en tampon**</span><span class="sxs-lookup"><span data-stu-id="065e4-404">**Buffering**</span></span>

<span data-ttu-id="065e4-405">La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-405">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="065e4-406">Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés.</span><span class="sxs-lookup"><span data-stu-id="065e4-406">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="065e4-407">Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque.</span><span class="sxs-lookup"><span data-stu-id="065e4-407">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="065e4-408">Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="065e4-408">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="065e4-409">Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.</span><span class="sxs-lookup"><span data-stu-id="065e4-409">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="065e4-410">La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="065e4-410">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="065e4-411">Stockage physique</span><span class="sxs-lookup"><span data-stu-id="065e4-411">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="065e4-412">Base de données</span><span class="sxs-lookup"><span data-stu-id="065e4-412">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="065e4-413">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="065e4-413">**Streaming**</span></span>

<span data-ttu-id="065e4-414">Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-414">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="065e4-415">La diffusion en continu n’améliore pas les performances de manière significative.</span><span class="sxs-lookup"><span data-stu-id="065e4-415">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="065e4-416">La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="065e4-416">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="065e4-417">La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="065e4-417">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="065e4-418">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique</span><span class="sxs-lookup"><span data-stu-id="065e4-418">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="065e4-419">Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="065e4-419">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="065e4-420">L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :</span><span class="sxs-lookup"><span data-stu-id="065e4-420">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="065e4-421">L’exemple suivant est similaire à l’exemple précédent, à l’exception de :</span><span class="sxs-lookup"><span data-stu-id="065e4-421">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="065e4-422">JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.</span><span class="sxs-lookup"><span data-stu-id="065e4-422">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="065e4-423">Il n’y a aucune validation.</span><span class="sxs-lookup"><span data-stu-id="065e4-423">There's no validation.</span></span>

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

<span data-ttu-id="065e4-424">Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-424">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="065e4-425">Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="065e4-425">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="065e4-426">Utilisez `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="065e4-426">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="065e4-427">Exemple :</span><span class="sxs-lookup"><span data-stu-id="065e4-427">For example:</span></span>

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

<span data-ttu-id="065e4-428">Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type de codage (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="065e4-428">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="065e4-429">Pour qu’un élément `files` Input prenne en charge le téléchargement de plusieurs fichiers, indiquez l’attribut `multiple` sur l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="065e4-429">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="065e4-430">Les fichiers individuels téléchargés sur le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="065e4-430">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="065e4-431">L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.</span><span class="sxs-lookup"><span data-stu-id="065e4-431">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="065e4-432">N’utilisez **pas** la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> autre que pour l’affichage et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="065e4-432">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="065e4-433">Lors de l’affichage ou de la journalisation, le nom du fichier est encodé au format HTML.</span><span class="sxs-lookup"><span data-stu-id="065e4-433">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="065e4-434">Une personne malveillante peut fournir un nom de fichier malveillant, y compris les chemins d’accès complets ou les chemins d’accès relatifs.</span><span class="sxs-lookup"><span data-stu-id="065e4-434">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="065e4-435">Les applications doivent :</span><span class="sxs-lookup"><span data-stu-id="065e4-435">Applications should:</span></span>
>
> * <span data-ttu-id="065e4-436">Supprimez le chemin d’accès du nom de fichier fourni par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="065e4-436">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="065e4-437">Enregistrez le nom de fichier encodé au format HTML, avec chemin d’accès supprimé pour l’interface utilisateur ou la journalisation.</span><span class="sxs-lookup"><span data-stu-id="065e4-437">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="065e4-438">Générez un nouveau nom de fichier aléatoire pour le stockage.</span><span class="sxs-lookup"><span data-stu-id="065e4-438">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="065e4-439">Le code suivant supprime le chemin d’accès du nom de fichier :</span><span class="sxs-lookup"><span data-stu-id="065e4-439">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="065e4-440">Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="065e4-440">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="065e4-441">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="065e4-441">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="065e4-442">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="065e4-442">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="065e4-443">Validation</span><span class="sxs-lookup"><span data-stu-id="065e4-443">Validation</span></span>](#validation)

<span data-ttu-id="065e4-444">Lors du chargement de fichiers à l’aide d’une liaison de modèle et <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :</span><span class="sxs-lookup"><span data-stu-id="065e4-444">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="065e4-445"><xref:Microsoft.AspNetCore.Http.IFormFile>unique.</span><span class="sxs-lookup"><span data-stu-id="065e4-445">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="065e4-446">L’un des regroupements suivants qui représentent plusieurs fichiers :</span><span class="sxs-lookup"><span data-stu-id="065e4-446">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="065e4-447">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="065e4-447">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="065e4-448">La liaison correspond aux fichiers de formulaire par nom.</span><span class="sxs-lookup"><span data-stu-id="065e4-448">Binding matches form files by name.</span></span> <span data-ttu-id="065e4-449">Par exemple, la valeur de `name` HTML dans `<input type="file" name="formFile">` doit correspondre C# au paramètre/à la propriété lié (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="065e4-449">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="065e4-450">Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="065e4-450">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="065e4-451">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="065e4-451">The following example:</span></span>

* <span data-ttu-id="065e4-452">Effectue une boucle sur un ou plusieurs fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-452">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="065e4-453">Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-453">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="065e4-454">Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-454">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="065e4-455">Retourne le nombre total et la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-455">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="065e4-456">Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="065e4-456">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="065e4-457">Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :</span><span class="sxs-lookup"><span data-stu-id="065e4-457">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="065e4-458">Le chemin d’accès passé au <xref:System.IO.FileStream> *doit* inclure le nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-458">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="065e4-459">Si le nom de fichier n’est pas fourni, une <xref:System.UnauthorizedAccessException> est levée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="065e4-459">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="065e4-460">Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement.</span><span class="sxs-lookup"><span data-stu-id="065e4-460">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="065e4-461">À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="065e4-461">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="065e4-462">Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="065e4-462">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="065e4-463">Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-463">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-464">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents.</span><span class="sxs-lookup"><span data-stu-id="065e4-464">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="065e4-465">La limite de 65 535 fichiers est une limite par serveur.</span><span class="sxs-lookup"><span data-stu-id="065e4-465">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="065e4-466">Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="065e4-466">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="065e4-467">GetTempFileNameA fonction)</span><span class="sxs-lookup"><span data-stu-id="065e4-467">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="065e4-468">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données</span><span class="sxs-lookup"><span data-stu-id="065e4-468">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="065e4-469">Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de groupe <xref:System.Byte> sur l’entité :</span><span class="sxs-lookup"><span data-stu-id="065e4-469">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="065e4-470">Spécifiez une propriété de modèle de page pour la classe qui comprend une <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="065e4-470">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="065e4-471"><xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="065e4-471"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="065e4-472">L’exemple précédent utilise une propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="065e4-472">The prior example uses a bound model property.</span></span>

<span data-ttu-id="065e4-473">Le `FileUpload` est utilisé dans le formulaire de Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="065e4-473">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="065e4-474">Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="065e4-474">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="065e4-475">Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :</span><span class="sxs-lookup"><span data-stu-id="065e4-475">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="065e4-476">L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="065e4-476">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="065e4-477">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="065e4-477">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="065e4-478">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="065e4-478">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-479">Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.</span><span class="sxs-lookup"><span data-stu-id="065e4-479">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="065e4-480">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="065e4-480">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="065e4-481">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.</span><span class="sxs-lookup"><span data-stu-id="065e4-481">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="065e4-482">Les exemples fournis ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="065e4-482">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="065e4-483">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="065e4-483">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="065e4-484">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="065e4-484">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="065e4-485">Validation</span><span class="sxs-lookup"><span data-stu-id="065e4-485">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="065e4-486">Charger des fichiers volumineux avec streaming</span><span class="sxs-lookup"><span data-stu-id="065e4-486">Upload large files with streaming</span></span>

<span data-ttu-id="065e4-487">L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="065e4-487">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="065e4-488">Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="065e4-488">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="065e4-489">Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="065e4-489">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="065e4-490">Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié.</span><span class="sxs-lookup"><span data-stu-id="065e4-490">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="065e4-491">Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="065e4-491">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="065e4-492">La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="065e4-492">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="065e4-493">L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :</span><span class="sxs-lookup"><span data-stu-id="065e4-493">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="065e4-494">Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="065e4-494">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="065e4-495">Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page de `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="065e4-495">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="065e4-496">Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner).</span><span class="sxs-lookup"><span data-stu-id="065e4-496">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="065e4-497">La méthode d’action fonctionne directement avec la propriété `Request`.</span><span class="sxs-lookup"><span data-stu-id="065e4-497">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="065e4-498">Un `MultipartReader` est utilisé pour lire chaque section.</span><span class="sxs-lookup"><span data-stu-id="065e4-498">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="065e4-499">Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="065e4-499">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="065e4-500">Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.</span><span class="sxs-lookup"><span data-stu-id="065e4-500">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="065e4-501">La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :</span><span class="sxs-lookup"><span data-stu-id="065e4-501">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="065e4-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :</span><span class="sxs-lookup"><span data-stu-id="065e4-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="065e4-503">La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :</span><span class="sxs-lookup"><span data-stu-id="065e4-503">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="065e4-504">Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="065e4-504">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="065e4-505">Validation</span><span class="sxs-lookup"><span data-stu-id="065e4-505">Validation</span></span>

<span data-ttu-id="065e4-506">La classe de `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu.</span><span class="sxs-lookup"><span data-stu-id="065e4-506">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="065e4-507">Pour traiter <xref:Microsoft.AspNetCore.Http.IFormFile> les chargements de fichiers mis en mémoire tampon dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="065e4-507">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="065e4-508">Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-508">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="065e4-509">Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-509">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="065e4-510">Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.</span><span class="sxs-lookup"><span data-stu-id="065e4-510">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="065e4-511">Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :</span><span class="sxs-lookup"><span data-stu-id="065e4-511">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="065e4-512">Comprenez pleinement l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="065e4-512">Fully understand the implementation.</span></span>
> * <span data-ttu-id="065e4-513">Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-513">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="065e4-514">**N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**</span><span class="sxs-lookup"><span data-stu-id="065e4-514">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="065e4-515">Validation du contenu</span><span class="sxs-lookup"><span data-stu-id="065e4-515">Content validation</span></span>

<span data-ttu-id="065e4-516">**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**</span><span class="sxs-lookup"><span data-stu-id="065e4-516">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="065e4-517">L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé.</span><span class="sxs-lookup"><span data-stu-id="065e4-517">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="065e4-518">Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="065e4-518">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="065e4-519">En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie.</span><span class="sxs-lookup"><span data-stu-id="065e4-519">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="065e4-520">Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal.</span><span class="sxs-lookup"><span data-stu-id="065e4-520">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="065e4-521">Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-521">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="065e4-522">À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="065e4-522">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="065e4-523">Validation de l’extension de fichier</span><span class="sxs-lookup"><span data-stu-id="065e4-523">File extension validation</span></span>

<span data-ttu-id="065e4-524">L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées.</span><span class="sxs-lookup"><span data-stu-id="065e4-524">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="065e4-525">Exemple :</span><span class="sxs-lookup"><span data-stu-id="065e4-525">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="065e4-526">Validation de la signature de fichier</span><span class="sxs-lookup"><span data-stu-id="065e4-526">File signature validation</span></span>

<span data-ttu-id="065e4-527">La signature d’un fichier est déterminée par les premiers octets au début d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-527">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="065e4-528">Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="065e4-528">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="065e4-529">L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants.</span><span class="sxs-lookup"><span data-stu-id="065e4-529">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="065e4-530">Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :</span><span class="sxs-lookup"><span data-stu-id="065e4-530">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="065e4-531">Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.</span><span class="sxs-lookup"><span data-stu-id="065e4-531">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="065e4-532">Sécurité des noms de fichiers</span><span class="sxs-lookup"><span data-stu-id="065e4-532">File name security</span></span>

<span data-ttu-id="065e4-533">N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique.</span><span class="sxs-lookup"><span data-stu-id="065e4-533">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="065e4-534">Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.</span><span class="sxs-lookup"><span data-stu-id="065e4-534">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="065e4-535">Razor code automatiquement les valeurs de propriété pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="065e4-535">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="065e4-536">Le code suivant peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="065e4-536">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="065e4-537">En dehors de Razor, <xref:System.Net.WebUtility.HtmlEncode*> toujours le contenu du nom de fichier à partir de la demande d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="065e4-537">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="065e4-538">De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom.</span><span class="sxs-lookup"><span data-stu-id="065e4-538">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="065e4-539">Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.</span><span class="sxs-lookup"><span data-stu-id="065e4-539">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="065e4-540">Validation de la taille</span><span class="sxs-lookup"><span data-stu-id="065e4-540">Size validation</span></span>

<span data-ttu-id="065e4-541">Limitez la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="065e4-541">Limit the size of uploaded files.</span></span>

<span data-ttu-id="065e4-542">Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets).</span><span class="sxs-lookup"><span data-stu-id="065e4-542">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="065e4-543">La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="065e4-543">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="065e4-544">Le `FileSizeLimit` est injecté dans des classes `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="065e4-544">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="065e4-545">Quand une taille de fichier dépasse la limite, le fichier est rejeté :</span><span class="sxs-lookup"><span data-stu-id="065e4-545">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="065e4-546">Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication</span><span class="sxs-lookup"><span data-stu-id="065e4-546">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="065e4-547">Dans les formulaires non Razor qui PUBLIEnt des données de formulaire ou utilisent directement les `FormData` JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="065e4-547">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="065e4-548">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="065e4-548">In the following example:</span></span>

* <span data-ttu-id="065e4-549">Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="065e4-549">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="065e4-550">Lorsque vous utilisez `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="065e4-550">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="065e4-551">Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :</span><span class="sxs-lookup"><span data-stu-id="065e4-551">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="065e4-552">Pour une méthode de gestionnaire de page Razor Pages nommée `Upload`:</span><span class="sxs-lookup"><span data-stu-id="065e4-552">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="065e4-553">Pour une méthode d’action du billet de contrôleur MVC :</span><span class="sxs-lookup"><span data-stu-id="065e4-553">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="065e4-554">Configuration du serveur et de l’application</span><span class="sxs-lookup"><span data-stu-id="065e4-554">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="065e4-555">Longueur limite du corps en plusieurs parties</span><span class="sxs-lookup"><span data-stu-id="065e4-555">Multipart body length limit</span></span>

<span data-ttu-id="065e4-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="065e4-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="065e4-557">Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lors de l’analyse.</span><span class="sxs-lookup"><span data-stu-id="065e4-557">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="065e4-558">La valeur par défaut est 134 217 728 (128 Mo).</span><span class="sxs-lookup"><span data-stu-id="065e4-558">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="065e4-559">Personnalisez la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="065e4-559">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="065e4-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="065e4-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="065e4-561">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="065e4-561">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="065e4-562">Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="065e4-562">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="065e4-563">Taille maximale du corps de la demande Kestrel</span><span class="sxs-lookup"><span data-stu-id="065e4-563">Kestrel maximum request body size</span></span>

<span data-ttu-id="065e4-564">Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="065e4-564">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="065e4-565">Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :</span><span class="sxs-lookup"><span data-stu-id="065e4-565">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="065e4-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="065e4-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="065e4-567">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="065e4-567">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="065e4-568">Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="065e4-568">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="065e4-569">Autres limites Kestrel</span><span class="sxs-lookup"><span data-stu-id="065e4-569">Other Kestrel limits</span></span>

<span data-ttu-id="065e4-570">D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="065e4-570">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="065e4-571">Nombre maximal de connexions client</span><span class="sxs-lookup"><span data-stu-id="065e4-571">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="065e4-572">Taux de données de demande et de réponse</span><span class="sxs-lookup"><span data-stu-id="065e4-572">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="065e4-573">Limite de longueur du contenu IIS</span><span class="sxs-lookup"><span data-stu-id="065e4-573">IIS content length limit</span></span>

<span data-ttu-id="065e4-574">La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="065e4-574">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="065e4-575">Personnaliser la limite dans le fichier *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="065e4-575">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="065e4-576">Ce paramètre s’applique seulement à IIS.</span><span class="sxs-lookup"><span data-stu-id="065e4-576">This setting only applies to IIS.</span></span> <span data-ttu-id="065e4-577">Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="065e4-577">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="065e4-578">Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="065e4-578">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="065e4-579">Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go.</span><span class="sxs-lookup"><span data-stu-id="065e4-579">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="065e4-580">Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="065e4-580">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="065e4-581">Résoudre les problèmes</span><span class="sxs-lookup"><span data-stu-id="065e4-581">Troubleshoot</span></span>

<span data-ttu-id="065e4-582">Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.</span><span class="sxs-lookup"><span data-stu-id="065e4-582">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="065e4-583">Erreur introuvable lors du déploiement sur un serveur IIS</span><span class="sxs-lookup"><span data-stu-id="065e4-583">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="065e4-584">L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :</span><span class="sxs-lookup"><span data-stu-id="065e4-584">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="065e4-585">Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="065e4-585">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="065e4-586">Échec de la connexion</span><span class="sxs-lookup"><span data-stu-id="065e4-586">Connection failure</span></span>

<span data-ttu-id="065e4-587">Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="065e4-587">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="065e4-588">Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="065e4-588">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="065e4-589">Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.</span><span class="sxs-lookup"><span data-stu-id="065e4-589">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="065e4-590">Exception de référence null avec IFormFile</span><span class="sxs-lookup"><span data-stu-id="065e4-590">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="065e4-591">Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile> mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="065e4-591">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="065e4-592">Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments de <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="065e4-592">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="065e4-593">Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="065e4-593">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="065e4-594">Le flux est trop long</span><span class="sxs-lookup"><span data-stu-id="065e4-594">Stream was too long</span></span>

<span data-ttu-id="065e4-595">Les exemples de cette rubrique reposent sur <xref:System.IO.MemoryStream> pour stocker le contenu du fichier chargé.</span><span class="sxs-lookup"><span data-stu-id="065e4-595">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="065e4-596">La limite de taille d’un `MemoryStream` est `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="065e4-596">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="065e4-597">Si le scénario de chargement de fichiers de l’application nécessite le maintien d’un contenu de fichier supérieur à 50 Mo, utilisez une autre approche qui ne repose pas sur une seule `MemoryStream` pour conserver le contenu d’un fichier chargé.</span><span class="sxs-lookup"><span data-stu-id="065e4-597">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="065e4-598">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="065e4-598">Additional resources</span></span>

* <span data-ttu-id="065e4-599">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="065e4-599">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
* [<span data-ttu-id="065e4-600">Sécurité Azure : frame de sécurité : validation des entrées | Atténuations</span><span class="sxs-lookup"><span data-stu-id="065e4-600">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="065e4-601">Modèles de conception de Cloud Azure : modèle de clé valet</span><span class="sxs-lookup"><span data-stu-id="065e4-601">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
