Here is the **fully formatted and professional documentation** for **Part 9 – Advanced Routing and Constraints in Web API**, based entirely on your transcript. Every original word and idea is preserved — only structured and presented in a clean, understandable tutorial format.

---

# 📘 Web API Tutorials – Part 9: Advanced Routing & Constraints in Web API

**Instructor:** Venkat
**Topic:** How to define and manage **routes** and apply **constraints** in ASP.NET Web API

---

## 📌 Recap

In the previous routing tutorial, we covered:

* How **HTTP verb + route path** defines which method is triggered
* Defining route templates using attributes
* How similar route paths but different verbs (GET, DELETE) are valid

---

## 🧭 Problem with Similar Routes

Let’s consider:

```csharp
[HttpGet("{id}")]
public Student GetById(int id)

[HttpGet("{name}")]
public Student GetByName(string name)
```

Both have the same route pattern (`api/student/{value}`), but differ **only in the parameter type** (`int` vs `string`).

### ⚠️ Issue:

When you test with **Postman** or **Swagger**, using either `/api/student/1` or `/api/student/venkat`:

> ❌ The API throws an error:
> **"The request matched multiple endpoints."**

Why? Because **Web API cannot differentiate between the two endpoints** just by the route — it needs **routing constraints**.

---

## 🛡️ Solution: Use Route Constraints

We fix this using **explicit data type constraints** in the route:

### ✅ Modified Actions

```csharp
[HttpGet("{id:int}", Name = "GetStudentById")]
public Student GetById(int id)

[HttpGet("name/{name:alpha}", Name = "GetStudentByName")]
public Student GetByName(string name)
```

* `{id:int}` → Only integers will match
* `{name:alpha}` → Only alphabetical strings will match

### 🔄 Result:

* `/api/student/1` calls `GetById`
* `/api/student/name/venkat` calls `GetByName`

✅ **Resolved ambiguity**

---

## 📜 Common Route Constraints in ASP.NET Web API

| Constraint     | Purpose                        | Example                |
| -------------- | ------------------------------ | ---------------------- |
| `int`          | Restrict to integer            | `{id:int}`             |
| `alpha`        | Restrict to letters (a–z, A–Z) | `{name:alpha}`         |
| `min(x)`       | Minimum value                  | `{age:min(1)}`         |
| `max(x)`       | Maximum value                  | `{age:max(100)}`       |
| `range(x,y)`   | Value must be between x and y  | `{age:range(0,100)}`   |
| `length(x)`    | Exact string length            | `{code:length(5)}`     |
| `maxlength(x)` | Max string length              | `{name:maxlength(10)}` |
| `minlength(x)` | Min string length              | `{name:minlength(3)}`  |
| `bool`         | Must be true/false             | `{flag:bool}`          |
| `datetime`     | Must be a valid date           | `{startDate:datetime}` |
| `guid`         | Must be a valid GUID           | `{id:guid}`            |

> ℹ️ You can also create **custom constraints** if built-in ones are not sufficient.

---

## 🧪 Testing in Swagger

### 1. **Get All Students**

* `/api/student/all` → Lists all students

### 2. **Get by ID**

* `/api/student/1` → Gets student with ID 1
* ✅ Valid because of `:int` constraint

### 3. **Get by Name**

* `/api/student/name/venkat`
* ✅ Valid due to `:alpha` constraint
* Returns student with name "venkat"

### 4. **Delete by ID**

```csharp
[HttpDelete("{id:int}", Name = "DeleteStudentById")]
public bool DeleteStudent(int id)
```

* `/api/student/1` → Deletes student with ID 1
* Returns: `true` if successful

---

## 🧹 Example Cleanup and Testing Flow

1. Call **GET All** → Returns two student records
2. Call **DELETE /api/student/1** → Deletes the first record
3. Call **GET All** again → Returns only one student

✅ All endpoints are working as expected after applying constraints.

---

## 🧠 Summary: Routing Constraints Improve Precision

| Problem                            | Solution                              |
| ---------------------------------- | ------------------------------------- |
| Similar routes causing ambiguity   | Use route constraints                 |
| Routes not matching correct method | Use explicit verbs + typed parameters |
| Want to restrict input range       | Use `range(min,max)` constraint       |

---

## 🧬 Advanced Tip

If you want to use both **routing and HTTP verb attributes together**, you can do this:

```csharp
[HttpGet("name/{name:alpha}", Name = "GetStudentByName")]
```

Or separately:

```csharp
[Route("name/{name:alpha}", Name = "GetStudentByName")]
[HttpGet]
```

Both are valid. Use what suits your style and consistency.

---

## ✅ Best Practices for Routing

* Always use **explicit parameter types** for dynamic route segments
* Use meaningful route paths like `/all`, `/name/{name}`, `/id/{id}`
* Avoid overloading with ambiguous routes
* Group routes by functionality and keep consistent naming

---

## 📽️ What’s Next?

In the next video, we will:

* Expand CRUD operations (Create, Update, Delete)
* Prepare for integration with **Entity Framework** (moving beyond in-memory repository)

---

## 🙏 Conclusion

You've now learned:

* How **Web API routing** works under the hood
* What causes **conflicts**
* How to use **constraints** to prevent ambiguity
* How to **safely and precisely map requests to actions**

---

### 🎬 For More Tutorials:

**Please Like, Share, and Subscribe!**
**Thank you!**

---

Would you like me to **compile all Parts 1–9 into a single downloadable PDF** for easy revision and sharing?
Absolutely! Here's a **detailed explanation** of **Part 9: Advanced Routing & Constraints in Web API** — with **code-by-code explanations, examples, reasons behind each concept**, and a **summary** at the end.

---

# 📘 Part 9: Advanced Routing & Constraints in ASP.NET Core Web API

**Instructor:** Venkat
**Topic:** Understand how **routing** works and how to use **constraints** to avoid conflicts and ensure precision in Web API

---

## 🔁 Recap of Routing

In Web API, routing maps a **URL + HTTP verb (GET, POST, etc.)** to a specific method in a controller.

```csharp
[HttpGet("{id}")]
public Student GetById(int id)
```

👆 Means this method responds to:

```
GET /api/student/1
```

But what happens if we write:

```csharp
[HttpGet("{name}")]
public Student GetByName(string name)
```

Now both methods look similar:

```
GET /api/student/1
GET /api/student/venkat
```

❌ **Problem**: Web API gets confused — “Is 1 a name or ID?”

---

## 🚨 Problem: Ambiguous Routes

### Code Example:

```csharp
[HttpGet("{id}")]
public Student GetById(int id)

[HttpGet("{name}")]
public Student GetByName(string name)
```

### ❌ Error:

> `"The request matched multiple endpoints."`

Because Web API sees both routes as `/api/student/{something}` — and can’t decide which method to call.

---

## ✅ Solution: Add Route Constraints

### Code (Fixed Version):

```csharp
[HttpGet("{id:int}", Name = "GetStudentById")]
public Student GetById(int id)

[HttpGet("name/{name:alpha}", Name = "GetStudentByName")]
public Student GetByName(string name)
```

### ✅ How it works:

* `{id:int}` → Only matches **numbers**
* `{name:alpha}` → Only matches **letters**

### 🔍 Examples:

| Request                    | Routed to   |
| -------------------------- | ----------- |
| `/api/student/1`           | `GetById`   |
| `/api/student/name/venkat` | `GetByName` |

---

## 🧠 Why Constraints Are Important

Without them:

* Routes can overlap
* API may break or behave unpredictably
* You can't safely map GET, POST, DELETE for the same path (`/api/student/1`)

With them:

* Each route is **precise**
* **Swagger** can document and test them separately
* Easy to debug and maintain

---

## 📜 Built-in Route Constraints

Here are commonly used route constraints:

| Constraint   | What It Does                  | Example                |
| ------------ | ----------------------------- | ---------------------- |
| `int`        | Allows only integers          | `{id:int}`             |
| `alpha`      | Allows only alphabet letters  | `{name:alpha}`         |
| `min(x)`     | Value must be ≥ x             | `{age:min(18)}`        |
| `max(x)`     | Value must be ≤ x             | `{age:max(65)}`        |
| `range(x,y)` | Value must be between x and y | `{score:range(0,100)}` |
| `length(x)`  | Must be exactly x characters  | `{code:length(6)}`     |
| `guid`       | Must be a valid GUID          | `{id:guid}`            |
| `datetime`   | Must be a valid date          | `{startDate:datetime}` |

---

## 🧪 Testing Routes in Swagger

### Example Endpoints:

1. **GET All Students** → `/api/student/all`
2. **GET by ID** → `/api/student/1`

   * ✅ Works only because of `:int` constraint
3. **GET by Name** → `/api/student/name/venkat`

   * ✅ Works due to `:alpha` constraint
4. **DELETE by ID** → `DELETE /api/student/1`

---

## 🧱 Full Example: Delete Student

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

### 🔍 What This Does:

* Allows DELETE requests like `/api/student/3`
* Checks if a student exists
* Deletes and returns `true`
* If not found, returns `false`

---

## 🧪 Testing Flow (Live Use Case)

1. Call `GET /api/student/all`
   ✅ Returns 2 students

2. Call `DELETE /api/student/1`
   ✅ Removes the first student

3. Call `GET /api/student/all` again
   ✅ Now returns only 1 student

---

## 🧬 Bonus: Inline Routing vs Separate Attributes

### ✅ Both of these are valid:

**Inline style:**

```csharp
[HttpGet("name/{name:alpha}", Name = "GetStudentByName")]
```

**Separate style:**

```csharp
[Route("name/{name:alpha}", Name = "GetStudentByName")]
[HttpGet]
```

Use the style that matches your **project standards or team preference**.

---

## 🧼 Best Practices

✅ Use clear and **distinct route patterns**
✅ Always add **type constraints** to dynamic segments
✅ Avoid using the same route structure for multiple methods
✅ Use meaningful names like `name/`, `id/`, `all/` to guide usage
✅ Keep route paths **consistent** and **RESTful**

---

## 🧠 Summary

| ✅ You Learned                   | 🔍 What It Means                        |
| ------------------------------- | --------------------------------------- |
| How routing works               | Maps URL + verb to method               |
| What causes route conflicts     | When paths look the same (`{param}`)    |
| How to fix it                   | Add constraints like `:int`, `:alpha`   |
| Built-in route constraints      | Allow type checking and clean routing   |
| How to delete and test students | Using `DELETE` with `id:int` constraint |

---

## 🎬 Coming Next

In **Part 10**, you’ll learn:

✅ How to perform **Create, Update, Delete** operations
✅ How to use **POST**, **PUT**, and **DELETE** effectively
✅ How to prepare for database integration using **Entity Framework**

---

Would you like me to create a **printable PDF guide for Parts 1–9** with code, screenshots, and practical testing tips?
