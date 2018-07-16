# <a name="custom-webhost-service-sample"></a><span data-ttu-id="0e4e1-101">Exemple de service WebHost personnalisé</span><span class="sxs-lookup"><span data-stu-id="0e4e1-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="0e4e1-102">Cet exemple montre comment héberger une application ASP.NET Core en tant que service Windows sans utiliser IIS.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="0e4e1-103">Cet exemple illustre le scénario décrit dans [Héberger une application ASP.NET Core dans un service Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="0e4e1-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="0e4e1-104">Instructions</span><span class="sxs-lookup"><span data-stu-id="0e4e1-104">Instructions</span></span>

<span data-ttu-id="0e4e1-105">L’exemple d’application est une application Razor Pages simple modifiée selon les instructions indiquées dans [Héberger une application ASP.NET Core dans un service Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="0e4e1-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="0e4e1-106">Pour exécuter l’application dans un service, effectuez les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0e4e1-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="0e4e1-107">Créez un dossier à l’emplacement *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="0e4e1-108">Publiez l’application sur le dossier à l’aide de la commande `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="0e4e1-109">La commande déplace les ressources de l’application vers le dossier *svc*, notamment le fichier `appsettings.json` requis et le dossier `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="0e4e1-110">Ouvrez une invite de commandes **administrateur**.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="0e4e1-111">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0e4e1-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="0e4e1-112">*L’espace entre le signe égal et le début de la chaîne de chemin est requis.*</span><span class="sxs-lookup"><span data-stu-id="0e4e1-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="0e4e1-113">Dans un navigateur, accédez à `http://localhost:5000` et vérifiez que le service est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="0e4e1-114">L’application redirige vers le point de terminaison sécurisé `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="0e4e1-115">Pour arrêter le service, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="0e4e1-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="0e4e1-116">Si l’application ne démarre pas comme prévu, une façon rapide de rendre les messages d’erreur accessibles consiste à ajouter un fournisseur de journalisation, tel que le [fournisseur EventLog Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="0e4e1-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="0e4e1-117">Une autre option consiste à vérifier le Journal des événements de l’application à l’aide de l’observateur d’événements sur le système.</span><span class="sxs-lookup"><span data-stu-id="0e4e1-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="0e4e1-118">Par exemple, voici une exception non prise en charge pour une erreur FileNotFound dans le Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="0e4e1-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
