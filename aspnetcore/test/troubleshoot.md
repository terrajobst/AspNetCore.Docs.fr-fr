---
title: Résoudre les problèmes des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938392"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Résoudre les problèmes des projets ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Les liens suivants fournissent des conseils de dépannage :

* [Résoudre les problèmes liés à ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Résoudre les problèmes liés à ASP.NET Core sur IIS](xref:host-and-deploy/iis/troubleshoot)
* [Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Conférence NDC (Londres, 2018) : Diagnostic des problèmes dans les Applications ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog de l’ASP.NET : Résolution des problèmes de performances d’ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Avertissements du SDK .NET core

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 bits et 64 bits des versions du SDK .NET Core sont installées.

Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :

> 32 et 64 bits des versions du SDK .NET Core sont installées. Seuls les modèles à partir de l’ou les versions 64 bits installée sur ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [du SDK .NET Core](https://www.microsoft.com/net/download/all) sont installés. Raisons courantes, les deux versions peuvent être installées sont les suivantes :

* Vous initialement téléchargé le programme d’installation du SDK .NET Core à l’aide d’un ordinateur 32 bits, mais ensuite copié sur et installé sur un ordinateur 64 bits.
* Le SDK .NET Core de 32 bits a été installé par une autre application.
* Une version incorrecte a été téléchargée et installée.

Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement. Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>Le SDK .NET Core est installé dans plusieurs emplacements

Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :

> Le SDK .NET Core est installé dans plusieurs emplacements. Seuls les modèles des SDK installés à ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

Ce message s’affiche lorsque vous avez au moins une installation du SDK .NET Core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\sdk\\*. Cela se produit généralement lorsque le SDK .NET Core a été déployé sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.

Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement. Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="no-net-core-sdks-were-detected"></a>Aucun SDK .NET Core ont été détectées.

Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :

> Aucun SDK .NET Core ont été détectées, assurez-vous qu’ils sont inclus dans la variable d’environnement « PATH ».

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/NoNetCore.png)

Cet avertissement apparaît lorsque la variable d’environnement `PATH` ne pointe pas vers n’importe quel SDK .NET Core sur l’ordinateur. Pour résoudre ce problème :

* Installer ou vérifiez que le SDK .NET Core est installé.
* Vérifiez que le `PATH` variable d’environnement pointe vers l’emplacement dans lequel le SDK est installé. Le programme d’installation définit normalement le `PATH`.
