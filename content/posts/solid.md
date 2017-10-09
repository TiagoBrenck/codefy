---
layout: post
title: S.O.L.I.D Principles
description: "The definition of SOLID principles and how important it is on an OO architecture."
date: 2017-09-22
tags: [Object Oriented]
image:
  feature: /images/post-background.png
---

Object Orientation has so many principles and theories behind it, that is quite impossible to have an architecture that uses 100% of it. To decide which principles should be ignored, you have to consider how complex is your app, how often will it be updated, how many users will access it and many others considerations. However, the **SOLID** principles fit on almost all scenarios and should be considered to be implemented on every architecture because the benefits of it are countless. Furthermore, the time you will spend implementing them is minimal.   

The 5 SOLID principles are:

- **S** -> [Single Responsibility principle](#single-responsibility)
- **O** -> [Open/Closed principle](#open-closed)
- **L** -> [Liskov Substitution principle](#liskov-substitution)
- **I** -> [Interface Segregation principle](#interface-segregation)
- **D** -> [Dependency Inversion principle](#dependency-inversion)

___

## Single Responsibility
A class should have only one reason to change. In other words, a class should be responsible for only one subject and has only functions related to that subject. For example, on an e-commerce app, a class called PricingServices should only have functions related to price. Submitting an order isn't its responsibility so it shouldn't have any code or business rules for that.
### Benefits
By adopting the Single Responsibility principle, you are ensuring that you have a level of clean code on your project, helping future maintenances easier to be done and less prone to bugs. Also, the principle makes your classes be more robust.   
___
## Open Closed
Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modifications, that is, such an entity can allow its behavior to be extended without modifying its source code. You want your code to be easier to be extended as possible and not having to modify the whole application.

Using the same PricingServices example that we used above, you have a class that knows how to price your selling products. Since your e-commerce only sells food, the tax rate to be applied on every product is fixed. If your company starts selling kitchen gadgets, you would have two categories of products with different taxes rates from each other. A good usage of the Open/Closed principle for this scenario is to have the class PricingServices' methods expecting a generic type of product. Using this approach, the prices calculations wouldn't be based on a fixed type of product and the class would be opened to deal with any kind of products, without having its code modified.

### Benefits
By using the Open/Closed principle, you are increasing the scalability of the software and reducing its coupling. Changes are easy to be implemented and you have the safety that the new extension won't brake any existing business rules.  
___   

## Liskov Substitution
 The object of a derived class should be able to replace an object of the base class without bringing any errors in the system or modifying the behavior of the base class. This principle is about following the contract of the base class.

 An example of a class structure which violates Liskov Substitution is:

 ```
 public interface IDuck
 {
    void Swim();
    // contract says that IsSwimming should be true if Swim has been called.
    bool IsSwimming { get; }
 }

 public class OrganicDuck : IDuck
 {
    public void Swim()
    {
       //do something to swim
    }

    bool IsSwimming { get { /* return if the duck is swimming */ } }
 }

 public class ElectricDuck : IDuck
 {
    bool _isSwimming;

    public void Swim()
    {
       if (!IsTurnedOn)
         return;

       _isSwimming = true;
       //swim logic  

    }

    bool IsSwimming { get { return _isSwimming; } }
 }
 ```
 And the calling code:

  ```
  void MakeDuckSwim(IDuck duck)
  {
      duck.Swim();
  }
  ```
As you can see, there are two examples of ducks. One organic duck and one electric duck. The electric duck can only swim if it's turned on. This breaks the Liskov Substitution principle since it must be turned on to be able to swim as the IsSwimming (which also is part of the contract) won't be set as in the base class. [Code Source](https://stackoverflow.com/questions/4428725/can-you-explain-liskov-substitution-principle-with-a-good-c-sharp-example)

### Benefits
Similar to Open/Closed principle, the benefits of using Liskov are to increase the scalability of the software. Also, it ensure that future new subclasses won't break any tested code.
 ___
## Interface Segregation
It states that no client should be forced to depend on methods it does not use. Basically the idea is to split interfaces that are very large into smaller and more specific ones. This principle has a small similarity with Single Responsibility, however it is focused on interfaces.

A super generic interface, that represents everything in your business world, is actually a spaghetti interface. If you have a vehicle subject app, and decides to create an interface that wraps the behavior of all kind of vehicles, you could come with something like this:

```
public IVehicle
{
  void StartEngine();
  void Accelerate();
  void Break();
  void ChangeGear(int newGear);
  void TurnRadioOnOff();
  void EjectCD();
}
```

Having that interface as example, if you have a car, that has radio with CD feature and is manual, the interface works fine. However, An automatic car is also a vehicle. As well as motorcycles and buses. They can only implement some of the interface contracts, while the others would have to return null or some illogical rule.

### Benefits
The benefits of using Interface Segregation resume to a clean and more cohered code. Adopting this principle may result into creating more interfaces, but on the other hand, you won't have concrete classes that implement an interface but are not actually represented by that interface.
___

## Dependency Inversion
High-level modules should not depend on low-level modules. Both should depend on abstractions.
Abstractions should not depend on details. Details should depend on abstractions.

Design your software in modules, the 3 tier architecture is a good example, and make them independent of each other. If one module should send data or use another module, do it through interfaces and dependency injection.

In a real world scenario, you have your business module containing all the business rules, validations and stuff. This module needs to get data from the database and send them back updated. Is it important to the business module what DB are you using? Or, if you are using an ORM such as Entity Framework or NHibernate? The answer should be no, because rules like new users must have a valid email, or the field foo is mandatory, should be applied if your DB is MYSQL, SQL Server, Mongo or whatever. So, to accomplish the dependency inversion, you create an interface for your DB module, which the business module will be dependent on. With that architecture, if you change your DB from MySql to SQL Server for example, your business module will not need any modification.

### Benefits
Adopting the Dependency Inversion principle, you increase your software scalability, decrease its coupling and create independent modules that can be modified or re-utilized without stress. 
