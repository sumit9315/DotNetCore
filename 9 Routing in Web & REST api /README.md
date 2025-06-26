Here is the **formatted and professional documentation** for **Part 9: Routing in Web API**, based entirely on your transcript. All original content is preserved — only formatting, headings, and code structuring have been applied for clarity and ease of understanding.

---

# 📘 Web API Tutorials – Part 9: Routing in Web API

**Instructor:** Venkat
**Topic:** Understanding how **routing** works in ASP.NET Web API using **attribute-based routing**

---

## 📌 What is Routing in Web API?

Routing is the process of **mapping a URI** to a specific **action method** in a controller. It determines which **method gets executed** when an HTTP request is made.

---

## 🎯 Core Concept

* In Web API, every **action method** is associated with:

  * An **HTTP verb** (like GET, POST, DELETE, etc.)
  * A **Route**

These two together determine which method will be called for a given request.

---

## 🧭 URI + HTTP Verb = Action Mapping

### Example: Get All Students

| Component         | Value                          |
| ----------------- | ------------------------------ |
| **HTTP Verb**     | `GET`                          |
| **URI**           | `http://localhost/api/student` |
| **Action Method** | `GetAllStudents()`             |

To trigger this action:

* Call the **full URL** (not just the controller name)
* Use the correct **HTTP verb**

---

## 🔁 Example 2: Get Student by ID

* **Action:** `GetStudentById(int id)`
* **Verb:** `GET`
* **URI:** `api/student/{id}`
* For example: `api/student/1`

Although both this and the “get all” method use `GET`, they are **distinguished by route**.

---

## ❌ Same URI, Different Verb

### Example:

| Method                      | Verb   | Route              |
| --------------------------- | ------ | ------------------ |
| `GetStudentById(int id)`    | GET    | `api/student/{id}` |
| `DeleteStudentById(int id)` | DELETE | `api/student/{id}` |

These methods use the **same URI** but **different verbs**, so they are **uniquely identified**.

✅ This is valid and a common pattern in RESTful APIs.

⚠️ Be careful:

* If you use `GET` on `api/student/1` → you retrieve the student
* If you use `DELETE` on `api/student/1` → you delete the student

---

## 🧱 Code-Level Routing

### Controller-Level Route

```csharp
[Route("api/[controller]")]
[ApiController]
public class StudentController : ControllerBase
```

* `[controller]` is a **token** that gets replaced by the controller name (without `Controller` suffix)
* If the controller name changes, the route auto-adjusts

---

## 🧩 Four Example Actions

### 1. Get All Students

```csharp
[HttpGet("all", Name = "GetAllStudents")]
public IEnumerable<Student> GetStudents()
```

### 2. Get Student by ID

```csharp
[HttpGet("{id:int}", Name = "GetStudentById")]
public ActionResult<Student> GetStudentById(int id)
```

### 3. Get Student by Name

```csharp
[HttpGet("name/{name}", Name = "GetStudentByName")]
public ActionResult<Student> GetStudentByName(string name)
```

### 4. Delete Student by ID

```csharp
[HttpDelete("{id:int}", Name = "DeleteStudentById")]
public bool DeleteStudent(int id)
{
    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);
    if (student != null)
    {
        CollegeRepository.Students.Remove(student);
        return true;
    }
    return false;
}
```

---

## ⚙️ Defining Routes with Attributes

There are two ways to define routes:

### ✅ Option 1: `Route` + Verb

```csharp
[Route("student/all")]
[HttpGet]
```

### ✅ Option 2: Inline Route in Verb Attribute

```csharp
[HttpGet("all", Name = "GetAllStudents")]
```

Use whichever style suits your application’s routing strategy.

---

## 🔁 Route Parameters

* Use `{}` for dynamic segments
* Example: `api/student/{id:int}`

| `{id}` | A placeholder for the value in the route |
| ------ | ---------------------------------------- |
| `int`  | Route constraint (only accept integers)  |

---

## ⚠️ Routing Conflicts

### Problem:

Two methods with similar dynamic route parameters:

```csharp
[HttpGet("{id}")]
public Student GetById(int id)

[HttpGet("{name}")]
public Student GetByName(string name)
```

⚠️ This causes a **conflict** because both routes expect a single parameter, and Web API **can't decide** which one to execute.

✅ Solution:

* Use explicit route prefixes like:

  * `"id/{id:int}"`
  * `"name/{name}"`

---

## ✅ Good Route Naming Practice

| Method               | Suggested Route                    |
| -------------------- | ---------------------------------- |
| Get All Students     | `api/student/all`                  |
| Get Student by ID    | `api/student/1`                    |
| Get Student by Name  | `api/student/name/venkat`          |
| Delete Student by ID | `api/student/1` (with DELETE verb) |

---

## 🧪 Testing Routes in Swagger or Postman

* Select the **HTTP verb**
* Enter the **full URI**
* Provide any required parameters
* Click **Execute**

✅ Swagger uses route attributes to auto-generate UI for testing endpoints.

---

## 🧠 Key Takeaways

| Topic             | Description                                      |
| ----------------- | ------------------------------------------------ |
| HTTP Verbs        | Determine the action (GET, POST, DELETE, etc.)   |
| Routes            | URI path to map to a controller method           |
| Attribute Routing | Makes route definitions clearer and explicit     |
| \[controller]     | Dynamic token for controller name                |
| Route Parameters  | Dynamic values in URI (e.g., `{id}`)             |
| Route Conflicts   | Avoid by using route constraints or custom paths |

---

## 📽️ What’s Next?

In the **next video**, we will deep dive into **CRUD operations** using our in-memory repository and apply routing to handle create, update, and delete actions.

---

## 🙏 Conclusion

You've now learned:

* How to use **attribute-based routing**
* How **verbs and URIs** work together to map requests
* How to **safely handle similar routes**
* Best practices for defining and naming your routes

---

### 🎬 For More Tutorials:

**Please Like, Share, and Subscribe!**
**Thank you!**

---

Would you like me to compile **Parts 1–9** into a single **PDF Web API tutorial guide** for easier revision?
Absolutely! Here's the **expanded version of Part 9** with **code-level explanations, real-world analogies**, and **why each line or block is used** — perfect for learners who want deep understanding.

---

# 📘 Web API Tutorials – Part 9: Routing in ASP.NET Core Web API

**Instructor:** Venkat
**Topic:** Deep dive into **routing** using **attribute-based routing** in controllers and actions

---

## 🧭 What is Routing?

### 🗺️ Simple Definition:

**Routing** is the mechanism that maps a **URL** + **HTTP verb** (like GET or POST) to a specific **method** in your controller.

---

## ✅ Real-World Analogy

Imagine:

* You’re placing a food order through a food delivery app.
* The **URI** is your address
* The **HTTP verb** (GET, POST, DELETE) is the **action** (like placing, cancelling, or viewing the order)
* The **controller method** is the kitchen deciding **what action to perform**

---

## 🔧 Basic Code Example

### Controller Setup

```csharp
[Route("api/[controller]")]
[ApiController]
public class StudentController : ControllerBase
```

### 🔍 What each part does:

| Code                          | Meaning                                                                        |
| ----------------------------- | ------------------------------------------------------------------------------ |
| `[Route("api/[controller]")]` | Sets the **base URI**. `[controller]` is replaced by `Student` automatically.  |
| `[ApiController]`             | Enables **automatic model validation**, routing, and helpful behavior for APIs |
| `ControllerBase`              | Gives access to `Ok()`, `NotFound()`, `BadRequest()`, etc. (Web API specific)  |

So the **base URL** for this controller is:

```
/api/student
```

---

## 📌 Key Concept: URI + HTTP Verb = Which Method Will Run

### 🧪 Example: Get All Students

```csharp
[HttpGet("all", Name = "GetAllStudents")]
public IEnumerable<Student> GetStudents()
```

| Aspect | Value                                              |
| ------ | -------------------------------------------------- |
| Verb   | GET                                                |
| Route  | `/api/student/all`                                 |
| Name   | Optional name for documentation or reverse routing |

✅ When the browser or Swagger calls `GET /api/student/all`, this method runs.

---

## 🧱 Example 2: Get Student by ID

```csharp
[HttpGet("{id:int}", Name = "GetStudentById")]
public ActionResult<Student> GetStudentById(int id)
```

### 🔍 Line-by-line explanation:

| Code                    | Purpose                                                                |
| ----------------------- | ---------------------------------------------------------------------- |
| `[HttpGet("{id:int}")]` | Route accepts an **integer** as a parameter                            |
| `{id:int}`              | Ensures only integer values match (prevents route confusion)           |
| `ActionResult<Student>` | Returns either a student object **or** a response like `404 Not Found` |

---

## 🚧 What If Two Methods Use Same Pattern?

```csharp
[HttpGet("{id}")]
public Student GetById(int id)

[HttpGet("{name}")]
public Student GetByName(string name)
```

### ❌ Problem:

Both have `{something}` — Web API **can’t tell** if you meant ID or Name.

### ✅ Solution:

Make routes distinct:

```csharp
[HttpGet("id/{id:int}")]
public Student GetById(int id)

[HttpGet("name/{name}")]
public Student GetByName(string name)
```

Now Web API knows:

* `/api/student/id/2` → ID route
* `/api/student/name/venkat` → Name route

---

## 🗑️ Delete Student by ID

```csharp
[HttpDelete("{id:int}", Name = "DeleteStudentById")]
public bool DeleteStudent(int id)
{
    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);
    if (student != null)
    {
        CollegeRepository.Students.Remove(student);
        return true;
    }
    return false;
}
```

### 🔍 What this does:

| Line                       | Meaning                                      |
| -------------------------- | -------------------------------------------- |
| `[HttpDelete("{id:int}")]` | Calls this method on `DELETE /api/student/1` |
| `FirstOrDefault(...)`      | Uses LINQ to find a student with matching ID |
| `Remove(student)`          | Removes the student from the in-memory list  |
| `return true/false`        | Indicates success or failure of deletion     |

---

## 💡 Route Patterns Recap

| Method                       | Route                      |
| ---------------------------- | -------------------------- |
| `GetStudents()`              | `/api/student/all`         |
| `GetStudentById(1)`          | `/api/student/1`           |
| `GetStudentByName("Venkat")` | `/api/student/name/venkat` |
| `DeleteStudent(1)`           | `DELETE /api/student/1`    |

---

## 📏 Route Constraint: `{id:int}`

This says:

* The value must be an **integer**
* If you pass `"abc"`, it won’t match

✅ Prevents accidental matches (like thinking `"abc"` is an ID)

---

## 🔄 Route Naming Approaches

### Option 1: `[Route]` + `[HttpVerb]`

```csharp
[Route("student/all")]
[HttpGet]
```

### Option 2: Inline Routing (Recommended)

```csharp
[HttpGet("all", Name = "GetAllStudents")]
```

📌 Both are valid — Option 2 is **cleaner and more common** in ASP.NET Core.

---

## 🧪 Testing Routes in Swagger or Postman

1. Open Swagger (usually `/swagger`)
2. Try calling:

   * `/api/student/all`
   * `/api/student/2`
   * `/api/student/name/Alice`
   * `DELETE /api/student/2`
3. Verify outputs (JSON results, status codes)

---

## ✅ Summary: Why Routing Matters

| Feature                  | Benefit                                                     |
| ------------------------ | ----------------------------------------------------------- |
| Attribute Routing        | Clear and visible mapping of URL to method                  |
| Route Constraints        | Avoid conflicts (like ID vs Name)                           |
| HTTP Verbs               | Support RESTful conventions (`GET`, `POST`, `DELETE`, etc.) |
| Custom Route Names       | Helpful for generating URLs or organizing documentation     |
| Separation by Verb + URI | Allows clean design (`GET` and `DELETE` on same URI)        |

---

## 🚀 What’s Next (Part 10 Preview)

In the next part, we’ll:

✅ Implement **CRUD operations**
✅ Use **POST** for Create, **PUT** for Update, **DELETE** for Delete
✅ Add input validation and model binding

---

## 🙏 Conclusion

You now know:

✅ What routing is
✅ How route patterns and HTTP verbs work together
✅ How to handle route parameters
✅ How to avoid routing conflicts
✅ Best practices for route naming

---

Would you like a compiled PDF guide of **Parts 1–9**, with all code + explanations formatted for offline study or sharing?
Sure! Here's a **concise summary** of **Part 9: Routing in ASP.NET Core Web API**:

---

## 📘 **Part 9 Summary – Routing in Web API**

### ✅ What is Routing?

* **Routing** maps a **URL + HTTP verb** (GET, POST, DELETE, etc.) to a specific **controller method**.

---

### 🔧 How It Works

* Routes are defined using **attribute routing** like `[HttpGet("all")]`.
* `[Route("api/[controller]")]` sets the base URL, replacing `[controller]` with the controller’s name (e.g., `student`).

---

### 🧩 Examples

| Method               | HTTP Verb | Route                     |
| -------------------- | --------- | ------------------------- |
| Get all students     | GET       | `/api/student/all`        |
| Get student by ID    | GET       | `/api/student/1`          |
| Get student by name  | GET       | `/api/student/name/alice` |
| Delete student by ID | DELETE    | `/api/student/1`          |

---

### 🧠 Key Concepts

* `HTTP verb + route` = unique method mapping
* `{id:int}` = **route constraint** (only integers allowed)
* Use distinct prefixes (`id/`, `name/`) to avoid **route conflicts**
* `ActionResult<T>` lets you return data and status codes (`200 OK`, `404 Not Found`)

---

### 🧪 Testing

* Use **Swagger or Postman** to test each route by sending the correct verb + URI.

---

### ✅ Best Practices

* Keep routes **clear and RESTful**
* Use **route constraints** to avoid ambiguity
* Group and name routes logically for easier API management

---

Would you like this summary as part of a **print-ready PDF** along with all previous parts?
