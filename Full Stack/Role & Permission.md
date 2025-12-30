There are 2 type of **Permission Model**
	- Role-based access control (RBAC)
	- Attribute-based access control(ABAC)
To view more detail 
https://www.nikhilajain.com/post/user-role-permission-model

**Set Middleware in backend**
why we need to write 
```
 ->withMiddleware(function (Middleware $middleware): void {
        //
        $middleware->alias([
            'permission' => CheckPermission::class
        ]);
    })
```
in booststrap/app.php 
and 
Middleware /CheckPermission.php
```
    public function handle(Request $request, Closure $next, $permission): Response
    {
        if(Auth::user()->hasPermission($permission)){
            return $next($request);
        }
        abort(403, " You are not permitted");
    }
```

 **Set up Authication in frontend**
 Setup storeAdmin(login, initFromStorage) -> Login.vue(handleLogin) -> 
 create folder middleware/auth.ts -> 
 Go to Index.vue paste in script ->
 ```javaScript
 definePageMeta({  
  middleware: 'auth',  
  requiresAuth: true,  
})
 ```
Go to app.vue paste this to onmounted
```javaScript
const storeAdmin = useAdmin()  
onMounted(async() => {  
 storeAdmin.initFromStorage()    
 })
```