# Java In-Depth Interview Questions - Core, OOP, Memory & Advanced Concepts

## 1. String is a reference type in Java, but why does it behave like a primitive sometimes?

**Answer:** String is indeed a reference type, but it appears to behave like a primitive due to several special features:

```java
public class StringBehavior {
    public static void main(String[] args) {
        // String literals go to String Pool (Heap memory)
        String s1 = "Hello";  // Reference to string pool
        String s2 = "Hello";  // Same reference to string pool
        
        System.out.println(s1 == s2);        // true (same reference)
        System.out.println(s1.equals(s2));   // true (same content)
        
        // new String() creates object in heap (outside pool)
        String s3 = new String("Hello");
        System.out.println(s1 == s3);        // false (different references)
        System.out.println(s1.equals(s3));   // true (same content)
        
        // String concatenation creates new objects
        String s4 = s1 + " World";  // New object created
        System.out.println(s1);     // Still "Hello" (immutable)
    }
}
```

**Why it seems like primitive:**
- **String Pool**: Literal strings are cached for memory efficiency
- **Immutability**: Cannot be modified after creation
- **Special syntax**: Can use `=` operator and `+` for concatenation
- **Compiler optimization**: Compile-time string concatenation

## 2. Explain memory allocation for objects in Java. Where exactly are objects stored?

**Answer:**

```java
public class MemoryAllocation {
    static int staticVar = 100;        // Method Area (Metaspace in Java 8+)
    int instanceVar = 50;              // Heap memory
    
    public void method() {
        int localVar = 10;             // Stack memory
        String str = "Hello";          // Reference in stack, object in heap (string pool)
        Object obj = new Object();     // Reference in stack, object in heap
        
        // Array allocation
        int[] arr = new int[5];        // Reference in stack, array in heap
    }
    
    public static void main(String[] args) {
        MemoryAllocation obj1 = new MemoryAllocation();  // Heap
        MemoryAllocation obj2 = new MemoryAllocation();  // Heap (separate object)
    }
}
```

**Memory Layout:**
- **Stack**: Local variables, method parameters, method call frames
- **Heap**: Objects, instance variables, arrays
  - **Young Generation**: Eden space, Survivor spaces (S0, S1)
  - **Old Generation**: Long-lived objects
- **Method Area/Metaspace**: Class metadata, static variables, constant pool
- **PC Register**: Current executing instruction
- **Native Method Stack**: Native method calls

## 3. Why is multiple inheritance not supported in Java through classes but allowed through interfaces?

**Answer:**

```java
// Diamond Problem with classes (not allowed)
/*
class A {
    void method() { System.out.println("A"); }
}

class B extends A {
    void method() { System.out.println("B"); }
}

class C extends A {
    void method() { System.out.println("C"); }
}

// This would cause ambiguity - which method() to inherit?
class D extends B, C { } // NOT ALLOWED
*/

// Solution with interfaces
interface Printable {
    default void print() {
        System.out.println("Printable");
    }
}

interface Drawable {
    default void print() {  // Same method signature
        System.out.println("Drawable");
    }
}

class Document implements Printable, Drawable {
    // Must override to resolve conflict
    @Override
    public void print() {
        System.out.println("Document print");
        // Can call specific interface methods
        Printable.super.print();
        Drawable.super.print();
    }
}

public class MultipleInheritance {
    public static void main(String[] args) {
        Document doc = new Document();
        doc.print();
        // Output:
        // Document print
        // Printable
        // Drawable
    }
}
```

**Reasons:**
- **Diamond Problem**: Ambiguity in method resolution
- **Implementation vs Contract**: Interfaces define contracts, not implementation (before Java 8)
- **Explicit Resolution**: With interfaces, conflicts must be explicitly resolved

## 4. What happens in memory when you create an object? Explain the complete object creation process.

**Answer:**

```java
public class ObjectCreation {
    static int staticCounter = 0;
    
    {
        // Instance initializer block
        System.out.println("Instance block executed");
    }
    
    static {
        // Static initializer block
        System.out.println("Static block executed");
        staticCounter = 10;
    }
    
    private int value;
    
    public ObjectCreation() {
        System.out.println("Constructor called");
        this.value = 20;
    }
    
    public ObjectCreation(int value) {
        this();  // Calls default constructor
        this.value = value;
        System.out.println("Parameterized constructor called");
    }
    
    public static void main(String[] args) {
        System.out.println("Main method starts");
        ObjectCreation obj1 = new ObjectCreation();
        ObjectCreation obj2 = new ObjectCreation(50);
    }
}
```

**Object Creation Process:**
1. **Class Loading**: JVM loads class into method area
2. **Memory Allocation**: JVM allocates memory in heap
3. **Default Values**: All instance variables get default values
4. **Constructor Chain**: super() called implicitly if not explicit
5. **Instance Initializers**: Instance blocks and variable initializers execute
6. **Constructor Body**: Constructor code executes
7. **Reference Return**: Reference to created object returned

**Output:**
```
Static block executed
Main method starts
Instance block executed
Constructor called
Instance block executed
Constructor called
Parameterized constructor called
```

## 5. Explain the difference between == and equals() method. Why does String behave differently?

**Answer:**

```java
public class EqualsVsDoubleEquals {
    public static void main(String[] args) {
        // Primitive comparison
        int a = 5, b = 5;
        System.out.println(a == b);  // true (value comparison)
        
        // Object comparison
        String s1 = new String("Hello");
        String s2 = new String("Hello");
        String s3 = "Hello";
        String s4 = "Hello";
        
        System.out.println("=== Reference Comparison (==) ===");
        System.out.println(s1 == s2);    // false (different objects in heap)
        System.out.println(s3 == s4);    // true (same object in string pool)
        System.out.println(s1 == s3);    // false (heap vs string pool)
        
        System.out.println("=== Content Comparison (equals) ===");
        System.out.println(s1.equals(s2));  // true (same content)
        System.out.println(s1.equals(s3));  // true (same content)
        
        // Custom class example
        Person p1 = new Person("John", 25);
        Person p2 = new Person("John", 25);
        
        System.out.println("=== Custom Object Comparison ===");
        System.out.println(p1 == p2);        // false (different objects)
        System.out.println(p1.equals(p2));   // true (if equals overridden)
    }
}

class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        
        Person person = (Person) obj;
        return age == person.age && 
               Objects.equals(name, person.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

**Key Differences:**
- `==` compares references for objects, values for primitives
- `equals()` compares content (if properly overridden)
- String pool makes string comparison tricky

## 6. Why is it recommended to override hashCode() when you override equals()?

**Answer:**

```java
import java.util.*;

class Student {
    private String name;
    private int id;
    
    public Student(String name, int id) {
        this.name = name;
        this.id = id;
    }
    
    // Only equals overridden, hashCode not overridden
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        
        Student student = (Student) obj;
        return id == student.id && Objects.equals(name, student.name);
    }
    
    // If hashCode is not overridden, it will cause issues
    /*
    @Override
    public int hashCode() {
        return Objects.hash(name, id);
    }
    */
    
    @Override
    public String toString() {
        return "Student{name='" + name + "', id=" + id + "}";
    }
}

public class HashCodeImportance {
    public static void main(String[] args) {
        Student s1 = new Student("John", 123);
        Student s2 = new Student("John", 123);
        
        System.out.println(s1.equals(s2));  // true
        System.out.println(s1.hashCode() == s2.hashCode());  // false (problem!)
        
        // HashSet/HashMap issues
        Set<Student> set = new HashSet<>();
        set.add(s1);
        System.out.println(set.contains(s2));  // false (should be true!)
    }
}
```

**Hash Code Contract:**
- If `a.equals(b)` is true, then `a.hashCode() == b.hashCode()` must be true
- If `a.equals(b)` is false, hashCodes can be same or different
- Objects used as keys in HashMap/HashSet must follow this contract

## 7. What is the difference between abstract class and interface? When to use which?

**Answer:**

```java
// Abstract class - partial implementation
abstract class Animal {
    protected String name;  // Can have state
    
    public Animal(String name) {  // Can have constructor
        this.name = name;
    }
    
    // Concrete method
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    // Abstract method - must implement
    public abstract void makeSound();
}

// Interface - contract definition
interface Flyable {
    int MAX_HEIGHT = 1000;  // public static final by default
    
    void fly();  // public abstract by default
    
    // Default method (Java 8+)
    default void land() {
        System.out.println("Landing safely");
    }
    
    // Static method (Java 8+)
    static void checkWeather() {
        System.out.println("Weather is good for flying");
    }
}

class Bird extends Animal implements Flyable {
    public Bird(String name) { super(name); }
    
    @Override
    public void makeSound() { System.out.println("Tweet"); }
    
    @Override
    public void fly() { System.out.println(name + " is flying"); }
}
```

**Abstract Class vs Interface:**

| Abstract Class | Interface |
|----------------|-----------|
| Can have constructors | No constructors |
| Can have instance variables | Only constants (public static final) |
| Can have concrete methods | Only abstract/default/static methods |
| Single inheritance | Multiple inheritance |
| Use when: IS-A relationship | Use when: CAN-DO capability |

## 8. Why are wrapper classes immutable in Java? What happens in memory?

**Answer:**

```java
public class WrapperImmutability {
    public static void main(String[] args) {
        Integer a = 100;
        Integer b = 100;
        Integer c = 200;
        Integer d = 200;
        
        System.out.println(a == b);  // true (Integer cache)
        System.out.println(c == d);  // false (outside cache range)
        
        // Autoboxing demonstration
        Integer x = 50;  // Integer.valueOf(50)
        x = x + 1;       // x.intValue() + 1, then Integer.valueOf(51)
    }
}
```

**Integer Cache Range:** -128 to 127 are cached for memory optimization

**Why Immutable:**
- **Thread Safety**: Multiple threads can safely access without synchronization
- **Hash Consistency**: hashCode never changes, safe for HashMap keys
- **Memory Efficiency**: String pool-like caching possible
- **Predictable Behavior**: No unexpected mutations

## 9. Explain method overloading vs method overriding with memory perspective.

**Answer:**

```java
class Parent {
    // Method overloading - same class, different parameters
    public void display() { System.out.println("Parent display()"); }
    public void display(String msg) { System.out.println("Parent: " + msg); }
    
    // Virtual method - can be overridden
    public void virtualMethod() { System.out.println("Parent virtual"); }
}

class Child extends Parent {
    // Method overriding - runtime polymorphism
    @Override
    public void virtualMethod() { System.out.println("Child virtual"); }
    
    // Method overloading in child class
    public void display(int num) { System.out.println("Child: " + num); }
}
```

**Overloading (Compile-time):**
- **Method Resolution**: Determined at compile time
- **Memory**: Each overloaded method has separate entry in method area
- **Performance**: No runtime overhead

**Overriding (Runtime):**
- **Dynamic Method Dispatch**: Actual method determined at runtime using vtable
- **Memory**: Virtual method table (vtable) stores method references
- **Performance**: Slight runtime overhead for method lookup

## 10. What is the difference between final, finally, and finalize?

**Answer:**

```java
class FinalExample {
    final int CONSTANT = 10;        // Cannot reassign
    final List<String> list = new ArrayList<>();  // Reference final, content mutable
    
    final void method() {}          // Cannot override
}

final class FinalClass {}          // Cannot extend

public class FinalFinallyFinalize {
    public static void main(String[] args) {
        try {
            // Some code that may throw exception
            int result = 10 / 0;
        } catch (Exception e) {
            System.out.println("Exception caught");
            return;  // finally still executes
        } finally {
            System.out.println("Finally always executes");
        }
    }
    
    @Override
    protected void finalize() throws Throwable {
        // Called by GC before object destruction
        System.out.println("Object being garbage collected");
        super.finalize();
    }
}
```

**Definitions:**
- **final**: Keyword for constants, preventing inheritance/overriding
- **finally**: Block that always executes (except System.exit())
- **finalize()**: Method called by garbage collector (deprecated in Java 9+)

## 11. Explain the concept of pass-by-value in Java with objects.

**Answer:**

```java
class Person {
    String name;
    Person(String name) { this.name = name; }
}

public class PassByValue {
    public static void modifyPrimitive(int x) {
        x = 100;  // Only local copy modified
    }
    
    public static void modifyObject(Person p) {
        p.name = "Modified";  // Object state changed
    }
    
    public static void reassignObject(Person p) {
        p = new Person("New Person");  // Only local reference changed
    }
    
    public static void main(String[] args) {
        int num = 10;
        modifyPrimitive(num);
        System.out.println(num);  // 10 (unchanged)
        
        Person person = new Person("Original");
        modifyObject(person);
        System.out.println(person.name);  // "Modified"
        
        reassignObject(person);
        System.out.println(person.name);  // "Modified" (not "New Person")
    }
}
```

**Key Concept:** Java is always pass-by-value
- **Primitives**: Value is copied
- **Objects**: Reference value is copied (not the object itself)

## 12. What is the difference between heap and stack memory? What causes StackOverflowError and OutOfMemoryError?

**Answer:**

```java
public class MemoryErrors {
    // Causes StackOverflowError
    public static void infiniteRecursion() {
        infiniteRecursion();  // No base case
    }
    
    // Causes OutOfMemoryError (Heap)
    public static void heapOverflow() {
        List<int[]> list = new ArrayList<>();
        while (true) {
            list.add(new int[1000000]);  // Keep creating large arrays
        }
    }
    
    // Stack memory demonstration
    public static void deepRecursion(int depth) {
        if (depth > 0) {
            int[] localArray = new int[1000];  // Stack frame data
            deepRecursion(depth - 1);
        }
    }
}
```

**Stack Memory:**
- **Stores**: Method calls, local variables, partial results
- **Characteristics**: LIFO, thread-specific, automatic cleanup
- **Error**: StackOverflowError when stack space exhausted

**Heap Memory:**
- **Stores**: Objects, instance variables, arrays
- **Characteristics**: Shared among threads, managed by GC
- **Error**: OutOfMemoryError when heap space exhausted

## 13. Explain static binding vs dynamic binding with examples.

**Answer:**

```java
class Animal {
    static void staticMethod() { System.out.println("Animal static"); }
    void instanceMethod() { System.out.println("Animal instance"); }
}

class Dog extends Animal {
    static void staticMethod() { System.out.println("Dog static"); }  // Hiding
    @Override
    void instanceMethod() { System.out.println("Dog instance"); }   // Overriding
}

public class BindingExample {
    public static void main(String[] args) {
        Animal animal = new Dog();
        
        // Static binding (compile-time)
        animal.staticMethod();    // "Animal static" - based on reference type
        
        // Dynamic binding (runtime)
        animal.instanceMethod();  // "Dog instance" - based on actual object
    }
}
```

**Static Binding (Early Binding):**
- **When**: Compile time
- **Methods**: static, private, final methods
- **Resolution**: Based on reference type

**Dynamic Binding (Late Binding):**
- **When**: Runtime
- **Methods**: Overridden instance methods
- **Resolution**: Based on actual object type using vtable

## 14. What is the difference between composition and inheritance? Which is better and why?

**Answer:**

```java
// Inheritance - IS-A relationship
class Vehicle {
    protected String brand;
    public void start() { System.out.println("Vehicle started"); }
}

class Car extends Vehicle {  // Car IS-A Vehicle
    public void drive() { System.out.println("Car driving"); }
}

// Composition - HAS-A relationship
class Engine {
    public void start() { System.out.println("Engine started"); }
}

class Wheel {
    public void rotate() { System.out.println("Wheel rotating"); }
}

class CarComposition {  // Car HAS-A Engine, HAS-A Wheels
    private Engine engine = new Engine();
    private Wheel[] wheels = new Wheel[4];
    
    public void start() {
        engine.start();  // Delegation
    }
}
```

**Composition vs Inheritance:**

| Composition | Inheritance |
|-------------|-------------|
| HAS-A relationship | IS-A relationship |
| Loose coupling | Tight coupling |
| Runtime flexibility | Compile-time binding |
| Can change behavior | Fixed behavior |
| Multiple inheritance possible | Single inheritance only |

**Why Composition is Often Better:**
- **Flexibility**: Can change implementation at runtime
- **Loose Coupling**: Changes in one class don't affect others
- **Testing**: Easier to mock dependencies
- **Design**: Follows "favor composition over inheritance" principle

## 15. Explain garbage collection in Java. What are different GC algorithms?

**Answer:**

```java
public class GarbageCollection {
    public static void main(String[] args) {
        // Creating objects
        String s1 = new String("Hello");  // Heap allocation
        String s2 = new String("World");  // Heap allocation
        
        s1 = null;  // Now eligible for GC
        s2 = null;  // Now eligible for GC
        
        // Requesting GC (not guaranteed)
        System.gc();  // Runtime.getRuntime().gc()
        
        // Creating unreachable objects
        createUnreachableObjects();
        
        // Memory information
        Runtime runtime = Runtime.getRuntime();
        System.out.println("Total memory: " + runtime.totalMemory());
        System.out.println("Free memory: " + runtime.freeMemory());
        System.out.println("Used memory: " + (runtime.totalMemory() - runtime.freeMemory()));
    }
    
    static void createUnreachableObjects() {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 1000; i++) {
            list.add("String " + i);
        }
        // list becomes unreachable when method ends
    }
}
```

**GC Algorithms:**

1. **Serial GC**: Single-threaded, suitable for small applications
2. **Parallel GC**: Multi-threaded, default for server applications
3. **G1 GC**: Low-latency collector for large heaps
4. **ZGC/Shenandoah**: Ultra-low latency collectors

**GC Process:**
- **Mark**: Identify reachable objects
- **Sweep**: Remove unreachable objects
- **Compact**: Defragment memory

## 16. What is the difference between String, StringBuffer, and StringBuilder?

**Answer:**

```java
public class StringComparison {
    public static void main(String[] args) {
        // String - Immutable
        String str = "Hello";
        str += " World";  // Creates new object, old "Hello" eligible for GC
        
        // StringBuffer - Mutable, Synchronized
        StringBuffer sb = new StringBuffer("Hello");
        sb.append(" World");  // Modifies existing buffer
        
        // StringBuilder - Mutable, Not Synchronized
        StringBuilder sbr = new StringBuilder("Hello");
        sbr.append(" World");  // Modifies existing buffer
        
        // Performance test
        performanceTest();
    }
    
    static void performanceTest() {
        long start = System.currentTimeMillis();
        
        String s = "";
        for (int i = 0; i < 10000; i++) {
            s += i;  // Creates 10000 new objects
        }
        
        System.out.println("String concatenation: " + 
                          (System.currentTimeMillis() - start) + "ms");
    }
}
```

**Comparison:**

| Feature | String | StringBuffer | StringBuilder |
|---------|--------|--------------|---------------|
| Mutability | Immutable | Mutable | Mutable |
| Thread Safety | N/A | Synchronized | Not Synchronized |
| Performance | Slow (creates new objects) | Medium | Fast |
| Memory | High (many objects) | Low | Low |

## 17. Explain exception hierarchy in Java. Difference between checked and unchecked exceptions?

**Answer:**

```java
// Custom checked exception
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

// Custom unchecked exception
class InvalidAccountException extends RuntimeException {
    public InvalidAccountException(String message) {
        super(message);
    }
}

public class ExceptionHierarchy {
    // Method declaring checked exception
    public static void validateAge(int age) throws InvalidAgeException {
        if (age < 0) {
            throw new InvalidAgeException("Age cannot be negative");
        }
    }
    
    // Method with unchecked exception
    public static void validateAccount(String account) {
        if (account == null) {
            throw new InvalidAccountException("Account cannot be null");
        }
    }
}
```

**Exception Hierarchy:**
```
Throwable
├── Error (Unchecked)
│   ├── OutOfMemoryError
│   └── StackOverflowError
└── Exception
    ├── RuntimeException (Unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   └── IllegalArgumentException
    └── Checked Exceptions
        ├── IOException
        ├── SQLException
        └── ClassNotFoundException
```

**Checked vs Unchecked:**
- **Checked**: Must be declared or handled, checked at compile time
- **Unchecked**: Runtime exceptions, not required to be declared

## 18. What happens when you call System.exit() in a finally block?

**Answer:**

```java
public class SystemExitInFinally {
    public static void main(String[] args) {
        try {
            System.out.println("In try block");
            throw new RuntimeException("Exception");
        } catch (Exception e) {
            System.out.println("In catch block");
        } finally {
            System.out.println("In finally block");
            System.exit(0);  // Terminates JVM
            System.out.println("This will never execute");
        }
        
        System.out.println("This will never execute either");
    }
}
```

**System.exit() Behavior:**
- **JVM Termination**: Immediately terminates the JVM
- **Finally Override**: Only scenario where finally doesn't complete
- **Shutdown Hooks**: Still execute if registered
- **Resource Cleanup**: May not complete properly

## 19. Explain the concept of method overriding rules and covariant return types.

**Answer:**

```java
class Animal {
    public Animal reproduce() {  // Return type: Animal
        return new Animal();
    }
    
    public void makeSound() throws Exception {  // Can throw checked exception
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    public Dog reproduce() {  // Covariant return type (Dog is subtype of Animal)
        return new Dog();
    }
    
    @Override
    public void makeSound() {  // Cannot throw broader exception than parent
        System.out.println("Woof!");
        // Cannot throw Exception here if parent throws IOException
    }
}
```

**Method Overriding Rules:**
1. **Same method signature** (name, parameters)
2. **Return type**: Same or covariant (subtype)
3. **Access modifier**: Same or more accessible
4. **Exceptions**: Same or more specific checked exceptions
5. **final, static, private methods**: Cannot be overridden

**Covariant Return Types:**
- Child class can return more specific type
- Introduced in Java 5
- Must be subtype of parent's return type

## 20. What is the difference between static and instance initialization blocks?

**Answer:**

```java
public class InitializationBlocks {
    static int staticVar;
    int instanceVar;
    
    // Static initialization block - executed once when class is loaded
    static {
        System.out.println("Static block 1");
        staticVar = 10;
    }
    
    // Instance initialization block - executed every time object is created
    {
        System.out.println("Instance block 1");
        instanceVar = 20;
    }
    
    static {
        System.out.println("Static block 2");  // Multiple static blocks allowed
    }
    
    {
        System.out.println("Instance block 2");  // Multiple instance blocks allowed
    }
    
    public InitializationBlocks() {
        System.out.println("Constructor");
    }
    
    public static void main(String[] args) {
        System.out.println("Creating first object");
        new InitializationBlocks();
        
        System.out.println("Creating second object");
        new InitializationBlocks();
    }
}
```

**Output:**
```
Static block 1
Static block 2
Creating first object
Instance block 1
Instance block 2
Constructor
Creating second object
Instance block 1
Instance block 2
Constructor
```

**Execution Order:**
1. Static blocks (when class is first loaded)
2. Instance blocks (before constructor, every object creation)
3. Constructor

---

This covers the depth questions focusing on core Java concepts, OOP principles, memory management, and tricky scenarios that test deep understanding rather than surface-level knowledge.
