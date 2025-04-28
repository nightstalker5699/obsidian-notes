In C#, threads are used to perform multiple tasks concurrently. They allow you to run code in parallel, which can improve the performance of your application, especially for tasks that are time-consuming or involve waiting for I/O operations.

### Basic Thread Creation

You can create a thread using the `Thread` class in the `System.Threading` namespace. Here’s a simple example:

```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // Create a new thread
        Thread myThread = new Thread(new ThreadStart(MyThreadMethod));
        
        // Start the thread
        myThread.Start();
        
        // Continue with the main thread
        Console.WriteLine("Main thread is running.");
        
        // Wait for the new thread to finish
        myThread.Join();
        
        Console.WriteLine("Main thread is finished.");
    }

    static void MyThreadMethod()
    {
        Console.WriteLine("Thread is running.");
    }
}
```

### Key Points

1. **ThreadStart Delegate**: The `ThreadStart` delegate is used to specify the method that the thread will execute.
2. **Start Method**: The `Start` method is used to begin the execution of the thread.
3. **Join Method**: The `Join` method is used to wait for the thread to finish. This ensures that the main thread waits for the new thread to complete before continuing.

### Passing Parameters to Threads

You can pass parameters to a thread using the `ParameterizedThreadStart` delegate:

```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // Create a new thread with a parameter
        Thread myThread = new Thread(new ParameterizedThreadStart(MyThreadMethod));
        
        // Start the thread and pass a parameter
        myThread.Start("Hello from thread!");
        
        // Continue with the main thread
        Console.WriteLine("Main thread is running.");
        
        // Wait for the new thread to finish
        myThread.Join();
        
        Console.WriteLine("Main thread is finished.");
    }

    static void MyThreadMethod(object data)
    {
        Console.WriteLine("Thread received: " + data);
    }
}
```

### Thread Safety

When working with threads, you need to be careful about thread safety. Multiple threads accessing shared resources can lead to race conditions and data corruption. You can use synchronization mechanisms like `lock`, `Monitor`, `Mutex`, `Semaphore`, and others to ensure thread safety.

### Example with `lock`

```csharp
using System;
using System.Threading;

class Program
{
    private static int sharedCounter = 0;
    private static readonly object lockObject = new object();

    static void Main()
    {
        Thread thread1 = new Thread(IncrementCounter);
        Thread thread2 = new Thread(IncrementCounter);

        thread1.Start();
        thread2.Start();

        thread1.Join();
        thread2.Join();

        Console.WriteLine("Final counter value: " + sharedCounter);
    }

    static void IncrementCounter()
    {
        for (int i = 0; i < 10000; i++)
        {
            lock (lockObject)
            {
                sharedCounter++;
            }
        }
    }
}
```

### Task Parallel Library (TPL)

Modern C# applications often use the Task Parallel Library (TPL) for parallel programming. TPL provides a higher-level abstraction over threads and makes it easier to work with asynchronous operations.

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static void Main()
    {
        Task myTask = Task.Run(() => MyTaskMethod());

        Console.WriteLine("Main thread is running.");

        myTask.Wait();

        Console.WriteLine("Main thread is finished.");
    }

    static void MyTaskMethod()
    {
        Console.WriteLine("Task is running.");
    }
}
```

### Summary

- **Threads**: Use the `Thread` class for basic threading.
- **Parameters**: Use `ParameterizedThreadStart` to pass parameters to threads.
- **Thread Safety**: Use synchronization mechanisms like `lock` to ensure thread safety.
- **Task Parallel Library (TPL)**: Use TPL for more advanced parallel programming and easier management of asynchronous tasks.

Threads are a powerful feature in C#, but they require careful management to avoid common pitfalls like race conditions and deadlocks.