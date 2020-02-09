---
layout: post
title: Concepts that I have never forget about Object Oriented Programming (OOP)
tags: [Java, OOP]
---

#### Classes

A class is a prototype from which objects are created. Models the state and behavior of a real world object.

Java syntax:

```java
class MyClass{
          //---here body class
}
```

#### Objects

An object is a Software bundle with state and behavior. Software objects are used to model the real word objects, for example, a desktop lamp, a bicycle, my sister’s dog (rufino), etc.. There are other objects more complex.

Java syntax:

```java
MyClass myObject = new MyClass();
```

#### Inheritance

Inheritance is used to organizing and structuring your software.

Java syntax:

```java
class MyClass extends MySuperClass{
     //--here body class    
}
```

#### Abstraction

An abstract class contains at least one abstract method. Abstract methods contain no implementation. Abstract methods are implemented in a way specific to each inherited class.

Java syntax:

```java
public abstract class MyAbstractClass{
    public abstract void MyAbstractMethod();
}
public class MyClass extends MyAbstractClass{
    public void MyAbstractMethod(){
        // here the body
    }
}
```

#### Interfaces

An Interface is a contact between a class and the outside world. In other words an Interface is a group of related methods with no implementation. Basically an Interface is an abstract class that contains all abstract methods, but none of the methods in the interface need to be declared abstract and are not extended, they are implemented by other classes.

Java syntax:

```java
interface MyInterface {
//--methods with empty bodies.
}
class MyClass implements MyInterface{
// interface's  methods implementation
}
```

#### Polymorphism (Many forms)

For example, imagine you are programming a video game, in which you have to save a cat, at the end of the game you have to choose between three doors, behind door one is a tiger, behind door two is a lion and behind door three is a cat. In this game an animal object may have three different forms(behaviors) at run time.
