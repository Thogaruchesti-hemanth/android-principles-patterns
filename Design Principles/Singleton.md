# 🦸‍♂️ Singleton Design Pattern in Android

## 📌 Introduction

The **Singleton Design Pattern** ensures that a **class has only one instance** in the entire application and provides a global access point to that instance.

In **Android development**, Singletons are widely used for managing **shared resources** like:
- Network clients
- Database instances
- SharedPreferences helpers

### ✅ Benefits
- **Global Access**: One central instance accessible throughout the app.
- **Memory Efficiency**: Avoids creating multiple objects for the same resource.
- **Consistency**: Ensures all parts of the app use the same data/resource.

### ⚠️ Potential Pitfalls
- **Memory Leaks**: Holding a reference to a `Context` (especially `Activity`) can prevent it from being garbage collected.
- **Hidden Dependencies**: Makes testing harder if overused.
- **Global State Abuse**: Overusing Singletons can lead to tightly coupled code.

---

## 📅 When to Use

**Common Android use cases for Singletons:**
- **Retrofit API Client** – One instance for all API calls.
- **Room Database Instance** – One DB connection for the whole app.
- **SharedPreferences Helper** – One handler for reading/writing preferences.
- **Logger** – Centralized logging utility.

---

## 🛠 Implementation

### **Kotlin — Thread-safe with `object`**
```kotlin
object RetrofitClient {
    private const val BASE_URL = "https://api.example.com/"

    val instance: Retrofit by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }
}
````

**Why it works:**

* `object` in Kotlin is **thread-safe** and ensures a single instance.
* `by lazy` creates the instance **only when first accessed**.

---

### **Kotlin — Classic Singleton Class**

```kotlin
class SharedPrefsHelper private constructor(context: Context) {

    private val prefs: SharedPreferences =
        context.applicationContext.getSharedPreferences("MyPrefs", Context.MODE_PRIVATE)

    companion object {
        @Volatile private var INSTANCE: SharedPrefsHelper? = null

        fun getInstance(context: Context): SharedPrefsHelper =
            INSTANCE ?: synchronized(this) {
                INSTANCE ?: SharedPrefsHelper(context).also { INSTANCE = it }
            }
    }
}
```

---

### **Java — Thread-safe Singleton**

```java
public class RetrofitClient {
    private static volatile RetrofitClient instance;
    private Retrofit retrofit;

    private RetrofitClient() {
        retrofit = new Retrofit.Builder()
            .baseUrl("https://api.example.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build();
    }

    public static RetrofitClient getInstance() {
        if (instance == null) {
            synchronized (RetrofitClient.class) {
                if (instance == null) {
                    instance = new RetrofitClient();
                }
            }
        }
        return instance;
    }

    public Retrofit getRetrofit() {
        return retrofit;
    }
}
```

---

## 🔍 Code Walkthrough

* **Private Constructor** → Prevents direct instantiation (`new`).
* **Static/Companion Instance** → Holds the single global instance.
* **Lazy Initialization** → Instance is created only when needed.
* **Thread-safety** → `synchronized` (Java) or `by lazy` (Kotlin) ensures safe creation in multithreaded apps.
* **Application Context** → Always use `context.getApplicationContext()` to avoid leaking Activities.

---

## 💡 Best Practices

* **Avoid holding strong references** to `Activity` or `View` to prevent memory leaks.
* **Use Application Context** in Singletons that need a context.
* **Don’t overuse Singletons** — prefer Dependency Injection (Hilt/Dagger) for better testability.
* **Keep Singletons stateless** whenever possible.

---

## 📋 Quick Reference Table

| Feature         | Description                                 | Android Example        |
| --------------- | ------------------------------------------- | ---------------------- |
| **What it is**  | One instance for entire app lifecycle       | RetrofitClient         |
| **When to use** | Shared resources, expensive object creation | Room Database          |
| **Key points**  | Thread-safe, lazy init, no memory leaks     | Use `object` in Kotlin |

---

## 📚 References

* [Android Developers: App Architecture](https://developer.android.com/jetpack/guide)
* [Effective Java – Item 3: Enforce the Singleton Property](https://learning.oreilly.com/library/view/effective-java/9780134686097/)
* [Kotlin Docs: Object Declarations](https://kotlinlang.org/docs/object-declarations.html)

---

> 💬 **Tip:** In modern Android, many Singletons are replaced with **Dependency Injection** (Hilt/Dagger) for improved scalability and testability.
