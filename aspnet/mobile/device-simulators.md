---
uid: mobile/device-simulators
title: Simuler des appareils mobiles courants pour le test | Microsoft Docs
author: rick-anderson
description: Vous pouvez télécharger des émulateurs pour les navigateurs et appareils mobiles courants en suivant les liens
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348375"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Simuler des appareils mobiles courants pour le test

> Vous pouvez télécharger des émulateurs pour les navigateurs et appareils mobiles courants en suivant les liens.

| Appareil ou navigateur | Émulateur / simulateur |
| --- | --- |
| BrowserStack hébergé virtualisation du navigateur ![BrowserStack hébergé virtualisation du navigateur](device-simulators/_static/image1.png) | [Virtualisation de navigateur hébergée BrowserStack](http://browserstack.com) tester votre environnement local ou de production dans n’importe quel navigateur sur n’importe quelle plateforme. Vous pouvez créer un tunnel entre votre ordinateur et le réseau BrowserStack dans votre propre ordinateur virtuel hébergé. Assurez-vous d’obtenir le [BrowserStack Visual Studio Extension](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) pour une expérience encore plus transparente. |
| iPhone / iPod / iPad | [Électrique Mobile Studio](http://www.electricplum.com/studio.aspx) iPhone et iPad simulateurs pour Windows, mais aussi un réactif outil de conception. |
| Android | [Android Studio](https://developer.android.com/studio/) ou [Visual Studio Emulator pour Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Émulateur Classic Mobile Opera](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Kit d’outils de développeur Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Notez que pour donner l’accès au réseau de téléphone, vous devez également l’adaptateur de réseau d’ordinateur virtuel inclus dans [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Pour vous connecter à Internet Explorer sur le téléphone à votre serveur de développement Visual Studio, consultez [billet de blog de Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> Si vous souhaitez afficher votre application sur un véritable appareil mobile (qui est la seule option pour des tests incomplets iPhone ou iPad, car il n’existe aucune véritable émulateur pour Windows), vous devez héberger votre application dans IIS ou IIS Express. Vous ne peut pas facilement utiliser le serveur web intégré de Visual Studio pour ce faire, car il ne répond pas aux demandes à partir d’autres ordinateurs.