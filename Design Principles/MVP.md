# 🎯 MVP Design Pattern in Android

A beginner-to-intermediate friendly guide to understanding and implementing the **MVP (Model–View–Presenter)** design pattern in Android development.

---

## 📌 1. Introduction

The **MVP (Model–View–Presenter)** design pattern is an **architectural pattern** used to structure Android applications.  
It helps **separate concerns** by dividing the app into three distinct layers:

- **Model** → Handles data, API calls, and business logic  
- **View** → Displays the UI and receives user input  
- **Presenter** → Acts as a middle layer to handle communication between the View and Model

MVP is often used in **legacy Android projects** or when you need **tight control over UI interactions** and **testable business logic**.

---

## 📌 2. MVP Components

### **1. Model**
- Responsible for **data** and **business logic**
- Can fetch data from **local DB** or **remote API**
- Should **not** know anything about the View or Presenter

```kotlin
data class User(val name: String, val email: String)

class UserRepository {
    fun getUser(): User {
        // Simulating API or database call
        return User("Sai Hemanth", "hemanth@example.com")
    }
}
````

---

### **2. View**

* Handles **UI rendering** and **user input**
* Contains **no business logic**
* Communicates only with the **Presenter**

```kotlin
interface UserView {
    fun showUser(user: User)
    fun showError(message: String)
}
```

---

### **3. Presenter**

* Acts as a **middleman** between Model and View
* Fetches data from the Model and updates the View
* Contains **presentation logic**

```kotlin
class UserPresenter(private val view: UserView, private val repository: UserRepository) {

    fun loadUser() {
        val user = repository.getUser()
        if (user != null) {
            view.showUser(user)
        } else {
            view.showError("User not found")
        }
    }
}
```

---

## 📌 3. How MVP Improves Separation of Concerns

| Layer         | Responsibility                     | Knows About    |
| ------------- | ---------------------------------- | -------------- |
| **Model**     | Data & business rules              | Nothing else   |
| **View**      | UI & user interaction              | Presenter only |
| **Presenter** | Controls flow between View & Model | View + Model   |

✅ **Benefits:**

* **Better Testability** → Business logic is separated from UI
* **Code Maintainability** → Clear responsibilities
* **Reusability** → UI changes don’t break logic

---

## 📌 4. MVP Architecture Diagram

```plaintext
   +-----------+        +-----------+        +-----------+
   |   Model   | <----> | Presenter | <----> |   View    |
   +-----------+        +-----------+        +-----------+
       ^   |                ^   |                ^   |
       |   v                |   v                |   v
   Data sources       Business logic          UI rendering
 (API, Database)       & communication        & user input
```

---

## 📌 5. Real-World Android MVP Example

**Scenario:** Display user profile from a repository.

### **1. Model**

```kotlin
data class User(val name: String, val email: String)

class UserRepository {
    fun getUser(): User {
        // Example: Fetch from API or DB
        return User("Sai Hemanth", "hemanth@example.com")
    }
}
```

### **2. View (Activity)**

```kotlin
class MainActivity : AppCompatActivity(), UserView {

    private lateinit var presenter: UserPresenter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        presenter = UserPresenter(this, UserRepository())
        presenter.loadUser()
    }

    override fun showUser(user: User) {
        findViewById<TextView>(R.id.tvName).text = user.name
        findViewById<TextView>(R.id.tvEmail).text = user.email
    }

    override fun showError(message: String) {
        Toast.makeText(this, message, Toast.LENGTH_SHORT).show()
    }
}
```

### **3. Presenter**

```kotlin
class UserPresenter(private val view: UserView, private val repository: UserRepository) {

    fun loadUser() {
        val user = repository.getUser()
        if (user.name.isNotEmpty()) {
            view.showUser(user)
        } else {
            view.showError("User not found")
        }
    }
}
```

---

## 📌 6. Best Practices for MVP in Android

✅ Keep **UI logic in View**, business logic in Presenter, and data logic in Model
✅ Use **interfaces** for the View to improve testability
✅ Keep **Presenter independent** from Android SDK classes for easier testing
✅ Keep **Model clean and reusable** for other parts of the app

---

## ⚠️ Common Mistakes to Avoid

❌ Putting API calls directly inside the Activity
❌ Allowing the Presenter to handle UI rendering
❌ Mixing business logic inside the View
❌ Tight coupling between Model, View, and Presenter

---

## 📌 7. MVP vs MVVM vs MVC

| Pattern  | Communication             | Testability | Modern Usage in Android |
| -------- | ------------------------- | ----------- | ----------------------- |
| **MVP**  | View ↔ Presenter ↔ Model  | High        | Legacy / Custom apps    |
| **MVVM** | View ↔ ViewModel ↔ Model  | Very High   | Jetpack recommended     |
| **MVC**  | View ↔ Controller ↔ Model | Low         | Rare in Android         |

---

## 📌 8. Quick Reference

| Component     | Responsibility        | Example          |
| ------------- | --------------------- | ---------------- |
| **Model**     | Data & business logic | `UserRepository` |
| **View**      | UI rendering          | `MainActivity`   |
| **Presenter** | Connects View & Model | `UserPresenter`  |

---

## 📚 References

* [Google Android Architecture Guide](https://developer.android.com/jetpack/guide)
* [MVP on Android](https://antonioleiva.com/mvp-android/)
* [Clean Architecture Concepts](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)

---

## 📜 License

This guide is licensed under the [MIT License](LICENSE).
