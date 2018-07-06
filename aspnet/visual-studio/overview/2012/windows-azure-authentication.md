---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure Authentication | Microsoft Docs
author: Rick-Anderson
description: Outils Microsoft ASP.NET pour Windows Azure Active Directory permet de facilement activer l’authentification pour les applications web hébergées sur les Sites Web Windows Azure...
ms.author: aspnetcontent
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: e3443fb627dc8d7d5011341828556b4c13836170
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812808"
---
<a name="windows-azure-authentication"></a>Windows Azure Authentication
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Outils de Microsoft ASP.NET pour Windows Azure Active Directory fait simple activer l’authentification pour les applications web hébergées sur [Sites Web Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Vous pouvez utiliser l’authentification Windows Azure pour authentifier les utilisateurs d’Office 365 à partir de votre organisation, les comptes d’entreprise synchronisés à partir de votre annuaire local Active Directory ou les utilisateurs créés dans votre propre domaine personnalisé de Windows Azure Active Directory. Activation de l’authentification de Windows Azure configure votre application pour authentifier les utilisateurs à l’aide d’un seul [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) locataire.
> 
> L’outil ASP.NET Windows Azure Authentication n’est pas pris en charge pour les rôles web dans un service cloud, mais nous prévoyons de le faire dans une version ultérieure. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) est pris en charge dans les rôles web Windows Azure.
> 
> Pour plus d’informations sur la configuration de la synchronisation entre votre annuaire local Active Directory et votre locataire Windows Azure Active Directory, consultez [utiliser AD FS 2.0 pour implémenter et gérer l’authentification unique](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory est actuellement disponible en tant qu’un [libre service en version préliminaire](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Configuration requise :

- Visual Studio 2012 ou [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Extensions pour Visual Studio 2012 d’outils Web](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) ou [Web Tools Extensions pour Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Outils de Microsoft ASP.NET pour Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) ou [des outils Microsoft ASP.NET pour Windows Azure Active Directory – Visual Studio Express 2012 pour le Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Créer une Application Web ASP.NET avec Visual Studio 2012

Vous pouvez créer n’importe quelle Application Web avec Visual Studio 2012, ce didacticiel utilise le modèle d’intranet ASP.NET MVC.

1. Créer une nouvelle Application Intranet ASP.NET MVC 4 et acceptez toutes les valeurs par défaut. (Il doit être un In **tra** net et non dans **RET** projet net).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Activer l’authentification de Windows Azure (lorsque vous êtes un administrateur du principe général)

Si vous n’avez pas d’un locataire Windows Azure Active Directory existant (par exemple, via un compte Office 365 existant), vous pouvez créer un nouveau locataire en vous connectant à un [nouveau compte Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. Dans le menu projet, sélectionnez **activer l’authentification Windows Azure**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Entrez le domaine de votre locataire Windows Azure Active Directory (par exemple, contoso.onmicrosoft.com) et cliquez sur **activer**:

![](windows-azure-authentication/_static/image3.png)

3. Dans l’authentification Web boîte de dialogue de connexion en tant qu’administrateur pour votre locataire Windows Azure Active Directory :  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Activer Windows Azure à un compte non administrateur du principe

Si vous n’avez pas de privilèges d’administrateur Global pour votre locataire Windows Azure Active Directory, vous pouvez décocher la case à cocher pour l’approvisionnement de l’application.

![](windows-azure-authentication/_static/image6.png)

La boîte de dialogue affichera le **domaine**, **Id de Principal de l’Application** et **URL de réponse** qui sont requis pour l’approvisionnement de l’application avec Azure Active Directory principe. Vous devez donner à ces informations à une personne qui a des privilèges suffisants pour configurer l’application. Consultez[comment implémenter l’authentification unique avec Windows Azure Active Directory - Application ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) pour plus d’informations sur la façon d’utiliser l’applet de commande pour créer le principal du service manuellement.  
Une fois que l’application a été configurée correctement, vous pouvez cliquer sur **continuer à mettre à jour web.config avec les paramètres sélectionnés**. Si vous souhaitez continuer à développer l’application lors de l’attente pour l’approvisionnement se produise, vous pouvez cliquer sur **fermer pour mémoriser les paramètres dans le fichier projet**. La prochaine fois vous appeler activer l’authentification Windows Azure et décochez la case à cocher approvisionnement, vous verrez les mêmes paramètres et vous pouvez cliquer sur **continuer**, puis cliquez sur, **appliquer ces paramètres dans le fichier web.config**.

1. Patienter pendant que votre application est configurée pour l’authentification Windows Azure et configurée avec Windows Azure Active Directory.
2. Une fois l’authentification Windows Azure a été activée pour votre application, cliquez sur **fermer :** 

    ![](windows-azure-authentication/_static/image7.png)
3. Appuyez sur F5 pour exécuter votre application. Il est possible que vous devez obtenir automatiquement redirigé vers la page de connexion. Utiliser les informations d’identification utilisateur de répertoire principe pour se connecter à l’application...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Étant donné que votre application utilise actuellement un certificat auto-signé de test vous recevrez un avertissement à partir du navigateur le certificat n’a pas été émis par une autorité de certification approuvée.

    Cet avertissement peut être ignoré en toute sécurité pendant le développement local en cliquant sur **poursuivre sur ce site Web :** 

    ![](windows-azure-authentication/_static/image8.png)
5. Vous avez maintenant correctement connecté à votre application à l’aide de l’authentification Windows Azure !

    ![](windows-azure-authentication/_static/image2.jpg)

L’activation de Windows Azure authentication apporte les modifications suivantes à votre application :

- Une falsification de requête Anti-Cross-Site ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) classe ( *application\_Start\AntiXsrfConfig.cs* ) est ajouté à votre projet.
- Les packages NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` est ajouté à votre projet.
- Paramètres de Windows Identity Foundation dans votre application seront configurés pour accepter les jetons de sécurité à partir de votre locataire Windows Azure Active Directory. Cliquez sur l’image ci-dessous pour afficher une vue développée des modifications apportées à la *Web.config* fichier.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Un principal de service pour votre application dans votre locataire Windows Azure Active Directory est approvisionné.
- HTTPS est activé.

## <a name="deploy-the-application-to-windows-azure"></a>Déployer l’application sur Windows Azure

Pour obtenir des instructions complètes, consultez [déploiement d’une Application Web de ASP.NET sur un Site Web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Pour publier une application à l’aide de l’authentification sur un Site Web Azure de Windows Azure :

1. Cliquez avec le bouton droit sur votre application et sélectionnez **publier :** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. À partir de la boîte de dialogue Publier le site Web télécharger et importer un profil de publication pour votre Site Web Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. Le **connexion** onglet montre le **URL de Destination** (URL publique pour votre application accessible sur). Cliquez sur **valider la connexion** pour tester votre connexion :

    ![](windows-azure-authentication/_static/image5.jpg)
4. Si vous avez publié sur ce Site Web Azure précédemment envisagez de vérifier le **supprimer les fichiers supplémentaires à la destination** paramètre pour que votre application publie correctement. Notez que le **activer l’authentification Windows Azure** case à cocher est sélectionné.  

    ![](windows-azure-authentication/_static/image10.png)
5. Facultatif : Dans le **aperçu** onglet, cliquez sur **démarrer l’aperçu** pour afficher les fichiers déployés.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Cliquez sur **publier.**

    Vous êtes invité à activer l’authentification Windows Azure pour l’hôte cible. Cliquez sur **activer** pour continuer :

    ![](windows-azure-authentication/_static/image11.png)
7. Entrez vos informations d’identification d’administrateur pour votre locataire Windows Azure Active Directory :

    ![](windows-azure-authentication/_static/image7.jpg)
8. Une fois que votre application a été publiée, un navigateur s’ouvre sur le site web publié.

    > [!NOTE]
    > Il peut prendre jusqu'à cinq minutes (généralement doit moins) pour votre application soit entièrement configurée avec Windows Azure Active Directory après l’activation de l’authentification Windows Azure pour l’hôte cible. Lorsque vous exécutez tout d’abord votre application si vous recevez l’erreur ACS50001 : partie de confiance avec le nom « [realm] » n’a pas été trouvé, patientez quelques minutes, puis essayez à nouveau l’application en cours d’exécution.
9. Lorsque vous y êtes invité, connectez-vous en tant qu’utilisateur dans votre répertoire :

    ![](windows-azure-authentication/_static/image8.jpg)
10. Vous avez maintenant correctement connecté à votre Azure hébergé l’application à l’aide de l’authentification Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problèmes connus

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Autorisation basée sur un rôle échoue lors de l’utilisation de Windows Azure Authentication < o : p >< / o : p >

L’authentification Windows Azure ne fournit pas actuellement la revendication de rôle nécessaires afin que l’autorisation basée sur le rôle peut être effectuée. Le rôle de l’utilisateur authentifié doit être récupéré manuellement à partir de Windows Azure Active Directory. < o : p >< / o : p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Accédant à une application avec des résultats de l’authentification Windows Azure dans l’erreur « ACS20016 le domaine de l’utilisateur connecté (live.com) ne correspond pas à toute autorisé domaine de ce STS » < o : p >< / o : p >

Si vous êtes déjà connecté à un Account Microsoft (par exemple hotmail.com, live.com, outlook.com) et que vous tentez d’accéder à une application qui a activé l’authentification Windows Azure vous pouvez obtenir une réponse de 400 erreur, car le domaine de votre Account Microsoft n’est pas reconnu par Windows Azure Active Directory. Pour vous connecter à l’application, se déconnecter de votre Account Microsoft tout d’abord. < o : p >< / o : p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Journalisation dans une application avec authentification Windows Azure est activée et un X509CertificateValidationMode autre que None entraîne des erreurs de validation de certificat pour le certificat accounts.accesscontrol.windows.net < o : p >< / o : p >

Validation du certificat n’est pas obligatoire et doit rester désactivée. L’empreinte numérique du certificat de l’émetteur est validé par le WSFederationAuthenticationModule. < o : p >< / o : p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Lorsque vous tentez d’activer l’authentification de Windows Azure la boîte de dialogue d’authentification Web affiche l’erreur « ACS20016 : le domaine de l’utilisateur connecté (contoso.onmicrosoft.com) ne correspond pas à aucun domaine autorisé de ce STS. » < o : p >< / o : p >

Vous pouvez voir cette erreur lorsque vous avez précédemment connecté avec succès à l’aide d’un compte Windows Azure Active Directory différent de dans le même processus Visual Studio. Déconnectez-vous du compte spécifié, ou redémarrez Visual Studio. Si connecté précédemment et sélectionné l’option « Maintenir la connexion », vous devrez peut-être effacer les cookies du navigateur. < o : p >< / o : p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012 : La demande n’est pas un message de protocole WS-Federation valid < o : p >< / o : p >

Cela peut se produire si vous êtes déjà connecté avec certains autres ID Microsoft à un des services Azure. Fenêtre de navigateur d’utilisation privée telles que InPrivate dans Internet Explorer ou Incognito dans Chrome ou effacer tous les cookies. < o : p >< / o : p >

## <a name="additional-resources"></a>Ressources supplémentaires

- [Outils de Microsoft ASP.NET pour Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure fonctionnalités : identité](https://docs.microsoft.com/azure/active-directory/)
- [TechNet : Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory : développer des applications pour votre organisation](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory : développer des applications pour plusieurs organisations](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Comment implémenter l’authentification unique avec Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Single Sign-On avec Windows Azure Active Directory : présentation approfondie de](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Utiliser AD FS 2.0 pour implémenter et gérer l’authentification unique](https://technet.microsoft.com/library/jj205462.aspx)
