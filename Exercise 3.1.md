
## Exercise 3.1

a.) 

The form of inheritance in the example is simple or single inheritance.

There is a base class 'CommandLineApp' which is abstract and contains method 'ask'. The use of 'abstract' keyword indicates that this class is to be used with subclass and not instantiated as such.

The 'LoginScreen' class inherits all non-private fields and methods of 'CommandLineApp'.

There is no explicit use of polymorphism in the example (such as method overriding), but inheritance lays groundwork for polymorphism.

b.) 

In the code, there is interface 'NaturalResource' and two classes that implement it: 'Coal' and 'Hydroelectric'.
Both classes get the implementation of 'amountLeft()' method. Both override the method (polymorphism).

Coal class works as it should. It manages the amount of coal remaining and spending it.

Hydroelectric class doesn't implement 'NaturalResource' interface correctly. This is because 'spend' method is private and not accessible. 'NaturalResource' interface states that spend should be public. As such, this violates the contract of the interface.

The benefits of using 'NaturalResource' interface is that it allows polymorphism. Different resource types can be managed with the same interface. This interface-style design is good for reusablity. Each class encapsulates it's own data, according to principles of object-oriented programming.

The drawbacks of the code: For one, 'NaturalResource' interface defines methods that might not be sensible for all types of implementations. Hydroelectric for example is supposed to be renewable. This could be an issue in the design. Also, 'Hydroelectric' class throws exception in 'spend' method, but this is not indicated in the interface. This can cause unexpected behavior when called, as objects of a superclass should be repleacable with objects of subclass without affecting correctness of the program.

To fix the current issues, 'Hydroelectric' class should have public 'spend' method, or design in general should be modified to account for renewable resources.

c.) 

In the example, 'RandomIntegerGenerator' class extends 'RandomGenerator' and overrides it's 'generate' metod.

In 'RandomGenerator' the 'generate' method returns an 'Object'. In 'RandomIntegerGenerator' the 'generate' method is overridden and returns 'Integer'.

There is a problem with this, as 'RandomIntegerGenerator' can't change return type to 'Integer' if the method in superclass returns 'Object'. Method override in Java requires that return type must be same type or subtype of the type declared in the superclass.

The benefits: 'RandomGenerator' class encapsulates the logic for generating random values. This can then be reused and extended. 'RandomIntegerGenerator' tries to specialize this generation process for integers.

The drawbacks: There are issues with the override in 'RandomIntegerGenerator', as the attempted change in return type leads to a compilation error.

