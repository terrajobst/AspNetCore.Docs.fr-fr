---
title: Remplacez la méthode machineKey ASP.NET dans ASP.NET Core
author: rick-anderson
description: Découvrez comment remplacer machineKey dans ASP.NET pour autoriser l’utilisation d’un système de protection de données nouvelle et plus sûre.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: ff36382d22a218a228b42a31ae4f8ad2eb2d5b5f
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068282"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>Remplacez la méthode machineKey ASP.NET dans ASP.NET Core

<a name="compatibility-replacing-machinekey"></a>

L’implémentation de la `<machineKey>` élément dans ASP.NET [est remplaçable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Ainsi, la plupart des appels aux routines de chiffrement ASP.NET puissent être acheminés via un mécanisme de protection des données de remplacement, y compris le nouveau système de protection des données.

## <a name="package-installation"></a>Installation de package

> [!NOTE]
> Le nouveau système de protection des données peut uniquement être installé dans une application ASP.NET existante ciblant .NET 4.5.1 ou version ultérieure. Installation échouera si l’application cible .NET 4.5 ou diminuer.

Pour installer le nouveau système de protection des données dans un projet de 4.5.1+ ASP.NET existant, installez le package Microsoft.AspNetCore.DataProtection.SystemWeb. Il instancie le système de protection de données à l’aide du [configuration par défaut](xref:security/data-protection/configuration/default-settings) paramètres.

Lorsque vous installez le package, il insère une ligne dans *Web.config* qui indique à ASP.NET pour l’utiliser pour [plus les opérations de chiffrement](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), y compris l’authentification par formulaire, l’état d’affichage et les appels à MachineKey.Protect. La ligne est insérée se présente comme suit.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Vous pouvez indiquer si le nouveau système de protection de données est actif en examinant les champs comme `__VIEWSTATE`, qui doit commencer par « CfDJ8 » comme dans l’exemple ci-dessous. « CfDJ8 » est la représentation en base64 de l’en-tête de magie « 09 F0 C9 F0 » qui identifie une charge utile protégée par le système de protection des données.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>configuration de package

Le système de protection des données est instancié avec une configuration par défaut de zéro. Toutefois, étant donné que par défaut, les clés sont conservés dans le système de fichiers local, cela ne fonctionne pas pour les applications qui sont déployées dans une batterie de serveurs. Pour résoudre ce problème, vous pouvez fournir de configuration en créant un type qui sous-classe DataProtectionStartup et remplace sa méthode ConfigureServices.

Voici un exemple d’un type de démarrage de protection des données personnalisées qui configuré à la fois où les clés sont conservées et comment elles sont chiffrées au repos. Elle remplace également la stratégie d’isolation d’application par défaut en fournissant son propre nom de l’application.

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> Vous pouvez également utiliser `<machineKey applicationName="my-app" ... />` à la place d’un appel explicite à SetApplicationName. Il s’agit d’un mécanisme pratique de ne pas forcer au développeur de créer un type dérivé de DataProtectionStartup si toutes les il souhaite configurer a été définissant le nom de l’application.

Pour activer cette configuration personnalisée, revenez au fichier Web.config et recherchez le `<appSettings>` élément qui installer le package ajouté au fichier de configuration. Il doit ressembler le balisage suivant :

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

Renseignez la valeur vide avec le nom qualifié d’assembly du type dérivé DataProtectionStartup que vous venez de créer. Si le nom de l’application est DataProtectionDemo, cela ressemblerait le ci-dessous.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Le système de protection des données qui vient d’être configuré est maintenant prêt à être utilisé à l’intérieur de l’application.
