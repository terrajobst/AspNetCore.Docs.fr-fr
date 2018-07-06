---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Création d’une contrainte de Route personnalisée (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisé. Nous implémentons un simple contrainte personnalisée qui empêche un itinéraire mis en correspondance w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 72389d11467cbf7baea4cc9452266edb8ab81125
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840466"
---
<a name="creating-a-custom-route-constraint-vb"></a>Création d’une contrainte de Route personnalisée (VB)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisé. Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire à partir de la mise en correspondance lors de l’exécution d’une demande de navigateur à partir d’un ordinateur distant.


L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer une contrainte d’itinéraire personnalisé. Une contrainte d’itinéraire personnalisé vous pouvez ainsi empêcher un itinéraire à partir de la mise en correspondance, sauf si une condition personnalisée est mis en correspondance.

Dans ce didacticiel, créons une contrainte de route de Localhost. La contrainte d’itinéraire Localhost correspond uniquement aux requêtes effectuées à partir de l’ordinateur local. Les demandes à distance à partir de sur Internet ne correspondent pas.

Vous implémentez une contrainte d’itinéraire personnalisée en implémentant l’interface IRouteConstraint. Il s’agit d’une interface très simple qui décrit une méthode unique :

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

La méthode retourne une valeur booléenne. Si vous retournez la valeur False, l’itinéraire associé à la contrainte ne correspondra à la demande de navigateur.

La contrainte de Localhost est contenue dans le Listing 1.

**Liste 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

La contrainte dans le Listing 1 tire parti de la propriété IsLocal exposée par la classe HttpRequest. Cette propriété retourne la valeur true lorsque l’adresse IP de la demande est soit 127.0.0.1 ou lorsque l’adresse IP de la demande est identique à l’adresse IP du serveur.

Vous utilisez une contrainte personnalisée au sein d’un itinéraire défini dans le fichier Global.asax. Le fichier Global.asax dans le Listing 2 utilise la contrainte de Localhost pour empêcher toute personne demandant une page d’administration, à moins qu’ils envoient la demande à partir du serveur local. Par exemple, une demande de /Admin/DeleteAll échouera lorsque effectuées à partir d’un serveur distant.

**Listing 2 - Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

La contrainte de Localhost est utilisée dans la définition de l’itinéraire de l’administrateur. Cet itinéraire ne doit correspondre à une demande de navigateur à distance. Sachez, cependant, que les autres itinéraires définis dans Global.asax peuvent correspondent à la même demande. Il est important de comprendre qu’une contrainte empêche un itinéraire particulier à partir d’une demande de mise en correspondance, et pas tous les itinéraires définis dans le fichier Global.asax.

Notez que l’itinéraire par défaut a été commenté à partir du fichier Global.asax dans le Listing 2. Si vous incluez l’itinéraire par défaut, l’itinéraire par défaut serait correspond aux requêtes pour le contrôleur de l’administrateur. Dans ce cas, les utilisateurs distants peuvent toujours invoquer des actions du contrôleur Admin même si leurs demandes ne correspondent pas l’itinéraire de l’administrateur.

> [!div class="step-by-step"]
> [Précédent](creating-a-route-constraint-vb.md)
