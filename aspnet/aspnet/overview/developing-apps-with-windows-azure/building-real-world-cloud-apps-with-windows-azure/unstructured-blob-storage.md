---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Stockage d’objets Blob non structurées (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1a56a76c9bf27fdd7afb090ca473ebeee4065fe2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577416"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Stockage d’objets Blob non structurées (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Dans le chapitre précédent nous avons étudié les schémas de partitionnement et expliqué comment l’application Fix It stocke les images dans le service de stockage Blob Azure et d’autres données de tâche dans la base de données SQL Azure. Dans ce chapitre, nous aller plus loin dans le service Blob et montrer comment il est implémenté dans le code de projet Fix It.

## <a name="what-is-blob-storage"></a>Quel est le stockage d’objets Blob ?

Le service de stockage Blob Azure fournit un moyen de stocker des fichiers dans le cloud. Le service Blob a un certain nombre d’avantages sur le stockage des fichiers dans un système de fichiers réseau local :

- Il est hautement évolutive. Un compte de stockage unique peut stocker [des centaines de téraoctets](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), et vous pouvez avoir plusieurs comptes de stockage. Certains des plus gros clients Azure stockent des centaines de plusieurs pétaoctets. Microsoft SkyDrive utilise le stockage blob.
- Il est durable. Chaque fichier que vous stockez dans le service Blob est automatiquement sauvegardé.
- Il offre une haute disponibilité. Le [SLA pour Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promesses 99,9 % ou 99,99 % temps d’activité, selon l’option géo-redondance que vous choisissez.
- Cette fonctionnalité est très platform-as-a-service (PaaS) d’Azure, ce qui signifie que vous venez de stockez et récupérer des fichiers, en payant uniquement la quantité réelle de stockage que vous utilisez, et Azure se charge automatiquement de la configuration et la gestion de toutes les machines virtuelles et les disques requis pour le service.
- Vous pouvez accéder au service Blob à l’aide d’une API REST ou à l’aide d’un langage de programmation API. Kits de développement logiciel sont disponibles pour .NET, Java, Ruby et d’autres.
- Lorsque vous stockez un fichier dans le service Blob, vous pouvez facilement rendre disponible publiquement via Internet.
- Vous pouvez sécuriser les fichiers dans l’objet Blob d’accéder au service afin qu’ils peuvent uniquement par les utilisateurs autorisés, ou vous pouvez fournir des jetons d’accès temporaire qui les rend disponibles pour un utilisateur uniquement pour une période limitée.

Chaque fois que vous créez une application pour Azure et que vous souhaitez stocker une grande quantité de données dans un environnement sur site seraient trouve dans les fichiers, tels que des images, vidéos, fichiers PDF, des feuilles de calcul, etc.--prendre en compte le service Blob.

## <a name="creating-a-storage-account"></a>Création d’un compte de stockage

Pour vous familiariser avec le service Blob, vous créez un compte de stockage dans Azure. Dans le portail, cliquez sur **New** -- **Data Services** -- **stockage** -- **création rapide**, puis entrez une URL et un emplacement de centre de données. L’emplacement de centre de données doit être identique à votre application web.

![Créer un compte de stockage](unstructured-blob-storage/_static/image1.png)

Vous pouvez choisir la région principale où vous souhaitez stocker le contenu, et si vous choisissez la [géo-réplication](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) option, Azure crée des réplicas de toutes vos données dans un autre centre de données dans une autre région du pays. Par exemple, si vous choisissez le centre de données ouest des États-Unis, lorsque vous stockez un fichier qu'il va également au centre de données ouest des États-Unis, mais en arrière-plan Azure le copie à un des autres centres de données des États-Unis. Si un incident se produit dans une région du pays, vos données sont toujours sûres.

Azure ne sera pas répliquée des données au-delà des limites géopolitique : Si votre emplacement principal est situé aux États-Unis, vos fichiers sont uniquement répliquées vers une autre région aux États-Unis ; Si votre emplacement principal est de l’Australie, vos fichiers sont uniquement répliquées vers un autre centre de données en Australie.

Bien sûr, vous pouvez également créer un compte de stockage en exécutant des commandes à partir d’un script, comme nous l’avons vu précédemment. Voici une commande Windows PowerShell pour créer un compte de stockage :

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Une fois que vous avez un compte de stockage, vous pouvez immédiatement commencer le stockage de fichiers dans le service Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>À l’aide du stockage d’objets Blob dans l’application Fix It

L’application Fix It permet de charger des photos.

![Créer une tâche Fix It](unstructured-blob-storage/_static/image2.png)

Lorsque vous cliquez sur **créer le correctif**, l’application télécharge le fichier image spécifié et la stocke dans le service Blob.

### <a name="set-up-the-blob-container"></a>Configurer le conteneur d’objets Blob

Afin de stocker un fichier dans le service Blob que vous avez besoin une *conteneur* à stocker dans. Un conteneur de service Blob correspond à un dossier de système de fichiers. Les scripts de création d’environnement que nous avons examiné dans le [automatiser tout chapitre](automate-everything.md) créer le compte de stockage, mais ils ne créent pas un conteneur. Par conséquent, l’objectif de la `CreateAndConfigure` méthode de la `PhotoService` classe consiste à créer un conteneur s’il n’existe pas déjà. Cette méthode est appelée à partir de la `Application_Start` méthode dans *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

La clé d’accès et le nom du compte stockage sont stockées dans le `appSettings` collection de la *Web.config* de fichiers et de code dans le `StorageUtils.StorageAccount` méthode utilise ces valeurs pour créer une chaîne de connexion et établir une connexion :

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Le `CreateAndConfigureAsync` méthode crée ensuite un objet qui représente le service Blob, et un objet qui représente un conteneur (dossier) nommé « images » dans le service Blob :

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Si un conteneur nommé « images » n’existe pas encore--qui aura la valeur true, la première fois que vous exécutez l’application sur un nouveau compte de stockage, le code crée le conteneur et définit des autorisations pour la rendre publique. (Par défaut, des conteneurs d’objets blob sont privées et sont uniquement accessibles aux utilisateurs qui sont autorisés à accéder à votre compte de stockage.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Store la photo téléchargée dans le stockage Blob

Pour charger et enregistrer le fichier image, l’application utilise un `IPhotoService` interface et l’implémentation de l’interface dans le `PhotoService` classe. Le *PhotoService.cs* fichier contient tout le code dans l’application Fix It qui communique avec le service Blob.

La méthode de contrôleur MVC suivante est appelée lorsque l’utilisateur clique sur **créer le correctif**. Dans ce code, `photoService` fait référence à une instance de la `PhotoService` (classe), et `fixittask` fait référence à une instance de la `FixItTask` classe d’entité qui stocke les données d’une nouvelle tâche.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Le `UploadPhotoAsync` méthode dans la `PhotoService` classe stocke le fichier chargé dans le service Blob et renvoie une URL qui pointe vers le nouvel objet blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Comme dans le `CreateAndConfigure` (méthode), le code se connecte au compte de stockage et crée un objet qui représente le conteneur d’objets blob « images », mais ici il part du principe que le conteneur existe déjà.

Il crée ensuite un identificateur unique pour l’image sur le point d’être chargé, en concaténant une nouvelle valeur GUID avec l’extension de fichier :

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Le code puis utilise l’objet de conteneur d’objets blob et le nouvel identificateur unique pour créer un objet blob, définit un attribut sur cet objet indiquant quel type de fichier est, elle utilise ensuite l’objet blob pour stocker le fichier dans le stockage blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Enfin, il obtient une URL qui fait référence à l’objet blob. Cette URL sera stockée dans la base de données et peut être utilisée dans les pages web Fix It pour afficher l’image chargée.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Cette URL est stockée dans la base de données en tant qu’une des colonnes de la table FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Avec uniquement l’URL dans la base de données et des images dans le stockage Blob, l’application Fix It conserve la base de données petit, évolutive et économique, tandis que les images sont stockées dans lequel le stockage est bon marché et capable de gérer plusieurs téraoctets ou pétaoctets. Un compte de stockage peut stocker des centaines de téraoctets de photos Fix It, et vous payez uniquement ce que vous utilisez. Par conséquent, vous pouvez commencer petits cents 9 payeurs pour le premier gigaoctet et ajouter des images pour centimes par gigaoctet supplémentaire.

### <a name="display-the-uploaded-file"></a>Afficher le fichier chargé

L’application Fix It affiche le fichier image chargée lors de l’affichage des détails d’une tâche.

![Détails de la tâche de corriger avec photo](unstructured-blob-storage/_static/image3.png)

Pour afficher l’image, la vue MVC effectuer suffit incluent la `PhotoUrl` valeur dans le code HTML envoyé au navigateur. Le serveur web et la base de données n’utilisent pas de cycles pour afficher l’image, ils servent uniquement de quelques octets à l’URL d’image. Dans le code Razor suivant, `Model` fait référence à une instance de la `FixItTask` classe d’entité.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Si vous examinez le code HTML de la page qui affiche, vous consultez l’URL qui pointe directement vers l’image dans le stockage blob, comme celui-ci :

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Récapitulatif

Vous avez vu comment l’application Fix It stocke les images dans le service Blob et les URL des images uniquement dans la base de données SQL. À l’aide du service Blob conserve la base de données SQL est beaucoup plus petite que sinon serait, rend possible à l’échelle jusqu'à un nombre presque illimité de tâches et peut être effectuée sans avoir à écrire beaucoup de code.

Vous pouvez avoir des centaines de téraoctets dans un compte de stockage, et le coût de stockage est beaucoup moins coûteux que le stockage de base de données SQL, en commençant à environ 3 cents par gigaoctet par mois, plus une charge de petites transactions. Et n’oubliez pas que vous ne payez pas pour la capacité maximale, mais uniquement pour la quantité que vous stockez, par conséquent, votre application est prête à l’échelle, mais vous ne payez pas supplémentaire pour toute cette capacité.

Dans le [chapitre suivant](design-to-survive-failures.md) nous allons parler de l’importance de rendre une application cloud capable de gérer correctement les échecs.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez les ressources suivantes :

- [Introduction à stockage BLOB Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog rédigé par Mike bois.
- [Comment utiliser le Service de stockage d’objets Blob Azure dans .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentation officielle sur le site MicrosoftAzure.com. Une brève introduction aux suivis d’exemples de code montrant comment se connecter au stockage d’objets blob, de stockage d’objets blob de créer des conteneurs, charger et télécharger des objets BLOB, etc.
- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de vidéos de neuf parties par Ulrich Homann, Marc Mercuri et Mark Simms. Présente les principaux concepts et principes d’architecture de manière très accessible et intéressante, avec récits dessinés à partir de l’expérience de Microsoft Customer Advisory Team (CAT) de clients réels. Pour une description de service de stockage Azure et les objets BLOB, consultez l’épisode 5 en commençant à 35:13.
- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Modèle de clé de Valet voir.

> [!div class="step-by-step"]
> [Précédent](data-partitioning-strategies.md)
> [Suivant](design-to-survive-failures.md)
