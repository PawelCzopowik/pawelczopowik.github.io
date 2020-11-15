[<< Previous Chapter](Csharp_v8-6.md) | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-8.md)

CHAPTER 7 Understanding Structured Exception Handling
===

- As of 7.0, `throw` is available as an Expression, not just Statement
- System exceptions are for core, application exceptions are to be used for app code. This is simply to distinguish origin of error.


Core Members of the System.Exception Type
- Data
- HelpLink
- InnerException
- Message
- Source
- StackTrace
- TargetSite


Throwing a general Exception
```c#
throw new Exception($"{PetName} has overheated!");
```
## Custom Exception example
```c#
using System.Runtime.Serialization;
[Serializable]
public class CarIsDeadException : ApplicationException
{
  public CarIsDeadException(string cause, DateTime time)
  : this(cause,time,string.Empty) { }
  
  public CarIsDeadException(
    string cause, DateTime time, string message)
  : this(cause,time,string.Empty, null) { }
    
  public CarIsDeadException(string cause, DateTime time,
    string message, System.Exception inner)
  : base(message, inner)
  {
    CauseOfError = cause;
    ErrorTimeStamp = time;
  }
    protected CarIsDeadException(string cause, DateTime time,
      SerializationInfo info, StreamingContext context)
    : base(info, context)
  {
    CauseOfError = cause;
    ErrorTimeStamp = time;
  }
// Any additional custom properties and data members...
}
```

# Filter
```c#
catch (CarIsDeadException e) when (e.ErrorTimeStamp.DayOfWeek != DayOfWeek.Friday)
{
  // This new line will only print if the when clause evaluates to true.
  Console.WriteLine("Catching car is dead!");
  Console.WriteLine(e.Message);
}
```

```c#

```

```c#

```

```c#

```

```c#

```

```c#

```

```c#

```

```c#

```
[<< Previous Chapter](Csharp_v8-6.md) | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-8.md)
