[<< Previous Chapter](csharp_v8-5.md) | [Content Table](../csharp) | [Next Chapter >>](csharp_v8-7.md)

# CHAPTER 6 Understanding Inheritance and Polymorphism

```c#
// MiniVan "is-a" Car.
class MiniVan : Car
{
}
```
## The Basic Mechanics of Inheritance
### The `sealed` Keyword
- prevents inheritance from occurring

## The Details of Inheritance
### Controlling Base Class Creation with the `base` Keyword

```c#
// As a general rule, all subclasses should explicitly call an appropriate
public Manager(string fullName, int age, int empId,
  float currPay, string ssn, int numbOfOpts)
  : base(fullName, age, empId, currPay, ssn,
EmployeePayTypeEnum.Salaried)
{
  // This property is defined by the Manager class.
  StockOptions = numbOfOpts;
}
```
### The `protected` Keyword
 - The benefit of defining protected members in a base class is that derived types no longer have to access the data indirectly using public methods or properties

### Understanding Nested Type Definitions
- Nested types allow you to gain complete control over the access level of the inner type because they may be declared privately (recall that non-nested classes cannot be declared using the private keyword).
- Because a nested type is a member of the containing class, it can access private members of the containing class.
- Often, a nested type is useful only as a helper for the outer class and is not intended for use by the outside world.


## C#’s Polymorphic Support

### The `virtual` and `override` Keywords
- `virtual` allows method to be overriden
- `override` does the overriding
- A method can be `sealed` to prevent further overriding

### Understanding Abstract Classes
- Cannot be instantiated, used as a common base for other classes

### Understanding the Polymorphic Interface
- Set of virtual and abstract methods
- **Abstract Members** do not supply a definition but must be implemented in derrived classes.

### Understanding Member Shadowing
- Using `new` to ignore inherited function of the same name 
- Can be applied to any member

```c#
// This class extends Circle and hides the inherited Draw() method.
class ThreeDCircle : Circle
{
  // Hide the PetName property above me.
  public new string PetName { get; set; }
  // Hide any Draw() implementation above me.
  public new void Draw()
  {
    Console.WriteLine("Drawing a 3D Circle");
  }
}

// and we can cast to call the parent:
((Circle)o).Draw();
```

## Understanding Base Class/Derived Class Casting Rules
- *implicit cast* - The first law of casting between class types is that when two classes are related by an “is-a” relationship, it is always safe to store a derived object within a base class reference.
- Section goes over casting - implicit and explicit. If you're too high in the hierarchy (object) then do explicit cast...

### The C# `as` Keyword
- C# provides the `as` keyword to quickly determine at runtime whether a given type is compatible with
another.


```c#
// Use "as" to test compatibility.
object[] things = new object[4];
things[0] = new Hexagon();
things[1] = false;
things[2] = new Manager();
things[3] = "Last thing";
foreach (object item in things)
{
  Hexagon h = item as Hexagon;
  if (h == null)
  {
    Console.WriteLine("Item is not a hexagon");
  }
  else
  {
    h.Draw();
  }
}
```
### The C# `is` Keyword (Updated 7.0)
- The is keyword to determine whether two items are compatible. Returns `false` if not compatible (instead of `null`).


```c#
static void GivePromotion(Employee emp)
{
  Console.WriteLine("{0} was promoted!", emp.Name);
  if (emp is SalesPerson)
  {
    Console.WriteLine("{0} made {1} sale(s)!", emp.Name,
      ((SalesPerson)emp).SalesNumber);
    Console.WriteLine();
  }
  else if (emp is Manager)
  {
    Console.WriteLine("{0} had {1} stock options...", emp.Name,
      ((Manager)emp).StockOptions);
    Console.WriteLine();
  }
}
```
- Can also assign the converted type to a variable if the cast works

```c#
static void GivePromotion(Employee emp)
{
  Console.WriteLine("{0} was promoted!", emp.Name);
  //Check if is SalesPerson, assign to variable s
  if (emp is SalesPerson s)
  {
    Console.WriteLine("{0} made {1} sale(s)!", s.Name,
      s.SalesNumber);
    Console.WriteLine();
  }
  //Check if is Manager, if it is, assign to variable m
  else if (emp is Manager m)
  {
    Console.WriteLine("{0} had {1} stock options...",
      m.Name, m.StockOptions);
    Console.WriteLine();
  }
}
```

### Discards with the is Keyword (New 7.0)
- Can create a catch-all

```c#
else if (emp is var _)
{
  Console.WriteLine("Unable to promote {0}. Wrong employee type", emp.Name);
  Console.WriteLine();
}
```

[<< Previous Chapter](csharp_v8-5.md) | [Content Table](../csharp) | [Next Chapter >>](csharp_v8-7.md)
