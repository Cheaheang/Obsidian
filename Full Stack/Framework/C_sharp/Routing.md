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