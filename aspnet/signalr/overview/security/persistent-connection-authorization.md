---
uid: signalr/overview/security/persistent-connection-authorization
title: Authentification et autorisation pour les connexions persistantes SignalR | Microsoft Docs
author: pfletcher
description: Cette rubrique décrit comment appliquer l’autorisation sur une connexion persistante. Pour des informations générales sur l’intégration de la sécurité dans une application SignalR,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d15ac6ec8b3bab041a13918a3577310c62e66b8f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372224"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="fe4ee-104">Authentification et autorisation pour les connexions persistantes SignalR</span><span class="sxs-lookup"><span data-stu-id="fe4ee-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="fe4ee-105">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fe4ee-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fe4ee-106">Cette rubrique décrit comment appliquer l’autorisation sur une connexion persistante.</span><span class="sxs-lookup"><span data-stu-id="fe4ee-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="fe4ee-107">Pour obtenir des informations générales sur l’intégration de la sécurité dans une application de SignalR, consultez [Introduction à la sécurité](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="fe4ee-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="fe4ee-108">Versions des logiciels utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="fe4ee-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="fe4ee-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="fe4ee-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="fe4ee-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="fe4ee-110">.NET 4.5</span></span>
> - <span data-ttu-id="fe4ee-111">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="fe4ee-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="fe4ee-112">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="fe4ee-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="fe4ee-113">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe4ee-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="fe4ee-114">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="fe4ee-114">Questions and comments</span></span>
> 
> <span data-ttu-id="fe4ee-115">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="fe4ee-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="fe4ee-116">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="fe4ee-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="fe4ee-117">Appliquer l’autorisation</span><span class="sxs-lookup"><span data-stu-id="fe4ee-117">Enforce authorization</span></span>

<span data-ttu-id="fe4ee-118">Pour appliquer des règles d’autorisation lorsque vous utilisez un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) vous devez substituer la `AuthorizeRequest` (méthode).</span><span class="sxs-lookup"><span data-stu-id="fe4ee-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="fe4ee-119">Vous ne pouvez pas utiliser le `Authorize` attribut avec des connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="fe4ee-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="fe4ee-120">Le `AuthorizeRequest` méthode est appelée par l’infrastructure SignalR avant chaque requête pour vérifier que l’utilisateur est autorisé à effectuer l’action demandée.</span><span class="sxs-lookup"><span data-stu-id="fe4ee-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="fe4ee-121">Le `AuthorizeRequest` méthode n’est pas appelée à partir du client ; au lieu de cela, vous authentifiez l’utilisateur via un mécanisme d’authentification standard de votre application.</span><span class="sxs-lookup"><span data-stu-id="fe4ee-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="fe4ee-122">L’exemple ci-dessous montre comment limiter les demandes aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="fe4ee-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="fe4ee-123">Vous pouvez ajouter une logique d’autorisation personnalisée dans la méthode AuthorizeRequest ; par exemple, la vérification si un utilisateur appartient à un rôle particulier.</span><span class="sxs-lookup"><span data-stu-id="fe4ee-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
