**Laravel(Spatie)**
```
composer require spatie/laravel-permission

php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="config" 

php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="migrations"

php artisan migrate

// if permission table doesn't exist
cp vendor/spatie/laravel-permission/database/migrations/create_permission_tables.php.stub database/migrations/2025_11_05_000000_create_permission_tables.php


php artisan make:seeder RolesAndPermissionsSeeder

php artisan db:seed --class=RolesAndPermissionsSeeder


```

**Process**
![[Screenshot 2025-11-05 at 11.55.22 in the morning.png]]