---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Envoi de données de formulaire HTML dans l’API Web ASP.NET : MIME en plusieurs parties et le téléchargement de fichiers | Documents Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28040140"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="e9762-102">Envoi de données de formulaire HTML dans l’API Web ASP.NET : MIME en plusieurs parties et le téléchargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="e9762-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="e9762-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e9762-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="e9762-104">Partie 2 : Téléchargement de fichiers et MIME à parties multiples</span><span class="sxs-lookup"><span data-stu-id="e9762-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="e9762-105">Ce didacticiel montre comment télécharger des fichiers dans une API web.</span><span class="sxs-lookup"><span data-stu-id="e9762-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="e9762-106">Elle décrit également comment traiter les données MIME en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="e9762-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="e9762-107">[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="e9762-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="e9762-108">Voici un exemple d’un formulaire HTML pour le téléchargement d’un fichier :</span><span class="sxs-lookup"><span data-stu-id="e9762-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="e9762-109">Ce formulaire contient un contrôle d’entrée de texte et un contrôle d’entrée de fichier.</span><span class="sxs-lookup"><span data-stu-id="e9762-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="e9762-110">Lorsqu’un formulaire contient un contrôle d’entrée de fichier, le **enctype** attribut doit toujours être &quot;multipart/form-data&quot;, qui spécifie que le formulaire est envoyé en tant qu’un message MIME en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="e9762-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="e9762-111">Le format d’un message MIME à parties multiples est plus facile à comprendre en examinant un exemple de demande :</span><span class="sxs-lookup"><span data-stu-id="e9762-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="e9762-112">Ce message est divisé en deux *parties*, un pour chaque contrôle de formulaire.</span><span class="sxs-lookup"><span data-stu-id="e9762-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="e9762-113">Limites de la partie sont indiquées par les lignes qui commencent par des tirets.</span><span class="sxs-lookup"><span data-stu-id="e9762-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="e9762-114">La limite de la partie inclut un composant aléatoire (&quot;41184676334&quot;) pour vous assurer que la chaîne de limite n’apparaît pas par inadvertance à l’intérieur d’une partie de message.</span><span class="sxs-lookup"><span data-stu-id="e9762-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="e9762-115">Chaque partie de message contient un ou plusieurs en-têtes, suivies du contenu de la partie.</span><span class="sxs-lookup"><span data-stu-id="e9762-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="e9762-116">L’en-tête Content-Disposition inclut le nom du contrôle.</span><span class="sxs-lookup"><span data-stu-id="e9762-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="e9762-117">Pour les fichiers, il contient également le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="e9762-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="e9762-118">L’en-tête Content-Type décrit les données dans la partie.</span><span class="sxs-lookup"><span data-stu-id="e9762-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="e9762-119">Si cet en-tête est omis, la valeur par défaut est text/plain.</span><span class="sxs-lookup"><span data-stu-id="e9762-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="e9762-120">Dans l’exemple précédent, l’utilisateur a téléchargé un fichier nommé GrandCanyon.jpg, avec le type de contenu image/jpeg ; et la valeur du texte d’entrée a été &quot;vacances d’été&quot;.</span><span class="sxs-lookup"><span data-stu-id="e9762-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="e9762-121">Téléchargement du fichier</span><span class="sxs-lookup"><span data-stu-id="e9762-121">File Upload</span></span>

<span data-ttu-id="e9762-122">Maintenant examinons un contrôleur d’API Web qui lit des fichiers à partir d’un message MIME en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="e9762-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="e9762-123">Le contrôleur de lit les fichiers de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="e9762-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="e9762-124">API Web prend en charge les actions asynchrones à l’aide de la [modèle de programmation basé sur des tâches](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9762-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="e9762-125">Tout d’abord, voici le code si vous ciblez .NET Framework 4.5, qui prend en charge la **async** et **await** mots clés.</span><span class="sxs-lookup"><span data-stu-id="e9762-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="e9762-126">Notez que l’action du contrôleur ne prend pas de paramètres.</span><span class="sxs-lookup"><span data-stu-id="e9762-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="e9762-127">C’est parce que nous traiter le corps de la demande à l’intérieur de l’action, sans appeler un formateur de type de média.</span><span class="sxs-lookup"><span data-stu-id="e9762-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="e9762-128">Le **IsMultipartContent** méthode vérifie si la demande contient un message MIME en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="e9762-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="e9762-129">Si ce n’est pas le cas, le contrôleur retourne le code d’état HTTP 415 (Type de média non pris en charge).</span><span class="sxs-lookup"><span data-stu-id="e9762-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="e9762-130">Le **MultipartFormDataStreamProvider** classe est un objet d’assistance qui alloue des flux de fichiers pour les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="e9762-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="e9762-131">Pour lire le message MIME à parties multiples, appelez le **ReadAsMultipartAsync** (méthode).</span><span class="sxs-lookup"><span data-stu-id="e9762-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="e9762-132">Cette méthode extrait toutes les parties de message et les écrit dans les flux fournis par le **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="e9762-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="e9762-133">Lorsque la méthode est terminée, vous pouvez obtenir des informations sur les fichiers à partir de la **FileData** propriété, qui est une collection de **MultipartFileData** objets.</span><span class="sxs-lookup"><span data-stu-id="e9762-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="e9762-134">**MultipartFileData.FileName** est le nom de fichier local sur le serveur où le fichier a été enregistré.</span><span class="sxs-lookup"><span data-stu-id="e9762-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="e9762-135">**MultipartFileData.Headers** contient l’en-tête de la partie (*pas* l’en-tête de demande).</span><span class="sxs-lookup"><span data-stu-id="e9762-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="e9762-136">Vous pouvez l’utiliser pour accéder au contenu\_les en-têtes Content-Type et la destruction.</span><span class="sxs-lookup"><span data-stu-id="e9762-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="e9762-137">Comme son nom l’indique, **ReadAsMultipartAsync** est une méthode asynchrone.</span><span class="sxs-lookup"><span data-stu-id="e9762-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="e9762-138">Pour effectuer le travail une fois la méthode terminée, utilisez un [tâche de continuation](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) ou le **await** (mot clé) (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="e9762-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="e9762-139">Voici la version de .NET Framework 4.0 du code précédent :</span><span class="sxs-lookup"><span data-stu-id="e9762-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="e9762-140">Lire des données de contrôle de formulaire</span><span class="sxs-lookup"><span data-stu-id="e9762-140">Reading Form Control Data</span></span>

<span data-ttu-id="e9762-141">Le formulaire HTML qu’illustré précédemment avait un contrôle d’entrée de texte.</span><span class="sxs-lookup"><span data-stu-id="e9762-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="e9762-142">Vous pouvez obtenir la valeur du contrôle à partir de la **FormData** propriété de la **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="e9762-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="e9762-143">**FormData** est un **NameValueCollection** qui contient les paires nom/valeur pour les contrôles du formulaire.</span><span class="sxs-lookup"><span data-stu-id="e9762-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="e9762-144">La collection peut contenir des clés en double.</span><span class="sxs-lookup"><span data-stu-id="e9762-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="e9762-145">Prenez en compte ce formulaire :</span><span class="sxs-lookup"><span data-stu-id="e9762-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="e9762-146">Le corps de requête peut ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="e9762-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="e9762-147">Dans ce cas, le **FormData** collection contient les paires clé/valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e9762-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="e9762-148">voyage : aller-retour</span><span class="sxs-lookup"><span data-stu-id="e9762-148">trip: round-trip</span></span>
- <span data-ttu-id="e9762-149">options : sans interruption</span><span class="sxs-lookup"><span data-stu-id="e9762-149">options: nonstop</span></span>
- <span data-ttu-id="e9762-150">options : dates</span><span class="sxs-lookup"><span data-stu-id="e9762-150">options: dates</span></span>
- <span data-ttu-id="e9762-151">siège : fenêtre</span><span class="sxs-lookup"><span data-stu-id="e9762-151">seat: window</span></span>
