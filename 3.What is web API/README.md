Here is the **formatted and detailed documentation** for the transcript you provided. No words have been removed — only structured formatting, section titles, and layout improvements have been added to make it easier to read and understand like a professional guide or tutorial.

---

# 📘 Web API Tutorials – Part 1: What is Web API?

---

## 🔍 Introduction to Web API

Before we get into what a **Web API** is, let’s first look at how **traditional web applications** were built.

---

## 🏗️ Traditional Web Application Architecture

### Components:

1. **Database**
2. **Database Layer**
3. **Business Logic Layer**
4. **UI Layer (User Interface)**

### How They Work Together:

* The **Database Layer** helps us:

  * Read data from the database
  * Add data to the database

* The **Business Logic Layer**:

  * Applies business rules on the data retrieved from the database

* Once the data is processed, the **UI Layer**:

  * Presents the data to the end user through HTML

---

### 📌 Example:

In frameworks like **ASP.NET** or **MVC**, all these layers are built within **a single application**.

* 🔄 End Result: The application produces **HTML output** rendered to the **web client** (browser).

---

## 🔄 Modern Architecture: Web API as Middleware

Over time, application architecture evolved.

### New Approach:

* **Each component is separated into independent layers**:

  * **Database Layer**
  * **Business Logic Layer**
  * **UI Layer**
  * **Web API (Middleware Layer)**

### Structure:

```
[ Database Layer ]
       ↓
[ Business Logic Layer ]
       ↓
[ Web API (Middleware) ]
       ↓
[ UI Layer (Angular, React, MVC, etc.) ]
```

---

### 🔁 Decoupled Projects:

In this modern setup:

* **Web API** is a **completely separate project** (middleware)
* **UI Layer** is also a **separate independent project**

  * Can be built using:

    * Angular
    * React
    * ASP.NET MVC
    * Any other frontend technology

---

## 🎯 Role of Web API

* **Web API** acts as a **middleware** between:

  * The **Business Logic Layer**
  * And the **UI Layer**

### Output of Web API:

* Returns **data only**, not HTML
* The data format can be:

  * ✅ **JSON** (most commonly used)
  * ✅ **XML** (optional)

---

## ✅ Benefits of Using Web API

* Clean **separation of concerns**
* Reusable backend logic
* Frontend and backend can be **developed and deployed independently**
* UI can be built using **modern frameworks** (Angular, React, etc.)
* API responses are **data-centric**, ideal for modern web and mobile apps

---

## 📌 Summary

* Traditional web apps generated **HTML** directly in a single project.
* Modern apps **decouple** backend and frontend:

  * Backend (API) delivers **data**
  * Frontend renders it using modern technologies
* **Web API** serves as a **middleware** that provides this data in **JSON or XML** format.

---

## 📽️ What's Next?

In the **next video**, we will cover:

> **Why do we need to make Web API as a middleware?**
> What is the **importance** and **advantage** of using Web API?

---

## 🙏 Conclusion

For more videos like this,
**Please Like, Share, and Subscribe**.
**Thank you!**

---

Would you like a **PDF version** of this formatted documentation for download or sharing?
