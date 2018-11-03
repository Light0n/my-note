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
* **extends** keyword: use to inherit non-private methods and attributes of a superclass. Ex: `class Subclass extends Superclass{}`
* **instanceof** operator: use to check whether or not an object belong to a class. Ex: `object instanceof Class;` return `true` if **object** can be upcast to the **Class** 
* Every class in Java is a subclass of **Object**
* When you instantiate **an object** of **a subclass**. The superclass constructor is called before the subclass constructor. Ex: `Dog dog = new Dog();`, then constructor called orders is Object() => Animal() => Dog() 
* **super**: to access parent methods 
* **public**: can be accessed by all classes in the same package
* **protected**: can be accessed only by the subclasses 
* **private**: cannot be accessed outside of the class 
* Subclasses inherit all data and methods of the superclass except private members of the superclass
* **final** classes cannot have subclass

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
        //cannot use super in 
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

