---
uid: whitepapers/request-validation
title: Validation des demandes - prévention des attaques de Script | Microsoft Docs
author: rick-anderson
description: Cet article décrit la fonctionnalité de validation de demande d’ASP.NET où, par défaut, l’application ne peut pas traiter non codé soumettre des contenus de HTML...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0dfbfcae70792c57d530fc5e6fb73f8f96ec6e02
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809743"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="4da89-103">Validation des demandes - prévention des attaques de Script</span><span class="sxs-lookup"><span data-stu-id="4da89-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="4da89-104">Cet article décrit la fonctionnalité de validation de demande d’ASP.NET où, par défaut, l’application ne peut pas traiter du contenu HTML non codé envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="4da89-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="4da89-105">Cette fonctionnalité de validation de demande peut être désactivée lors de l’application a été conçue pour traiter en toute sécurité des données HTML.</span><span class="sxs-lookup"><span data-stu-id="4da89-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="4da89-106">S’applique à ASP.NET 1.1 et ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="4da89-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="4da89-107">Validation de la demande, une fonctionnalité d’ASP.NET depuis la version 1.1, empêche l’acceptation de contenu HTML non encodé contenant le serveur.</span><span class="sxs-lookup"><span data-stu-id="4da89-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="4da89-108">Cette fonctionnalité est conçue pour aider à éviter certaines attaques d’injection de script par laquelle le code de script client ou HTML peut être envoyée involontairement à un serveur, stockée et ensuite présenté à d’autres utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="4da89-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="4da89-109">Nous vous recommandons vivement que vous validez les données d’entrée et encoder en HTML, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="4da89-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="4da89-110">Par exemple, vous créez une page Web qui demande l’adresse de messagerie d’un utilisateur, puis stocke cette adresse dans une base de données de messagerie.</span><span class="sxs-lookup"><span data-stu-id="4da89-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="4da89-111">Si l’utilisateur entre &lt;SCRIPT&gt;alerte (« hello from script »)&lt;/SCRIPT&gt; au lieu d’une adresse e-mail valide, lorsque ces données sont présentées, ce script peut être exécuté si le contenu n’est pas encodé correctement.</span><span class="sxs-lookup"><span data-stu-id="4da89-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="4da89-112">La fonctionnalité de validation de demande d’ASP.NET permet d’éviter ce problème.</span><span class="sxs-lookup"><span data-stu-id="4da89-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="4da89-113">Pourquoi cette fonctionnalité est utile</span><span class="sxs-lookup"><span data-stu-id="4da89-113">Why this feature is useful</span></span>

<span data-ttu-id="4da89-114">De nombreux sites ne sont pas conscients qu’ils sont ouverts aux attaques par injection de script simple.</span><span class="sxs-lookup"><span data-stu-id="4da89-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="4da89-115">Si l’objectif de ces attaques est de modifier le site en affichant le code HTML ou potentiellement exécuter un script client pour rediriger l’utilisateur vers un site pirate, les attaques par injection de script sont un problème que vous doivent faire face les développeurs Web.</span><span class="sxs-lookup"><span data-stu-id="4da89-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="4da89-116">Les attaques par injection de script sont une préoccupation de tous les développeurs web, si elles sont à l’aide de ASP.NET, ASP ou autres technologies de développement web.</span><span class="sxs-lookup"><span data-stu-id="4da89-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="4da89-117">La fonctionnalité de validation de demande ASP.NET empêche proactive ces attaques en n’autorisant ne pas de contenu HTML non codé être traités par le serveur, sauf si le développeur décide d’autoriser que le contenu.</span><span class="sxs-lookup"><span data-stu-id="4da89-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="4da89-118">À quoi s’attendre : Page d’erreur</span><span class="sxs-lookup"><span data-stu-id="4da89-118">What to expect: Error Page</span></span>

<span data-ttu-id="4da89-119">La capture d’écran ci-dessous montre un exemple de code ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="4da89-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="4da89-120">Exécutez les résultats de ce code dans une page simple qui vous permet d’entrer du texte dans la zone de texte, cliquez sur le bouton et afficher le texte dans le contrôle d’étiquette :</span><span class="sxs-lookup"><span data-stu-id="4da89-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="4da89-121">Toutefois, ont été JavaScript, telles que `<script>alert("hello!")</script>` soit entré et soumis nous obtiendrions une exception :</span><span class="sxs-lookup"><span data-stu-id="4da89-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="4da89-122">Le message d’erreur indique qu’un « potentiellement dangereux Request.Form valeur a été détectée » et fournit plus de détails dans la description d’exactement ce qui s’est produite et comment modifier le comportement.</span><span class="sxs-lookup"><span data-stu-id="4da89-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="4da89-123">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4da89-123">For example:</span></span>

<span data-ttu-id="4da89-124">Validation de la demande a détecté une valeur d’entrée du client potentiellement dangereux et traitement de la demande a été abandonné.</span><span class="sxs-lookup"><span data-stu-id="4da89-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="4da89-125">Cette valeur peut indiquer une tentative de compromettre la sécurité de votre application, tel qu’une attaque de script entre sites.</span><span class="sxs-lookup"><span data-stu-id="4da89-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="4da89-126">Vous pouvez désactiver la validation de la demande en définissant `validateRequest=false` dans la directive de Page ou dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="4da89-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="4da89-127">Toutefois, il est fortement recommandé que votre application vérifie explicitement toutes les entrées dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="4da89-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="4da89-128">Désactivation de la validation de demande sur une page</span><span class="sxs-lookup"><span data-stu-id="4da89-128">Disabling request validation on a page</span></span>

<span data-ttu-id="4da89-129">Pour désactiver la validation de demande sur une page, vous devez définir le `validateRequest` attribut de la directive de Page à `false`:</span><span class="sxs-lookup"><span data-stu-id="4da89-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="4da89-130">Lors de la validation de la demande est désactivée, le contenu peut être soumis à une page ; C’est la responsabilité du développeur de page pour vous assurer que le contenu est correctement encodée ou traitée.</span><span class="sxs-lookup"><span data-stu-id="4da89-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="4da89-131">Désactivation de la validation de demande pour votre application</span><span class="sxs-lookup"><span data-stu-id="4da89-131">Disabling request validation for your application</span></span>

<span data-ttu-id="4da89-132">Pour désactiver la validation de la demande pour votre application, vous devez modifier ou créer un fichier Web.config pour votre application et définissez l’attribut validateRequest de la `<pages />` section à `false`:</span><span class="sxs-lookup"><span data-stu-id="4da89-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="4da89-133">Si vous souhaitez désactiver la validation de demande pour toutes les applications sur votre serveur, vous pouvez effectuer cette modification à votre fichier Machine.config.</span><span class="sxs-lookup"><span data-stu-id="4da89-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="4da89-134">Lors de la validation de la demande est désactivée, le contenu peut être envoyé à votre application ; C’est la responsabilité du développeur de l’application pour vous assurer que le contenu est correctement encodée ou traitée.</span><span class="sxs-lookup"><span data-stu-id="4da89-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="4da89-135">Le code ci-dessous est modifié pour désactiver la validation de demande :</span><span class="sxs-lookup"><span data-stu-id="4da89-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="4da89-136">Maintenant, si le code JavaScript suivant a été entré dans la zone de texte `<script>alert("hello!")</script>` le résultat serait :</span><span class="sxs-lookup"><span data-stu-id="4da89-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="4da89-137">Pour éviter ce problème, avec la validation de la demande mises hors tension, nous devons au format HTML encoder le contenu.</span><span class="sxs-lookup"><span data-stu-id="4da89-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="4da89-138">Comment coder du contenu au format HTML</span><span class="sxs-lookup"><span data-stu-id="4da89-138">How to HTML encode content</span></span>

<span data-ttu-id="4da89-139">Si vous avez désactivé la validation de la demande, il est conseillé de contenu à encoder en HTML qui est stocké pour une utilisation ultérieure.</span><span class="sxs-lookup"><span data-stu-id="4da89-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="4da89-140">L’encodage HTML remplacent automatiquement les «&lt;'ou'&gt;' (ainsi que plusieurs autres symboles) avec HTML correspondantes représentation encodée.</span><span class="sxs-lookup"><span data-stu-id="4da89-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="4da89-141">Par exemple, «&lt;« est remplacé par »&amp;lt ;' et '&gt;« est remplacé par »&amp;gt ;'.</span><span class="sxs-lookup"><span data-stu-id="4da89-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="4da89-142">Les navigateurs utilisent ces codes spéciaux pour afficher le «&lt;« ou »&gt;» dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="4da89-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="4da89-143">Contenu peut être facilement codées en HTML sur le serveur en utilisant le `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="4da89-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="4da89-144">Contenu peut également être facilement décodée en HTML, autrement dit, restauré vers HTML standard à l’aide du `Server.HtmlDecode(string)` (méthode).</span><span class="sxs-lookup"><span data-stu-id="4da89-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="4da89-145">Ce qui entraîne :</span><span class="sxs-lookup"><span data-stu-id="4da89-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
