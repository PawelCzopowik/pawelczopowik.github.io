| -- | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-4.md)
# C# V8 Book notes


# CHAPTER 1 Introducing .NET Core

## Blocks of the .NET Core Platform (.NET core runtine environment)
- CoreCLR - Common Language Runtime, 
- CoreFX - .NET libraries, mostly platform agnostic
- CTS - Common Type System, describes all possible data types and all programming constructs
- CLS - Common Language Specification (for compliance)


---
- .NET Core binaries do not contain platform-specific instructions but rather platform-agnostic Intermediate Language (IL) and type metadata.
- .NET Core projects are always compiled to a file with a .dll extension, even if the project is an executable.
- In addition to CIL instructions, a .NET Core assembly contains full, complete, and accurate metadata, which describes every type defined in the binary, as well as the members of each type.
- Assembly also has metadata about itself.

## Understanding the Common Type System
- CTS Class Types
- CTS Interface Types
- CTS Structure Types
- CTS Enumeration Types
- CTS Delegate Types (Function pointers)
- CTS Type Members
- Intrinsic CTS Data Types
  - System.UInt64, System.String, System.Boolean

Visual Class Designer

# PART II Core C# Programming
# CHAPTER 3 Core C# Programming Constructs, Part 1

```c#
class Program
{
  static void Main(string[] args)
  {}
  
  // Get arguments using System.Environment.
string[] theArgs = Environment.GetCommandLineArgs();
```
## System.Environment Class
```c#
// Helper method within the Program class.
ShowEnvironmentDetails();
// Print out the drives on this machine,
// and other interesting details.
foreach (string drive in Environment.GetLogicalDrives())
  {
    //properties examples:
    drive;
    Environment.OSVersion
    Environment.ProcessorCount
    
  }
```
## System.Console Class
- Beep() 
- BackgroundColor
- ForegroundColo
- BufferHeightBufferWidth
- Title
- WindowHeight
- WindowWidth
- WindowTop
- WindowLeft
- Clear()

## Formatting console output
```c#
Console.WriteLine("{1}, {0}, {2}", 10, 20, 30);
Console.WriteLine("The value 99999 in various formats:");
Console.WriteLine("c format: {0:c}", 99999); c format: $99,999.00
Console.WriteLine("d9 format: {0:d9}", 99999); d9 format: 000099999
Console.WriteLine("f3 format: {0:f3}", 99999); f3 format: 99999.000
Console.WriteLine("n format: {0:n}", 99999); n format: 99,999.00

```

## Default Literal (7.1)
```c#
int myInt = default;
```
__Intrinsic datatype__ has a defaul constructor.

## Handy system.__ class members
```c#
int.Maxvalue
bool.FalseString
char.IsDigit, IsLetter, IsWhitespace...
int.Parse("8"), Char.Parse("w")... 

// Time and Date

DateTime dt = new DateTime(2015, 10, 17);
Console.WriteLine("The day of {0} is {1}", dt.Date, dt.DayOfWeek);
dt = dt.AddMonths(2);
Console.WriteLine("Daylight savings: {0}", dt.IsDaylightSavingTime());
// This constructor takes (hours, minutes, seconds).
TimeSpan ts = new TimeSpan(4, 30, 0);
Console.WriteLine(ts.Subtract(new TimeSpan(0, 15, 0))); // - 15 min

// Digit separators
Console.WriteLine(123_456.1234F); //float
Console.WriteLine(0x_00_00_FF); //hex
Console.WriteLine("Sixteen: {0}",0b_0001_0000); //binary


//// STRINGS! They're immutable

string s3 = s1 + s2;

// Insterpolation
// Using curly-bracket syntax.
string greeting = string.Format("Hello {0} you are {1} years old.", name, age);
// Using string interpolation
string greeting2 = $"Hello {name.ToUpper()} you are {age} years old.";

// The following string is printed verbatim,
// thus all escape characters are displayed.
Console.WriteLine(@"C:\MyApp\bin\Debug");
Console.WriteLine(@"Cerebus said ""Darrr! Pret-ty sun-sets"""); // use double "" for internal "

// Interpolated AND verbatim:
string myLongString2 = $@"This is a very
very
long string with {interp}";


//// System.Text.StringBuilder for larger text types? Mutable!? (efficient)
StringBuilder sb = new StringBuilder("**** Fantastic Games ****");
sb.Append("\n");
sb.AppendLine("Half Life");
Console.WriteLine(sb.ToString());
sb.Replace("2", " Invisible War");

//// Overflow protection (possible to enable on project, throw exc on overflow)
try
{
  checked
  {
    byte sum = (byte)Add(b1, b2);
    Console.WriteLine("sum = {0}", sum);
  }
}
catch (OverflowException ex)
{
  Console.WriteLine(ex.Message);
}

//unchecked to override when checking overflow is enabled.
```
## TryParse
```c#
static void ParseFromStringsWithTryParse()
{
  Console.WriteLine("=> Data type parsing with TryParse:");
  if (bool.TryParse("True", out bool b))
    {
    Console.WriteLine("Value of b: {0}", b);
    }
}
```
## Implicit declaration with numerics
- Allowed only as locals in a method or property.
- Must be initialized. 
- NOTE! Implicit is still strong typed! Not dynamic like JS.
- Primary use case is foreach loops and LINQ! Don't litter with this

```c#
var int = 4;
var myLong = 0L;
```


## Pattern matching (7.0)


```c#
static void IfElsePatternMatching()
{
Console.WriteLine("===If Else Pattern Matching ===/n");
object testItem1 = 123;
object testItem2 = "Hello";
if (testItem1 is string myStringValue1)
{
  Console.WriteLine($"{myStringValue1} is a string");
}
if (testItem1 is int myValue1)
{
  Console.WriteLine($"{myValue1} is an int");
}
```

## Switch statement
- Cannot fall through cases! (use `goto` to go to next)


```c#
switch (n)
{
  case 1:
  Console.WriteLine("Good choice, C# is a fine language.");
  break;
  default:
  break;
}

// With patter matching
switch (choice)
{
  case int i:
    ...
    break;
  case string s:
    ...
}

// With `where` clause
switch (choice)
{
  case int i when i == 2:
  break;
  case int i when i == 1:
}

// Switch expressions (8.0)

switch (colorBand)
{
  case "Red":
  return "#FF0000";
  case "Orange":
  return "#FF7F00";
  ...
}

// Becomes
return colorBand switch
{
  "Red" => "#FF0000",
  "Orange" => "#FF7F00",
  _ => "#FFFFFF",
 };
```

| -- | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-4.md)
