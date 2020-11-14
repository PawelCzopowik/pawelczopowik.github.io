[<< Previous Chapter](Csharp_v8-4.md) | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-6.md)
# PART III Object-Oriented Programming with C#
# CHAPTER 5 Understanding Encapsulation

## Constructors As Expression-Bodied Members (New 7.0)
Oneliner constructor
```c#
public Car(string pn) => petName = pn;
```

## Constructors with out Parameters (New 7.3)
```c#
public Car(string pn, int cs, out bool inDanger) {...}
```

## Chaining Constructor Calls Using this
- Use a function to validate, called by ctor
- Have a "master" constructor that does all the validation, chain the rest
- __Alternative is optional params (default values)__

```c#
class Motorcycle
{
  public int driverIntensity;
  public string driverName;
  
  // Constructor chaining.
  public Motorcycle() {}
  public Motorcycle(int intensity)
    : this(intensity, "") {}
  public Motorcycle(string name)
    : this(0, name) {}
  
  // This is the 'master' constructor that does all the real work.
  public Motorcycle(int intensity, string name)
  {
    if (intensity > 10)
    {
      intensity = 10;
    }
    driverIntensity = intensity;
    driverName = name;
  }
  ...
}
```

Optional arguments
```c#

class Motorcycle
{
  // Single constructor using optional args.
  public Motorcycle(int intensity = 0, string name = "")
  {
    if (intensity > 10)
    {
      intensity = 10;
    }
      driverIntensity = intensity;
      driverName = name;
    }
  ...
}
```

## Understanding the static Keyword
- Initialize static data so its set once, rather than on each constructor call  
`public static double currInterestRate = 0.04;`

### Defining Static Constructors
- Simply put, a static constructor is a special constructor that is an ideal place to initialize the values of static data when the value is not known at compile time
   - A given class may define only a single static constructor. In other words, the static constructor cannot be overloaded.
   - A static constructor does not take an access modifier and cannot take any parameters.
   - A static constructor executes exactly one time, regardless of how many objects of the type are created.
   - The runtime invokes the static constructor when it creates an instance of the class or before accessing the first static member invoked by the caller.
   - The static constructor executes before any instance-level constructors.


### Defining Static Classes
- Static class can only have static members
### Importing Static Members via the C# using Keyword
- For static imports, use `static`
```c#
using static System.Console;
```

## Defining the Pillars of OOP
- *Encapsulation:* How does this language hide an object’s internal implementation details and preserve data integrity?
- *Inheritance:* How does this language promote code reuse?
- *Polymorphism:* How does this language let you treat related objects in a similar way?

## C# Access Modifiers (Updated 7.2)
### The Default Access Modifiers
- By default, type members are implicitly private, while types are implicitly internal

```c#
class Radio
{
  Radio(){}
}

// Same as
internal class Radio
{
  private Radio(){}
}
```

## The First Pillar: C#’s Encapsulation Services
### Encapsulation Using Properties
- See example in here for Expression-Bodied Members =>
- Use properties in a class, have validation in set/get
- Properties can be `private` (ex. private set) or `static`


```c#
class Employee
{
  // Field data.
  private string _empName;
  private int _empId;

  private float _currPay;
  // Properties!
  public string Name // Note lack of parentheses
  {
    get { return _empName; }
    set
    {
      if (value.Length > 15)
      {
        Console.WriteLine("Error! Name length exceeds 15 characters!");
      }
      else
      {
        _empName = value;
      }
    }
  }
  
  // We could add additional business rules to the sets of these properties;
  // however, there is no need to do so for this example.
  public int Id
  {
    get { return _empId; }
    set { _empId = value; }
    
    // Example of Expression-bodied (7.0)
    get => _empID;
    set => _empId = value;
  }
  public float Pay
  {
    get { return _currPay; }
    set { _currPay = value; }
  }
  ...
}
```

### Automatic properties
- VB shortcut `prop`
- Just basic encapsulation

```c#
public int Speed { get; set }
```
#### Initialization of Automatic Properties
```c#
class Garage
{
  // The hidden backing field is set to 1.
  public int NumberOfCars { get; set; } = 1;
  
  // The hidden backing field is set to a new Car object.
  public Car MyAuto { get; set; } = new Car();
  
  public Garage(){}
  public Garage(Car car, int number)
  {
    MyAuto = car;
    NumberOfCars = number;
  }
}
```
## Understanding Object Initialization Syntax
- This calls defaul ctor and sets values via properties
```c#
static void Main(string[] args)
{
  // Or make a Point using object init syntax.
  Point finalPoint = new Point { X = 30, Y = 30 };
}
```
- But we can call custom ctors
```c#
// Calling a custom constructor.
Point pt = new Point(10, 16) { X = 100, Y = 100 };  // Redundant, results in 100/100
```

### Initializing Data with Initialization Syntax
- Since automatic properties set all fields of class variables to null, we use a traditional property syntax
- This initialization reduces code
```c#
// Create and initialize a Rectangle.
Rectangle myRect = new Rectangle
{
  TopLeft = new Point { X = 10, Y = 10 },
  BottomRight = new Point { X = 200, Y = 200}
};
```
## Working with Constant Field Data
- Constant fields of a class are implicitly `static` but local `const` are allowed

### Understanding Read-Only Fields
- `readonly` keyword
- Not to be confused with r/o property
- Can be assigned during runtime

```c#
class MyMathClass
{
  // Read-only fields can be assigned in ctors, but nowhere else.
  public readonly double PI;
  public MyMathClass ()
  {
    PI = 3.14;
  }
}
```
### Static Read-Only Fields
- Case: not known at compile time, but needs to be static

## Understanding Partial Classes
- Allows you to split a class definition among files



```c#
// Employee.cs
partial class Employee
{
  // Methods
  // Properties
}

// Employee.Core.cs
partial class Employee
{
  // Field data
  // Properties
}
```
[<< Previous Chapter](Csharp_v8-4.md) | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-6.md)
