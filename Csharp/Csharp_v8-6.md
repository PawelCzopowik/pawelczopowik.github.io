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
public Manager(string fullName, int age, int empId,
  float currPay, string ssn, int numbOfOpts)
  : base(fullName, age, empId, currPay, ssn,
EmployeePayTypeEnum.Salaried)
{
  // This property is defined by the Manager class.
  StockOptions = numbOfOpts;
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
