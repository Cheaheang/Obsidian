
**Middelware:** is a component that is assembled to application pipeline to handle requests and response 

middleware are chained one-after-other and execute in the same sequence how they're add

middleware1->middleware2->middleware3

Middleware can be request delegate (anonymous method or lambda express ) or a class

app.Use is use for middle that can either forward subsequence middleware and terminate middleware change

**Fun fact:** 
- In **Program.cs** can only once **app.Use** or **app.Run**
- In **app.Use** take 2 parameter(HttpContext, RequestDelegate) and **app.Run** take only 1 parameter(HttpContext)

**Solution for run multiple app**
```cs
// middleware 1
app.Use(async (HttpContext context, RequestDelegate next) =>
{
    await context.Response.WriteAsync("Hello from Run middleware with USE!");
    await next(context);
});
// middleware 2
app.Use(async (HttpContext context, RequestDelegate next) =>
{
    await context.Response.WriteAsync("Hello from Run middleware with USE_2! \n");
    await next(context);
});
// middleware 3
app.Run(async (HttpContext context) =>
{
    await context.Response.WriteAsync("Hello from Run middleware!");
});

app.Run();
```
**Middleware chain**
![[Pasted image 20251230155240.png]]

## Custom middleware class
Middleware class is used to separate the middleware logic from a lambda expression to a separate / reusable class

![[Pasted image 20251230160006.png]]

**The process of separate middle to class**
Middleware-class => Middleware-service=>UseMiddleware
- Crete middleware-class 
- Call that class via Middleware-service 
- UseMiddleware<Middle_Class_Name>();

## Custom middleware extension
**Fun fact:** In one namespace can have multiple class 

Middleware extension allow you to create new method then inject it to an existing object like app

**To create a method for app**

```cs
public static class CustomMiddlewareExtension
{
	public static IApplicationBuilder UseCustomMiddleware(this IApplicationBuilder app)
	{
	
	}
}
```

**Why we need multiple middleware**
Because it will put authentication or other function one after other

## Conventional Middleware(Need to understand custom middleware extension before study this)
To create this middleware, you need: 
- Add Item
- Middleware Class
- Put it add program.cs

## Right Order of Middleware
![[Pasted image 20251230184554.png]]