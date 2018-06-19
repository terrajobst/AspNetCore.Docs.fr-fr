---
title: Charger des fichiers dans une page Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment charger des fichiers sur une page Razor.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 5f86164b3d227e55e11244da7600394809b6a4a7
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
ms.locfileid: "32006126"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="f1c1b-103">Charger des fichiers dans une page Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1c1b-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="f1c1b-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1c1b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f1c1b-105">Dans cette section, vous allez découvrir comment charger des fichiers sur une page Razor.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="f1c1b-106">[L’exemple d’application Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) de ce didacticiel utilise une liaison de données simple pour charger des fichiers, ce qui fonctionne bien pour charger des fichiers de petite taille.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="f1c1b-107">Pour plus d’informations sur le streaming de fichiers volumineux, consultez [Chargement de fichiers volumineux par streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="f1c1b-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="f1c1b-108">Dans les étapes suivantes, une fonctionnalité de chargement de fichiers de planification vidéo est ajoutée dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="f1c1b-109">Une planification vidéo est représentée par une classe `Schedule` .</span><span class="sxs-lookup"><span data-stu-id="f1c1b-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="f1c1b-110">La classe inclut deux versions de la planification.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="f1c1b-111">Une version est fournie aux clients, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="f1c1b-112">L’autre version est utilisée pour les employés de la société, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="f1c1b-113">Chaque version est chargée dans un fichier distinct.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="f1c1b-114">Le didacticiel décrit comment effectuer deux chargements de fichier à partir d’une page en envoyant une seule commande POST au serveur.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="f1c1b-115">Considérations relatives à la sécurité</span><span class="sxs-lookup"><span data-stu-id="f1c1b-115">Security considerations</span></span>

<span data-ttu-id="f1c1b-116">Vous devez être vigilant lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="f1c1b-117">Les attaquants peuvent procéder à une attaque [par déni de service](/windows-hardware/drivers/ifs/denial-of-service) et à d’autres attaques sur un système.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="f1c1b-118">Vous trouverez ci-dessous certaines étapes de sécurité qui réduisent la probabilité d’une attaque réussie :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="f1c1b-119">Chargez les fichiers dans une zone de chargement de fichiers dédiée sur le système, ce qui facilite la prise de mesures de sécurité sur le contenu chargé.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="f1c1b-120">Lorsque vous autorisez des chargements de fichiers, assurez-vous que les autorisations d’exécution sont désactivées sur l’emplacement de chargement.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="f1c1b-121">Utilisez un nom de fichier sécurisé déterminé par l’application, et non entré par l’utilisateur, ni le nom du fichier chargé.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="f1c1b-122">Autorisez uniquement un ensemble spécifique d’extensions de fichiers approuvées.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="f1c1b-123">Vérifiez que des vérifications côté client sont effectuées sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="f1c1b-124">Les vérifications côté client sont faciles à contourner.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="f1c1b-125">Vérifiez la taille du chargement et empêchez des chargements plus volumineux que prévu.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="f1c1b-126">Exécutez une analyse antivirus/contre les programmes malveillants sur le contenu chargé.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="f1c1b-127">Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="f1c1b-128">Prendre complètement en charge un système.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="f1c1b-129">Surcharger un système, entraînant une défaillance complète de celui-ci.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="f1c1b-130">Compromettre les données utilisateur ou système.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="f1c1b-131">Appliquer un graffiti sur une interface publique.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="f1c1b-132">Ajouter une classe FileUpload</span><span class="sxs-lookup"><span data-stu-id="f1c1b-132">Add a FileUpload class</span></span>

<span data-ttu-id="f1c1b-133">Créez une page Razor qui gère deux chargements de fichiers.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="f1c1b-134">Ajoutez une classe `FileUpload` liée à la page pour obtenir les données de planification.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="f1c1b-135">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-135">Right click the *Models* folder.</span></span> <span data-ttu-id="f1c1b-136">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="f1c1b-137">Nommez la classe **FileUpload** et ajoutez les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="f1c1b-138">La classe compte une propriété pour le titre de la planification et une propriété pour chacune des deux versions de la planification.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="f1c1b-139">Les trois propriétés sont obligatoires, et le titre doit comprendre entre 3 et 60 caractères.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="f1c1b-140">Ajouter une méthode d’assistance pour charger des fichiers</span><span class="sxs-lookup"><span data-stu-id="f1c1b-140">Add a helper method to upload files</span></span>

<span data-ttu-id="f1c1b-141">Pour éviter la duplication de code dans le traitement des fichiers de planification chargés, ajoutez d’abord une méthode d’assistance statique.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="f1c1b-142">Créez un dossier *Utilities* dans l’application et ajoutez un fichier *FileHelpers.cs* avec le contenu suivant.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="f1c1b-143">La méthode d’assistance, `ProcessFormFile`, prend un [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) et un [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary), puis retourne une chaîne contenant la taille et le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="f1c1b-144">Le type de contenu et la longueur sont vérifiés.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-144">The content type and length are checked.</span></span> <span data-ttu-id="f1c1b-145">Si le fichier échoue à la vérification de validation, une erreur est ajoutée dans `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="f1c1b-146">Enregistrer le fichier sur le disque</span><span class="sxs-lookup"><span data-stu-id="f1c1b-146">Save the file to disk</span></span>

<span data-ttu-id="f1c1b-147">L’exemple d’application enregistre les fichiers chargés dans des champs de base de données.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="f1c1b-148">Pour enregistrer un fichier sur un disque, utilisez un [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="f1c1b-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="f1c1b-149">L’exemple suivant copie un fichier détenu par `FileUpload.UploadPublicSchedule` dans le `FileStream` d’une méthode `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="f1c1b-150">Le `FileStream` écrit le fichier sur le disque dans l’emplacement `<PATH-AND-FILE-NAME>` fourni :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="f1c1b-151">Le processus Worker doit avoir des autorisations en écriture pour l’emplacement spécifié par `filePath`.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="f1c1b-152">`filePath` *doit* inclure le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="f1c1b-153">Si le nom de fichier n’est pas fourni, une exception [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) est levée à l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="f1c1b-154">Ne conservez jamais les fichiers chargés dans la même arborescence que celle de l’application.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="f1c1b-155">L’exemple de code ne fournit aucune protection côté serveur contre le chargement de fichiers malveillants.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="f1c1b-156">Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="f1c1b-157">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)</span><span class="sxs-lookup"><span data-stu-id="f1c1b-157">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="f1c1b-158">Sécurité Azure : Vérifier que les contrôles appropriés sont en place quand vous acceptez des fichiers d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="f1c1b-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="f1c1b-159">Enregistrer le fichier dans le stockage Blob Azure</span><span class="sxs-lookup"><span data-stu-id="f1c1b-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="f1c1b-160">Pour charger le contenu du fichier dans le stockage Blob Azure, consultez [Bien démarrer avec le stockage Blob Azure à l’aide de .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="f1c1b-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="f1c1b-161">La rubrique montre comment utiliser [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) pour enregistrer un [FileStream](/dotnet/api/system.io.filestream) dans le stockage Blob.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="f1c1b-162">Ajouter la classe Schedule</span><span class="sxs-lookup"><span data-stu-id="f1c1b-162">Add the Schedule class</span></span>

<span data-ttu-id="f1c1b-163">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-163">Right click the *Models* folder.</span></span> <span data-ttu-id="f1c1b-164">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="f1c1b-165">Nommez la classe **Schedule**, puis ajoutez les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-165">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="f1c1b-166">La classe utilise les attributs `Display` et `DisplayFormat` qui donnent une mise en forme et des titres conviviaux quand les données de planification sont restituées.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="f1c1b-167">Mettre à jour MovieContext</span><span class="sxs-lookup"><span data-stu-id="f1c1b-167">Update the MovieContext</span></span>

<span data-ttu-id="f1c1b-168">Spécifiez `DbSet` dans `MovieContext` (*Models/MovieContext.cs*) pour les planifications :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-168">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="f1c1b-169">Ajouter la table Schedule à la base de données</span><span class="sxs-lookup"><span data-stu-id="f1c1b-169">Add the Schedule table to the database</span></span>

<span data-ttu-id="f1c1b-170">Ouvrez la console du gestionnaire de package : **Outils** > **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-170">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="f1c1b-172">Dans la console du Gestionnaire de package, exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-172">In the PMC, execute the following commands.</span></span> <span data-ttu-id="f1c1b-173">Ces commandes ajoutent une table `Schedule` à la base de données :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-173">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="f1c1b-174">Ajouter une page Razor de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="f1c1b-174">Add a file upload Razor Page</span></span>

<span data-ttu-id="f1c1b-175">Dans le dossier *Pages*, créez un dossier *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-175">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="f1c1b-176">Dans le dossier *Schedules*, créez une page nommée *Index.cshtml* pour charger une planification avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-176">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="f1c1b-177">Chaque groupe de formulaires inclut un élément **\<label>** qui affiche le nom de chaque propriété de classe.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-177">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="f1c1b-178">Les attributs `Display` du modèle `FileUpload` fournissent les valeurs d’affichage des étiquettes.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-178">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="f1c1b-179">Par exemple, le nom d’affichage de la propriété `UploadPublicSchedule` est défini avec `[Display(Name="Public Schedule")]` et affiche donc « Public Schedule » sur l’étiquette, quand le formulaire est restitué.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-179">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="f1c1b-180">Chaque groupe de formulaires comprend un élément **\<span>** de validation.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-180">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="f1c1b-181">Si l’entrée de l’utilisateur ne respecte pas les attributs de propriété définis dans la classe `FileUpload` ou si l’une des vérifications de validation de fichier de la méthode `ProcessFormFile` échoue, le modèle échoue à la validation.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-181">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="f1c1b-182">En cas d’échec de la validation du modèle, un message de validation utile est affiché pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-182">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="f1c1b-183">Par exemple, la propriété `Title` est annotée avec `[Required]` et `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-183">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="f1c1b-184">Si l’utilisateur ne parvient pas à fournir un titre, il reçoit un message indiquant qu’une valeur est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-184">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="f1c1b-185">Si l’utilisateur entre une valeur inférieure à trois caractères ou supérieure à 60 caractères, il reçoit un message indiquant que la valeur a une longueur incorrecte.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-185">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="f1c1b-186">Si un fichier fourni n’a pas de contenu, un message apparaît indiquant que le fichier est vide.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-186">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="f1c1b-187">Ajouter le modèle de page</span><span class="sxs-lookup"><span data-stu-id="f1c1b-187">Add the page model</span></span>

<span data-ttu-id="f1c1b-188">Ajoutez le modèle de page (*Index.cshtml.cs*) au dossier *Schedules* :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-188">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="f1c1b-189">Le modèle de page (`IndexModel` dans *Index.cshtml.cs*) relie la classe `FileUpload` :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-189">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="f1c1b-190">Le modèle utilise également la liste des planifications (`IList<Schedule>`) pour afficher les planifications stockées dans la base de données sur la page :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-190">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="f1c1b-191">Quand la page est chargée avec `OnGetAsync`, `Schedules` est rempli à partir de la base de données et permet de générer une table HTML des planifications chargées :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-191">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="f1c1b-192">Quand le formulaire est publié sur le serveur, `ModelState` est vérifié.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-192">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="f1c1b-193">S’il n’est pas valide, `Schedule` est regénéré et la page s’affiche avec un ou plusieurs messages de validation indiquant pourquoi la validation de la page a échoué.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-193">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="f1c1b-194">S’il est valide, les propriétés `FileUpload` sont utilisées dans *OnPostAsync* pour effectuer le chargement de fichier pour les deux versions de la planification et créer un objet `Schedule` afin de stocker les données.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-194">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="f1c1b-195">La planification est ensuite enregistrée dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-195">The schedule is then saved to the database:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="f1c1b-196">Lier la page Razor de chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="f1c1b-196">Link the file upload Razor Page</span></span>

<span data-ttu-id="f1c1b-197">Ouvrez *_Layout.cshtml* et ajoutez un lien vers la barre de navigation pour accéder à la page de chargement de fichiers :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-197">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="f1c1b-198">Ajouter une page pour confirmer la suppression de la planification</span><span class="sxs-lookup"><span data-stu-id="f1c1b-198">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="f1c1b-199">Quand l’utilisateur clique pour supprimer une planification, il existe la possibilité d’annuler l’opération.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-199">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="f1c1b-200">Ajoutez une page de confirmation de suppression (*Delete.cshtml*) au dossier *Schedules* :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-200">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="f1c1b-201">Le modèle de page (*Delete.cshtml.cs*) charge une seule planification identifiée par `id` dans les données de routage de la demande.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-201">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="f1c1b-202">Ajoutez le fichier *Delete.cshtml.cs* au dossier *Schedules* :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-202">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="f1c1b-203">La méthode `OnPostAsync` gère la suppression de la planification par son `id` :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-203">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="f1c1b-204">Après avoir supprimé la planification, `RedirectToPage` renvoie l’utilisateur à la page *Index.cshtml* des planifications.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-204">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="f1c1b-205">Page Razor Schedules en cours</span><span class="sxs-lookup"><span data-stu-id="f1c1b-205">The working Schedules Razor Page</span></span>

<span data-ttu-id="f1c1b-206">Quand la page est chargée, les étiquettes et les entrées correspondant au titre de la planification, à la planification publique et à la planification privée sont affichées avec un bouton d’envoi :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-206">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Page Razor Schedules lors du chargement initial, sans erreur de validation et avec des champs vides](uploading-files/_static/browser1.png)

<span data-ttu-id="f1c1b-208">Le fait de sélectionner le bouton **Charger** sans remplir les champs constitue une violation des attributs `[Required]` sur le modèle.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-208">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="f1c1b-209">`ModelState` n'est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-209">The `ModelState` is invalid.</span></span> <span data-ttu-id="f1c1b-210">Les messages d’erreur de validation sont affichés à l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-210">The validation error messages are displayed to the user:</span></span>

![Messages d’erreur de validation apparaissant à côté de chaque contrôle d’entrée](uploading-files/_static/browser2.png)

<span data-ttu-id="f1c1b-212">Tapez deux lettres dans le champ **Titre**.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-212">Type two letters into the **Title** field.</span></span> <span data-ttu-id="f1c1b-213">Le message de validation change pour indiquer que le titre doit comprendre entre 3 et 60 caractères :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-213">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Message de validation du titre modifié](uploading-files/_static/browser3.png)

<span data-ttu-id="f1c1b-215">Quand une ou plusieurs planifications sont chargées, la section **Planifications chargées** affiche les planifications chargées :</span><span class="sxs-lookup"><span data-stu-id="f1c1b-215">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Table des planifications chargées, affichant le titre de chaque planification, date de chargement en heure UTC, taille de fichier de la version publique et taille de fichier de la version privée](uploading-files/_static/browser4.png)

<span data-ttu-id="f1c1b-217">L’utilisateur peut cliquer sur le lien **Supprimer** à partir d’ici pour accéder à la vue de confirmation de suppression, où il peut confirmer ou annuler l’opération de suppression.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-217">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f1c1b-218">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="f1c1b-218">Troubleshooting</span></span>

<span data-ttu-id="f1c1b-219">Pour obtenir des informations sur la résolution des problèmes avec le chargement `IFormFile`, consultez [Chargements de fichier dans ASP.NET Core : Résolution des problèmes](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="f1c1b-219">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="f1c1b-220">Nous vous remercions d’avoir effectué cette introduction aux pages Razor.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-220">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="f1c1b-221">Votre avis nous intéresse.</span><span class="sxs-lookup"><span data-stu-id="f1c1b-221">We appreciate feedback.</span></span> <span data-ttu-id="f1c1b-222">Pour compléter ce tutoriel, vous pouvez consulter [Bien démarrer avec MVC et EF Core](xref:data/ef-mvc/intro).</span><span class="sxs-lookup"><span data-stu-id="f1c1b-222">[Get started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1c1b-223">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f1c1b-223">Additional resources</span></span>

* [<span data-ttu-id="f1c1b-224">Chargements de fichier dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1c1b-224">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="f1c1b-225">IFormFile</span><span class="sxs-lookup"><span data-stu-id="f1c1b-225">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

> [!div class="step-by-step"]
> [<span data-ttu-id="f1c1b-226">Précédent : Validation</span><span class="sxs-lookup"><span data-stu-id="f1c1b-226">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
