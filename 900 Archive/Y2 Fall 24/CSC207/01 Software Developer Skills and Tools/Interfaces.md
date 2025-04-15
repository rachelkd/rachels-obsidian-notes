---
dg-publish: true
tags: ["#lecture", "#note", cs, java, university]
Course Code:
  - "[[CSC207]]"
Week: 3
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-19
Date created: Fri., Sep. 20, 2024, 12:11:04 pm
Date modified: Tue., Dec. 10, 2024, 4:41:00 pm
---

# Rethinking Inheritance

- Classes in Java have *one* parent
- What if an abstract class only contains abstract methods but no instance variables?
    - Wasteful; not making use of instance variables
- What if we want our new class to *inherit* from both a concrete class and this abstract class?
    - Forced to pick which parent we care about more inheriting from

# Interfaces

- An Interface is like an abstract class:
    - Separate from inheritance hierarchy
        - → a class can implement any number of interfaces
    - Cannot have instance variables
        - i.e., no state information
    - Methods must either be abstract or provide a default implementation
        - Do not have to use `abstract` keyword since methods are assumed to be abstract
        - `default` keyword is used if interface provides a default implementation
- We can ==create variables whose reference type is that of an Interface==
    - Still benefit from *polymorphism*
    - Can create variables which can refer to any object which implements the provided interface
- `instanceof` can be used to check if an object implements an interface
    - Like how it checks if an object is part of an inheritance hierarchy
- e.g., A `List` is an example of an **interface**
    - e.g., `ArrayList`, `LinkedList`
    - No instance attributes, but should be able to `add`, `remove`, `indexOf`

## Demo. Shapes

From [github/paulgries](https://github.com/paulgries/ShapeExample/tree/main/src):

```java file:Main.java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Rectangle r1 = new Rectangle(10, 20);
        Rectangle r2 = new Rectangle(10, 30);
        Circle c1 = new Circle(10);
        List<Shape> myShapes = new ArrayList<>();
        myShapes.add(r1);
        myShapes.add(r2);
        myShapes.add(c1);
        System.out.println(calculateTotalArea(myShapes));
    }

    private static double calculateTotalArea(List<Shape> myShapes) {
        double totalArea = 0;
        for (Shape shape : myShapes) {
                totalArea += shape.getArea();
        }

        return totalArea;
    }
}
```

- Can’t tell which `getArea` method is called as the developer
    - Don’t know what is going to be in the `List` until you run the program
- Java has **interfaces** that allow developers to write **generic** code

```java file:Shape.java
public interface Shape {
    public double getArea();
}
```

- All classes that *implement* `Shape` have the method `getArea`

```java file:Rectangle.java
public class Rectangle implements Shape {
    private final int width;
    private final int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public double getArea() {
        return width * height;
    }
}
```

```java file:Circle.java
public class Circle implements Shape {
    private final int radius;

    public Circle(int radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }

    public double getArea() {
        return Math.pow(radius, 2) * Math.PI;
    }

}
```

# Abstract Class vs. Interface

Consider an abstract class `Shape`.

```java
public abstract class Shape {
    public abstract double getArea();
}
```

> [!important] Change the class signatures of `Rectangle` and `Circle` to *extends* instead of implements

- Very similar to interfaces
- Difference:
    - Can only extend *one* class
    - Can implement as many interfaces as you want
