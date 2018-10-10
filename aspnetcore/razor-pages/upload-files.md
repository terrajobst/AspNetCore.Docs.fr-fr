---
title: Charger des fichiers dans une page Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment charger des fichiers sur une page Razor.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/11/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 92e72869967b6e3202c97b92e341ea22adc69651
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912499"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a>Charger des fichiers dans une page Razor dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Cette rubrique s’appuie sur le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) dans <xref:tutorials/razor-pages/razor-pages-start>.

Cette rubrique montre comment utiliser la liaison de modèle simple pour charger des fichiers, ce qui fonctionne bien pour charger des fichiers de petite taille. Pour plus d’informations sur le streaming de fichiers volumineux, consultez [Chargement de fichiers volumineux par streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

Dans les étapes suivantes, une fonctionnalité de chargement de fichiers de planification vidéo est ajoutée dans l’exemple d’application. Une planification vidéo est représentée par une classe `Schedule` . La classe inclut deux versions de la planification. Une version est fournie aux clients, `PublicSchedule`. L’autre version est utilisée pour les employés de la société, `PrivateSchedule`. Chaque version est chargée dans un fichier distinct. Le didacticiel décrit comment effectuer deux chargements de fichier à partir d’une page en envoyant une seule commande POST au serveur.

## <a name="security-considerations"></a>Considérations relatives à la sécurité

Vous devez être vigilant lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur. Les attaquants peuvent procéder à une attaque [par déni de service](/windows-hardware/drivers/ifs/denial-of-service) et à d’autres attaques sur un système. Vous trouverez ci-dessous certaines étapes de sécurité qui réduisent la probabilité d’une attaque réussie :

* Chargez les fichiers dans une zone de chargement de fichiers dédiée sur le système, ce qui facilite la prise de mesures de sécurité sur le contenu chargé. Lorsque vous autorisez des chargements de fichiers, assurez-vous que les autorisations d’exécution sont désactivées sur l’emplacement de chargement.
* Utilisez un nom de fichier sécurisé déterminé par l’application, et non entré par l’utilisateur, ni le nom du fichier chargé.
* Autorisez uniquement un ensemble spécifique d’extensions de fichiers approuvées.
* Vérifiez que des vérifications côté client sont effectuées sur le serveur. Les vérifications côté client sont faciles à contourner.
* Vérifiez la taille du chargement et empêchez des chargements plus volumineux que prévu.
* Exécutez une analyse antivirus/contre les programmes malveillants sur le contenu chargé.

> [!WARNING]
> Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :
> * Prendre complètement en charge un système.
> * Surcharger un système, entraînant une défaillance complète de celui-ci.
> * Compromettre les données utilisateur ou système.
> * Appliquer un graffiti sur une interface publique.

## <a name="add-a-fileupload-class"></a>Ajouter une classe FileUpload

Créez une page Razor qui gère deux chargements de fichiers. Ajoutez une classe `FileUpload` liée à la page pour obtenir les données de planification. Cliquez avec le bouton droit sur le dossier *Models*. Sélectionnez **Ajouter** > **Classe**. Nommez la classe **FileUpload** et ajoutez les propriétés suivantes :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

La classe compte une propriété pour le titre de la planification et une propriété pour chacune des deux versions de la planification. Les trois propriétés sont obligatoires, et le titre doit comprendre entre 3 et 60 caractères.

## <a name="add-a-helper-method-to-upload-files"></a>Ajouter une méthode d’assistance pour charger des fichiers

Pour éviter la duplication de code dans le traitement des fichiers de planification chargés, ajoutez d’abord une méthode d’assistance statique. Créez un dossier *Utilities* dans l’application et ajoutez un fichier *FileHelpers.cs* avec le contenu suivant. La méthode d’assistance, `ProcessFormFile`, prend un [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) et un [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary), puis retourne une chaîne contenant la taille et le contenu du fichier. Le type de contenu et la longueur sont vérifiés. Si le fichier échoue à la vérification de validation, une erreur est ajoutée dans `ModelState`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a>Enregistrer le fichier sur le disque

L’exemple d’application enregistre les fichiers chargés dans des champs de base de données. Pour enregistrer un fichier sur un disque, utilisez un [FileStream](/dotnet/api/system.io.filestream). L’exemple suivant copie un fichier détenu par `FileUpload.UploadPublicSchedule` dans le `FileStream` d’une méthode `OnPostAsync`. Le `FileStream` écrit le fichier sur le disque dans l’emplacement `<PATH-AND-FILE-NAME>` fourni :

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

Le processus Worker doit avoir des autorisations en écriture pour l’emplacement spécifié par `filePath`.

> [!NOTE]
> `filePath` *doit* inclure le nom de fichier. Si le nom de fichier n’est pas fourni, une exception [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) est levée à l’exécution.

> [!WARNING]
> Ne conservez jamais les fichiers chargés dans la même arborescence que celle de l’application.
>
> L’exemple de code ne fournit aucune protection côté serveur contre le chargement de fichiers malveillants. Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :
>
> * [Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)
> * [Sécurité Azure : Vérifier que les contrôles appropriés sont en place quand vous acceptez des fichiers d’utilisateurs](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a>Enregistrer le fichier dans le stockage Blob Azure

Pour charger le contenu du fichier dans le stockage Blob Azure, consultez [Bien démarrer avec le stockage Blob Azure à l’aide de .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs). La rubrique montre comment utiliser [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) pour enregistrer un [FileStream](/dotnet/api/system.io.filestream) dans le stockage Blob.

## <a name="add-the-schedule-class"></a>Ajouter la classe Schedule

Cliquez avec le bouton droit sur le dossier *Models*. Sélectionnez **Ajouter** > **Classe**. Nommez la classe **Schedule**, puis ajoutez les propriétés suivantes :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

La classe utilise les attributs `Display` et `DisplayFormat` qui donnent une mise en forme et des titres conviviaux quand les données de planification sont restituées.

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a>Mettre à jour RazorPagesMovieContext

Spécifiez `DbSet` dans `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) pour les planifications :

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a>Mettre à jour MovieContext

Spécifiez `DbSet` dans `MovieContext` (*Models/MovieContext.cs*) pour les planifications :

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a>Ajouter la table Schedule à la base de données

Ouvrez la console du gestionnaire de package : **Outils** > **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.

![Menu Console du Gestionnaire de package](upload-files/_static/pmc.png)

Dans la console du Gestionnaire de package, exécutez les commandes suivantes. Ces commandes ajoutent une table `Schedule` à la base de données :

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Ajouter une page Razor de chargement de fichiers

Dans le dossier *Pages*, créez un dossier *Schedules*. Dans le dossier *Schedules*, créez une page nommée *Index.cshtml* pour charger une planification avec le contenu suivant :

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

Chaque groupe de formulaires inclut un élément **\<label>** qui affiche le nom de chaque propriété de classe. Les attributs `Display` du modèle `FileUpload` fournissent les valeurs d’affichage des étiquettes. Par exemple, le nom d’affichage de la propriété `UploadPublicSchedule` est défini avec `[Display(Name="Public Schedule")]` et affiche donc « Public Schedule » sur l’étiquette, quand le formulaire est restitué.

Chaque groupe de formulaires comprend un élément **\<span>** de validation. Si l’entrée de l’utilisateur ne respecte pas les attributs de propriété définis dans la classe `FileUpload` ou si l’une des vérifications de validation de fichier de la méthode `ProcessFormFile` échoue, le modèle échoue à la validation. En cas d’échec de la validation du modèle, un message de validation utile est affiché pour l’utilisateur. Par exemple, la propriété `Title` est annotée avec `[Required]` et `[StringLength(60, MinimumLength = 3)]`. Si l’utilisateur ne parvient pas à fournir un titre, il reçoit un message indiquant qu’une valeur est obligatoire. Si l’utilisateur entre une valeur inférieure à trois caractères ou supérieure à 60 caractères, il reçoit un message indiquant que la valeur a une longueur incorrecte. Si un fichier fourni n’a pas de contenu, un message apparaît indiquant que le fichier est vide.

## <a name="add-the-page-model"></a>Ajouter le modèle de page

Ajoutez le modèle de page (*Index.cshtml.cs*) au dossier *Schedules* :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

Le modèle de page (`IndexModel` dans *Index.cshtml.cs*) relie la classe `FileUpload` :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

Le modèle utilise également la liste des planifications (`IList<Schedule>`) pour afficher les planifications stockées dans la base de données sur la page :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

Quand la page est chargée avec `OnGetAsync`, `Schedules` est rempli à partir de la base de données et permet de générer une table HTML des planifications chargées :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

Quand le formulaire est publié sur le serveur, `ModelState` est vérifié. S’il n’est pas valide, `Schedule` est regénéré et la page s’affiche avec un ou plusieurs messages de validation indiquant pourquoi la validation de la page a échoué. S’il est valide, les propriétés `FileUpload` sont utilisées dans *OnPostAsync* pour effectuer le chargement de fichier pour les deux versions de la planification et créer un objet `Schedule` afin de stocker les données. La planification est ensuite enregistrée dans la base de données :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a>Lier la page Razor de chargement de fichiers

Ouvrez *Pages/Shared/_Layout.cshtml* et ajoutez un lien vers la barre de navigation pour accéder à la page de planifications :

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Ajouter une page pour confirmer la suppression de la planification

Quand l’utilisateur clique pour supprimer une planification, il existe la possibilité d’annuler l’opération. Ajoutez une page de confirmation de suppression (*Delete.cshtml*) au dossier *Schedules* :

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

Le modèle de page (*Delete.cshtml.cs*) charge une seule planification identifiée par `id` dans les données de routage de la demande. Ajoutez le fichier *Delete.cshtml.cs* au dossier *Schedules* :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

La méthode `OnPostAsync` gère la suppression de la planification par son `id` :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

Après avoir supprimé la planification, `RedirectToPage` renvoie l’utilisateur à la page *Index.cshtml* des planifications.

## <a name="the-working-schedules-razor-page"></a>Page Razor Schedules en cours

Quand la page est chargée, les étiquettes et les entrées correspondant au titre de la planification, à la planification publique et à la planification privée sont affichées avec un bouton d’envoi :

![Page Razor Schedules lors du chargement initial, sans erreur de validation et avec des champs vides](upload-files/_static/browser1.png)

Le fait de sélectionner le bouton **Charger** sans remplir les champs constitue une violation des attributs `[Required]` sur le modèle. `ModelState` n'est pas valide. Les messages d’erreur de validation sont affichés à l’utilisateur :

![Messages d’erreur de validation apparaissant à côté de chaque contrôle d’entrée](upload-files/_static/browser2.png)

Tapez deux lettres dans le champ **Titre**. Le message de validation change pour indiquer que le titre doit comprendre entre 3 et 60 caractères :

![Message de validation du titre modifié](upload-files/_static/browser3.png)

Quand une ou plusieurs planifications sont chargées, la section **Planifications chargées** affiche les planifications chargées :

![Table des planifications chargées, affichant le titre de chaque planification, date de chargement en heure UTC, taille de fichier de la version publique et taille de fichier de la version privée](upload-files/_static/browser4.png)

L’utilisateur peut cliquer sur le lien **Supprimer** à partir d’ici pour accéder à la vue de confirmation de suppression, où il peut confirmer ou annuler l’opération de suppression.

## <a name="troubleshooting"></a>Résolution des problèmes

Pour obtenir des informations avec `IFormFile` chargement, consultez [chargements de fichiers dans ASP.NET Core : résolution des problèmes](xref:mvc/models/file-uploads#troubleshooting).
