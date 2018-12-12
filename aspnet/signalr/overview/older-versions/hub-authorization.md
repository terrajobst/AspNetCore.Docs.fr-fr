---
uid: signalr/overview/older-versions/hub-authorization
title: Authentification et autorisation pour SignalR Hubs (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 1bd61b8faff22db3337e9d9c1ed2532d3e64440d
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287466"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="e5f36-103">Authentification et autorisation pour SignalR Hubs (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="e5f36-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="e5f36-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e5f36-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="e5f36-105">Cette rubrique décrit comment faire pour restreindre les utilisateurs ou les rôles peuvent accéder aux méthodes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="e5f36-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="e5f36-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e5f36-106">Overview</span></span>

<span data-ttu-id="e5f36-107">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="e5f36-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e5f36-108">Autoriser l’attribut</span><span class="sxs-lookup"><span data-stu-id="e5f36-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="e5f36-109">Exiger l’authentification pour tous les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="e5f36-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="e5f36-110">D’autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="e5f36-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="e5f36-111">Passer des informations d’authentification pour les clients</span><span class="sxs-lookup"><span data-stu-id="e5f36-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="e5f36-112">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="e5f36-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="e5f36-113">Cookie avec l’authentification par formulaire</span><span class="sxs-lookup"><span data-stu-id="e5f36-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="e5f36-114">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="e5f36-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="e5f36-115">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="e5f36-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="e5f36-116">Certificat</span><span class="sxs-lookup"><span data-stu-id="e5f36-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="e5f36-117">Autoriser l’attribut</span><span class="sxs-lookup"><span data-stu-id="e5f36-117">Authorize attribute</span></span>

<span data-ttu-id="e5f36-118">SignalR fournit le [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribut pour spécifier quels utilisateurs ou les rôles ont accès à une méthode ou le hub.</span><span class="sxs-lookup"><span data-stu-id="e5f36-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="e5f36-119">Cet attribut se trouve dans le `Microsoft.AspNet.SignalR` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="e5f36-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="e5f36-120">Vous appliquez le `Authorize` d’attribut à un concentrateur ou des méthodes particulières dans un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="e5f36-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="e5f36-121">Lorsque vous appliquez le `Authorize` attribut à une classe de concentrateur, l’exigence d’autorisation spécifié est appliqué à toutes les méthodes dans le hub.</span><span class="sxs-lookup"><span data-stu-id="e5f36-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="e5f36-122">Les différents types de conditions d’autorisation que vous pouvez appliquer sont présentées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e5f36-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="e5f36-123">Sans le `Authorize` attribut, toutes les méthodes publiques sur le hub sont disponibles pour un client qui est connecté au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="e5f36-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="e5f36-124">Si vous avez défini un rôle nommé « Admin » dans votre application web, vous pouvez spécifier que seuls les utilisateurs de ce rôle peuvent accéder à un hub par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e5f36-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="e5f36-125">Ou bien, vous pouvez spécifier qu’un concentrateur contient une méthode qui est disponible pour tous les utilisateurs et une deuxième méthode qui est uniquement disponible pour les utilisateurs authentifiés, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e5f36-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="e5f36-126">Les exemples suivants d’autorisation différents scénarios :</span><span class="sxs-lookup"><span data-stu-id="e5f36-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="e5f36-127">`[Authorize]` : seuls les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="e5f36-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="e5f36-128">`[Authorize(Roles = "Admin,Manager")]` : seuls les utilisateurs aux rôles spécifiés authentifiés</span><span class="sxs-lookup"><span data-stu-id="e5f36-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="e5f36-129">`[Authorize(Users = "user1,user2")]` : seuls les utilisateurs avec des noms d’utilisateurs spécifiés authentifiés</span><span class="sxs-lookup"><span data-stu-id="e5f36-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="e5f36-130">`[Authorize(RequireOutgoing=false)]` – les utilisateurs authentifiés peuvent appeler le concentrateur, mais les appels à partir du serveur aux clients ne sont pas limités par autorisation, telles que, lorsque seuls certains utilisateurs peuvent envoyer un message, mais tous les autres utilisateurs peuvent recevoir le message.</span><span class="sxs-lookup"><span data-stu-id="e5f36-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="e5f36-131">La RequireOutgoing propriété peut uniquement être appliquée à l’ensemble du hub, pas sur les méthodes de personnes dans le hub.</span><span class="sxs-lookup"><span data-stu-id="e5f36-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="e5f36-132">Lorsque RequireOutgoing n’est pas définie sur false, seuls les utilisateurs qui répond à la condition d’autorisation sont appelés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="e5f36-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="e5f36-133">Exiger l’authentification pour tous les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="e5f36-133">Require authentication for all hubs</span></span>

<span data-ttu-id="e5f36-134">Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant le [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) méthode lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="e5f36-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="e5f36-135">Vous pouvez utiliser cette méthode lorsque vous avez plusieurs concentrateurs et que vous souhaitez appliquer une exigence d’authentification pour toutes les.</span><span class="sxs-lookup"><span data-stu-id="e5f36-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="e5f36-136">Avec cette méthode, vous ne pouvez pas spécifier de rôle, utilisateur ou l’autorisation sortante.</span><span class="sxs-lookup"><span data-stu-id="e5f36-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="e5f36-137">Vous ne pouvez spécifier qu’accès aux méthodes de concentrateur l'est limité aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="e5f36-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="e5f36-138">Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux hubs ou des méthodes pour spécifier des exigences supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="e5f36-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="e5f36-139">Toute condition que vous spécifiez dans les attributs est appliquée en plus de l’exigence d’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="e5f36-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="e5f36-140">L’exemple suivant montre un fichier Global.asax, ce qui limite toutes les méthodes de concentrateur aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="e5f36-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="e5f36-141">Si vous appelez le `RequireAuthentication()` méthode après le traitement d’une requête SignalR, SignalR lève un `InvalidOperationException` exception.</span><span class="sxs-lookup"><span data-stu-id="e5f36-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="e5f36-142">Cette exception est levée, car vous ne pouvez pas ajouter un module à HubPipeline après que le pipeline a été appelé.</span><span class="sxs-lookup"><span data-stu-id="e5f36-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="e5f36-143">L’exemple précédent illustre l’appel la `RequireAuthentication` méthode dans le `Application_Start` méthode qui est exécutée une fois avant la première demande.</span><span class="sxs-lookup"><span data-stu-id="e5f36-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="e5f36-144">D’autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="e5f36-144">Customized authorization</span></span>

<span data-ttu-id="e5f36-145">Si vous avez besoin personnaliser la façon dont l’autorisation est définie, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et remplacer le [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) (méthode).</span><span class="sxs-lookup"><span data-stu-id="e5f36-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="e5f36-146">Cette méthode est appelée pour chaque demande afin de déterminer si l’utilisateur est autorisé à effectuer la demande.</span><span class="sxs-lookup"><span data-stu-id="e5f36-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="e5f36-147">Dans la méthode substituée, vous fournissez la logique nécessaire pour votre scénario d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e5f36-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="e5f36-148">L’exemple suivant montre comment appliquer l’autorisation via l’identité basée sur les revendications.</span><span class="sxs-lookup"><span data-stu-id="e5f36-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="e5f36-149">Passer des informations d’authentification pour les clients</span><span class="sxs-lookup"><span data-stu-id="e5f36-149">Pass authentication information to clients</span></span>

<span data-ttu-id="e5f36-150">Vous devrez peut-être utiliser les informations d’authentification dans le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="e5f36-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="e5f36-151">Vous transmettez les informations requises lors de l’appel des méthodes sur le client.</span><span class="sxs-lookup"><span data-stu-id="e5f36-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="e5f36-152">Par exemple, une méthode d’application de conversation peut passer comme paramètre le nom d’utilisateur de la personne à publier un message, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e5f36-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="e5f36-153">Ou bien, vous pouvez créer un objet pour représenter les informations d’authentification et passez cet objet en tant que paramètre, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e5f36-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="e5f36-154">Vous ne devez jamais passer les id de connexion d’un client à d’autres clients, car un utilisateur malveillant pourrait utiliser pour simuler une demande à partir de ce client.</span><span class="sxs-lookup"><span data-stu-id="e5f36-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="e5f36-155">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="e5f36-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="e5f36-156">Lorsque vous avez un client .NET, par exemple une application console, qui interagit avec un concentrateur est limité aux utilisateurs authentifiés, vous pouvez passer les informations d’identification de l’authentification dans un cookie, l’en-tête de connexion ou un certificat.</span><span class="sxs-lookup"><span data-stu-id="e5f36-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="e5f36-157">Les exemples de cette section montrent comment utiliser ces méthodes différentes pour authentifier un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e5f36-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="e5f36-158">Ils ne sont pas des applications SignalR entièrement fonctionnelles.</span><span class="sxs-lookup"><span data-stu-id="e5f36-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="e5f36-159">Pour plus d’informations sur les clients .NET avec SignalR, consultez [Guide de l’API Hubs - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="e5f36-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="e5f36-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="e5f36-160">Cookie</span></span>

<span data-ttu-id="e5f36-161">Quand votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="e5f36-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="e5f36-162">Vous ajoutez le cookie à la `CookieContainer` propriété sur le [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objet.</span><span class="sxs-lookup"><span data-stu-id="e5f36-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="e5f36-163">L’exemple suivant montre une application console qui Récupère un cookie d’authentification à partir d’une page web et ajoute ce cookie à la connexion.</span><span class="sxs-lookup"><span data-stu-id="e5f36-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="e5f36-164">L’URL `https://www.contoso.com/RemoteLogin` dans l’exemple pointe vers une page web que vous aurez à créer.</span><span class="sxs-lookup"><span data-stu-id="e5f36-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="e5f36-165">La page voulez-vous récupérer le nom d’utilisateur validée et le mot de passe et tentent de se connecter l’utilisateur avec les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="e5f36-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="e5f36-166">L’application de console publie les informations d’identification à www.contoso.com/RemoteLogin qui peut faire référence à une page vide qui contient le fichier code-behind suivant.</span><span class="sxs-lookup"><span data-stu-id="e5f36-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="e5f36-167">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="e5f36-167">Windows authentication</span></span>

<span data-ttu-id="e5f36-168">Lorsque vous utilisez l’authentification Windows, vous pouvez passer des informations d’identification de l’utilisateur actuel à l’aide de la [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) propriété.</span><span class="sxs-lookup"><span data-stu-id="e5f36-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="e5f36-169">Vous définissez les informations d’identification pour la connexion à la valeur de DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="e5f36-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="e5f36-170">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="e5f36-170">Connection header</span></span>

<span data-ttu-id="e5f36-171">Si votre application n’utilise pas les cookies, vous pouvez passer des informations utilisateur dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="e5f36-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="e5f36-172">Par exemple, vous pouvez passer un jeton dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="e5f36-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="e5f36-173">Puis, dans le hub, vérifiez que le jeton d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e5f36-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="e5f36-174">Certificat</span><span class="sxs-lookup"><span data-stu-id="e5f36-174">Certificate</span></span>

<span data-ttu-id="e5f36-175">Vous pouvez transmettre un certificat client pour vérifier que l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e5f36-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="e5f36-176">Vous ajoutez le certificat lors de la création de la connexion.</span><span class="sxs-lookup"><span data-stu-id="e5f36-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="e5f36-177">L’exemple suivant montre uniquement comment ajouter un certificat client à la connexion ; Il n’affiche pas l’application console complète.</span><span class="sxs-lookup"><span data-stu-id="e5f36-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="e5f36-178">Il utilise le [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe qui fournit différentes façons de créer le certificat.</span><span class="sxs-lookup"><span data-stu-id="e5f36-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
