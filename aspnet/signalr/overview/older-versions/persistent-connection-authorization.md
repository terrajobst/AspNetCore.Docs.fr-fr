---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentification et autorisation pour les connexions persistantes SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Cette rubrique décrit comment appliquer l’autorisation sur une connexion persistante. Pour des informations générales sur l’intégration de la sécurité dans une application SignalR,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 471bce03a37a401c2afe3735afef6f838555cc26
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365108"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="4bd70-104">Authentification et autorisation pour les connexions persistantes SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="4bd70-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="4bd70-105">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4bd70-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4bd70-106">Cette rubrique décrit comment appliquer l’autorisation sur une connexion persistante.</span><span class="sxs-lookup"><span data-stu-id="4bd70-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="4bd70-107">Pour obtenir des informations générales sur l’intégration de la sécurité dans une application de SignalR, consultez [Introduction à la sécurité](index.md).</span><span class="sxs-lookup"><span data-stu-id="4bd70-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="4bd70-108">Appliquer l’autorisation</span><span class="sxs-lookup"><span data-stu-id="4bd70-108">Enforce authorization</span></span>

<span data-ttu-id="4bd70-109">Pour appliquer des règles d’autorisation lorsque vous utilisez un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) vous devez substituer la `AuthorizeRequest` (méthode).</span><span class="sxs-lookup"><span data-stu-id="4bd70-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="4bd70-110">Vous ne pouvez pas utiliser le `Authorize` attribut avec des connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="4bd70-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="4bd70-111">Le `AuthorizeRequest` méthode est appelée par l’infrastructure SignalR avant chaque requête pour vérifier que l’utilisateur est autorisé à effectuer l’action demandée.</span><span class="sxs-lookup"><span data-stu-id="4bd70-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="4bd70-112">Le `AuthorizeRequest` méthode n’est pas appelée à partir du client ; au lieu de cela, vous authentifiez l’utilisateur via un mécanisme d’authentification standard de votre application.</span><span class="sxs-lookup"><span data-stu-id="4bd70-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="4bd70-113">L’exemple ci-dessous montre comment limiter les demandes aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="4bd70-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="4bd70-114">Vous pouvez ajouter une logique d’autorisation personnalisée dans la méthode AuthorizeRequest ; par exemple, la vérification si un utilisateur appartient à un rôle particulier.</span><span class="sxs-lookup"><span data-stu-id="4bd70-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
