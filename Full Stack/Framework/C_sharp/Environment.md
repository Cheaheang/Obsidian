**Environment** represent a system in which the application is deployed and executed
- Developer : The environment where the developer makes changes in the code. commits code to the source control.
- Staging : The environment, where the application runs on a server, from which other developers and quality controllers access the application.
- Production: The environment, where the real user access the application
In Program.cs
```cs
if(app.Environment.IsDevelopment() || app.Environment.IsStaging()
    || app.Environment.IsEnvironment("Beta"))
{
    app.UseDeveloperExceptionPage();
}

```
This method will hide all the error exception for developer, when it not Developer Environment


### Access Environment

HomeController.cs
```cs
 public class HomeController : Controller
 {
     private readonly IWebHostEnvironment _webHostEnvironment;
     public HomeController(IWebHostEnvironment webHostEnvironment) {
         _webHostEnvironment = webHostEnvironment;
         }
     [Route("/")]
     public IActionResult Index()
     {
         ViewBag.CurrentEnvironment =
         _webHostEnvironment.EnvironmentName;
         return View();
     }
 }
```
Views/Home/index.cshtml
```cs
    <div>
        Environment: @ViewBag.CurrentEnvironment
    </div>
```
### Environment Tag Helper
ViewImports.cshtml
```cs
    @addTagHelper "*, Microsoft.AspNetCore.Mvc.TagHelpers"
```
Views/Home/index.cshtml
```cs
    <environment include="Beta" >
    <button>Button for only development</button>
    </environment>

```
### Process-Level Environment
 In the server there is no visual studio, so we need to config it via powershell
- Open Powershell
- cd to_your_project_dir(solution Folder)
- cd to_your_project_dir(actual project)
- type "dotnet run"
If you don't want to run with launchStting
- type "dotnet run --no-launch-profile"
	- $Env:ASPNETCORE_ENVIRONMENT="Staging"

**Fun fact:** when you delete launchSetting.json. It will choose production enviroment


#### Configuration as Environment Variable

in **Window Powershell**
```cs
$Env:ParentKey__ChildKey = "value"
// ex: $Env:weatherapi__ClientID="Classic"
	dotnet run --no-launch-profile
```

- It is one of the most secured way of setting-up sensitive values in configuration.
- __(underscore and underscore) is the separator between parent key and child key

#### Custom Json configuration 
```cs
builder.Host.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile("MyOwnConfig.json", optional: true, reloadOnChange: true);
});
```
**Note:**
- optional : true = mean that that json is optional
- reloadOnChange : true= when you make change in that file, this function will restart

#### HTTP Client
**HttpClient** is a class for sending HTTP requests to a specific HTTP resource (using it url) and receiving HTTP response from the same 


Program.cs
```cs
using StocksApp.Services;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllersWithViews();
builder.Services.AddHttpClient();
builder.Services.AddScoped<MyService>();
var app = builder.Build();


app.UseStaticFiles();
app.UseRouting();
app.MapControllers();

app.Run();

```
Controllers/HomeController.cs
```cs
  public class HomeController : Controller
  {
      private readonly MyService _myService;

      public HomeController(MyService myService)
      {
          _myService = myService;
      }
      [Route("/")]
      public async Task<IActionResult> Index()
      {
          await _myService.method();
          return View();
      }
  }
```
Service/MyService.cs
```cs
public class MyService
{
    private readonly IHttpClientFactory _httpClientFactory;

    public MyService(IHttpClientFactory httpClientFactory)
    {
        _httpClientFactory = httpClientFactory;
    }

    public async Task method()
    {
        using(HttpClient httpClient = _httpClientFactory.CreateClient())
        {
            HttpRequestMessage httpRequestMessage = new HttpRequestMessage()
            {
                RequestUri = new Uri("https://finnhub.io/api/v1/quote?symbol=AAPL&token=d5ohmvpr01qjast6q8s0d5ohmvpr01qjast6q8sg"),
                Method = HttpMethod.Get
            };
            HttpResponseMessage httpResponseMessage = await httpClient.SendAsync(httpRequestMessage);
            // Content represent the body of the response
            Stream stream = httpResponseMessage.Content.ReadAsStream();

            StreamReader streamReader = new StreamReader(stream);
            string response = streamReader.ReadToEnd();
        }
    }
}
```
Views/ViewStart.cshtml
```cs
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}
```
View/Index.cshtml
```cs
@{
    ViewBag.Title = "Home Page";
}
<h1>Home</h1>
```
Views/Shared/Layout
```cs
<!DOCTYPE html>

<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>@ViewBag.Title</title>
    <link href="#" rel="stylesheet" />  
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/7.0.1/css/all.min.css" 
    integrity="sha512-2SwdPD6INVrV/lHTZbO2nodKhrnDdJK9/kg2XD1r9uGqPo1cUbujc+IYdlYdEErWNu69gVcYgdxlmVmzTWnetw==" 
    crossorigin="anonymous" referrerpolicy="no-referrer" />
</head>
<body>
    <div>
        <div class="page-content ">
            <div class="margin-bottom">
                <div class="flex" id="top-bar-div">
                    <div class="flex-1">
                        <h1 class="app-title">
                            <i class="fa-solid fa-chart-line"></i>
                            Stocks App
                            
                        </h1>
                    </div>
                    <div class="flex-1">
                        Login
                    </div>
                </div>
            </div>
                @RenderBody()
        </div>
    </div>
</body>
</html>
```