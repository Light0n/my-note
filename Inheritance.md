# Inheritance

## Base Class
* or **Superclass**, **Parent class**

## Derived Class
* or **Subclass**, **Child Class**
* has **is a(n)** relationship with base class. Ex: Dog (subclass) is a Animal (superclass).

## Containment (Class inside class)
* Composition: members should not exist without the object that contains them
    * Ex: a **Business** has an array of many **Department** objects. If the **Business** is deleted, an array of **Department** should deleted also.
* Aggreation: members should continue to exist without the object that contains them
    * Ex: **Department** object might contain an array of **Employee** objects. If the **Department** is closed, the employees would continue to exist.

## Inheritance in Java
* **extends** keyword: use to inherit methods and attributes of a superclass. Ex: `class Subclass extends Superclass{}`
* **instanceof** operator: use to check whether or not an object belong to a class. Ex: `object instanceof Class;` return `true` if **object** can be upcast to the **Class** 
* Every class in Java is a subclass of **Object**
* When you instantiate **an object** of **a subclass**. The superclass constructor is called before the subclass constructor. Ex: **Dog** inherit from **Animal**, `Dog dog = new Dog();`, then constructor called orders is Object() => Animal() => Dog() 
* **super**: to access parent methods 
* **public**: can be accessed by all classes in the same package
* **protected**: can be accessed only by the subclasses 
* **private**: cannot be accessed outside of the class 
* Subclasses inherit all data and methods of the superclass except private members of the superclass
* **final** classes cannot have subclass
* **abstract** classes cannot create object, just use as a superclass
    * **Nonabstract methods** are implemented in the abstract class and are inherited by its children
    * **Abstract methods** have nobody and must be implemented in child classes

Superclass constructors|Must have a Subclass constructor
---|:---:
None|No
Default constructor|No
Only nondefault constructors|Yes, must contain a constructor that calls the superclass constructor **at the first statement, before any comment** 

```Java
class Superclass{
    private int a;
    private double b;
    private String c;

    //The only Supperclass nondefault constructor
    public Superclass(int a, double b, String c){
        this.a = a; this.b = b; this.c = c;
    }

}

class Subclass{
    //Subclass constructor is required
    public Subclass(){
        super(10,99.99,"abc");
        //this is the first comment
    }

    //Overload subclass constructor
    public SubClass(int a, double b, String c){
        super(a,b,c);
        //this is the first comment
    }
}
```

## Polymorphism in Java
* **Overriding** Superclass Methods: Create a method in a child class which has the same name and parameter list as a method in its parent class. Should use *@Override* anotation to get support from compiler.
* **Overloading** Superclass Methods: Create a method in a child class which has the same name but different parameter list as a method in its parent class 
* The method used is determined when the program runs
* Superclass methods cannot overide in subclass:
    1. **static** methods
    2. **final** methods: increase program's performance
    3. methods of **final** classes are all **final**

* Overriding vs Hiding
```Java
class Foo {
    public static void classMethod() {
        System.out.println("classMethod() in Foo");
    }

    public void instanceMethod() {
        System.out.println("instanceMethod() in Foo");
    }

    public void nonOverrideInstanceMethod(){
        System.out.println("nonOverrideInstanceMethod() in Foo");
    }
}

class Bar extends Foo {
    public static void classMethod() {
        //cannot use super in a static method
        //but can use Foo.classMethod() to accept parent class method
        System.out.println("classMethod() in Bar");
    }

    @Override
    public void instanceMethod() {
        System.out.println("instanceMethod() in Bar");
    }

    public void accessOverridedMethods(){
        //call instanceMethod() in Bar
        instanceMethod();
        this.instanceMethod();

        //call instanceMethod() method in Foo
        super.instanceMethod();
    }

    public void accessNonOverridedMethods(){
        //three method calls are equal
        nonOverrideInstanceMethod();
        this.nonOverrideInstanceMethod();
        super.nonOverrideInstanceMethod();
    }

    //Overload method nonOverrideInstanceMethod()
    public void nonOverrideInstanceMethod(String str){
        System.out.println("overload nonOverrideInstanceMethod() method in Foo with nonOverrideInstanceMethod(String str) in Bar");
    }
}

class Test {
    public static void main(String[] args) {
        Foo f = new Foo();
        f.instanceMethod();//   => instanceMethod() in Foo
        f.classMethod();//      => classMethod() in Foo

        Foo fb = new Bar();// fb is declared as Foo but it is a Bar object
        //Bar overriding instanceMethod() from Foo
        fb.instanceMethod();//  => instanceMethod() in Bar
        //Bar hiding static classMethod() from Foo not Overriding
        fb.classMethod();//     => classMethod() in Foo

        Bar b = new Bar();
        b.instanceMethod();//   => instanceMethod() in Bar
        b.classMethod();//      => classMethod() in Bar
    }
}
```

## Abstract Class

* opposite with **final class**, use as **parent class**
* cannot create object
* contain *nonabstract methods* and *abstract methods* 


```Java
public abstract class Animal{//Abstract class
    private String name;
    public abstract void speak();//abstract method

    //nonabstract methods
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
}

Animal animal = new Animal();//ERROR

public class Dog extends Animal{//Concrete class
    //abstract method must be implemented
    @Override
    public void speak(){
        System.out.println("Woof!");
    }
} 

public class Snake extends Animal{//Concrete class
    //abstract method must be implemented
    @Override
    public void speak(){
        System.out.println("Ssss!");
    }
} 

Dog dog = new Dog();//OK
Snake snake = new Snake();//OK
Animal aDog = new Dog();//OK, upcasting, promotion
Animal aSnake = new Snake();//OK, or implicit conversion

//use superclass array type to hold array of subclasses
Animal[] animal = new Animal[2];
animal[0] = new Dog();
animal[1] = new Snake();
```

Dynamic method binding|Static method binding
---|---
or late method binding| or fixed method binding
select the correct subclass method depending on the argument type| opposite
instance methods|static method
more flexible program|operate more quickly
need to create a method that has one or more parameters that might be one of several types|


```Java
public void talkingAnimal(Animal animal)
{
    System.out.println("Come one. Come all.");
    System.out.println
    ("See the amazing talking animal!");
    System.out.println(animal.getName() +
    " says");
    animal.speak();
    System.out.println("***************");
}
//can accept dog, snake, aDog, aSnake as argument
```

## Object Class

* Every class in Java is a subclass of **Object class**
* **toString()** method
  ```Java
  //inherited toString() method from Object class print: ClassName@hashcode
  System.out.println(dog.toString());//Dog@d0a78d9c

  //we can Override toString() method to get different result
  public class Dog{
      //... other statements
      @Override
      public String toString(){
          return "This is a dog!";
      }
      //... other statements
  }

  System.out.println(dog.toString());//This is a dog!
  ```
* **equals()** method: *public boolean equals(Object obj)*; considers two objects to be equal only if they have the same
hash code, they point to the same object  
    Three ways to compare two objects:
    * create custom method like **isEqual()**
    * overload **equals()**
    * override **equals()**
  ```Java
  //overload equals()
  public boolean equals(Student student){
      return this.studentId == student.studentId;
  }

  //override equals()
  @Override
  public boolean equals(Object obj){
      //cast Object to Student
      Student student = (Student)obj;
      return this.studentId == student.studentId;
  }

  //recommend override equals()
  @Override
  public boolean equals(Object obj){
      boolean result;
      if(obj == this)
        result = true;
      else
        if(obj ==  null)
          result = false;
        else
          if(obj.getClass() != this.getClass())
          result = false;
      //cast Object to Student
      Student student = (Student)obj;
      result =  this.studentId == student.studentId;
      return result;
  }//also override hashcode() method
  ```

## Interface - Multiple Inheritance in Java
* describe what a class does, but not how it is done
* all methods are **public** and **abstract**
* all attributes are **public** (because all methods are abstract => cannot retrieve private data), **static** (because cannot create object) and **final** (because all methods are abstract => cannot alter private data)
  ```Java
  public interface PizzaConstants
  {
    public static final int SMALL_DIAMETER = 12;
    public static final int LARGE_DIAMETER = 16;
    public static final double TAX_RATE = 0.07;
    public static final String COMPANY = "Antonio’s Pizzeria";
  }//provide constants
  ```
* use **implements** keyword to use an **interface**

```Java
public interface Worker{
    public void work();//abstract method
}

public class WorkingDog extends Dog implements Worker
{
    private int hoursOfTraining;
    public void setHoursOfTraining(int hrs)
    {
        hoursOfTraining = hrs;
    }
    public int getHoursOfTraining()
    {
        return hoursOfTraining;
    }
    //implement abstract method
    public void work()
    {
        speak();
        System.out.println("I am a dog who works");
        System.out.println("I have " + hoursOfTraining +
        " hours of professional training!");
    }
}
```

## Interface vs Abstract Class
Abstract class| Interface
---|---
cannot instantiate concrete objects|similar
can contain **nonabstract methods**|all methods must be **abstract**
a class can inherit from only one **abstract class**| but can implement multiple **interfaces** 
provide data or methods that subclasses can inherit, but at the same time these subclasses maintain the ability to override the inherited methods| know what actions you want to include, but you also want every user to separately define the behavior that must occur when the method executes



