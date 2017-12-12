---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: "À l’aide d’un test CAPTCHA pour empêcher des robots d’utiliser votre Razor ASP.NET Web) de Site | Documents Microsoft"
author: microsoft
description: "Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatiques (robots) d’effectuer des tâches dans une page Web ASP.NET (Razor) nous..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="19a3f-103">À l’aide d’un test CAPTCHA pour empêcher des robots d’utiliser votre Razor ASP.NET Web) de Site</span><span class="sxs-lookup"><span data-stu-id="19a3f-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="19a3f-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="19a3f-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="19a3f-105">Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatiques (robots) à effectuer des tâches dans un site Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="19a3f-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="19a3f-106">**Ce que vous allez apprendre :**</span><span class="sxs-lookup"><span data-stu-id="19a3f-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="19a3f-107">Comment ajouter un test CAPTCHA à votre site.</span><span class="sxs-lookup"><span data-stu-id="19a3f-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="19a3f-108">Voici les fonctionnalités d’ASP.NET introduites dans l’article :</span><span class="sxs-lookup"><span data-stu-id="19a3f-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="19a3f-109">Le `ReCaptcha` helper.</span><span class="sxs-lookup"><span data-stu-id="19a3f-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="19a3f-110">Les informations contenues dans cet article s’applique à la version 1.0 de Pages Web ASP.NET et Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="19a3f-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="19a3f-111">À propos de CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="19a3f-111">About CAPTCHAs</span></span>

<span data-ttu-id="19a3f-112">Chaque fois que vous permettent aux utilisateurs inscrire dans votre site, ou même à entrer un nom et une URL (par exemple, pour un commentaire de blog), vous risquez d’obtenir un flux excessif de noms fausses.</span><span class="sxs-lookup"><span data-stu-id="19a3f-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="19a3f-113">Ceux-ci sont souvent conservés par des programmes automatisés (robots) qui tentent de laisser les URL dans tous les sites Web qu’ils peuvent trouver.</span><span class="sxs-lookup"><span data-stu-id="19a3f-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="19a3f-114">(Surtout est pour valider les URL de produits pour la vente.)</span><span class="sxs-lookup"><span data-stu-id="19a3f-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="19a3f-115">Vous pouvez aider à vous assurer qu’un utilisateur est la personne et non un programme de l’ordinateur en utilisant un *CAPTCHA* pour valider les utilisateurs lorsqu’ils inscriront ou sinon entrer leur nom et le site.</span><span class="sxs-lookup"><span data-stu-id="19a3f-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="19a3f-116">CAPTCHA signifie test entièrement automatisée publique Turing indiquer les uns des autres utilisateurs et ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="19a3f-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="19a3f-117">Un test CAPTCHA est un *stimulation-réponse* test dans lequel l’utilisateur est invité à effectuer une opération facile pour une personne effectue mais difficile pour un programme automatisé à faire.</span><span class="sxs-lookup"><span data-stu-id="19a3f-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="19a3f-118">Le type le plus courant de CAPTCHA est un où vous consultez certaines lettres de distorsion et que vous êtes invité à les taper.</span><span class="sxs-lookup"><span data-stu-id="19a3f-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="19a3f-119">(La distorsion est prévu pour le rendre difficile robots à déchiffrer les lettres).</span><span class="sxs-lookup"><span data-stu-id="19a3f-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="19a3f-120">Ajout d’un Test de ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="19a3f-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="19a3f-121">Dans les pages ASP.NET, vous pouvez utiliser la `ReCaptcha` application d’assistance pour restituer un test CAPTCHA basé sur le service ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="19a3f-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="19a3f-122">Le `ReCaptcha` affiche une image de deux mots déformés dont disposent les utilisateurs à entrer correctement avant que la page est validée.</span><span class="sxs-lookup"><span data-stu-id="19a3f-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="19a3f-123">La réponse de l’utilisateur est validée par le service ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="19a3f-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="19a3f-124">Inscrire votre site Web, consultez ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="19a3f-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="19a3f-125">Lorsque vous avez terminé l’inscription, vous obtenez une clé publique et une clé privée.</span><span class="sxs-lookup"><span data-stu-id="19a3f-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="19a3f-126">Ajoutez la bibliothèque de programmes d’assistance ASP.NET Web à votre site Web, comme décrit dans [programmes d’assistance de l’installation dans un Site de Pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas encore.</span><span class="sxs-lookup"><span data-stu-id="19a3f-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="19a3f-127">Si vous n’avez pas déjà un  *\_AppStart.cshtml* de fichiers, dans le dossier racine d’un site Web, créez un fichier nommé  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="19a3f-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="19a3f-128">Ajoutez le code suivant `Recaptcha` les paramètres d’assistance dans le  *\_AppStart.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="19a3f-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="19a3f-129">Définir le `PublicKey` et `PrivateKey` propriétés à l’aide de vos propres clés publiques et privées.</span><span class="sxs-lookup"><span data-stu-id="19a3f-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="19a3f-130">Enregistrer le  *\_AppStart.cshtml* de fichiers et de le fermer.</span><span class="sxs-lookup"><span data-stu-id="19a3f-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="19a3f-131">Dans le dossier racine d’un site Web, créer une nouvelle page nommée *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="19a3f-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="19a3f-132">Remplacez le contenu existant avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="19a3f-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="19a3f-133">Exécutez le *Recaptcha.cshtml* page dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="19a3f-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="19a3f-134">Si le `PrivateKey` valeur est valide, la page affiche le contrôle ReCaptcha et un bouton.</span><span class="sxs-lookup"><span data-stu-id="19a3f-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="19a3f-135">Si vous n’aviez pas globalement dans définir les clés  *\_AppStart.html*, la page affiche une erreur.</span><span class="sxs-lookup"><span data-stu-id="19a3f-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="19a3f-136">Entrez les termes pour le test.</span><span class="sxs-lookup"><span data-stu-id="19a3f-136">Enter the words for the test.</span></span> <span data-ttu-id="19a3f-137">Si vous passez le test ReCaptcha, vous consultez un message à cet effet.</span><span class="sxs-lookup"><span data-stu-id="19a3f-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="19a3f-138">Dans le cas contraire, vous voyez un message d’erreur et le contrôle de ReCaptcha s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="19a3f-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="19a3f-139">Si votre ordinateur se trouve sur un domaine qui utilise le serveur proxy, vous devrez peut-être configurer le `defaultproxy` élément de la *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="19a3f-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="19a3f-140">L’exemple suivant montre un *Web.config* de fichiers avec le `defaultproxy` élément configuré pour activer le service ReCaptcha fonctionne.</span><span class="sxs-lookup"><span data-stu-id="19a3f-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="19a3f-141">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="19a3f-141">Additional Resources</span></span>


- [<span data-ttu-id="19a3f-142">Personnalisation du comportement de l’échelle du Site pour les Sites des Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="19a3f-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="19a3f-143">ReCaptcha site</span><span class="sxs-lookup"><span data-stu-id="19a3f-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
