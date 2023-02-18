---
title: "Solid"
date: 2023-02-17T18:45:50-08:00
draft: true
---

## SOLID principles 
> are a set of principles used to design software that is easy to maintain and extend over time. 

### The principles are:

>> ### Single Responsibility Principle (SRP)
>> ### Open-Closed Principle (OCP)
>> ### Liskov Substitution Principle (LSP)
>> ### Interface Segregation Principle (ISP)
>> ### Dependency Inversion Principle (DIP)

Here are examples of each principle in Java and how they are interrelated:

### Single Responsibility Principle (SRP)
The SRP states that a class should have only one reason to change. In other words, a class should only have one responsibility. For example, a User class should only be responsible for managing user data, not for displaying it on the screen.

```java
public class User {
  private String username;
  private String password;

  // getters and setters for username and password

  public void save() {
    // save the user to the database
  }
}
```

### Open-Closed Principle (OCP)
The OCP states that a class should be open for extension but closed for modification. In other words, you should be able to add new functionality to a class without changing the existing code. For example, if you want to add a new type of user to your system, you should be able to do so without modifying the existing User class.

```java
public interface User {
  public void save();
}

public class RegularUser implements User {
  public void save() {
    // save the regular user to the database
  }
}

public class AdminUser implements User {
  public void save() {
    // save the admin user to the database
  }
}
```
Liskov Substitution Principle (LSP)
The LSP states that a subclass should be substitutable for its parent class. In other words, you should be able to use a subclass wherever the parent class is used. For example, if you have a method that takes a User object as a parameter, you should be able to pass in a RegularUser object or an AdminUser object without any problems.

```java
public class UserService {
  public void saveUser(User user) {
    user.save();
  }
}
Interface Segregation Principle (ISP)
The ISP states that a class should not be forced to implement methods that it doesn't need. In other words, you should separate your interfaces into smaller, more specific interfaces. For example, if you have a class that needs to read data from a database, you should create a separate interface for that functionality.

```java
public interface DatabaseReader {
  public List<String> readData();
}

public class MySqlDatabaseReader implements DatabaseReader {
 public List<String> readData() {
    // read data from a MySQL database
  }
}
```
### Dependency Inversion Principle (DIP)
The DIP states that high-level modules should not depend on low-level modules, but both should depend on abstractions. In other words, you should use interfaces to decouple your code. For example, if you have a class that needs to read data from a database, you should depend on the DatabaseReader interface, not on the MySqlDatabaseReader class.
```java
public class UserService {
  private DatabaseReader databaseReader;

  public UserService(DatabaseReader databaseReader) {
    this.databaseReader = databaseReader;
  }

  public List<String> getUsers() {
    return databaseReader.readData();
  }
}
```
### Interrelationships between the principles

Now, let's discuss how these principles are interrelated:

SRP and OCP: These principles overlap because they both deal with how to structure classes. SRP says that a class should have only one responsibility, while OCP says that a
class should be open for extension but closed for modification. In order to follow both principles, you need to make sure that each class has a single responsibility and that you can extend the class without modifying the existing code.

OCP and LSP: These principles are closely related because they both deal with how to design classes for flexibility. OCP says that a class should be open for extension, while LSP says that a subclass should be substitutable for its parent class. In order to follow both principles, you need to design your classes in a way that allows for easy extension without breaking existing code.
LSP and ISP: These principles are related because they both deal with how to use interfaces in your code. LSP says that a subclass should be substitutable for its parent class, which means that you should use interfaces to define the contract between your classes. ISP says that a class should not be forced to implement methods that it doesn't need, which means that you should separate your interfaces into smaller, more specific interfaces.
ISP and DIP: These principles are related because they both deal with how to use interfaces in your code. ISP says that a class should not be forced to implement methods that it doesn't need, which means that you should separate your interfaces into smaller, more specific interfaces. DIP says that high-level modules should not depend on low-level modules, but both should depend on abstractions, which means that you should use interfaces to decouple your code.
While these principles are interrelated, they can also pull in different directions, leading to compromises. For example, following the SRP may require breaking the LSP, because a class may only have one responsibility but may not be substitutable for its parent class. In such cases, you may need to balance the principles and find a compromise that works best for your particular use case.

### The business builds up:
Evolve this over time, as new requirements are placed. So start off with perimeters for hollow shapes, areas for solid shapes, then computing costs . At each point, show examples where each  was violated, then give examples for how it is refactored.
Sure, here's an example of how the SOLID principles can be applied in a Java program that deals with different shapes.

Let's start with the following Shapes interface that defines two methods, perimeter and area:

```java
public interface Shapes {
    double perimeter();
    double area();
}
```
Let's say we have two types of shapes, Circle and Triangle, that implement this interface:

```java

public class Circle implements Shapes {
    private double radius;
    public Circle(double radius) {
        this.radius = radius;
    }
    public double perimeter() {
        return 2 * Math.PI * radius;
    }
    public double area() {
        return Math.PI * radius * radius;
    }
}

public class Triangle implements Shapes {
    private double base;
    private double height;
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    public double perimeter() {
        return 3 * base;
    }
    public double area() {
        return 0.5 * base * height;
    }
}
```
Now, let's say we need to calculate the cost of each shape. We could add a cost method to the Shapes interface:

```java
public interface Shapes {
    double perimeter();
    double area();
    double cost();
}
```
At this point, we may have violated the Interface Segregation Principle (ISP) because not all shapes have a cost. For example, a Circle may not have a cost associated with it. To address this, we could split the Shapes interface into multiple interfaces:

```java
public interface Perimeter {
    double perimeter();
}

public interface Area {
    double area();
}

public interface Cost {
    double cost();
}

public interface Shapes extends Perimeter, Area {
}

public class Circle implements Shapes, Cost {
    private double radius;
    private double costPerUnit;
    public Circle(double radius, double costPerUnit) {
        this.radius = radius;
        this.costPerUnit = costPerUnit;
    }
    public double cost() {
        return costPerUnit * area();
    }
}

public class Triangle implements Shapes, Cost {
    private double base;
    private double height;
    private double costPerUnit;
    public Triangle(double base, double height, double costPerUnit) {
        this.base = base;
        this.height = height;
        this.costPerUnit = costPerUnit;
    }
    public double cost() {
        return costPerUnit * area();
    }
}
```
Now, let's say we need to create a new type of shape, a Rectangle. We could add a new class that implements the Shapes interface:

```java
public class Rectangle implements Shapes, Cost {
    private double width;
    private double height;
    private double costPerUnit;
    public Rectangle(double width, double height, double costPerUnit) {
        this.width = width;
        this.height = height;
        this.costPerUnit = costPerUnit;
    }
    public double perimeter() {
        return 2 * (width + height);
    }
    public double area() {
        return width * height;
    }
    public double cost() {
        return costPerUnit * area();
    }
}
```

At this point, we may have violated the Open-Closed Principle (OCP) because we had to modify the Shapes interface to add the cost method, as well as modify the Circle, Triangle, and Rectangle classes to implement the Cost interface. To address this, we could use the Strategy pattern and create a separate CostCalculator class that can calculate the cost of any shape


