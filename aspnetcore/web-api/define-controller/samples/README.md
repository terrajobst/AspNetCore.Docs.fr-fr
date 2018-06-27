# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="1cf31-101">Exemple de contrôleur d’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1cf31-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="1cf31-102">Cet exemple d’application est composé des projets suivants :</span><span class="sxs-lookup"><span data-stu-id="1cf31-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="1cf31-103">**WebApiSample.Api** : projet ASP.NET Core 2.1 ciblant .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1cf31-103">**WebApiSample.Api**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="1cf31-104">**WebApiSample.Api.Pre21** : projet ASP.NET Core 2.0 ciblant .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1cf31-104">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="1cf31-105">**WebApiSample.DataAccess** : bibliothèque de classes .NET Standard 2.0 servant de couche d’accès aux données pour les deux projets d’API web.</span><span class="sxs-lookup"><span data-stu-id="1cf31-105">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="1cf31-106">Cet exemple illustre différentes façons de créer un contrôleur d’API web :</span><span class="sxs-lookup"><span data-stu-id="1cf31-106">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="1cf31-107">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="1cf31-107">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#derive-class-from-controllerbase)
- [<span data-ttu-id="1cf31-108">Annoter la classe avec ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="1cf31-108">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#annotate-class-with-apicontrollerattribute)