---
title: Charger des fichiers dans ASP.NET Core
author: guardrex
description: Comment utiliser la liaison de modèle et le streaming pour charger des fichiers dans ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: de8bfee22e39dfc5a6ed254cf0555887891d4590
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179307"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="de0a5-103">Charger des fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de0a5-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="de0a5-104">Par [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)et [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="de0a5-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="de0a5-105">ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux</span><span class="sxs-lookup"><span data-stu-id="de0a5-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="de0a5-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="de0a5-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="de0a5-107">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="de0a5-107">Security considerations</span></span>

<span data-ttu-id="de0a5-108">Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="de0a5-109">Les attaquants peuvent tenter d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="de0a5-110">Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="de0a5-111">Télécharger des virus ou des programmes malveillants.</span><span class="sxs-lookup"><span data-stu-id="de0a5-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="de0a5-112">Compromettre les réseaux et les serveurs d’une autre manière.</span><span class="sxs-lookup"><span data-stu-id="de0a5-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="de0a5-113">Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="de0a5-114">Chargez les fichiers dans une zone de chargement de fichier dédiée sur le système, de préférence sur un lecteur non-système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="de0a5-115">À l’aide d’un emplacement dédié, il est plus facile d’imposer des restrictions de sécurité sur les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="de0a5-116">Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="de0a5-117">Ne conserve jamais les fichiers téléchargés dans la même arborescence de répertoires que l’application. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="de0a5-118">Utilisez un nom de fichier sécurisé déterminé par l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="de0a5-119">N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé. &dagger; Pour afficher un nom de fichier non approuvé dans une interface utilisateur ou dans un message de journalisation, encodez la valeur en HTML.</span><span class="sxs-lookup"><span data-stu-id="de0a5-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="de0a5-120">Autorise uniquement un ensemble spécifique d’extensions de fichiers approuvées. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="de0a5-121">Vérifiez la signature de format de fichier pour empêcher un utilisateur de télécharger un fichier fictif. &dagger; Par exemple, n’autorisez pas un utilisateur à télécharger un fichier *. exe* avec une extension *. txt* .</span><span class="sxs-lookup"><span data-stu-id="de0a5-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="de0a5-122">Vérifiez que les vérifications côté client sont également effectuées sur le serveur. &dagger; Les vérifications côté client sont faciles à contourner.</span><span class="sxs-lookup"><span data-stu-id="de0a5-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="de0a5-123">Vérifiez la taille d’un fichier téléchargé et empêchez les chargements supérieurs à ceux attendus. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="de0a5-124">Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="de0a5-125">**Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="de0a5-126">l’exemple d’application &dagger;The illustre une approche qui répond aux critères.</span><span class="sxs-lookup"><span data-stu-id="de0a5-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-127">Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :</span><span class="sxs-lookup"><span data-stu-id="de0a5-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="de0a5-128">Maîtrisez complètement un système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="de0a5-129">Surchargez un système avec le résultat du blocage du système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="de0a5-130">Compromettre les données utilisateur ou système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="de0a5-131">Appliquez Graffiti à une interface utilisateur publique.</span><span class="sxs-lookup"><span data-stu-id="de0a5-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="de0a5-132">Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="de0a5-133">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="de0a5-133">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * <span data-ttu-id="de0a5-134">Sécurité de la @no__t 0Azure : Vérifiez que les contrôles appropriés sont en place lors de l’acceptation des fichiers des utilisateurs @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="de0a5-134">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="de0a5-135">Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="de0a5-136">Scénarios de stockage</span><span class="sxs-lookup"><span data-stu-id="de0a5-136">Storage scenarios</span></span>

<span data-ttu-id="de0a5-137">Les options de stockage courantes pour les fichiers sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-137">Common storage options for files include:</span></span>

* <span data-ttu-id="de0a5-138">Base de données</span><span class="sxs-lookup"><span data-stu-id="de0a5-138">Database</span></span>

  * <span data-ttu-id="de0a5-139">Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).</span><span class="sxs-lookup"><span data-stu-id="de0a5-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="de0a5-140">Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).</span><span class="sxs-lookup"><span data-stu-id="de0a5-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="de0a5-141">Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="de0a5-142">Stockage physique (système de fichiers ou partage réseau)</span><span class="sxs-lookup"><span data-stu-id="de0a5-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="de0a5-143">Pour les chargements de fichiers volumineux :</span><span class="sxs-lookup"><span data-stu-id="de0a5-143">For large file uploads:</span></span>
    * <span data-ttu-id="de0a5-144">Les limites de base de données peuvent limiter la taille du téléchargement.</span><span class="sxs-lookup"><span data-stu-id="de0a5-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="de0a5-145">Le stockage physique est souvent moins économique que le stockage dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="de0a5-146">Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="de0a5-147">Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage.</span><span class="sxs-lookup"><span data-stu-id="de0a5-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="de0a5-148">**N’accordez jamais l’autorisation EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="de0a5-149">Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="de0a5-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="de0a5-150">Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.</span><span class="sxs-lookup"><span data-stu-id="de0a5-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="de0a5-151">Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.</span><span class="sxs-lookup"><span data-stu-id="de0a5-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="de0a5-152">Pour plus d’informations, consultez [Démarrage rapide : Utilisez .NET pour créer un objet BLOB dans le stockage d’objets @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="de0a5-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="de0a5-153">La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisé pour enregistrer un <xref:System.IO.FileStream> dans le stockage BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="de0a5-154">Scénarios de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="de0a5-154">File upload scenarios</span></span>

<span data-ttu-id="de0a5-155">La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="de0a5-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="de0a5-156">**Mise en tampon**</span><span class="sxs-lookup"><span data-stu-id="de0a5-156">**Buffering**</span></span>

<span data-ttu-id="de0a5-157">La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="de0a5-158">Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="de0a5-159">Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque.</span><span class="sxs-lookup"><span data-stu-id="de0a5-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="de0a5-160">Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="de0a5-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="de0a5-161">Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.</span><span class="sxs-lookup"><span data-stu-id="de0a5-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="de0a5-162">La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="de0a5-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="de0a5-163">Stockage physique</span><span class="sxs-lookup"><span data-stu-id="de0a5-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="de0a5-164">Sauvegarde de la base de données</span><span class="sxs-lookup"><span data-stu-id="de0a5-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="de0a5-165">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="de0a5-165">**Streaming**</span></span>

<span data-ttu-id="de0a5-166">Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="de0a5-167">La diffusion en continu n’améliore pas les performances de manière significative.</span><span class="sxs-lookup"><span data-stu-id="de0a5-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="de0a5-168">La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="de0a5-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="de0a5-169">La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="de0a5-170">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique</span><span class="sxs-lookup"><span data-stu-id="de0a5-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="de0a5-171">Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="de0a5-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="de0a5-172">L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :</span><span class="sxs-lookup"><span data-stu-id="de0a5-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="de0a5-173">L’exemple suivant est similaire à l’exemple précédent, à l’exception de :</span><span class="sxs-lookup"><span data-stu-id="de0a5-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="de0a5-174">JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.</span><span class="sxs-lookup"><span data-stu-id="de0a5-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="de0a5-175">Il n’y a aucune validation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-175">There's no validation.</span></span>

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

<span data-ttu-id="de0a5-176">Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="de0a5-177">Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="de0a5-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="de0a5-178">Utilisez `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="de0a5-179">Exemple :</span><span class="sxs-lookup"><span data-stu-id="de0a5-179">For example:</span></span>

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

<span data-ttu-id="de0a5-180">Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type d’encodage (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="de0a5-181">Pour qu’un élément d’entrée `files` prenne en charge le téléchargement de plusieurs fichiers, fournissez l’attribut `multiple` sur l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="de0a5-182">Les fichiers individuels téléchargés vers le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="de0a5-183">L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.</span><span class="sxs-lookup"><span data-stu-id="de0a5-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-184">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="de0a5-185">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML de la valeur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="de0a5-186">Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="de0a5-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="de0a5-187">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="de0a5-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de0a5-188">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="de0a5-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de0a5-189">Validation</span><span class="sxs-lookup"><span data-stu-id="de0a5-189">Validation</span></span>](#validation)

<span data-ttu-id="de0a5-190">Lors du chargement de fichiers à l’aide de la liaison de modèle et de <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :</span><span class="sxs-lookup"><span data-stu-id="de0a5-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="de0a5-191">Une seule <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="de0a5-192">L’un des regroupements suivants qui représentent plusieurs fichiers :</span><span class="sxs-lookup"><span data-stu-id="de0a5-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="de0a5-193">[Liste](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="de0a5-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="de0a5-194">La liaison correspond aux fichiers de formulaire par nom.</span><span class="sxs-lookup"><span data-stu-id="de0a5-194">Binding matches form files by name.</span></span> <span data-ttu-id="de0a5-195">Par exemple, la valeur HTML `name` dans `<input type="file" name="formFile">` doit correspondre au C# paramètre/à la propriété lié (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="de0a5-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="de0a5-196">Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="de0a5-197">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="de0a5-197">The following example:</span></span>

* <span data-ttu-id="de0a5-198">Effectue une boucle sur un ou plusieurs fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="de0a5-199">Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="de0a5-200">Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="de0a5-201">Retourne le nombre total et la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-201">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="de0a5-202">Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="de0a5-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="de0a5-203">Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :</span><span class="sxs-lookup"><span data-stu-id="de0a5-203">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="de0a5-204">Le chemin d’accès passé à la <xref:System.IO.FileStream> *doit* inclure le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="de0a5-205">Si le nom de fichier n’est pas fourni, un <xref:System.UnauthorizedAccessException> est levé au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="de0a5-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="de0a5-206">Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement.</span><span class="sxs-lookup"><span data-stu-id="de0a5-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="de0a5-207">À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="de0a5-208">Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="de0a5-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="de0a5-209">Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-210">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents.</span><span class="sxs-lookup"><span data-stu-id="de0a5-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="de0a5-211">La limite de 65 535 fichiers est une limite par serveur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="de0a5-212">Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="de0a5-213">GetTempFileNameA fonction)</span><span class="sxs-lookup"><span data-stu-id="de0a5-213">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="de0a5-214">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données</span><span class="sxs-lookup"><span data-stu-id="de0a5-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="de0a5-215">Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de tableau <xref:System.Byte> sur l’entité :</span><span class="sxs-lookup"><span data-stu-id="de0a5-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="de0a5-216">Spécifiez une propriété de modèle de page pour la classe qui comprend un <xref:Microsoft.AspNetCore.Http.IFormFile> :</span><span class="sxs-lookup"><span data-stu-id="de0a5-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="de0a5-217"><xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="de0a5-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="de0a5-218">L’exemple précédent utilise une propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="de0a5-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="de0a5-219">La `FileUpload` est utilisée dans le formulaire de Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="de0a5-219">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="de0a5-220">Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="de0a5-221">Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :</span><span class="sxs-lookup"><span data-stu-id="de0a5-221">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="de0a5-222">L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="de0a5-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="de0a5-223">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="de0a5-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="de0a5-224">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="de0a5-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-225">Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.</span><span class="sxs-lookup"><span data-stu-id="de0a5-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="de0a5-226">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="de0a5-227">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.</span><span class="sxs-lookup"><span data-stu-id="de0a5-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="de0a5-228">Les exemples fournis ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="de0a5-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="de0a5-229">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="de0a5-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de0a5-230">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="de0a5-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de0a5-231">Validation</span><span class="sxs-lookup"><span data-stu-id="de0a5-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="de0a5-232">Charger des fichiers volumineux avec streaming</span><span class="sxs-lookup"><span data-stu-id="de0a5-232">Upload large files with streaming</span></span>

<span data-ttu-id="de0a5-233">L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="de0a5-234">Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="de0a5-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="de0a5-235">Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="de0a5-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="de0a5-236">Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié.</span><span class="sxs-lookup"><span data-stu-id="de0a5-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="de0a5-237">Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="de0a5-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="de0a5-238">La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="de0a5-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="de0a5-239">L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :</span><span class="sxs-lookup"><span data-stu-id="de0a5-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="de0a5-240">Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="de0a5-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="de0a5-241">Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="de0a5-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="de0a5-242">Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner).</span><span class="sxs-lookup"><span data-stu-id="de0a5-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="de0a5-243">La méthode d’action fonctionne directement avec la propriété `Request`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="de0a5-244">Un `MultipartReader` est utilisé pour lire chaque section.</span><span class="sxs-lookup"><span data-stu-id="de0a5-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="de0a5-245">Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="de0a5-246">Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.</span><span class="sxs-lookup"><span data-stu-id="de0a5-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="de0a5-247">La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :</span><span class="sxs-lookup"><span data-stu-id="de0a5-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="de0a5-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :</span><span class="sxs-lookup"><span data-stu-id="de0a5-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="de0a5-249">La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :</span><span class="sxs-lookup"><span data-stu-id="de0a5-249">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="de0a5-250">Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-250">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="de0a5-251">Validation</span><span class="sxs-lookup"><span data-stu-id="de0a5-251">Validation</span></span>

<span data-ttu-id="de0a5-252">La classe `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu.</span><span class="sxs-lookup"><span data-stu-id="de0a5-252">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="de0a5-253">Pour le traitement des chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="de0a5-253">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="de0a5-254">Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-254">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-255">Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-255">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="de0a5-256">Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.</span><span class="sxs-lookup"><span data-stu-id="de0a5-256">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="de0a5-257">Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :</span><span class="sxs-lookup"><span data-stu-id="de0a5-257">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="de0a5-258">Comprenez pleinement l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-258">Fully understand the implementation.</span></span>
> * <span data-ttu-id="de0a5-259">Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-259">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="de0a5-260">**N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-260">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="de0a5-261">Validation du contenu</span><span class="sxs-lookup"><span data-stu-id="de0a5-261">Content validation</span></span>

<span data-ttu-id="de0a5-262">**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-262">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="de0a5-263">L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé.</span><span class="sxs-lookup"><span data-stu-id="de0a5-263">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="de0a5-264">Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-264">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="de0a5-265">En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie.</span><span class="sxs-lookup"><span data-stu-id="de0a5-265">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="de0a5-266">Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal.</span><span class="sxs-lookup"><span data-stu-id="de0a5-266">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="de0a5-267">Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-267">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="de0a5-268">À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="de0a5-268">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="de0a5-269">Validation de l’extension de fichier</span><span class="sxs-lookup"><span data-stu-id="de0a5-269">File extension validation</span></span>

<span data-ttu-id="de0a5-270">L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées.</span><span class="sxs-lookup"><span data-stu-id="de0a5-270">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="de0a5-271">Exemple :</span><span class="sxs-lookup"><span data-stu-id="de0a5-271">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="de0a5-272">Validation de la signature de fichier</span><span class="sxs-lookup"><span data-stu-id="de0a5-272">File signature validation</span></span>

<span data-ttu-id="de0a5-273">La signature d’un fichier est déterminée par les premiers octets au début d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-273">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="de0a5-274">Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-274">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="de0a5-275">L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants.</span><span class="sxs-lookup"><span data-stu-id="de0a5-275">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="de0a5-276">Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :</span><span class="sxs-lookup"><span data-stu-id="de0a5-276">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="de0a5-277">Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.</span><span class="sxs-lookup"><span data-stu-id="de0a5-277">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="de0a5-278">Sécurité des noms de fichiers</span><span class="sxs-lookup"><span data-stu-id="de0a5-278">File name security</span></span>

<span data-ttu-id="de0a5-279">N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique.</span><span class="sxs-lookup"><span data-stu-id="de0a5-279">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="de0a5-280">Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.</span><span class="sxs-lookup"><span data-stu-id="de0a5-280">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="de0a5-281">Razor code automatiquement les valeurs de propriété pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="de0a5-281">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="de0a5-282">Le code suivant peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="de0a5-282">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="de0a5-283">En dehors de Razor, toujours <xref:System.Net.WebUtility.HtmlEncode*> le contenu du nom de fichier à partir de la demande d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-283">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="de0a5-284">De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom.</span><span class="sxs-lookup"><span data-stu-id="de0a5-284">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="de0a5-285">Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-285">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="de0a5-286">Validation de la taille</span><span class="sxs-lookup"><span data-stu-id="de0a5-286">Size validation</span></span>

<span data-ttu-id="de0a5-287">Limitez la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-287">Limit the size of uploaded files.</span></span>

<span data-ttu-id="de0a5-288">Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets).</span><span class="sxs-lookup"><span data-stu-id="de0a5-288">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="de0a5-289">La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="de0a5-289">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="de0a5-290">La `FileSizeLimit` est injectée dans les classes `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-290">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="de0a5-291">Quand une taille de fichier dépasse la limite, le fichier est rejeté :</span><span class="sxs-lookup"><span data-stu-id="de0a5-291">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="de0a5-292">Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication</span><span class="sxs-lookup"><span data-stu-id="de0a5-292">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="de0a5-293">Dans les formulaires non-Razor qui PUBLIEnt des données de formulaire ou utilisent directement le `FormData` de JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-293">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="de0a5-294">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="de0a5-294">In the following example:</span></span>

* <span data-ttu-id="de0a5-295">Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-295">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="de0a5-296">Lors de l’utilisation de `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-296">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="de0a5-297">Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :</span><span class="sxs-lookup"><span data-stu-id="de0a5-297">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="de0a5-298">Pour une méthode de gestionnaire de page Razor Pages nommée `Upload` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-298">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="de0a5-299">Pour une méthode d’action du billet de contrôleur MVC :</span><span class="sxs-lookup"><span data-stu-id="de0a5-299">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="de0a5-300">Configuration du serveur et de l’application</span><span class="sxs-lookup"><span data-stu-id="de0a5-300">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="de0a5-301">Longueur limite du corps en plusieurs parties</span><span class="sxs-lookup"><span data-stu-id="de0a5-301">Multipart body length limit</span></span>

<span data-ttu-id="de0a5-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="de0a5-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="de0a5-303">Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lorsqu’elles sont analysées.</span><span class="sxs-lookup"><span data-stu-id="de0a5-303">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="de0a5-304">La valeur par défaut est 134 217 728 (128 Mo).</span><span class="sxs-lookup"><span data-stu-id="de0a5-304">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="de0a5-305">Personnaliser la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-305">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de0a5-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="de0a5-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="de0a5-307">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-307">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de0a5-308">Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="de0a5-308">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="de0a5-309">Taille maximale du corps de la demande Kestrel</span><span class="sxs-lookup"><span data-stu-id="de0a5-309">Kestrel maximum request body size</span></span>

<span data-ttu-id="de0a5-310">Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="de0a5-310">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="de0a5-311">Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :</span><span class="sxs-lookup"><span data-stu-id="de0a5-311">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="de0a5-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="de0a5-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="de0a5-313">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-313">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de0a5-314">Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="de0a5-314">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="de0a5-315">La `RequestSizeLimitAttribute` peut également être appliquée à l’aide de la directive Razor [@attribute](xref:mvc/views/razor#attribute) :</span><span class="sxs-lookup"><span data-stu-id="de0a5-315">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="de0a5-316">Autres limites Kestrel</span><span class="sxs-lookup"><span data-stu-id="de0a5-316">Other Kestrel limits</span></span>

<span data-ttu-id="de0a5-317">D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="de0a5-317">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="de0a5-318">Nombre maximal de connexions client</span><span class="sxs-lookup"><span data-stu-id="de0a5-318">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="de0a5-319">Taux de données de demande et de réponse</span><span class="sxs-lookup"><span data-stu-id="de0a5-319">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="de0a5-320">Limite de longueur du contenu IIS</span><span class="sxs-lookup"><span data-stu-id="de0a5-320">IIS content length limit</span></span>

<span data-ttu-id="de0a5-321">La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="de0a5-321">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="de0a5-322">Personnaliser la limite dans le fichier *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="de0a5-322">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="de0a5-323">Ce paramètre s’applique seulement à IIS.</span><span class="sxs-lookup"><span data-stu-id="de0a5-323">This setting only applies to IIS.</span></span> <span data-ttu-id="de0a5-324">Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="de0a5-324">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="de0a5-325">Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="de0a5-325">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="de0a5-326">Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go.</span><span class="sxs-lookup"><span data-stu-id="de0a5-326">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="de0a5-327">Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="de0a5-327">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="de0a5-328">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="de0a5-328">Troubleshoot</span></span>

<span data-ttu-id="de0a5-329">Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.</span><span class="sxs-lookup"><span data-stu-id="de0a5-329">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="de0a5-330">Erreur introuvable lors du déploiement sur un serveur IIS</span><span class="sxs-lookup"><span data-stu-id="de0a5-330">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="de0a5-331">L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :</span><span class="sxs-lookup"><span data-stu-id="de0a5-331">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="de0a5-332">Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-332">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="de0a5-333">Échec de la connexion</span><span class="sxs-lookup"><span data-stu-id="de0a5-333">Connection failure</span></span>

<span data-ttu-id="de0a5-334">Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="de0a5-334">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="de0a5-335">Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-335">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="de0a5-336">Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.</span><span class="sxs-lookup"><span data-stu-id="de0a5-336">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="de0a5-337">Exception de référence null avec IFormFile</span><span class="sxs-lookup"><span data-stu-id="de0a5-337">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="de0a5-338">Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>, mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-338">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="de0a5-339">Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-339">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="de0a5-340">Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="de0a5-340">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="de0a5-341">ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux</span><span class="sxs-lookup"><span data-stu-id="de0a5-341">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="de0a5-342">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="de0a5-342">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="de0a5-343">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="de0a5-343">Security considerations</span></span>

<span data-ttu-id="de0a5-344">Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-344">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="de0a5-345">Les attaquants peuvent tenter d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-345">Attackers may attempt to:</span></span>

* <span data-ttu-id="de0a5-346">Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-346">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="de0a5-347">Télécharger des virus ou des programmes malveillants.</span><span class="sxs-lookup"><span data-stu-id="de0a5-347">Upload viruses or malware.</span></span>
* <span data-ttu-id="de0a5-348">Compromettre les réseaux et les serveurs d’une autre manière.</span><span class="sxs-lookup"><span data-stu-id="de0a5-348">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="de0a5-349">Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-349">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="de0a5-350">Chargez les fichiers dans une zone de chargement de fichier dédiée sur le système, de préférence sur un lecteur non-système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-350">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="de0a5-351">À l’aide d’un emplacement dédié, il est plus facile d’imposer des restrictions de sécurité sur les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-351">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="de0a5-352">Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-352">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="de0a5-353">Ne conserve jamais les fichiers téléchargés dans la même arborescence de répertoires que l’application. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-353">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="de0a5-354">Utilisez un nom de fichier sécurisé déterminé par l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-354">Use a safe file name determined by the app.</span></span> <span data-ttu-id="de0a5-355">N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé. &dagger; Pour afficher un nom de fichier non approuvé dans une interface utilisateur ou dans un message de journalisation, encodez la valeur en HTML.</span><span class="sxs-lookup"><span data-stu-id="de0a5-355">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="de0a5-356">Autorise uniquement un ensemble spécifique d’extensions de fichiers approuvées. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-356">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="de0a5-357">Vérifiez la signature de format de fichier pour empêcher un utilisateur de télécharger un fichier fictif. &dagger; Par exemple, n’autorisez pas un utilisateur à télécharger un fichier *. exe* avec une extension *. txt* .</span><span class="sxs-lookup"><span data-stu-id="de0a5-357">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="de0a5-358">Vérifiez que les vérifications côté client sont également effectuées sur le serveur. &dagger; Les vérifications côté client sont faciles à contourner.</span><span class="sxs-lookup"><span data-stu-id="de0a5-358">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="de0a5-359">Vérifiez la taille d’un fichier téléchargé et empêchez les chargements supérieurs à ceux attendus. &dagger;</span><span class="sxs-lookup"><span data-stu-id="de0a5-359">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="de0a5-360">Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-360">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="de0a5-361">**Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-361">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="de0a5-362">l’exemple d’application &dagger;The illustre une approche qui répond aux critères.</span><span class="sxs-lookup"><span data-stu-id="de0a5-362">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-363">Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :</span><span class="sxs-lookup"><span data-stu-id="de0a5-363">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="de0a5-364">Maîtrisez complètement un système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-364">Completely gain control of a system.</span></span>
> * <span data-ttu-id="de0a5-365">Surchargez un système avec le résultat du blocage du système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-365">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="de0a5-366">Compromettre les données utilisateur ou système.</span><span class="sxs-lookup"><span data-stu-id="de0a5-366">Compromise user or system data.</span></span>
> * <span data-ttu-id="de0a5-367">Appliquez Graffiti à une interface utilisateur publique.</span><span class="sxs-lookup"><span data-stu-id="de0a5-367">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="de0a5-368">Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-368">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="de0a5-369">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="de0a5-369">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * <span data-ttu-id="de0a5-370">Sécurité de la @no__t 0Azure : Vérifiez que les contrôles appropriés sont en place lors de l’acceptation des fichiers des utilisateurs @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="de0a5-370">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="de0a5-371">Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-371">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="de0a5-372">Scénarios de stockage</span><span class="sxs-lookup"><span data-stu-id="de0a5-372">Storage scenarios</span></span>

<span data-ttu-id="de0a5-373">Les options de stockage courantes pour les fichiers sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-373">Common storage options for files include:</span></span>

* <span data-ttu-id="de0a5-374">Base de données</span><span class="sxs-lookup"><span data-stu-id="de0a5-374">Database</span></span>

  * <span data-ttu-id="de0a5-375">Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).</span><span class="sxs-lookup"><span data-stu-id="de0a5-375">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="de0a5-376">Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).</span><span class="sxs-lookup"><span data-stu-id="de0a5-376">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="de0a5-377">Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-377">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="de0a5-378">Stockage physique (système de fichiers ou partage réseau)</span><span class="sxs-lookup"><span data-stu-id="de0a5-378">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="de0a5-379">Pour les chargements de fichiers volumineux :</span><span class="sxs-lookup"><span data-stu-id="de0a5-379">For large file uploads:</span></span>
    * <span data-ttu-id="de0a5-380">Les limites de base de données peuvent limiter la taille du téléchargement.</span><span class="sxs-lookup"><span data-stu-id="de0a5-380">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="de0a5-381">Le stockage physique est souvent moins économique que le stockage dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-381">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="de0a5-382">Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-382">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="de0a5-383">Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage.</span><span class="sxs-lookup"><span data-stu-id="de0a5-383">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="de0a5-384">**N’accordez jamais l’autorisation EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-384">**Never grant execute permission.**</span></span>

* <span data-ttu-id="de0a5-385">Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="de0a5-385">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="de0a5-386">Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.</span><span class="sxs-lookup"><span data-stu-id="de0a5-386">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="de0a5-387">Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.</span><span class="sxs-lookup"><span data-stu-id="de0a5-387">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="de0a5-388">Pour plus d’informations, consultez [Démarrage rapide : Utilisez .NET pour créer un objet BLOB dans le stockage d’objets @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="de0a5-388">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="de0a5-389">La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisé pour enregistrer un <xref:System.IO.FileStream> dans le stockage BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-389">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="de0a5-390">Scénarios de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="de0a5-390">File upload scenarios</span></span>

<span data-ttu-id="de0a5-391">La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="de0a5-391">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="de0a5-392">**Mise en tampon**</span><span class="sxs-lookup"><span data-stu-id="de0a5-392">**Buffering**</span></span>

<span data-ttu-id="de0a5-393">La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-393">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="de0a5-394">Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-394">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="de0a5-395">Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque.</span><span class="sxs-lookup"><span data-stu-id="de0a5-395">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="de0a5-396">Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="de0a5-396">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="de0a5-397">Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.</span><span class="sxs-lookup"><span data-stu-id="de0a5-397">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="de0a5-398">La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="de0a5-398">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="de0a5-399">Stockage physique</span><span class="sxs-lookup"><span data-stu-id="de0a5-399">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="de0a5-400">Sauvegarde de la base de données</span><span class="sxs-lookup"><span data-stu-id="de0a5-400">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="de0a5-401">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="de0a5-401">**Streaming**</span></span>

<span data-ttu-id="de0a5-402">Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-402">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="de0a5-403">La diffusion en continu n’améliore pas les performances de manière significative.</span><span class="sxs-lookup"><span data-stu-id="de0a5-403">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="de0a5-404">La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="de0a5-404">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="de0a5-405">La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-405">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="de0a5-406">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique</span><span class="sxs-lookup"><span data-stu-id="de0a5-406">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="de0a5-407">Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="de0a5-407">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="de0a5-408">L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :</span><span class="sxs-lookup"><span data-stu-id="de0a5-408">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="de0a5-409">L’exemple suivant est similaire à l’exemple précédent, à l’exception de :</span><span class="sxs-lookup"><span data-stu-id="de0a5-409">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="de0a5-410">JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.</span><span class="sxs-lookup"><span data-stu-id="de0a5-410">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="de0a5-411">Il n’y a aucune validation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-411">There's no validation.</span></span>

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

<span data-ttu-id="de0a5-412">Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-412">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="de0a5-413">Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="de0a5-413">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="de0a5-414">Utilisez `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-414">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="de0a5-415">Exemple :</span><span class="sxs-lookup"><span data-stu-id="de0a5-415">For example:</span></span>

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

<span data-ttu-id="de0a5-416">Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type d’encodage (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-416">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="de0a5-417">Pour qu’un élément d’entrée `files` prenne en charge le téléchargement de plusieurs fichiers, fournissez l’attribut `multiple` sur l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-417">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="de0a5-418">Les fichiers individuels téléchargés vers le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-418">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="de0a5-419">L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.</span><span class="sxs-lookup"><span data-stu-id="de0a5-419">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-420">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-420">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="de0a5-421">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML de la valeur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-421">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="de0a5-422">Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="de0a5-422">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="de0a5-423">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="de0a5-423">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de0a5-424">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="de0a5-424">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de0a5-425">Validation</span><span class="sxs-lookup"><span data-stu-id="de0a5-425">Validation</span></span>](#validation)

<span data-ttu-id="de0a5-426">Lors du chargement de fichiers à l’aide de la liaison de modèle et de <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :</span><span class="sxs-lookup"><span data-stu-id="de0a5-426">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="de0a5-427">Une seule <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-427">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="de0a5-428">L’un des regroupements suivants qui représentent plusieurs fichiers :</span><span class="sxs-lookup"><span data-stu-id="de0a5-428">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="de0a5-429">[Liste](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="de0a5-429">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="de0a5-430">La liaison correspond aux fichiers de formulaire par nom.</span><span class="sxs-lookup"><span data-stu-id="de0a5-430">Binding matches form files by name.</span></span> <span data-ttu-id="de0a5-431">Par exemple, la valeur HTML `name` dans `<input type="file" name="formFile">` doit correspondre au C# paramètre/à la propriété lié (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="de0a5-431">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="de0a5-432">Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-432">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="de0a5-433">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="de0a5-433">The following example:</span></span>

* <span data-ttu-id="de0a5-434">Effectue une boucle sur un ou plusieurs fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-434">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="de0a5-435">Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-435">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="de0a5-436">Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-436">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="de0a5-437">Retourne le nombre total et la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-437">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="de0a5-438">Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="de0a5-438">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="de0a5-439">Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :</span><span class="sxs-lookup"><span data-stu-id="de0a5-439">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="de0a5-440">Le chemin d’accès passé à la <xref:System.IO.FileStream> *doit* inclure le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-440">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="de0a5-441">Si le nom de fichier n’est pas fourni, un <xref:System.UnauthorizedAccessException> est levé au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="de0a5-441">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="de0a5-442">Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement.</span><span class="sxs-lookup"><span data-stu-id="de0a5-442">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="de0a5-443">À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="de0a5-443">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="de0a5-444">Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="de0a5-444">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="de0a5-445">Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-445">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-446">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents.</span><span class="sxs-lookup"><span data-stu-id="de0a5-446">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="de0a5-447">La limite de 65 535 fichiers est une limite par serveur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-447">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="de0a5-448">Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="de0a5-448">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="de0a5-449">GetTempFileNameA fonction)</span><span class="sxs-lookup"><span data-stu-id="de0a5-449">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="de0a5-450">Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données</span><span class="sxs-lookup"><span data-stu-id="de0a5-450">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="de0a5-451">Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de tableau <xref:System.Byte> sur l’entité :</span><span class="sxs-lookup"><span data-stu-id="de0a5-451">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="de0a5-452">Spécifiez une propriété de modèle de page pour la classe qui comprend un <xref:Microsoft.AspNetCore.Http.IFormFile> :</span><span class="sxs-lookup"><span data-stu-id="de0a5-452">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="de0a5-453"><xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="de0a5-453"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="de0a5-454">L’exemple précédent utilise une propriété de modèle liée.</span><span class="sxs-lookup"><span data-stu-id="de0a5-454">The prior example uses a bound model property.</span></span>

<span data-ttu-id="de0a5-455">La `FileUpload` est utilisée dans le formulaire de Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="de0a5-455">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="de0a5-456">Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="de0a5-456">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="de0a5-457">Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :</span><span class="sxs-lookup"><span data-stu-id="de0a5-457">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="de0a5-458">L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="de0a5-458">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="de0a5-459">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="de0a5-459">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="de0a5-460">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="de0a5-460">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-461">Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.</span><span class="sxs-lookup"><span data-stu-id="de0a5-461">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="de0a5-462">N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-462">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="de0a5-463">La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.</span><span class="sxs-lookup"><span data-stu-id="de0a5-463">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="de0a5-464">Les exemples fournis ne prennent pas en compte les considérations de sécurité.</span><span class="sxs-lookup"><span data-stu-id="de0a5-464">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="de0a5-465">Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="de0a5-465">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="de0a5-466">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="de0a5-466">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="de0a5-467">Validation</span><span class="sxs-lookup"><span data-stu-id="de0a5-467">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="de0a5-468">Charger des fichiers volumineux avec streaming</span><span class="sxs-lookup"><span data-stu-id="de0a5-468">Upload large files with streaming</span></span>

<span data-ttu-id="de0a5-469">L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-469">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="de0a5-470">Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="de0a5-470">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="de0a5-471">Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="de0a5-471">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="de0a5-472">Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié.</span><span class="sxs-lookup"><span data-stu-id="de0a5-472">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="de0a5-473">Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="de0a5-473">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="de0a5-474">La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="de0a5-474">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="de0a5-475">L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :</span><span class="sxs-lookup"><span data-stu-id="de0a5-475">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="de0a5-476">Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :</span><span class="sxs-lookup"><span data-stu-id="de0a5-476">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="de0a5-477">Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="de0a5-477">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="de0a5-478">Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner).</span><span class="sxs-lookup"><span data-stu-id="de0a5-478">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="de0a5-479">La méthode d’action fonctionne directement avec la propriété `Request`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-479">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="de0a5-480">Un `MultipartReader` est utilisé pour lire chaque section.</span><span class="sxs-lookup"><span data-stu-id="de0a5-480">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="de0a5-481">Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-481">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="de0a5-482">Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.</span><span class="sxs-lookup"><span data-stu-id="de0a5-482">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="de0a5-483">La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :</span><span class="sxs-lookup"><span data-stu-id="de0a5-483">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="de0a5-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :</span><span class="sxs-lookup"><span data-stu-id="de0a5-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="de0a5-485">La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :</span><span class="sxs-lookup"><span data-stu-id="de0a5-485">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="de0a5-486">Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-486">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="de0a5-487">Validation</span><span class="sxs-lookup"><span data-stu-id="de0a5-487">Validation</span></span>

<span data-ttu-id="de0a5-488">La classe `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu.</span><span class="sxs-lookup"><span data-stu-id="de0a5-488">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="de0a5-489">Pour le traitement des chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="de0a5-489">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="de0a5-490">Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-490">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="de0a5-491">Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-491">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="de0a5-492">Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.</span><span class="sxs-lookup"><span data-stu-id="de0a5-492">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="de0a5-493">Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :</span><span class="sxs-lookup"><span data-stu-id="de0a5-493">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="de0a5-494">Comprenez pleinement l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="de0a5-494">Fully understand the implementation.</span></span>
> * <span data-ttu-id="de0a5-495">Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-495">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="de0a5-496">**N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-496">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="de0a5-497">Validation du contenu</span><span class="sxs-lookup"><span data-stu-id="de0a5-497">Content validation</span></span>

<span data-ttu-id="de0a5-498">**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**</span><span class="sxs-lookup"><span data-stu-id="de0a5-498">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="de0a5-499">L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé.</span><span class="sxs-lookup"><span data-stu-id="de0a5-499">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="de0a5-500">Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-500">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="de0a5-501">En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie.</span><span class="sxs-lookup"><span data-stu-id="de0a5-501">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="de0a5-502">Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal.</span><span class="sxs-lookup"><span data-stu-id="de0a5-502">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="de0a5-503">Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-503">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="de0a5-504">À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="de0a5-504">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="de0a5-505">Validation de l’extension de fichier</span><span class="sxs-lookup"><span data-stu-id="de0a5-505">File extension validation</span></span>

<span data-ttu-id="de0a5-506">L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées.</span><span class="sxs-lookup"><span data-stu-id="de0a5-506">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="de0a5-507">Exemple :</span><span class="sxs-lookup"><span data-stu-id="de0a5-507">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="de0a5-508">Validation de la signature de fichier</span><span class="sxs-lookup"><span data-stu-id="de0a5-508">File signature validation</span></span>

<span data-ttu-id="de0a5-509">La signature d’un fichier est déterminée par les premiers octets au début d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-509">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="de0a5-510">Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="de0a5-510">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="de0a5-511">L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants.</span><span class="sxs-lookup"><span data-stu-id="de0a5-511">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="de0a5-512">Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :</span><span class="sxs-lookup"><span data-stu-id="de0a5-512">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="de0a5-513">Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.</span><span class="sxs-lookup"><span data-stu-id="de0a5-513">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="de0a5-514">Sécurité des noms de fichiers</span><span class="sxs-lookup"><span data-stu-id="de0a5-514">File name security</span></span>

<span data-ttu-id="de0a5-515">N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique.</span><span class="sxs-lookup"><span data-stu-id="de0a5-515">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="de0a5-516">Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.</span><span class="sxs-lookup"><span data-stu-id="de0a5-516">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="de0a5-517">Razor code automatiquement les valeurs de propriété pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="de0a5-517">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="de0a5-518">Le code suivant peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="de0a5-518">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="de0a5-519">En dehors de Razor, toujours <xref:System.Net.WebUtility.HtmlEncode*> le contenu du nom de fichier à partir de la demande d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-519">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="de0a5-520">De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom.</span><span class="sxs-lookup"><span data-stu-id="de0a5-520">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="de0a5-521">Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.</span><span class="sxs-lookup"><span data-stu-id="de0a5-521">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="de0a5-522">Validation de la taille</span><span class="sxs-lookup"><span data-stu-id="de0a5-522">Size validation</span></span>

<span data-ttu-id="de0a5-523">Limitez la taille des fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="de0a5-523">Limit the size of uploaded files.</span></span>

<span data-ttu-id="de0a5-524">Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets).</span><span class="sxs-lookup"><span data-stu-id="de0a5-524">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="de0a5-525">La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="de0a5-525">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="de0a5-526">La `FileSizeLimit` est injectée dans les classes `PageModel` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-526">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="de0a5-527">Quand une taille de fichier dépasse la limite, le fichier est rejeté :</span><span class="sxs-lookup"><span data-stu-id="de0a5-527">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="de0a5-528">Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication</span><span class="sxs-lookup"><span data-stu-id="de0a5-528">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="de0a5-529">Dans les formulaires non-Razor qui PUBLIEnt des données de formulaire ou utilisent directement le `FormData` de JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="de0a5-529">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="de0a5-530">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="de0a5-530">In the following example:</span></span>

* <span data-ttu-id="de0a5-531">Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-531">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="de0a5-532">Lors de l’utilisation de `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-532">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="de0a5-533">Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :</span><span class="sxs-lookup"><span data-stu-id="de0a5-533">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="de0a5-534">Pour une méthode de gestionnaire de page Razor Pages nommée `Upload` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-534">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="de0a5-535">Pour une méthode d’action du billet de contrôleur MVC :</span><span class="sxs-lookup"><span data-stu-id="de0a5-535">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="de0a5-536">Configuration du serveur et de l’application</span><span class="sxs-lookup"><span data-stu-id="de0a5-536">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="de0a5-537">Longueur limite du corps en plusieurs parties</span><span class="sxs-lookup"><span data-stu-id="de0a5-537">Multipart body length limit</span></span>

<span data-ttu-id="de0a5-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="de0a5-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="de0a5-539">Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lorsqu’elles sont analysées.</span><span class="sxs-lookup"><span data-stu-id="de0a5-539">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="de0a5-540">La valeur par défaut est 134 217 728 (128 Mo).</span><span class="sxs-lookup"><span data-stu-id="de0a5-540">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="de0a5-541">Personnaliser la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-541">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de0a5-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="de0a5-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="de0a5-543">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-543">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de0a5-544">Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="de0a5-544">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="de0a5-545">Taille maximale du corps de la demande Kestrel</span><span class="sxs-lookup"><span data-stu-id="de0a5-545">Kestrel maximum request body size</span></span>

<span data-ttu-id="de0a5-546">Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="de0a5-546">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="de0a5-547">Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :</span><span class="sxs-lookup"><span data-stu-id="de0a5-547">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="de0a5-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.</span><span class="sxs-lookup"><span data-stu-id="de0a5-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="de0a5-549">Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="de0a5-549">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="de0a5-550">Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="de0a5-550">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="de0a5-551">Autres limites Kestrel</span><span class="sxs-lookup"><span data-stu-id="de0a5-551">Other Kestrel limits</span></span>

<span data-ttu-id="de0a5-552">D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="de0a5-552">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="de0a5-553">Nombre maximal de connexions client</span><span class="sxs-lookup"><span data-stu-id="de0a5-553">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="de0a5-554">Taux de données de demande et de réponse</span><span class="sxs-lookup"><span data-stu-id="de0a5-554">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="de0a5-555">Limite de longueur du contenu IIS</span><span class="sxs-lookup"><span data-stu-id="de0a5-555">IIS content length limit</span></span>

<span data-ttu-id="de0a5-556">La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="de0a5-556">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="de0a5-557">Personnaliser la limite dans le fichier *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="de0a5-557">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="de0a5-558">Ce paramètre s’applique seulement à IIS.</span><span class="sxs-lookup"><span data-stu-id="de0a5-558">This setting only applies to IIS.</span></span> <span data-ttu-id="de0a5-559">Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="de0a5-559">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="de0a5-560">Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="de0a5-560">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="de0a5-561">Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go.</span><span class="sxs-lookup"><span data-stu-id="de0a5-561">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="de0a5-562">Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="de0a5-562">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="de0a5-563">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="de0a5-563">Troubleshoot</span></span>

<span data-ttu-id="de0a5-564">Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.</span><span class="sxs-lookup"><span data-stu-id="de0a5-564">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="de0a5-565">Erreur introuvable lors du déploiement sur un serveur IIS</span><span class="sxs-lookup"><span data-stu-id="de0a5-565">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="de0a5-566">L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :</span><span class="sxs-lookup"><span data-stu-id="de0a5-566">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="de0a5-567">Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-567">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="de0a5-568">Échec de la connexion</span><span class="sxs-lookup"><span data-stu-id="de0a5-568">Connection failure</span></span>

<span data-ttu-id="de0a5-569">Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="de0a5-569">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="de0a5-570">Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="de0a5-570">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="de0a5-571">Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.</span><span class="sxs-lookup"><span data-stu-id="de0a5-571">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="de0a5-572">Exception de référence null avec IFormFile</span><span class="sxs-lookup"><span data-stu-id="de0a5-572">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="de0a5-573">Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>, mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-573">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="de0a5-574">Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="de0a5-574">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="de0a5-575">Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="de0a5-575">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="de0a5-576">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="de0a5-576">Additional resources</span></span>

* <span data-ttu-id="de0a5-577">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="de0a5-577">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
* <span data-ttu-id="de0a5-578">Sécurité de la @no__t 0Azure : Cadre de sécurité : Validation des entrées | Atténuations @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="de0a5-578">[Azure Security: Security Frame: Input Validation | Mitigations](/azure/security/azure-security-threat-modeling-tool-input-validation)</span></span>
* <span data-ttu-id="de0a5-579">Modèles de conception de Cloud @no__t 0Azure : Modèle de clé valet @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="de0a5-579">[Azure Cloud Design Patterns: Valet Key pattern](/azure/architecture/patterns/valet-key)</span></span>
