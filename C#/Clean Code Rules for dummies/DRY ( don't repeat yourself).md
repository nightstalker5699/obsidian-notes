The DRY (Don't Repeat Yourself) principle is a fundamental concept in software development that emphasizes the importance of avoiding code duplication. The idea is to write code in such a way that each piece of knowledge or logic has a single, unambiguous representation within a system. This makes the code easier to maintain, understand, and modify.

### Example in C#

Let's say you have a C# application where you need to calculate the area of a rectangle in multiple places. Without following the DRY principle, you might write the same logic multiple times:

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Calculating area in multiple places
        int length1 = 10;
        int width1 = 5;
        int area1 = length1 * width1;
        Console.WriteLine("Area 1: " + area1);

        int length2 = 7;
        int width2 = 3;
        int area2 = length2 * width2;
        Console.WriteLine("Area 2: " + area2);

        int length3 = 12;
        int width3 = 4;
        int area3 = length3 * width3;
        Console.WriteLine("Area 3: " + area3);
    }
}
```

In this example, the logic for calculating the area of a rectangle is repeated multiple times. This violates the DRY principle.

### Applying DRY Principle

To adhere to the DRY principle, you can encapsulate the area calculation logic in a method and reuse it wherever needed:

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Reusing the CalculateArea method
        int length1 = 10;
        int width1 = 5;
        Console.WriteLine("Area 1: " + CalculateArea(length1, width1));

        int length2 = 7;
        int width2 = 3;
        Console.WriteLine("Area 2: " + CalculateArea(length2, width2));

        int length3 = 12;
        int width3 = 4;
        Console.WriteLine("Area 3: " + CalculateArea(length3, width3));
    }

    // Method to calculate area
    static int CalculateArea(int length, int width)
    {
        return length * width;
    }
}
```

### Benefits of DRY Principle

1. **Maintainability**: If you need to change the logic for calculating the area, you only need to update it in one place.
2. **Readability**: The code is easier to read and understand because the logic is encapsulated in a method with a meaningful name.
3. **Reduced Errors**: By reducing duplication, you minimize the risk of introducing bugs when making changes.

### Another Example: Using a Class

You can also encapsulate related properties and methods in a class to further adhere to the DRY principle:

```csharp
class Rectangle
{
    public int Length { get; set; }
    public int Width { get; set; }

    public Rectangle(int length, int width)
    {
        Length = length;
        Width = width;
    }

    public int CalculateArea()
    {
        return Length * Width;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Rectangle rect1 = new Rectangle(10, 5);
        Console.WriteLine("Area 1: " + rect1.CalculateArea());

        Rectangle rect2 = new Rectangle(7, 3);
        Console.WriteLine("Area 2: " + rect2.CalculateArea());

        Rectangle rect3 = new Rectangle(12, 4);
        Console.WriteLine("Area 3: " + rect3.CalculateArea());
    }
}
```

In this example, the `Rectangle` class encapsulates the properties and methods related to a rectangle, making the code even more modular and reusable.

By following the DRY principle, you ensure that your code is more efficient, easier to maintain, and less prone to errors.