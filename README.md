# Java Practice Problem — The Shape Area Calculator

**Course:** Object-Oriented Pattern Design Lab
**Course Code:** ICT 2208
**Learning Topics:** Abstract Classes · Method Overriding · Early Binding · Downcasting · `toString()`

---

## The Story

Rafi is a first-year student at MBSTU who just started learning Java. His professor gives the class a small project:

> "Build a shape calculator for a geometry app. The app must support **rectangles** and **circles**. Every shape has a color and can be filled or unfilled. Each shape must be able to report its **area**, its **perimeter**, and a text summary of itself."

The professor adds one important rule:

> "You must design this so that any new shape — triangle, pentagon, anything — can be added later without rewriting the calculator code."

Rafi realizes he needs an **abstract class** called `Shape` that enforces a contract: every shape *must* implement `getArea()` and `getPerimeter()`. `Rectangle` and `Circle` will extend `Shape` and provide their own formulas.

---

## UML Class Diagram

<img width="1472" height="1138" alt="image" src="https://github.com/user-attachments/assets/2ae8136e-44c8-4297-86e2-c7ff7207fecd" />


Alternative UML: (if not visible above)
```
+------------------------------------------+
|              <<abstract>>                |
|                 Shape                    |
+------------------------------------------+
| - color  : String                        |
| - filled : boolean                       |
+------------------------------------------+
| + Shape(color, filled)                   |
| + getArea()      : double  <<abstract>>  |
| + getPerimeter() : double  <<abstract>>  |
| + toString()     : String                |
+------------------------------------------+
              ^               ^
              |               |
         extends         extends
              |               |
+---------------------------+   +---------------------------+
|        Rectangle          |   |          Circle           |
+---------------------------+   +---------------------------+
| - width  : double         |   | - radius : double         |
| - length : double         |   +---------------------------+
+---------------------------+   | + Circle(color,filled,    |
| + Rectangle(color,filled, |   |          radius)          |
|             width,length) |   | + getArea()      : double |
| + getArea()      : double |   | + getPerimeter() : double |
| + getPerimeter() : double |   | + toString()     : String |
| + toString()     : String |   +---------------------------+
+---------------------------+
```

**Formulas:**

```
Rectangle  →  area = width × length           perimeter = 2 × (width + length)
Circle     →  area = π × radius²              circumference = 2 × π × radius
```

---

## Concepts You Will Practice

| Concept | Where It Appears |
|---|---|
| Abstract class | `Shape` cannot be instantiated directly |
| Abstract method | `getArea()` and `getPerimeter()` declared but not implemented in `Shape` |
| Method overriding | Each subclass provides its own area and perimeter formulas |
| Early binding | `toString()` on a `Shape` reference — compile-time type governs accessibility, runtime type governs which version runs |
| Downcasting | Casting a `Shape` reference back to `Rectangle` to call `getWidth()` |
| `toString()` | Each class formats itself as a readable one-block string |

---

## Project Structure

Please see below. No need src folder. 

## Starter Code

### `Shape.java`

```java
package shapes;

public abstract class Shape {

    private String  color;
    private boolean filled;

    public Shape(String color, boolean filled) {
        this.color  = color;
        this.filled = filled;
    }

    public abstract double getArea();
    public abstract double getPerimeter();

    @Override
    public String toString() {
        return "Color : " + color + "\n" +
               "Filled: " + (filled ? "Yes" : "No");
    }

    public String  getColor()           { return color;  }
    public boolean isFilled()           { return filled; }
    public void    setColor(String c)   { this.color  = c; }
    public void    setFilled(boolean f) { this.filled = f; }
}
```

### `Rectangle.java`

```java
package shapes;

public class Rectangle extends Shape {

    private double width;
    private double length;

    public Rectangle(String color, boolean filled, double width, double length) {
        super(color, filled);
        this.width  = width;
        this.length = length;
    }

    @Override
    public double getArea() {
        return width * length;
    }

    @Override
    public double getPerimeter() {
        return 2 * (width + length);
    }

    @Override
    public String toString() {
        return "[ Rectangle ]\n" +
               super.toString()  + "\n" +
               "Width : " + width  + "\n" +
               "Length: " + length + "\n" +
               String.format("Area      : %.2f", getArea())      + "\n" +
               String.format("Perimeter : %.2f", getPerimeter());
    }

    public double getWidth()  { return width;  }
    public double getLength() { return length; }
}
```

### `Circle.java`

```java
package shapes;

public class Circle extends Shape {

    private double radius;

    public Circle(String color, boolean filled, double radius) {
        super(color, filled);
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }

    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }

    @Override
    public String toString() {
        return "[ Circle ]\n" +
               super.toString() + "\n" +
               "Radius: " + radius + "\n" +
               String.format("Area         : %.2f", getArea())      + "\n" +
               String.format("Circumference: %.2f", getPerimeter());
    }

    public double getRadius() { return radius; }
}
```

---

## Your Task — `ShapeMain.java`

Read input from `System.in` using a `Scanner`. The program will receive a series of shape descriptions and must print each shape's full summary followed by a blank line.

### Input format

```
<numberOfShapes>
For each shape:
  <shapeType>          ← RECTANGLE or CIRCLE
  <color> <filled>     ← e.g.  Red true
  <dimensions>         ← width length  (Rectangle)  OR  radius  (Circle)
```

### Output format

Print each shape's `toString()` output followed by one blank line.  
All decimal values must be formatted to **exactly 2 decimal places**.

---

# Shapes Assignment — Java OOP

A Java OOP assignment implementing an abstract `Shape` class with two concrete subclasses: `Rectangle` and `Circle`. The program reads shape data from standard input and prints area and perimeter details.

---

## Project Structure

```
your-repo/
├── .github/
│   ├── classroom/
│   │   └── autograding.json
│   └── workflows/
│       └── classroom.yml
└── shapes/
    ├── Shape.java
    ├── Circle.java
    ├── Rectangle.java
    └── ShapeMain.java
```

---

## How to Compile

```bash
javac -d . shapes/Shape.java shapes/Rectangle.java shapes/Circle.java shapes/ShapeMain.java
```

---

## How to Run

```bash
echo "<shapeType> <color> <filled> <dimensions>" | java shapes.ShapeMain
```

| Argument      | Description                        | Example       |
|---------------|------------------------------------|---------------|
| `shapeType`   | `rectangle` or `circle`            | `rectangle`   |
| `color`       | Any color string                   | `Red`         |
| `filled`      | `true` or `false`                  | `true`        |
| `dimensions`  | `width length` or `radius`         | `5.0 3.0`     |

---

## Tests

### Test 1 — Rectangle

**Test Name:** `test_rectangle_basic`

**Input:**
```
rectangle Red true 5.0 3.0
```

**Run:**
```bash
echo "rectangle Red true 5.0 3.0" | java shapes.ShapeMain
```

**Expected Output:**
```
[ shapes.Rectangle ]
Color : Red
Filled: Yes
Width : 5.0
Length: 3.0
Area      : 15.00
Perimeter : 16.00
Area: 15.00
Perimeter: 16.00
```

**How the values are calculated:**

| Property  | Formula              | Result  |
|-----------|----------------------|---------|
| Area      | width × length       | 15.00   |
| Perimeter | 2 × (width + length) | 16.00   |

---

### Test 2 — Circle

**Test Name:** `test_circle_basic`

**Input:**
```
circle Blue false 7.0
```

**Run:**
```bash
echo "circle Blue false 7.0" | java shapes.ShapeMain
```

**Expected Output:**
```
[ Circle ]
Color : Blue
Filled: No
Radius: 7.0
Area         : 153.94
Circumference: 43.98
Area: 153.94
Perimeter: 43.98
```

**How the values are calculated:**

| Property      | Formula       | Result  |
|---------------|---------------|---------|
| Area          | π × r²        | 153.94  |
| Circumference | 2 × π × r     | 43.98   |

---

## Autograding

This assignment uses GitHub Classroom autograding. Each test is worth **50 points** (100 points total).

| Test Name              | Shape     | Points |
|------------------------|-----------|--------|
| `test_rectangle_basic` | Rectangle | 50     |
| `test_circle_basic`    | Circle    | 50     |

Scores are reported automatically on the GitHub Classroom dashboard after every push to `main` or `master`.

---

## Classes Overview

### `Shape` (abstract)
Base class for all shapes. Holds `color` and `filled` properties.

| Method           | Description                        |
|------------------|------------------------------------|
| `getArea()`      | Abstract — returns area            |
| `getPerimeter()` | Abstract — returns perimeter       |
| `toString()`     | Returns color and filled status    |

### `Rectangle extends Shape`
| Method           | Formula                    |
|------------------|----------------------------|
| `getArea()`      | `width × length`           |
| `getPerimeter()` | `2 × (width + length)`     |

### `Circle extends Shape`
| Method           | Formula                    |
|------------------|----------------------------|
| `getArea()`      | `π × radius²`              |
| `getPerimeter()` | `2 × π × radius`           |

---

## Known Issue

`Rectangle.java` contains a duplicate nested `Shape` abstract class that must be removed. The top-level `Shape.java` already defines this class. Leaving the nested version in will cause a **compile error**.

**Fix:** Delete the `public abstract static class Shape { ... }` block from the bottom of `Rectangle.java`.

---

## Requirements

- Java 17 or higher
- No external libraries required

## Grading Rubric

| Test | Description | Points |
|---|---|---|
| Test 1 | Single rectangle — area, perimeter, toString | 2 |
| Test 2 | Single circle — area, circumference, toString | 2 |
| Test 3 | Mixed rectangle + circle + downcast check | 2 |
| Test 4 | Rectangles only — downcast reports both | 2 |
| Test 5 | Large decimal values formatted correctly | 2 |
| **Total** | | **10** |

---

## Concept Explanation Notes

### Abstract class

`Shape` is declared `abstract`, so `new Shape(...)` causes a **compile-time error**:

```
error: Shape is abstract; cannot be instantiated
```

Its job is to define the contract — every shape must provide `getArea()` and `getPerimeter()` — without specifying how to compute them.

### Method overriding

When `Rectangle` writes `@Override public double getArea()`, it replaces `Shape`'s declaration with its own formula. Rules for a valid override:

- Same method name and parameter list.
- Same or covariant return type.
- Cannot reduce visibility (`public` cannot become `private`).

The `@Override` annotation is optional but strongly recommended — the compiler catches signature typos.

### Early binding vs late binding

`static`, `final`, and `private` methods use **early (static) binding** — resolved at compile time.  
All other methods use **late (dynamic) binding** — the JVM checks the actual object type at runtime.

```java
Shape s = new Rectangle("Red", true, 5, 8);
s.getArea();      // calls Rectangle.getArea() at runtime — late binding
s.toString();     // calls Rectangle.toString() at runtime — late binding
```

The declared type `Shape` controls what method *names* are accessible.  
The runtime type `Rectangle` controls which *version* runs.

### Downcasting

```java
Shape s1 = new Rectangle("Red", true, 5.0, 8.0);

// s1.getWidth();          ← compile error: Shape has no getWidth()

if (s1 instanceof Rectangle) {
    Rectangle r = (Rectangle) s1;   // downcast
    r.getWidth();                    // now accessible
}
```

Always guard with `instanceof` before casting. An unguarded cast to the wrong type throws `ClassCastException` at runtime.

### Why `toString()` in each class?

`System.out.println(shape)` automatically calls `toString()` on the object. By overriding it in every subclass (and calling `super.toString()` to reuse the parent's color/filled lines), each shape can print a complete, neatly formatted summary of itself with no extra code in `main`.

---

## Common Mistakes to Watch For

| Mistake | What happens |
|---|---|
| `new Shape("Red", true)` | Compile error — cannot instantiate abstract class |
| Forgetting `@Override` on `getArea()` | Silently creates a new method; the abstract method remains unimplemented — compile error |
| `(Rectangle) s2` where `s2` holds a `Circle` | `ClassCastException` at runtime |
| Calling `getWidth()` on a `Shape` reference | Compile error — not declared in `Shape` |
| `toString()` not calling `super.toString()` | Output loses `color` and `filled` lines — test fails |
| Extra space or wrong decimal places | Autograder uses exact match — `40.0` fails where `40.00` is expected |

---

## Submission Checklist

- [ ] `src/shapes/Shape.java` — abstract class, two abstract methods, `toString()`
- [ ] `src/shapes/Rectangle.java` — overrides `getArea()`, `getPerimeter()`, `toString()`
- [ ] `src/shapes/Circle.java` — overrides `getArea()`, `getPerimeter()`, `toString()`
- [ ] `src/shapes/ShapeMain.java` — reads input, fills array, prints shapes, prints downcast check
- [ ] All 5 autograder tests pass (100 / 100)
- [ ] Written answer for Task 6 is filled in as a comment
