**Controller** is a class that is used to group-up a set of actions / action-method

**Fun fact:** Think controller as Service

**Responsibilities of Controllers**
- Reading requests
- Invoking models
- Validation
- Preparing Response

### **ContentResult**
**ContentResult** can represent any type of response, based on the specified MIME type. 
**MIME** type represents type of the content such as text/plain, text/html, application/json, application, xml, application/pdf etc.

### JsonResult
**JsonResult** can represent an object in Javascript Object Notation(JSON) format. 

### File Results
**File Result** sends the content of a file as response.
**Type of File result**
- VirtualFileReult 
```cs
[Route("virtual_file")]
public VirtualFileResult VirtualFile()
        {
            //================= Method 1 ======================
            //return new VirtualFileResult("/medical.pdf", "application/pdf");
            //return new VirtualFileResult("/test01.png", "image/png");
            //================= Method 2 ======================
            return File("/test01.png", "image/png");
        }


```
- PhysicalFileResult
```cs
[Route("physical_file")]
public PhysicalFileResult PhysicalFile()
        {
            //================= Method 1 ======================
            //return new VirtualFileResult("/medical.pdf", "application/pdf");
            //PhysicalFileResult only take lower-letter-case
            //return new PhysicalFileResult(@"c:\users\administrator\documents\c# lesson\controllerexample\controllerexample\wwwroot\medical.pdf", "application/pdf");
            //================= Method 2 ======================
            return PhysicalFile(@"c:\users\administrator\documents\c# lesson\controllerexample\controllerexample\wwwroot\medical.pdf", "application/pdf");
        }


```
- FileContentResult
```cs
[Route("content_file")]
public FileContentResult ContentFile()
        {
            //return new VirtualFileResult("/medical.pdf", "application/pdf");
            byte[] bytes = System.IO.File.ReadAllBytes(@"c:\users\administrator\documents\c# lesson\controllerexample\controllerexample\wwwroot\medical.pdf");
            //================= Method 1 ======================
            //PhysicalFileResult only take lower-letter-case
            //return new FileContentResult(bytes, "application/pdf");
            //================= Method 2 ======================
            return File(bytes, "application/pdf");
        }

```

**Fun fact:** 
- VirtualFileResult is convince than PhysicalFIleResult
- FileContentResult is best with the file you want to encrytp


### IActionResult
It is the parent interface for all action result classes such as ContentResult, JsonResult, RedirectResult, StatusCodeResult, ViewResult etc.

**When to use it**
Use it when there are multiple return type (Content_result, File)

![[Pasted image 20260103194007.png]]