 In C#
- Add additional column we do it database first then code. Opposite from laravel, which is create migratation file first then database

How to
- Run this cmd. It will migrate entities file accord to database
```cs
 Scaffold-DbContext "data source=camgsm-odsdb01.camgsm.com.kh;initial catalog=ESS-App;user id=DIP;password=`$Rootuser7270" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Entities\EPlannedWork -Schemas EPW -contextDir Data\EPWDbContext -DataAnnotations -Force

```
Paste it to **Package Manager Console**
