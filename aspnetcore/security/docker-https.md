---
title: Hébergement d’images ASP.NET Core avec l’arrimeur sur HTTPs
author: rick-anderson
description: Découvrez comment héberger des images ASP.NET Core avec l’arrimeur sur HTTPs
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
no-loc:
- Let's Encrypt
uid: security/docker-https
ms.openlocfilehash: 47027033c0b7130f2d38d22c02a54945b2cc31b3
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358911"
---
# <a name="hosting-aspnet-core-images-with-docker-over-https"></a><span data-ttu-id="1be0e-103">Hébergement d’images ASP.NET Core avec l’arrimeur sur HTTPs</span><span class="sxs-lookup"><span data-stu-id="1be0e-103">Hosting ASP.NET Core images with Docker over HTTPS</span></span>

<span data-ttu-id="1be0e-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1be0e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1be0e-105">ASP.NET Core utilise le [protocole HTTPS par défaut](/aspnet/core/security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="1be0e-105">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="1be0e-106">[Https](https://en.wikipedia.org/wiki/HTTPS) s’appuie sur des [certificats](https://en.wikipedia.org/wiki/Public_key_certificate) pour l’approbation, l’identité et le chiffrement.</span><span class="sxs-lookup"><span data-stu-id="1be0e-106">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="1be0e-107">Ce document explique comment exécuter des images conteneur prégénérées avec HTTPs.</span><span class="sxs-lookup"><span data-stu-id="1be0e-107">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="1be0e-108">Consultez [développement d’Applications ASP.net core avec l’arrimeur sur https](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) pour les scénarios de développement.</span><span class="sxs-lookup"><span data-stu-id="1be0e-108">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="1be0e-109">Cet exemple requiert l' [ancrage 17,06](https://docs.docker.com/release-notes/docker-ce) ou une version ultérieure du [client dockr](https://www.docker.com/products/docker).</span><span class="sxs-lookup"><span data-stu-id="1be0e-109">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1be0e-110">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="1be0e-110">Prerequisites</span></span>

<span data-ttu-id="1be0e-111">Le [Kit de développement logiciel (SDK) .net Core 2,2](https://www.microsoft.com/net/download) ou version ultérieure est requis pour certaines des instructions contenues dans ce document.</span><span class="sxs-lookup"><span data-stu-id="1be0e-111">The [.NET Core 2.2 SDK](https://www.microsoft.com/net/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="1be0e-112">Certificats</span><span class="sxs-lookup"><span data-stu-id="1be0e-112">Certificates</span></span>

<span data-ttu-id="1be0e-113">Un certificat d’une [autorité de certification](https://wikipedia.org/wiki/Certificate_authority) est requis pour l' [Hébergement de production](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) pour un domaine.</span><span class="sxs-lookup"><span data-stu-id="1be0e-113">A certificate from a [certificate authority](https://wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span> <span data-ttu-id="1be0e-114">[Let's Encrypt](https://letsencrypt.org/) est une autorité de certification qui offre des certificats gratuits.</span><span class="sxs-lookup"><span data-stu-id="1be0e-114">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="1be0e-115">Ce document utilise des [certificats de développement auto-signés](https://en.wikipedia.org/wiki/Self-signed_certificate) pour héberger des images prégénérées sur `localhost`.</span><span class="sxs-lookup"><span data-stu-id="1be0e-115">This document uses [self-signed development certificates](https://en.wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="1be0e-116">Les instructions sont similaires à l’utilisation de certificats de production.</span><span class="sxs-lookup"><span data-stu-id="1be0e-116">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="1be0e-117">Pour les certificats de production :</span><span class="sxs-lookup"><span data-stu-id="1be0e-117">For production certs:</span></span>

* <span data-ttu-id="1be0e-118">L’outil `dotnet dev-certs` n’est pas requis.</span><span class="sxs-lookup"><span data-stu-id="1be0e-118">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="1be0e-119">Les certificats n’ont pas besoin d’être stockés à l’emplacement utilisé dans les instructions.</span><span class="sxs-lookup"><span data-stu-id="1be0e-119">Certificates do not need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="1be0e-120">Tout emplacement doit fonctionner, bien que le stockage des certificats dans votre répertoire de site ne soit pas recommandé.</span><span class="sxs-lookup"><span data-stu-id="1be0e-120">Any location should work, although storing certs within your site directory is not recommended.</span></span>

<span data-ttu-id="1be0e-121">Instructions de montage de certificats de volume dans des conteneurs.</span><span class="sxs-lookup"><span data-stu-id="1be0e-121">The instructions volume mount certificates into containers.</span></span> <span data-ttu-id="1be0e-122">Vous pouvez ajouter des certificats dans des images de conteneur à l’aide d’une commande `COPY` dans un *fichier dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="1be0e-122">You can add certificates into container images with a `COPY` command in a *Dockerfile*.</span></span> <span data-ttu-id="1be0e-123">La copie de certificats dans une image n’est pas recommandée pour les raisons suivantes :</span><span class="sxs-lookup"><span data-stu-id="1be0e-123">Copying certificates into an image isn't recommended for the following reasons:</span></span>

* <span data-ttu-id="1be0e-124">Il est difficile d’utiliser la même image pour le test avec des certificats de développeur.</span><span class="sxs-lookup"><span data-stu-id="1be0e-124">It makes difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="1be0e-125">Il est difficile d’utiliser la même image pour l’hébergement avec des certificats de production.</span><span class="sxs-lookup"><span data-stu-id="1be0e-125">It makes difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="1be0e-126">Il y a un risque significatif de divulgation de certificats.</span><span class="sxs-lookup"><span data-stu-id="1be0e-126">There is significant risk of certificate disclosure.</span></span>

## <a name="running-pre-built-container-images-with-https"></a><span data-ttu-id="1be0e-127">Exécution d’images conteneur prégénérées avec HTTPs</span><span class="sxs-lookup"><span data-stu-id="1be0e-127">Running pre-built container images with HTTPS</span></span>

<span data-ttu-id="1be0e-128">Utilisez les instructions suivantes pour la configuration de votre système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="1be0e-128">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="1be0e-129">Windows utilisant des conteneurs Linux</span><span class="sxs-lookup"><span data-stu-id="1be0e-129">Windows using Linux containers</span></span>

<span data-ttu-id="1be0e-130">Générer un certificat et configurer l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="1be0e-130">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="1be0e-131">Dans les commandes précédentes, remplacez `{ password here }` par un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1be0e-131">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="1be0e-132">Exécutez l’image de conteneur avec ASP.NET Core configuré pour le protocole HTTPs :</span><span class="sxs-lookup"><span data-stu-id="1be0e-132">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="1be0e-133">Le mot de passe doit correspondre au mot de passe utilisé pour le certificat.</span><span class="sxs-lookup"><span data-stu-id="1be0e-133">The password must match the password used for the certificate.</span></span>

### <a name="macos-or-linux"></a><span data-ttu-id="1be0e-134">macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="1be0e-134">macOS or Linux</span></span>

<span data-ttu-id="1be0e-135">Générer un certificat et configurer l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="1be0e-135">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="1be0e-136">`dotnet dev-certs https --trust` est pris en charge uniquement sur macOS et Windows.</span><span class="sxs-lookup"><span data-stu-id="1be0e-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="1be0e-137">Vous devez faire confiance aux certificats sur Linux de la manière prise en charge par votre distribution.</span><span class="sxs-lookup"><span data-stu-id="1be0e-137">You need to trust certs on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="1be0e-138">Il est probable que vous ayez besoin d’approuver le certificat dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="1be0e-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="1be0e-139">Dans les commandes précédentes, remplacez `{ password here }` par un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1be0e-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="1be0e-140">Exécutez l’image de conteneur avec ASP.NET Core configuré pour le protocole HTTPs :</span><span class="sxs-lookup"><span data-stu-id="1be0e-140">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v ${HOME}/.aspnet/https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="1be0e-141">Le mot de passe doit correspondre au mot de passe utilisé pour le certificat.</span><span class="sxs-lookup"><span data-stu-id="1be0e-141">The password must match the password used for the certificate.</span></span>

### <a name="windows-using-windows-containers"></a><span data-ttu-id="1be0e-142">Windows utilisant des conteneurs Windows</span><span class="sxs-lookup"><span data-stu-id="1be0e-142">Windows using Windows containers</span></span>

<span data-ttu-id="1be0e-143">Générer un certificat et configurer l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="1be0e-143">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="1be0e-144">Dans les commandes précédentes, remplacez `{ password here }` par un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="1be0e-144">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="1be0e-145">Exécutez l’image de conteneur avec ASP.NET Core configuré pour le protocole HTTPs :</span><span class="sxs-lookup"><span data-stu-id="1be0e-145">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=\https\aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:C:\https\ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="1be0e-146">Le mot de passe doit correspondre au mot de passe utilisé pour le certificat.</span><span class="sxs-lookup"><span data-stu-id="1be0e-146">The password must match the password used for the certificate.</span></span>
