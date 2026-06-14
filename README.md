# Java Practice Problem — The Shape Area Calculator

**Course:** Object-Oriented Programming  
**Topics Covered:** Abstract Classes · Method Overriding · Early Binding · Downcasting · `toString()`

---

## The Story

Rafi is a first-year student at MBSTU who just started learning Java. His professor gives the class a small project:

> "Build a shape calculator for a geometry app. The app must support **rectangles** and **circles**. Every shape has a color and can be filled or unfilled. Each shape must be able to report its **area**, its **perimeter**, and a text summary of itself."

The professor adds one important rule:

> "You must design this so that any new shape — triangle, pentagon, anything — can be added later without rewriting the calculator code."

Rafi realizes he needs an **abstract class** called `Shape` that enforces a contract: every shape *must* implement `getArea()` and `getPerimeter()`. `Rectangle` and `Circle` will extend `Shape` and provide their own formulas.

---

## UML Class Diagram

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

Your GitHub Classroom repository must have exactly this layout:

```
src/
  shapes/
    Shape.java
    Rectangle.java
    Circle.java
    ShapeMain.java
```

The autograder compiles from the `src/` root and runs `shapes.ShapeMain`.

---

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

## Sample Input and Output

### Test 1 — one rectangle

**Input**
```
1
RECTANGLE
Red true
5.0 8.0
```

**Output**
```
[ Rectangle ]
Color : Red
Filled: Yes
Width : 5.0
Length: 8.0
Area      : 40.00
Perimeter : 26.00

```

---

### Test 2 — one circle

**Input**
```
1
CIRCLE
Blue false
7.0
```

**Output**
```
[ Circle ]
Color : Blue
Filled: No
Radius: 7.0
Area         : 153.94
Circumference: 43.98

```

---

### Test 3 — mixed shapes

**Input**
```
3
RECTANGLE
Green true
4.0 9.0
CIRCLE
Yellow false
3.5
RECTANGLE
Black false
10.0 2.5
```

**Output**
```
[ Rectangle ]
Color : Green
Filled: Yes
Width : 4.0
Length: 9.0
Area      : 36.00
Perimeter : 26.00

[ Circle ]
Color : Yellow
Filled: No
Radius: 3.5
Area         : 38.48
Circumference: 21.99

[ Rectangle ]
Color : Black
Filled: No
Width : 10.0
Length: 2.5
Area      : 25.00
Perimeter : 25.00

```

---

### Test 4 — downcasting check (rectangle-only run)

**Input**
```
2
RECTANGLE
White true
6.0 6.0
RECTANGLE
Purple false
1.5 12.0
```

**Output**
```
[ Rectangle ]
Color : White
Filled: Yes
Width : 6.0
Length: 6.0
Area      : 36.00
Perimeter : 24.00

[ Rectangle ]
Color : Purple
Filled: No
Width : 1.5
Length: 12.0
Area      : 18.00
Perimeter : 27.00

```

---

### Test 5 — large values

**Input**
```
2
CIRCLE
Orange true
100.0
RECTANGLE
Gray true
999.9 888.8
```

**Output**
```
[ Circle ]
Color : Orange
Filled: Yes
Radius: 100.0
Area         : 31415.93
Circumference: 628.32

[ Rectangle ]
Color : Gray
Filled: Yes
Width : 999.9
Length: 888.8
Area      : 888711.12
Perimeter : 3777.40

```

---

## `ShapeMain.java` Skeleton

```java
package shapes;

import java.util.Scanner;

public class ShapeMain {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // ── TASK 1 ────────────────────────────────────────────
        // Read the number of shapes from input.
        //
        int n = /* YOUR CODE */;

        // ── TASK 2 ────────────────────────────────────────────
        // Declare a Shape array of size n.
        //
        Shape[] shapes = /* YOUR CODE */;

        // ── TASK 3 ────────────────────────────────────────────
        // Loop n times. For each iteration:
        //   a) Read the shape type  (RECTANGLE or CIRCLE)
        //   b) Read color and filled on the same line
        //   c) Read dimensions (width + length OR radius)
        //   d) Create the correct object and store it in shapes[i]
        //      Use the Shape reference type for the array element.
        //
        for (int i = 0; i < n; i++) {
            String type   = /* YOUR CODE */;
            String color  = /* YOUR CODE */;
            boolean filled = /* YOUR CODE */;

            if (type.equals("RECTANGLE")) {
                double width  = /* YOUR CODE */;
                double length = /* YOUR CODE */;
                shapes[i] = /* YOUR CODE */;

            } else if (type.equals("CIRCLE")) {
                double radius = /* YOUR CODE */;
                shapes[i] = /* YOUR CODE */;
            }
        }

        // ── TASK 4 ────────────────────────────────────────────
        // Loop through the shapes array.
        // For each shape: print it (toString() is called automatically)
        // then print a blank line.
        //
        for (Shape s : shapes) {
            /* YOUR CODE */
        }

        // ── TASK 5 — Downcasting ─────────────────────────────
        // After printing all shapes, loop through the array again.
        // If the shape is a Rectangle, downcast it and print:
        //   "Rectangle width=" + width + " length=" + length
        // If the shape is a Circle, downcast it and print:
        //   "Circle radius=" + radius
        //
        System.out.println("--- Downcast Check ---");
        for (Shape s : shapes) {
            if (s instanceof Rectangle) {
                Rectangle r = /* YOUR CODE */;
                System.out.println("Rectangle width="  + r.getWidth()
                                 + " length=" + r.getLength());
            } else if (s instanceof Circle) {
                Circle c = /* YOUR CODE */;
                System.out.println("Circle radius=" + c.getRadius());
            }
        }

        // ── TASK 6 — Abstract class (written answer) ──────────
        // What error does the compiler give if you write:
        //     Shape s = new Shape("Red", true);
        //
        // Write your answer here as a comment:
        // ANSWER: _______________________________________________

        sc.close();
    }
}
```

---

## Updated Expected Output (with Downcast Check)

The autograder tests `ShapeMain` end-to-end including the downcast section.  
Below is the full expected output for **Test 3**:

```
[ Rectangle ]
Color : Green
Filled: Yes
Width : 4.0
Length: 9.0
Area      : 36.00
Perimeter : 26.00

[ Circle ]
Color : Yellow
Filled: No
Radius: 3.5
Area         : 38.48
Circumference: 21.99

[ Rectangle ]
Color : Black
Filled: No
Width : 10.0
Length: 2.5
Area      : 25.00
Perimeter : 25.00

--- Downcast Check ---
Rectangle width=4.0 length=9.0
Circle radius=3.5
Rectangle width=10.0 length=2.5
```

---

## GitHub Classroom Autograder Setup

Add the following to `.github/classroom/autograding.json` in your repository:

```json
{
  "tests": [
    {
      "name": "Test 1 - Single rectangle",
      "setup": "javac -d out src/shapes/Shape.java src/shapes/Rectangle.java src/shapes/Circle.java src/shapes/ShapeMain.java",
      "run": "java -cp out shapes.ShapeMain",
      "input": "1\nRECTANGLE\nRed true\n5.0 8.0\n",
      "output": "[ Rectangle ]\nColor : Red\nFilled: Yes\nWidth : 5.0\nLength: 8.0\nArea      : 40.00\nPerimeter : 26.00\n\n--- Downcast Check ---\nRectangle width=5.0 length=8.0\n",
      "comparison": "exact",
      "timeout": 10,
      "points": 20
    },
    {
      "name": "Test 2 - Single circle",
      "setup": "javac -d out src/shapes/Shape.java src/shapes/Rectangle.java src/shapes/Circle.java src/shapes/ShapeMain.java",
      "run": "java -cp out shapes.ShapeMain",
      "input": "1\nCIRCLE\nBlue false\n7.0\n",
      "output": "[ Circle ]\nColor : Blue\nFilled: No\nRadius: 7.0\nArea         : 153.94\nCircumference: 43.98\n\n--- Downcast Check ---\nCircle radius=7.0\n",
      "comparison": "exact",
      "timeout": 10,
      "points": 20
    },
    {
      "name": "Test 3 - Mixed shapes",
      "setup": "javac -d out src/shapes/Shape.java src/shapes/Rectangle.java src/shapes/Circle.java src/shapes/ShapeMain.java",
      "run": "java -cp out shapes.ShapeMain",
      "input": "3\nRECTANGLE\nGreen true\n4.0 9.0\nCIRCLE\nYellow false\n3.5\nRECTANGLE\nBlack false\n10.0 2.5\n",
      "output": "[ Rectangle ]\nColor : Green\nFilled: Yes\nWidth : 4.0\nLength: 9.0\nArea      : 36.00\nPerimeter : 26.00\n\n[ Circle ]\nColor : Yellow\nFilled: No\nRadius: 3.5\nArea         : 38.48\nCircumference: 21.99\n\n[ Rectangle ]\nColor : Black\nFilled: No\nWidth : 10.0\nLength: 2.5\nArea      : 25.00\nPerimeter : 25.00\n\n--- Downcast Check ---\nRectangle width=4.0 length=9.0\nCircle radius=3.5\nRectangle width=10.0 length=2.5\n",
      "comparison": "exact",
      "timeout": 10,
      "points": 20
    },
    {
      "name": "Test 4 - Rectangles only",
      "setup": "javac -d out src/shapes/Shape.java src/shapes/Rectangle.java src/shapes/Circle.java src/shapes/ShapeMain.java",
      "run": "java -cp out shapes.ShapeMain",
      "input": "2\nRECTANGLE\nWhite true\n6.0 6.0\nRECTANGLE\nPurple false\n1.5 12.0\n",
      "output": "[ Rectangle ]\nColor : White\nFilled: Yes\nWidth : 6.0\nLength: 6.0\nArea      : 36.00\nPerimeter : 24.00\n\n[ Rectangle ]\nColor : Purple\nFilled: No\nWidth : 1.5\nLength: 12.0\nArea      : 18.00\nPerimeter : 27.00\n\n--- Downcast Check ---\nRectangle width=6.0 length=6.0\nRectangle width=1.5 length=12.0\n",
      "comparison": "exact",
      "timeout": 10,
      "points": 20
    },
    {
      "name": "Test 5 - Large values",
      "setup": "javac -d out src/shapes/Shape.java src/shapes/Rectangle.java src/shapes/Circle.java src/shapes/ShapeMain.java",
      "run": "java -cp out shapes.ShapeMain",
      "input": "2\nCIRCLE\nOrange true\n100.0\nRECTANGLE\nGray true\n999.9 888.8\n",
      "output": "[ Circle ]\nColor : Orange\nFilled: Yes\nRadius: 100.0\nArea         : 31415.93\nCircumference: 628.32\n\n[ Rectangle ]\nColor : Gray\nFilled: Yes\nWidth : 999.9\nLength: 888.8\nArea      : 888711.12\nPerimeter : 3777.40\n\n--- Downcast Check ---\nCircle radius=100.0\nRectangle width=999.9 length=888.8\n",
      "comparison": "exact",
      "timeout": 10,
      "points": 20
    }
  ]
}
```

---

## Grading Rubric

| Test | Description | Points |
|---|---|---|
| Test 1 | Single rectangle — area, perimeter, toString | 20 |
| Test 2 | Single circle — area, circumference, toString | 20 |
| Test 3 | Mixed rectangle + circle + downcast check | 20 |
| Test 4 | Rectangles only — downcast reports both | 20 |
| Test 5 | Large decimal values formatted correctly | 20 |
| **Total** | | **100** |

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
