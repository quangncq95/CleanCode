## Objects and data structures
### Data abstraction
 Hiding implementation is not just a matter of putting a layer of functions between the variables. Hiding implementation is about abstractions! A class does not simply push its variables out through getters and setters. Rather it exposes abstract interfaces that allow its users to manipulate the _essence_ of the data, without having to know its implementation.
### Data/Object Anti-Symmetry
This exposes the fundamental dichotomy between objects and data structures:
_Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions._

The complement is also true: 
_Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change._
Procedural:
```
public class Square { 
	public Point topLeft; 
	public double side; 
} 
public class Rectangle { 
	public Point topLeft; 
	public double height; 
	public double width; 
} 
public class Circle { 
	public Point center; 
	public double radius; 
} 
public class Geometry { 
	public final double PI = 3.141592653589793; 
	public double area(Object shape) throws NoSuchShapeException { 
	if (shape instanceof Square) { 
		Square s = (Square)shape; 
		return s.side * s.side; 
	}else if (shape instanceof Rectangle) { 
		Rectangle r = (Rectangle)shape; 
		return r.height * r.width; 
	}else if (shape instanceof Circle) { 
		Circle c = (Circle)shape; 
		return PI * c.radius * c.radius; 
	} throw new NoSuchShapeException(); 
}
```


OOP code :
```
public class Square implements Shape { 
	private Point topLeft; 
	private double side; 
	public double area() { 
		return side*side; 
	} 
} 
public class Rectangle implements Shape {
	 private Point topLeft; 
	 private double height; 
	 private double width; 
	 public double area() { 
		 return height * width; 
	 }
 }
public class Circle implements Shape { 
	private Point center; 
	private double radius; 
	public final double PI = 3.141592653589793; 
	public double area() { 
		return PI * radius * radius; 
	}
}
```
### Law of Demeter
_Law of Demeter_ states that an object should not know the internal details of other objects, and should only interact with its immediate friends.
More precisely, the Law of Demeter says that a method f of a class C should only call the methods of these:  
- C 
- An object created by f
- An object passed as an argument to f 
-  An object held in an instance variable of C

The method should not invoke methods on objects that are returned by any of the allowed functions. 
In other words, **talk to friends, not to strangers.**
violate:
```
// This method violates the law of demeter by reaching through
// model, selectedPlanet and fleet to access numberOfFrigates

public void m(Model model) {
int numberOfFrigates = model.getSelectedPlanet().getFleet().getNumberOfFrigates();
// do something with numberOfFrigates
}
```
follow:
```
// This method follows the law of demeter by only interacting with
// the A object, which is passed as an argument
public void m(A a) {
D d = a.getD();
// do something with d
}

// The A object encapsulates the logic of accessing the D object
public class A {
// other fields and methods

public D getD() {
return getB().getC().getD();
}
}
```
### Data transfer object
The quintessential form of a data structure is a class with public variables and no functions. This is sometimes called a data transfer object, or DTO. DTOs are very useful structures, especially when communicating with databases or parsing messages from sockets, and so on. They often become the first in a series of translation stages that convert raw data in a database into objects in the application code.
