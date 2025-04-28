The **Liskov Substitution Principle (LSP)** is one of the five **SOLID** principles of object-oriented programming and design. It was introduced by **Barbara Liskov** in 1987 and is a fundamental guideline for creating robust and maintainable class hierarchies.

---

### **Liskov Substitution Principle (LSP)**
The LSP states that:

> **Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.**

In other words:
- If a class `B` is a subclass of class `A`, then you should be able to replace any instance of `A` with an instance of `B` without altering the behavior or correctness of the program.
- The subclass should **extend** the behavior of the superclass, not **alter** or **break** it.

---

### **Why is LSP Important?**
1. **Maintainability**: Ensures that subclasses behave consistently with the superclass, making the code easier to understand and maintain.
2. **Reusability**: Promotes the reuse of code through inheritance without introducing bugs or unexpected behavior.
3. **Polymorphism**: Enables safe use of polymorphism, where a subclass can be used wherever its superclass is expected.

---

### **Key Rules of LSP**
To adhere to the Liskov Substitution Principle, the following rules must be followed:

4. **Method Signatures Must Match**:
   - Subclasses must implement all methods of the superclass with the same signatures (name, parameters, and return type).

5. **Preconditions Cannot Be Strengthened**:
   - A subclass should not impose stricter conditions (preconditions) on input parameters than the superclass.

6. **Postconditions Cannot Be Weakened**:
   - A subclass should not weaken the guarantees (postconditions) made by the superclass about the results of a method.

7. **Invariants Must Be Preserved**:
   - A subclass must maintain the invariants (consistent states) defined by the superclass.

8. **No New Exceptions**:
   - A subclass should not throw new exceptions that are not expected by the superclass.

---

### **Example of LSP Violation**
Here’s an example that violates the Liskov Substitution Principle:

```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int CalculateArea()
    {
        return Width * Height;
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = base.Height = value; }
    }
}

// Usage
Rectangle rect = new Square();
rect.Width = 5;
rect.Height = 10;
Console.WriteLine(rect.CalculateArea()); // Output: 100 (Expected: 50)
```

In this example:
- `Square` is a subclass of `Rectangle`.
- When you set the `Width` or `Height` of a `Square`, both properties are set to the same value (since a square has equal sides).
- This behavior violates the LSP because a `Square` cannot be substituted for a `Rectangle` without changing the expected behavior (e.g., the area calculation).

---

### **How to Fix the LSP Violation**
To fix the above example, you should avoid forcing a `Square` to inherit from `Rectangle`. Instead, use a more appropriate design, such as composition or a common interface:

```csharp
public interface IShape
{
    int CalculateArea();
}

public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public int CalculateArea()
    {
        return Width * Height;
    }
}

public class Square : IShape
{
    public int SideLength { get; set; }

    public int CalculateArea()
    {
        return SideLength * SideLength;
    }
}

// Usage
IShape rect = new Rectangle { Width = 5, Height = 10 };
IShape square = new Square { SideLength = 5 };

Console.WriteLine(rect.CalculateArea()); // Output: 50
Console.WriteLine(square.CalculateArea()); // Output: 25
```

In this fixed design:
- Both `Rectangle` and `Square` implement the `IShape` interface.
- Each class maintains its own invariants and behavior without violating the LSP.

---

### **Real-World Analogy**
Think of the LSP as a rule for **plug-and-play compatibility**:
- If you have a power outlet (superclass) designed for a specific type of plug, any plug (subclass) that fits into the outlet should work without causing issues (e.g., short circuits or fires).
- If a plug doesn’t fit or causes problems, it violates the LSP.

---

### **Benefits of Following LSP**
9. **Code Reusability**: Subclasses can be used interchangeably with their superclass, promoting reuse.
10. **Flexibility**: Enables polymorphic behavior without introducing bugs.
11. **Testability**: Easier to write unit tests for superclasses and subclasses.
12. **Maintainability**: Reduces the risk of introducing bugs when extending or modifying classes.

By adhering to the Liskov Substitution Principle, you can create class hierarchies that are more robust, flexible, and maintainable.