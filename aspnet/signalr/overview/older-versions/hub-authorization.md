---
uid: signalr/overview/older-versions/hub-authorization
title: Authentification et autorisation pour les concentrateurs SignalR (SignalR 1.x) | Documents Microsoft
author: pfletcher
description: Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042873"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="0ac69-103">Authentification et autorisation pour les concentrateurs SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="0ac69-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="0ac69-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0ac69-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0ac69-105">Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="0ac69-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="0ac69-106">Overview</span></span>

<span data-ttu-id="0ac69-107">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="0ac69-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="0ac69-108">Autoriser l’attribut</span><span class="sxs-lookup"><span data-stu-id="0ac69-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="0ac69-109">Exiger l’authentification pour tous les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="0ac69-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="0ac69-110">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="0ac69-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="0ac69-111">Passer des informations d’authentification pour les clients</span><span class="sxs-lookup"><span data-stu-id="0ac69-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="0ac69-112">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="0ac69-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="0ac69-113">Cookie d’authentification par formulaires</span><span class="sxs-lookup"><span data-stu-id="0ac69-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="0ac69-114">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="0ac69-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="0ac69-115">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="0ac69-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="0ac69-116">Certificate</span><span class="sxs-lookup"><span data-stu-id="0ac69-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="0ac69-117">Autoriser l’attribut</span><span class="sxs-lookup"><span data-stu-id="0ac69-117">Authorize attribute</span></span>

<span data-ttu-id="0ac69-118">SignalR fournit le [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribut pour spécifier les utilisateurs ou les rôles ont accès à une méthode ou un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="0ac69-119">Cet attribut se trouve dans le `Microsoft.AspNet.SignalR` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="0ac69-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="0ac69-120">Vous appliquez le `Authorize` d’attribut à un concentrateur ou des méthodes dans un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="0ac69-121">Lorsque vous appliquez le `Authorize` attribut à une classe de concentrateur, l’exigence d’autorisation spécifié est appliqué à toutes les méthodes dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="0ac69-122">Les différents types de spécifications d’autorisation que vous pouvez appliquer sont indiquées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0ac69-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="0ac69-123">Sans le `Authorize` attribut, toutes les méthodes publiques sur le concentrateur sont disponibles pour un client qui est connecté au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="0ac69-124">Si vous avez défini un rôle nommé « Admin » dans votre application web, vous pouvez spécifier que seuls les utilisateurs de ce rôle peuvent accéder à un concentrateur avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="0ac69-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="0ac69-125">Ou bien, vous pouvez spécifier qu’un concentrateur contient une méthode qui est disponible pour tous les utilisateurs et une deuxième méthode est uniquement disponible pour les utilisateurs authentifiés, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0ac69-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="0ac69-126">Les exemples suivants d’autorisation différents scénarios :</span><span class="sxs-lookup"><span data-stu-id="0ac69-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="0ac69-127">`[Authorize]`: seuls les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="0ac69-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="0ac69-128">`[Authorize(Roles = "Admin,Manager")]`: seuls les utilisateurs spécifiés aux rôles authentifiés</span><span class="sxs-lookup"><span data-stu-id="0ac69-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="0ac69-129">`[Authorize(Users = "user1,user2")]`: seuls les utilisateurs avec des noms d’utilisateurs spécifiés authentifiés</span><span class="sxs-lookup"><span data-stu-id="0ac69-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="0ac69-130">`[Authorize(RequireOutgoing=false)]`: seuls les utilisateurs authentifiés peuvent appeler le concentrateur, mais les appels à partir du serveur aux clients ne sont pas limités par l’autorisation, tels que, lorsque seuls certains utilisateurs peuvent envoyer un message, mais tous les autres utilisateurs peuvent recevoir le message.</span><span class="sxs-lookup"><span data-stu-id="0ac69-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="0ac69-131">La propriété RequireOutgoing applicable uniquement pour le hub entier, pas sur les méthodes de personnes dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="0ac69-132">RequireOutgoing n’est pas définie sur false, seuls les utilisateurs qui répond aux exigences d’autorisation sont appelés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="0ac69-133">Exiger l’authentification pour tous les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="0ac69-133">Require authentication for all hubs</span></span>

<span data-ttu-id="0ac69-134">Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant le [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) méthode lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="0ac69-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="0ac69-135">Vous pouvez utiliser cette méthode lorsque vous avez plusieurs concentrateurs et que vous souhaitez appliquer une exigence d’authentification pour l’ensemble des.</span><span class="sxs-lookup"><span data-stu-id="0ac69-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="0ac69-136">Avec cette méthode, vous ne pouvez pas spécifier de rôle, un utilisateur ou l’autorisation sortante.</span><span class="sxs-lookup"><span data-stu-id="0ac69-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="0ac69-137">Vous ne pouvez spécifier qu’accès aux méthodes de concentrateur l'est limité aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="0ac69-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="0ac69-138">Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux concentrateurs ou des méthodes pour spécifier des exigences supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="0ac69-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="0ac69-139">Les exigences que vous spécifiez dans les attributs sont appliqué en plus de la condition d’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="0ac69-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="0ac69-140">L’exemple suivant montre un fichier Global.asax qui limite toutes les méthodes de concentrateur aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="0ac69-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="0ac69-141">Si vous appelez le `RequireAuthentication()` méthode après le traitement d’une requête SignalR, SignalR lèvera une `InvalidOperationException` exception.</span><span class="sxs-lookup"><span data-stu-id="0ac69-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="0ac69-142">Cette exception est levée, car il est impossible d’ajouter un module à la HubPipeline une fois que le pipeline a été appelé.</span><span class="sxs-lookup"><span data-stu-id="0ac69-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="0ac69-143">L’exemple précédent montre l’appel du `RequireAuthentication` méthode dans le `Application_Start` méthode qui est exécutée une fois avant la première requête.</span><span class="sxs-lookup"><span data-stu-id="0ac69-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="0ac69-144">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="0ac69-144">Customized authorization</span></span>

<span data-ttu-id="0ac69-145">Si vous avez besoin personnaliser la façon dont l’autorisation est définie, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et remplacez le [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) (méthode).</span><span class="sxs-lookup"><span data-stu-id="0ac69-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="0ac69-146">Cette méthode est appelée pour chaque demande afin de déterminer si l’utilisateur est autorisé à traiter la demande.</span><span class="sxs-lookup"><span data-stu-id="0ac69-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="0ac69-147">Dans la méthode substituée, vous fournir la logique nécessaire pour votre scénario d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="0ac69-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="0ac69-148">L’exemple suivant montre comment appliquer l’autorisation via une identité basée sur les revendications.</span><span class="sxs-lookup"><span data-stu-id="0ac69-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="0ac69-149">Passer des informations d’authentification pour les clients</span><span class="sxs-lookup"><span data-stu-id="0ac69-149">Pass authentication information to clients</span></span>

<span data-ttu-id="0ac69-150">Vous devrez peut-être utiliser les informations d’authentification dans le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="0ac69-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="0ac69-151">Vous transmettez les informations requises lors de l’appel de méthodes sur le client.</span><span class="sxs-lookup"><span data-stu-id="0ac69-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="0ac69-152">Par exemple, une méthode d’application de conversation Impossible passer en tant que paramètre le nom d’utilisateur de la personne à publier un message, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0ac69-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="0ac69-153">Ou bien, vous pouvez créer un objet pour représenter les informations d’authentification et transférer cet objet en tant que paramètre, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0ac69-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="0ac69-154">Vous ne devez jamais passer les id de connexion d’un client à d’autres clients, comme un utilisateur malveillant peut utiliser pour simuler une demande à partir de ce client.</span><span class="sxs-lookup"><span data-stu-id="0ac69-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="0ac69-155">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="0ac69-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="0ac69-156">Lorsque vous disposez d’un client .NET, comme une application console, qui interagit avec un concentrateur est limité aux utilisateurs authentifiés, vous pouvez passer les informations d’identification de l’authentification dans un cookie, l’en-tête de connexion ou un certificat.</span><span class="sxs-lookup"><span data-stu-id="0ac69-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="0ac69-157">Les exemples de cette section montrent comment utiliser les différentes méthodes pour authentifier un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="0ac69-158">Ils ne sont pas des applications SignalR entièrement fonctionnelles.</span><span class="sxs-lookup"><span data-stu-id="0ac69-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="0ac69-159">Pour plus d’informations sur les clients .NET avec SignalR, consultez [concentrateurs API Guide - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="0ac69-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="0ac69-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="0ac69-160">Cookie</span></span>

<span data-ttu-id="0ac69-161">Quand votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="0ac69-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="0ac69-162">Vous ajoutez le cookie à la `CookieContainer` propriété sur le [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objet.</span><span class="sxs-lookup"><span data-stu-id="0ac69-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="0ac69-163">L’exemple suivant montre une application console qui Récupère un cookie d’authentification à partir d’une page web et ajoute ce cookie à la connexion.</span><span class="sxs-lookup"><span data-stu-id="0ac69-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="0ac69-164">L’URL `https://www.contoso.com/RemoteLogin` dans l’exemple pointe vers une page web que vous devrez créer.</span><span class="sxs-lookup"><span data-stu-id="0ac69-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="0ac69-165">La page serait récupérer le nom d’utilisateur enregistré et le mot de passe et essayer de se connecter l’utilisateur avec les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="0ac69-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="0ac69-166">L’application console publie les informations d’identification www.contoso.com/RemoteLogin qui peut faire référence à une page vide qui contient le fichier code-behind suivante.</span><span class="sxs-lookup"><span data-stu-id="0ac69-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="0ac69-167">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="0ac69-167">Windows authentication</span></span>

<span data-ttu-id="0ac69-168">Lorsque vous utilisez l’authentification Windows, vous pouvez passer des informations d’identification de l’utilisateur actuel à l’aide de la [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) propriété.</span><span class="sxs-lookup"><span data-stu-id="0ac69-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="0ac69-169">Vous définissez les informations d’identification pour la connexion à la valeur de DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="0ac69-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="0ac69-170">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="0ac69-170">Connection header</span></span>

<span data-ttu-id="0ac69-171">Si votre application n’utilise pas les cookies, vous pouvez passer des informations sur l’utilisateur dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="0ac69-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="0ac69-172">Par exemple, vous pouvez passer un jeton dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="0ac69-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="0ac69-173">Puis, dans le concentrateur, vérifiez que le jeton d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="0ac69-174">Certificat</span><span class="sxs-lookup"><span data-stu-id="0ac69-174">Certificate</span></span>

<span data-ttu-id="0ac69-175">Vous pouvez passer un certificat client pour vérifier que l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0ac69-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="0ac69-176">Vous ajoutez le certificat lors de la création de la connexion.</span><span class="sxs-lookup"><span data-stu-id="0ac69-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="0ac69-177">L’exemple suivant montre uniquement comment ajouter un certificat client pour la connexion ; Il n’affiche pas l’application console complète.</span><span class="sxs-lookup"><span data-stu-id="0ac69-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="0ac69-178">Elle utilise le [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe qui fournit plusieurs façons différentes pour créer le certificat.</span><span class="sxs-lookup"><span data-stu-id="0ac69-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
