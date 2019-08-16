---
title: ASP.NET Core la gestion de l’État éblouissant
author: guardrex
description: Découvrez comment rendre l’état persistant dans les applications côté serveur éblouissantes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/state-management
ms.openlocfilehash: af040635302fbf2dae8192dcf37d55bfcfedfcec
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030368"
---
# <a name="aspnet-core-blazor-state-management"></a><span data-ttu-id="26b5a-103">ASP.NET Core la gestion de l’État éblouissant</span><span class="sxs-lookup"><span data-stu-id="26b5a-103">ASP.NET Core Blazor state management</span></span>

<span data-ttu-id="26b5a-104">Par [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="26b5a-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="26b5a-105">Le côté serveur de éblouissant est une infrastructure d’application avec état.</span><span class="sxs-lookup"><span data-stu-id="26b5a-105">Blazor server-side is a stateful app framework.</span></span> <span data-ttu-id="26b5a-106">La plupart du temps, l’application maintient une connexion continue au serveur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-106">Most of the time, the app maintains an ongoing connection to the server.</span></span> <span data-ttu-id="26b5a-107">L’état de l’utilisateur est conservé dans la mémoire du serveur dans un *circuit*.</span><span class="sxs-lookup"><span data-stu-id="26b5a-107">The user's state is held in the server's memory in a *circuit*.</span></span> 

<span data-ttu-id="26b5a-108">Voici des exemples d’État détenu pour le circuit d’un utilisateur:</span><span class="sxs-lookup"><span data-stu-id="26b5a-108">Examples of state held for a user's circuit include:</span></span>

* <span data-ttu-id="26b5a-109">Interface utilisateur&mdash;du rendu hiérarchie des instances de composant et leur sortie de rendu la plus récente.</span><span class="sxs-lookup"><span data-stu-id="26b5a-109">The rendered UI&mdash;the hierarchy of component instances and their most recent render output.</span></span>
* <span data-ttu-id="26b5a-110">Valeurs de tous les champs et propriétés des instances de composant.</span><span class="sxs-lookup"><span data-stu-id="26b5a-110">The values of any fields and properties in component instances.</span></span>
* <span data-ttu-id="26b5a-111">Données conservées dans des instances de service d' [injection de dépendance (di)](xref:fundamentals/dependency-injection) dont l’étendue correspond au circuit.</span><span class="sxs-lookup"><span data-stu-id="26b5a-111">Data held in [dependency injection (DI)](xref:fundamentals/dependency-injection) service instances that are scoped to the circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="26b5a-112">Cet article traite de la persistance de l’État dans les applications côté serveur éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="26b5a-112">This article addresses state persistence in Blazor server-side apps.</span></span> <span data-ttu-id="26b5a-113">Les applications côté client éblouissantes peuvent tirer parti de [la persistance de l’État côté client dans le navigateur,](#client-side-in-the-browser) mais elles nécessitent des solutions personnalisées ou des packages tiers au-delà du cadre de cet article.</span><span class="sxs-lookup"><span data-stu-id="26b5a-113">Blazor client-side apps can take advantage of [client-side state persistence in the browser](#client-side-in-the-browser) but require custom solutions or 3rd party packages beyond the scope of this article.</span></span>

## <a name="blazor-circuits"></a><span data-ttu-id="26b5a-114">Circuits éblouissants</span><span class="sxs-lookup"><span data-stu-id="26b5a-114">Blazor circuits</span></span>

<span data-ttu-id="26b5a-115">Si un utilisateur subit une perte de connexion réseau temporaire, éblouissant tente de reconnecter l’utilisateur à son circuit d’origine afin qu’il puisse continuer à utiliser l’application.</span><span class="sxs-lookup"><span data-stu-id="26b5a-115">If a user experiences a temporary network connection loss, Blazor attempts to reconnect the user to their original circuit so they can continue to use the app.</span></span> <span data-ttu-id="26b5a-116">Toutefois, la reconnexion d’un utilisateur à son circuit d’origine dans la mémoire du serveur n’est pas toujours possible:</span><span class="sxs-lookup"><span data-stu-id="26b5a-116">However, reconnecting a user to their original circuit in the server's memory isn't always possible:</span></span>

* <span data-ttu-id="26b5a-117">Le serveur ne peut pas conserver un circuit déconnecté de façon infinie.</span><span class="sxs-lookup"><span data-stu-id="26b5a-117">The server can't retain a disconnected circuit forever.</span></span> <span data-ttu-id="26b5a-118">Le serveur doit libérer un circuit déconnecté après un délai d’attente ou lorsque le serveur subit une sollicitation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="26b5a-118">The server must release a disconnected circuit after a timeout or when the server is under memory pressure.</span></span>
* <span data-ttu-id="26b5a-119">Dans les environnements de déploiement multiserveur avec équilibrage de charge, toutes les demandes de traitement de serveur peuvent devenir indisponibles à un moment donné.</span><span class="sxs-lookup"><span data-stu-id="26b5a-119">In multiserver, load-balanced deployment environments, any server processing requests may become unavailable at any given time.</span></span> <span data-ttu-id="26b5a-120">Les serveurs individuels peuvent échouer ou être automatiquement supprimés lorsqu’il n’est plus nécessaire de gérer le volume global de demandes.</span><span class="sxs-lookup"><span data-stu-id="26b5a-120">Individual servers may fail or be automatically removed when no longer required to handle the overall volume of requests.</span></span> <span data-ttu-id="26b5a-121">Le serveur d’origine n’est peut-être pas disponible lorsque l’utilisateur tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="26b5a-121">The original server may not be available when the user attempts to reconnect.</span></span>
* <span data-ttu-id="26b5a-122">L’utilisateur peut fermer et rouvrir son navigateur ou recharger la page, ce qui supprime tout État contenu dans la mémoire du navigateur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-122">The user might close and re-open their browser or reload the page, which removes any state held in the browser's memory.</span></span> <span data-ttu-id="26b5a-123">Par exemple, les valeurs définies par le biais d’appels Interop JavaScript sont perdues.</span><span class="sxs-lookup"><span data-stu-id="26b5a-123">For example, values set through JavaScript interop calls are lost.</span></span>

<span data-ttu-id="26b5a-124">Lorsqu’un utilisateur ne peut pas être reconnecté à son circuit d’origine, l’utilisateur reçoit un nouveau circuit avec un état vide.</span><span class="sxs-lookup"><span data-stu-id="26b5a-124">When a user can't be reconnected to their original circuit, the user receives a new circuit with an empty state.</span></span> <span data-ttu-id="26b5a-125">Cela équivaut à fermer et à rouvrir une application de bureau.</span><span class="sxs-lookup"><span data-stu-id="26b5a-125">This is equivalent to closing and re-opening a desktop app.</span></span>

## <a name="preserve-state-across-circuits"></a><span data-ttu-id="26b5a-126">Conserver l’état entre les circuits</span><span class="sxs-lookup"><span data-stu-id="26b5a-126">Preserve state across circuits</span></span>

<span data-ttu-id="26b5a-127">Dans certains scénarios, il est souhaitable de conserver l’état entre les circuits.</span><span class="sxs-lookup"><span data-stu-id="26b5a-127">In some scenarios, preserving state across circuits is desirable.</span></span> <span data-ttu-id="26b5a-128">Une application peut conserver des données importantes pour un utilisateur dans les cas suivants:</span><span class="sxs-lookup"><span data-stu-id="26b5a-128">An app can retain important data for a user if:</span></span>

* <span data-ttu-id="26b5a-129">Le serveur Web n’est plus disponible.</span><span class="sxs-lookup"><span data-stu-id="26b5a-129">The web server becomes unavailable.</span></span>
* <span data-ttu-id="26b5a-130">Le navigateur de l’utilisateur est obligé de démarrer un nouveau circuit avec un nouveau serveur Web.</span><span class="sxs-lookup"><span data-stu-id="26b5a-130">The user's browser is forced to start a new circuit with a new web server.</span></span>

<span data-ttu-id="26b5a-131">En règle générale, la conservation de l’état entre les circuits s’applique aux scénarios où les utilisateurs créent activement des données, et non simplement à la lecture des données qui existent déjà.</span><span class="sxs-lookup"><span data-stu-id="26b5a-131">In general, maintaining state across circuits applies to scenarios where users are actively creating data, not simply reading data that already exists.</span></span>

<span data-ttu-id="26b5a-132">Pour conserver l’État au-delà d’un seul circuit, *ne stockez pas simplement les données dans la mémoire du serveur*.</span><span class="sxs-lookup"><span data-stu-id="26b5a-132">To preserve state beyond a single circuit, *don't merely store the data in the server's memory*.</span></span> <span data-ttu-id="26b5a-133">L’application doit conserver les données dans un autre emplacement de stockage.</span><span class="sxs-lookup"><span data-stu-id="26b5a-133">The app must persist the data to some other storage location.</span></span> <span data-ttu-id="26b5a-134">La persistance de l'&mdash;État n’est pas automatique. vous devez prendre des mesures lors du développement de l’application pour implémenter la persistance des données avec état.</span><span class="sxs-lookup"><span data-stu-id="26b5a-134">State persistence isn't automatic&mdash;you must take steps when developing the app to implement stateful data persistence.</span></span>

<span data-ttu-id="26b5a-135">La persistance des données est généralement requise uniquement pour l’état de valeur élevée que les utilisateurs ont consacrés à la création.</span><span class="sxs-lookup"><span data-stu-id="26b5a-135">Data persistence is typically only required for high-value state that users have expended effort to create.</span></span> <span data-ttu-id="26b5a-136">Dans les exemples suivants, l’état persistant fait gagner du temps ou contribue à des activités commerciales:</span><span class="sxs-lookup"><span data-stu-id="26b5a-136">In the following examples, persisting state either saves time or aids in commercial activities:</span></span>

* <span data-ttu-id="26b5a-137">WebForm &ndash; à plusieurs étapes il prend beaucoup de temps pour qu’un utilisateur saisit à nouveau les données pour plusieurs étapes terminées d’un processus à plusieurs étapes si leur état est perdu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-137">Multistep webform &ndash; It's time-consuming for a user to re-enter data for several completed steps of a multistep process if their state is lost.</span></span> <span data-ttu-id="26b5a-138">Un utilisateur perd l’État dans ce scénario s’il quitte le formulaire à étapes et retourne au formulaire par la suite.</span><span class="sxs-lookup"><span data-stu-id="26b5a-138">A user loses state in this scenario if they navigate away from the multistep form and return to the form later.</span></span>
* <span data-ttu-id="26b5a-139">Panier &ndash; tout composant commercial important d’une application qui représente un chiffre d’affaires potentiel peut être maintenu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-139">Shopping cart &ndash; Any commercially important component of an app that represents potential revenue can be maintained.</span></span> <span data-ttu-id="26b5a-140">Un utilisateur qui perd son état et, par conséquent, son panier, peut acheter moins de produits ou de services lorsqu’ils reviennent sur le site ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="26b5a-140">A user who loses their state, and thus their shopping cart, may purchase fewer products or services when they return to the site later.</span></span>

<span data-ttu-id="26b5a-141">En règle générale, il n’est pas nécessaire de conserver un État facile à recréer, tel que le nom d’utilisateur entré dans une boîte de dialogue de connexion qui n’a pas été envoyée.</span><span class="sxs-lookup"><span data-stu-id="26b5a-141">It's usually not necessary to preserve easily-recreated state, such as the username entered into a sign-in dialog that hasn't been submitted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="26b5a-142">Une application peut conserver uniquement l’état de l' *application*.</span><span class="sxs-lookup"><span data-stu-id="26b5a-142">An app can only persist *app state*.</span></span> <span data-ttu-id="26b5a-143">Les interfaces utilisateur ne peuvent pas être rendues persistantes, telles que les instances de composant et leurs arborescences de rendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-143">UIs can't be persisted, such as component instances and their render trees.</span></span> <span data-ttu-id="26b5a-144">Les composants et les arborescences de rendu ne sont généralement pas sérialisables.</span><span class="sxs-lookup"><span data-stu-id="26b5a-144">Components and render trees aren't generally serializable.</span></span> <span data-ttu-id="26b5a-145">Pour conserver un aspect similaire à l’état de l’interface utilisateur, tel que les nœuds développés d’un contrôle TreeView, l’application doit disposer d’un code personnalisé pour modéliser le comportement comme étant un état d’application sérialisable.</span><span class="sxs-lookup"><span data-stu-id="26b5a-145">To persist something similar to UI state, such as the expanded nodes of a TreeView, the app must have custom code to model the behavior as serializable app state.</span></span>

## <a name="where-to-persist-state"></a><span data-ttu-id="26b5a-146">Emplacement de conservation de l’État</span><span class="sxs-lookup"><span data-stu-id="26b5a-146">Where to persist state</span></span>

<span data-ttu-id="26b5a-147">Trois emplacements communs existent pour conserver l’État dans une application côté serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="26b5a-147">Three common locations exist for persisting state in a Blazor server-side app.</span></span> <span data-ttu-id="26b5a-148">Chaque approche est la mieux adaptée à différents scénarios et présente des inconvénients différents:</span><span class="sxs-lookup"><span data-stu-id="26b5a-148">Each approach is best suited to different scenarios and has different caveats:</span></span>

* [<span data-ttu-id="26b5a-149">Côté serveur dans une base de données</span><span class="sxs-lookup"><span data-stu-id="26b5a-149">Server-side in a database</span></span>](#server-side-in-a-database)
* [<span data-ttu-id="26b5a-150">URL</span><span class="sxs-lookup"><span data-stu-id="26b5a-150">URL</span></span>](#url)
* [<span data-ttu-id="26b5a-151">Côté client dans le navigateur</span><span class="sxs-lookup"><span data-stu-id="26b5a-151">Client-side in the browser</span></span>](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a><span data-ttu-id="26b5a-152">Côté serveur dans une base de données</span><span class="sxs-lookup"><span data-stu-id="26b5a-152">Server-side in a database</span></span>

<span data-ttu-id="26b5a-153">Pour la persistance des données permanente ou pour toutes les données qui doivent s’étendre sur plusieurs utilisateurs ou périphériques, une base de données indépendante côté serveur est certainement le meilleur choix.</span><span class="sxs-lookup"><span data-stu-id="26b5a-153">For permanent data persistence or for any data that must span multiple users or devices, an independent server-side database is almost certainly the best choice.</span></span> <span data-ttu-id="26b5a-154">Les options sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="26b5a-154">Options include:</span></span>

* <span data-ttu-id="26b5a-155">Base de données SQL relationnelle</span><span class="sxs-lookup"><span data-stu-id="26b5a-155">Relational SQL database</span></span>
* <span data-ttu-id="26b5a-156">Magasin clé-valeur</span><span class="sxs-lookup"><span data-stu-id="26b5a-156">Key-value store</span></span>
* <span data-ttu-id="26b5a-157">Magasin d’objets BLOB</span><span class="sxs-lookup"><span data-stu-id="26b5a-157">Blob store</span></span>
* <span data-ttu-id="26b5a-158">Magasin de tables</span><span class="sxs-lookup"><span data-stu-id="26b5a-158">Table store</span></span>

<span data-ttu-id="26b5a-159">Une fois les données enregistrées dans la base de données, un nouveau circuit peut être démarré par un utilisateur à tout moment.</span><span class="sxs-lookup"><span data-stu-id="26b5a-159">After data is saved in the database, a new circuit can be started by a user at any time.</span></span> <span data-ttu-id="26b5a-160">Les données de l’utilisateur sont conservées et disponibles dans n’importe quel nouveau circuit.</span><span class="sxs-lookup"><span data-stu-id="26b5a-160">The user's data is retained and available in any new circuit.</span></span>

<span data-ttu-id="26b5a-161">Pour plus d’informations sur les options de stockage de données Azure, consultez la documentation sur le [stockage Azure](/azure/storage/) et les [bases de données Azure](https://azure.microsoft.com/product-categories/databases/).</span><span class="sxs-lookup"><span data-stu-id="26b5a-161">For more information on Azure data storage options, see the [Azure Storage Documentation](/azure/storage/) and [Azure Databases](https://azure.microsoft.com/product-categories/databases/).</span></span>

### <a name="url"></a><span data-ttu-id="26b5a-162">URL</span><span class="sxs-lookup"><span data-stu-id="26b5a-162">URL</span></span>

<span data-ttu-id="26b5a-163">Pour les données temporaires représentant l’état de navigation, modélisez les données en tant que partie de l’URL.</span><span class="sxs-lookup"><span data-stu-id="26b5a-163">For transient data representing navigation state, model the data as a part of the URL.</span></span> <span data-ttu-id="26b5a-164">Voici des exemples d’État modélisé dans l’URL:</span><span class="sxs-lookup"><span data-stu-id="26b5a-164">Examples of state modeled in the URL include:</span></span>

* <span data-ttu-id="26b5a-165">ID d’une entité affichée.</span><span class="sxs-lookup"><span data-stu-id="26b5a-165">The ID of a viewed entity.</span></span>
* <span data-ttu-id="26b5a-166">Numéro de page actuel dans une grille paginée.</span><span class="sxs-lookup"><span data-stu-id="26b5a-166">The current page number in a paged grid.</span></span>

<span data-ttu-id="26b5a-167">Le contenu de la barre d’adresse du navigateur est conservé:</span><span class="sxs-lookup"><span data-stu-id="26b5a-167">The contents of the browser's address bar are retained:</span></span>

* <span data-ttu-id="26b5a-168">Si l’utilisateur recharge manuellement la page.</span><span class="sxs-lookup"><span data-stu-id="26b5a-168">If the user manually reloads the page.</span></span>
* <span data-ttu-id="26b5a-169">Si le serveur Web devient indisponible&mdash;, l’utilisateur est obligé de recharger la page afin de se connecter à un autre serveur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-169">If the web server becomes unavailable&mdash;the user is forced to reload the page in order to connect to a different server.</span></span>

<span data-ttu-id="26b5a-170">Pour plus d’informations sur la définition de `@page` modèles d’URL <xref:blazor/routing>avec la directive, consultez.</span><span class="sxs-lookup"><span data-stu-id="26b5a-170">For information on defining URL patterns with the `@page` directive, see <xref:blazor/routing>.</span></span>

### <a name="client-side-in-the-browser"></a><span data-ttu-id="26b5a-171">Côté client dans le navigateur</span><span class="sxs-lookup"><span data-stu-id="26b5a-171">Client-side in the browser</span></span>

<span data-ttu-id="26b5a-172">Pour les données temporaires que l’utilisateur crée activement, un magasin de stockage commun est le regroupement `localStorage` et `sessionStorage` le navigateur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-172">For transient data that the user is actively creating, a common backing store is the browser's `localStorage` and `sessionStorage` collections.</span></span> <span data-ttu-id="26b5a-173">L’application n’est pas requise pour gérer ou effacer l’État stocké si le circuit est abandonné, ce qui constitue un avantage par rapport au stockage côté serveur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-173">The app isn't required to manage or clear the stored state if the circuit is abandoned, which is an advantage over server-side storage.</span></span>

> [!NOTE]
> <span data-ttu-id="26b5a-174">«Côté client» dans cette section fait référence aux scénarios côté client dans le navigateur, et non au [modèle d’hébergement côté client éblouissant](xref:blazor/hosting-models#client-side).</span><span class="sxs-lookup"><span data-stu-id="26b5a-174">"Client-side" in this section refers to client-side scenarios in the browser, not the [Blazor client-side hosting model](xref:blazor/hosting-models#client-side).</span></span> <span data-ttu-id="26b5a-175">`localStorage`et `sessionStorage` peuvent être utilisés dans les applications côté client éblouissantes, mais uniquement en écrivant du code personnalisé ou à l’aide d’un package tiers.</span><span class="sxs-lookup"><span data-stu-id="26b5a-175">`localStorage` and `sessionStorage` can be used in Blazor client-side apps but only by writing custom code or using a 3rd party package.</span></span>

<span data-ttu-id="26b5a-176">`localStorage`et `sessionStorage` diffèrent comme suit:</span><span class="sxs-lookup"><span data-stu-id="26b5a-176">`localStorage` and `sessionStorage` differ as follows:</span></span>

* <span data-ttu-id="26b5a-177">`localStorage`est étendu au navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-177">`localStorage` is scoped to the user's browser.</span></span> <span data-ttu-id="26b5a-178">Si l’utilisateur recharge la page ou ferme et ouvre à nouveau le navigateur, l’état persiste.</span><span class="sxs-lookup"><span data-stu-id="26b5a-178">If the user reloads the page or closes and re-opens the browser, the state persists.</span></span> <span data-ttu-id="26b5a-179">Si l’utilisateur ouvre plusieurs onglets de navigateur, l’État est partagé à travers les onglets.</span><span class="sxs-lookup"><span data-stu-id="26b5a-179">If the user opens multiple browser tabs, the state is shared across the tabs.</span></span> <span data-ttu-id="26b5a-180">Les données sont conservées dans `localStorage` jusqu’à ce qu’elles soient explicitement effacées.</span><span class="sxs-lookup"><span data-stu-id="26b5a-180">Data persists in `localStorage` until explicitly cleared.</span></span>
* <span data-ttu-id="26b5a-181">`sessionStorage`est étendu à l’onglet navigateur de l’utilisateur. Si l’utilisateur recharge l’onglet, l’état persiste.</span><span class="sxs-lookup"><span data-stu-id="26b5a-181">`sessionStorage` is scoped to the user's browser tab. If the user reloads the tab, the state persists.</span></span> <span data-ttu-id="26b5a-182">Si l’utilisateur ferme l’onglet ou le navigateur, l’État est perdu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-182">If the user closes the tab or the browser, the state is lost.</span></span> <span data-ttu-id="26b5a-183">Si l’utilisateur ouvre plusieurs onglets de navigateur, chaque onglet possède sa propre version indépendante des données.</span><span class="sxs-lookup"><span data-stu-id="26b5a-183">If the user opens multiple browser tabs, each tab has its own independent version of the data.</span></span>

<span data-ttu-id="26b5a-184">En règle `sessionStorage` générale, il est plus sûr d’utiliser.</span><span class="sxs-lookup"><span data-stu-id="26b5a-184">Generally, `sessionStorage` is safer to use.</span></span> <span data-ttu-id="26b5a-185">`sessionStorage`évite le risque qu’un utilisateur ouvre plusieurs onglets et rencontre les éléments suivants:</span><span class="sxs-lookup"><span data-stu-id="26b5a-185">`sessionStorage` avoids the risk that a user opens multiple tabs and encounters the following:</span></span>

* <span data-ttu-id="26b5a-186">Bogues dans le stockage d’État sur les onglets.</span><span class="sxs-lookup"><span data-stu-id="26b5a-186">Bugs in state storage across tabs.</span></span>
* <span data-ttu-id="26b5a-187">Comportement confus quand une tabulation remplace l’état d’autres onglets.</span><span class="sxs-lookup"><span data-stu-id="26b5a-187">Confusing behavior when a tab overwrites the state of other tabs.</span></span>

<span data-ttu-id="26b5a-188">`localStorage`est le meilleur choix si l’application doit conserver l’État dans la fermeture et la réouverture du navigateur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-188">`localStorage` is the better choice if the app must persist state across closing and re-opening the browser.</span></span>

<span data-ttu-id="26b5a-189">Avertissements relatifs à l’utilisation du stockage du navigateur:</span><span class="sxs-lookup"><span data-stu-id="26b5a-189">Caveats for using browser storage:</span></span>

* <span data-ttu-id="26b5a-190">À l’instar de l’utilisation d’une base de données côté serveur, le chargement et l’enregistrement des données sont asynchrones.</span><span class="sxs-lookup"><span data-stu-id="26b5a-190">Similar to the use of a server-side database, loading and saving data are asynchronous.</span></span>
* <span data-ttu-id="26b5a-191">Contrairement à une base de données côté serveur, le stockage n’est pas disponible pendant le prérendu, car la page demandée n’existe pas dans le navigateur pendant l’étape de prérendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-191">Unlike a server-side database, storage isn't available during prerendering because the requested page doesn't exist in the browser during the prerendering stage.</span></span>
* <span data-ttu-id="26b5a-192">Le stockage de quelques kilo-octets de données est raisonnable à conserver pour les applications côté serveur éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="26b5a-192">Storage of a few kilobytes of data is reasonable to persist for Blazor server-side apps.</span></span> <span data-ttu-id="26b5a-193">Au-delà de quelques kilo-octets, vous devez prendre en compte les implications en termes de performances, car les données sont chargées et enregistrées sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="26b5a-193">Beyond a few kilobytes, you must consider the performance implications because the data is loaded and saved across the network.</span></span>
* <span data-ttu-id="26b5a-194">Les utilisateurs peuvent afficher ou altérer les données.</span><span class="sxs-lookup"><span data-stu-id="26b5a-194">Users may view or tamper with the data.</span></span> <span data-ttu-id="26b5a-195">La [protection des données](xref:security/data-protection/introduction) ASP.net Core peut atténuer le risque.</span><span class="sxs-lookup"><span data-stu-id="26b5a-195">ASP.NET Core [Data Protection](xref:security/data-protection/introduction) can mitigate the risk.</span></span>

## <a name="third-party-browser-storage-solutions"></a><span data-ttu-id="26b5a-196">Solutions de stockage de navigateur tiers</span><span class="sxs-lookup"><span data-stu-id="26b5a-196">Third-party browser storage solutions</span></span>

<span data-ttu-id="26b5a-197">Les packages NuGet tiers fournissent des API pour travailler avec `localStorage` et `sessionStorage`.</span><span class="sxs-lookup"><span data-stu-id="26b5a-197">Third-party NuGet packages provide APIs for working with `localStorage` and `sessionStorage`.</span></span>

<span data-ttu-id="26b5a-198">Il est judicieux de choisir un package qui utilise de manière transparente la [protection des données](xref:security/data-protection/introduction)de ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="26b5a-198">It's worth considering choosing a package that transparently uses ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="26b5a-199">ASP.NET Core protection des données chiffre les données stockées et réduit le risque potentiel de falsification des données stockées.</span><span class="sxs-lookup"><span data-stu-id="26b5a-199">ASP.NET Core Data Protection encrypts stored data and reduces the potential risk of tampering with stored data.</span></span> <span data-ttu-id="26b5a-200">Si les données sérialisées JSON sont stockées en texte clair, les utilisateurs peuvent voir les données à l’aide des outils de développement du navigateur et également modifier les données stockées.</span><span class="sxs-lookup"><span data-stu-id="26b5a-200">If JSON-serialized data is stored in plaintext, users can see the data using browser developer tools and also modify the stored data.</span></span> <span data-ttu-id="26b5a-201">La sécurisation des données n’est pas toujours un problème, car les données peuvent être de nature insignifiante.</span><span class="sxs-lookup"><span data-stu-id="26b5a-201">Securing data isn't always a problem because the data might be trivial in nature.</span></span> <span data-ttu-id="26b5a-202">Par exemple, la lecture ou la modification de la couleur stockée d’un élément d’interface utilisateur n’est pas un risque de sécurité significatif pour l’utilisateur ou l’organisation.</span><span class="sxs-lookup"><span data-stu-id="26b5a-202">For example, reading or modifying the stored color of a UI element isn't a significant security risk to the user or the organization.</span></span> <span data-ttu-id="26b5a-203">Évitez d’autoriser les utilisateurs à inspecter ou altérer des *données sensibles*.</span><span class="sxs-lookup"><span data-stu-id="26b5a-203">Avoid allowing users to inspect or tamper with *sensitive data*.</span></span>

## <a name="protected-browser-storage-experimental-package"></a><span data-ttu-id="26b5a-204">Package d’expérimentation de stockage protégé du navigateur</span><span class="sxs-lookup"><span data-stu-id="26b5a-204">Protected Browser Storage experimental package</span></span>

<span data-ttu-id="26b5a-205">Voici un exemple de package NuGet qui fournit une protection des `localStorage` [données](xref:security/data-protection/introduction) pour et `sessionStorage` est [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span><span class="sxs-lookup"><span data-stu-id="26b5a-205">An example of a NuGet package that provides [Data Protection](xref:security/data-protection/introduction) for `localStorage` and `sessionStorage` is [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>

> [!WARNING]
> <span data-ttu-id="26b5a-206">`Microsoft.AspNetCore.ProtectedBrowserStorage`est un package expérimental non pris en charge inapproprié pour une utilisation en production à l’heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="26b5a-206">`Microsoft.AspNetCore.ProtectedBrowserStorage` is an unsupported experimental package unsuitable for production use at this time.</span></span>

### <a name="installation"></a><span data-ttu-id="26b5a-207">Installation</span><span class="sxs-lookup"><span data-stu-id="26b5a-207">Installation</span></span>

<span data-ttu-id="26b5a-208">Pour installer le `Microsoft.AspNetCore.ProtectedBrowserStorage` package:</span><span class="sxs-lookup"><span data-stu-id="26b5a-208">To install the `Microsoft.AspNetCore.ProtectedBrowserStorage` package:</span></span>

1. <span data-ttu-id="26b5a-209">Dans le projet d’application côté serveur éblouissant, ajoutez une référence de package à [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span><span class="sxs-lookup"><span data-stu-id="26b5a-209">In the Blazor server-side app project, add a package reference to [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>
1. <span data-ttu-id="26b5a-210">Dans le code HTML de niveau supérieur (par exemple, dans le fichier *pages/_Host. cshtml* dans le modèle de projet par défaut), `<script>` ajoutez la balise suivante:</span><span class="sxs-lookup"><span data-stu-id="26b5a-210">In the top-level HTML (for example, in the *Pages/_Host.cshtml* file in the default project template), add the following `<script>` tag:</span></span>

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. <span data-ttu-id="26b5a-211">Dans la `Startup.ConfigureServices` méthode, appelez `AddProtectedBrowserStorage` pour ajouter `localStorage` des `sessionStorage` services à la collection de services:</span><span class="sxs-lookup"><span data-stu-id="26b5a-211">In the `Startup.ConfigureServices` method, call `AddProtectedBrowserStorage` to add `localStorage` and `sessionStorage` services to the service collection:</span></span>

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a><span data-ttu-id="26b5a-212">Enregistrer et charger des données dans un composant</span><span class="sxs-lookup"><span data-stu-id="26b5a-212">Save and load data within a component</span></span>

<span data-ttu-id="26b5a-213">Dans tout composant nécessitant le chargement ou l’enregistrement de données dans le [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) stockage du navigateur, utilisez pour injecter une instance de l’un des éléments suivants:</span><span class="sxs-lookup"><span data-stu-id="26b5a-213">In any component that requires loading or saving data to browser storage, use [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) to inject an instance of either of the following:</span></span>

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

<span data-ttu-id="26b5a-214">Le choix dépend du magasin de stockage que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="26b5a-214">The choice depends on which backing store you wish to use.</span></span> <span data-ttu-id="26b5a-215">Dans l’exemple suivant, `sessionStorage` est utilisé:</span><span class="sxs-lookup"><span data-stu-id="26b5a-215">In the following example, `sessionStorage` is used:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

<span data-ttu-id="26b5a-216">L' `@using` instruction peut être placée dans un fichier *_Imports. Razor* plutôt que dans le composant.</span><span class="sxs-lookup"><span data-stu-id="26b5a-216">The `@using` statement can be placed into an *_Imports.razor* file instead of in the component.</span></span> <span data-ttu-id="26b5a-217">L’utilisation du fichier *_Imports. Razor* rend l’espace de noms disponible pour les plus grands segments de l’application ou de l’application entière.</span><span class="sxs-lookup"><span data-stu-id="26b5a-217">Use of the *_Imports.razor* file makes the namespace available to larger segments of the app or the whole app.</span></span>

<span data-ttu-id="26b5a-218">Pour rendre la `currentCount` valeur persistante `Counter` dans le composant du modèle de projet, `IncrementCount` modifiez la méthode `ProtectedSessionStore.SetAsync`pour utiliser:</span><span class="sxs-lookup"><span data-stu-id="26b5a-218">To persist the `currentCount` value in the `Counter` component of the project template, modify the `IncrementCount` method to use `ProtectedSessionStore.SetAsync`:</span></span>

```csharp
private async Task IncrementCount()
{
    currentCount++;
    await ProtectedSessionStore.SetAsync("count", currentCount);
}
```

<span data-ttu-id="26b5a-219">Dans les applications plus volumineuses et plus réalistes, le stockage de champs individuels est un scénario peu probable.</span><span class="sxs-lookup"><span data-stu-id="26b5a-219">In larger, more realistic apps, storage of individual fields is an unlikely scenario.</span></span> <span data-ttu-id="26b5a-220">Les applications sont plus susceptibles de stocker des objets de modèle entiers qui incluent un État complexe.</span><span class="sxs-lookup"><span data-stu-id="26b5a-220">Apps are more likely to store entire model objects that include complex state.</span></span> <span data-ttu-id="26b5a-221">`ProtectedSessionStore`sérialise et désérialise automatiquement les données JSON.</span><span class="sxs-lookup"><span data-stu-id="26b5a-221">`ProtectedSessionStore` automatically serializes and deserializes JSON data.</span></span>

<span data-ttu-id="26b5a-222">Dans l’exemple de code précédent, `currentCount` les données sont stockées `sessionStorage['count']` sous la forme dans le navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-222">In the preceding code example, the `currentCount` data is stored as `sessionStorage['count']` in the user's browser.</span></span> <span data-ttu-id="26b5a-223">Les données ne sont pas stockées en texte clair mais sont protégées à l’aide de la [protection des données](xref:security/data-protection/introduction)de ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="26b5a-223">The data isn't stored in plaintext but rather is protected using ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="26b5a-224">Les données chiffrées peuvent être consultées `sessionStorage['count']` si est évalué dans la console de développement du navigateur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-224">The encrypted data can be seen if `sessionStorage['count']` is evaluated in the browser's developer console.</span></span>

<span data-ttu-id="26b5a-225">Pour récupérer les `currentCount` données si l’utilisateur retourne `Counter` au composant ultérieurement (y compris s’il s’agit d’un circuit entièrement nouveau), `ProtectedSessionStore.GetAsync`utilisez:</span><span class="sxs-lookup"><span data-stu-id="26b5a-225">To recover the `currentCount` data if the user returns to the `Counter` component later (including if they're on an entirely new circuit), use `ProtectedSessionStore.GetAsync`:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

<span data-ttu-id="26b5a-226">Si les paramètres du composant incluent l’état de navigation `ProtectedSessionStore.GetAsync` , appelez et assignez le `OnInitializedAsync`résultat dans `OnParametersSetAsync`, et non.</span><span class="sxs-lookup"><span data-stu-id="26b5a-226">If the component's parameters include navigation state, call `ProtectedSessionStore.GetAsync` and assign the result in `OnParametersSetAsync`, not `OnInitializedAsync`.</span></span> <span data-ttu-id="26b5a-227">`OnInitializedAsync`n’est appelé qu’une seule fois lors de la première instanciation du composant.</span><span class="sxs-lookup"><span data-stu-id="26b5a-227">`OnInitializedAsync` is only called one time when the component is first instantiated.</span></span> <span data-ttu-id="26b5a-228">`OnInitializedAsync`n’est pas rappelée ultérieurement si l’utilisateur accède à une autre URL tout en restant sur la même page.</span><span class="sxs-lookup"><span data-stu-id="26b5a-228">`OnInitializedAsync` isn't called again later if the user navigates to a different URL while remaining on the same page.</span></span>

> [!WARNING]
> <span data-ttu-id="26b5a-229">Les exemples de cette section ne fonctionnent que si le prérendu n’est pas activé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-229">The examples in this section only work if the server doesn't have prerendering enabled.</span></span> <span data-ttu-id="26b5a-230">Quand le prérendu est activé, une erreur est générée de la façon suivante:</span><span class="sxs-lookup"><span data-stu-id="26b5a-230">With prerendering enabled, an error is generated similar to:</span></span>
>
> > <span data-ttu-id="26b5a-231">Impossible d’émettre des appels Interop JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="26b5a-231">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="26b5a-232">Cela est dû au fait que le composant est en cours de prérendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-232">This is because the component is being prerendered.</span></span>
>
> <span data-ttu-id="26b5a-233">Désactivez le prérendu ou ajoutez du code supplémentaire pour utiliser le prérendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-233">Either disable prerendering or add additional code to work with prerendering.</span></span> <span data-ttu-id="26b5a-234">Pour en savoir plus sur l’écriture de code qui fonctionne avec le prérendu, consultez la section [handle](#handle-prerendering) PreRender.</span><span class="sxs-lookup"><span data-stu-id="26b5a-234">To learn more about writing code that works with prerendering, see the [Handle prerendering](#handle-prerendering) section.</span></span>

### <a name="handle-the-loading-state"></a><span data-ttu-id="26b5a-235">Gérer l’état de chargement</span><span class="sxs-lookup"><span data-stu-id="26b5a-235">Handle the loading state</span></span>

<span data-ttu-id="26b5a-236">Étant donné que le stockage du navigateur est asynchrone (accessible via une connexion réseau), il y a toujours un certain temps avant que les données soient chargées et disponibles pour une utilisation par un composant.</span><span class="sxs-lookup"><span data-stu-id="26b5a-236">Since browser storage is asynchronous (accessed over a network connection), there's always a period of time before the data is loaded and available for use by a component.</span></span> <span data-ttu-id="26b5a-237">Pour obtenir les meilleurs résultats, affichez un message d’état de chargement pendant le chargement en cours au lieu d’afficher les données vides ou par défaut.</span><span class="sxs-lookup"><span data-stu-id="26b5a-237">For the best results, render a loading-state message while loading is in progress instead of displaying blank or default data.</span></span>

<span data-ttu-id="26b5a-238">Une approche consiste à déterminer si les données sont `null` (toujours en cours de chargement) ou non.</span><span class="sxs-lookup"><span data-stu-id="26b5a-238">One approach is to track whether the data is `null` (still loading) or not.</span></span> <span data-ttu-id="26b5a-239">Dans le composant `Counter` par défaut, le nombre est conservé dans `int`un.</span><span class="sxs-lookup"><span data-stu-id="26b5a-239">In the default `Counter` component, the count is held in an `int`.</span></span> <span data-ttu-id="26b5a-240">Rendez `currentCount` la valeur null en ajoutant un`?`point d’interrogation (`int`) au type ():</span><span class="sxs-lookup"><span data-stu-id="26b5a-240">Make `currentCount` nullable by adding a question mark (`?`) to the type (`int`):</span></span>

```csharp
private int? currentCount;
```

<span data-ttu-id="26b5a-241">Au lieu d’afficher sans condition le bouton nombre et **incrément** , choisissez d’afficher ces éléments uniquement si les données sont chargées:</span><span class="sxs-lookup"><span data-stu-id="26b5a-241">Instead of unconditionally displaying the count and **Increment** button, choose to display these elements only if the data is loaded:</span></span>

```cshtml
@if (currentCount.HasValue)
{
    <p>Current count: <strong>@currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a><span data-ttu-id="26b5a-242">Gérer le prérendu</span><span class="sxs-lookup"><span data-stu-id="26b5a-242">Handle prerendering</span></span>

<span data-ttu-id="26b5a-243">Lors du prérendu:</span><span class="sxs-lookup"><span data-stu-id="26b5a-243">During prerendering:</span></span>

* <span data-ttu-id="26b5a-244">Une connexion interactive au navigateur de l’utilisateur n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="26b5a-244">An interactive connection to the user's browser doesn't exist.</span></span>
* <span data-ttu-id="26b5a-245">Le navigateur ne dispose pas encore d’une page dans laquelle il peut exécuter du code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="26b5a-245">The browser doesn't yet have a page in which it can run JavaScript code.</span></span>

<span data-ttu-id="26b5a-246">`localStorage`ou `sessionStorage` ne sont pas disponibles pendant le prérendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-246">`localStorage` or `sessionStorage` aren't available during prerendering.</span></span> <span data-ttu-id="26b5a-247">Si le composant tente d’interagir avec le stockage, une erreur est générée de la façon suivante:</span><span class="sxs-lookup"><span data-stu-id="26b5a-247">If the component attempts to interact with storage, an error is generated similar to:</span></span>

> <span data-ttu-id="26b5a-248">Impossible d’émettre des appels Interop JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="26b5a-248">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="26b5a-249">Cela est dû au fait que le composant est en cours de prérendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-249">This is because the component is being prerendered.</span></span>

<span data-ttu-id="26b5a-250">L’une des méthodes permettant de résoudre l’erreur consiste à désactiver le prérendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-250">One way to resolve the error is to disable prerendering.</span></span> <span data-ttu-id="26b5a-251">C’est généralement le meilleur choix si l’application utilise beaucoup le stockage basé sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-251">This is usually the best choice if the app makes heavy use of browser-based storage.</span></span> <span data-ttu-id="26b5a-252">Le prérendu ajoute de la complexité et ne tire pas parti de l’application, car l’application ne `localStorage` peut `sessionStorage` pas prérestituer de contenu utile tant que ou n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="26b5a-252">Prerendering adds complexity and doesn't benefit the app because the app can't prerender any useful content until `localStorage` or `sessionStorage` are available.</span></span>

<span data-ttu-id="26b5a-253">Pour désactiver le prérendu:</span><span class="sxs-lookup"><span data-stu-id="26b5a-253">To disable prerendering:</span></span>

1. <span data-ttu-id="26b5a-254">Ouvrez le fichier *pages/_Host. cshtml* et supprimez l’appel `Html.RenderComponentAsync`à.</span><span class="sxs-lookup"><span data-stu-id="26b5a-254">Open the *Pages/_Host.cshtml* file and remove the call to `Html.RenderComponentAsync`.</span></span>
1. <span data-ttu-id="26b5a-255">Ouvrez le `Startup.cs` fichier et remplacez l’appel à `endpoints.MapBlazorHub()` par `endpoints.MapBlazorHub<App>("app")`.</span><span class="sxs-lookup"><span data-stu-id="26b5a-255">Open the `Startup.cs` file and replace the call to `endpoints.MapBlazorHub()` with `endpoints.MapBlazorHub<App>("app")`.</span></span> <span data-ttu-id="26b5a-256">`App`est le type du composant racine.</span><span class="sxs-lookup"><span data-stu-id="26b5a-256">`App` is the type of the root component.</span></span> <span data-ttu-id="26b5a-257">`"app"`est un sélecteur CSS spécifiant l’emplacement du composant racine.</span><span class="sxs-lookup"><span data-stu-id="26b5a-257">`"app"` is a CSS selector specifying the location for the root component.</span></span>

<span data-ttu-id="26b5a-258">Le prérendu peut être utile pour d’autres pages qui n' `localStorage` utilisent `sessionStorage`pas ou.</span><span class="sxs-lookup"><span data-stu-id="26b5a-258">Prerendering might be useful for other pages that don't use `localStorage` or `sessionStorage`.</span></span> <span data-ttu-id="26b5a-259">Pour conserver le prérendu activé, différez l’opération de chargement jusqu’à ce que le navigateur soit connecté au circuit.</span><span class="sxs-lookup"><span data-stu-id="26b5a-259">To keep prerendering enabled, defer the loading operation until the browser is connected to the circuit.</span></span> <span data-ttu-id="26b5a-260">Voici un exemple de stockage d’une valeur de compteur:</span><span class="sxs-lookup"><span data-stu-id="26b5a-260">The following is an example for storing a counter value:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore
@inject IComponentContext ComponentContext

... rendering code goes here ...

@code {
    private int? currentCount;
    private bool isWaitingForConnection;

    protected override async Task OnInitializedAsync()
    {
        if (ComponentContext.IsConnected)
        {
            // It looks like the app isn't prerendering, so the data can be
            // immediately loaded from browser storage.
            await LoadStateAsync();
        }
        else
        {
            // Prerendering is in progress, so the app defers the load operation
            // until later.
            isWaitingForConnection = true;
        }
    }

    protected override async Task OnAfterRenderAsync()
    {
        // By this stage, the client has connected back to the server, and
        // browser services are available. If the app didn't load the data earlier,
        // the app should do so now and then trigger a new render.
        if (isWaitingForConnection)
        {
            isWaitingForConnection = false;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        currentCount++;
        await ProtectedSessionStore.SetAsync("count", currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a><span data-ttu-id="26b5a-261">Factoriser la conservation de l’État à un emplacement commun</span><span class="sxs-lookup"><span data-stu-id="26b5a-261">Factor out the state preservation to a common location</span></span>

<span data-ttu-id="26b5a-262">Si de nombreux composants reposent sur un stockage basé sur un navigateur, la réimplémentation du code du fournisseur d’état de nombreuses fois crée une duplication du code.</span><span class="sxs-lookup"><span data-stu-id="26b5a-262">If many components rely on browser-based storage, re-implementing state provider code many times creates code duplication.</span></span> <span data-ttu-id="26b5a-263">Une option pour éviter la duplication de code consiste à créer un *composant parent du fournisseur d’État* qui encapsule la logique du fournisseur d’État.</span><span class="sxs-lookup"><span data-stu-id="26b5a-263">One option for avoiding code duplication is to create a *state provider parent component* that encapsulates the state provider logic.</span></span> <span data-ttu-id="26b5a-264">Les composants enfants peuvent fonctionner avec des données persistantes sans tenir compte du mécanisme de persistance de l’État.</span><span class="sxs-lookup"><span data-stu-id="26b5a-264">Child components can work with persisted data without regard to the state persistence mechanism.</span></span>

<span data-ttu-id="26b5a-265">Dans l’exemple suivant d’un `CounterStateProvider` composant, les données de compteur sont conservées:</span><span class="sxs-lookup"><span data-stu-id="26b5a-265">In the following example of a `CounterStateProvider` component, counter data is persisted:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (hasLoaded)
{
    <CascadingValue Value="@this">
        @ChildContent
    </CascadingValue>
}
else
{
    <p>Loading...</p>
}

@code {
    private bool hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

<span data-ttu-id="26b5a-266">Le `CounterStateProvider` composant gère la phase de chargement en n’affichant pas son contenu enfant tant que le chargement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="26b5a-266">The `CounterStateProvider` component handles the loading phase by not rendering its child content until loading is complete.</span></span>

<span data-ttu-id="26b5a-267">Pour utiliser le `CounterStateProvider` composant, encapsulez une instance du composant autour de tout autre composant qui requiert l’accès à l’état du compteur.</span><span class="sxs-lookup"><span data-stu-id="26b5a-267">To use the `CounterStateProvider` component, wrap an instance of the component around any other component that requires access to the counter state.</span></span> <span data-ttu-id="26b5a-268">Pour rendre l’état accessible à tous les composants d’une application, encapsulez le `CounterStateProvider` composant autour `App` du `Router` dans le composant (*app. Razor*):</span><span class="sxs-lookup"><span data-stu-id="26b5a-268">To make the state accessible to all components in an app, wrap the `CounterStateProvider` component around the `Router` in the `App` component (*App.razor*):</span></span>

```cshtml
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

<span data-ttu-id="26b5a-269">Les composants encapsulés reçoivent et peuvent modifier l’état du compteur persistant.</span><span class="sxs-lookup"><span data-stu-id="26b5a-269">Wrapped components receive and can modify the persisted counter state.</span></span> <span data-ttu-id="26b5a-270">Le composant `Counter` suivant implémente le modèle:</span><span class="sxs-lookup"><span data-stu-id="26b5a-270">The following `Counter` component implements the pattern:</span></span>

```cshtml
@page "/counter"

<p>Current count: <strong>@CounterStateProvider.CurrentCount</strong></p>

<button @onclick="IncrementCount">Increment</button>

@code {
    [CascadingParameter]
    private CounterStateProvider CounterStateProvider { get; set; }

    private async Task IncrementCount()
    {
        CounterStateProvider.CurrentCount++;
        await CounterStateProvider.SaveChangesAsync();
    }
}
```

<span data-ttu-id="26b5a-271">Le composant précédent n’est pas requis pour `ProtectedBrowserStorage`interagir avec et ne gère pas une phase de «chargement».</span><span class="sxs-lookup"><span data-stu-id="26b5a-271">The preceding component isn't required to interact with `ProtectedBrowserStorage`, nor does it deal with a "loading" phase.</span></span>

<span data-ttu-id="26b5a-272">Pour traiter le prérendu comme décrit précédemment, `CounterStateProvider` peut être modifié de façon à ce que tous les composants qui consomment les données de compteur fonctionnent automatiquement avec le prérendu.</span><span class="sxs-lookup"><span data-stu-id="26b5a-272">To deal with prerendering as described earlier, `CounterStateProvider` can be amended so that all of the components that consume the counter data automatically work with prerendering.</span></span> <span data-ttu-id="26b5a-273">Pour plus d’informations, consultez la section relative au [prérendu des handles](#handle-prerendering).</span><span class="sxs-lookup"><span data-stu-id="26b5a-273">See the [Handle prerendering](#handle-prerendering) section for details.</span></span>

<span data-ttu-id="26b5a-274">En général, le modèle de *composant parent du fournisseur d’État* est recommandé:</span><span class="sxs-lookup"><span data-stu-id="26b5a-274">In general, *state provider parent component* pattern is recommended:</span></span>

* <span data-ttu-id="26b5a-275">Pour utiliser l’État dans de nombreux autres composants.</span><span class="sxs-lookup"><span data-stu-id="26b5a-275">To consume state in many other components.</span></span>
* <span data-ttu-id="26b5a-276">S’il n’existe qu’un seul objet d’état de niveau supérieur à conserver.</span><span class="sxs-lookup"><span data-stu-id="26b5a-276">If there's just one top-level state object to persist.</span></span>

<span data-ttu-id="26b5a-277">Pour conserver un grand nombre d’objets d’état différents et utiliser différents sous-ensembles d’objets à des emplacements différents, il est préférable d’éviter de gérer le chargement et l’enregistrement de l’État globalement.</span><span class="sxs-lookup"><span data-stu-id="26b5a-277">To persist many different state objects and consume different subsets of objects in different places, it's better to avoid handling the loading and saving of state globally.</span></span>
