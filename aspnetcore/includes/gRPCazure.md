## <a name="grpc-not-supported-on-azure-app-service"></a>gRPC non pris en charge sur Azure App Service

> [!WARNING]
> [ASP.net Core gRPC](xref:grpc/index) n’est actuellement pas pris en charge sur Azure App service ou IIS. L’implémentation HTTP/2 de http. sys ne prend pas en charge les en-têtes de fin de réponse HTTP sur lesquels gRPC s’appuie. Pour plus d’informations, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/9020).
