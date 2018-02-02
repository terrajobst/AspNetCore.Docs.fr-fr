# <a name="custom-webhost-service-sample"></a><span data-ttu-id="715b1-101">Exemple de Service WebHost personnalisé</span><span class="sxs-lookup"><span data-stu-id="715b1-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="715b1-102">Cet exemple montre la méthode recommandée pour héberger une application ASP.NET Core sur Windows sans utiliser IIS comme un Service Windows.</span><span class="sxs-lookup"><span data-stu-id="715b1-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="715b1-103">Cet exemple illustre les fonctionnalités décrites dans [héberger une application ASP.NET Core dans un Service Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="715b1-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="715b1-104">Instructions</span><span class="sxs-lookup"><span data-stu-id="715b1-104">Instructions</span></span>

<span data-ttu-id="715b1-105">L’exemple d’application est une application web MVC simple modifiée en suivant les instructions de [héberger une application ASP.NET Core dans un Service Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="715b1-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="715b1-106">Pour exécuter l’application dans un service, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="715b1-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="715b1-107">Créer un dossier au *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="715b1-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="715b1-108">Publier l’application dans le dossier avec `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="715b1-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="715b1-109">La commande déplacera les ressources de l’application dans le dossier, y compris les `appsettings.json` fichier et le `wwwroot` dossier avec son contenu.</span><span class="sxs-lookup"><span data-stu-id="715b1-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="715b1-110">Ouvrir un **administrateur** interface de commande.</span><span class="sxs-lookup"><span data-stu-id="715b1-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="715b1-111">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="715b1-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="715b1-112">Dans un navigateur, accédez à `http://localhost:5000` pour vérifier que le service est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="715b1-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="715b1-113">Pour arrêter le service, utilisez la commande :</span><span class="sxs-lookup"><span data-stu-id="715b1-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="715b1-114">Si l’application ne démarre pas comme prévu lors de l’exécution dans un service, un moyen rapide pour rendre les messages d’erreur accessible est pour ajouter un fournisseur de journalisation, tels que les [fournisseur du journal des événements Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="715b1-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="715b1-115">Une autre option consiste à vérifier le journal des événements à l’aide de l’Observateur d’événements sur le système.</span><span class="sxs-lookup"><span data-stu-id="715b1-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="715b1-116">Par exemple, voici une exception non prise en charge pour une erreur de fichier introuvable dans le journal des événements Application :</span><span class="sxs-lookup"><span data-stu-id="715b1-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
