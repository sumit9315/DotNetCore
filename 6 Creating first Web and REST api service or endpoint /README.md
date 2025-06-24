Here is the **formatted and detailed documentation** for **Part 6: Creating Your First Web API Service**, based entirely on your transcript. Every word is preserved — only structure, headings, and layout have been added for readability and professional presentation.

---

# 📘 Web API Tutorials – Part 6: Creating Your First Web API Service

**Instructor:** Venkat
**Topic:** Step-by-step creation of your first Web API service using Visual Studio 2022

---

## 🧰 Tools Required

In this tutorial, we will be using:

* ✅ **Microsoft Visual Studio 2022**
* ✅ **.NET 6.0 Framework**
* ✅ **C#**
* ✅ **Swagger UI (for testing the API)**

---

## 🏁 Step 1: Create a New Web API Project

1. **Open Visual Studio 2022**

   * Left side: Recent projects
   * Right side: Options to create or open a project

2. **Click on:**
   `Create a new project`

3. **Choose Template:**

   * Search for `ASP.NET Core Web API`
   * Choose the one built for **C#**

4. **Click on:**
   `Next`

5. **Enter Project Name:**

   * Example: `CollegeApp`

6. **Select Framework:**

   * Choose `.NET 6.0` (or the latest installed)

7. **Click on:**
   `Create`

---

## 📁 Step 2: Project Structure Overview

Once the project is created:

* You’ll see folders and files such as:

  * `Program.cs` – Main configuration file
  * `appsettings.json` – For app-level configurations
  * `Controllers` folder – To contain your controller files

---

## 🧹 Step 3: Clean Up Default Controllers

* Delete the default controller files from the `Controllers` folder to start fresh.

---

## 🏗️ Step 4: Add Your First Controller (Manual Approach)

1. **Right-click** on the `Controllers` folder

2. Click on → `Add` → `Class`

3. Name the class:
   `StudentController.cs`

4. Modify the class:

```csharp
[ApiController]
[Route("api/[controller]")]
public class StudentController : ControllerBase
{
    [HttpGet]
    public string GetStudentName()
    {
        return "student name one";
    }
}
```

5. Make sure to:

   * Inherit from `ControllerBase`
   * Use `[ApiController]` and `[Route("api/[controller]")]`
   * Decorate your action method with `[HttpGet]` to avoid runtime errors

---

## ▶️ Step 5: Run the Web API

* Click the **green play button** (or press **F5**) to build and launch your API
* The browser will open Swagger UI
* Swagger auto-generates docs and allows testing

### 🧪 Test Your Endpoint in Swagger:

* Click on the `Student` controller
* Click on `GET` → `Execute`
* You should see the response:
  `"student name one"`

---

## ✏️ Step 6: Alternative – Add Controller via Template

1. **Stop the running project**

2. **Right-click** on the `Controllers` folder

3. Click → `Add` → `Controller`

4. In the pop-up:

   * Select: `API Controller – Empty`
   * Click `Add`

5. This method auto-generates:

```csharp
[ApiController]
[Route("api/[controller]")]
public class DefaultController : ControllerBase
{
    // Auto-created structure
}
```

### ✅ Benefit:

* Using the controller **template** instead of a blank class saves time and avoids missing attributes/configurations.

---

## 📝 Notes:

* For future controllers, prefer using the **Controller Template** method over manually creating a class.
* We’ll later enhance the controller to return **full student details** instead of just a static string.

---

## 🙏 Conclusion

You’ve now created your **first Web API service**, added a simple GET endpoint, and tested it using **Swagger UI**.

---

### 🎬 For More Tutorials:

**Please Like, Share, and Subscribe!**
**Thank you!**

---

Would you like this entire tutorial series in a **PDF format** for download or sharing?
