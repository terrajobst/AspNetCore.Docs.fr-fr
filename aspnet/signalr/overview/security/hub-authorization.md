---
uid: signalr/overview/security/hub-authorization
title: Authentification et autorisation pour les concentrateurs SignalR | Documents Microsoft
author: pfletcher
description: "Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur. Versions du logiciel utilisé dans cette rubrique Visual Studio 2013 .NET 4.5 SignalR ve..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: cb0f06a3ca2b39a4a952c33cea70136c7c5af7a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="981d7-104">Authentification et autorisation pour les concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="981d7-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="981d7-105">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="981d7-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="981d7-106">Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="981d7-107">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="981d7-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="981d7-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="981d7-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="981d7-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="981d7-109">.NET 4.5</span></span>
> - <span data-ttu-id="981d7-110">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="981d7-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="981d7-111">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="981d7-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="981d7-112">Pour plus d’informations sur les versions antérieures de SignalR, consultez [SignalR des Versions antérieures](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="981d7-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="981d7-113">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="981d7-113">Questions and comments</span></span>
> 
> <span data-ttu-id="981d7-114">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="981d7-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="981d7-115">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="981d7-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="981d7-116">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="981d7-116">Overview</span></span>

<span data-ttu-id="981d7-117">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="981d7-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="981d7-118">Autoriser l’attribut</span><span class="sxs-lookup"><span data-stu-id="981d7-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="981d7-119">Exiger l’authentification pour tous les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="981d7-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="981d7-120">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="981d7-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="981d7-121">Passer des informations d’authentification pour les clients</span><span class="sxs-lookup"><span data-stu-id="981d7-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="981d7-122">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="981d7-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="981d7-123">Cookie d’authentification par formulaires</span><span class="sxs-lookup"><span data-stu-id="981d7-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="981d7-124">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="981d7-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="981d7-125">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="981d7-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="981d7-126">Certificate</span><span class="sxs-lookup"><span data-stu-id="981d7-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="981d7-127">Autoriser l’attribut</span><span class="sxs-lookup"><span data-stu-id="981d7-127">Authorize attribute</span></span>

<span data-ttu-id="981d7-128">SignalR fournit le [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribut pour spécifier les utilisateurs ou les rôles ont accès à une méthode ou un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="981d7-129">Cet attribut se trouve dans le `Microsoft.AspNet.SignalR` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="981d7-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="981d7-130">Vous appliquez le `Authorize` d’attribut à un concentrateur ou des méthodes dans un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="981d7-131">Lorsque vous appliquez le `Authorize` attribut à une classe de concentrateur, l’exigence d’autorisation spécifié est appliqué à toutes les méthodes dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="981d7-132">Cette rubrique fournit des exemples des différents types de spécifications d’autorisation que vous pouvez appliquer.</span><span class="sxs-lookup"><span data-stu-id="981d7-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="981d7-133">Sans le `Authorize` attribut connecté client peut accéder à n’importe quelle méthode publique sur le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="981d7-134">Si vous avez défini un rôle nommé « Admin » dans votre application web, vous pouvez spécifier que seuls les utilisateurs de ce rôle peuvent accéder à un concentrateur avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="981d7-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="981d7-135">Ou bien, vous pouvez spécifier qu’un concentrateur contient une méthode qui est disponible pour tous les utilisateurs et une deuxième méthode est uniquement disponible pour les utilisateurs authentifiés, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="981d7-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="981d7-136">Les exemples suivants d’autorisation différents scénarios :</span><span class="sxs-lookup"><span data-stu-id="981d7-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="981d7-137">`[Authorize]`: seuls les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="981d7-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="981d7-138">`[Authorize(Roles = "Admin,Manager")]`: seuls les utilisateurs spécifiés aux rôles authentifiés</span><span class="sxs-lookup"><span data-stu-id="981d7-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="981d7-139">`[Authorize(Users = "user1,user2")]`: seuls les utilisateurs avec des noms d’utilisateurs spécifiés authentifiés</span><span class="sxs-lookup"><span data-stu-id="981d7-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="981d7-140">`[Authorize(RequireOutgoing=false)]`: seuls les utilisateurs authentifiés peuvent appeler le concentrateur, mais les appels à partir du serveur aux clients ne sont pas limités par l’autorisation, tels que, lorsque seuls certains utilisateurs peuvent envoyer un message, mais tous les autres utilisateurs peuvent recevoir le message.</span><span class="sxs-lookup"><span data-stu-id="981d7-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="981d7-141">La propriété RequireOutgoing applicable uniquement pour le hub entier, pas sur les méthodes de personnes dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="981d7-142">RequireOutgoing n’est pas définie sur false, seuls les utilisateurs qui répond aux exigences d’autorisation sont appelés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="981d7-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="981d7-143">Exiger l’authentification pour tous les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="981d7-143">Require authentication for all hubs</span></span>

<span data-ttu-id="981d7-144">Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant le [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) méthode lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="981d7-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="981d7-145">Vous pouvez utiliser cette méthode lorsque vous avez plusieurs concentrateurs et que vous souhaitez appliquer une exigence d’authentification pour l’ensemble des.</span><span class="sxs-lookup"><span data-stu-id="981d7-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="981d7-146">Avec cette méthode, vous ne pouvez pas spécifier de configuration requise pour le rôle, un utilisateur ou d’autorisation sortante.</span><span class="sxs-lookup"><span data-stu-id="981d7-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="981d7-147">Vous ne pouvez spécifier qu’accès aux méthodes de concentrateur l'est limité aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="981d7-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="981d7-148">Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux concentrateurs ou des méthodes pour spécifier des exigences supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="981d7-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="981d7-149">Aucune exigence que vous spécifiez dans un attribut est ajouté à la condition d’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="981d7-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="981d7-150">L’exemple suivant montre un fichier de démarrage qui limite toutes les méthodes de concentrateur aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="981d7-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="981d7-151">Si vous appelez le `RequireAuthentication()` méthode après le traitement d’une requête SignalR, SignalR lèvera une `InvalidOperationException` exception.</span><span class="sxs-lookup"><span data-stu-id="981d7-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="981d7-152">SignalR lève cette exception, car il est impossible d’ajouter un module à la HubPipeline une fois que le pipeline a été appelé.</span><span class="sxs-lookup"><span data-stu-id="981d7-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="981d7-153">L’exemple précédent montre l’appel du `RequireAuthentication` méthode dans le `Configuration` méthode qui est exécutée une fois avant la première requête.</span><span class="sxs-lookup"><span data-stu-id="981d7-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="981d7-154">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="981d7-154">Customized authorization</span></span>

<span data-ttu-id="981d7-155">Si vous avez besoin personnaliser la façon dont l’autorisation est définie, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et remplacez le [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) (méthode).</span><span class="sxs-lookup"><span data-stu-id="981d7-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="981d7-156">Pour chaque demande, SignalR appelle cette méthode pour déterminer si l’utilisateur est autorisé à traiter la demande.</span><span class="sxs-lookup"><span data-stu-id="981d7-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="981d7-157">Dans la méthode substituée, vous fournir la logique nécessaire pour votre scénario d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="981d7-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="981d7-158">L’exemple suivant montre comment appliquer l’autorisation via une identité basée sur les revendications.</span><span class="sxs-lookup"><span data-stu-id="981d7-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="981d7-159">Passer des informations d’authentification pour les clients</span><span class="sxs-lookup"><span data-stu-id="981d7-159">Pass authentication information to clients</span></span>

<span data-ttu-id="981d7-160">Vous devrez peut-être utiliser les informations d’authentification dans le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="981d7-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="981d7-161">Vous transmettez les informations requises lors de l’appel de méthodes sur le client.</span><span class="sxs-lookup"><span data-stu-id="981d7-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="981d7-162">Par exemple, une méthode d’application de conversation Impossible passer en tant que paramètre le nom d’utilisateur de la personne à publier un message, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="981d7-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="981d7-163">Ou bien, vous pouvez créer un objet pour représenter les informations d’authentification et transférer cet objet en tant que paramètre, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="981d7-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="981d7-164">Vous ne devez jamais passer les id de connexion d’un client à d’autres clients, comme un utilisateur malveillant peut utiliser pour simuler une demande à partir de ce client.</span><span class="sxs-lookup"><span data-stu-id="981d7-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="981d7-165">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="981d7-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="981d7-166">Lorsque vous disposez d’un client .NET, comme une application console, qui interagit avec un concentrateur est limité aux utilisateurs authentifiés, vous pouvez passer les informations d’identification de l’authentification dans un cookie, l’en-tête de connexion ou un certificat.</span><span class="sxs-lookup"><span data-stu-id="981d7-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="981d7-167">Les exemples de cette section montrent comment utiliser les différentes méthodes pour authentifier un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="981d7-168">Ils ne sont pas des applications SignalR entièrement fonctionnelles.</span><span class="sxs-lookup"><span data-stu-id="981d7-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="981d7-169">Pour plus d’informations sur les clients .NET avec SignalR, consultez [concentrateurs API Guide - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="981d7-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="981d7-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="981d7-170">Cookie</span></span>

<span data-ttu-id="981d7-171">Quand votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="981d7-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="981d7-172">Vous ajoutez le cookie à la `CookieContainer` propriété sur le [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objet.</span><span class="sxs-lookup"><span data-stu-id="981d7-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="981d7-173">L’exemple suivant montre une application console qui Récupère un cookie d’authentification à partir d’une page web et ajoute ce cookie à la connexion.</span><span class="sxs-lookup"><span data-stu-id="981d7-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="981d7-174">L’application console publie les informations d’identification **www.contoso.com/RemoteLogin** qui peut faire référence à une page vide qui contient le fichier code-behind suivante.</span><span class="sxs-lookup"><span data-stu-id="981d7-174">The console app posts the credentials to **www.contoso.com/RemoteLogin** which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="981d7-175">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="981d7-175">Windows authentication</span></span>

<span data-ttu-id="981d7-176">Lorsque vous utilisez l’authentification Windows, vous pouvez passer des informations d’identification de l’utilisateur actuel à l’aide de la [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) propriété.</span><span class="sxs-lookup"><span data-stu-id="981d7-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="981d7-177">Vous définissez les informations d’identification pour la connexion à la valeur de DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="981d7-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="981d7-178">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="981d7-178">Connection header</span></span>

<span data-ttu-id="981d7-179">Si votre application n’utilise pas les cookies, vous pouvez passer des informations sur l’utilisateur dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="981d7-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="981d7-180">Par exemple, vous pouvez passer un jeton dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="981d7-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="981d7-181">Puis, dans le concentrateur, vérifiez que le jeton d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="981d7-182">Certificat</span><span class="sxs-lookup"><span data-stu-id="981d7-182">Certificate</span></span>

<span data-ttu-id="981d7-183">Vous pouvez passer un certificat client pour vérifier que l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="981d7-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="981d7-184">Vous ajoutez le certificat lors de la création de la connexion.</span><span class="sxs-lookup"><span data-stu-id="981d7-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="981d7-185">L’exemple suivant montre uniquement comment ajouter un certificat client pour la connexion ; Il n’affiche pas l’application console complète.</span><span class="sxs-lookup"><span data-stu-id="981d7-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="981d7-186">Elle utilise le [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe qui fournit plusieurs façons différentes pour créer le certificat.</span><span class="sxs-lookup"><span data-stu-id="981d7-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
