---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Partie 8 : Pages finales, gestion des exceptions et Conclusion | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 8 ajoute une page de contact, sur la page et l’exception...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f1d855e157f6a58995d301a793e660925767fb4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380251"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Partie 8 : Pages finales, gestion des exceptions et Conclusion
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 8 ajoute une page de contact, sur la page et la gestion des exceptions. Il s’agit de la conclusion de la série.


## <a id="_Toc260221680"></a>  Contactez Page (envoi de message électronique à partir d’ASP.NET)

Créer une page nommée ContactUs.aspx

À l’aide du concepteur, créez le formulaire suivant, en notant spécial pour inclure le ToolkitScriptManager et le contrôle d’édition à partir de la AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Double-cliquez sur le bouton « Submit » pour générer un gestionnaire d’événements dans le fichier code-behind et implémenter une méthode pour envoyer les informations de contact sous forme d’e-mail.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Ce code nécessite que votre fichier web.config contient une entrée dans la section de configuration qui spécifie le serveur SMTP à utiliser pour l’envoi du message électronique.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Sur la Page

Créez une page nommée AboutUs.aspx et ajoutez le contenu que vous le souhaitez.

## <a id="_Toc260221682"></a>  Gestionnaire d’exceptions global

Enfin, tout au long de l’application nous avons levée des exceptions et des circonstances imprévues autrement à froid également cause non prise en charge des exceptions dans notre application web.

Nous voulons jamais une exception non gérée à afficher pour un visiteur du site web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Outre une expérience utilisateur terrible les exceptions non gérées peuvent également être un problème de sécurité.

Pour résoudre ce problème, nous implémenterons un gestionnaire d’exceptions global.

Pour ce faire, ouvrez le fichier Global.asax et notez le Gestionnaire d’événements prégénérées.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Ajoutez du code pour implémenter l’Application\_Gestionnaire d’erreurs comme suit.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Ensuite, ajoutez une page nommée Error.aspx à la solution et ajoutez cet extrait de balisage.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Maintenant, dans la Page\_charger d’extraction de gestionnaire d’événements les messages d’erreur de l’objet Request.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Conclusion

Nous avons vu que Web Forms ASP.NET facilite la création d’un site Web sophistiqué avec un accès de base de données, l’appartenance, AJAX, etc. assez rapidement.

J’espère que ce didacticiel vous a donné les outils que vous avez besoin pour commencer à créer votre propre WebForms ASP.NET applications !

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-7.md)
