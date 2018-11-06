---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Envoi d’E-mail à partir d’un site ASP.NET Web Pages (Razor) Site | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment envoyer un message électronique automatisés à partir d’un site Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: f388ac1a97bde39cffe6b592b436d7af0d419a5f
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021389"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Envoi d’E-mails à partir d’un Site ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment envoyer un message électronique à partir d’un site Web lorsque vous utilisez les Pages Web ASP.NET (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment envoyer un message électronique à partir de votre site Web.
> - Guide pratique pour attacher un fichier à un message électronique.
> 
> Il s’agit de la fonctionnalité d’ASP.NET introduite dans l’article :
> 
> - Le `WebMail` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Envoi d’E-mails à partir de votre site Web

Il existe toutes sortes de raisons pour lesquelles vous devrez peut-être envoyer un e-mail à partir de votre site Web. Vous pouvez envoyer des messages de confirmation aux utilisateurs, ou vous pouvez envoyer des notifications à vous-même (par exemple, un nouvel utilisateur a enregistré.) Le `WebMail` helper facilite l’utilisation pour pouvoir envoyer un courrier électronique.

Pour utiliser le `WebMail` helper, vous devez avoir accès à un serveur SMTP. (Abréviation de *Simple Mail Transfer Protocol*.) Un serveur SMTP est un serveur de messagerie qui transfère uniquement les messages vers le serveur du destinataire &#8212; doivent impérativement du côté sortant de courrier électronique. Si vous utilisez un fournisseur d’hébergement pour votre site Web, ils probablement configurer vous avec l’adresse e-mail et il peuvent vous indiquer ce qui est le nom de votre serveur SMTP. Si vous travaillez à l’intérieur d’un réseau d’entreprise, un administrateur ou votre service informatique peut généralement vous donner les informations sur un serveur SMTP que vous pouvez utiliser. Si vous travaillez à domicile, vous pourrez même tester à l’aide de votre fournisseur de messagerie ordinaire, ce qui peut vous indiquer le nom du serveur SMTP. Vous devez généralement :

- Le nom du serveur SMTP.
- Le numéro de port. C’est presque toujours 25. Toutefois, votre ISP vous obligent à utiliser le port 587. Si vous utilisez SSL (SSL) pour le courrier électronique, vous devrez peut-être un port différent. Vérifiez auprès de votre fournisseur de messagerie.
- Informations d’identification (nom d’utilisateur, mot de passe).

Dans cette procédure, vous créez deux pages. La première page a un formulaire qui permet aux utilisateurs d’entrer une description, comme si elles ont été remplissant un formulaire de support technique. La première page envoie ses informations à une deuxième page. Dans la deuxième page, le code extrait des informations de l’utilisateur et envoie un message électronique. Il affiche également un message confirmant que le rapport de problème a été reçu.

![[image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Pour simplifier cet exemple, le code initialise le `WebMail` droite d’assistance dans la page où vous l’utilisez. Toutefois, pour les sites Web réels, il est plus judicieux de placer le code d’initialisation comme suit dans un fichier global, afin que vous initialisez le `WebMail` auxiliaire pour tous les fichiers dans votre site Web. Pour plus d’informations, consultez [personnalisation du comportement de l’échelle du Site pour ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Créer un nouveau site Web.
2. Ajoutez une nouvelle page nommée *EmailRequest.cshtml* et ajoutez le balisage suivant : 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Notez que le `action` attribut de l’élément de formulaire a été défini sur *ProcessRequest.cshtml*. Cela signifie que le formulaire est envoyé à cette page au lieu de nouveau à la page actuelle.
3. Ajoutez une nouvelle page nommée *ProcessRequest.cshtml* au site Web et ajoutez le code et le balisage suivant :   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Dans le code, vous obtenez les valeurs des champs de formulaire qui ont été soumises à la page. Vous appelez ensuite la `WebMail` l’Assistant `Send` méthode pour créer et envoyer le message électronique. Dans ce cas, les valeurs à utiliser sont constituées de texte qui vous concaténez avec les valeurs qui ont été soumises à partir du formulaire.

    Le code de cette page est à l’intérieur d’un `try/catch` bloc. Si un message électronique ne fonctionne pas pour une raison quelconque la tentative d’envoi (par exemple, les paramètres ne sont pas droit), le code dans le `catch` bloc s’exécute et définit le `errorMessage` variable à l’erreur qui s’est produite. (Pour plus d’informations sur `try/catch` blocs ou les `<text>` balise, consultez [Introduction à ASP.NET Web Pages programmation à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Dans le corps de la page, si le `errorMessage` variable est vide (la valeur par défaut), l’utilisateur voit un message e-mail a été envoyé. Si le `errorMessage` variable est définie sur true, l’utilisateur consulte un message qu’il a été un problème d’envoi du message.

    Notez que dans la partie de la page qui affiche un message d’erreur, il existe un test supplémentaire : `if(debuggingFlag)`. Il s’agit d’une variable que vous pouvez définir sur true si vous rencontrez des problèmes de l’envoi de courrier électronique. Lorsque `debuggingFlag` a la valeur true, et s’il existe un problème d’envoi de courrier électronique, un message d’erreur supplémentaires s’affiche qui indique ce que ASP.NET a signalé lorsqu’il a essayé d’envoyer le message électronique. Avertissement juste, toutefois : les messages d’erreur ASP.NET signale quand il ne peut pas envoyer un message électronique peuvent être génériques. Par exemple, si ASP.NET ne peut pas contacter le serveur SMTP (par exemple, étant donné que vous fait une erreur dans le nom du serveur), l’erreur est `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Important** si vous obtenez un message d’erreur d’un objet d’exception (`ex` dans le code), procédez comme *pas* transmet régulièrement ce message aux utilisateurs. Objets d’exception incluent souvent des informations que les utilisateurs ne doivent pas voir et qui peut même être une faille de sécurité. C’est pourquoi ce code inclut la variable `debuggingFlag` qui est utilisé comme un commutateur pour afficher le message d’erreur, et pourquoi la variable par défaut est définie sur false. Vous devez définir cette variable sur true (et par conséquent afficher le message d’erreur) *uniquement* si vous rencontrez un problème avec l’envoi de courrier électronique et que vous devez déboguer. Une fois que vous avez résolu les problèmes, définir `debuggingFlag` à false.

    Modifier les paramètres associés dans le code de messagerie de ce qui suit :

   - Définissez `your-SMTP-host` au nom du serveur SMTP que vous avez accès.
   - Définissez `your-user-name-here` au nom d’utilisateur pour votre compte de serveur SMTP.
   - Définissez `your-account-password` au mot de passe pour votre compte de serveur SMTP.
   - Définissez `your-email-address-here` à votre propre adresse e-mail. Il s’agit de l’adresse de messagerie, le message est envoyé à partir de. (Certains fournisseurs de messagerie ne vous permettent de spécifier un autre `From` d’adresses et utilisera votre nom d’utilisateur en tant que le `From` adresse.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Configuration des paramètres de messagerie
     > 
     > Il peut être un défi parfois pour vous assurer que vous avez les bons paramètres pour le serveur SMTP, le numéro de port et ainsi de suite. Voici quelques conseils :
     > 
     > - Le nom du serveur SMTP est souvent quelque chose comme `smtp.provider.com` ou `smtp.provider.net`. Toutefois, si vous publiez votre site sur un fournisseur d’hébergement, le nom du serveur SMTP à ce stade peut être `localhost`. Il s’agit, car une fois que vous avez publié et que votre site est en cours d’exécution sur le serveur du fournisseur, le serveur de messagerie peut être local du point de vue de votre application. Cette modification dans les noms de serveur peut signifier que vous devez modifier le nom du serveur SMTP dans le cadre de votre processus de publication.
     > - Le numéro de port est généralement 25. Toutefois, certains fournisseurs vous obligent à utiliser port 587 ou un autre port.
     > - Assurez-vous que vous utilisez les informations d’identification correctes. Si vous avez publié votre site vers un fournisseur d’hébergement, utilisez les informations d’identification que le fournisseur a spécifiquement sont pour le courrier électronique. Ceux-ci peuvent différer les informations d’identification que vous utilisez pour publier.
     > - Parfois, vous n’avez pas besoin d’informations d’identification du tout. Si vous envoyez un e-mail à l’aide de votre fournisseur de services Internet, votre fournisseur de messagerie déjà connaissez peut-être vos informations d’identification. Une fois que vous publiez, vous devrez peut-être utiliser différentes informations d’identification que lorsque vous testez sur votre ordinateur local.
     > - Si votre fournisseur de messagerie utilise le chiffrement, vous devez définir `WebMail.EnableSsl` à `true`.
4. Exécutez le *EmailRequest.cshtml* page dans un navigateur. (Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.)
5. Entrez votre nom et une description du problème, puis cliquez sur le **envoyer** bouton. Vous êtes redirigé vers la *ProcessRequest.cshtml* page, ce qui confirme votre message et qui vous envoie un message électronique. 

    ![[image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Envoi d’un fichier à l’aide de la messagerie

Vous pouvez également envoyer des fichiers qui sont joints aux messages électroniques. Dans cette procédure, vous créez un fichier texte et deux pages HTML. Vous allez utiliser le fichier texte en tant que pièce jointe d’e-mail.

1. Dans le site Web, ajoutez un nouveau fichier texte et nommez-le *MyFile.txt*.
2. Copiez le texte suivant et collez-le dans le fichier : 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Créez une page nommée *SendFile.cshtml* et ajoutez le balisage suivant : 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Créez une page nommée *ProcessFile.cshtml* et ajoutez le balisage suivant : 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modifier les paramètres associés dans le code de l’exemple de messagerie de ce qui suit :

    - Définissez `your-SMTP-host` au nom d’un serveur SMTP que vous avez accès.
    - Définissez `your-user-name-here` au nom d’utilisateur pour votre compte de serveur SMTP.
    - Définissez `your-email-address-here` à votre propre adresse e-mail. Il s’agit de l’adresse de messagerie, le message est envoyé à partir de.
    - Définissez `your-account-password` au mot de passe pour votre compte de serveur SMTP.
    - Définissez `target-email-address-here` à votre propre adresse e-mail. (Comme avant, serait normalement d’envoyer un e-mail à quelqu'un d’autre, mais pour le test, vous pouvez l’envoyer à vous-même.)
6. Exécutez le *SendFile.cshtml* page dans un navigateur.
7. Entrez votre nom, une ligne d’objet et le nom du fichier texte à joindre (*MyFile.txt*).
8. Cliquez sur le bouton `Submit`. Comme auparavant, vous êtes redirigé vers la *ProcessFile.cshtml* page, ce qui confirme votre message et qui vous envoie un message électronique avec le fichier joint.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


- [ASP.NET Web Pages (Razor) - Guide de résolution des problèmes](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personnalisation du comportement de l’échelle du Site pour ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
