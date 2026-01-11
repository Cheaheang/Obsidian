**Configuration** are the constant key/value pair that are set at the common location and can be use from anywhere

**Configuration Source**
- appsettings.json
- Environment Variable
- File Configuration (JSON, INI OR XML files)
- In-Memory Configuration
- Secret Manger

When you want to access data from appsettings.json
```cs
app.UseEndpoints(endpoints =>
{
    endpoints.Map("/", async context =>
    {
        await context.Response.WriteAsync(app.Configuration["myKey"]);
    });
});
```
#### IConfiguration in Controller

HomeController.cs
```cs
    public class HomeController : Controller
    {
        private readonly IConfiguration _configuration;

        // Constructor to inject IConfiguration
        public HomeController(IConfiguration configuration)
        {
            _configuration = configuration;
        }
             
        [Route("/")]
        public IActionResult Index()
        {
            ViewBag.MyKey = _configuration["MyKey"];
            ViewBag.MyAPIKey = _configuration.GetValue("MyAPIKey", "Missing API Key");
            return View();
        }
    }
```
Views/Home/index.cshtml
```cs
            <div class="box">
                <h3>@ViewBag.MyKey</h3>
                <h3>@ViewBag.MyAPIKey</h3>
            </div>

```

### Hierarchical Configuration
f 
```cs
  [Route("/")]
  public IActionResult Index()
  {
// ================== Method 1 ===================
ViewBag.name = _configuration.GetValue("data:name", "data");
// ================== Method 2 ===================
ViewBag.name = _configuration.GetSection("data")["name"];
// ================== Method 3 ===================
IConfigurationSection data = _configuration.GetSection("data");
ViewBag.name = data["name"];
return View();
  }
```

#### Option Pattern
**Option Pattern** uses custom classes to specify what configuration settings are to be loaded into properties
```cs
  [Route("/")]
  public IActionResult Index()
{
// ================== Method 4 (Option Pattern) ===================
            WeatherApiOptions option = _configuration.GetSection("weatherapi").Get<WeatherApiOptions>();

            ViewBag.name = option.ClientId;
            ViewBag.secret = option.ClientSecret;
	return View();
}
```
WeatherApiOptions.cs
```cs
   public class WeatherApiOptions
   {
       public string? ClientId { get; set; }
       public string? ClientSecret { get; set; }
   }
```

#### Configuration as Service

Setup 
program.cs

```cs
// Supply an object of WeatherApiOptions  (with 'weatherapi' section) as a service 

builder.Services.Configure<WeatherApiOptions>(builder.Configuration.GetSection("weatherapi"));

```
HomeController.cs
```cs
  public class HomeController : Controller
  {
      private readonly WeatherApiOptions _weatherconfig;

      // Constructor to inject IConfiguration
      public HomeController(IOptions<WeatherApiOptions> weatherApiOption)
      {
          _weatherconfig = weatherApiOption.Value;
      }
           
      [Route("/")]
      public IActionResult Index()
      {
            ViewBag.name = _weatherconfig.ClientId;
            ViewBag.secret = _weatherconfig.ClientSecret;
          return View();
      }
  }
```

#### Environment Specific Configuration
![[Pasted image 20260111142915.png]]

In your project **appsettings.json** have (**appsettings.Development.json**, **appsettings.Staging.json**, **appsettings.Production.json**), IF it doesn't have it will the data from **appsettings.json**

**Secrets Manager** : stores the user secrets (sensitive configuration data) in a separate location on the developer machine,

CMD to setup
```cs
dotnet user-secrets init
dotnet user-secrets set "key" "Value"
dotnet user-secrets list
```
**Fun fact:** Secret.json -> appsetting.Development.cs -> appsetting.cs

#### Environment Variable Configuration