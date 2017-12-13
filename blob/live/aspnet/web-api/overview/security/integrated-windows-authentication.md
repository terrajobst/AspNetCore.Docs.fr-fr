---
uid: web-api/overview/security/integrated-windows-authentication
title: "L’authentification intégrée Windows | Documents Microsoft"
author: MikeWasson
description: "Décrit l’utilisation de l’authentification Windows intégrée dans l’API Web ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="9ee99-103">Authentification Windows intégrée</span><span class="sxs-lookup"><span data-stu-id="9ee99-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="9ee99-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9ee99-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9ee99-105">L’authentification Windows intégrée permet aux utilisateurs de se connecter avec leurs informations d’identification Windows, à l’aide de Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="9ee99-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="9ee99-106">Le client envoie des informations d’identification dans l’en-tête d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="9ee99-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="9ee99-107">L’authentification Windows est idéale pour un environnement intranet.</span><span class="sxs-lookup"><span data-stu-id="9ee99-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="9ee99-108">Pour plus d’informations, consultez [l’authentification Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="9ee99-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="9ee99-109">Avantages</span><span class="sxs-lookup"><span data-stu-id="9ee99-109">Advantages</span></span> | <span data-ttu-id="9ee99-110">Inconvénients</span><span class="sxs-lookup"><span data-stu-id="9ee99-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="9ee99-111">-Intégrée à IIS.</span><span class="sxs-lookup"><span data-stu-id="9ee99-111">- Built into IIS.</span></span> <span data-ttu-id="9ee99-112">-Ne pas envoyer les informations d’identification de l’utilisateur dans la requête.</span><span class="sxs-lookup"><span data-stu-id="9ee99-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="9ee99-113">-Si l’ordinateur client appartient au domaine (par exemple, les applications intranet), l’utilisateur est inutile d’entrer des informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="9ee99-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="9ee99-114">-N’est pas recommandé pour les applications Internet.</span><span class="sxs-lookup"><span data-stu-id="9ee99-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="9ee99-115">-Nécessite la prise en charge Kerberos ou NTLM dans le client.</span><span class="sxs-lookup"><span data-stu-id="9ee99-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="9ee99-116">-Le client doit être dans le domaine Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9ee99-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="9ee99-117">Si votre application est hébergée sur Azure, vous disposez d’un domaine Active Directory sur site, envisagez de fédérer votre AD local avec Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9ee99-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="9ee99-118">De cette façon, les utilisateurs peuvent se connecter avec leurs informations d’identification sur site, mais l’authentification est effectuée par Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ee99-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="9ee99-119">Pour plus d’informations, consultez [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="9ee99-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="9ee99-120">Pour créer une application qui utilise l’authentification Windows intégrée, sélectionnez le modèle de « Application Intranet » dans l’Assistant de projet MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9ee99-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="9ee99-121">Ce modèle de projet met le paramètre suivant dans le fichier Web.config :</span><span class="sxs-lookup"><span data-stu-id="9ee99-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="9ee99-122">Du côté client, l’authentification Windows intégrée fonctionne avec n’importe quel navigateur prenant en charge la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schéma d’authentification, qui inclut la plupart des principaux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="9ee99-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="9ee99-123">Pour les applications clientes .NET, le **HttpClient** classe prend en charge l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="9ee99-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="9ee99-124">L’authentification Windows est vulnérable aux attaques de contrefaçon (CSRF) demande entre sites.</span><span class="sxs-lookup"><span data-stu-id="9ee99-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="9ee99-125">Consultez [empêcher les attaques de Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="9ee99-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
