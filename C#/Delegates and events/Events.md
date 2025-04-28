In C#, an **event** is a language feature that enables a class or object to **notify other classes or objects** when something happens. Events are built on top of **delegates** and are commonly used for implementing **event-driven programming**, such as handling user interactions (e.g., button clicks) or responding to system events.

---

### **Key Concepts of Events:**
1. **Publisher**: The class that raises (triggers) the event.
2. **Subscriber**: The class that listens to and handles the event.
3. **Delegate**: The event is based on a delegate, which defines the signature of the event handler methods.
4. **Event Handler**: A method that responds to the event.

---

### **How Events Work:**
1. The **publisher** defines an event using the `event` keyword and a delegate type.
2. The **subscriber** registers an event handler method to the event using the `+=` operator.
3. When the event is triggered (e.g., a button is clicked), all registered event handlers are invoked.

---

### **Example of an Event:**

#### Step 1: Define a Delegate for the Event
```csharp
public delegate void EventHandler(string message);
```

#### Step 2: Create a Publisher Class
The publisher class defines the event and a method to raise it.

```csharp
public class Publisher
{
    // Declare the event
    public event EventHandler OnEvent;

    // Method to raise the event
    public void RaiseEvent(string message)
    {
        // Check if there are any subscribers
        if (OnEvent != null)
        {
            // Raise the event
            OnEvent(message);
        }
        // OR we can use this
        OnEvent?.Invoke(message)
    }
}
```

#### Step 3: Create a Subscriber Class
The subscriber class defines the event handler method and subscribes to the event.

```csharp
public class Subscriber
{
    // Event handler method
    public void HandleEvent(string message)
    {
        Console.WriteLine("Subscriber received: " + message);
    }
}
```

#### Step 4: Use the Event
```csharp
class Program
{
    static void Main()
    {
        // Create instances of the publisher and subscriber
        Publisher publisher = new Publisher();
        Subscriber subscriber = new Subscriber();

        // Subscribe to the event
        publisher.OnEvent += subscriber.HandleEvent;

        // Raise the event
        publisher.RaiseEvent("Hello, Event!");

        // Output: Subscriber received: Hello, Event!
    }
}
```

---

### **Key Features of Events:**
1. **Encapsulation**: Events encapsulate the delegate, preventing external classes from invoking the delegate directly.
2. **Multiple Subscribers**: Multiple subscribers can register event handlers to the same event.
3. **Safe Invocation**: The event is checked for `null` before invocation to ensure there are subscribers.

---

### **Real-World Example: Button Click Event**
Events are commonly used in GUI frameworks like Windows Forms or WPF to handle user interactions.

#### Example:
```csharp
public class Button
{
    // Define the event
    public event EventHandler Click;

    // Method to simulate a button click
    public void SimulateClick()
    {
        Console.WriteLine("Button clicked!");
        if (Click != null)
        {
            Click(this, EventArgs.Empty);
        }
    }
}

class Program
{
    static void Main()
    {
        Button button = new Button();

        // Subscribe to the Click event
        button.Click += (sender, e) => Console.WriteLine("Event handler 1: Button was clicked!");
        button.Click += (sender, e) => Console.WriteLine("Event handler 2: Button was clicked!");

        // Simulate a button click
        button.SimulateClick();

        // Output:
        // Button clicked!
        // Event handler 1: Button was clicked!
        // Event handler 2: Button was clicked!
    }
}
```

---

### **Built-in EventHandler Delegate**
C# provides a built-in `EventHandler` delegate and `EventArgs` class for defining events.

#### Example:
```csharp
public class Button
{
    // Define the event using the built-in EventHandler delegate
    public event EventHandler Click;

    public void SimulateClick()
    {
        Console.WriteLine("Button clicked!");
        Click?.Invoke(this, EventArgs.Empty); // Raise the event
    }
}

class Program
{
    static void Main()
    {
        Button button = new Button();

        // Subscribe to the Click event
        button.Click += (sender, e) => Console.WriteLine("Button was clicked!");

        // Simulate a button click
        button.SimulateClick();

        // Output:
        // Button clicked!
        // Button was clicked!
    }
}
```

---

### **Custom Event Arguments**
You can create custom event arguments by deriving from `EventArgs`.

#### Example:
```csharp
public class ButtonClickedEventArgs : EventArgs
{
    public string Message { get; }

    public ButtonClickedEventArgs(string message)
    {
        Message = message;
    }
}

public class Button
{
    // Define the event with custom event arguments
    public event EventHandler<ButtonClickedEventArgs> Click;

    public void SimulateClick()
    {
        Console.WriteLine("Button clicked!");
        Click?.Invoke(this, new ButtonClickedEventArgs("Custom message"));
    }
}

class Program
{
    static void Main()
    {
        Button button = new Button();

        // Subscribe to the Click event
        button.Click += (sender, e) => Console.WriteLine("Event handler: " + e.Message);

        // Simulate a button click
        button.SimulateClick();

        // Output:
        // Button clicked!
        // Event handler: Custom message
    }
}
```

---

##### MyExample:

```csharp
public delegate void TemperatureChangeHandler(string message);


public class TemperatureMonitor
{
    public event TemperatureChangeHandler OnTemperatureChanged;

    private int _tempernature;

    public int Tempenature
    {
        get => _tempernature;
        set
        {
            _tempernature = value;
            if (_tempernature > 30)
            {
                // Raise Event
                RaiseTempernatureChangedEvent("Temperature is above the threshold");
            }
        }
    }

    protected virtual void RaiseTempernatureChangedEvent(string message)
    {
        OnTemperatureChanged?.Invoke(message);
    }
}
    public class TemperatureAlert
    {
        public void OnTemperatureAlertChanged(string message) 
        {
            Console.WriteLine($"Alert :{message}");
        }
    }

internal class Program
{
    static void Main(string[] args)
    {

        var myAlert = new TemperatureAlert();
        var myTempMonitor = new TemperatureMonitor();
        myTempMonitor.OnTemperatureChanged += myAlert.OnTemperatureAlertChanged;
        myTempMonitor.Tempenature = 20;

        Console.WriteLine("ENTER THE TEMPERATURE");
        myTempMonitor.Tempenature = int.Parse(Console.ReadLine());
        Console.ReadLine();
    }
}
```

using event handler delegate :
```csharp 

    public class TemperatureChangedEventArgs : EventArgs
    {
        public int Temperature { get; }


        public TemperatureChangedEventArgs(int tempernature)
        {
            Temperature = tempernature;
        }
    }


    public class TemperatureMonitor
    {
        //public event TemperatureChangeHandler OnTemperatureChanged;
        public event EventHandler<TemperatureChangedEventArgs> OnTemperatureChanged;  
        private int _tempernature;

        public int Tempenature
        {
            get => _tempernature;
            set
            {
                if (_tempernature != value)
                {
                    // Raise Event
                    _tempernature = value;
                    RaiseTempernatureChangedEvent(new TemperatureChangedEventArgs(_tempernature)) ;
                }
            }
        }

        protected virtual void RaiseTempernatureChangedEvent(TemperatureChangedEventArgs e)
        {
            OnTemperatureChanged?.Invoke(this,e);
        }
    }
        public class TemperatureAlert
        {
            public void OnTemperatureAlertChanged(object sender ,TemperatureChangedEventArgs e) 
            {
                Console.WriteLine($"Alert :Temperature is {e.Temperature} \n sender is {sender}");
            }
        }

```


---

### **Summary:**
- An **event** in C# is a mechanism for notifying subscribers when something happens.
- Events are based on **delegates** and are used extensively in **event-driven programming**.
- The **publisher** raises the event, and the **subscriber** handles it by registering an event handler.
- Events provide **encapsulation**, **multiple subscribers**, and **safe invocation**.
- You can use built-in types like `EventHandler` and `EventArgs` or create custom event arguments.

Events are a fundamental part of C# and are widely used in GUI frameworks, asynchronous programming, and other scenarios where decoupled communication is needed.