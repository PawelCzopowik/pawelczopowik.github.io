[<< Previous Chapter](csharp_v8-7.md) | [Content Table](../csharp) | [Next Chapter >>](csharp_v8-9.md)

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

////// WE MUST CAST?

static void Main(string[] args)
{
  Octagon oct = new Octagon();
  // We now must use casting to access the Draw() members.
  IDrawToForm itfForm = (IDrawToForm)oct;
  itfForm.Draw();
 
 // Shorthand notation if you don't need  the interface variable for later use.
  ((IDrawToPrinter)oct).Draw();
  
  // Could also use the "is" keyword.
  if (oct is IDrawToMemory dtm)
  {
    dtm.Draw();
  }

}
```

# The IEnumerable and IEnumerator Interfaces
left off page 307

```c#
// This interface informs the caller
// that the object's items can be enumerated.
public interface IEnumerable
{
  IEnumerator GetEnumerator();
}

// This interface allows the caller to
// obtain a container's items.
public interface IEnumerator
{
  bool MoveNext (); // Advance the internal position of the cursor.
  object Current { get;} // Get the current item (read-only property).
  void Reset (); // Reset the cursor before the first member.
}
```
You can simply delegate the request to the System.Array as follows
```c#
// Return the array object's IEnumerator.
public IEnumerator GetEnumerator()
  => carArray.GetEnumerator();
```

Or hide the functionality from object, but retain its function in the background (for-each).
```c#
// Return the array object's IEnumerator.
IEnumerator IEnumerable.GetEnumerator()
  => return carArray.GetEnumerator();
```

### Building Iterator Methods with the yield Keyword


```c#
public class Garage : IEnumerable
{
...
// Iterator method.
public IEnumerator GetEnumerator()
{
    foreach (Car c in carArray)
    {
      yield return c;
    }
  }
}

// This would work too with yield:
public IEnumerator GetEnumerator()
{
  yield return carArray[0];
  yield return carArray[1];
  yield return carArray[2];
  yield return carArray[3];
  }
```
### Guard Clauses with Local Functions (New 7.0)
- Calling a local sub-method to trigger the exception?
- It’s not until `MoveNext()` is called that the code will execute and the exception is thrown.
```c#
public IEnumerator GetEnumerator()
{
  //This will not get thrown until MoveNext() is called
  throw new Exception("This won't get called");
  foreach (Car c in carArray)
  {
    yield return c;
  }
}

// Update the method to this:

public IEnumerator GetEnumerator()
{
  //This will get thrown immediately
  throw new Exception("This will get called");
  return ActualImplementation();
  //this is the local function and the actual IEnumerator implementation
  IEnumerator ActualImplementation()
  {
    foreach (Car c in carArray)
    {
      yield return c;
    }
  }
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
[<< Previous Chapter](csharp_v8-7.md) | [Content Table](../csharp) | [Next Chapter >>](csharp_v8-9.md)
