# Dapper vs EFCore vs EFCore.BulkExtensions vs SqlBulkCopy
* https://github.com/TRIPTYK/ember-boilerplate/
* https://dev.to/milanjovanovictech/fast-sql-bulk-inserts-with-c-and-ef-core-4mb6
  * Performance is all that matters? **SqlBulkCopy** is your solution.
  * Need excellent speed and streamlined development? **EF Core** is a smart choice.
* https://dotnetfullstackdev.medium.com/bulk-insert-as-batch-sized-into-sql-database-from-net-web-application-2e8607cf9a61
  * Log and Alert
  * Manual Intervention
  * Automated Recovery
  * Rollback and Retry (Transaction)
  * Error Handling Improvements
* [Callbacks in C#](https://dotnetfullstackdev.medium.com/callbacks-in-c-made-simple-delegates-multicast-delegates-real-callback-flow-end-to-end-b1f227a5bb6e)
  * Background jobs notifying UI
  * File upload progress
  * Retry mechanisms
  * Notifications in libraries
  * Event-based designs
  * Framework code that calls your code (inversion of control)
* [AppSettings.json in .NET Core](https://dotnetfullstackdev.medium.com/appsettings-in-net-core-the-game-changer-for-configurations-a994d842e34c)
  * appsettings.json
  * appsettings.Development.json
  * appsettings.Testing.json
  * appsettings.Staging.json
  * appsettings.Production.json

## appSettings.json
```
{
  "AppSettings": {
    "FeatureToggle": false,
    "ApplicationName": "Modern Portal",
    "MaxUsers": 50
  }
}
```

## Models\AppSettings.cs
```
public class AppSettings
{
    public bool FeatureToggle { get; set; }
    public string ApplicationName { get; set; }
    public int MaxUsers { get; set; }
}
```

## Program.cs using WebApplicationBuilder
```
var builder = WebApplication.CreateBuilder(args);

var appSettings = builder.Configuration.GetSection("AppSettings");

Console.WriteLine($"FeatureToggle: {appSettings.FeatureToggle}");
Console.WriteLine($"Application Name: {appSettings.ApplicationName}");
Console.WriteLine($"Max Users: {appSettings.MaxUsers}");
```

## Program.cs using ConfigurationBuilder
```
var environment = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") ?? "Production";

var config = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json")
    .AddJsonFile($"appsettings.{environment}.json", optional: true)
    .Build();

builder.Services.Configure<AppSettings>(builder.Configuration.GetSection("AppSettings"));

IConfiguration config = builder.Build();  // ← It is MANDATORY to call Build first

var appSettings = config.GetSection("AppSettings").Get<AppSettings>();

Console.WriteLine($"FeatureToggle: {appSettings.FeatureToggle}");
Console.WriteLine($"Application Name: {appSettings.ApplicationName}");
Console.WriteLine($"Max Users: {appSettings.MaxUsers}");
```

## Controllers\HomeController.cs
```
public class HomeController : Controller
{
    private readonly AppSettings _appSettings;

    public HomeController(IOptions<AppSettings> appSettings)
    {
        _appSettings = appSettings.Value;
    }

    public IActionResult Index()
    {
        ViewData["AppName"] = _appSettings.ApplicationName;
        return View();
    }
}
```

| Method | Size | Speed |
| --- | --- | --- |
| EF_OneByOne | 100 | 19.800 ms |
| EF_OneByOne | 1000 | 259.870 ms |
| EF_OneByOne | 10000 | 8,860.790 ms |
| EF_OneByOne | 100000 | N/A |
| **EF_OneByOne** | 1000000 | **N/A** |
| Dapper_Insert | 100 | 10.650 ms |
| Dapper_Insert | 1000 | 113.137 ms |
| Dapper_Insert | 10000 | 1,027.979 ms |
| Dapper_Insert | 100000 | 10,916.628 ms |
| **Dapper_Insert** | 1000000 | **109,064.815 ms** |
| EF_AddAll | 100 | 2.064 ms |
| EF_AddAll | 1000 | 17.906 ms |
| EF_AddAll | 10000 | 202.975 ms |
| EF_AddAll | 100000 | 2,129.370 ms |
| **EF_AddAll** | 1000000 | **21,557.136 ms** |
| EF_AddRange | 100 | 2.035 ms |
| EF_AddRange | 1000 | 17.857 ms |
| EF_AddRange | 10000 | 204.029 ms |
| EF_AddRange | 100000 | 2,111.106 ms |
| **EF_AddRange** | 1000000 | **21,605.668 ms** |
| BulkExtensions | 100 | 1.922 ms |
| BulkExtensions | 1000 | 7.943 ms |
| BulkExtensions | 10000 | 76.406 ms |
| BulkExtensions | 100000 | 742.325 ms |
| **BulkExtensions** | 1000000 | **8,333.950 ms** |
| BulkCopy | 100 | 1.721 ms |
| BulkCopy | 1000 | 7.380 ms |
| BulkCopy | 10000 | 68.364 ms |
| BulkCopy | 100000 | 646.219 ms |
| **BulkCopy** | 1000000 | **7,339.298 ms** |
