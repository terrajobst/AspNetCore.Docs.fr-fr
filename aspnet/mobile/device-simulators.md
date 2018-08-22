---
uid: mobile/device-simulators
title: Simuler des appareils mobiles courants pour le test | Microsoft Docs
author: rick-anderson
description: Vous pouvez télécharger des émulateurs pour les navigateurs et appareils mobiles courants en suivant les liens
ms.author: riande
ms.date: 01/28/2011
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 9498c370003d20ba0b8a835a8d4cd86961c3bf3c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828282"
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Simuler des appareils mobiles courants pour le test
====================
> Vous pouvez télécharger des émulateurs pour les navigateurs et appareils mobiles courants en suivant les liens


| Appareil ou navigateur | Émulateur / simulateur |
| --- | --- |
| BrowserStack hébergé virtualisation du navigateur ![BrowserStack hébergé virtualisation du navigateur](device-simulators/_static/image1.png) | [Virtualisation de navigateur hébergée BrowserStack](http://browserstack.com) tester votre environnement local ou de production dans n’importe quel navigateur sur n’importe quelle plateforme. Vous pouvez créer un tunnel entre votre ordinateur et le réseau BrowserStack dans votre propre ordinateur virtuel hébergé. Assurez-vous d’obtenir le [BrowserStack Visual Studio Extension](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) pour une expérience encore plus transparente. |
| Windows Phone | [Téléchargements du Kit de développement logiciel Windows Phone](https://dev.windowsphone.com/downloadsdk) le Windows Phone logiciel Kit de développement (SDK) inclut tous les outils dont vous avez besoin pour développer des applications et des jeux pour Windows Phone |
| iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone et iPad simulateurs pour Windows, mais aussi un réactif outil de conception. Peut intégrer avec l’option « Parcourir avec.. » de Visual Studio 2012. |
| Android | [Page d’accueil du Kit de développement logiciel Android](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | Dernières versions : [outils de développement Opera accueil](http://www.opera.com/developer/tools/) Opera Mini 4.2 : [simulateur basé sur Java en ligne](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Kit d’outils de développeur Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Notez que pour donner l’accès au réseau de téléphone, vous devez également l’adaptateur de réseau d’ordinateur virtuel inclus dans [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Pour vous connecter à Internet Explorer sur le téléphone à votre serveur de développement Visual Studio, consultez [billet de blog de Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Images d’émulateur pour Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Notez que si vous souhaitez afficher votre application sur un véritable appareil mobile (qui est la seule option pour des tests incomplets iPhone ou iPad, car il n’existe aucune véritable émulateur pour Windows), vous aurez besoin héberger votre application dans IIS ou IIS Express. Vous ne peut pas facilement utiliser le serveur web intégré de Visual Studio pour ce faire, car il ne répond pas aux demandes à partir d’autres ordinateurs.
