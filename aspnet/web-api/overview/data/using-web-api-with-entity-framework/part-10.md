---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publier l’application dans Azure App Service Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 5c1a70ceded85681046065881f62c5c95c5d8740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824487"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publier l’application dans Azure App Service Azure
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

La dernière étape, vous allez publier l’application dans Azure. Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **publier**.

![](part-10/_static/image1.png)

En cliquant sur **publier** appelle le **publier le site Web** boîte de dialogue. Si vous avez coché **hôte dans le Cloud** lorsque vous avez créé le projet, puis la connexion et paramètres sont déjà configurés. Dans ce cas, cliquez simplement sur le **paramètres** et vérifiez &quot;Execute Code First Migrations&quot;. (Si vous n’avez pas activé **hôte dans le Cloud** au début, puis suivez les étapes décrites dans le [section suivante](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Pour déployer l’application, cliquez sur **publier**. Vous pouvez afficher la progression de la publication dans le **activité de publication Web** fenêtre. (À partir de la **vue** menu, sélectionnez **Windows autres**, puis sélectionnez **activité de publication Web**.)

![](part-10/_static/image4.png)

Lorsque Visual Studio a terminé le déploiement de l’application, le navigateur par défaut ouvre automatiquement l’URL du site Web déployé, et l’application que vous avez créé est maintenant en cours d’exécution dans le cloud. L’URL dans la barre d’adresses du navigateur montre que le site est chargé à partir d’Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Déploiement vers un nouveau site Web

Si vous n’avez pas coché **hôte dans le Cloud** lorsque vous avez créé le projet, vous pouvez désormais configurer une nouvelle application web. Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **publier**. Sélectionnez le **profil** onglet et cliquez sur **sites Web Microsoft Azure**. Si vous n’êtes pas actuellement connecté à Azure, vous devrez vous connecter.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

Dans le **sites Web existants** boîte de dialogue, cliquez sur **New**.

![](part-10/_static/image9.png)

Entrez un nom de site. Sélectionnez votre abonnement Azure et la région. Sous **serveur de base de données**, sélectionnez **créer un nouveau serveur**, ou sélectionnez un serveur existant. Cliquez sur **Créer**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Cliquez sur le **paramètres** et vérifiez &quot;Execute Code First Migrations&quot;. Puis cliquez sur **publier**.

> [!div class="step-by-step"]
> [Précédent](part-9.md)
