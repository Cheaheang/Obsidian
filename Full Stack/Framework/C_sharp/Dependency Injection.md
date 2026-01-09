

![[Pasted image 20260107194226.png]]

When user click on submit. It will trigger the POST request. Then It will trigger Controller, Controller will trigger the service. Then the service calculate the service and store data to in the data layer. 

**Service** is a class that contains business logic  such as business calculations, business validations that are specific to the domain of the client's business.


### Setup Project
- Create project
- Right click on Solution and click add. Then select New Project. Then Select Class Library
- Create Controller folder and controller file
- Create wwwroot folder
- Add StyleSheet.css to the wwwroot folder
- Create Views folder, then right click on the folder and click add, and select Razor view-empty(It will create Home Folder and index.cshtml file)
- Select Views folder, then right click on the folder and click add, and select Razor Layout and click ok
- Select Views folder, then right click on the folder and click add, and select Razor View Start and click ok
- Select Views folder, then right click on the folder and click add, and select Razor View Import and click ok


**To add your Service to your project**
- Method 1
Select project folder and right click on it, Select add and find **Project Reference....**
- Method 2 
Select dependencies in your project and select **Add Project Reference**


#### PROCESS
Program.cs -> HomeController.cs -> Constructor of Home Controller -> CitiesService.cs -> invoke the Constructor of Cities Service(Create CitiesService Object) -> Execute function with route path -> Home/index.cshtml(the function return view) -> _Layout.cshtml


### Dependency Inversion Principle (DIY)
**Dependency Problem**
Higher-level modules(Controller) depend on lower-level modules(Service). Means, both are tightly-coupled.
- Any changes made in the lower-level module effects changes in the higher-level module. 
- The developer of higher-level module SHOULD WAIT until the completion of development of lower-level module
- Requires much code change in to interchange an alternative lower-level module.
- Difficult to test a single module without effecting / testing other module 

Solution
**DIY** is design principle(guideline), which is a solution for the dependency problem
- The higher-level module (client) should not depend on the lower module. Both should depend on abstractions(interface or abstract class)
- Abstraction should not depend on details (both client or dependency). Details ( both client and dependency ) should depend on abstractions.

The one create higher-module(Controller) is the one, who create the interface

**How to Connect interface with Controller and Service**
- In CitiesService (Don't forget to reference from ServiceContracts)
```cs
    public class CitiesService: ICitiesService
    {
		//code
	} 
```
- In Controller
```cs
    public class HomeController : Controller
    {
        private readonly ICitiesService _citiesService;

		//code
	}
```


![[Pasted image 20260108114255.png]]

### Inversion of Control(IoC)
**Inversion of Controll(IoC)** is a design pattern (reusable solution for a common problem), which suggests "IoC container" for implementation of Dependency Inversion Principle (DIP).
- "Don't call us, we will call you" pattern
- It Inverse the control by shifting the control to IoC container.
- There are a lot of method for IoC: Service Locator, Events or even, Dependency Injection(Most popular)

In program.cs
```cs
// Add service to IoC container
// when ever some class looking for ICitiesService it will give CitiesService
builder.Services.Add(new ServiceDescriptor(
    typeof(ICitiesService), 
    typeof(CitiesService), 
    ServiceLifetime.Transient
    ));
```

### Dependency Injection(DI)
**DI** is a design pattern, Which is a technique for achieving "Inversion of Control (IoC)" between clients and their dependencies.
- It allow you to inject (supply) a concrete implementation object of a low-level component into a high-level component( It's a technique to receive the object of the Cities Service into the homeController  ) .
- The client class(controller class) receives the dependency object as a parameter either in the constructor or in a method

HomeController.cs
```cs
public class HomeController : Controller
{
    private readonly ICitiesService _citiesService;

    //constructor 
    public HomeController(ICitiesService citiesService)
    {
        // Create object of CitiesService class
        _citiesService = citiesService ;
    }
    //code
}
```
**Method Injection**
- **Constructor**(Constructor Inject)
```cs
    public class HomeController : Controller
    {
         Dependency Injection via Constructor 
        private readonly ICitiesService _citiesService;

        //constructor 
        public HomeController(ICitiesService citiesService)
        {
            // Create object of CitiesService class
            _citiesService = citiesService ;
        }


        [Route("/")]
        public IActionResult Index()
        {
           List<string> cities = _citiesService.GetCities();
            return View(cities);
        }
    }
```
- **FromService** (Inject Service to method/Function)
```cs
    public class HomeController : Controller
    {
        [Route("/")]
        //Dependency Injection via FromServices
        public IActionResult Index([FromServices]ICitiesService _citiesService)
        {
           List<string> cities = _citiesService.GetCities();
            return View(cities);
        }
    }

```

### Service Lifetimes in DI (Transient, Singleton, Scoped)
A **Service lifetimes** indicates when a new object of the service has to be created by the IoC/ DI container.
- **Transient** : Per injection
**Transient** lifetime service objects are created each time when they are injected.

Service instances are disposed at the end of the scope (usually, a browser request)



- **Scoped** : Per scope
**Scoped** lifetime service objects are created once per scope(usually, a browser request)

Service instances are disposed at the end of the scope (usually, a browser request)

```cs

```
- **Singleton** : For entire application lifetime
**Singleton** lifetime service objects are created for the first time when the are requested.

**When to use**
- **Singleton** : use when you want to store temporary data, such as cache service.  
- **Scoped** : use when there a DB context, because it open and close 
- **Transient** : use when you want service to be shorted-lived. For one time one controller

Service instance are disposed at application shutdown
**Fun Fact :** You can inject a service into other service
![[Pasted image 20260108155545.png]]

**To setup testing to understand**
HomeController.cs
```cs
 public class HomeController : Controller
 {
     // Dependency Injection via Constructor 
     private readonly ICitiesService _citiesService1;
     private readonly ICitiesService _citiesService2;
     private readonly ICitiesService _citiesService3;

     //constructor
     public HomeController(ICitiesService citiesService1, ICitiesService citiesService2, ICitiesService citiesService3)
     {
         // Create object of CitiesService class
         _citiesService1 = citiesService1;
         _citiesService2 = citiesService2;
         _citiesService3 = citiesService3;
     }


     [Route("/")]
     //Dependency Injection via FromServices
     //public IActionResult Index([FromServices]ICitiesService _citiesService)
     public IActionResult Index()
     {
        List<string> cities = _citiesService1.GetCities();
         ViewBag.InstanceId_CitiesService_1 = _citiesService1.ServiceInstanceId;
         ViewBag.InstanceId_CitiesService_2 = _citiesService2.ServiceInstanceId;
         ViewBag.InstanceId_CitiesService_3 = _citiesService3.ServiceInstanceId;
         return View(cities);
     }
 }
 ```
Program.cs
```cs
builder.Services.Add(new ServiceDescriptor(
    typeof(ICitiesService), 
    typeof(CitiesService), 
    ServiceLifetime.Scoped
    ));
```
ICitiesService.cs
```cs
 public interface ICitiesService
 {
     Guid ServiceInstanceId { get; }
     List<string> GetCities();
 }
```
CitiesService.cs
```cs
 public class CitiesService : ICitiesService
 {
     private List<string> _cities;
     private Guid _serviceInstanceId;

     public Guid ServiceInstanceId { get { return _serviceInstanceId; } }
     public CitiesService()
     {
         _serviceInstanceId = Guid.NewGuid();
         _cities = new List<string>() { "New York", "Los Angeles", "Chicago", "Houston", "Phoenix" };
     }
     public List<string> GetCities()
     {
         return _cities;
     }
 }
```

Home/index.cshtml
```cs
<div>
    @ViewBag.InstanceId_CitiesService_1
</div>
<div>
    @ViewBag.InstanceId_CitiesService_2
</div>
<div>
    @ViewBag.InstanceId_CitiesService_3
</div>

```

**Fun fact:** IDisposable is interface that can generate 

#### Service Scope
**Child scope** is dispose when it function finish
![[Pasted image 20260109012302.png]]
HomeController.cs
```cs
public class HomeController : Controller
{
    // Dependency Injection via Constructor 
    private readonly ICitiesService _citiesService1;
    private readonly ICitiesService _citiesService2;
    private readonly ICitiesService _citiesService3;
    private readonly IServiceScopeFactory _serviceScopeFactory;

    //constructor
    public HomeController(ICitiesService citiesService1, ICitiesService citiesService2, ICitiesService citiesService3, IServiceScopeFactory serviceScopeFactory)
    {
        // Create object of CitiesService class
        _citiesService1 = citiesService1;
        _citiesService2 = citiesService2;
        _citiesService3 = citiesService3;
        _serviceScopeFactory = serviceScopeFactory;
    }


    [Route("/")]
    //Dependency Injection via FromServices
    //public IActionResult Index([FromServices]ICitiesService _citiesService)
    public IActionResult Index()
    {
       List<string> cities = _citiesService1.GetCities();
        ViewBag.InstanceId_CitiesService_1 = _citiesService1.ServiceInstanceId;
        ViewBag.InstanceId_CitiesService_2 = _citiesService2.ServiceInstanceId;
        ViewBag.InstanceId_CitiesService_3 = _citiesService3.ServiceInstanceId;
        ViewBag.InstanceId_CitiesService_3 = _citiesService3.ServiceInstanceId;
        using (IServiceScope scope = _serviceScopeFactory.CreateScope())
        {
            // Inject CitiesService class object
            ICitiesService scopedCitiesService =
            scope.ServiceProvider.GetRequiredService<ICitiesService>();
            // DB work
            ViewBag.InstanceId_CitiesService_Scoped = scopedCitiesService.ServiceInstanceId;
        } // end of using scope, it'll call Dispose method of CitiesService class
        return View(cities);
    }
}
```
home/index.cshtml
```cs
<div>
    @ViewBag.InstanceId_CitiesService_Scoped
</div>
```

### AddTransient(), AddScoped(),  AddSingleton
Default(longer method) :
```cs
// Add service to IoC container
// when ever some class looking for ICitiesService it will give CitiesService
builder.Services.Add(new ServiceDescriptor(
    typeof(ICitiesService), 
    typeof(CitiesService), 
    ServiceLifetime.Scoped
    ));
```
AddTransient(), AddScoped(), AddSingleton() (Short method())
```cs
builder.Services.AddTransient<ICitiesService, CitiesService>(); 
builder.Services.AddScoped<ICitiesService, CitiesService>(); 
builder.Services.AddSingleton<ICitiesService, CitiesService>(); 
        
```

### View Injection
_ViewImports.cshtml
!!!csb;pcl@using ServiceContracts
home/index.cshtml
```cs
@inject ICitiesService citiesServiceInView
<div>
    </>
    @{
        List<string> citiesFromServiceInView =
            citiesServiceInView.GetCities();
    }
</div>
```

### Best Practice fro DI
**Tip:**
- **Service Locator Pattern** Avoid using service locator pattern, without creating a child scope, because it will be harder to know about dependencies of  a class. Don't invoke the DIspose() method manually for the service injected via DI. The IoC container automatically  invoke Dispose(), at the end of it scope
- **Captive Dependencies**: Don't inject scoped or transient services.
	Because, in this case, transient or scoped services act as singleton services, inside of singleton service.

![[Pasted image 20260109105903.png]]

### Autofac
**Autofac** is another  IoC container library for .Net Core Mean, both are tightly-coupled

Alternaive of **IoC_container** is **Autofac**

Replace of singleton, scope, Transient
Program.css
```cs
builder.Host.ConfigureContainer<ContainerBuilder>
(containerBuilder =>
{ 
  containerBuilder.RegisterType<CitiesService>().As<ICitiesService>().InstancePerDependency();//Transient
  containerBuilder.RegisterType<CitiesService>().As<ICitiesService>().InstancePerLifetimeScope();//Scoped
  containerBuilder.RegisterType<CitiesService>().As<ICitiesService>().SingleInstance();//Singleton
});

```

HomeController.cs
```cs
public class HomeController : Controller
{
    // Dependency Injection via Constructor 
    private readonly ICitiesService _citiesService1;
    private readonly ICitiesService _citiesService2;
    private readonly ICitiesService _citiesService3;
    // Autofac ILifetimeScope
	private readonly ILifetimeScope _lifeTimeScope;
	
	 public HomeController(ICitiesService citiesService1, ICitiesService citiesService2, ICitiesService citiesService3, IServiceScopeFactory serviceScopeFactory, ILifetimeScope lifeTimeScope)
 {
     // Create object of CitiesService class
     _citiesService1 = citiesService1;
     _citiesService2 = citiesService2;
     _citiesService3 = citiesService3;
     // Autofac ILifetimeScope
     _lifeTimeScope = lifeTimeScope;
 }
 
    [Route("/")]
   //Dependency Injection via FromServices
   //public IActionResult Index([FromServices]ICitiesService _citiesService)
   public IActionResult Index()
   {
      List<string> cities = _citiesService1.GetCities();
       ViewBag.InstanceId_CitiesService_1 = _citiesService1.ServiceInstanceId;
       ViewBag.InstanceId_CitiesService_2 = _citiesService2.ServiceInstanceId;
       ViewBag.InstanceId_CitiesService_3 = _citiesService3.ServiceInstanceId;
       ViewBag.InstanceId_CitiesService_3 = _citiesService3.ServiceInstanceId;
        // Creating scope using Autofac ILifetimeScope
	  using (ILifetimeScope scope = _lifeTimeScope.BeginLifetimeScope())
	  {
      ICitiesService citiesService = scope.Resolve<ICitiesService>();
      ViewBag.InstanceId_CitiesService_Scoped = citiesService.ServiceInstanceId;
	  }

      return View(cities);
      } 
}
```