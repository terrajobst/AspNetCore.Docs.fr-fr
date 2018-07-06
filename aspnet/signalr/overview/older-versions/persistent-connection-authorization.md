---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentification et autorisation pour les connexions persistantes SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Cette rubrique décrit comment appliquer l’autorisation sur une connexion persistante. Pour des informations générales sur l’intégration de la sécurité dans une application SignalR,...
ms.author: aspnetcontent
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 28984518346ef7e79c976a565dae5e5ab924b678
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805325"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="c99df-104">Authentification et autorisation pour les connexions persistantes SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="c99df-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="c99df-105">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c99df-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c99df-106">Cette rubrique décrit comment appliquer l’autorisation sur une connexion persistante.</span><span class="sxs-lookup"><span data-stu-id="c99df-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="c99df-107">Pour obtenir des informations générales sur l’intégration de la sécurité dans une application de SignalR, consultez [Introduction à la sécurité](index.md).</span><span class="sxs-lookup"><span data-stu-id="c99df-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="c99df-108">Appliquer l’autorisation</span><span class="sxs-lookup"><span data-stu-id="c99df-108">Enforce authorization</span></span>

<span data-ttu-id="c99df-109">Pour appliquer des règles d’autorisation lorsque vous utilisez un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) vous devez substituer la `AuthorizeRequest` (méthode).</span><span class="sxs-lookup"><span data-stu-id="c99df-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="c99df-110">Vous ne pouvez pas utiliser le `Authorize` attribut avec des connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="c99df-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="c99df-111">Le `AuthorizeRequest` méthode est appelée par l’infrastructure SignalR avant chaque requête pour vérifier que l’utilisateur est autorisé à effectuer l’action demandée.</span><span class="sxs-lookup"><span data-stu-id="c99df-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="c99df-112">Le `AuthorizeRequest` méthode n’est pas appelée à partir du client ; au lieu de cela, vous authentifiez l’utilisateur via un mécanisme d’authentification standard de votre application.</span><span class="sxs-lookup"><span data-stu-id="c99df-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="c99df-113">L’exemple ci-dessous montre comment limiter les demandes aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="c99df-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="c99df-114">Vous pouvez ajouter une logique d’autorisation personnalisée dans la méthode AuthorizeRequest ; par exemple, la vérification si un utilisateur appartient à un rôle particulier.</span><span class="sxs-lookup"><span data-stu-id="c99df-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
