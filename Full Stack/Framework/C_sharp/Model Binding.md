**Model Binding** is a feature of asp.net of asp.net core that reads values from http requests and pass them as arguments to the action method. 

**Route Data:**
```
https://api.example.com/employees/123
```
**Query String**
```
https://api.example.com/products?category=electronics&sort=price
```

**FromQuery and FromRoute**
If you don't mention FromQuery & FromRoute, the controller function take both
**FromQuery** get data via Query String
**FromRoute** get data via Route Data

### Models
**Models** is a class that represents structure of data(as properties) that you would like to receive from the request and/or send to the response. 

You can use FromQuery and FromRoute in Model

```cs 
    public class Book
    {
        [FromQuery]
        public int BookId { get; set; }
        [FromQuery]
        public string Author { get; set; }
        override public string ToString()
        {
            return $"BookId: {BookId}, Author: {Author}";
        }
    }

```


### Model Validation
```cs
    public class Person
    {
        [Required]
        public string? PersonName { get; set; }
        public override string ToString()
        {
	    return $"PersonName: {PersonName}, Email: {Email}, Phone: {Phone}, Password: {Password}, ConfirmPassword: {ConfirmPassword}, Price: {Price}";
        }
    }

```

### Model state
- **IsValid** Specific whether there is at least one validation error or not(true or false) 
- **Values** Contains each model property value with corresponding "Errors" property that contains list of validation errors of that model property 
- **ErrorCount** Returns number of errors

### Model Validation

```cs
[Required(ErrorMessage = "value")]
```
Specifies that the property value is required (can't be blank or empty)

```cs
[StringLength(int maximunLength, MinimumLength = value, ErrorMessage = "value")]
```
Specifies minimum and maximum length (number of characters) allowed in the string

```cs
[Range(int minimum, int maximum, ErrorMessage = "value")]
```
Specifies minimum and maximum numerical value allowed.

```cs
[RegularExpression(string pattern, ErrorMessage = "value")]
```
Specifies the valid pattern (regular expression)

```cs
[EmailAddress(ErrorMessage = "value")]
```
Specifies that the value should be a valid email address

```cs
[Phone(ErrorMessage = "value")]
```
Specifies that the value shoudl be a valid phone number
ex: (999)-999-9999 or 099 406 6077

```cs
[compare(string otherProperty, ErrorMessage = "value")]
```
Specifies that the values of current property and other property should be the same

```cs
[Url(ErrorMessage = "value")]
```
Specifies that the value should be a valid url (website address)

```cs
[ValidateNever]
```
Specifies that the property should not be validated (excludes the property from model validation)

### Custom validation

```cs
 public class Person: IValidatableObject
 {
	[MinimumYearValidator(2005)]
	public DateTime? DateOfBirth { get; set; }
 }
```
**MinimumYearValidator**
```cs
namespace ModelValidationExample.Controllers.CustomValidators
{
    public class MinimumYearValidatorAttribute: ValidationAttribute
    {
        public int MinimumYear { get; set; } = 2000;
        public string DefaultErrorMessage { get; set; } = "Year should not be less than {0}";

        public MinimumYearValidatorAttribute()
        { }
        public MinimumYearValidatorAttribute(int minimumYear)
        {
            MinimumYear = minimumYear;
        }

        protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)
        {
            if(value != null)
            {
                DateTime date = (DateTime)value; 
                if(date.Year >= MinimumYear)
                {
                    return new ValidationResult(string.Format(ErrorMessage ?? DefaultErrorMessage, MinimumYear));
                    //return new ValidationResult($"\n Minimum year allowed is {MinimumYear} \n");
                }
            }else
            {
                return ValidationResult.Success;
            }
            return null;
        }
    }
}

```

### Multiple Custom validation
```cs
public DateTime? FromDate { get; set; }
[DateRangeValidators("FromDate", ErrorMessage = "From Date shouldn't be older than or equal than To Date")]
public DateTime? ToDate { get; set; }
```
**DateRangeValidators**
```cs
using System.Reflection;

namespace ModelValidationExample.Controllers.CustomValidators
{
    public class DateRangeValidators : ValidationAttribute
    {
        public string OtherPropertyName { get; set; }
        public DateRangeValidators(string otherProperty)
        {
                OtherPropertyName = otherProperty;
        }
        protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)
        {
            if(value != null)
            {
            // Get the value of to_date
            DateTime? to_date = Convert.ToDateTime(value);
            //get from_date
            // other property represent the from date
            //Property
            PropertyInfo? otherProperty = validationContext.ObjectType.GetProperty(OtherPropertyName);

                if(otherProperty != null)
                {
           DateTime from_date = Convert.ToDateTime(otherProperty.GetValue(validationContext.ObjectInstance));

            if(from_date > to_date)
            {
                return new ValidationResult(ErrorMessage, new string[] { OtherPropertyName, validationContext.MemberName});
                    }
                    else
                    {
                        return ValidationResult.Success;
                    }

        }
                    return null;
                }
                return null;
            }
    }
}

```
### IValidatableObject
**Fun fact:** 
- IEnumerable accept multiple return value
- Use **yield**, when you want to return **multi return** 
- Use **IValidatableObject**, when use don't want to **reuse** this validation

In the same class of Person.cs (Object)
```cs
    public class Person: IValidatableObject
    {
    
    public DateTime? DateOfBirth { get; set; }

    public int? Age { get; set; }


        public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
        {
            if(DateOfBirth.HasValue == false && Age.HasValue == false)
            {
                // Benefit of variable here is when you want rename Age. You can change by refector only the first one
               yield return new ValidationResult("Either Date of birth or age must supplied", new[] { nameof(Age)});
            }
        }

}
    

```

### Bind
**Bind** attribute specifics that only the specified properties should be included in model binding
**Fun fact:** 
- In general, **Bind** is attribute of properties would you like to include in **Model Binding**
- use **nameof**, when you want to create call the object. Whenever you want to rename, **DON'T** rename it manually. Right click and select **Rename**

We use Bind in Controller


```cs
    public class HomeController : Controller
    {
        [Route("register")]
        public IActionResult Index([Bind(nameof(Person.Email), nameof(Person.Password), nameof(Person.PersonName), nameof(Person.ConfirmPassword))] Person person)
        {
	// code
}
}
```

When there are 25 properties and you want only 20 properties and unwanted 5 properties
In **Person.cs**
In HomeController.cs
```cs
    public class HomeController : Controller
    {
        [Route("register")]
        public IActionResult Index(Person person)
        {
        //code
		}
	}
```
In Person.cs
```cs
    public class Person: IValidatableObject
    {

        [MinimumYearValidator(2005)]
        [BindNever]
        public DateTime? DateOfBirth { get; set; }
    }

```
With this, **DateOfBirth** never include

### Custom Model Bind
When working on complex logic, you need to use custom model binder
We can **reuse** the model binding

Add this ([ModelBinder(BinderType = typeof(PersonModelBinder))])  to controller
```cs
        public IActionResult Index([ModelBinder(BinderType = typeof(PersonModelBinder))]Person person)
        {
		// Code
		}
```

Create **CustomModelBinder** folder, then create **PersonModelBinders.cs** file
```cs
namespace ModelValidationExample.CustomModelBinders
{
    public class PersonModelBinder : IModelBinder
    {
        public Task BindModelAsync(ModelBindingContext bindingContext)
        {
            Person person = new Person();

            // Firstname and Lastname
            if(bindingContext.ValueProvider.GetValue("FirstName").Length > 0)
            {
               person.PersonName = bindingContext.ValueProvider.GetValue("FirstName").FirstValue;
            }
            if(bindingContext.ValueProvider.GetValue("LastName").Length > 0)
            {
                person.PersonName += " " + bindingContext.ValueProvider.GetValue("LastName").FirstValue;
            }

            // Email
            if (bindingContext.ValueProvider.GetValue("Email").Length > 0)
            {
                person.Email = bindingContext.ValueProvider.GetValue("Email").FirstValue;
            }

            // Phone
            if (bindingContext.ValueProvider.GetValue("Phone").Length > 0)
            {
                person.Phone = bindingContext.ValueProvider.GetValue("Phone").FirstValue;
            }

            // DateOfBirth
            if (bindingContext.ValueProvider.GetValue("DateOfBirth").Length > 0)
            {
                person.DateOfBirth = Convert.ToDateTime(bindingContext.ValueProvider.GetValue("DateOfBirth").FirstValue);
            }

            // FromDate
            if (bindingContext.ValueProvider.GetValue("FromDate").Length > 0)
            {
                person.FromDate = Convert.ToDateTime(bindingContext.ValueProvider.GetValue("FromDate").FirstValue);
            }

            // ToDate
            if(bindingContext.ValueProvider.GetValue("ToDate").Length > 0)
            {
                person.ToDate = Convert.ToDateTime(bindingContext.ValueProvider.GetValue("ToDate").FirstValue);
            }

            // Password
            if(bindingContext.ValueProvider.GetValue("Password").Length > 0)
            {
                person.Password = bindingContext.ValueProvider.GetValue("Password").FirstValue;
            }

            // ConfirmPassword
            if (bindingContext.ValueProvider.GetValue("ConfirmPassword").Length > 0)
            {
                person.ConfirmPassword = bindingContext.ValueProvider.GetValue("ConfirmPassword").FirstValue;
            }

            // Price
            if (bindingContext.ValueProvider.GetValue("Price").Length > 0)
            {
                person.Price = bindingContext.ValueProvider.GetValue("Price").FirstValue;
            }

            bindingContext.Result = ModelBindingResult.Success(person);

            return Task.CompletedTask;
        }

    }
}

```

### Custom Model Binder Provider

**Fun fact:** 
- We put **0** because, we want the ModelBinderProvider to reach. The 0 will make that model execute first 

In **CustomModelBinder** 
```cs
        [Route("register")]
        //Binding Data
        //public IActionResult Index([Bind(nameof(Person.Email), nameof(Person.Password), nameof(Person.PersonName), nameof(Person.ConfirmPassword))] Person person)
        //ModelBinder
        //public IActionResult Index([ModelBinder(BinderType = typeof(PersonModelBinder))]Person person)
        //ModelBinderProvider
        public IActionResult Index(Person person)
        {
        // code
        }
```
Create **PersonBinderProvider.cs** in **CustomModelBinders**
```cs
    public class PersonBinderProvider : IModelBinderProvider
    {
        //This Provider execute before ModelBinder
        public IModelBinder? GetBinder(ModelBinderProviderContext context)
        {
            if (context.Metadata.ModelType == typeof(Person))
            {
                return new BinderTypeModelBinder(typeof(PersonModelBinder));
            }
            return null;
        }
    }
```
Modify **Program.cs**
```cs
// Normal controller with BinderProvider
//builder.Services.AddControllers();
// Controller with BinderProvider
builder.Services.AddControllers(options =>
{
    options.ModelBinderProviders.Insert(0, new PersonBinderProvider());
});
```

### Collection Binding
When there is multiple phone number or other field that require multiple .
use List 
```cs
    public List<string> Tags { get; set; } = new List<string>();
```

### FromHeader
This method is use for query data from header that you need 
```cs
// Method 1 
            //ControllerContext.HttpContext.Request.Headers["key"];
// Method 2 
public IActionResult Index(Person person, [FromHeader(Name = "User-Agent")]string UserAgent)
{
//code
}
```

### Input Formatter 
When in postman you make request with XML format. You need to make your controller accept XML Format. By default Input format accept JSON
In **Program.cs**

```cs
builder.Services.AddControllers(options =>
{
    //options.ModelBinderProviders.Insert(0, new PersonBinderProvider());
}).AddXmlDataContractSerializerFormatters();

```
