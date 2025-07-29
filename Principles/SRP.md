# 🧠 Single Responsibility Principle (SRP) – Android Development Guide

## What is SRP (Single Responsibility Principle)?

The **Single Responsibility Principle (SRP)** is the **first** and most fundamental principle in the **SOLID** design principles.

> 📘 **Definition**:  
> *A class should have only one reason to change.*  
> – Robert C. Martin (Uncle Bob)

### 🔰 Beginner-Friendly Meaning:
> Every class, function, or module should **do one thing** and **do it well**.

This means: **Don't mix multiple responsibilities** (like handling UI, business logic, and database tasks) in a single class. Instead, **divide them clearly**.

---

## 🤖 Why is SRP Important in Android Development?

Android projects grow fast and can easily become messy if one class tries to do everything.

### ✅ Benefits of SRP in Android:
- Makes your code **clean**, **modular**, and **easier to test**
- Reduces **bugs** when making changes
- Improves **reusability** of components
- Follows **MVVM** and **Clean Architecture** standards

### 📱 Android-Specific Examples:
- Don’t write network/API code inside an **Activity**  
- Don’t do business logic in a **ViewModel**  
- Keep **Room database logic** in a separate **Repository**

---

## ☕ Real-Life Analogy

> Imagine you hire a **chef** 🧑‍🍳.  
You expect them to **cook food**, not to also **clean the floor**, **serve tables**, and **manage accounts**.

Just like in real life:
- A **person** should do **one job**
- A **class** should do **one thing**

---

## 🚫 Bad vs ✅ Good Example in Android

### 🚫 Violates SRP – Mixing Responsibilities in an Activity

```java
public class MainActivity extends AppCompatActivity {
    // 🚫 UI + business logic + data logic — all mixed here!

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // UI setup
        Button loginButton = findViewById(R.id.btn_login);

        loginButton.setOnClickListener(v -> {
            // 🚫 Login logic inside Activity (not ViewModel or UseCase)
            if (username.equals("admin") && password.equals("1234")) {
                // 🚫 Business logic inside UI layer
                Toast.makeText(this, "Login Success", Toast.LENGTH_SHORT).show();
            }

            // 🚫 Data access logic (wrong place!)
            SharedPreferences prefs = getSharedPreferences("user", MODE_PRIVATE);
            prefs.edit().putString("loggedUser", username).apply();
        });
    }
}
````

---

### ✅ Follows SRP – Responsibilities Split into Layers

```kotlin
// ✅ ViewModel (Handles logic)
class LoginViewModel(private val repository: UserRepository) : ViewModel() {
    fun login(username: String, password: String): Boolean {
        return repository.authenticateUser(username, password)
    }
}
```

```kotlin
// ✅ Repository (Handles data access)
class UserRepository {
    fun authenticateUser(username: String, password: String): Boolean {
        return username == "admin" && password == "1234"
    }
}
```

```kotlin
// ✅ Activity (Handles UI only)
class LoginActivity : AppCompatActivity() {
    private lateinit var viewModel: LoginViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        viewModel = LoginViewModel(UserRepository())

        findViewById<Button>(R.id.btn_login).setOnClickListener {
            val success = viewModel.login("admin", "1234")
            Toast.makeText(this, if (success) "Login Success" else "Login Failed", Toast.LENGTH_SHORT).show()
        }
    }
}
```

---

## 🛠️ Best Practices for Using SRP in Android

* ✅ Split code across **Activity/Fragment**, **ViewModel**, **Repository**
* ✅ Keep **one job per class** (e.g., don’t put DB + API logic together)
* ✅ Use **utility classes** for reusable logic (e.g., DateFormatter)
* ✅ Follow **MVVM** or **Clean Architecture** guidelines

---

## ⚠️ Common Mistakes to Avoid

* ❌ Doing everything inside `Activity` or `Fragment`
* ❌ Putting both **UI logic + business logic** in ViewModel
* ❌ Accessing the database directly from the UI
* ❌ Ignoring proper architectural layering

---

## 🕒 When to Use SRP in Android

* ✅ From the **start** of your project — apply it as a habit
* 🔁 During **refactoring** – break down large classes
* 🧪 When writing **tests** – SRP makes unit testing easier

---

## 📊 Good vs Bad – Quick Table

| 👎 Bad Practice                 | ✅ Good Practice                         |
| ------------------------------- | --------------------------------------- |
| Activity handles everything     | Activity only handles UI                |
| ViewModel contains API calls    | ViewModel delegates to Repository       |
| One class does multiple jobs    | Each class has a single responsibility  |
| Spaghetti code hard to maintain | Modular, clean, and easy-to-update code |

---

## 🔗 Related Concepts

* 🧱 **MVVM Architecture** – Enforces SRP via View → ViewModel → Repository layers
* 🏗️ **Repository Pattern** – Handles only data sources (follows SRP)
* 🔁 **SOLID Principles** – SRP is the foundation for the rest
* 🔍 **Separation of Concerns** – Closely tied to SRP

---

## 📘 Learn More

* [Official Android Architecture Guide](https://developer.android.com/jetpack/guide)
* [Clean Architecture for Android (Google I/O)](https://www.youtube.com/watch?v=4E2ytm3U0WE)
* [SOLID Principles – Uncle Bob](https://youtu.be/rtmFCcjEgEw)

---

> ✅ **Keep it clean. Keep it single. Respect SRP.**
