
## Exercise 3.2

a.) 

Polymorphism and inheritance manifest in many ways in the code.

The classes 'WeirdProblem' and 'TrickyProblem' inherit from 'Problem', creating a hierarchy of exceptions:

'Problem' is an abstract class extending 'Exception'. So, 'WeirdProblem' and 'TrickyProblem' are by extension also subclasses of 'Exception'.

In 'Experiment' class, the method 'perform' is declared to throw 'WeirdProblem' or 'TrickyProblem'.

Meanwhile 'Experiment2' class has method 'perform' which is declared to throw 'Problem'. In this case, it can throw any subclass of 'Problem', including the ones mentioned above.

In the try block 'e1.perform() can throw 'WeirdProblem' (caught by first catch block) or 'TrickyProblem' (caught by second catch block). The 'e2.perform' can throw both of these, but as it's been declared to throw 'Problem', it conforms to it's superclass exception type.

The try-catch structure of the code uses polymorphism by catching exceptions by order of specificity. The 'catch (Problem w)' block is a polymorphic handler to catch any exception of type 'Problem' that is not already caught by more specific catch blocks.

With this structure, exceptions are handled as specifically as possible. Inheritance is used in the class hierarchy and polymorphism is used in the catch blocks that handle many different subclass instances.

b.)

'Printer1' is inheritance-based, as it extends 'Fancy', inheriting it's 'decorate' method. It also implents the 'Printer' interface. The decoration mechanism is such that the 'decorate' method from 'Fancy' class is used, which returns the string decorated as: '== input =='

This implementation uses simple inheritance and is easy to understand. It is not very flexible, as it is tightly coupled to 'Fancy' class. Changing the decoration dynamically is difficult.

'Printer2' utilizes composition-based reuse. It implements both 'Decorator' and 'Printer' interfaces. The decoration mechanism is delegated to a 'Fancy' instance created by 'generateDecorator' method.

This implementation is more flexible, as the decoration type can be changed by modifying 'generateDecorator'. This approach is however somewhat more complex than 'Printer1'.
<br>
<br>

Extending Functionality for Printer3:

The extension implemented in two different ways:

- Version 1: Where the number of characters is defined in 'Decorator' interface.
<br>

- Version 2: Where the number of characters is defined in 'Printer' interface.

Version 1 implementation:

```Java
interface Decorator {
    String decorate(String input);
    // number of characters
    int getDecorLength();
}

class FancyTwo implements Decorator {
    @Override
    public String decorate(String input) {
        int len = getDecorLength();
        String dash = "-".repeat(len);
        return dash + "== " + input + " ==" + dash;
    }

    @Override
    public int getDecorLength() {
        return 2;
    }
}

class Printer3 implements Decorator, Printer {
    private final Decorator decorator = generateDecorator();

    Decorator generateDecorator() {
        return new FancyTwo();
    }

    @Override
    public String decorate(String input) {
        return decorator.decorate(input);
    }

    @Override
    public int getDecorLength() {
        return decorator.getDecorLength();
    }

    @Override
    public void print(String s) {
        System.out.println(s);
    }

    @Override
    public void run() {
        print(decorate("main"));
    }
}
```
Version 2 implementation:

```Java
interface Printer {
    void print(String s);
    void run();
    // number of characters
    int getDecorLength();
}

interface Decorator {
    String decorate(String input);
}

class FancyTwo implements Decorator {
    @Override
    public String decorate(String input) {
        return "== " + input + " ==";
    }
}

class Printer3 implements Decorator, Printer {
    private final Decorator decorator = generateDecorator();

    Decorator generateDecorator() {
        return new FancyTwo();
    }

    @Override
    public String decorate(String input) {
        int len = getDecorLength();
        String dash = "-".repeat(len);
        return dash + decorator.decorate(input) + dash;
    }

    @Override
    public void print(String s) {
        System.out.println(s);
    }

    @Override
    public void run() {
        print(decorate("main"));
    }

    @Override
    public int getDecorLength() {
        return 2;
    }
}
```
___