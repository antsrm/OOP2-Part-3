
## Exercise 3.3

The program could use a complete overhaul along object-oriented principles to make it more usable and maintainable.

A modular design using classes and overriding methods where necessary seems the best way to go. In this way, new shapes / features can more easily be added later. There should be an abstract base class of shape that is inherited by more specific types of shapes.

New iteration of the program:

```Java
import java.util.Scanner;

abstract class Shape {
    public abstract double calculateArea();
    public abstract Point[] calculateBoundaries();
}

class Point {
    int x, y;

    Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}

class Triangle extends Shape {
    private Point[] points;

    public Triangle(Point[] points) {
        if (points.length != 3) throw new IllegalArgumentException("Triangle must have three points!");
        this.points = points;
    }

    @Override
    public double calculateArea() {
        int x1 = points[0].x;
        int y1 = points[0].y;
        int x2 = points[1].x;
        int y2 = points[1].y;
        int x3 = points[2].x;
        int y3 = points[2].y;
        return Math.abs(x1*(y2-y3) + x2*(y3-y1) + x3*(y1-y2)) / 2.0;
    }

    @Override
    public Point[] calculateBoundaries() {
        int xMin = Math.min(points[0].x, Math.min(points[1].x, points[2].x));
        int xMax = Math.max(points[0].x, Math.max(points[1].x, points[2].x));
        int yMin = Math.min(points[0].y, Math.min(points[1].y, points[2].y));
        int yMax = Math.max(points[0].y, Math.max(points[1].y, points[2].y));
        return new Point[] { new Point(xMin, yMin), new Point(xMax, yMax) };
    }
}

class Quadrilateral extends Shape {
    private Point[] points;

    public Quadrilateral(Point[] points) {
        if (points.length != 4) throw new IllegalArgumentException("Quadrilateral needs 4 points!");
        this.points = points;
    }

    @Override
    public double calculateArea() {
        int x1 = points[0].x;
        int y1 = points[0].y;
        int x2 = points[1].x;
        int y2 = points[1].y;
        int x3 = points[2].x;
        int y3 = points[2].y;
        int x4 = points[3].x;
        int y4 = points[3].y;

        return Math.abs(x1*y2 + x2*y3 + x3*y4 + x4*y1 - y1*x2 - y2*x3 - y3*x4 - y4*x1) / 2.0;
    }

    @Override
    public Point[] calculateBoundaries() {
        int xMin = Math.min(points[0].x, Math.min(points[1].x, Math.min(points[2].x, points[3].x)));
        int xMax = Math.max(points[0].x, Math.max(points[1].x, Math.max(points[2].x, points[3].x)));
        int yMin = Math.min(points[0].y, Math.min(points[1].y, Math.min(points[2].y, points[3].y)));
        int yMax = Math.max(points[0].y, Math.max(points[1].y, Math.max(points[2].y, points[3].y)));

        return new Point[] { new Point(xMin, yMin), new Point(xMax, yMax) };
    }
}

class Circle extends Shape {
    private Point center;
    private Point perimeter;

    public Circle(Point center, Point perimeter) {
        this.center = center;
        this.perimeter = perimeter;
    }

    @Override
    public double calculateArea() {
        double radius = Math.sqrt(Math.pow(perimeter.x - center.x, 2) + Math.pow(perimeter.y - center.y, 2));
        return Math.PI * radius * radius;
    }

    @Override
    public Point[] calculateBoundaries() {
        double radius = Math.sqrt(Math.pow(perimeter.x - center.x, 2) + Math.pow(perimeter.y - center.y, 2));
        int xMin = (int) (center.x - radius);
        int xMax = (int) (center.x + radius);
        int yMin = (int) (center.y - radius);
        int yMax = (int) (center.y + radius);

        return new Point[] { new Point(xMin, yMin), new Point(xMax, yMax) };
    }
}

public class Exercise3 {
    public Exercise3() {
        System.out.println("Exercise 3");
    }

    static String readType() {
        System.out.print("Enter the pattern type (triangle, quadrilateral, circle): ");
        return new Scanner(System.in).next();
    }

    static Point readPoint() {
        System.out.print("Enter the x-coordinate of the point: ");
        int x = new Scanner(System.in).nextInt();
        System.out.print("Enter the y-coordinate of the point: ");
        int y = new Scanner(System.in).nextInt();
        return new Point(x, y);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Shape[] shapes = new Shape[2];

        for (int i = 0; i < shapes.length; i++) {
            String type = readType();
            switch (type) {
                case "triangle" -> {
                    Point[] points = new Point[3];
                    for (int j = 0; j < 3; j++) points[j] = readPoint();
                    shapes[i] = new Triangle(points);
                }
                case "quadrilateral" -> {
                    Point[] points = new Point[4];
                    for (int j = 0; j < 4; j++) points[j] = readPoint();
                    shapes[i] = new Quadrilateral(points);
                }
                case "circle" -> {
                    Point center = readPoint();
                    Point perimeter = readPoint();
                    shapes[i] = new Circle(center, perimeter);
                }
                default -> throw new IllegalArgumentException("Unknown type");
            }
        }

        double totalArea = 0;
        for (Shape shape : shapes) {
            totalArea += shape.calculateArea();
        }
        System.out.printf(totalArea);

        int x1 = Integer.MIN_VALUE, y1 = Integer.MIN_VALUE, x2 = Integer.MAX_VALUE, y2 = Integer.MAX_VALUE;
        for (Shape shape : shapes) {
            Point[] boundaries = shape.calculateBoundaries();
            x1 = Math.max(x1, boundaries[1].x);
            y1 = Math.max(y1, boundaries[1].y);
            x2 = Math.min(x2, boundaries[0].x);
            y2 = Math.min(y2, boundaries[0].y);
        }
        
        System.out.printf("The boundaries of patterns: (%d, %d) x (%d, %d)%n", x2, y2, x1, y1);
    }
}
```
Design choices:

- The new design uses inheritance and polymorphism. 'Shape' defines an abstract class that has subclasses 'Triangle', 'Quadrilateral' and 'Circle'. The subclasses uses polymorphism as they override the 'calculateArea' and 'calculateBoundaries' methods to fit the attributes of shape being calculated.
- In the program execution, user input is collected and type of calculation is determined. Based on type given, switch statement determines the case to be used.
- With this OOP-based design, it is easier to add new shapes as subclasses.
- The code overall is much more readable.
