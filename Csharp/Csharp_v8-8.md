[<< Previous Chapter](Csharp_v8-7.md) | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-9.md)

CHAPTER 8 Working with Interfaces
===
- Basically allows multiple inheritance
- Interface expresses *behavior*
- Type that defines behavior but no implementation
- Regarding abstract classes There are only two real differences: interfaces can’t have nonstatic constructors, and a class can implement multiple interfaces.

### Obtaining Interface References: The as Keyword
- You can determine whether a given type supports an interface by using the as keyword

```c#
static void Main(string[] args)
{
  ...
  // Can we treat hex2 as IPointy?
  Hexagon hex2 = new Hexagon("Peter");
  IPointy itfPt2 = hex2 as IPointy;
  if(itfPt2 != null)
  {
    Console.WriteLine("Points: {0}", itfPt2.Points);
  }
  else
  {
    Console.WriteLine("OOPS! Not pointy...");
  }
  Console.ReadLine();
}
```
## Obtaining Interface References: The is Keyword (Updated 7.0)
- But of course we can get a true/false via is

```c#
static void Main(string[] args)
{
  Console.WriteLine("***** Fun with Interfaces *****\n");
  ...
  if(hex2 is IPointy itfPt3)
  {
    Console.WriteLine("Points: {0}", itfPt3.Points);
  }
  else
  {
    Console.WriteLine("OOPS! Not pointy...");
  }
  Console.ReadLine();
}
```
## Default Implementations (New 8.0)
- C# 8.0 added the ability for interface methods and properties to have a default implementation.
- Perimiter won't be accessible via object, it will need to be explicitly cast to the interface

```c#
interface IRegularPointy : IPointy
{
  int SideLength { get; set; }
  int NumberOfSides { get; set; }
  int Perimeter => SideLength * NumberOfSides;
}
```

To get around this problem, **code to the interface**

```c#
IRegularPointy sq = new Square("Boxy") {NumberOfSides = 4, SideLength = 4};
```

## Interface as parameters (and return values)

```c#
// I'll draw anyone supporting IDraw3D.
static void DrawIn3D(IDraw3D itf3d)
{
  Console.WriteLine("-> Drawing IDraw3D compatible type");
  itf3d.Draw3D();
}
```
## Explicit Interface Implementation
- If you have a name clash inheriting many interfaces with same named methods, qualify them
- Otherwise all interfaces will be able to call the same-named function
- Explicitly implemented members are automatically private.

```c#
returnType InterfaceName.MethodName(params){}

class Octagon : IDrawToForm, IDrawToMemory, IDrawToPrinter
{
  // Explicitly bind Draw() implementations
  // to a given interface.
  void IDrawToForm.Draw()
  {
    Console.WriteLine("Drawing to form...");
  }
  
  void IDrawToMemory.Draw()
  {
    Console.WriteLine("Drawing to memory...");
  }
  
  void IDrawToPrinter.Draw()
  {
    Console.WriteLine("Drawing to a printer...");
  }
}
```

```c#

```
[<< Previous Chapter](Csharp_v8-7.md) | [Content Table](../Csharp) | [Next Chapter >>](Csharp_v8-9.md)
