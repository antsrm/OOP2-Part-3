
## Exercise 3.4

a.)

The suitable approach for representing temperatures, is using a class with value and scale encapsulated, with methods to convert and calculate the diffrent units.

Use of primitive types / simple classes would not be suitable as they do not provide the required scale distinction and custom behavior. Records are immutable and might not provide the required flexibility for the conversions and arithmetic required here.

Code:

```Java

public class Temperature {
    private final double value;
    private final Scale scale;

    public enum Scale { CELSIUS, FAHRENHEIT, KELVIN }

    public Temperature(double value, Scale scale) {
        this.value = value;
        this.scale = scale;
    }

    public double getValue() { return value; }
    public Scale getScale() { return scale; }

    // Muunnokset
    public Temperature toCelsius() {}
    public Temperature toFahrenheit() {}
    public Temperature toKelvin() {}

    public Temperature add(Temperature other) {}
    public Temperature subtract(Temperature other) {}

    @Override
    public String toString() {
        return value + "°" + scale.name().charAt(0);
    }
}
```
b.)

The suitable approach for representation of diseases with name and symptoms is to use class with fields for name and symptoms.

Records are immutable and as such not a good fit for diseases that evolve. Likewise, enums are too restrictive for evolving information.

Code:

```Java
public class Disease {
    private final String name;
    private final List<String> symptoms;
    private final Human target;

    public Disease(String name, List<String> symptoms, Human target) {
        this.name = name;
        this.symptoms = new ArrayList<>(symptoms);
        this.target = target;
    }

    public String getName() { return name; }
    public List<String> getSymptoms() { return new ArrayList<>(symptoms); }
    public Human getTarget() { return target; }
    
    public void addSymptom(String symptom) {
        symptoms.add(symptom);
    }
}
```
c.)

For searching university student database, a suitable approach is to use class or record for representation of the rows.

Class would likely be more complex than necessary. Primitive arrays / collections would be less maintainable. A record would therefore be best.

Code:

```Java
public record StudentRow(String name, String postalAddress, String emailAddress, String degreeProgram, int credits) {}
```
d.)

To filter students based on credits, one could define a method with functional interface.

Hard-coding logic of filter in a class would be less flexible.

Code:

```Java
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class StudentFilter {
    public static List<StudentRow> filterStudents(List<StudentRow> students, Predicate<StudentRow> predicate) {
        return students.stream().filter(predicate).collect(Collectors.toList());
    }
}

// Use:
List<StudentRow> students = ;
List<StudentRow> eligibleStudents = StudentFilter.filterStudents(students, student -> student.credits() >= 280);
```
e.)

A class with array would allow for modification but encapsulation and usage implementation requires more attention.

A record with number class adds greater abstaction. Could be unnecassary.

A record with primitive int is immutable and simple, probably best in this case:

```Java
public record Point(int x, int y) {}
```
___
