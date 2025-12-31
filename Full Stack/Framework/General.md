**Fun fact:**
- In contrast of 
```php
public function store(Response res){
	dd(res.all())
}
```
```cs
app.Run(async (HttpContext context)=> {
	StreamReader reader = new StreamReader(context.Request.Body);
	
})
```