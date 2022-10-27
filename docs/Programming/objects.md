# Objects
Objects are programming constructs that are used to represent physical objects and ideas in the real world.  They are made up of [Data Structures](dataStructures.md) and [Procedures](procedures.md) that define the object's attributes and the tasks that it can perform.  Attributes are pieces of data that describe an object's physical appearance and maintain its current state.  An example might be the diameter of a wheel and its number of rotations.  The first is and example of a Constant data type since the diameter of a wheel does not change.  The latter is a variable, or mutable, data type since the number of times that a wheel has rotated may change thoughout the execution of the program. In [Object Oriented Programming(OPP)](https://en.wikipedia.org/wiki/Object-oriented_programming) **Procedures** are referred to as **Methods**.

![Programming Concepts](../images/FRCProgramming/FRCProgramming.018.jpeg)

All Objects are derived from [Classes](classes.md) that provide a common template for their creation.  There are three major components of a Java class.

- **Constructor**  
A constructor is a special method that is used to create and initialize an object of a Java class.  Every Java class must have a constructor, and if we don’t declare one the compiler creates a default constructor for us.  A constructor must have the same name as the Java class. 

- **Attributes**  
These are the data structures that describe the object's physical appearance and maintain its current state.  All attributes must be initialize when an object is constructed, otherwise the object would be in an inconsistent state.

- **Methods**  
A method is a block of code which only runs when it's called. You can pass data, known as parameters, into a method. Methods are used to perform certain actions, and they are also known as functions.

The W3 Schools Java Tutorial has an introduction to [Object Oriented Programming (OOP)](https://www.w3schools.com/java/java_oop.asp).  The tutorial also explains [Attributes](https://www.w3schools.com/java/java_class_attributes.asp) and [Methods](https://www.w3schools.com/java/java_class_methods.asp), the OOP name for a **Procedures**. 

## References

- W3Schools -  [Java Classes and Objects](https://www.w3schools.com/java/java_classes.asp)

- Wikipedia - [Object Oriented Programming(OPP)](https://en.wikipedia.org/wiki/Object-oriented_programming)

<!-- <h3><span style="float:left">
<a href="procedures">Previous</a></span>
<span style="float:right">
<a href="stateMachines">Next</a></span></h3> -->