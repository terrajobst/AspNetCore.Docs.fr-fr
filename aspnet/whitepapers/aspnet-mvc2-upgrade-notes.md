---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: La mise à niveau une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Ce document décrit à la fois la mise à niveau avec un Assistant et manuellement une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2. Ce document est également disponible pour d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 3de69df7e80037de35c2609232f4574bc9d03c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836436"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>La mise à niveau une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2
====================
> Ce document décrit à la fois la mise à niveau avec un Assistant et manuellement une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2. Ce document est également disponible pour [télécharger](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Introduction

ASP.NET MVC 2 peut être installé côte à côte avec ASP.NET MVC 1.0 sur le même serveur. Flexibilité application aux développeurs de choisir quand mettre à niveau une application ASP.NET MVC 1.0 vers ASP.NET MVC 2.

Visual Studio 2010 inclut un Assistant que les mises à niveau des projets ASP.NET MVC 1.0 existants générés avec Visual Studio 2008 pour ASP.NET MVC 2. L’Assistant Mise à niveau est lancé en ouvrant un projet ASP.NET MVC 1.0 dans Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Mise à niveau de l’Assistant pour ASP.NET MVC 1.0 sur Visual Studio 2008 SP1

Pour mettre à niveau une application ASP.NET MVC 1.0 vers ASP.NET MVC 2 dans Visual Studio 2008 SP1, utilisez l’application MvcAppConverter (non pris en charge). Vous pouvez télécharger cette application à partir de l’URL suivante :

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Mise à niveau manuelle d’un projet ASP.NET MVC 1.0

Pour mettre manuellement à une application ASP.NET MVC 1.0 vers la version 2, procédez comme suit :

1. Effectuez une sauvegarde du projet existant.
2. Dans un éditeur de texte, ouvrez le fichier de projet (le fichier avec l’extension de fichier .csproj ou .vbproj) et recherchez l’élément ProjectTypeGuid. En tant que la valeur de cet élément, remplacez le GUID {603c0e0b-db56-11dc-be95-000d561079b0} avec {F85E285D-A4E0-4152-9332-AB1D724D3325}. Lorsque vous avez terminé, il se peut que la valeur de cet élément doit être comme suit : 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Dans le dossier racine d’application Web, modifiez le fichier Web.config. Recherchez System.Web.Mvc, Version = 1.0.0.0 et remplacez toutes les instances avec System.Web.Mvc, Version = 2.0.0.0.
4. Répétez l’étape précédente pour le fichier Web.config situé dans le dossier Views.
5. Ouvrez le projet à l’aide de Visual Studio, puis, dans **l’Explorateur de solutions**, développez le **références** nœud. Supprimez la référence à System.Web.Mvc (qui pointe vers l’assembly de la version 1.0). Ajoutez une référence à System.Web.Mvc (v2.0.0.0).
6. Ajoutez l’élément bindingRedirect suivant au fichier Web.config dans la racine de l’application sous la section de configuration :   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Créez une application ASP.NET MVC 2 vide. Copiez les fichiers à partir du dossier Scripts de la nouvelle application dans le dossier Scripts de l’application existante.
8. Mettre à jour le € d’application existant™ fichier CSS de s avec les définitions de style CSS dans le fichier Site.css.
9. Compilez l’application et exécutez-la. Si des erreurs se produisent, reportez-vous à la section de modifications avec rupture de la [What ' s New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.
