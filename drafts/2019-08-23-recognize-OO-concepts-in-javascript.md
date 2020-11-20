---
title:  "Recognize basic OO concepts in JavaScript"
categories:
tags: [Programming]
---

### 1 Some background about Object-Oriented Programming

If you are familiar with the concept of OOP and have tasted one or more OO languages before. You should know a class is basically a template which can be used to create objects. And these objects can get some states and behaviors from the class they instantiated from. It's a pretty neat concept that manifests in many OO languages.

> **[Object-oriented programming (OOP)](https://en.wikipedia.org/wiki/Object-oriented_programming)** is a programming paradigm based on the concept of "objects", which can contain data, in the form of fields (often known as attributes or properties), and code, in the form of procedures (often known as methods).

But if you've been acquainted with classic OOP approach, then you learned OOP in Javascript, you may feel there's something unnatural. That's not the OOP you learned before. JavaScript proclaims that it's a ["prototype-based" language](https://developer.mozilla.org/en-US/docs/Glossary/Prototype-based_programming). What is "prototypal-based"?

> [Prototype-based programming](https://en.wikipedia.org/wiki/Prototype-based_programming) is a style of object-oriented programming in which behavior reuse (known as inheritance) is performed via a process of reusing existing objects via delegation that serve as prototypes.

Since prototype-based programming is still *'a style of object-oriented programming'*. So it can't deviate too far from the basic concepts of OOP.

### 2 A basic OO example in JavaScript

Let's inspect some OO code in JavaScript. Javascript can use constructor to instantiate object instances. Normally, instances get states(properties) from their constructor and get behaviors(methods) from their constructor's prototype object -- You can inspect this prototype object by retrieve the constructor's `prototype` property.

```js
function Person(age, name) {
  this.age = age; // state(property)
  this.name = name; // state(property)
}

Person.prototype.act = function act() {  // behavior(method)
  let behavior = this.name + " is " + this.age.toString() + " years old and he is walking.";
  console.log(behavior);
}

Person.prototype // Person { act: [Function: act] }
var p = new Person(18, "Bob"); // instantiation
p.act(); // perform some behavior
// logs: Bob is 18 years old and he is walking.
```

First, Let us review some key terms in OOP:
- class: a template(blueprint) which can be used to create objects(instances of that class)
- instance: objects instantiated from their class by certain interface
  - states/attributes/properties: data held by an object
  - behaviors/methods: functions(procedures/code) held by an object
- instantiation: the process of creating an object from a class

### 3 Same OO case in different languages

Now let's see how to do similar thing in several other languages with OO style while observing the similarities across the different cases. Basically what all the code does is:

1. define a `Person` class which
  - defines a constructor(instantiation) method to set object instances' `age` and `name` states
  - defines a simple instance method `act`
2. instantiate a `Person` object
  - call `act` on the object which logs message `"Bob is 18 years old and he is walking."`

##### 3.1 The Python way

```py
class Person:
    def __init__(self, age, name):
        self.age = age
        self.name = name

    def act(self):
        behavior = self.name + " is " + str(self.age) + " years old and he is walking.";
        print(behavior)

p = Person(18, 'Bob')
p.act()
```

##### 3.2 The Ruby way

```ruby
class Person
  def initialize(age, name)
    @age = age;
    @name = name;
  end

  def act
    behavior = @name + " is " + @age.to_s + " years old and he is walking.";
    puts behavior;
  end
end

p = Person.new(18, 'Bob')
p.act
```

##### 3.3 The Java way

This is a bit different from the previous two, but only on syntactic level.

```java
public class TestPerson {
  public static void main(String[] args) {
    class Person { // just focus on the code below ----------------------------
      int age;
      String name;

      public Person(int age, String name) {
        this.age = age;
        this.name = name;
      };

      public void act() {
        String behavior = name + " is " + Integer.toString(age) + " years old and he is walking.";
        System.out.println(behavior);
      };
    }  // ---------------------------------------------------------------------

    Person p = new Person(18, "Bob");
    p.act();
  }
}
```

### 4 Commonalities over syntax difference in classic OO model

Despite the syntax differences, the 3 examples from python, ruby and java are analogous to each other. If you are familiar with basic OO concepts, you'll immediately recognize what each of the code snippet is doing. Because the OO concepts and thinking processes behind the scene are the same:

- class definition go first
- then objects can be instantiated from the class

### 5 Searching OO concepts in JavaScript

Here's the Js example code again:

```js
function Person(age, name) {
  this.age = age; // state(property)
  this.name = name; // state(property)
}

Person.prototype.act = function act() {  // behavior(method)
  let behavior = this.name + " is " + this.age.toString() + " years old and he is walking.";
  console.log(behavior);
}

Person.prototype // Person { act: [Function: act] }
var p = new Person(18, "Bob"); // instantiation
p.act(); // perform some behavior
// logs: Bob is 18 years old and he is walking.
```

Let's bring the basic OO concepts into this JavaScript example, see if we can find some of them.

##### 5.1 Constructor and instantiation

What is JavaScript's counterpart of `__init__`, `initialize` method? Method name with `init`(java's syntax is different) in it is often used as an interface to instantiate object from a class, it is usually called *constructor*.

In Javascript, `Person` function is constructor. In fact it shares the concept behind `__init__`, `initialize`. That's the method that will be called if we instantiate objects from the real `Person` class -- no matter in python or ruby or java. Although Java's syntax of defining constructor is similar to JavaScript's, but in Java -- like the other two languages -- the constructor is defined *inside* the class.

The most important thing to be remembered is to find **the interface used to instantiate object**. Whether this interface is hidden(e.g. in Ruby) or visible(e.g. in Js).

##### 5.2 Prototype and class

According to the other 3 languages' naming convention for a class, `Person` really looks like a class in JavaScript. Plus the syntax we instantiate `Person` object(`new Person(18, "Bob")`), it seems `Person` ought to be the class. But now we know it's a constructor -- the interface to instantiating object.

If `Person` is not the class, who is? Back to basic concepts again, since a class is where we write shared methods in and we did this in `Person.prototype` object, so it should be the conceptual class. And indeed it is.

### 6 What's the difference between the OO model in Js and in classic OO

First, Javascript first has an external constructor which is used to instantiate objects, and that constructor is exposed out of the class(prototype object), then it kind of hanging a `prototype`(conceptually, a class) on that constructor.

Second, it's not easy to determine the meaning of "prototype" in JavaScript without context. `prototype` is a property of a constructor. The syntax `Person.prototype` makes it feels like we are calling out the prototype(class) of `Person`, but what is the real prototype(class) of `Person`?

```js
Object.getPrototypeOf(Person) === Person.prototype; // false
Object.getPrototypeOf(Person);
// [Function]
```

What makes this worse is the `instanceof` operator. In our Js example, `Person` is a constructor, but `new Person(18, 'Bob') instanceof Person` returns `true`. `Person` seems having a dual identity: 1) the interface for instantiation; 2) a fake class reflects the type of an instance

> The `instanceof` operator tests whether the `prototype` property of a constructor appears anywhere in the prototype chain of an object.

It checks if `Person.prototype` appears anywhere in the prototype chain of an instance. If we were allowed to call something like `p instanceof Person.prototype` where `p` is a `Person` instance, things would be more clear. Unfortunately, this syntax throws: `TypeError: Right-hand side of 'instanceof' is not callable`.

### 7 How to get out of the swamp

##### 7.1 back to the basic concepts

The short answer is: *focus on the basic concepts and OO thinking process.*

Remember the commonalities of normal OO use cases are:

- class defines shared behaviors(for instances)
- object(instance) instantiated from class
- constructor
  - defines states/attributes/properties for instances
  - is the interface to instantiate new object of a class

Then we can do some simple logic inference in Js:


**Class:**

- constructors are functions(methods) --> not objects --> so they can't be conceptual class --> class is where defines shared methods --> we do this in `Constructor.prototype` object --> so that's the conceptual class

**Interface to instantiate objects:**

Logically, there will be an interface to instantiate objects. This interface may present in different ways, but once we recognize it, we see through the terminology and names. No matter we call it "constructor" or "initializer", no matter it named `init` or defined as public function, we know the purpose of its existence. Then it can become an anchor or clue to help us understand other related components.

**Behavior sharing:**

If we look up documentation of methods like `Array.prototype.map()`. According to the dot notation chain, we know `map` is defined as a property of `Array.prototype` object. But when we use it we can put an array instance in the front like `[1, 2, 3].map(...)`. A method defined in one object can be used by other objects which are actually instances of the former. That's a basic use case of classic OO model. Behaviors written in `Array.prototype` are shared to all `Array` instance. If we step back further, we may even drop the term "class" for a while. Whether it's called "class" or "prototype", it's just a place we can write shared behaviors.

##### 7.2 Think beyond terms

Terms may not make sense at all time. The term "prototype" in JavaScript may confuse us if we are not aware of the context. When we encounter the word "prototype" in JavaScript, we should slow down, examine what does "prototype" mean under the current context. But there's one thing keeps the same -- when we talk about 'prototype', we are talking about objects and class.

##### 7.3 Realize what causes your confusion

Here I draw a simplified graph to show you a main difference between classic OO model and OO JavaScript model.

![](https://image-for-articles.s3-ap-southeast-1.amazonaws.com/image-bucket-1/oojs.jpg)

What they both share is the basic OO concepts. However, according to the graph, one big difference is the visible parts are different. Classic OO model exposes *classes* to the audience while OO JavaScript exposes *constructors* to audience. And substitution of the `prototype` object of a constructor can't be seen if we only look at the constructor.

### Conclusion

Often, things on the surface may confuse us, but there's always something that can guide us -- the basic concepts and thinking process. If we keep these in mind, the next time we encounter another language featured OO with prototype-based(or whatever-based) way, we are able to figure out what're the basic concepts behind, as well as how they are applied in this language.
