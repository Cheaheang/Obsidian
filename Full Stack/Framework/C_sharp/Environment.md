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