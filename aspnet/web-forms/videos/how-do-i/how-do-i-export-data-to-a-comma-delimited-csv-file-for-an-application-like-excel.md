---
uid: web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
title: "[Comment faire] Exporter des données dans un fichier délimité (CSV) une virgule pour une Application comme Excel | Documents Microsoft"
author: rick-anderson
description: "Dans cette vidéo, Chris Pels montre comment extraire des données d’une base de données ou une autre source et l’exporter dans un fichier délimitée par des virgules qui peut être utilisé dans une application li..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/22/2009
ms.topic: article
ms.assetid: c9df86ad-aec2-43d5-bb8a-413ebb666673
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
msc.type: video
ms.openlocfilehash: ead1a76ec27a98422cc61cbd4d46d9da10c8e502
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel"></a><span data-ttu-id="da838-103">[Comment faire] Exporter des données dans un fichier délimité (CSV) une virgule pour une Application comme Excel.</span><span class="sxs-lookup"><span data-stu-id="da838-103">[How Do I:] Export Data to a Comma Delimited (CSV) File for an Application Like Excel</span></span>
====================
<span data-ttu-id="da838-104">par [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="da838-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="da838-105">Dans cette vidéo Chris Pels montre comment extraire des données d’une base de données ou une autre source et l’exporter dans un fichier délimitée par des virgules qui peut être utilisé dans une application comme Excel.</span><span class="sxs-lookup"><span data-stu-id="da838-105">In this video Chris Pels shows how to take data from a database or other source and export it to a comma delimited file that can be used in an application like Excel.</span></span> <span data-ttu-id="da838-106">Tout d’abord, un jeu de données est créé en tant qu’objet DataTable.</span><span class="sxs-lookup"><span data-stu-id="da838-106">First, a set of data is created as a DataTable object.</span></span> <span data-ttu-id="da838-107">Ensuite, la réponse à la demande de page web en cours est désactivée et l’en-tête et le type de contenu sont configurés pour être un fichier csv.</span><span class="sxs-lookup"><span data-stu-id="da838-107">Next, the Response for the current web page request is cleared and the header and content type are configured to be a csv file.</span></span> <span data-ttu-id="da838-108">Ensuite, les données réelles sont ajoutées au flux de réponse en écrivant les en-têtes de colonne du fichier csv suivi par les valeurs de données du premier.</span><span class="sxs-lookup"><span data-stu-id="da838-108">Then the actual data is added to the response stream by first writing the column headers for the csv file followed by the data values.</span></span> <span data-ttu-id="da838-109">Cette approche peut être utile lorsque les utilisateurs ont besoin d’exportation de données pour puissent être manipulé localement dans un programme comme Excel.</span><span class="sxs-lookup"><span data-stu-id="da838-109">This approach can be useful when users require an export of data so it can be manipulated locally in a program like Excel.</span></span>

[<span data-ttu-id="da838-110">&#9654; Regardez la vidéo (minutes 19)</span><span class="sxs-lookup"><span data-stu-id="da838-110">&#9654; Watch video (19 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel)
