---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: Fichier ReadMe de ASP.NET Web Pages 2 Developer Preview | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 914e99491908294cea1e04dd23ccdb66c1dc05a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364428"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>Fichier ReadMe de ASP.NET Web Pages 2 Developer Preview
====================
par [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>Fichier ReadMe de ASP.NET Web Pages 2 Developer Preview

14 septembre 2011

### <a name="contents"></a>Sommaire

#### <a id="_Toc303701284"></a>  Notes d’installation

Pour installer Web Pages 2 Developer Preview, vous disposez des options suivantes :

- Installation à l’aide de WebMatrix 2 bêta le [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix est un ensemble d’outils de développement web gratuit qui inclut des Pages Web ASP.NET. Pour plus d’informations, consultez la section installation dans [fonctionnalités principales dans ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Installer directement à l’aide de Web Pages 2 Developer Preview le [lien de téléchargement](https://go.microsoft.com/fwlink/?LinkID=226335). Utilisez cette approche si vous souhaitez créer des applications de Pages Web à l’aide d’un texte éditeur tel que le bloc-notes. Pour exécuter des applications Web Pages 2, vous devez disposer d’IIS Express 7.5. (Cela est inclus automatiquement avec WebMatrix.) Pour obtenir des conseils sur la façon de tester une page Web Pages à l’aide d’IIS Express, consultez l’encadré « Création et test ASP.NET Pages à l’aide de votre propre éditeur de texte » dans [prise en main WebMatrix et ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview peut être installé et peut s’exécuter côte à côte avec ASP.NET Web Pages 1. <a id="a"></a>Pour plus d’informations, consultez la section « En cours d’exécution des Pages Web Applications côté à côte » dans [fonctionnalités principales dans Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Documentation

Didacticiels et autres informations sur les Pages Web ASP.NET sont disponibles sur la page du site Web ASP.NET Web Pages ([https://www.asp.net/web-pages/](../../index.md)). Pour plus d’informations sur les nouvelles fonctionnalités et améliorations dans les Pages Web 2, consultez [fonctionnalités principales dans Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Prise en charge

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Ceci est une version préliminaire et n’est pas officiellement pris en charge. Si vous avez des questions sur l’utilisation de cette version, les publier sur le forum ASP.NET Web Pages ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), où les membres de la Communauté ASP.NET peuvent souvent fournir un support informel.

#### <a id="_Toc303701287"></a>  Configuration logicielle requise

ASP.NET Web Pages 2 nécessite le .NET Framework 4. Elle fonctionne également avec la version de .NET Framework 4.5 Developer Preview.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Correctifs, les problèmes connus et les modifications avec rupture

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Est\* méthodes (par exemple, IsDateTime) maintenant retour valeurs correctes pour toutes les cultures.** Certaines méthodes telles que *IsDateTime* précédemment retourné *false* quand ils doivent avoir retournés *true* car ils ont été précédemment effectue les vérifications spécifiques à la culture. Ces méthodes ont été corrigés maintenant culture tenir compte. Il s’agit d’une modification avec rupture ; Si votre application s’appuie sur l’ancien comportement, il s’arrête.
- **Le comportement de la méthode Href a changé.** Auparavant, l’appelant Href("~/SomeFile") retournerait une URL relative au fichier en cours d’exécution. Maintenant Href("~/SomeFile") retourne toujours un chemin d’accès absolu à partir de la racine de l’application. Pour la plupart des cas, ce comportement ne faire une différence dans la valeur de retour. Cette modification a été effectuée pour résoudre certains scénarios Ajax. Par exemple, considérez l’exemple de code suivant : 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Ce code précédemment se présentera Images/Logo.jpg, qui serait incorrect pour une requête Ajax à cette page. Cela va maintenant résoudre à la racine de la (/ MySite/Images/Logo.jpg).
- **La méthode HttpContext.RedirectLocal a changé**. Cette méthode accepte désormais uniquement les URL sont relatives à l’application actuelle. URL complet sont rejetées.
- **La méthode ModelState.IsValid maintenant vous oblige à appeler d’abord valider**. Si vous convertissez votre application pour utiliser les nouvelles méthodes de validation d’entrée et que vous appelez le *ModelState.IsValid* (méthode), vous devez à présent appeler *Validation.Validate* au préalable. Par exemple, vous devez désormais suivre ce modèle : 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Toutefois, nous recommandons que si vous utilisez les nouvelles méthodes de validation d’entrée, n’utilisez pas *ModelState.IsValid*. Structure à la place, votre code comme suit : 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Validation côté client ne fonctionne pas sur Internet Explorer 7 et Internet Explorer 8,**. Validation côté client ne fonctionne pas en raison d’incompatibilités avec jQuery 1.6.2, qui est inclus dans le modèle de projet par défaut. (La validation côté serveur fonctionne.).

#### <a id="_Toc303701289"></a>  Exclusion de responsabilité

© 2011 Microsoft Corporation. Tous droits réservés. Ce document est fourni « en tant que-est. » Informations et opinions exprimées dans ce document, y compris les URL et autres références à des sites Web Internet, peuvent changer sans préavis. Vous assumez tous les risques liés à leur utilisation.
