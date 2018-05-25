---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Utilisation de SSL dans l’API Web | Documents Microsoft
author: MikeWasson
description: Montre comment utiliser le protocole SSL avec l’API Web ASP.NET, y compris l’utilisation de certificats de client SSL.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="acba0-103">Utilisation de SSL dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="acba0-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="acba0-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="acba0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="acba0-105">Plusieurs schémas d’authentification courantes ne sont pas sécurisés sur HTTP en clair.</span><span class="sxs-lookup"><span data-stu-id="acba0-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="acba0-106">En particulier, l’authentification de base et l’authentification par formulaire envoient des informations d’identification non chiffrées.</span><span class="sxs-lookup"><span data-stu-id="acba0-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="acba0-107">Pour être sécurisés, ces schémas d’authentification *doit* utiliser SSL.</span><span class="sxs-lookup"><span data-stu-id="acba0-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="acba0-108">En outre, les certificats clients SSL peuvent être utilisés pour authentifier les clients.</span><span class="sxs-lookup"><span data-stu-id="acba0-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="acba0-109">L’activation de SSL sur le serveur</span><span class="sxs-lookup"><span data-stu-id="acba0-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="acba0-110">Pour configurer SSL dans IIS 7 ou version ultérieure :</span><span class="sxs-lookup"><span data-stu-id="acba0-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="acba0-111">Créez ou obtenez un certificat.</span><span class="sxs-lookup"><span data-stu-id="acba0-111">Create or get a certificate.</span></span> <span data-ttu-id="acba0-112">Pour le test, vous pouvez créer un certificat auto-signé.</span><span class="sxs-lookup"><span data-stu-id="acba0-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="acba0-113">Ajouter une liaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="acba0-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="acba0-114">Pour plus d’informations, consultez [comment la configuration de SSL sur IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="acba0-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="acba0-115">Pour le test local, vous pouvez activer SSL dans IIS Express à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acba0-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="acba0-116">Dans la fenêtre Propriétés, définissez **SSL activé** à **True**.</span><span class="sxs-lookup"><span data-stu-id="acba0-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="acba0-117">Notez la valeur de **URL SSL**; utiliser cette URL pour le test des connexions HTTPS.</span><span class="sxs-lookup"><span data-stu-id="acba0-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="acba0-118">Application de SSL dans un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="acba0-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="acba0-119">Si vous avez à la fois une liaison HTTP et HTTPS, les clients peuvent toujours utiliser HTTP pour accéder au site.</span><span class="sxs-lookup"><span data-stu-id="acba0-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="acba0-120">Vous pouvez autoriser des ressources pour être disponible via le protocole HTTP, tandis que les autres ressources exiger le protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="acba0-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="acba0-121">Dans ce cas, utilisez un filtre d’action pour exiger SSL pour les ressources protégées.</span><span class="sxs-lookup"><span data-stu-id="acba0-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="acba0-122">Le code suivant montre un filtre API Web d’authentification qui vérifie si SSL :</span><span class="sxs-lookup"><span data-stu-id="acba0-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="acba0-123">Ajoutez ce filtre à toutes les actions de l’API Web nécessitant SSL :</span><span class="sxs-lookup"><span data-stu-id="acba0-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="acba0-124">Certificats clients SSL</span><span class="sxs-lookup"><span data-stu-id="acba0-124">SSL Client Certificates</span></span>

<span data-ttu-id="acba0-125">Le protocole SSL fournit l’authentification à l’aide de certificats d’Infrastructure à clé publique.</span><span class="sxs-lookup"><span data-stu-id="acba0-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="acba0-126">Le serveur doit fournir un certificat qui authentifie le serveur au client.</span><span class="sxs-lookup"><span data-stu-id="acba0-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="acba0-127">Il est moins courant pour le client fournisse un certificat pour le serveur, mais il s’agit d’une option pour l’authentification de clients.</span><span class="sxs-lookup"><span data-stu-id="acba0-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="acba0-128">Pour utiliser des certificats de client avec SSL, vous avez besoin d’un moyen de distribuer des certificats auto-signés pour vos utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="acba0-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="acba0-129">Pour de nombreux types d’application, il s’agit pas d’une bonne expérience utilisateur, mais dans certains environnements (par exemple, enterprise), il peut être possible.</span><span class="sxs-lookup"><span data-stu-id="acba0-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="acba0-130">Avantages</span><span class="sxs-lookup"><span data-stu-id="acba0-130">Advantages</span></span> | <span data-ttu-id="acba0-131">Inconvénients</span><span class="sxs-lookup"><span data-stu-id="acba0-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="acba0-132">-Informations d’identification de certificat sont plus fortes que le nom d’utilisateur/mot de passe.</span><span class="sxs-lookup"><span data-stu-id="acba0-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="acba0-133">-SSL fournit un canal sécurisé complète, avec l’authentification, l’intégrité des messages et le chiffrement du message.</span><span class="sxs-lookup"><span data-stu-id="acba0-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="acba0-134">-Vous devez obtenir et gérer des certificats d’infrastructure à clé publique.</span><span class="sxs-lookup"><span data-stu-id="acba0-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="acba0-135">-La plateforme du client doit prendre en charge les certificats clients SSL.</span><span class="sxs-lookup"><span data-stu-id="acba0-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="acba0-136">Pour configurer IIS pour accepter les certificats de client, ouvrez le Gestionnaire des services Internet et procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="acba0-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="acba0-137">Cliquez sur le nœud de site dans l’arborescence.</span><span class="sxs-lookup"><span data-stu-id="acba0-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="acba0-138">Double-cliquez sur le **paramètres SSL** fonctionnalité dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="acba0-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="acba0-139">Sous **les certificats clients**, sélectionnez une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="acba0-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="acba0-140">**Accepter**: IIS accepte un certificat à partir du client, mais n’en requiert pas.</span><span class="sxs-lookup"><span data-stu-id="acba0-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="acba0-141">**Nécessitent**: nécessitent un certificat client.</span><span class="sxs-lookup"><span data-stu-id="acba0-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="acba0-142">(Pour activer cette option, vous devez également sélectionner « Exiger SSL »)</span><span class="sxs-lookup"><span data-stu-id="acba0-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="acba0-143">Vous pouvez également définir ces options dans le fichier ApplicationHost.config :</span><span class="sxs-lookup"><span data-stu-id="acba0-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="acba0-144">Le **SslNegotiateCert** indicateur signifie IIS accepte un certificat à partir du client, mais ne requiert pas (équivalent à l’option « Accepter » dans le Gestionnaire des services Internet).</span><span class="sxs-lookup"><span data-stu-id="acba0-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="acba0-145">Pour demander un certificat, vous devez définir le **le paramètre SslRequireCert** indicateur.</span><span class="sxs-lookup"><span data-stu-id="acba0-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="acba0-146">Pour le test, vous pouvez également définir ces options dans IIS Express, dans le hôte d’application local. Fichier de configuration situé dans « Documents\IISExpress\config ».</span><span class="sxs-lookup"><span data-stu-id="acba0-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="acba0-147">Création d’un certificat de Client de test</span><span class="sxs-lookup"><span data-stu-id="acba0-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="acba0-148">À des fins de test, vous pouvez utiliser [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) pour créer un certificat client.</span><span class="sxs-lookup"><span data-stu-id="acba0-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="acba0-149">Commencez par créer une autorité racine de test :</span><span class="sxs-lookup"><span data-stu-id="acba0-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="acba0-150">Makecert vous invite à entrer un mot de passe pour la clé privée.</span><span class="sxs-lookup"><span data-stu-id="acba0-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="acba0-151">Ensuite, ajoutez le certificat pour le test de stockage « Trusted Root Certification Authorities » du serveur, comme suit :</span><span class="sxs-lookup"><span data-stu-id="acba0-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="acba0-152">Ouvrez la console MMC.</span><span class="sxs-lookup"><span data-stu-id="acba0-152">Open MMC.</span></span>
2. <span data-ttu-id="acba0-153">Sous **fichier**, sélectionnez **ajouter/supprimer un composant logiciel enfichable**.</span><span class="sxs-lookup"><span data-stu-id="acba0-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="acba0-154">Sélectionnez **compte d’ordinateur**.</span><span class="sxs-lookup"><span data-stu-id="acba0-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="acba0-155">Sélectionnez **ordinateur Local** et terminez l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="acba0-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="acba0-156">Sous le volet de navigation, développez le nœud « Trusted Root Certification Authorities ».</span><span class="sxs-lookup"><span data-stu-id="acba0-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="acba0-157">Sur le **Action** menu, pointez sur **toutes les tâches**, puis cliquez sur **importation** pour démarrer l’Assistant Importation de certificat.</span><span class="sxs-lookup"><span data-stu-id="acba0-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="acba0-158">Recherchez le fichier de certificat, TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="acba0-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="acba0-159">Cliquez sur **ouvrir**, puis cliquez sur **suivant** et terminez l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="acba0-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="acba0-160">(Vous devrez entrer à nouveau le mot de passe.)</span><span class="sxs-lookup"><span data-stu-id="acba0-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="acba0-161">Créer un certificat de client qui est signé par le premier certificat :</span><span class="sxs-lookup"><span data-stu-id="acba0-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="acba0-162">À l’aide de certificats de Client dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="acba0-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="acba0-163">Côté serveur, vous pouvez obtenir le certificat client en appelant [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) sur le message de demande.</span><span class="sxs-lookup"><span data-stu-id="acba0-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="acba0-164">La méthode retourne null s’il n’existe aucun certificat client.</span><span class="sxs-lookup"><span data-stu-id="acba0-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="acba0-165">Sinon, elle retourne un **X509Certificate2** instance.</span><span class="sxs-lookup"><span data-stu-id="acba0-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="acba0-166">Utilisez cet objet pour obtenir des informations à partir du certificat, telles que l’émetteur et le sujet.</span><span class="sxs-lookup"><span data-stu-id="acba0-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="acba0-167">Ensuite, vous pouvez utiliser ces informations pour l’authentification et/ou l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="acba0-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
