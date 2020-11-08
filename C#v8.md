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
