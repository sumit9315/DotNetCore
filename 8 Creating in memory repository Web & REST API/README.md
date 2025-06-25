Here is the **formatted and detailed documentation** for **Part 8: Creating an In-Memory Repository in Web API**, based on your transcript. Every word is preserved — only structured formatting, headings, and layout have been added for clarity and professional use.

---

# 📘 Web API Tutorials – Part 8: Creating an In-Memory Repository

**Instructor:** Venkat
**Topic:** Moving from controller logic to a static **in-memory repository** for better code structure

---

## 🔁 Recap (from Part 7)

In the previous video, we:

* Returned **complex object responses** from an endpoint (a list of students)
* Noted that creating and manipulating data **directly inside the controller** is not a good practice

---

## 🎯 Objective

In this part, we will:

* ✅ Create an **in-memory repository**
* ✅ Move data logic out of the controller
* ✅ Access students list and individual student by ID
* ✅ Demo basic usage using Swagger UI

---

## 🏗️ Step 1: Create the Repository Class

1. Create a new class named `CollegeRepository.cs`
2. Make it a **`static` class** to store shared in-memory data

### 📄 CollegeRepository.cs

```csharp
using YourApp.Models; // Import your Student model namespace
using System.Collections.Generic;
using System.Linq;

public static class CollegeRepository
{
    public static List<Student> Students { get; } = new List<Student>
    {
        new Student
        {
            StudentId = 1,
            StudentName = "Student 1",
            Email = "email1@gmail.com",
            Address = "Hyderabad, India"
        },
        new Student
        {
            StudentId = 2,
            StudentName = "Student 2",
            Email = "email2@gmail.com",
            Address = "Bangalore, India"
        }
    };

    public static Student GetStudentById(int id)
    {
        return Students.FirstOrDefault(s => s.StudentId == id);
    }
}
```

---

## 🧠 How It Works

* `Students` is a **static list** — it acts as our in-memory database
* `GetStudentById(int id)` is a utility method to retrieve a single student using LINQ

```csharp
return Students.FirstOrDefault(s => s.StudentId == id);
```

---

## 🧱 Step 2: Use Repository in Controller

Modify `StudentController.cs`:

### Get All Students

```csharp
[HttpGet]
public IEnumerable<Student> GetStudents()
{
    return CollegeRepository.Students;
}
```

### Get Student by ID

```csharp
[HttpGet("{id:int}")]
public ActionResult<Student> GetStudentById(int id)
{
    var student = CollegeRepository.GetStudentById(id);
    if (student == null)
        return NotFound();
    return student;
}
```

📝 The route parameter `{id:int}` ensures that only **integer values** are accepted.

---

## 🧪 Step 3: Run and Test in Swagger

1. Run the application
2. Open **Swagger UI**

### Test 1: Get All Students

* Click on `/api/student`
* Click `Try it out` → `Execute`

✅ You should get a list of both students.

---

### Test 2: Get Student by ID

* Click on `/api/student/{id}`
* Click `Try it out`
* Enter a value like `2`
* Click `Execute`

✅ You should get the second student's details.

### 🛑 Type Safety:

If you enter a **string** instead of an **integer**, it will **not work**, because we defined the route with `{id:int}`.

---

## 📘 Summary

| Feature                               | Implemented? |
| ------------------------------------- | ------------ |
| Created a static in-memory repository | ✅            |
| Separated data from controller logic  | ✅            |
| Used `List<Student>` to store data    | ✅            |
| Implemented `GetStudentById(int)`     | ✅            |
| Used Swagger to test endpoints        | ✅            |

---

## 🚀 What's Next?

In the **next video**, we will explore the **routing process in detail** — how routes work, how to customize them, and how Web API maps requests to endpoints.

---

## 🙏 Conclusion

You've now:

* Cleaned up your controller
* Created a centralized **repository**
* Learned how to use `List<T>` and `LINQ`
* Improved code **readability** and **maintainability**

---

### 🎬 For More Tutorials:

**Please Like, Share, and Subscribe!**
**Thank you!**

---

Would you like a **PDF version** of this full Web API series (Parts 1–8)?
Absolutely! Here's the **updated and fully annotated version of Part 8**, now with:

✅ Line-by-line code explanation
✅ Why **each block** is used (not just what it does)
✅ Beginner-friendly comments and real-world comparisons

---

# 📘 Web API Tutorials – Part 8: Creating an In-Memory Repository

**Instructor:** Venkat
**Topic:** Moving student data from the controller to a centralized repository for better structure and scalability

---

## 🔁 Recap (Part 7)

Earlier, we created an API that returned a list of `Student` objects. But putting hardcoded data directly inside the controller:

* ❌ Makes the controller bloated
* ❌ Breaks separation of concerns
* ❌ Is not maintainable

So in this part, we’ll **refactor** the logic into a **separate repository class**.

---

## 🏗️ Step 1: Create `CollegeRepository.cs`

This file holds all **student-related data** and methods. Think of it like a **mini in-memory database**.

### 📄 Code with Explanations:

```csharp
// Required to use the Student model
using YourApp.Models;

// Required to use List<T>
using System.Collections.Generic;

// Required for LINQ functions like FirstOrDefault
using System.Linq;
```

🔍 **Why are these `using` statements needed?**

* They bring in the required namespaces so we can use types like `List<Student>`, `FirstOrDefault()`, etc.

---

```csharp
// This is a static class — meaning we don’t need to create an object of it
public static class CollegeRepository
```

🔍 **Why `static`?**

* Because we don’t need to change the state of the class or instantiate it
* Makes the data globally accessible like a shared utility

---

```csharp
    // A static list that acts as our in-memory database
    public static List<Student> Students { get; } = new List<Student>
```

🔍 **Why use `List<Student>` here?**

* We need to store multiple student records
* List is good when we may want to add/update/delete items (in future examples)

---

```csharp
    {
        new Student
        {
            StudentId = 1,
            StudentName = "Student 1",
            Email = "email1@gmail.com",
            Address = "Hyderabad, India"
        },
        new Student
        {
            StudentId = 2,
            StudentName = "Student 2",
            Email = "email2@gmail.com",
            Address = "Bangalore, India"
        }
    };
```

🔍 **Why hardcoded data?**

* For simplicity — this is just for **learning/demo**
* In a real project, you’ll fetch this data from a database

---

```csharp
    // Method to fetch a student by ID
    public static Student GetStudentById(int id)
    {
        return Students.FirstOrDefault(s => s.StudentId == id);
    }
```

🔍 **Why this method?**

* To **encapsulate** the logic of searching a student by ID
* Uses `FirstOrDefault` which:

  * Returns the **first match** if found
  * Returns `null` if not found (safe behavior)

---

## 🧱 Step 2: Update `StudentController.cs`

Instead of hardcoding the data, we’ll now fetch it using `CollegeRepository`.

---

### ✅ Block 1: Return All Students

```csharp
[HttpGet]
public IEnumerable<Student> GetStudents()
{
    return CollegeRepository.Students;
}
```

🔍 **Why this block?**

* `[HttpGet]` tells Web API to use this method for `GET /api/student`
* It fetches and returns the full list from the repository

---

### ✅ Block 2: Return Student by ID

```csharp
[HttpGet("{id:int}")]
public ActionResult<Student> GetStudentById(int id)
{
    var student = CollegeRepository.GetStudentById(id);
    if (student == null)
        return NotFound(); // Return 404 if no match found
    return student;
}
```

🔍 **Why this block?**

* `[HttpGet("{id:int}")]` tells Web API to expect `/api/student/1`, `/api/student/2`, etc.
* `ActionResult<Student>` allows flexibility:

  * Return student (`200 OK`)
  * Return error (`404 Not Found`)
* `return NotFound()` improves the API’s **usability** and **error handling**

---

## 🧪 Step 3: Test with Swagger

When you run the app:

1. Go to `https://localhost:<port>/swagger`
2. Use Swagger to try both endpoints

---

### 🔍 Why Use Swagger?

* Automatically generates UI for your APIs
* Lets you test without Postman
* Shows expected input/output and status codes

---

## 📘 Why Each Block Matters

| Block                    | Why It's Important                                                    |
| ------------------------ | --------------------------------------------------------------------- |
| `CollegeRepository`      | Central place to store and access data (not tied to controller logic) |
| `List<Student>`          | Used for flexible storage and potential future additions              |
| `static` class & members | Keeps data and logic globally accessible and consistent               |
| `GetStudentById(int)`    | Keeps filtering logic **reusable and testable**                       |
| Controller methods       | Now focus purely on **API routing and responses**, not data logic     |
| `ActionResult<Student>`  | Enables **custom error responses**, making the API user-friendly      |

---

## ✅ Summary

You’ve now:

| ✅ Feature                           | 💡 Benefit                                   |
| ----------------------------------- | -------------------------------------------- |
| Created a static repository         | Organizes your data in a single place        |
| Cleaned up the controller           | Easier to read, debug, and test              |
| Used LINQ (`FirstOrDefault`)        | Efficient and safe way to query collections  |
| Added route constraint (`{id:int}`) | Ensures only valid input types               |
| Tested in Swagger                   | Visual confirmation that API works correctly |

---

## 📦 Real-World Analogy

> Think of your **controller** as a **reception desk** and the **repository** as a **filing cabinet**.
> The controller should not store all the student records — it should **only access them** from the filing cabinet (repository).

---

## 🚀 What’s Next?

In **Part 9**, we’ll explore:

* How routing works in more detail
* How Web API maps incoming URLs to methods
* How to customize route behavior

---

## 🧾 Want a PDF Version?

Let me know if you’d like a full **PDF guide of Parts 1–8**, including these annotated explanations, sample output, and testing steps.

---

Would you like me to generate the PDF now?
Here is your **enhanced documentation for Part 8**, now with **code-by-code explanations** and **keyword meaning** — great for both beginners and those reviewing best practices.

---

# 📘 Web API Tutorials – Part 8: Creating an In-Memory Repository

**Instructor:** Venkat
**Topic:** Refactoring to use a **static repository** to store and manage data separately from the controller logic

---

## 🔁 Recap (Part 7)

Previously, we:

✅ Returned a **list of `Student` objects** from the controller
❌ Noted that storing data **inside the controller** is not scalable or clean

---

## 🎯 Goal of This Tutorial

By the end of this part, you will:

✅ Create a **central repository** for student data
✅ Access **all students** or a **specific student by ID**
✅ Use **LINQ** and proper **route handling**
✅ Improve **code separation and maintainability**

---

## 🏗️ Step 1: Create `CollegeRepository.cs`

### 📄 Full Code:

```csharp
using YourApp.Models; // Allows access to the Student class
using System.Collections.Generic; // Gives access to List<T>
using System.Linq; // Enables LINQ queries like FirstOrDefault

public static class CollegeRepository
{
    // Static list acting as in-memory data source (mock database)
    public static List<Student> Students { get; } = new List<Student>
    {
        new Student
        {
            StudentId = 1,
            StudentName = "Student 1",
            Email = "email1@gmail.com",
            Address = "Hyderabad, India"
        },
        new Student
        {
            StudentId = 2,
            StudentName = "Student 2",
            Email = "email2@gmail.com",
            Address = "Bangalore, India"
        }
    };

    // Method to get student by ID using LINQ
    public static Student GetStudentById(int id)
    {
        return Students.FirstOrDefault(s => s.StudentId == id);
    }
}
```

---

### 🔍 Line-by-Line Code Explanation:

| Line                                           | Purpose                                                            |
| ---------------------------------------------- | ------------------------------------------------------------------ |
| `public static class CollegeRepository`        | Declares a static class — no need to create an object to access it |
| `List<Student> Students { get; }`              | Property that exposes the **in-memory list** of students           |
| `= new List<Student> { ... }`                  | Initializes the list with two sample student objects               |
| `public static Student GetStudentById(int id)` | Static method to retrieve a student based on ID                    |
| `Students.FirstOrDefault(...)`                 | LINQ method: returns the **first match** or **null** if no match   |

---

### 🔑 Important Keywords:

| Keyword            | Meaning                                                                             |
| ------------------ | ----------------------------------------------------------------------------------- |
| `static`           | Makes members accessible **without instantiating** the class                        |
| `List<T>`          | A strongly-typed collection (generic)                                               |
| `LINQ`             | Allows querying collections using readable syntax (`Where`, `FirstOrDefault`, etc.) |
| `FirstOrDefault()` | Returns the **first item** that matches the condition, or **null**                  |

---

## 🧱 Step 2: Use the Repository in the Controller

### 📄 `StudentController.cs`

#### ✅ Get All Students

```csharp
[HttpGet]
public IEnumerable<Student> GetStudents()
{
    return CollegeRepository.Students;
}
```

##### 🔍 Explanation:

| Code                         | What it Does                                 |
| ---------------------------- | -------------------------------------------- |
| `[HttpGet]`                  | Maps this method to `GET /api/student`       |
| `IEnumerable<Student>`       | Returns a collection (readonly) of students  |
| `CollegeRepository.Students` | Accesses the static list from the repository |

---

#### ✅ Get Student by ID

```csharp
[HttpGet("{id:int}")]
public ActionResult<Student> GetStudentById(int id)
{
    var student = CollegeRepository.GetStudentById(id);
    if (student == null)
        return NotFound(); // 404 if not found
    return student;
}
```

##### 🔍 Explanation:

| Code                                   | What it Does                                                             |
| -------------------------------------- | ------------------------------------------------------------------------ |
| `[HttpGet("{id:int}")]`                | Maps to `GET /api/student/2` — only allows integers                      |
| `ActionResult<Student>`                | Allows returning both **data (200 OK)** and **status codes (e.g., 404)** |
| `CollegeRepository.GetStudentById(id)` | Calls our repository method                                              |
| `return NotFound();`                   | Returns 404 status if student not found                                  |
| `return student;`                      | Returns 200 OK with student object                                       |

---

## 🧪 Step 3: Test in Swagger UI

Run the project and open **Swagger** (usually at `/swagger`).

### ▶️ Test: Get All Students

* Click on `/api/student`
* Click **Try it out** → **Execute**
* ✅ Response: List of student objects in JSON

### ▶️ Test: Get by ID

* Click `/api/student/{id}`
* Try value `2` → Execute
* ✅ Returns that student's details

---

## 🚨 Route Safety: `{id:int}`

By specifying `{id:int}`:

* ❌ `/api/student/abc` → will **not match**
* ✅ `/api/student/2` → valid request

This makes your API **type-safe and predictable**.

---

## 🧠 Summary

| Feature                 | Purpose / Benefit                  |
| ----------------------- | ---------------------------------- |
| `CollegeRepository.cs`  | Central location for student data  |
| `List<Student>`         | Stores in-memory student records   |
| `GetStudentById(int)`   | Find a student using LINQ          |
| `StudentController.cs`  | Lightweight, uses repository logic |
| Swagger Testing         | Easily test `GET` endpoints        |
| `[HttpGet("{id:int}")]` | Ensures type-safe routing          |

---

## 🚀 What’s Next?

In **Part 9**, we’ll explore:

* How routing works in Web API
* Customizing route patterns
* Default vs Attribute Routing

---

## 🙏 Conclusion

By now, you’ve:

✅ Moved logic out of the controller
✅ Built a simple but powerful in-memory data layer
✅ Used LINQ and proper routing patterns
✅ Prepared your app for real-world scaling

---

## 📄 Want a PDF?

Would you like a **PDF version** of this complete series (Part 1–8), including code, explanations, and Swagger testing tips?

Let me know and I’ll generate it for you!
