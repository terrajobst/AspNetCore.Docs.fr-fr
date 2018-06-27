# <a name="custom-webhost-service-sample"></a>Exemple de service WebHost personnalisé

Cet exemple montre comment héberger une application ASP.NET Core en tant que service Windows sans utiliser IIS. Cet exemple illustre le scénario décrit dans [Héberger une application ASP.NET Core dans un service Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Instructions

L’exemple d’application est une application Razor Pages simple modifiée selon les instructions indiquées dans [Héberger une application ASP.NET Core dans un service Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Pour exécuter l’application dans un service, effectuez les étapes suivantes :

1. Créez un dossier à l’emplacement *c:\svc*.

1. Publiez l’application sur le dossier à l’aide de la commande `dotnet publish --configuration Release --output c:\\svc`. La commande déplace les ressources de l’application vers le dossier *svc*, notamment le fichier `appsettings.json` requis et le dossier `wwwroot`.

1. Ouvrez une invite de commandes **administrateur**.

1. Exécutez les commandes suivantes :

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *L’espace entre le signe égal et le début de la chaîne de chemin est requis.*

1. Dans un navigateur, accédez à `http://localhost:5000` et vérifiez que le service est en cours d’exécution. L’application redirige vers le point de terminaison sécurisé `https://localhost:5001`.

1. Pour arrêter le service, utilisez la commande suivante :

   ```console
   sc stop MyService
   ```

Si l’application ne démarre pas comme prévu, une façon rapide de rendre les messages d’erreur accessibles consiste à ajouter un fournisseur de journalisation, tel que le [fournisseur EventLog Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Une autre option consiste à vérifier le Journal des événements de l’application à l’aide de l’observateur d’événements sur le système. Par exemple, voici une exception non prise en charge pour une erreur FileNotFound dans le Journal des événements de l’application :

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
