---
uid: webhooks/source
title: "Code source de WebHooks d’ASP.NET et les packages NuGet | Documents Microsoft"
author: rick-anderson
description: "Liens vers le code source de WebHooks d’ASP.NET et les packages NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="a015b-103">Code source de WebHooks d’ASP.NET et les packages NuGet</span><span class="sxs-lookup"><span data-stu-id="a015b-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="a015b-104">Microsoft ASP.NET WebHooks fait partie de la famille Microsoft ASP.NET des modules et est hébergé comme une [ouvrir le projet Source sur GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="a015b-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="a015b-105">Cela signifie que nous acceptons les contributions, mais que vous regardez le [Contribution instructions](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) avant de soumettre une requête de tirage.</span><span class="sxs-lookup"><span data-stu-id="a015b-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="a015b-106">Cette documentation en ligne qui vous lisez maintenant est également hébergée en tant que [Open Source sur GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) et accepte également les contributions.</span><span class="sxs-lookup"><span data-stu-id="a015b-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="a015b-107">Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="a015b-107">Nuget Packages</span></span>

<span data-ttu-id="a015b-108">Microsoft ASP.NET WebHooks est également disponible comme afficher un aperçu de ce qui signifie que vous devez sélectionner l’indicateur de version préliminaire dans Visual Studio afin des pour voir les packages Nuget.</span><span class="sxs-lookup"><span data-stu-id="a015b-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="a015b-109">Le [les packages Nuget](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sont devided en trois parties :</span><span class="sxs-lookup"><span data-stu-id="a015b-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="a015b-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un package commun qui est partagé entre les expéditeurs et destinataires.</span><span class="sxs-lookup"><span data-stu-id="a015b-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="a015b-111">[Expéditeur](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un ensemble de packages prenant en charge l’envoi de vos propres WebHooks à d’autres personnes.</span><span class="sxs-lookup"><span data-stu-id="a015b-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="a015b-112">La fonctionnalité pour l’envoi de WebHooks est décrite plus en détail dans [envoi le WebHooks](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="a015b-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="a015b-113">[Récepteurs](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un ensemble de packages prenant en charge la réception de WebHooks à partir d’autres.</span><span class="sxs-lookup"><span data-stu-id="a015b-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="a015b-114">La fonctionnalité de réception des WebHooks est décrite plus en détail dans [recevoir le WebHooks](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="a015b-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
