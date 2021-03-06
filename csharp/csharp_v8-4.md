[<< Previous Chapter](csharp_v8.md) | [Content Table](../csharp) | [Next Chapter >>](csharp_v8-5.md)

# CHAPTER 4 Core C# Programming Constructs, Pt 2

## Arrays

```c#
int[] myInts = new int[3];
string[] stringArray = new string[] { "one", "two", "three" };

// Array initialization syntax without using the new keyword.
bool[] boolArray = { false, false, true };

var a = new[] { 1, 10, 100, 1000 };

// An array of objects can be anything at all.
object[] myObjects = new object[4];
myObjects[0] = 10;
myObjects[1] = false;
myObjects[2] = new DateTime(1969, 3, 24);

// Rectangular array
int[,] myMatrix;
myMatrix = new int[3,4];

// Jagged array
// Here we have an array of 5 different arrays.
int[][] myJagArray = new int[5][];
myJagArray[i] = new int[i + 7];

```

`System.Array` base class
Some of these are static so `Array.Sort()`
- Clear()
- CopyTo()
- Length
- Rank
- Reverse()
- Sort()

### Indices and Ranges (New 8.0)
```c#
Index idx = i   // i from a loop
Index idx = ^i  // i from the end of sequence 

// range
foreach (var itm in gothicBands[0..2])
Range r = 0..2; //the end of the range is exclusive
foreach (var itm in gothicBands[r])

Index idx1 = 0;
Index idx2 = 2;
Range r = idx1..idx2; // the end of the range is exclusive
gothicBands[..]     // no beginnign or end provided, so assumed.  
```

## Methods

### Expression-Bodied Members
For single line methods `static int Add(int x, int y) => x + y;`

### Local functions (7.0) (methods within methods)
Added to C# for custom iterator methods and asynch.. not useful
Declare the inner function `static` to prevent changing incoming vars
```c#
static int AddWrapper(int x, int y)
{
  //Do some validation here
  return Add();
  int Add()
  {
    return x + y;
  }
}
```

## Method Parameters
### Method Parameter Modifiers
- __(none)__ - by val
- `out`  - by ref, output param must be assigned by the method called
- `ref` - by ref
- `in` - read-only by ref (const)
- `params` - OPTIONALLY allows variable number of args. Must be last and only one. 

#### The Default Parameter-Passing Behavior
Without a modifier, defaults to value by value, ref by ref. Exception is string, its a ref, but default passed by value.
Default for reference types is __by reference for its properties, but by value for itself.__

#### out Modifier
```c#
static void Add(int x, int y, out int ans)
{
  ans = x + y;
}

int ans;
Add(90, 90, out ans);
```
or in 7.0 declared inside call `Add(90, 90, out int ans);`

##### Discards (New 7.0)
If you don't care about results of an out param
```c#
//This only gets the value for a, and ignores the other two parameters
FillTheseValues(out int a, out _, out _);
```

##### params Modifier
The difference between passing an array and a `params` is that `params` is **optional!**
`static double CalculateAverage(params double[] values)`

##### Defining Optional Parameters
`static void EnterLogData(string message, string owner = "Programmer")` owner is defaulted thus optional. Must be known at compile time and at end of list of params.

##### Using Named Arguments (Updated 7.2)
Useful with optional parameters.
```c#
static void DisplayFancyMessage(ConsoleColor textColor, ConsoleColor backgroundColor, string message) {...}
DisplayFancyMessage(message: "Wow! Very Fancy indeed!", textColor: ConsoleColor.DarkRed, backgroundColor: ConsoleColor.White);
```


## enum
Can specify underlying datatype
```c#
enum EmpTypeEnum : byte
{
  Manager = 10,
  Grunt = 1,
  Contractor = 100,
  VicePresident = 9
}

EmpTypeEnum emp = EmpType.Contractor;
Enum.GetUnderlyingType(emp.GetType());
Enum.GetUnderlyingType(typeof(EmpTypeEnum));
emp.ToString();   // gets the name
(byte)emp;        // gets the number

// Get all name-value pairs for incoming parameter.
Array enumData = Enum.GetValues(e.GetType());
// After looping:
Console.WriteLine("Name: {0}, Value: {0:D}", enumData.GetValue(i));
// Output: Name: Sunday, Value: 0

Enum.Format()
```

## Understanding the Structure (aka Value Type) (aka lightweight class)
Structure types are well suited for modeling mathematical, geometrical, and other “atomic” entities in your application.

Structs are derived from `System.ValueType`.

```c#
// Set all fields to default values
// using the default constructor.
Point p1 = new Point();

// Prints X=0,Y=0.
p1.Display();


struct Point
{
  // Fields of the structure.
  public int X;
  public int Y;
  
  // A custom constructor.
  public Point(int xPos, int yPos)
  {
    X = xPos;
    Y = yPos;
  }
...
}
// Call custom constructor.
Point p2 = new Point(50, 60);

// Read-only struct (New 7.2)
readonly struct ReadOnlyPoint
{
  // Fields of the structure.
  public int X {get; }
  public int Y { get; }
  ...
}


// Readonly members (New 8.0)
struct PointWithReadOnly
{
  // Fields of the structure.
  public int X;
  public readonly int Y;
  ...
}

// ref Structs (New 7.2)
?? eh...

// Disposable ref Structs (New 8.0)
ref struct DisposableRefStruct
{
  public int X;
  public readonly int Y;
  public readonly void Display()
  
  public void Dispose()
  {
    //clean up any resources here
    Console.WriteLine("Disposed!");
  }
}

```

## Understanding Value Types and Reference Types
- Functionally, the only purpose of `System.ValueType` is to override the virtual methods defined by `System.Object` to use value-based vs. reference-based semantics.
- Value types assignment copies member-by-member
- Ref assignment is a pointer
- __!!__ Nesting a ref inside a value type struct, will follow the above rules. Meaning, the vals will be copied and refs will remain a pointer!

### Passing Reference Types by Value or Reference
- If a reference type is passed by reference, the callee may change the values of the object’s state data, as well as the object it is referencing.
- If a reference type is passed by value, the callee may change the values of the object’s state data but not the object it is referencing.


## Understanding C# Nullable Types
Defined by appending `?` which is a shorthand for `System.Nullable<T>`. Useful for DBs where the result can come back NULL.

```c#
// Define some local nullable variables.
int? nullableInt = 10;
double? nullableDouble = 3.14;
bool? nullableBool = null;
Nullable<bool> nullableBool2 = null; // Generic version
char? nullableChar = 'a';
int?[] arrayOfNullableInts = new int?[10];
```

### Nullable Reference Types (New 8.0)
Same as above but not inheriting `Nullable<T>` since its not a value type.
Support for nullable reference types is controlled by setting a Nullable Context.
- Nullable Annotation Context: This enables/disables the nullable annotation (?) for nullable reference types.
- Nullable Warning Context: This enables/disables the compiler warnings for nullable reference types.

There are project and compiler directives (local) settings to allow for nullable ref types.

#### The Null-Coalescing Operator
`??` This operator allows you to assign a value to a nullable type if the retrieved value is in fact `null`.
```c#
// If the value from GetIntFromDatabase() is null, assign local variable to 100.
int myData = dr.GetIntFromDatabase() ?? 100;

```
#### The Null-Coalescing Assignment Operator (New 8.0)
`??=` This operator assigns the left-hand side to the right-hand side only if the left-hand side is `null`.
`nullableInt ??= 12;`

#### The Null Conditional Operator
```c#
// for checking null like this: if (args != null)
Console.WriteLine($"You sent me {args?.Length} arguments.");
Console.WriteLine($"You sent me {args?.Length ?? 0} arguments.");
```

## Tuples (New/Updated 7.0)
- The fields are not validated.
- You cannot define your own methods.
- can be `==` or `!=`

```c#
(string, int, string) values = ("a", 5, "c");
var values = ("a", 5, "c");

// compiler assigns it Item#
Console.WriteLine($"First item: {values.Item1}");
Console.WriteLine($"Second item: {values.Item2}");
Console.WriteLine($"Third item: {values.Item3}");

// Named
(string FirstLetter, int TheNumber, string SecondLetter) valuesWithNames = ("a", 5, "c"); // right side names are ignored
var valuesWithNames2 = (FirstLetter: "a", TheNumber: 5, SecondLetter: "c");   // right side names are necessary with var
Console.WriteLine($"Third item: {valuesWithNames.SecondLetter}");
//Using the item notation still works!
Console.WriteLine($"First item: {valuesWithNames.Item1}");

// Returning a Tupel (vs out param)
static (int a,string b,bool c) FillTheseValues()
{
  return (9,"Enjoy your string.",true);
}

// Discards
var (first, _, last) = SplitNames("Philip F Japikse");

// Pattern matching (8.0)
//Switch expression with Tuples
static string RockPaperScissors(string first, string second)
{
  return (first, second) switch
  {
    ("rock", "paper") => "Paper wins.",
    ...
    (_, _) => "Tie.",
  };
}

// Deconstruction

struct Point
{
  // Fields of the structure.
  public int X;
  public int Y;
  
  // A custom constructor.
  public Point(int XPos, int YPos)
  {
    X = XPos;
    Y = YPos;
  }
  public (int XPos, int YPos) Deconstruct() => (X, Y); // returns tuple with xpos and ypos.
}
```
## Deconstructing Tuples with Positional Pattern Matching (New 8.0)
tbfinished... zzzzzzz


[<< Previous Chapter](csharp_v8.md) | [Content Table](../csharp) | [Next Chapter >>](csharp_v8-5.md)
