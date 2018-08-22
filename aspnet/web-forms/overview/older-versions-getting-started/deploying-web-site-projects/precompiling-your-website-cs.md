---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Précompilation de votre site Web (c#) | Microsoft Docs
author: rick-anderson
description: 'Visual Studio offre aux développeurs ASP.NET deux types de projets : projets d’Application Web (WAP) et les projets de Site Web (WSP). Parmi le principales différences betwe...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: af11b8f13979eb4613195d1fa30f2c1adf508187
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823677"
---
<a name="precompiling-your-website-c"></a>Précompilation de votre site Web (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio offre aux développeurs ASP.NET deux types de projets : projets d’Application Web (WAP) et les projets de Site Web (WSP). Une des principales différences entre les deux types de projets est que WAP doit avoir le code compilé explicitement avant le déploiement tandis que le code dans un fichier WSP peut être compilé automatiquement sur le serveur web. Toutefois, il est possible de précompiler un fichier WSP avant le déploiement. Ce didacticiel explore les avantages de précompilation et montre comment précompiler un site Web à partir de Visual Studio et à partir de la ligne de commande.


## <a name="introduction"></a>Introduction

Visual Studio offre aux développeurs ASP.NET deux différents types de projets : projets d’Application Web (WAP) et les projets de Site Web (WSP). Une des principales différences entre ces types de projets est que WAP nécessite *compilation explicite* tandis que wsp utilisent *compilation automatique*, par défaut. Avec WAP, vous compilez le code de l’application web dans un assembly unique, qui est créé dans le site de Web `Bin` dossier. Déploiement implique la copie le contenu du balisage (le `.aspx.ascx`, et `.master` fichiers) dans le projet, ainsi que l’assembly dans le `Bin` dossier ; le code-behind des fichiers de classe eux-mêmes ne doivent pas être déployés. En revanche, vous déployez wsp en copiant les pages de balisage et leurs classes de code-behind correspondant à l’environnement de production. Les classes de code-behind sont compilés à la demande sur le serveur web.

> [!NOTE]
> Revenir à la section « Explicite Compilation par rapport à une Compilation automatique » dans le [ *déterminer quels fichiers doivent être déployés* didacticiel](determining-what-files-need-to-be-deployed-cs.md) pour plus d’informations sur les différences entre le projet modèles, compilation automatique et explicite, et comment le modèle de compilation affecte le déploiement.


L’option de compilation automatique est simple à utiliser. Aucune étape de compilation explicite, et seuls les fichiers qui ont été modifiées doivent être déployés, tandis que la compilation explicite nécessite de déployer les pages modifiées de balisage et l’assembly compilé juste-. Toutefois, le déploiement automatique présente deux inconvénients potentiels :

- Étant donné que les pages doivent être compilés automatiquement quand elles sont consultées tout d’abord, il peut y avoir un délai court mais notable lorsqu’une page ASP.NET est demandée pour la première fois après son déploiement.
- Compilation automatique requiert que les deux le balisage et la source de code déclaratif sur le serveur web. Cela peut être une option intéressante si vous comptez vendre de l’application web pour les clients qui installeront sur leurs serveurs web.

Si soit de deux ci-dessus défauts lexicaux de la transaction, vous pouvez soit basculer vers le modèle WAP ou *précompiler* WSP avant le déploiement. Ce didacticiel examine les options de précompilation idéal pour un site Web hébergé et guide à travers le processus de précompilation et le déploiement d’un site Web précompilé.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Une vue d’ensemble de la génération de Code ASP.NET et la Compilation

Avant d’examiner les options de précompilation disponibles, nous allons tout d’abord parler la génération de code et la compilation qui se produit lorsqu’une page ASP.NET est demandée pour la première fois dans la mesure où elle a été créée ou dernière mise à jour. Comme vous le savez, les pages ASP.NET sont composés de deux parties : un balisage déclaratif dans le `.aspx` fichier ; et une partie du code source, généralement dans un fichier de classe code-behind distinct (`.aspx.cs`). Les étapes effectuées par le runtime lors de la demande d’une page ASP.NET dépend du modèle de compilation de l’application.

Avec WAP, code de source de la pages doit être explicitement compilé dans un assembly unique avant le déploiement. Au cours du déploiement, cet assembly et les différentes pages de balisage sont copiés dans l’environnement de production. Lorsqu’une requête arrive sur le serveur web d’une page ASP.NET, le runtime crée une instance de la classe code-behind de la page et appelle son `ProcessRequest` (méthode), qui démarre le cycle de vie de page et, finalement, génère le contenu de page, qui est renvoyé au le demandeur. Le runtime peut fonctionner avec la classe de code-behind de la page ASP.NET, car la classe code-behind était déjà compilée dans un assembly avant le déploiement.

Avec wsp et compilation automatique, il n’existe aucune étape de compilation explicite avant le déploiement. Au lieu de cela, déploiement implique la copie des déclaratif et le contenu de code source dans l’environnement de production. Lorsqu’une requête arrive sur le serveur web d’une page ASP.NET pour la première fois dans la mesure où la page a été créée ou mises à jour, le runtime doit tout d’abord compiler la classe code-behind dans un assembly. Cet assembly compilé est enregistré dans le dossier `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, bien que l’emplacement de ce dossier peut être personnalisé via le `<compilation tempDirectory="" />` élément de `<system.web>`, généralement dans `Web.config`. Étant donné que l’assembly est enregistré sur le disque, il est inutile d’être recompilé pour les requêtes ultérieures à la même page.

> [!NOTE]
> Comme vous pouvez l’imaginer, il existe un léger délai lors de la demande une page pour la première fois (ou pour la première fois dans la mesure où elle a été modifiée) dans un site qui utilise une compilation automatique car peut prendre quelques instants pour le serveur à compiler le code de la page et enregistrer l’assembly résultant à disque.


En bref, avec la compilation explicite vous sont requis pour compiler source code du site Web avant le déploiement, l’enregistrement de l’exécution d’avoir à effectuer cette étape. Lors d’une compilation automatique le runtime gère la compilation de code source des pages, mais avec un coût d’initialisation légère pour la première visite à la page, car il a été créé ou mis à jour.

Mais qu’en est-il de la partie déclarative des pages ASP.NET (le `.aspx` fichier) ? Il est évident qu’il existe une relation entre le `.aspx` le code dans leurs classes de code-behind, comme les contrôles Web définis dans le balisage déclaratif et les fichiers sont accessibles dans le code. Il est également évident que le contenu de la `.aspx` fichiers influence dans le balisage rendu généré par la page. Par conséquent, comment le runtime fonctionne-t-il avec le texte, HTML, et la syntaxe des contrôles Web définis dans le `.aspx` fichier pour générer la page demandée du contenu rendu ?

Je ne souhaite pas obtenir trop dévié de votre sujet sur les détails d’implémentation de bas niveau, qui varient entre WAP et wsp, mais en bref, le runtime génère automatiquement un fichier de classe qui contient les différents contrôles Web en tant que membres protégés et des méthodes. Ce fichier généré est implémenté comme un *classe partielle* à la classe code-behind correspondant. ([Des classes partielles](http://www.dotnet-guide.com/partialclasses.html) autoriser pour le contenu d’une classe unique pour être réparties sur plusieurs fichiers.) Par conséquent, la classe code-behind est définie dans deux emplacements : dans le `.aspx.cs` fichier que vous créez et dans cette classe générée automatiquement créée par le runtime. Cette classe générée automatiquement est stockée dans le `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` dossier.

L’important emporter ici est que, pour une page ASP.NET doit être affiché par le runtime à la fois son déclarative et parties du code source doivent être compilés dans un assembly. Avec WAP, le code source est explicitement compilé dans un assembly avant le déploiement, mais le balisage déclaratif doit toujours être converti en code et compilé par le runtime sur le serveur web. Avec wsp à l’aide d’une compilation automatique, le code source et le balisage déclaratif doivent être compilées par le serveur web.

Il est possible d’utiliser la compilation explicite avec le modèle WSP. Vous pouvez explicitement compiler la portion de code source, comme avec le modèle WAP. De plus, vous pouvez également compiler le balisage déclaratif.

## <a name="precompilation-options"></a>Options de précompilation

Le .NET Framework est livré avec un [outil de compilation ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) qui vous permet de compiler le code source (et même le contenu) d’une application ASP.NET créée à l’aide du modèle WSP. Cet outil a été publié avec la version 2.0 du .NET Framework et se trouve dans le `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` dossier ; il peut être utilisé à partir de la ligne de commande ou lancé au sein de Visual Studio via l’option Publier le Site Web du menu Build.

L’outil de compilation fournit deux formes générales de la compilation : la précompilation et précompilation pour le déploiement sur place. Avec la précompilation sur place, vous exécutez le `aspnet_compiler.exe` l’outil en ligne de commande et spécifiez le chemin d’accès du répertoire virtuel ou un chemin d’accès physique d’un site Web qui se trouve sur votre ordinateur. L’outil de compilation compile ensuite chaque page ASP.NET dans le projet, stockage de la version compilée dans le `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` dossier comme si les pages avaient chacun été visités pour la première fois à partir d’un navigateur. La précompilation sur place peut accélérer la première demande adressée à des pages ASP.NET qui vient d’être déployés sur votre site, car elle allège le runtime d’avoir à effectuer cette étape. Toutefois, la précompilation sur place n’est pas utile pour la majorité des sites Web hébergés, car elle nécessite que vous êtes en mesure d’exécuter des programmes à partir du serveur web en ligne de commande. Dans les environnements d’hébergement partagés, ce niveau d’accès n’est pas autorisé.

> [!NOTE]
> Pour plus d’informations sur la précompilation sur place, consultez [How To : précompiler des Sites Web ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) et [précompilation dans ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Au lieu de compiler les pages dans le site Web pour le `Temporary ASP.NET Files` dossier, la précompilation pour le déploiement compile les pages dans un répertoire de votre choix et dans un format qui peut être déployé à l’environnement de production.

Il existe deux types de précompilation pour le déploiement nous explorerons dans ce didacticiel : précompilation avec une interface utilisateur d’être mise à jour et précompilation avec une interface utilisateur non modifiable. La précompilation avec une interface utilisateur d’être mise à jour quitte le balisage déclaratif dans le `.aspx`, `.ascx`, et `.master` fichiers, ce qui permet un développeur afficher et, si vous le souhaitez, modifiez le balisage déclaratif sur le serveur de production. Génère la précompilation avec une interface utilisateur non modifiable `.aspx` pages void de n’importe quel contenu et supprime `.ascx` et `.master` fichiers, ainsi masquer le balisage déclaratif et interdire à un développeur à sa modification de la environnement de production.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Précompilation pour le déploiement avec une Interface utilisateur d’être mise à jour

La meilleure façon de comprendre la précompilation pour le déploiement consiste à voir un exemple en action. Nous allons précompiler livre révisions WSP pour le déploiement à l’aide d’une interface utilisateur d’être mise à jour. L’outil de compilation ASP.NET peut être appelée à partir du menu de génération de Visual Studio ou à partir de la ligne de commande. Cette section examine à l’aide de l’outil à partir de Visual Studio. la section « précompilation à partir de la ligne de commande » examine l’outil compilateur en cours d’exécution à partir de la ligne de commande.

Ouvrez le WSP de révision de livre dans Visual Studio, dans le menu Build et sélectionnez l’option de menu Publier le Site Web. Cette action lance la boîte de dialogue Publier le Site Web (voir **Figure 1**), où vous pouvez spécifier l’emplacement cible, interface d’utilisateur du site précompilé soit ou non être mise à jour et autres options de l’outil du compilateur. L’emplacement cible peut être un serveur web à distance ou un serveur FTP, mais pour l’instant, choisissez un dossier sur votre disque dur. Étant donné que nous souhaitons précompiler le site avec une interface utilisateur d’être mise à jour, laissez la case à cocher « Autoriser ce site précompilé à être mis à jour » activée et cliquez sur OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Figure 1**: l’outil de Compilation ASP.NET sera précompiler votre site Web à l’emplacement cible spécifié  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> L’option Publier le Site Web dans le menu Build n’est pas disponible dans Visual Web Developer. Si vous utilisez Visual Web Developer, vous devez utiliser la version de ligne de commande de l’outil de compilation ASP.NET, qui est présentée dans la section « précompilation à partir de la ligne de commande ».


Après avoir précompilé le site Web, accédez à l’emplacement cible que vous avez entré dans la boîte de dialogue Publier le Site Web. Prenez un moment pour comparer le contenu de ce dossier avec le contenu de votre site Web. **Figure 2** affiche le dossier de site Web critiques de livres. Notez qu’il contient à la fois `.aspx` et `.aspx.cs` fichiers. Notez également que le `Bin` répertoire inclut uniquement un seul fichier, `Elmah.dll`, dont nous avons ajouté dans le [didacticiel précédent](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Figure 2**: le répertoire du projet contient `.aspx` et `.aspx.cs` fichiers ; le `Bin` dossier inclut uniquement `Elmah.dll`  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image6.png))

**Figure 3** affiche le dossier d’emplacement cible dont le contenu ont été créé par l’outil de compilation ASP.NET. Ce dossier ne contient pas tous les fichiers code-behind. En outre, de ce dossier `Bin` répertoire inclut plusieurs assemblys et deux `.compiled` fichiers en plus de la `Elmah.dll` assembly.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Figure 3**: le dossier d’emplacement cible inclut les fichiers pour le déploiement  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image9.png))

Contrairement à la compilation explicite dans WAP, la précompilation pour le processus de déploiement ne crée pas un assembly pour l’ensemble du site. Au lieu de cela, elle traite par lot ensemble plusieurs pages dans chaque assembly. Il compile également le `Global.asax` de fichiers (le cas échéant) dans son propre assembly, ainsi que toutes les classes dans le `App_Code` dossier. Les fichiers qui contiennent le balisage déclaratif pour ASP.NET web pages, les contrôles utilisateur et les pages maîtres (`.aspx`, `.ascx`, et `.master` des fichiers, respectivement) sont copiés en tant que-est dans le répertoire d’emplacement cible. De même, le `Web.config` fichier est copié directement, ainsi que tous les fichiers statiques, tels que des images, des classes CSS et des fichiers PDF. Pour obtenir une description plus formelle de la façon dont l’outil de compilation gère différents types de fichiers, reportez-vous à [gestion de fichier au cours de précompilation ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Vous pouvez demander à l’outil de compilation pour créer un assembly par page ASP.NET, contrôle utilisateur ou page maître en cochant la case « Utilisé fixé d’affectation de noms et assemblys d’une page unique » à partir de la boîte de dialogue Publier le Site Web. Le fait d’avoir chaque page ASP.NET compilé dans son propre assembly permet un contrôle plus précis sur le déploiement. Par exemple, si vous mis à jour d’une page web ASP.NET et nécessaires pour déployer cette modification, vous devez déployer uniquement de cette page `.aspx` fichier et assembly associé à l’environnement de production. Consultez [Comment : générer des noms fixes avec l’outil de Compilation ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) pour plus d’informations.


Le répertoire d’emplacement cible contient également un fichier qui n’était pas inclus dans le projet web précompilé, à savoir `PrecompiledApp.config`. Ce fichier informe le runtime ASP.NET que l’application a été précompilée et si elle a été précompilé avec une interface utilisateur de mettre à jour ou mise à jour de midi.

Enfin, prenez un moment pour ouvrir un de le `.aspx` fichiers à l’emplacement cible à l’aide de Visual Studio ou votre éditeur de texte de choix. Lors de la précompilation pour le déploiement avec une interface utilisateur d’être mise à jour, les pages ASP.NET dans le répertoire d’emplacement cible contiennent le même balisage exactement comme les fichiers correspondants dans le site Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Précompilation pour le déploiement avec une Interface utilisateur Non modifiable

L’outil de compilateur ASP.NET peut également servir à précompiler un site pour le déploiement avec une interface utilisateur non modifiable. Précompiler le site avec une interface utilisateur non modifiable fonctionne comme la précompilation avec une interface utilisateur être mise à jour, la principale différence étant que les pages ASP.NET, les contrôles utilisateur et les pages maîtres dans le répertoire cible sont supprimés de leurs balises. Pour précompiler un site Web pour le déploiement avec une interface utilisateur non modifiable, choisissez l’option Publier le Site Web dans le menu Générer, mais décochez l’option « Autoriser ce site précompilé à être mis à jour » (voir **Figure 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Figure 4**: désactivez le « Autoriser ce site précompilé à être mis à jour » Option à précompiler avec une mise à jour Non interface utilisateur  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image12.png))

**Figure 5** montre le dossier d’emplacement cible après avoir précompilé avec une interface utilisateur non modifiable.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Figure 5**: le dossier d’emplacement cible pour le déploiement avec une interface utilisateur Non modifiable  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image15.png))

Comparer **Figure 3** à **Figure 5**. Bien que les deux dossiers d’aspect identiques, notez que le dossier non modifiable de l’interface utilisateur ne dispose pas de la page maître, `Site.master`. Et alors que **Figure 5** inclut les différentes pages ASP.NET, si vous affichez le contenu de ces fichiers, vous verrez avoir été supprimés de leur balisage déclaratif et remplacés par le texte d’espace réservé : « Voici un fichier marqueur généré par l’outil de précompilation et ne doit pas être supprimé ! »

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Figure 5**: le balisage déclaratif a été supprimé à partir des Pages ASP.NET

Le `Bin` dossiers dans **Figures 3** et **5** diffèrent de plus grande partie. En plus des assemblys, le `Bin` dossier **Figure 5** inclut un `.compiled` fichier pour chaque page ASP.NET, contrôle utilisateur et la page maître.

La précompilation d’un site avec une interface utilisateur non modifiable est utile dans les situations où vous ne souhaitez pas de contenu des pages ASP.NET doivent être modifiées par la personne ou de la société qui installe ou gère le site Web dans l’environnement de production. Si vous générez une application web ASP.NET que vous vendez aux clients d’installer sur leurs propres serveurs web, vous souhaiterez vous assurer qu’ils ne modifient pas l’apparence de votre site en modifiant directement le `.aspx` pages les expédier. En recompilant votre site Web avec une interface utilisateur non modifiable, vous expédiez l’espace réservé `.aspx` pages dans le cadre de l’installation, empêchant ainsi vos clients d’examen ou de modifier leur contenu.

### <a name="precompiling-from-the-command-line"></a>La précompilation à partir de la ligne de commande

Dans les coulisses, boîte de dialogue Publier le Site Web de Visual Studio appelle l’outil de compilation ASP.NET (`aspnet_compiler.exe`) pour précompiler le site Web. Vous pouvez également appeler cet outil à partir de la ligne de commande. En fait, si vous utilisez Visual Web Developer puis vous devrez exécuter l’outil compilateur de ligne de commande, comme le menu Générer de Visual Web Developer n’inclut pas l’option Publier le Site Web.

Pour utiliser l’outil compilateur de ligne de commande, commencez par la suppression de la ligne de commande et en accédant au répertoire du framework, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Ensuite, entrez l’instruction suivante dans la ligne de commande :

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

La commande ci-dessus lance l’outil de compilateur ASP.NET (`aspnet_compiler.exe`) et, par le biais de la `-p` commutateur, lui indique de précompiler le site Web racine est à *physique\_chemin d’accès\_à\_application*; Cette valeur sera quelque chose comme `C:\MySites\BookReviews`et doit être délimité par des guillemets.

Le `-v` commutateur spécifie le répertoire virtuel du site. Si votre site est inscrit en tant que le site Web par défaut dans la métabase IIS, vous pouvez omettre le `-p` basculer et il suffit de spécifier le répertoire virtuel de l’application. Si vous utilisez le `-p` basculer, la procédure de la valeur la `-v` commutateur indique la racine du site Web et est utilisé pour résoudre les références de la racine de l’application. Par exemple, si vous spécifiez une valeur de `-v /MySite` puis référence dans l’application à `~/path/file` sera résolu en tant que `~/MySite/path/file`. Étant donné que le site de critiques de livres se trouve dans le répertoire racine à mon entreprise d’hébergement web, j’ai utilisé le commutateur `-v /`.

Le `-f` , le cas échéant, demande à l’outil de compilation pour remplacer le *cible\_emplacement\_dossier* répertoire s’il existe déjà. Si vous omettez le `-f` commutateur et le dossier d’emplacement cible existe déjà, l’outil de compilation s’arrêtera avec l’erreur : « erreur ASPRUNTIME : le répertoire cible n’est pas vide. Veuillez le supprimer manuellement ou choisissez une autre cible. »

Le `-u` commutateur, le cas échéant, informe l’outil pour créer une interface utilisateur d’être mise à jour. Ignorez ce commutateur pour précompiler le site avec une interface utilisateur non modifiable.

Enfin, le *cible\_emplacement\_dossier* est le chemin d’accès physique vers le répertoire d’emplacement cible ; cette valeur sera quelque chose comme `C:\MySites\Output\BookReviews`et doit être délimité par des guillemets.

## <a name="deploying-the-precompiled-website"></a>Déploiement du site Web précompilé

À ce stade, nous avons vu comment utiliser l’outil de compilation ASP.NET pour précompiler un site Web à l’aide de ces deux options d’interface utilisateur d’être mise à jour et non modifiable. Toutefois, nos exemples jusqu'à présent avaient précompilé le site Web à un dossier local et non à l’environnement de production. La bonne nouvelle est que le déploiement du site Web précompilé est un jeu d’enfant et peut être effectuée via Visual Studio ou un autre mécanisme de copie de fichier, telles que depuis un client FTP autonome.

La boîte de dialogue Publier le Site Web (affichée en premier dans **Figure 1**) dispose d’une option d’emplacement cible, ce qui indique où sont copiés les fichiers du site Web précompilé à. Cet emplacement peut être un serveur web à distance ou un serveur FTP. Saisie d’un serveur distant dans cette zone de texte précompile et déploie le site Web sur le serveur spécifié en une seule étape. Ou bien, vous pouvez précompiler le site Web dans un dossier local et puis copiez manuellement le contenu de ce dossier à l’environnement de production via FTP ou une autre approche.

Site Web précompilé automatiquement déployé via la boîte de dialogue Publier le Site Web de Visual Studio s’avère utile pour les sites simples lorsqu’il n’y a aucune différence de configuration entre les environnements de développement et de production. Toutefois, comme indiqué dans le [ *différences entre développement Configuration commun et Production* didacticiel](common-configuration-differences-between-development-and-production-cs.md) il n’est pas rare pour telles différences existent. Par exemple, l’application web de critiques de livres utilise une autre base de données dans l’environnement de production que dans l’environnement de développement. Quand Visual Studio publie le site Web à un serveur distant, qu'il copie les informations du fichier de configuration dans l’environnement de développement.

Pour les sites avec des différences de configuration entre les environnements de développement et de production, il peut être préférable de précompiler le site dans un répertoire local, recopiez les fichiers de configuration spécifiques à la production et copiez le contenu de la sortie précompilé production.

Pour un rappel sur la copie des fichiers à partir de l’environnement de développement sur l’environnement de production, reportez-vous à la [ *déploiement de votre site Web à l’aide d’un FTP Client* ](deploying-your-site-using-an-ftp-client-cs.md) et [  *Déploiement de votre site Web à l’aide de Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md) didacticiels.

## <a name="summary"></a>Récapitulatif

ASP.NET prend en charge deux modes de compilation : automatique et explicite. Comme indiqué dans les didacticiels précédents, les projets d’Application Web (WAP) utilisation de la compilation explicite tandis que les projets de Site Web (WSP) utilisent une compilation automatique, par défaut. Toutefois, il est possible de compiler un fichier WSP avant le déploiement à l’aide de l’outil de compilation ASP.NET.

Ce didacticiel se concentre sur la précompilation de l’outil de compilation pour la prise en charge du déploiement. Lors de la précompilation pour le déploiement, l’outil de compilation crée un dossier d’emplacement cible, compile le code source de l’application web spécifié et copie ces compilé les assemblys et les fichiers de contenu dans le dossier d’emplacement cible. L’outil de compilation peut être configuré pour créer une interface utilisateur d’être mise à jour ou non modifiable. Lors de la précompilation avec une interface utilisateur non modifiable, le balisage déclaratif dans les fichiers de contenu est supprimé. En bref, la précompilation vous autorise à déployer votre application basée sur le projet de Site Web sans y compris les fichiers de code source et avec le balisage déclaratif supprimé, si vous le souhaitez.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Précompilation du Site Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Code-behind et la Compilation dans ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Précompilation dans ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Options de Site précompilé dans ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Précédent](logging-error-details-with-elmah-cs.md)
> [Suivant](users-and-roles-on-the-production-website-cs.md)
