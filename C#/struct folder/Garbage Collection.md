In C#, **Garbage Collection (GC)** is an automatic memory management feature provided by the **.NET runtime**. It helps manage the allocation and release of memory for objects in your application, ensuring that memory is used efficiently and preventing common issues like memory leaks.

---

### Key Concepts of Garbage Collection:

1. **Managed Heap**:
   - The .NET runtime allocates memory for objects on a managed heap.
   - The heap is divided into generations: **Generation 0**, **Generation 1**, and **Generation 2** (more on this below).

2. **Garbage Collector**:
   - The Garbage Collector is a background process that automatically reclaims memory by freeing objects that are no longer in use.
   - It tracks all objects in the managed heap and identifies which objects are still reachable (in use) and which are not (eligible for collection).

3. **Roots**:
   - Roots are references to objects that are always considered "in use." These include:
     - Static fields.
     - Local variables on the stack.
     - CPU registers.
   - The GC starts from these roots and traverses the object graph to determine which objects are still reachable.

4. **Generations**:
   - The managed heap is divided into three generations to optimize garbage collection:
     - **Generation 0**: Short-lived objects (e.g., temporary variables). Most objects are collected here.
     - **Generation 1**: Objects that survive a Generation 0 collection.
     - **Generation 2**: Long-lived objects (e.g., global or static variables).
   - The GC performs collections more frequently on younger generations (Generation 0) and less frequently on older generations (Generation 2).

5. **Finalization**:
   - Objects that require cleanup (e.g., releasing unmanaged resources like file handles) can implement a **finalizer** using the `~ClassName` syntax.
   - The GC places such objects in a **finalization queue** and calls their finalizers before reclaiming their memory.

---

### How Garbage Collection Works:

1. **Allocation**:
   - When you create a new object using the `new` keyword, memory is allocated on the managed heap.

2. **Collection**:
   - When the heap is full or the system is low on memory, the GC is triggered.
   - The GC identifies unreachable objects (objects no longer referenced by any roots).
   - It reclaims memory by freeing these objects and compacts the heap to reduce fragmentation.

3. **Finalization**:
   - If an object has a finalizer, it is moved to the finalization queue before being collected.
   - The finalizer is executed on a separate thread, and the object's memory is reclaimed in a subsequent GC cycle.

---

### Example of Garbage Collection:

```csharp
class Program
{
    static void Main()
    {
        // Create an object
        MyClass obj = new MyClass();

        // obj is now reachable (rooted)
        obj = null; // obj is no longer reachable (eligible for GC)

        // Force garbage collection (for demonstration purposes only)
        GC.Collect();
        GC.WaitForPendingFinalizers(); // Wait for finalizers to run

        Console.WriteLine("Garbage collection completed.");
    }
}

class MyClass
{
    // Constructor
    public MyClass()
    {
        Console.WriteLine("MyClass object created.");
    }

    // Finalizer
    ~MyClass()
    {
        Console.WriteLine("MyClass object finalized.");
    }
}
```

#### Output:
```
MyClass object created.
MyClass object finalized.
Garbage collection completed.
```

---

### Summary:
- Garbage Collection in C# is an automatic process that manages memory allocation and deallocation.
- It uses generations to optimize performance and reclaims memory for unreachable objects.
- Avoid manual GC calls and minimize the use of finalizers for better performance.

Let me know if you need further clarification or examples!