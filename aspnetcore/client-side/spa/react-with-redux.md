---
title: Utiliser le modèle de projet React-with-Redux avec ASP.NET Core
author: SteveSandersonMS
description: Découvrez comment démarrer avec le modèle de projet d’application monopage ASP.NET Core pour React-with-Redux et create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291482"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Utiliser le modèle de projet React-with-Redux avec ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Cette documentation ne concerne pas le modèle de projet React-with-Redux inclus dans ASP.NET Core 2.0. Elle traite du nouveau modèle React-with-Redux vers lequel vous pouvez effectuer une mise à jour manuellement. Par défaut, le modèle est inclus dans ASP.NET Core 2.1.

::: moniker-end

Le modèle de projet React-with-Redux mis à jour fournit un point de départ pratique pour les applications ASP.NET Core utilisant les conventions React, Redux et [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) pour implémenter une interface utilisateur (IU) côté client enrichie.

À l’exception de la commande de création de projet, toutes les informations sur le modèle React-with-Redux sont les mêmes que celles relatives au modèle React. Pour créer ce type de projet, exécutez `dotnet new reactredux` au lieu de `dotnet new react`. Pour plus d’informations sur les fonctionnalités communes aux deux modèles basés sur React, consultez la [documentation relative aux modèles React](xref:spa/react).
