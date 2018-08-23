---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET-accès refusé aux répertoires IIS | Microsoft Docs
author: rick-anderson
description: Ce livre blanc décrit la marche à suivre si une demande à votre application ASP.NET renvoie l’erreur « accès refusé au répertoire DirectoryName. Échec de s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: c3a14f51df7aaf5c5935cf60ee4e687c10048e91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831815"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET-accès refusé aux répertoires IIS
====================
> Ce livre blanc décrit la marche à suivre si une demande à votre application ASP.NET retourne l’erreur, « refuser l’accès à *NomRépertoire* directory. Échec de démarrage de l’analyse des modifications d’annuaire. »
> 
> S’applique à ASP.NET 1.0 et ASP.NET 1.1.


ASP.NET V1 RTM s’exécute désormais à l’aide d’une moins privilégié d’un compte windows - enregistrés en tant que le compte « ASPNET » sur un ordinateur local.

Sur certains systèmes verrouillés, ce compte peut pas par défaut ont accès en lecture sécurité pour les répertoires de contenu d’un site Web, le répertoire racine de l’application ou le répertoire racine du site web. Dans ce cas, vous recevrez l’erreur suivante lors de la demande des pages à partir d’une application web donnée :

![](denied-access-to-iis-directories/_static/image1.jpg)

Pour résoudre ce problème, vous devrez modifier les autorisations de sécurité sur les répertoires appropriés.

Plus précisément, ASP.NET nécessite une lecture, exécuter et liste d’accès pour le compte ASPNET pour la racine du site web (par exemple : c:\inetpub\wwwroot ou n’importe quel répertoire de site autre que vous avez peut-être configuré dans IIS), le répertoire de contenu et le répertoire racine de l’application pour surveiller les modifications de fichier de configuration. La racine de l’application correspond au chemin de dossier associé au répertoire virtuel d’application dans l’outil d’Administration IIS (inetmgr).

Par exemple, considérez la hiérarchie d’application suivante sous le dossier wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Pour cet exemple, le compte ASPNET doit les autorisations de lecture définies ci-dessus pour le contenu dans le myapp et le répertoire wwwroot. Une ACL unique ou sur le dossier racine peut également éventuellement servir pour les deux répertoires si ils sont imbriqués.

Pour ajouter des autorisations à un répertoire, procédez comme suit :

- À l’aide de l’Explorateur Windows, accédez au répertoire
- Cliquez avec le bouton droit sur le dossier du répertoire et choisissez « Propriétés »
- Accédez à l’onglet « Sécurité » dans la boîte de dialogue de propriété
- Cliquez sur le bouton « Ajouter » et entrez le nom d’ordinateur suivi par le nom du compte ASPNET. Par exemple, sur un ordinateur nommé « webdev », vous serez Entrez webdev\ASPNET et appuyez sur « OK ».
- Assurez-vous que le compte ASPNET dispose le « en lecture &amp; Execute », « Contenu du dossier » et « Lecture » les cases cochées.
- Appuyez sur OK pour fermer la boîte de dialogue et enregistrer les modifications.

![](denied-access-to-iis-directories/_static/image2.jpg)

Si vous le souhaitez, ces modifications peuvent être automatisées à l’aide de scripts ou l’outil « cacls.exe » qui est fourni avec Windows. Pour plus d’informations sur le compte ASPNET, veuillez consulter la [document FAQ](https://go.microsoft.com/fwlink/?LinkId=5828).

Si une application web donnée repose sur l’existence d’écriture ou modifier les autorisations pour un dossier particulier ou un fichier, celui-ci peut être octroyé en suivant la même procédure et en vérifiant les cases à cocher « Écriture » et/ou « Modifier ».

Ne rencontrerez aucun problème sur les ordinateurs qui permettent la lecture de groupe utilisateurs l’accès à ces répertoires (qui est la configuration par défaut) ou à tout le monde, et les étapes ci-dessus ne sera pas nécessaire.
