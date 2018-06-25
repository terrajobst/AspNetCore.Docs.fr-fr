---
title: Tag Helper Image dans ASP.NET Core
author: pkellner
description: Montre comment utiliser un Tag Helper Image
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 7ed160354b25aa0183ac49db93307b1f1b4d0666
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276641"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="ca3d0-103">Tag Helper Image dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca3d0-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="ca3d0-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ca3d0-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ca3d0-105">Le Tag Helper Image améliore la balise `img` (`<img>`).</span><span class="sxs-lookup"><span data-stu-id="ca3d0-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="ca3d0-106">Il nécessite une balise `src` ainsi que l’attribut `boolean` `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="ca3d0-107">Si la source d’image (`src`) est un fichier statique sur le serveur web hôte, une chaîne de cache busting unique est ajoutée en tant que paramètre de requête à la source d’image.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="ca3d0-108">Ainsi, si le fichier sur le serveur web hôte change, une URL de requête unique qui inclut le paramètre de requête mis à jour est générée.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="ca3d0-109">La chaîne de cache busting est une valeur unique représentant le hachage du fichier image statique.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="ca3d0-110">Si la source d’image (`src`) n’est pas un fichier statique (par exemple, une URL distante ou le fichier n’existe pas sur le serveur), l’attribut `src` de la balise `<img>` est généré sans paramètre de chaîne de requête de cache busting.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="ca3d0-111">Attributs de Tag Helper Image</span><span class="sxs-lookup"><span data-stu-id="ca3d0-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="ca3d0-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="ca3d0-112">asp-append-version</span></span>

<span data-ttu-id="ca3d0-113">Quand il est spécifié avec un attribut `src`, le Tag Helper Image est appelé.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="ca3d0-114">Un exemple de Tag Helper `img` valide est :</span><span class="sxs-lookup"><span data-stu-id="ca3d0-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="ca3d0-115">Si le fichier statique existe dans le répertoire *..wwwroot/images/asplogo.png*, le code html généré est semblable au suivant (le hachage sera différent) :</span><span class="sxs-lookup"><span data-stu-id="ca3d0-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="ca3d0-116">La valeur affectée au paramètre `v` est la valeur de hachage du fichier sur le disque.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="ca3d0-117">Si le serveur web ne peut pas obtenir l’accès en lecture au fichier statique référencé, aucun paramètre `v` n’est ajouté à l’attribut `src`.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="ca3d0-118">src</span><span class="sxs-lookup"><span data-stu-id="ca3d0-118">src</span></span>

<span data-ttu-id="ca3d0-119">Pour activer le Tag Helper Image, l’attribut src est obligatoire sur l’élément `<img>`.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="ca3d0-120">Le Tag Helper Image utilise le fournisseur `Cache` sur le serveur web local pour stocker le hachage `Sha512` calculé d’un fichier donné.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="ca3d0-121">Si le fichier est à nouveau demandé, `Sha512` ne doit pas être recalculé.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="ca3d0-122">Le cache est invalidé par un observateur de fichier qui est associé au fichier quand le hachage `Sha512` du fichier est calculé.</span><span class="sxs-lookup"><span data-stu-id="ca3d0-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca3d0-123">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ca3d0-123">Additional resources</span></span>

* <xref:performance/caching/memory>
