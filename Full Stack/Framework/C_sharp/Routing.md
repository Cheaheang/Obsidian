There 2 method for routing in the past 
- UseRouting
- UseEndpoints
**Nowadays** Routing is handle automatically by Routing configuration 
![[Pasted image 20251231141216.png]]

**To Route in isp.net**
```cs
// for get
app.MapGet("map1", async context =>
{
    await context.Response.WriteAsync("in Map 1");
});
// for get with PARAMETER & Defualt value
app.MapGet("default/{filename=text}", async context =>
{
    string? fileName = Convert.ToString(context.Request.RouteValues["filename"]);
    await context.Response.WriteAsync($"in Map with file { fileName } as default value ");
});

// for post
app.MapPost("map2", async context =>
{
    await context.Response.WriteAsync("in Map 2");
});
// for both post & get
app.Map("map2", async context =>
{
    await context.Response.WriteAsync("in Map 2");
});
// When there is no match route. it will direct to this fallback
app.MapFallback(async context =>
{
    await context.Response.WriteAsync("in Fallback");
});

```
**Fun Fact:** Parameter don't space. if have error 

## Route Constraints
![[Pasted image 20251231151645.png]]

```cs
// for get with constrains route(int)
app.MapGet("constrains/{filename:int?}", async context =>
{
    string? fileName = Convert.ToString(context.Request.RouteValues["filename"]);
    await context.Response.WriteAsync($"in Map with file { fileName } as default value ");
});
// for get with constrains route(timestamp)
app.MapGet("constrains-timestamp/{time:datetime?}", async context =>
{
    DateTime time = Convert.ToDateTime(context.Request.RouteValues["time"]);
    await context.Response.WriteAsync($"in Map with time {time.ToShortDateString() } as constrain value ");
});
// for get with constrains route(timestamp)
app.MapGet("constrains-timestamp/{time:datetime?}", async context =>
{
    DateTime time = Convert.ToDateTime(context.Request.RouteValues["time"]);
    await context.Response.WriteAsync($"in Map with time {time.ToShortDateString() } as constrain value ");
});
// for get with constrains route(guid)
app.MapGet("constrains-guid/{guid:guid}", async context =>
{
    Guid guid = Guid.Parse(Convert.ToString(context.Request.RouteValues["guid"]));
    await context.Response.WriteAsync($"in Map with guid {guid} as constrain value ");
});

// for get with constrains route(minlength-maxlength)
// range == length
app.MapGet("constrains-min-max-length/{guid:length(4,12)}", async context =>
{
    Guid guid = Guid.Parse(Convert.ToString(context.Request.RouteValues["guid"]));
    await context.Response.WriteAsync($"in Map with minlength {guid} as constrain value ");
});

// for get with constrains route(regex)
// use regex for formate or filter the value 
// for get with constrains route(regex)
app.Map("constrains-regex/{month:regex(^(apr|jul|oct|jan)$)}", async context =>
{
    string? month = Convert.ToString(context.Request.RouteValues["month"]);
    await context.Response.WriteAsync($"in Map with regex {month} as constrain value ");
});


```

## Custom Route Constraint Class
Create class for constraint
To make it work you need to add(AddRouting) to your class to program.cs
if the regular express(regex) didn't match it return false 
Use this method when you want to reuse the route parameter format

create class for Custom Constriant
```cs
public class MonthCustomContrains : IRouteConstraint
{
        public bool Match(HttpContext? httpContext, IRouter? route, string routeKey, RouteValueDictionary values, RouteDirection routeDirection)
        {
            if(!values.ContainsKey(routeKey))
            {
                return false;
            }

            Regex regex = new Regex($"^(apr|jul|oct|jan)$");
            // By default route value is returned as object
            string? monthValue = Convert.ToString(values[routeKey]);

            if(regex.IsMatch(monthValue))
            {
                return true;
            }
            return false;


        }
}
```
In program.cs
```cs
using RoutingExample.CustomConstrains;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRouting(option =>
{
    option.ConstraintMap.Add("months", typeof(MonthCustomContrains));
});

app.Map("route-constrains-regex/{month:months}", async context =>
{
    string? month = Convert.ToString(context.Request.RouteValues["month"]);
    await context.Response.WriteAsync($"in Map with route constrain regex {month} as constrain value ");
});
```

## Endpoint Selection order

If you have route with the same name or similar it will compare via below. if it higher it execute first

| Description                                                                                                                | Example                              |
| -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| URL template with more segments                                                                                            | "a/b/c/d" is higher than "a/b/c"     |
| URL template with literal text has more precedence than a parameter segment.                                               | "a/b" is higher than "a/{parameter}" |
| URL template that has a parameter segment with constraint has more precedence than a parameter segment without constraints | "a/{b:int}" is higher than "a/{b}"   |
| Catch-all parameters (**)                                                                                                  | "a/{b}" is higher than "a/**"        |
|                                                                                                                            |                                      |

## WebRoot and UseStaticFiles()
The default WebRoot folder is "wwwrot"
You can configutre manually
**WebRoot**
![[Pasted image 20260103122950.png]]