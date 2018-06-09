---
title: Résoudre les problèmes pour ASP.NET Core
author: Rick-Anderson
description: Comprendre et résoudre les avertissements et erreurs avec les projets ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 64a353a9bdf0753c63f676f9d07f42ba45acdcab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217563"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Résoudre les problèmes des projets ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Les liens suivants fournissent des conseils de dépannage :

* [Résoudre les problèmes liés à ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Résoudre les problèmes liés à ASP.NET Core sur IIS](xref:host-and-deploy/iis/troubleshoot)
* [Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube : diagnostic des problèmes dans les applications ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Avertissements du Kit de développement .NET core

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Les versions de 64 bits du Kit de développement .NET Core et 32 bits sont installées.
Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant : 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [le Kit de développement .NET Core](https://www.microsoft.com/net/download/all) sont installés. Les deux versions peuvent être installées causes courantes :

* Vous initialement téléchargé le programme d’installation du Kit de développement .NET Core à l’aide d’un ordinateur 32 bits, mais puis copié sur et installez sur un ordinateur 64 bits. 
* Le Kit de développement 32 bits .NET Core a été installée par une autre application.
* Version incorrecte a été téléchargée et installée.

Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement. Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>Le Kit de développement .NET Core est installé dans plusieurs emplacements
Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant : 

 Le Kit de développement .NET Core est installé dans plusieurs emplacements. Seuls les modèles à partir de la SDK(s) installé sur « C:\Program Files\dotnet\sdk\' s’affiche.

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

Ce message s’affiche lorsque vous avez au moins une installation du Kit de développement .NET Core dans un répertoire en dehors de * C:\Program Files\dotnet\sdk\*. Cela se produit généralement lorsque le Kit de développement .NET Core a été déployée sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.

Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement. Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**. Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.

### <a name="no-net-core-sdks-were-detected"></a>Aucun kits de développement logiciel .NET Core ont été détectés.
Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant : 

**Aucun kits de développement logiciel .NET Core ont été détectés, assurez-vous qu’ils sont inclus dans la variable d’environnement « PATH »**

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/NoNetCore.png)

Cet avertissement s’affiche lorsque la variable d’environnement `PATH` ne pointe pas vers les kits de développement logiciel .NET Core sur l’ordinateur. Pour résoudre ce problème :

* Installer ou vérifiez que le Kit de développement .NET Core est installé.
* Vérifiez le `PATH` variable d’environnement pointe vers l’emplacement du Kit de développement logiciel est installé. Le programme d’installation définit normalement le `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a>Utilisation de IHtmlHelper.Partial peut générer des blocages d’application

Dans ASP.NET Core 2.1 et versions ultérieures, l’appel `Html.Partial` génère un avertissement de l’analyseur en raison du risque de blocages. Le message d’avertissement est :

*Utilisation de IHtmlHelper.Partial peut entraîner des blocages d’application. Envisagez d’utiliser `<partial>` application d’assistance de balise ou `IHtmlHelper.PartialAsync`.*

Les appels à `@Html.Partial` doit être remplacé par `@await Html.PartialAsync` ou l’application d’assistance d’étiquette partielle `<partial name="_Partial" />`.

::: moniker-end
