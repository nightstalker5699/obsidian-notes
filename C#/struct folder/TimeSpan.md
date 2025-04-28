In C#, `TimeSpan` is a **struct** (value type) that represents a time interval or duration. It is part of the `System` namespace and is commonly used to measure or manipulate differences between two dates or times, or to represent a specific duration (e.g., 5 hours, 10 minutes, 30 seconds).

---

### Key Features of `TimeSpan`:
1. **Represents a Duration**: It can store days, hours, minutes, seconds, milliseconds, and even smaller units like ticks (100-nanosecond intervals).
2. **Immutable**: Like `DateTime`, `TimeSpan` is immutable. Any operation on a `TimeSpan` object returns a new `TimeSpan` instance.
3. **Precision**: It can represent time intervals with a precision of **100 nanoseconds** (1 tick).
4. **Common Use Cases**:
   - Calculating the difference between two `DateTime` values.
   - Adding or subtracting time intervals from a `DateTime`.
   - Representing a fixed duration (e.g., a timeout or a delay).

---

### Creating a `TimeSpan` Object:
You can create a `TimeSpan` object using its constructors, static methods, or factory methods.

#### Example:
```csharp
// Using constructor (hours, minutes, seconds)
TimeSpan time1 = new TimeSpan(2, 30, 0); // 2 hours, 30 minutes, 0 seconds

// Using constructor (days, hours, minutes, seconds, milliseconds)
TimeSpan time2 = new TimeSpan(1, 2, 30, 45, 500); // 1 day, 2 hours, 30 minutes, 45 seconds, 500 milliseconds

// Using static methods
TimeSpan time3 = TimeSpan.FromHours(2.5); // 2 hours and 30 minutes
TimeSpan time4 = TimeSpan.FromMinutes(90); // 1 hour and 30 minutes
TimeSpan time5 = TimeSpan.FromTicks(10000000); // 1 second (1 tick = 100 nanoseconds)
```

---

### Common Properties of `TimeSpan`:
| Property          | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| `Days`            | Gets the number of whole days in the interval.                              |
| `Hours`           | Gets the number of whole hours in the interval (0 to 23).                   |
| `Minutes`         | Gets the number of whole minutes in the interval (0 to 59).                 |
| `Seconds`         | Gets the number of whole seconds in the interval (0 to 59).                 |
| `Milliseconds`    | Gets the number of whole milliseconds in the interval (0 to 999).           |
| `Ticks`           | Gets the total number of ticks (100-nanosecond intervals) in the interval.  |
| `TotalDays`       | Gets the total duration in days (including fractional parts).               |
| `TotalHours`      | Gets the total duration in hours (including fractional parts).              |
| `TotalMinutes`    | Gets the total duration in minutes (including fractional parts).            |
| `TotalSeconds`    | Gets the total duration in seconds (including fractional parts).            |
| `TotalMilliseconds` | Gets the total duration in milliseconds (including fractional parts).     |

#### Example:
```csharp
TimeSpan duration = new TimeSpan(1, 2, 30, 45, 500); // 1 day, 2 hours, 30 minutes, 45 seconds, 500 milliseconds

Console.WriteLine($"Days: {duration.Days}"); // Output: 1
Console.WriteLine($"Hours: {duration.Hours}"); // Output: 2
Console.WriteLine($"Minutes: {duration.Minutes}"); // Output: 30
Console.WriteLine($"Seconds: {duration.Seconds}"); // Output: 45
Console.WriteLine($"Milliseconds: {duration.Milliseconds}"); // Output: 500
Console.WriteLine($"Total Hours: {duration.TotalHours}"); // Output: 26.5125
Console.WriteLine($"Total Minutes: {duration.TotalMinutes}"); // Output: 1590.75
Console.WriteLine($"Total Seconds: {duration.TotalSeconds}"); // Output: 95445.5
Console.WriteLine($"Total Milliseconds: {duration.TotalMilliseconds}"); // Output: 95445500
```

---

### Common Methods of `TimeSpan`:
| Method                     | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| `Add(TimeSpan ts)`         | Adds a `TimeSpan` to the current `TimeSpan`.                                |
| `Subtract(TimeSpan ts)`    | Subtracts a `TimeSpan` from the current `TimeSpan`.                         |
| `ToString()`               | Converts the `TimeSpan` to a string representation.                         |
| `Parse(string s)`          | Converts a string to a `TimeSpan`.                                          |
| `TryParse(string s, out TimeSpan result)` | Safely attempts to convert a string to a `TimeSpan`.                |
| `FromDays(double value)`   | Creates a `TimeSpan` from a specified number of days.                       |
| `FromHours(double value)`  | Creates a `TimeSpan` from a specified number of hours.                      |
| `FromMinutes(double value)`| Creates a `TimeSpan` from a specified number of minutes.                    |
| `FromSeconds(double value)`| Creates a `TimeSpan` from a specified number of seconds.                    |
| `FromMilliseconds(double value)` | Creates a `TimeSpan` from a specified number of milliseconds.         |

#### Example:
```csharp
TimeSpan time1 = new TimeSpan(2, 30, 0); // 2 hours, 30 minutes
TimeSpan time2 = new TimeSpan(0, 45, 0); // 45 minutes

TimeSpan totalTime = time1.Add(time2); // 3 hours, 15 minutes
TimeSpan difference = time1.Subtract(time2); // 1 hour, 45 minutes

Console.WriteLine($"Total Time: {totalTime}"); // Output: 03:15:00
Console.WriteLine($"Difference: {difference}"); // Output: 01:45:00
```

---

### Formatting `TimeSpan`:
You can format a `TimeSpan` object using the `ToString` method with custom or standard format strings.

#### Example:
```csharp
TimeSpan duration = new TimeSpan(1, 2, 30, 45, 500);

// Standard format
Console.WriteLine(duration.ToString()); // Output: 1.02:30:45.5000000

// Custom format
Console.WriteLine(duration.ToString(@"dd\.hh\:mm\:ss")); // Output: 01.02:30:45
```

---

### Parsing `TimeSpan`:
You can convert a string to a `TimeSpan` using `TimeSpan.Parse` or `TimeSpan.TryParse`.

#### Example:
```csharp
string timeString = "02:30:00";
TimeSpan parsedTime = TimeSpan.Parse(timeString);
Console.WriteLine(parsedTime); // Output: 02:30:00

// Using TryParse to avoid exceptions
if (TimeSpan.TryParse("02:30:00", out TimeSpan result))
{
    Console.WriteLine("Parsed successfully: " + result);
}
else
{
    Console.WriteLine("Invalid time format.");
}
```

---

### Common Use Cases:
1. **Calculating Time Differences**:
   ```csharp
   DateTime start = new DateTime(2023, 10, 15, 10, 0, 0);
   DateTime end = new DateTime(2023, 10, 15, 14, 30, 0);
   TimeSpan difference = end - start;
   Console.WriteLine($"Difference: {difference}"); // Output: 04:30:00
   ```

2. **Adding/Subtracting Time Intervals**:
   ```csharp
   DateTime now = DateTime.Now;
   TimeSpan duration = new TimeSpan(2, 0, 0); // 2 hours
   DateTime futureTime = now.Add(duration);
   Console.WriteLine($"Future Time: {futureTime}");
   ```

3. **Representing Fixed Durations**:
   ```csharp
   TimeSpan timeout = TimeSpan.FromSeconds(30); // 30-second timeout
   Console.WriteLine($"Timeout: {timeout}");
   ```

---

### Summary:
- `TimeSpan` is used to represent a time interval or duration.
- It provides properties and methods for manipulating and formatting time intervals.
- It is commonly used with `DateTime` for time-based calculations.

Let me know if you need further clarification or examples!