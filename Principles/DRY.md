# 💡 DRY Principle in Android – Don’t Repeat Yourself


## 🧠 What is DRY (Don’t Repeat Yourself)?

The **DRY Principle** is a core software engineering guideline that states:

> "**Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.**"  
> – Andy Hunt and Dave Thomas (*The Pragmatic Programmer*)

In simpler terms:

> **Don’t write the same logic or code more than once.** Instead, reuse it through functions, classes, or modules.

---

## 🚀 Why is DRY Important in Android Development?

In Android projects, it's **very easy to repeat code** — especially in:
- Activities and Fragments (boilerplate UI logic)
- ViewModels (duplicated logic for similar screens)
- Repositories (repeating API or DB access code)
- Adapters (similar item bindings)

### ❌ Real Issue Example:
Copy-pasting the same `Toast`, `Log.d()`, or `network call logic` in multiple places:
- Makes debugging harder
- Increases chance of inconsistent behavior
- Slows down feature changes and updates
- Violates clean architecture principles

---

## 🎯 Real-Life Analogy

**Imagine you’re a chef** preparing 10 dishes and **writing the same pancake recipe** by hand every time.

Wouldn’t it be smarter to **write it once and refer to it whenever needed?**

That’s **DRY** in action — keep your logic in one place and reuse it.

---

## 🧑‍💻 Android Code Example: Bad vs. Good Practice

### ❌ Bad Code (Violating DRY – Duplicated Toasts)
```kotlin
// MainActivity.kt
Toast.makeText(this, "User saved successfully", Toast.LENGTH_SHORT).show()

// AnotherActivity.kt
Toast.makeText(this, "User saved successfully", Toast.LENGTH_SHORT).show()
````

### ✅ Good Code (Following DRY – Using Utility Function)

```kotlin
// Utils.kt
fun Context.showToast(message: String) {
    Toast.makeText(this, message, Toast.LENGTH_SHORT).show()
}

// MainActivity.kt
showToast("User saved successfully")

// AnotherActivity.kt
showToast("User saved successfully")
```

---

## ✅ Best Practices to Apply DRY in Android

* 📦 **Extract common logic** into helper methods or utility classes
* 📤 **Reuse ViewModels** for shared business logic across screens
* 📚 Use **Base classes** (e.g., BaseActivity, BaseAdapter) for shared lifecycle or view logic
* 🧱 Break down UI into **reusable Composables/Custom Views**
* 🧪 Write **extension functions** for common logic like Toasts, Logs, etc.

---

## 🕐 When to Use DRY in Android Projects

Use DRY when you see:

* Repeated code snippets across Activities, Fragments, ViewModels
* Similar `API calls`, `database queries`, or `form validation logic`
* Shared `event handling` (e.g., button clicks, form submissions)
* Common `UI components` with repeated styling

---

## 📊 Visual Summary: Bad vs. Good Comparison

| Aspect            | ❌ Bad (Repeating)              | ✅ Good (DRY)                       |
| ----------------- | ------------------------------ | ---------------------------------- |
| Toast Message     | Written in every Activity      | Extracted to a utility function    |
| API Call Logic    | Rewritten for every call       | Centralized in a Repository class  |
| ViewModel Logic   | Copied between similar screens | Shared via a single ViewModel      |
| RecyclerView Code | Duplicated in each screen      | Reused using a generic BaseAdapter |

---

## 🔁 Related Concepts & Patterns

* **SRP (Single Responsibility Principle)** – DRY supports this by isolating responsibilities
* **Abstraction** – Helps implement DRY via interfaces and base classes
* **MVVM Architecture** – Separates logic and encourages reusability
* **Repository Pattern** – Centralizes data access logic to reduce repetition

---

## 📎 Related Resources

* [Google Android Docs – Code Style](https://developer.android.com/kotlin/style-guide)
* [Android Clean Architecture (Google Codelab)](https://developer.android.com/topic/architecture)
* [The Pragmatic Programmer (Book)](https://pragprog.com/titles/tpp20/the-pragmatic-programmer/)
* [SOLID Principles in Android (ProAndroidDev)](https://proandroiddev.com/solid-principles-with-android-examples-every-developer-should-know-869ff09f2cb1)

---

## 📘 Learn More

Looking to master Android clean code practices?
→ Explore: `SRP.md`, `MVVM.md`, `RepositoryPattern.md`

---
