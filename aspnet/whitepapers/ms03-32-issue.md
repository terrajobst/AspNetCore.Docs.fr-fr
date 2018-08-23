---
uid: whitepapers/ms03-32-issue
title: Correctif pour l’erreur de « Application serveur non disponible » après avoir appliqué la mise à jour de sécurité pour Internet Explorer | Microsoft Docs
author: rick-anderson
description: Cet article décrit le correctif logiciel qui résout un problème avec la mise à jour de sécurité MS03-32 pour Internet Explorer qui affecte les applications ASP.NET 1.0 en cours d’exécution sur Wi...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: ce2d705a93577b0c6d28f86069873c6ecd891db6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838059"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Correction d’erreur de « Application serveur non disponible » après avoir appliqué la mise à jour de sécurité pour Internet Explorer
====================
> Cet article décrit le correctif logiciel qui résout un problème avec la mise à jour de sécurité MS03-32 pour Internet Explorer qui affecte les applications ASP.NET 1.0 s’exécutant sur Windows XP Professionnel.
> 
> S’applique à ASP.NET 1.0 et Windows XP Professionnel.


Microsoft a identifié un problème avec la mise à jour de sécurité MS03-32 pour le correctif de sécurité d’Internet Explorer et ASP.NET 1.0 s’exécutant sur Windows XP. Ce correctif peut être installé manuellement ou en obtenant les récentes mises à jour critiques à partir du site Windows Update.

Le symptôme de ce problème est qu’après avoir installé le correctif logiciel sur un ordinateur Windows XP, toutes les demandes pour les applications ASP.NET exécutées sur le serveur web IIS 5.1 local entraînent un message d’erreur indiquant que « Server Application indisponible ». Demandes aux serveurs web à distance ne sont pas affectés.

Ce problème affecte uniquement les installations en cours d’exécution ASP.NET 1.0 sur Windows XP. Il n’affecte pas les ordinateurs exécutant Windows 2000 ou Windows Server 2003. Également, il n’affecte pas les ordinateurs exécutant Windows XP avec ASP.NET 1.1 est installé.

Veuillez noter que ce problème **n’est pas** un bogue de sécurité avec ASP.NET. Il **pas** ouvrir ou de permettre des attaques malveillantes contre un serveur ou une application ASP.NET. Au lieu de cela, il est purement un bogue fonctionnel causé par le correctif proprement dit.

Nous travaillons d’arrache-pied sur une solution permanente de ce problème. En attendant, vous pouvez exécuter le fichier de commandes suivant comme un moyen de contourner le problème. Le fichier de commandes effectue les opérations suivantes :

1. Arrête les services d’état IIS et ASP.NET
2. Supprime et recrée le compte ASPNET avec un mot de passe temporaire connu
3. Utilise le Windows `runas` commande pour lancer un exécutable qui crée un profil utilisateur ASPNET
4. Enregistre à nouveau ASP.NET. Cela crée un nouveau mot de passe aléatoire pour le compte et applique les paramètres de contrôle d’accès par défaut ASP.NET pour celui-ci
5. Redémarre le service IIS

Le fichier de commandes contient un mot de passe temporaire codé en dur de «<strong>1pass@word</strong>» qui vous serez invité à entrer pour la commande runas, exécutez lorsque le fichier de commandes est exécuté. Une fois la commande runas terminée, le mot de passe du compte ASPNET est recréé avec une valeur aléatoire forte. Notez que le fichier de commandes peut échouer si le mot de passe codé en dur ne répond pas aux exigences de complexité de mot de passe dans votre environnement. Si tel est le cas, vous pouvez le modifier à une autre valeur qui est appropriée pour votre environnement.

*> [!IMPORTANT]* Si vous avez ajouté les paramètres de contrôle d’accès personnalisés ou des autorisations de compte de base de données pour le compte ASPNET, elles devra être recréée issue de ce fichier de commandes. Il s’agit, car lorsque le compte est recréé, il obtiendra un nouvel identificateur de sécurité (SID).

*> [!IMPORTANT]* Si vous exécutez le processus de travail ASP.NET avec un compte autre que le compte ASPNET personnalisé, vous ne devez pas exécuter ce fichier de commandes. Au lieu de cela, vous devez connecter de manière interactive ou utiliser la commande runas avec ce compte pour créer un profil utilisateur pour ce compte.

Le fichier de commandes est inclus dans l’archive auto-extractible ci-dessous. Pour l’utiliser :

1. Vous devez exécuter en tant que compte avec des privilèges d’administrateur
2. [Téléchargez et ouvrez le fichier exécutable auto-extractible](ms03-32-issue/_static/fixup1.exe)
3. Extraire le contenu dans c:\
4. Sélectionnez Exécuter... dans le menu Démarrer, puis entrez `cmd.exe`
5. Dans la fenêtre de commande ouverte, tapez `c:\fixup.cmd`.
6. Lorsque vous y êtes invité, entrez <strong>1pass@word</strong> en tant que le mot de passe.
7. Si vous avez des paramètres de contrôle d’accès précédemment personnalisés ou des autorisations de compte de base de données pour le compte ASPNET, vous devrez réappliquer ces paramètres maintenant.

Nombre d’excuses pour le désagrément occasionné. Nous allons envoyer des informations supplémentaires qu’il est disponible.

Le tableau ci-dessous présente les plateformes et la version affectée par ce problème.

| .NET Framework | Plateforme | Affectés |
| --- | --- | --- |
| Version 1.0 | Windows 2000 Professionnel | Non |
| Version 1.0 | Windows 2000 Server | Non |
| Version 1.0 | Windows XP Professionnel | Oui |
| Version 1.0 | Windows Server 2003 | Non |
| Version 1.0 | Windows XP Édition familiale avec Cassini | Non |
| Version 1.1 | Windows 2000 Professionnel | Non |
| Version 1.1 | Windows 2000 Server | Non |
| Version 1.1 | Windows XP Professionnel | Non |
| Version 1.1 | Windows Server 2003 | Non |
| Version 1.1 | Windows XP Édition familiale avec Cassini | Non |

Merci,   
 L’équipe ASP.NET
