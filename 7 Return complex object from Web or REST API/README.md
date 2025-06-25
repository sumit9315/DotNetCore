Here is the **formatted and professional documentation** for **Part 7: Returning Complex Objects from a Web API Endpoint**. The content is fully preserved — only structured formatting and section titles have been added for clarity and better presentation.

---

# 📘 VBPI Tutorials – Part 7: Returning Complex Objects from a Web API Endpoint

**Instructor:** Venkat
**Topic:** How to return a **complex object** (single class or a list of classes) from an API endpoint

---

## 📌 Introduction

Welcome back to **VBPI Tutorials**, I’m **Venkat**, and this is **Part 7**.

In the **previous video**, we saw how to return a **simple string** from an action method or endpoint.

In this video, we will learn:

* ✅ How to return a **complex object**
* ✅ How to return a **list of objects**
* ✅ How to structure the code using a **model class**
* ✅ A sneak peek into using a **repository**

---

## 🛠️ Step 1: Clean Up Default Controllers

We had two controllers previously.
The default `ValuesController` is **no longer required**, so it can be **removed**.

We’ll continue with the existing `StudentController`.

---

## 🏗️ Step 2: Modify Action Method to Return a List

Update your method inside `StudentController` to:

```csharp
public IEnumerable<Student> GetStudents()
```

---

## 📁 Step 3: Create the Student Model

1. Add a new folder called `Models`
2. Inside it, add a new class called `Student.cs`

### `Student.cs`:

```csharp
public class Student
{
    public int StudentId { get; set; }
    public string StudentName { get; set; }
    public string Email { get; set; }
    public string Address { get; set; }
}
```

✅ This class will represent a **complex object** structure returned from the API.

---

## ✏️ Step 4: Return a List of Students

In `StudentController.cs`, import the `Models` namespace and update your method to return data like:

```csharp
using YourApp.Models; // Replace with your actual namespace

[HttpGet]
public IEnumerable<Student> GetStudents()
{
    return new List<Student>
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
}
```

📌 The endpoint now returns a **list of complex Student objects**.

---

## 🧪 Step 5: Test in Swagger UI

1. Run the application
2. Open **Swagger UI**
3. Find the `GET /api/student` endpoint
4. Click `Try it out` → `Execute`

### ✅ Sample Output:

```json
[
  {
    "studentId": 1,
    "studentName": "Student 1",
    "email": "email1@gmail.com",
    "address": "Hyderabad, India"
  },
  {
    "studentId": 2,
    "studentName": "Student 2",
    "email": "email2@gmail.com",
    "address": "Bangalore, India"
  }
]
```

---

## 🧼 Step 6: Code Clean-Up (Next Step Preview)

Currently, we’re creating the **student list directly inside the controller**, which:

* ❌ Makes the controller bulky
* ❌ Is not scalable
* ❌ Violates clean code practices

### ✅ Recommended Next Step:

Move this logic into a **static repository class** to:

* Separate concerns
* Improve maintainability
* Follow a cleaner architecture

---

## ✅ Summary

In this part, you’ve learned how to:

* Return a **complex object** using Web API
* Use a **Model class** (`Student.cs`)
* Return a **list of objects** instead of a static string
* View structured JSON response in **Swagger**

---

## 🎬 What’s Next?

In the next video, we will:

* Create a **static repository**
* Move student list logic into the repository
* Call the repository from the controller

---

## 🙏 Conclusion

For more videos like this,
**Please Like, Share, and Subscribe!**
**Thank you!**

---

Would you like a full **PDF version** of this entire tutorial series from Part 1 to 7?
