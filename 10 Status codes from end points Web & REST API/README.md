Here is a **well-structured, professional documentation** for **Part 10: Responding with Status Codes in Web API**, based completely on your transcript. All content is preserved — only structured and enhanced for better clarity and learning.

---

# 📘 VBPI Tutorials – Part 10: Responding with Status Codes in Web API

**Instructor:** Venkat
**Topic:** How to return **standard HTTP status codes** from Web API actions

---

## 📌 Objective

In this tutorial, we will learn how to:

* Use **`ActionResult<T>`** to standardize Web API responses
* Return different HTTP **status codes** like `200 OK`, `400 BadRequest`, `404 NotFound`, etc.
* Improve API clarity by sending meaningful responses

---

## 🧾 Why Use ActionResult?

Returning just an object (like `Student`) from a controller is simple but **not complete**.

To follow RESTful conventions, we need to:

* Return both **data** and **status codes**
* Use `ActionResult<T>` so we can return:

  * ✅ 200 OK
  * ❌ 400 BadRequest
  * ❌ 404 NotFound
  * And others

---

## 🏗️ Step-by-Step Code Refactor

### 1️⃣ Returning List of Students

```csharp
[HttpGet("all")]
public ActionResult<IEnumerable<Student>> GetStudents()
{
    return Ok(CollegeRepository.Students); // 200 OK
}
```

---

### 2️⃣ Get Student by ID with Validation

```csharp
[HttpGet("{id:int}")]
public ActionResult<Student> GetStudentById(int id)
{
    if (id <= 0)
        return BadRequest("Student ID must be a positive number."); // 400 Bad Request

    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);

    if (student == null)
        return NotFound($"Student with ID {id} not found."); // 404 Not Found

    return Ok(student); // 200 OK
}
```

---

### 3️⃣ Get Student by Name with Validation

```csharp
[HttpGet("name/{name:alpha}")]
public ActionResult<Student> GetStudentByName(string name)
{
    if (string.IsNullOrWhiteSpace(name))
        return BadRequest("Name is required."); // 400 Bad Request

    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentName == name);

    if (student == null)
        return NotFound($"Student with name '{name}' not found."); // 404 Not Found

    return Ok(student); // 200 OK
}
```

---

### 4️⃣ Delete Student by ID

```csharp
[HttpDelete("{id:int}")]
public ActionResult<bool> DeleteStudentById(int id)
{
    if (id <= 0)
        return BadRequest("Student ID must be positive."); // 400 Bad Request

    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);

    if (student == null)
        return NotFound($"Student with ID {id} not found."); // 404 Not Found

    CollegeRepository.Students.Remove(student);
    return Ok(true); // 200 OK with true
}
```

---

## ✅ HTTP Status Code Summary

| Method                | Condition                               | Status Code      | Explanation                |
| --------------------- | --------------------------------------- | ---------------- | -------------------------- |
| `Ok(data)`            | Success                                 | `200 OK`         | Data returned successfully |
| `BadRequest(message)` | Input invalid (e.g. `id <= 0`)          | `400 BadRequest` | Client sent bad data       |
| `NotFound(message)`   | Data not found (e.g. student not found) | `404 Not Found`  | Resource does not exist    |

---

## 🧠 Why This Is Important

* **Improves API design**: Clear, correct responses
* **Helps clients**: Know exactly what went wrong or succeeded
* **Encourages RESTful standards**: Every action clearly maps to an HTTP status

---

## 🧪 Testing Scenarios in Swagger/Postman

| Endpoint                       | Test Case       | Expected Status    |
| ------------------------------ | --------------- | ------------------ |
| `GET /api/student/1`           | Valid ID        | ✅ `200 OK`         |
| `GET /api/student/0`           | Invalid ID      | ❌ `400 BadRequest` |
| `GET /api/student/5`           | ID not found    | ❌ `404 NotFound`   |
| `GET /api/student/name/venkat` | Valid Name      | ✅ `200 OK`         |
| `DELETE /api/student/1`        | Existing ID     | ✅ `200 OK`         |
| `DELETE /api/student/9`        | Non-existent ID | ❌ `404 NotFound`   |

---

## 🧾 Summary of Key Concepts

| Concept               | Use                            |
| --------------------- | ------------------------------ |
| `ActionResult<T>`     | Enables flexible return types  |
| `Ok(data)`            | Returns 200 with data          |
| `BadRequest(message)` | Returns 400 with error message |
| `NotFound(message)`   | Returns 404 with message       |

---

## 🙏 Conclusion

You’ve now learned:

* How to respond with different **HTTP status codes**
* How to handle **invalid input** and **missing data**
* How to return **meaningful messages** with your responses

---

### 🎬 For More Tutorials:

**Please Like, Share, and Subscribe!**
**Thank you!**

---

Would you like a full **PDF version of all tutorials (Parts 1–10)** to revise or share easily?
Absolutely! Here's a more **detailed, line-by-line explanation** of **Part 10: Responding with Status Codes in Web API**, including *why* each piece of code is used, how it improves your API, and a clear **summary at the end**.

---

# 📘 Part 10: Responding with HTTP Status Codes in ASP.NET Core Web API

**Instructor:** Venkat
**Topic:** How to respond from your API methods with proper **HTTP status codes** to indicate success, failure, or errors.

---

## 🎯 Why Is This Important?

In real-world APIs:

* Just returning **data** (like a `Student` object) isn't enough.
* You must return the correct **status code** (like 200, 400, 404) so the **frontend or client** understands what happened.

### 🔍 Example:

| Situation         | Bad Practice      | Good Practice               |
| ----------------- | ----------------- | --------------------------- |
| Student not found | Return `null`     | Return `404 Not Found`      |
| Invalid input     | Return empty list | Return `400 Bad Request`    |
| Success           | Return object     | Return `200 OK` with object |

---

## 🧾 Why Use `ActionResult<T>`?

Instead of returning just `Student`, use:

```csharp
ActionResult<Student>
```

This lets you return:

* ✅ `Ok(student)` → 200
* ❌ `BadRequest("message")` → 400
* ❌ `NotFound("message")` → 404

✅ Now your API is **self-explanatory** for clients.

---

## 🏗️ Step-by-Step Code Walkthrough with Explanations

---

### 1️⃣ **Get All Students** – Returns 200 OK with a list

```csharp
[HttpGet("all")]
public ActionResult<IEnumerable<Student>> GetStudents()
{
    return Ok(CollegeRepository.Students); // Always succeeds, so return 200 OK
}
```

#### 🔍 Why:

* `ActionResult<IEnumerable<Student>>` gives flexibility
* `Ok()` wraps the data with a **200 OK status**

---

### 2️⃣ **Get Student by ID** – With Input Validation

```csharp
[HttpGet("{id:int}")]
public ActionResult<Student> GetStudentById(int id)
{
    if (id <= 0)
        return BadRequest("Student ID must be a positive number."); // Invalid input = 400

    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);

    if (student == null)
        return NotFound($"Student with ID {id} not found."); // ID not found = 404

    return Ok(student); // Found = 200 OK
}
```

#### 🔍 What it does:

* `id <= 0` is **invalid input** → returns 400
* If no student is found → 404
* Otherwise → returns student with 200 OK

---

### 3️⃣ **Get Student by Name** – With Name Validation

```csharp
[HttpGet("name/{name:alpha}")]
public ActionResult<Student> GetStudentByName(string name)
{
    if (string.IsNullOrWhiteSpace(name))
        return BadRequest("Name is required."); // Name is empty = 400

    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentName == name);

    if (student == null)
        return NotFound($"Student with name '{name}' not found."); // Not found = 404

    return Ok(student); // Found = 200 OK
}
```

#### 🔍 Why:

* Checks if input name is valid
* Checks if the student exists
* Returns appropriate code and message

---

### 4️⃣ **Delete Student by ID** – With Confirmation

```csharp
[HttpDelete("{id:int}")]
public ActionResult<bool> DeleteStudentById(int id)
{
    if (id <= 0)
        return BadRequest("Student ID must be positive."); // 400 for invalid input

    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);

    if (student == null)
        return NotFound($"Student with ID {id} not found."); // 404 if not found

    CollegeRepository.Students.Remove(student);
    return Ok(true); // Successfully deleted = 200 OK with true
}
```

#### 🔍 Highlights:

* Returns **boolean success**
* Validates input and data existence
* Cleanly communicates what happened

---

## ✅ HTTP Status Code Reference

| Code             | Method                  | When Triggered                |
| ---------------- | ----------------------- | ----------------------------- |
| `200 OK`         | `Ok(data)`              | When everything is successful |
| `400 BadRequest` | `BadRequest("message")` | Input is invalid or missing   |
| `404 NotFound`   | `NotFound("message")`   | Record does not exist         |

---

## 🧪 Testing These in Swagger / Postman

| Endpoint                       | Test Case        | Expected Result  |
| ------------------------------ | ---------------- | ---------------- |
| `GET /api/student/1`           | Valid ID         | ✅ 200 OK         |
| `GET /api/student/0`           | Invalid ID       | ❌ 400 BadRequest |
| `GET /api/student/999`         | Not in list      | ❌ 404 NotFound   |
| `GET /api/student/name/venkat` | Valid Name       | ✅ 200 OK         |
| `DELETE /api/student/1`        | Existing student | ✅ 200 OK         |
| `DELETE /api/student/9`        | Not in list      | ❌ 404 NotFound   |

---

## 🧠 Why This Makes Your API Professional

| Feature                  | Benefit                                       |
| ------------------------ | --------------------------------------------- |
| Returns clear HTTP codes | Makes it easy for clients to handle responses |
| Validates input          | Prevents bad data from being processed        |
| Returns helpful messages | Easier debugging and better UX                |
| RESTful standard         | Follows industry best practices               |

---

## 🧾 Summary of Key Concepts

| Concept           | Purpose                                 |
| ----------------- | --------------------------------------- |
| `ActionResult<T>` | Allows combining data with status codes |
| `Ok(data)`        | Returns 200 with data                   |
| `BadRequest()`    | Returns 400 for invalid input           |
| `NotFound()`      | Returns 404 if record not found         |

---

## 🙏 Conclusion

By using `ActionResult<T>`, you now know how to:

✅ Send correct HTTP status codes
✅ Validate inputs before processing
✅ Return helpful error messages
✅ Make your API professional and user-friendly

---

### 🎬 Coming Up Next

In **Part 11**, you’ll learn how to:

✅ Accept input from the **request body** using `[FromBody]`
✅ Create a **POST endpoint** for adding new students
✅ Add input model validation

---

Would you like a **compiled PDF guide of Parts 1–10** with code, examples, and explanation for each topic?
Great question! Let's break down the return type step by step to understand:

---

## 🧠 What does this mean?

```csharp
public ActionResult<IEnumerable<Student>> GetStudents()
```

This line says:

* The method returns an **HTTP response** (`ActionResult`)
* That response may contain a **collection of students** (`IEnumerable<Student>`)
* This allows returning:

  * ✅ `200 OK` with student list
  * ❌ `400 BadRequest`
  * ❌ `500 InternalServerError` (if needed later)
  * ❌ `204 NoContent`, etc.

---

## 🔍 Why not just `IEnumerable<Student>`?

If we had written:

```csharp
public IEnumerable<Student> GetStudents()
```

This would **only return the data**, like:

```json
[
  { "studentId": 1, "studentName": "Alice" },
  { "studentId": 2, "studentName": "Bob" }
]
```

But you **cannot return status codes like**:

* `400 Bad Request`
* `404 Not Found`
* `500 Internal Server Error`

And if something goes wrong, you're stuck — you can only return an empty list.

---

## 🔍 Why not just `ActionResult`?

This would work too:

```csharp
public ActionResult GetStudents()
```

But then you'd lose the **type information** (`Student`). Swagger and API consumers won’t know that the response contains a **list of students**.

---

## ✅ Best of Both: `ActionResult<IEnumerable<Student>>`

This gives you **the best of both worlds**:

| Feature                                        | Supported? |
| ---------------------------------------------- | ---------- |
| Return `List<Student>`                         | ✅ Yes      |
| Return HTTP status codes (`200`, `400`, `404`) | ✅ Yes      |
| Let Swagger show correct return type           | ✅ Yes      |
| Enable flexible API responses                  | ✅ Yes      |

---

## 🧪 Example Return Scenarios with `ActionResult<IEnumerable<Student>>`

### ✅ Successful response:

```csharp
return Ok(CollegeRepository.Students); // returns 200 OK with data
```

### ❌ Failure response (optional later):

```csharp
if (CollegeRepository.Students == null)
    return NotFound("No students found."); // returns 404
```

You **can't do this** if you return only `IEnumerable<Student>`.

---

## 🧠 Summary

| Return Type                          | What It Does                                   | Limitation                |
| ------------------------------------ | ---------------------------------------------- | ------------------------- |
| `IEnumerable<Student>`               | Just returns data                              | Can’t return status codes |
| `ActionResult`                       | Can return any status                          | Doesn't specify data type |
| `ActionResult<IEnumerable<Student>>` | ✅ Returns data + status codes + full type info | Best choice for real APIs |

---

### ✅ Conclusion

We use:

```csharp
ActionResult<IEnumerable<Student>>
```

because it lets us return both:

* A list of students (`IEnumerable<Student>`)
* A proper HTTP response (via `ActionResult`)

This makes your API **RESTful, testable, and client-friendly**.

Would you like similar detailed explanations for `ActionResult<Student>`, `ActionResult<bool>`, or `FromBody` in POST requests?
Excellent follow-up!

Let’s break down what would happen if you used **`ActionResult<Student>`** in this line:

```csharp
[HttpGet("all")]
public ActionResult<Student> GetStudents()
```

---

## ❌ Why This Would Be Incorrect

You’re trying to **return a list of students**, like this:

```json
[
  { "studentId": 1, "studentName": "Alice" },
  { "studentId": 2, "studentName": "Bob" }
]
```

But you're telling the compiler:

> "I will return a **single `Student` object** (not a list)."

So if you write:

```csharp
return Ok(CollegeRepository.Students); // This is a List<Student>
```

### ❌ What happens?

* ⚠️ **Compilation error** — because you’re returning `IEnumerable<Student>` but saying you'll return `Student`
* Or worse: the client gets confused if the response format mismatches expectations

---

## 🧠 Why This Happens

Let’s compare:

| Return Type                          | What It's Meant For           | Example Return Value                         |
| ------------------------------------ | ----------------------------- | -------------------------------------------- |
| `ActionResult<Student>`              | Return a **single student**   | `{ "studentId": 1, "studentName": "Alice" }` |
| `ActionResult<IEnumerable<Student>>` | Return a **list of students** | `[ { "studentId": 1 }, { "studentId": 2 } ]` |

So if you return a list but say you'll return a single object — it breaks the contract.

---

## ✅ When Should You Use `ActionResult<Student>`?

Use this when you're returning a **single student**, like:

```csharp
[HttpGet("{id:int}")]
public ActionResult<Student> GetStudentById(int id)
{
    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);
    
    if (student == null)
        return NotFound();

    return Ok(student);
}
```

✅ Works because you are returning **one student**, not many.

---

## 🧪 What Would Go Wrong in Swagger?

If you used this:

```csharp
[HttpGet("all")]
public ActionResult<Student> GetStudents()
```

But returned a **list**, then:

* **Swagger would show**: response = `Student`
* **API would return**: `List<Student>`
* ❌ The mismatch would confuse developers using your API

---

## ✅ Final Summary

| Situation                             | Correct Return Type                  |
| ------------------------------------- | ------------------------------------ |
| You return a **single student**       | `ActionResult<Student>`              |
| You return a **list of students**     | `ActionResult<IEnumerable<Student>>` |
| You return **nothing or status only** | `ActionResult` or `IActionResult`    |

---

### ✅ Correct Usage in Your Case:

```csharp
[HttpGet("all")]
public ActionResult<IEnumerable<Student>> GetStudents()
```

You are returning a **list**, so you must wrap it in `IEnumerable<Student>`, and use `ActionResult` to support status codes like `200 OK`, `404`, etc.

Let me know if you'd like visual examples in Swagger or Postman for each return type!
Awesome! Let’s now visualize and compare how Swagger and Postman behave when using:

1. ✅ `ActionResult<IEnumerable<Student>>` — **Correct for list**
2. ❌ `ActionResult<Student>` — **Incorrect for list**
3. ✅ `ActionResult<Student>` — **Correct for single item**

---

## 🔍 Scenario 1: ✅ Returning a List (Correct)

### Code:

```csharp
[HttpGet("all")]
public ActionResult<IEnumerable<Student>> GetStudents()
{
    return Ok(CollegeRepository.Students);
}
```

### 🧪 Swagger Behavior:

* Shows response schema as:

  ```json
  [
    {
      "studentId": 1,
      "studentName": "Alice",
      "email": "alice@email.com",
      "address": "Hyderabad"
    },
    {
      "studentId": 2,
      "studentName": "Bob",
      "email": "bob@email.com",
      "address": "Bangalore"
    }
  ]
  ```
* Clearly shows: **array of students**
* Status: ✅ `200 OK`

### 📬 Postman Response:

Same JSON array above
Header: `200 OK`
Content-Type: `application/json`

✅ No issues at all.

---

## 🔍 Scenario 2: ❌ Returning a List but Declaring a Single Student

### Code:

```csharp
[HttpGet("all")]
public ActionResult<Student> GetStudents()
{
    return Ok(CollegeRepository.Students); // returns List<Student>
}
```

### 🧪 Swagger Behavior:

* ❌ Will show this as the expected schema:

  ```json
  {
    "studentId": 1,
    "studentName": "..."
  }
  ```
* But your method returns:

  ```json
  [
    { "studentId": 1 }, { "studentId": 2 }
  ]
  ```

### ⚠️ Problem:

* Schema mismatch
* Swagger shows a **single object**, but response is an **array**
* Clients may break when trying to parse response

### 📬 Postman Response:

* Still returns array (because C# doesn’t crash at runtime here)
* But API consumers will **assume wrong structure** if they rely on Swagger or OpenAPI

---

## 🔍 Scenario 3: ✅ Returning a Single Student (Correct)

### Code:

```csharp
[HttpGet("{id:int}")]
public ActionResult<Student> GetStudentById(int id)
{
    var student = CollegeRepository.Students.FirstOrDefault(s => s.StudentId == id);

    if (student == null)
        return NotFound();

    return Ok(student);
}
```

### 🧪 Swagger Behavior:

* Shows:

  ```json
  {
    "studentId": 1,
    "studentName": "Alice",
    "email": "...",
    "address": "..."
  }
  ```
* Correct schema for a **single object**
* Status: ✅ `200 OK` or ❌ `404 Not Found`

### 📬 Postman:

* Returns:

  ```json
  {
    "studentId": 1,
    "studentName": "Alice",
    ...
  }
  ```
* Header: `200 OK`
* Or:

  ```json
  {
    "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    "title": "Not Found",
    "status": 404,
    "traceId": "..."
  }
  ```

---

## ✅ Summary Table

| Scenario                           | Return Type                          | Response Shape | Swagger Output | Recommended |
| ---------------------------------- | ------------------------------------ | -------------- | -------------- | ----------- |
| Return list of students            | `ActionResult<IEnumerable<Student>>` | ✅ Array        | ✅ Correct      | ✅ Yes       |
| Return list, but type is `Student` | `ActionResult<Student>`              | ❌ Mismatch     | ❌ Wrong schema | ❌ No        |
| Return single student by ID        | `ActionResult<Student>`              | ✅ Object       | ✅ Correct      | ✅ Yes       |

---

Would you like a **PDF version with screenshots of Swagger and Postman for each case**, or a small working **demo project** you can test locally?
Great question again — and it’s an important distinction!

Let’s clarify what happens if you write:

---

## ❌ `ActionResult<Students>` — Why This is Incorrect

### Let's look at what you're doing:

```csharp
[HttpGet("all")]
public ActionResult<Students> GetStudents()
{
    return Ok(CollegeRepository.Students);
}
```

### 🔍 The problem here:

* `Students` is **not a valid type** unless you explicitly define it as a **custom class** (like `public class Students { ... }`)
* But in most cases, `Students` is a **mistaken plural form** of `Student`

So unless you wrote:

```csharp
public class Students
{
    public List<Student> StudentList { get; set; }
}
```

Then this will cause a **compilation error**:

```
The type or namespace name 'Students' could not be found...
```

---

## ✅ What You're Probably Trying to Do

You likely meant to return a **list of student objects**, which is correctly done as:

```csharp
public ActionResult<IEnumerable<Student>> GetStudents()
```

or:

```csharp
public ActionResult<List<Student>> GetStudents()
```

Both are ✅ valid because:

* `IEnumerable<Student>` is a **collection of Student**
* Swagger understands this as a **JSON array of objects**

---

## 🔁 If You Really Want to Use `Students` as a Custom Class

You would need to define it like this:

```csharp
public class Students
{
    public List<Student> StudentList { get; set; }
}
```

Then return:

```csharp
[HttpGet("all")]
public ActionResult<Students> GetStudents()
{
    return Ok(new Students { StudentList = CollegeRepository.Students });
}
```

### 📦 JSON Output:

```json
{
  "studentList": [
    {
      "studentId": 1,
      "studentName": "Alice"
    },
    {
      "studentId": 2,
      "studentName": "Bob"
    }
  ]
}
```

But this is **not common** unless you need to wrap the list with metadata (like count, status, etc.)

---

## ✅ Summary

| Return Type                          | Result                                 | Recommended?                     |
| ------------------------------------ | -------------------------------------- | -------------------------------- |
| `ActionResult<Students>`             | ❌ Invalid unless `Students` is defined | ❌ No                             |
| `ActionResult<Student>`              | ✅ Valid for single object              | ✅ Yes (when returning 1 student) |
| `ActionResult<IEnumerable<Student>>` | ✅ Valid for list/array                 | ✅ Yes (recommended)              |

---

### ✅ Final Answer:

Use:

```csharp
public ActionResult<IEnumerable<Student>> GetStudents()
```

Unless you’ve intentionally created a `Students` class to wrap the list — and have a good reason for it.

Would you like an example of when wrapping a list inside a `Students` class is actually helpful (e.g., with pagination)?
