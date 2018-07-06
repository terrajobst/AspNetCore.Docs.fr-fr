---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Traitement des Exceptions non gérées (VB) | Microsoft Docs
author: rick-anderson
description: Lorsqu’une erreur d’exécution se produit sur une application web en production, il est important pour avertir un développeur et d’enregistrer l’erreur afin qu’il peut être diagnostiqué en a la...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: f925a2e2a8cf2785aa2df89c82d2a29965a543d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835228"
---
<a name="processing-unhandled-exceptions-vb"></a>Traitement des Exceptions non gérées (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([procédure de téléchargement](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Lorsqu’une erreur d’exécution se produit sur une application web en production, il est important pour avertir un développeur et d’enregistrer l’erreur afin qu’il peut être diagnostiqué à un moment ultérieur dans le temps. Ce didacticiel fournit une présentation de la façon dont ASP.NET traite les erreurs d’exécution et examine une façon d’exécuter un code personnalisé chaque fois qu’un bulles d’exception non gérée à l’exécution d’ASP.NET.


## <a name="introduction"></a>Introduction

Lorsqu’une exception non gérée se produit dans une application ASP.NET, il se propage jusqu'à l’exécution d’ASP.NET, ce qui déclenche le `Error` événement et affiche la page d’erreur approprié. Il existe trois types de pages d’erreur : le Runtime erreur jaune écran de décès (YSOD) ; les détails d’Exception YSOD ; et les pages d’erreurs personnalisées. Dans le [didacticiel précédent](displaying-a-custom-error-page-vb.md) nous avons configuré l’application pour utiliser une page d’erreur personnalisée pour les utilisateurs distants et le YSOD de détails d’Exception pour les utilisateurs qui accèdent localement.

À l’aide d’une page d’erreur personnalisée conviviale qui correspond à l’apparence du site est préférée à la valeur par défaut YSOD d’erreur de Runtime, mais afficher une page d’erreur personnalisée n'est qu’une partie d’une solution de gestion d’erreur complexe. Lorsqu’une erreur se produit dans une application en production, il est important que les développeurs sont informés de l’erreur afin qu’ils peuvent pointe la cause de l’exception et les résoudre. En outre, les détails de l’erreur doivent être consignés afin que l’erreur peut être examinée et de diagnostiquer les problèmes plus tard dans le temps.

Ce didacticiel montre comment accéder aux détails d’une exception non gérée afin qu’ils peuvent être enregistrés et un développeur averti. Les didacticiels de deux suivent explorent les bibliothèques de journalisation erreur qui, après un peu de configuration, avertir les développeurs d’erreurs d’exécution et consigner les détails de leurs automatiquement.

> [!NOTE]
> Les informations examinées dans ce didacticiel sont particulièrement utiles si vous avez besoin traiter les exceptions non gérées d’une manière unique ou personnalisée. Dans les cas où vous devez uniquement consigner l’exception et de notifier un développeur, à l’aide d’une bibliothèque de journalisation d’erreur est la meilleure option. Les deux didacticiels fournissent une vue d’ensemble de deux de ces bibliothèques.


## <a name="executing-code-when-theerrorevent-is-raised"></a>L’exécution de Code lorsque le`Error`événement est déclenché.

Événements fournissent un mécanisme permettant de signaler que quelque chose d’intéressant s’est produite et pour un autre objet exécuter du code en réponse à un objet. En tant que développeur d’ASP.NET, vous êtes habitué à penser en termes d’événements. Si vous souhaitez exécuter du code lorsque le visiteur clique sur un bouton particulier, vous créez un gestionnaire d’événements de ce bouton `Click` événements et votre code il. Étant donné que le runtime ASP.NET déclenche son [ `Error` événement](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) chaque fois qu’une exception non gérée se produit, il s’ensuit que le code permettant d’enregistrer les détails de l’erreur se trouve dans un gestionnaire d’événements. Mais comment créer un gestionnaire d’événements pour le `Error` événement ?

Le `Error` événement est un des nombreux événements dans le [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) qui sont déclenchés à certaines étapes dans le pipeline HTTP pendant la durée de vie d’une demande. Par exemple, le `HttpApplication` la classe [ `BeginRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) est déclenché au début de chaque demande ; son [ `AuthenticateRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) est déclenché lorsqu’un module de sécurité a identifié le demandeur. Ces `HttpApplication` événements donnent le développeur de pages un moyen d’exécuter une logique personnalisée aux divers points dans la durée de vie d’une demande.

Gestionnaires d’événements pour le `HttpApplication` événements peuvent être placés dans un fichier spécial nommé `Global.asax`. Pour créer ce fichier dans votre site Web, ajoutez un nouvel élément à la racine de votre site Web à l’aide du modèle de classe d’Application globale portant le nom `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Figure 1**: ajoutez `Global.asax` à votre Application Web  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image3.png))

Le contenu et la structure de la `Global.asax` fichier créé par Visual Studio varier légèrement en fonction de si vous utilisez un projet d’Application Web (WAP) ou d’un projet de Site Web (WSP). Avec un proxy d’application Web, le `Global.asax` est implémenté en tant que deux fichiers distincts - `Global.asax` et `Global.asax.vb`. Le `Global.asax` fichier ne contient rien mais un `@Application` directive qui référence le `.vb` fichier ; l’événement gestionnaires d’intérêt sont définis dans le `Global.asax.vb` fichier. Pour wsp, qu’un seul fichier est créé, `Global.asax`, et les gestionnaires d’événements sont définis dans un `<script runat="server">` bloc.

Le `Global.asax` fichier créé dans un proxy d’application Web par le modèle de classe d’Application globale de Visual Studio inclut des gestionnaires d’événements nommés `Application_BeginRequest`, `Application_AuthenticateRequest`, et `Application_Error`, qui sont des gestionnaires d’événements pour le `HttpApplication` événements `BeginRequest`, `AuthenticateRequest`, et `Error`, respectivement. Il existe également des gestionnaires d’événements nommés `Application_Start`, `Session_Start`, `Application_End`, et `Session_End`, qui sont des gestionnaires d’événements qui se déclenchent lorsque l’application web démarre, lorsque le démarrage d’une nouvelle session, lorsque l’application se termine, et quand une session se termine, respectivement. Le `Global.asax` fichier créé dans un fichier WSP par Visual Studio contient simplement le `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, et `Session_End` gestionnaires d’événements.

> [!NOTE]
> Lors du déploiement de l’application ASP.NET vous devez copier le `Global.asax` fichier à l’environnement de production. Le `Global.asax.vb` fichier, qui est créé dans le proxy d’application Web, n’a pas besoin être copiés vers la production, car ce code est compilé dans l’assembly du projet.


Les gestionnaires d’événements créés par le modèle de classe d’Application globale de Visual Studio ne sont pas exhaustifs. Vous pouvez ajouter un gestionnaire d’événements pour les `HttpApplication` événement en nommant le Gestionnaire d’événements `Application_EventName`. Par exemple, vous pouvez ajouter le code suivant à la `Global.asax` fichier pour créer un gestionnaire d’événements pour le [ `AuthorizeRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

De même, vous pouvez supprimer les gestionnaires d’événements créés par le modèle de classe d’Application globale qui ne sont pas nécessaires. Pour ce didacticiel, nous avons besoin uniquement un gestionnaire d’événements pour le `Error` événement ; vous pouvez supprimer les autres gestionnaires d’événements à partir de la `Global.asax` fichier.

> [!NOTE]
> *Les Modules HTTP* offrent une autre manière de définir des gestionnaires d’événements pour `HttpApplication` événements. Les Modules HTTP sont créés en tant qu’un fichier de classe qui peut être placé directement dans le projet d’application web ou séparé et répartis dans une bibliothèque de classes distincte. Car ils peuvent être séparées dans une bibliothèque de classes, des Modules HTTP offrent un modèle plus flexible et réutilisable pour la création de `HttpApplication` gestionnaires d’événements. Tandis que le `Global.asax` fichier est spécifique à l’application web dans lequel il réside, les Modules HTTP peut être compilés dans des assemblys, à quel point l’ajout du HTTP Module à un site Web est aussi simple que la suppression de l’assembly le `Bin` dossier et l’enregistrement le Module dans `Web.config`. Ce didacticiel ne recherche pas dans la création et l’utilisation des Modules HTTP, mais les bibliothèques de journalisation des erreurs deux utilisées dans les deux didacticiels suivants sont implémentées sous forme de Modules HTTP. Pour plus d’informations sur les avantages de Modules HTTP, reportez-vous à [à l’aide des Modules HTTP et des gestionnaires pour créer des composants enfichables ASP.NET](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Récupérer des informations sur l’Exception non gérée

À ce stade, nous avons un fichier Global.asax avec une `Application_Error` Gestionnaire d’événements. Lorsque ce gestionnaire d’événements s’exécute que nous devons informer un développeur de l’erreur et de consigner ses détails. Pour accomplir ces tâches, nous devons tout d’abord déterminer les détails de l’exception qui a été déclenché. Utilisez l’objet serveur [ `GetLastError` méthode](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) pour récupérer les détails de l’exception non gérée qui a provoqué le `Error` l’événement.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

Le `GetLastError` méthode retourne un objet de type `Exception`, qui est le type de base pour toutes les exceptions dans le .NET Framework. Toutefois, dans le code ci-dessus je suis cast de l’objet Exception renvoyée par `GetLastError` dans un `HttpException` objet. Si le `Error` événement est déclenché, car une exception a été levée pendant le traitement d’une ressource ASP.NET, puis l’exception qui a été levée est encapsulée dans un `HttpException`. Pour obtenir l’exception réelle qui a activé l’utilisation d’événements erreur le `InnerException` propriété. Si le `Error` événement a été déclenché suite à une exception basée sur HTTP, telle qu’une demande pour une page inexistante, un `HttpException` est levée, mais il ne dispose pas d’une exception interne.

Le code suivant utilise la `GetLastErrormessage` pour récupérer les informations relatives à l’exception qui a déclenché le `Error` événement, stocker le `HttpException` dans une variable nommée `lastErrorWrapper`. Il stocke ensuite le type, le message et la trace de la pile de l’exception d’origine dans trois variables de chaîne, de vérifier si le `lastErrorWrapper` est l’exception réelle qui a déclenché le `Error` événement (dans le cas d’exceptions basé sur HTTP) ou si elle est simplement un wrapper pour une exception a été levée lors du traitement de la demande.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

À ce stade, vous avez toutes les informations que vous avez besoin d’écrire du code qui enregistre les détails de l’exception dans une table de base de données. Vous pouvez créer une table de base de données avec des colonnes pour chacune des détails de l’erreur d’intérêt - le type, le message, la trace de la pile et ainsi de suite - ainsi que d’autres informations utiles, telles que l’URL de la page demandée et le nom de l’utilisateur actuellement connecté. Dans le `Application_Error` Gestionnaire d’événements vous ensuite se connecter à la base de données et insérer un enregistrement dans la table. De même, vous pouvez ajouter le code pour un développeur de l’erreur par courrier électronique d’alerte.

Les bibliothèques de journalisation erreur examinés dans les deux didacticiels fournissent cette fonctionnalité dès le départ, il est donc pas nécessaire pour générer cette journalisation des erreurs et la notification vous-même. Toutefois, pour montrer que le `Error` événement est déclenché et que le `Application_Error` Gestionnaire d’événements peut être utilisé pour consigner les détails de l’erreur et notifier un développeur, nous allons ajouter du code qui notifie un développeur lorsqu’une erreur se produit.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notifier un développeur lorsqu’une Exception non gérée se produit

Lorsqu’une exception non gérée se produit dans l’environnement de production, il est important alerter l’équipe de développement afin qu’ils peuvent évaluer l’erreur et déterminer les actions à entreprendre. Par exemple, s’il existe une erreur dans la connexion à la base de données, vous devez double Vérifiez votre chaîne de connexion et, par exemple, ouvrez un ticket de support avec votre entreprise d’hébergement web. Si l’exception s’est produite en raison d’une erreur de programmation, la logique de validation ou de code supplémentaire peut-être à ajouter à éviter ces erreurs à l’avenir.

Les classes .NET Framework dans le [ `System.Net.Mail` espace de noms](https://msdn.microsoft.com/library/system.net.mail.aspx) rendent simple pour envoyer un courrier électronique. Le [ `MailMessage` classe](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) représente un message électronique et possède des propriétés comme `To`, `From`, `Subject`, `Body`, et `Attachments`. Le `SmtpClass` est utilisé pour envoyer un `MailMessage` à l’aide d’un serveur SMTP spécifié de l’objet ; les paramètres du serveur SMTP peuvent être spécifiées par programmation ou de façon déclarative dans le [ `<system.net>` élément](https://msdn.microsoft.com/library/6484zdc1.aspx) dans le `Web.config file`. Pour plus d’informations sur l’envoi de courrier électronique des messages dans une application ASP.NET consultez mon article, [envoi de courrier électronique dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)et le [System.Net.Mail FAQ](http://systemnetmail.com/).

> [!NOTE]
> Le `<system.net>` élément contient les paramètres du serveur SMTP utilisés par le `SmtpClient` classe lors de l’envoi d’un message électronique. Votre entreprise d’hébergement web a un serveur SMTP que vous pouvez utiliser pour envoyer un e-mail à partir de votre application. Pour plus d’informations sur les paramètres du serveur SMTP que vous devez utiliser dans votre application web, consultez la section de prise en charge de votre hôte web.


Ajoutez le code suivant à la `Application_Error` Gestionnaire d’événements à envoyer un e-mail d’un développeur lorsqu’une erreur se produit :

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Alors que le code ci-dessus est assez long, la majeure partie de celui-ci crée le HTML qui s’affiche dans l’e-mail envoyé au développeur. Le code commence par référencer le `HttpException` retourné par la `GetLastError` (méthode) (`lastErrorWrapper`). L’exception réelle qui a été déclenchée par la demande est récupérée `lastErrorWrapper.InnerException` et est affectée à la variable `lastError`. Le type, le message et la pile des informations de trace sont extraites de `lastError` et stockées dans trois variables de chaîne.

Ensuite, un `MailMessage` objet nommé `mm` est créé. Le corps du message est au format HTML et affiche l’URL de la page demandée, le nom de l’utilisateur actuellement connecté et des informations sur l’exception (le type de message et trace de pile). Un des aspects plus intéressants du `HttpException` classe est que vous pouvez générer le code HTML utilisé pour créer l’Exception détails jaune écran de décès (YSOD) en appelant le [GetHtmlErrorMessage méthode](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Cette méthode est utilisée ici pour récupérer le balisage YSOD de détails d’Exception et l’ajouter à l’adresse e-mail en pièce jointe. Un mot de prudence : si l’exception qui a déclenché le `Error` événement a été une exception basée sur HTTP (par exemple, une demande pour une page inexistante) puis le `GetHtmlErrorMessage` méthode retournera `null`.

L’étape finale consiste à envoyer le `MailMessage`. Cela s’effectue en créant un nouveau `SmtpClient` (méthode) et en appelant son `Send` (méthode).

> [!NOTE]
> Avant d’utiliser ce code dans votre application web, vous souhaiterez modifier les valeurs dans le `ToAddress` et `FromAddress` constantes de support@example.com à toute adresse e-mail adresse l’e-mail de notification d’erreur doivent être envoyées à et provenir. Vous devez également spécifier les paramètres du serveur SMTP dans le `<system.net>` section `Web.config`. Consultez votre fournisseur d’hébergement web pour déterminer les paramètres du serveur SMTP à utiliser.


Chaque fois qu’une erreur s’est le développeur est envoyé avec ce code en place un message électronique qui résume l’erreur et inclut le YSOD. Dans le didacticiel précédent, nous avons présenté une erreur d’exécution en visitant Genre.aspx et en transmettant un non valide `ID` valeur via la chaîne de requête, comme `Genre.aspx?ID=foo`. Visiter la page avec le `Global.asax` fichier sur place génère la même expérience utilisateur, comme dans le didacticiel précédent - dans l’environnement de développement vous continuerez à voir l’Exception détails jaune écran de mort, tandis que dans l’environnement de production, vous allez consultez la page d’erreur personnalisée. En plus de ce comportement existant, le développeur reçoit un e-mail.

**Figure 2** montrant l’e-mail reçu lors de la visite `Genre.aspx?ID=foo`. Le corps du message résume les informations sur l’exception, tandis que le `YSOD.htm` pièce jointe affiche le contenu qui est indiqué dans le YSOD de détails d’Exception (consultez **Figure 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Figure 2**: le développeur est envoyé une Notification par E-mail chaque fois qu’une Exception non gérée  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Figure 3**: la Notification par courrier électronique inclut les détails d’Exception YSOD comme pièce jointe  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Que se passe-t-il si nous utilisions la Page d’erreur personnalisée ?

Ce didacticiel vous a montré comment utiliser `Global.asax` et `Application_Error` Gestionnaire d’événements pour exécuter du code lorsqu’une exception non gérée se produit. Plus précisément, nous avons utilisé ce gestionnaire d’événements pour avertir un développeur d’une erreur ; Nous pourrions l’étendre pour également consigner les détails de l’erreur dans une base de données. La présence de la `Application_Error` Gestionnaire d’événements n’affecte pas l’expérience de l’utilisateur final. Ils voient toujours la page d’erreurs configuré, qu’il s’agisse de la YSOD détails de l’erreur, le YSOD d’erreur de Runtime ou la page d’erreur personnalisée.

Il est normal de vous demander si le `Global.asax` fichier et `Application_Error` événement n’est nécessaire lors de l’utilisation d’une page d’erreur personnalisée. Lorsqu’une erreur se produit à l’utilisateur voit la page d’erreur personnalisée, pourquoi ne pouvons pas nous en mettre le code pour informer le développeur et de consigner des détails de l’erreur dans la classe code-behind de la page d’erreur personnalisée ? Bien que vous pouvez certainement ajouter du code à la classe de code-behind de la page d’erreur personnalisés vous n’avez pas d’accès aux détails de l’exception qui a déclenché le `Error` événement lors de l’utilisation de la technique que nous avons exploré dans le didacticiel précédent. Appel de la `GetLastError` méthode à partir de la page d’erreur personnalisée retourne `Nothing`.

La raison de ce comportement est, car la page d’erreur personnalisée est atteinte via une redirection. Lorsqu’une exception non gérée atteint le runtime ASP.NET, le moteur ASP.NET déclenche son `Error` événement (qui exécute le `Application_Error` Gestionnaire d’événements), puis *redirige* l’utilisateur à la page d’erreur personnalisée en émettant un `Response.Redirect(customErrorPageUrl)`. Le `Response.Redirect` méthode envoie une réponse au client avec un code d’état HTTP 302 demandant le navigateur pour demander une nouvelle URL, à savoir la page d’erreur personnalisée. Le navigateur demande ensuite automatiquement cette nouvelle page. Vous pouvez indiquer que la page d’erreur personnalisée a été demandée séparément à partir de la page d’origine de l’erreur car les changements de la barre d’adresses du navigateur à l’URL de page d’erreur personnalisée (consultez **Figure 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Figure 4**: lorsqu’une erreur se produit le navigateur est redirigé vers l’URL de Page d’erreur personnalisés  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image12.png))

L’effet net est que la demande où l’exception non gérée s’est produite se termine lorsque le serveur répond avec la redirection HTTP 302. La demande suivante à la page d’erreur personnalisée est une toute nouvelle demande ; à ce stade ASP.NET moteur a rejeté les informations d’erreur et, en outre, n’a aucun moyen pour associer l’exception non gérée dans la demande précédente à la nouvelle demande de la page d’erreur personnalisée. C’est pourquoi `GetLastError` retourne `null` lorsqu’elle est appelée à partir de la page d’erreur personnalisée.

Toutefois, il est possible que la page d’erreur personnalisée exécutée pendant la même requête qui a provoqué l’erreur. Le [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) méthode transfère l’exécution à l’URL spécifiée et les traite dans la même demande. Vous pouvez déplacer le code le `Application_Error` Gestionnaire d’événements à la classe de code-behind de la page d’erreur personnalisée, en remplaçant dans `Global.asax` par le code suivant :

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Quand une exception non gérée se produit désormais le `Application_Error` Gestionnaire d’événements transfère le contrôle à la page d’erreur personnalisée appropriée selon le code d’état HTTP. Étant donné que le contrôle a été transféré, la page d’erreur personnalisée a accès aux informations d’exception non gérée par le biais de `Server.GetLastError` peut notifier un développeur de l’erreur et consigner ses détails. Le `Server.Transfer` appel arrête le moteur ASP.NET à partir de la redirection de l’utilisateur vers la page d’erreur personnalisée. Au lieu de cela, le contenu de la page d’erreur personnalisée est retourné en réponse à la page qui a généré l’erreur.

## <a name="summary"></a>Récapitulatif

Lorsqu’une exception non gérée se produit dans une application web ASP.NET le runtime ASP.NET déclenche le `Error` événement et affiche la page d’erreurs configuré. Nous pouvons le développeur de l’erreur, consigner ses détails ou traiter dans une autre façon, en créant un gestionnaire d’événements pour l’événement d’erreur. Il existe deux façons de créer un gestionnaire d’événements `HttpApplication` événements, tels que `Error`: dans le `Global.asax` fichier ou à partir d’un HTTP Module. Ce didacticiel vous a montré comment créer un `Error` Gestionnaire d’événements dans le `Global.asax` fichier qui avertit les développeurs d’une erreur au moyen d’un message électronique.

Création d’un `Error` Gestionnaire d’événements est utile si vous avez besoin traiter les exceptions non gérées d’une manière unique ou personnalisée. Toutefois, créer votre propre `Error` Gestionnaire d’événements à consigner l’exception ou pour avertir un développeur n’est pas l’utilisation la plus efficace de votre temps, car il existe déjà des bibliothèques de journalisation erreur gratuit et facile à utiliser qui peuvent être configurées dans quelques minutes. Les deux didacticiels examiner deux de ces bibliothèques.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Vue d’ensemble des gestionnaires HTTP et de Modules HTTP ASP.NET](https://support.microsoft.com/kb/307985)
- [Répondre correctement aux Exceptions non gérées - traitement des Exceptions non gérées](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Classe et l’objet d’Application ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Les gestionnaires HTTP et des Modules HTTP dans ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Envoi d’E-mails dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Comprendre les `Global.asax` fichier](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [À l’aide des Modules HTTP et des gestionnaires pour créer des composants enfichables ASP.NET](https://msdn.microsoft.com/library/aa479332.aspx)
- [Utilisation de l’ASP.NET `Global.asax` fichier](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Utilisation de `HttpApplication` Instances](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Précédent](displaying-a-custom-error-page-vb.md)
> [Suivant](logging-error-details-with-asp-net-health-monitoring-vb.md)
