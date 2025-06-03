# Complete C# Interview Questions Guide

## Section 1: Object-Oriented Programming Fundamentals

### Q1: What is OOP and What are the main concepts of OOP?

**Object-Oriented Programming (OOP)** is a programming paradigm based on the concept of "objects" which contain data (attributes) and code (methods).

**Main Concepts:**
- **Encapsulation**: Bundling data and methods together
- **Inheritance**: Creating new classes based on existing ones
- **Polymorphism**: Same interface, different implementations
- **Abstraction**: Hiding implementation details

```csharp
public class Car
{
    private string engine; // Encapsulation
    public virtual void Start() { } // Polymorphism
}

public class SportsCar : Car // Inheritance
{
    public override void Start() { } // Abstraction
}
```

### Q2: What are the advantages and limitations of OOP?

**Advantages:**
- Code reusability through inheritance
- Modularity and maintainability
- Data security through encapsulation
- Problem-solving approach mirrors real world

**Limitations:**
- Steeper learning curve
- Can be slower than procedural programming
- Requires more memory
- Over-engineering for simple problems

### Q3: What are Classes and Objects?

**Class**: A blueprint or template that defines the structure and behavior of objects.
**Object**: An instance of a class with actual values.

```csharp
// Class definition
public class Student
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public void Study() 
    { 
        Console.WriteLine($"{Name} is studying"); 
    }
}

// Object creation
Student student1 = new Student();
student1.Name = "John";
student1.Age = 20;
```

### Q4: What is Inheritance? Why is Inheritance important?

**Inheritance** allows a class to inherit properties and methods from another class, promoting code reuse.

**Importance:**
- Code reusability
- Method overriding
- Hierarchical classification
- Polymorphism support

```csharp
public class Animal
{
    public virtual void MakeSound() 
    { 
        Console.WriteLine("Animal makes sound"); 
    }
}

public class Dog : Animal
{
    public override void MakeSound() 
    { 
        Console.WriteLine("Dog barks"); 
    }
}
```

### Q5: What are the different types of Inheritance?

1. **Single Inheritance**: One base class, one derived class
2. **Multilevel Inheritance**: Chain of inheritance
3. **Hierarchical Inheritance**: Multiple classes inherit from one base class
4. **Multiple Inheritance**: Not supported in C# (use interfaces)

```csharp
// Single
class A { }
class B : A { }

// Multilevel
class A { }
class B : A { }
class C : B { }

// Hierarchical
class Animal { }
class Dog : Animal { }
class Cat : Animal { }
```

### Q6: How to prevent a class from being Inherited?

Use the `sealed` keyword to prevent inheritance.

```csharp
public sealed class FinalClass
{
    public void Method1() { }
}

// This will cause compilation error
// public class DerivedClass : FinalClass { }
```

### Q7: What is Abstraction?

**Abstraction** hides implementation details and shows only essential features to the user.

```csharp
public abstract class Shape
{
    public abstract double CalculateArea(); // Abstract method
    
    public void Display() // Concrete method
    {
        Console.WriteLine("This is a shape");
    }
}

public class Circle : Shape
{
    private double radius;
    
    public override double CalculateArea()
    {
        return Math.PI * radius * radius;
    }
}
```

### Q8: What is Encapsulation?

**Encapsulation** bundles data and methods together and restricts direct access to internal data.

```csharp
public class BankAccount
{
    private decimal balance; // Private field
    
    public decimal Balance // Public property
    {
        get { return balance; }
        private set { balance = value; }
    }
    
    public void Deposit(decimal amount)
    {
        if (amount > 0)
            balance += amount;
    }
}
```

### Q9: What is Polymorphism and what are its types?

**Polymorphism** allows objects of different types to be treated as instances of the same type.

**Types:**
1. **Compile-time (Static)**: Method Overloading, Operator Overloading
2. **Runtime (Dynamic)**: Method Overriding, Virtual Methods

```csharp
// Runtime Polymorphism
public class Animal
{
    public virtual void Speak() { }
}

public class Dog : Animal
{
    public override void Speak() 
    { 
        Console.WriteLine("Bark"); 
    }
}

// Compile-time Polymorphism
public class Calculator
{
    public int Add(int a, int b) { return a + b; }
    public double Add(double a, double b) { return a + b; }
}
```

### Q10: What is Method Overloading? In how many ways can a method be overloaded?

**Method Overloading** allows multiple methods with the same name but different parameters.

**Ways to overload:**
1. Different number of parameters
2. Different parameter types
3. Different parameter order

```csharp
public class MathOperations
{
    public int Add(int a, int b) { return a + b; }
    public int Add(int a, int b, int c) { return a + b + c; }
    public double Add(double a, double b) { return a + b; }
    public int Add(string a, int b) { return int.Parse(a) + b; }
}
```

### Q11: What is the difference between Overloading and Overriding?

| Overloading | Overriding |
|-------------|------------|
| Same method name, different parameters | Same method signature |
| Compile-time polymorphism | Runtime polymorphism |
| Within same class | Between base and derived class |
| No inheritance required | Requires inheritance |

```csharp
// Overloading
public void Print(int x) { }
public void Print(string x) { }

// Overriding
public virtual void Display() { }
public override void Display() { }
```

### Q12: What is the difference between Method Overriding and Method Hiding?

**Method Overriding**: Uses `virtual` and `override` keywords, supports polymorphism.
**Method Hiding**: Uses `new` keyword, hides base class method.

```csharp
public class Base
{
    public virtual void Method1() { }
    public void Method2() { }
}

public class Derived : Base
{
    public override void Method1() { } // Overriding
    public new void Method2() { }      // Hiding
}
```

## Section 2: Abstract Classes and Interfaces

### Q13: What is the difference between an Abstract class and an Interface?

| Abstract Class | Interface |
|----------------|-----------|
| Can have implementation | Only method signatures (C# 8+ allows default) |
| Can have fields | Cannot have fields |
| Single inheritance | Multiple inheritance |
| Can have constructors | Cannot have constructors |

```csharp
public abstract class Animal
{
    protected string name;
    public abstract void MakeSound();
    public void Sleep() { Console.WriteLine("Sleeping"); }
}

public interface IFlyable
{
    void Fly();
}
```

### Q14: When to use Interface and when Abstract class?

**Use Interface when:**
- Multiple inheritance needed
- Contract definition required
- Unrelated classes need same functionality

**Use Abstract Class when:**
- Sharing code among related classes
- Common functionality with some abstract methods
- Access modifiers other than public needed

### Q15: Why create Interfaces?

- **Contract Definition**: Ensure classes implement specific methods
- **Multiple Inheritance**: C# supports multiple interface inheritance
- **Loose Coupling**: Depend on abstractions, not concrete implementations
- **Testability**: Easy to mock for unit testing

```csharp
public interface IRepository<T>
{
    void Add(T entity);
    T GetById(int id);
    void Update(T entity);
    void Delete(int id);
}
```

### Q16: Can Interface have a Constructor?

**No**, interfaces cannot have constructors because:
- Interfaces cannot be instantiated
- Constructors initialize instance data
- Interfaces don't have instance data

### Q17: Can an Interface have private access specifier?

**No** (before C# 8), **Yes** (C# 8+) for default implementations only.

```csharp
public interface IExample
{
    void PublicMethod();
    
    // C# 8+ feature
    private void PrivateHelper() 
    { 
        Console.WriteLine("Private helper"); 
    }
}
```

### Q18: Can you create an instance of an Abstract class or Interface?

**No**, you cannot directly instantiate abstract classes or interfaces. You can only create instances of concrete derived classes.

```csharp
// This is invalid
// Animal animal = new Animal();
// IFlyable flyable = new IFlyable();

// This is valid
Dog dog = new Dog(); // Dog inherits from Animal
```

## Section 3: C# Fundamentals

### Q19: What are Access Specifiers? What is the default access modifier in a class?

**Access Specifiers:**
- `public`: Accessible everywhere
- `private`: Accessible within same class only
- `protected`: Accessible within class and derived classes
- `internal`: Accessible within same assembly
- `protected internal`: Accessible within assembly or derived classes

**Default**: `private` for class members, `internal` for classes.

```csharp
public class Example
{
    private int field1;        // Default is private
    public int field2;
    protected int field3;
    internal int field4;
}
```

### Q20: What is Boxing and Unboxing?

**Boxing**: Converting value type to reference type (object).
**Unboxing**: Converting reference type back to value type.

```csharp
// Boxing
int value = 123;
object boxed = value; // Boxing

// Unboxing
int unboxed = (int)boxed; // Unboxing

// Performance consideration
List<object> list = new List<object>();
list.Add(10); // Boxing occurs here
```

### Q21: What is the difference between "String" and "StringBuilder"?

| String | StringBuilder |
|--------|---------------|
| Immutable | Mutable |
| Creates new object for each operation | Modifies existing buffer |
| Better for few operations | Better for many operations |
| Thread-safe | Not thread-safe |

```csharp
// String - creates multiple objects
string str = "Hello";
str += " World"; // New string object created

// StringBuilder - modifies buffer
StringBuilder sb = new StringBuilder("Hello");
sb.Append(" World"); // Same object modified
```

### Q22: What are the basic string operations in C#?

```csharp
string text = "Hello World";

// Length
int length = text.Length;

// Substring
string sub = text.Substring(0, 5); // "Hello"

// Replace
string replaced = text.Replace("World", "C#");

// Split
string[] words = text.Split(' ');

// ToUpper/ToLower
string upper = text.ToUpper();
string lower = text.ToLower();

// Contains
bool contains = text.Contains("World");

// IndexOf
int index = text.IndexOf("World");
```

### Q23: What are Nullable types?

**Nullable types** allow value types to represent `null` values.

```csharp
// Nullable value types
int? nullableInt = null;
DateTime? nullableDate = null;

// Check for null
if (nullableInt.HasValue)
{
    int value = nullableInt.Value;
}

// Null coalescing operator
int result = nullableInt ?? 0;
```

### Q24: Explain Generics in C#? When and why to use them?

**Generics** enable type-safe collections and methods without boxing/unboxing.

**Benefits:**
- Type safety at compile time
- Performance (no boxing/unboxing)
- Code reusability

```csharp
// Generic class
public class Repository<T>
{
    private List<T> items = new List<T>();
    
    public void Add(T item) { items.Add(item); }
    public T Get(int index) { return items[index]; }
}

// Generic method
public T GetMax<T>(T a, T b) where T : IComparable<T>
{
    return a.CompareTo(b) > 0 ? a : b;
}

// Usage
Repository<string> stringRepo = new Repository<string>();
Repository<int> intRepo = new Repository<int>();
```

## Section 4: Exception Handling

### Q25: How to implement Exception Handling in C#?

Use `try-catch-finally` blocks to handle exceptions gracefully.

```csharp
try
{
    int result = 10 / 0; // Potential exception
}
catch (DivideByZeroException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"General error: {ex.Message}");
}
finally
{
    Console.WriteLine("This always executes");
}
```

### Q26: Can we execute multiple Catch blocks?

**No**, only one catch block executes. The first matching catch block handles the exception.

```csharp
try
{
    // Code that may throw exception
}
catch (ArgumentNullException ex) // Most specific first
{
    // Handle ArgumentNullException
}
catch (ArgumentException ex) // Less specific
{
    // Handle ArgumentException
}
catch (Exception ex) // Most general last
{
    // Handle any other exception
}
```

### Q27: What is a Finally block and when to use it?

**Finally block** always executes regardless of whether an exception occurs. Used for cleanup operations.

```csharp
FileStream file = null;
try
{
    file = new FileStream("data.txt", FileMode.Open);
    // Process file
}
catch (FileNotFoundException ex)
{
    Console.WriteLine("File not found");
}
finally
{
    file?.Close(); // Cleanup - always executes
}
```

### Q28: Can we have only "Try" block without "Catch" block?

**Yes**, but you must have a `finally` block.

```csharp
try
{
    // Some code
}
finally
{
    // Cleanup code
}
```

### Q29: What is the difference between "throw ex" and "throw"?

| throw ex | throw |
|----------|-------|
| Resets stack trace | Preserves original stack trace |
| Loses original exception information | Maintains exception context |

```csharp
try
{
    // Some operation
}
catch (Exception ex)
{
    // throw ex;  // BAD - loses stack trace
    throw;        // GOOD - preserves stack trace
}
```

## Section 5: Collections and Loops

### Q30: What are the Loop types in C#?

```csharp
// For loop
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(i);
}

// While loop
int j = 0;
while (j < 10)
{
    Console.WriteLine(j++);
}

// Do-while loop
int k = 0;
do
{
    Console.WriteLine(k++);
} while (k < 10);

// Foreach loop
int[] numbers = {1, 2, 3, 4, 5};
foreach (int num in numbers)
{
    Console.WriteLine(num);
}
```

### Q31: What is the difference between "continue" and "break" statement?

**Continue**: Skips current iteration, continues with next iteration.
**Break**: Exits the loop completely.

```csharp
for (int i = 0; i < 10; i++)
{
    if (i == 3) continue; // Skip when i is 3
    if (i == 7) break;    // Exit loop when i is 7
    Console.WriteLine(i); // Prints: 0,1,2,4,5,6
}
```

### Q32: What is the difference between Array and ArrayList?

| Array | ArrayList |
|-------|-----------|
| Fixed size | Dynamic size |
| Type-safe | Not type-safe |
| Better performance | Boxing/unboxing overhead |
| Strongly typed | Stores objects |

```csharp
// Array
int[] numbers = new int[5] {1, 2, 3, 4, 5};

// ArrayList
ArrayList list = new ArrayList();
list.Add(1);      // Boxing occurs
list.Add("text"); // Can store different types
int value = (int)list[0]; // Unboxing required
```

### Q33: What is the difference between ArrayList and Hashtable?

| ArrayList | Hashtable |
|-----------|-----------|
| Indexed collection | Key-value pairs |
| Ordered | Unordered |
| Duplicate values allowed | Unique keys required |
| Access by index | Access by key |

```csharp
// ArrayList
ArrayList list = new ArrayList();
list.Add("First");
list.Add("Second");
string item = list[0].ToString();

// Hashtable
Hashtable table = new Hashtable();
table["key1"] = "value1";
table["key2"] = "value2";
string value = table["key1"].ToString();
```

### Q34: What are Collections in C# and what are their types?

**Collections** are data structures that store and manipulate groups of objects.

**Types:**
1. **Generic Collections**: `List<T>`, `Dictionary<K,V>`, `Queue<T>`, `Stack<T>`
2. **Non-Generic Collections**: `ArrayList`, `Hashtable`, `Queue`, `Stack`
3. **Specialized Collections**: `NameValueCollection`, `StringCollection`

```csharp
// Generic collections (preferred)
List<int> numbers = new List<int> {1, 2, 3};
Dictionary<string, int> ages = new Dictionary<string, int>();
Queue<string> queue = new Queue<string>();
Stack<int> stack = new Stack<int>();
```

### Q35: What is IEnumerable in C#?

**IEnumerable** is the base interface for all collections that can be enumerated (iterated).

```csharp
public interface IEnumerable
{
    IEnumerator GetEnumerator();
}

// Usage
public void ProcessItems(IEnumerable<string> items)
{
    foreach (string item in items)
    {
        Console.WriteLine(item);
    }
}

// Can accept List, Array, etc.
ProcessItems(new List<string> {"a", "b", "c"});
ProcessItems(new string[] {"x", "y", "z"});
```

### Q36: What is the difference between IEnumerable and IQueryable?

| IEnumerable | IQueryable |
|-------------|------------|
| In-memory collections | Database queries |
| LINQ to Objects | LINQ to SQL/Entities |
| Client-side execution | Server-side execution |
| Less efficient for large datasets | More efficient for databases |

```csharp
// IEnumerable - executes in memory
IEnumerable<Student> students1 = GetStudents();
var result1 = students1.Where(s => s.Age > 18); // Executes locally

// IQueryable - executes on database
IQueryable<Student> students2 = context.Students;
var result2 = students2.Where(s => s.Age > 18); // Converts to SQL
```

## Section 6: Methods and Parameters

### Q37: What is the difference between "out" and "ref" parameters?

| ref | out |
|-----|-----|
| Must be initialized before passing | No need to initialize |
| Value can be read in method | Value must be assigned in method |
| Optional to assign new value | Must assign value before return |

```csharp
// ref parameter
public void RefExample(ref int value)
{
    Console.WriteLine(value); // Can read existing value
    value = 20; // Optional to assign
}

// out parameter
public void OutExample(out int value)
{
    // Console.WriteLine(value); // Error - cannot read
    value = 30; // Must assign before return
}

// Usage
int refValue = 10;
RefExample(ref refValue);

int outValue; // No initialization needed
OutExample(out outValue);
```

### Q38: What is the purpose of "params" keyword?

**params** allows a method to accept variable number of arguments.

```csharp
public int Sum(params int[] numbers)
{
    int total = 0;
    foreach (int num in numbers)
        total += num;
    return total;
}

// Usage - all are valid
int result1 = Sum(1, 2, 3);
int result2 = Sum(1, 2, 3, 4, 5);
int result3 = Sum(new int[] {1, 2, 3});
```

### Q39: What is a Constructor?

**Constructor** is a special method called when an object is created to initialize the object.

```csharp
public class Person
{
    private string name;
    private int age;
    
    // Default constructor
    public Person()
    {
        name = "Unknown";
        age = 0;
    }
    
    // Parameterized constructor
    public Person(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
    
    // Static constructor
    static Person()
    {
        Console.WriteLine("Static constructor called");
    }
}
```

### Q40: What are Extension Methods in C#? When to use them?

**Extension methods** allow adding new methods to existing types without modifying them.

```csharp
public static class StringExtensions
{
    public static bool IsEmail(this string input)
    {
        return input.Contains("@") && input.Contains(".");
    }
    
    public static string Reverse(this string input)
    {
        return new string(input.Reverse().ToArray());
    }
}

// Usage
string email = "test@example.com";
bool isValid = email.IsEmail(); // Extension method call
string reversed = "hello".Reverse();
```

### Q41: What is a Delegate? When to use them?

**Delegate** is a type that represents references to methods with specific signature.

```csharp
// Declare delegate
public delegate void MyDelegate(string message);
public delegate int CalculateDelegate(int a, int b);

// Methods matching delegate signature
public void ShowMessage(string msg) 
{ 
    Console.WriteLine(msg); 
}

public int Add(int x, int y) 
{ 
    return x + y; 
}

// Usage
MyDelegate del = ShowMessage;
del("Hello World");

CalculateDelegate calc = Add;
int result = calc(5, 3);
```

### Q42: What are Multicast Delegates?

**Multicast delegates** can hold references to multiple methods and invoke them all.

```csharp
public delegate void MultiDelegate(string message);

public void Method1(string msg) { Console.WriteLine($"Method1: {msg}"); }
public void Method2(string msg) { Console.WriteLine($"Method2: {msg}"); }

// Multicast delegate
MultiDelegate del = Method1;
del += Method2; // Add another method
del("Hello"); // Calls both methods

del -= Method1; // Remove a method
del("World"); // Calls only Method2
```

### Q43: What are Anonymous Delegates in C#?

**Anonymous delegates** are delegates without explicitly defined methods.

```csharp
// Anonymous delegate
Button.Click += delegate(object sender, EventArgs e)
{
    MessageBox.Show("Button clicked!");
};

// Lambda expression (preferred)
Button.Click += (sender, e) => 
{
    MessageBox.Show("Button clicked!");
};

// With delegate type
Func<int, int, int> add = delegate(int x, int y)
{
    return x + y;
};
```

### Q44: What are the differences between Events and Delegates?

| Delegates | Events |
|-----------|--------|
| Can be assigned directly | Cannot be assigned directly |
| Public access to invoke | Protected access |
| Can use = operator | Can only use += and -= |
| More flexible | More secure |

```csharp
public class Publisher
{
    // Delegate - can be assigned directly
    public Action<string> MyDelegate;
    
    // Event - cannot be assigned directly
    public event Action<string> MyEvent;
    
    public void RaiseEvent()
    {
        MyEvent?.Invoke("Event raised");
    }
}

// Usage
Publisher pub = new Publisher();
pub.MyDelegate = msg => Console.WriteLine(msg); // OK
// pub.MyEvent = msg => Console.WriteLine(msg);  // Error
pub.MyEvent += msg => Console.WriteLine(msg);   // OK
```

## Section 7: Keywords and Operators

### Q45: What is "this" keyword in C#? When to use it?

**this** refers to the current instance of the class.

**Uses:**
- Distinguish between parameters and fields
- Pass current object to other methods
- Call other constructors

```csharp
public class Person
{
    private string name;
    
    public Person(string name)
    {
        this.name = name; // Distinguish parameter from field
    }
    
    public Person() : this("Unknown") // Call other constructor
    {
    }
    
    public void SetName(string name)
    {
        this.name = name;
    }
    
    public Person GetCopy()
    {
        return this; // Return current instance
    }
}
```

### Q46: What is the purpose of "using" keyword in C#?

**using** has multiple purposes:

1. **Namespace directive**
2. **Resource disposal**
3. **Type aliases**

```csharp
// 1. Namespace directive
using System;
using System.Collections.Generic;

// 2. Resource disposal
using (FileStream file = new FileStream("data.txt", FileMode.Open))
{
    // File automatically disposed when block exits
}

// 3. Type alias
using IntList = System.Collections.Generic.List<int>;

// C# 8+ using declaration
using var file = new FileStream("data.txt", FileMode.Open);
// File disposed at end of method
```

### Q47: What is the difference between "is" and "as" operators?

| is | as |
|----|----| 
| Returns boolean | Returns object or null |
| Type checking | Type casting |
| Doesn't perform conversion | Performs safe conversion |

```csharp
object obj = "Hello World";

// is operator
if (obj is string)
{
    string str = (string)obj; // Safe cast
}

// as operator
string str2 = obj as string; // Returns string or null
if (str2 != null)
{
    Console.WriteLine(str2);
}

// Pattern matching (C# 7+)
if (obj is string str3)
{
    Console.WriteLine(str3); // str3 is available here
}
```

### Q48: What is the difference between "Readonly" and "Constant" variables?

| const | readonly |
|-------|----------|
| Compile-time constant | Runtime constant |
| Must be initialized at declaration | Can be initialized in constructor |
| Static by default | Can be instance or static |
| Value types and strings only | Any type |

```csharp
public class Example
{
    // const - compile time constant
    public const int MAX_SIZE = 100;
    public const string APP_NAME = "MyApp";
    
    // readonly - runtime constant
    public readonly int MinSize;
    public readonly DateTime CreatedDate;
    public static readonly string ConfigPath;
    
    public Example(int minSize)
    {
        MinSize = minSize; // Can initialize in constructor
        CreatedDate = DateTime.Now; // Runtime value
    }
    
    static Example()
    {
        ConfigPath = GetConfigPath(); // Can call method
    }
}
```

### Q49: What is "Static" class? When to use it?

**Static class** cannot be instantiated and contains only static members.

**Characteristics:**
- Cannot be instantiated
- Cannot be inherited
- All members must be static
- Sealed by default

```csharp
public static class MathUtility
{
    public static double Pi = 3.14159;
    
    public static double CalculateArea(double radius)
    {
        return Pi * radius * radius;
    }
    
    public static int Add(int a, int b)
    {
        return a + b;
    }
}

// Usage - no instantiation needed
double area = MathUtility.CalculateArea(5);
int sum = MathUtility.Add(10, 20);
```

### Q50: What is the difference between "var" and "dynamic" in C#?

| var | dynamic |
|-----|---------|
| Compile-time type inference | Runtime type resolution |
| Type determined at compilation | Type determined at runtime |
| IntelliSense support | Limited IntelliSense |
| Type safety | No compile-time type checking |

```csharp
// var - compile-time type inference
var number = 10;        // Inferred as int
var text = "Hello";     // Inferred as string
var list = new List<int>(); // Inferred as List<int>

// number = "text"; // Compile error - type is fixed

// dynamic - runtime type resolution
dynamic dynVar = 10;
Console.WriteLine(dynVar); // 10

dynVar = "Hello";
Console.WriteLine(dynVar); // Hello

dynVar = new List<string>();
dynVar.Add("Item"); // No compile-time checking

// Runtime error possible
try
{
    dynVar.NonExistentMethod(); // Runtime error
}
catch (RuntimeBinderException ex)
{
    Console.WriteLine("Method not found");
}
```

---

## Summary

This guide covers all 50 essential C# interview questions with clear explanations and practical code examples. Key areas include:

- **OOP Concepts**: Inheritance, Polymorphism, Encapsulation, Abstraction
- **Advanced Topics**: Generics, Delegates, Events, Extension Methods
- **Collections**: Arrays, Lists, Dictionaries, IEnumerable vs IQueryable
- **Exception Handling**: Try-catch-finally blocks, custom exceptions
- **Language Features**: var vs dynamic, const vs readonly, boxing/unboxing

Remember to practice these concepts with hands-on coding to solidify your understanding!
