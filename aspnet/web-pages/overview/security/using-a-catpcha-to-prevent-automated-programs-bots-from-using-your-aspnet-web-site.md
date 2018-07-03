---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Site à l’aide d’un test CAPTCHA pour empêcher des robots d’utiliser votre Razor Web ASP.NET) | Microsoft Docs
author: microsoft
description: Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés (robots) d’effectuer des tâches dans un ASP.NET Web Pages (Razor) nous...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: cfdc8561f8ee4b97cc1ef2111113846a1c579d5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389070"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Site à l’aide d’un test CAPTCHA pour empêcher des robots d’utiliser votre Razor Web ASP.NET)
====================
par [Microsoft](https://github.com/microsoft)

> Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés (robots) d’effectuer des tâches dans un site Web ASP.NET Web Pages (Razor).
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment ajouter un test CAPTCHA à votre site.
> 
> Voici les fonctionnalités d’ASP.NET introduites dans l’article :
> 
> - Le `ReCaptcha` helper.
> 
> > [!NOTE]
> > Les informations contenues dans cet article s’applique à ASP.NET Web Pages 1.0 et Pages Web 2.


## <a name="about-captchas"></a>À propos de CAPTCHAs

N’importe quel moment que vous permettent de s’inscrire dans votre site, ou même simplement entrer un nom et une URL (comme pour un commentaire de blog), vous risquez d’obtenir un déluge de faux noms. Ceux-ci sont souvent laissés par les programmes automatisés (robots) qui tentent de laisser les URL dans tous les sites Web qu’ils trouveront. (Surtout consiste à publier les URL de produits pour la vente.)

Vous contribuez à garantir qu’un utilisateur est la personne réelle et non un programme de l’ordinateur en utilisant un *CAPTCHA* pour valider les utilisateurs lorsqu’ils s’inscrire ou sinon entrent leur nom et le site. Représente le CAPTCHA de test entièrement automatisée publique Turing indiquer les ordinateurs et les uns des autres êtres humains. Un test CAPTCHA est un *stimulation / réponse* test dans lequel l’utilisateur est invité à faire quelque chose qui est facile pour une personne à faire mais difficile pour un programme automatisé à faire. Le type le plus courant de CAPTCHA est un où vous consultez certaines lettres déformées et que vous êtes invité à les entrer. (La distorsion est supposée pour le rendre difficile pour les robots à déchiffrer les lettres).

## <a name="adding-a-recaptcha-test"></a>Ajout d’un Test ReCaptcha

Dans les pages ASP.NET, vous pouvez utiliser la `ReCaptcha` helper pour restituer un test CAPTCHA qui repose sur le service ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). Le `ReCaptcha` helper affiche une image de deux mots déformés dont disposent les utilisateurs à entrer correctement avant que la page est validée. La réponse de l’utilisateur est validée par le service ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Inscrire votre site Web à ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Lorsque vous avez terminé l’inscription, vous obtiendrez une clé publique et une clé privée.
2. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà.
3. Si vous n’avez pas déjà un  *\_AppStart.cshtml* de fichiers, dans le dossier racine d’un site Web, créez un fichier nommé  *\_AppStart.cshtml*.
4. Ajoutez le code suivant `Recaptcha` paramètres d’assistance dans le  *\_AppStart.cshtml* fichier : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Définir le `PublicKey` et `PrivateKey` propriétés à l’aide de vos propres clés publiques et privées.
6. Enregistrer le  *\_AppStart.cshtml* fichier et fermez-le.
7. Dans le dossier racine d’un site Web, créez la nouvelle page nommée *Recaptcha.cshtml*.
8. Remplacez le contenu existant par le suivant : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Exécutez le *Recaptcha.cshtml* page dans un navigateur. Si le `PrivateKey` valeur n’est valide, la page affiche le contrôle ReCaptcha et un bouton. Si vous n’aviez pas défini les clés dans le monde entier en  *\_AppStart.html*, la page affiche une erreur. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Entrez les mots pour le test. Si vous passez le test ReCaptcha, affiche un message à cet effet. Sinon, vous voyez un message d’erreur et le contrôle ReCaptcha est réaffiché.

> [!NOTE]
> Si votre ordinateur se trouve sur un domaine qui utilise le serveur proxy, vous devrez peut-être configurer le `defaultproxy` élément de la *Web.config* fichier. L’exemple suivant montre un *Web.config* de fichiers avec le `defaultproxy` élément configuré pour activer le service ReCaptcha fonctionne.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


- [Personnalisation du comportement de l’échelle du Site pour les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Site de ReCaptcha](https://www.google.com/recaptcha)
