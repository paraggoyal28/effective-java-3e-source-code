# 📘 Effective Java — Chapter 2 Cheat Sheet  
## Creating and Destroying Objects  
*(from **Effective Java** by Joshua Bloch)*

> **Golden Rule:** Object creation and lifecycle decisions are API design decisions.

---

## 1️⃣ Prefer Static Factory Methods over Constructors

**Why**
- Descriptive names (`of()`, `valueOf()`, `getInstance()`)
- Can return cached or shared instances
- Can return interfaces or different subtypes
- Decouples API from implementation

**Avoid when**
- Clients must subclass the class
- Discoverability via `new` is important

**Interview line**
> “Static factory methods give flexibility and control over object creation.”

---

## 2️⃣ Use Builder Pattern for Many or Optional Parameters

**Use when**
- 4+ constructor parameters
- Many optional fields
- Need immutability with validation

**Benefits**
- Readable and self-documenting
- Safe object construction
- Easy to evolve APIs

**Cost**
- Extra builder object

**Rule of thumb**
> *Complex construction → Builder pattern*

---

## 3️⃣ Singleton — Prefer Enum (If Needed)

**Best approach**

enum Service {
    INSTANCE
}

**Why**
- Thread-safe by JVM spec
- Serialization-safe
- Reflection-proof

**Caution**
- Introduces global state
- Reduces testability

## 4️⃣ Prevent Instantiation of Utility Classes

**Pattern**
private Utils() {
    throw new AssertionError();
}

**Purpose**
- Prevent accidental instantiation
- Clearly communicate intent

## 5️⃣ Prefer Dependency Injection to Hard-Wired Resources

**Bad**
- Static or hardcoded dependencies

**Good**
- Inject dependencies via constructor

**Benefits**
- Easier testing (mocking)
- Better flexibility
- Loose coupling

## 6️⃣ Avoid Creating Unnecessary Objects

**Prefer**
- Reuse immutable objects
- Use primitives over boxed types

**Avoid**
- Creating objects inside hot loops
- Redundant object wrappers

**Note**
Optimize only when performance matters; clarity first.

## 7️⃣ Eliminate Obsolete Object References

**Problem**
Garbage Collector cannot free objects that are still referenced

**Fix**
Explicitly null out unused references

**Common sources**
- Custom collections
- Caches
- Long-lived objects holding short-lived data

## 8️⃣ Avoid Finalizers and Cleaners

**Why**
- Unpredictable execution
- Performance overhead
- No guarantee they run

**Use instead**
Explicit resource management (close())

## 9️⃣ Prefer try-with-resources over try-finally

**Why**
- Cleaner and more readable
- Correct handling of suppressed exceptions
- Safer resource cleanup

**Requirement**
Implement AutoCloseable