## <a name="debug-diagnostics"></a>Diagnostics de débogage

Pour obtenir une sortie de diagnostic de routage détaillée, définissez `Logging:LogLevel:Microsoft` sur `Debug`. Par exemple, dans l’environnement de développement, définissez *appSettings. Development. JSON*:

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}