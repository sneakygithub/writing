Polymorphism sure is a thing. It's a thing that lets us do a lot of other things. That sentence may seem vague and useless, but it's actually a human language representation of what polymorphism does. If we look at it in another form it might look something like this: 

```java
class Polymorphism implements Thing {
  ...

  @Override
  public Other thingMethod()...
}
```

 In object oriented programming, polymorphism usually means that multiple objects have a method with the same name and different behavior. In _Practical Object-Oriented Design in Ruby_, Sandi Metz defines Polymorphism as
>...the ability of many different objects to respond to the same message. Senders of the message need not care about the class of the receiver; receivers supply their own specific version of the behavior.

While this is a useful description of what Polymorphism is, it's only half of the information we need. A couple of paragraphs later, Metz also says
>Polymorphic methods honor an implicit bargain; they agree to be interchangeable from the sender's point of view. Any object implementing a polymorphic method can be substituted for any other; the sender of the message need not know or care about this substitution.

Although saying that polymorphism is one function name that covers many different behaviors is sufficiently descriptive of the concept, Sandi Metz's making explicit the "implicit bargain" makes the concept actually usable in one's code.

Here's an example of polymorphism that follows the "implicit bargain": You tell a ballerina and a tap dancer both to "dance," and the ballerina does ballet while the tap dancer taps. In both cases, the dancer is moving and performing, and while these behaviors aren't completely interchangeable, they are close enough that the "sender" doesn't need to care which one is happening. On the other hand, if you tell two people to "lie" and one says "The sky is green" and the other gets in a prone position, that's a problem. In this case you have one person returning a string and the other one changing state. These things are not interchangeable and can have effects later on in the program. While there could be an argument that this _technically_ may still fall under the category of polymorphic since both objects (people) respond to the same message ("lie"), it certainly doesn't fall under that definition in any practical or useful way.

In OOP you can implement polymorphism through inheritance (subtype polymorphism). A Dancer class can have the function dance, and anything that inherits from Dancer can have its own specific implementation of that function.


```ruby
class Dancer
  def dance
    "moves to the music"
  end
end

class Ballerina < Dancer
  def dance
    "plié, jeté"
  end
end

class TapDancer < Dancer
  def dance
    "tappity tap tap"
  end
end

class Performance
  attr_reader :performers

  def innititalize(args)
    @performers = args[:performers]
  end

  def perform
    performers.each {|performer| performer.dance}
  end
end
```

As we can see above, I know nothing about dancing, but also we can initialize `Performance` with an array of `Dancer` objects that can be generic dancers or of any subclass. They all have a `dance` method and the show will go on!

In languages that aren't strongly typed, like Ruby, you can use duck typing for the same thing and not even need inheritance. A butterfly and an airplane probably shouldn't inherit from the same superclass, but they can both fly, and maybe in some point in the code, an object that can fly gets passed in, and that's all that matters. 

Statically typed OOP languages can't do duck typing because duck typing relies on a dynamic type system, but it does have what's called ad hoc polymorphism, where it's defined by the interface that multiple unrelated data-types can implement instead of through inheritance. So they can implement, say, a Flyable interface in the case of the butterfly and the airplane. Through interfaces or inheritance, polymorphism is usually 'safer' in statically typed languages because one doesn't even have to worry about the "implicit bargain". This is because the return value is made _explicit_, as is the rest of the method signature, so there's no messing it up that way.

In functional programming languages, since we don't have objects and classes, things have to work a little differently, but polymorphism can still happen. One functional language, Clojure, has something called multi-methods that can switch what happens based on the arguments given. For example, going back to dancers:


```clojure
(defmulti dance (fn [dancer] (:skills dancer)))

  (defmethod dance :ballet [_]
    "plié, jeté")

  (defmethod dance :tap [_]
    "tappity tap tap")

  (defmethod dance :default [_]
    "moves to the music")
```

And now we can call whichever one we want as long as we throw the right arguments in. Clojure also has something called protocols that work a lot more like objects -- where multi-methods allow you to have a single method with multiple implementations, protocols allow you work polymorphically with groups. And Clojure isn't the only functional language that works like this.

But not _all_ functional languages work like that. They can't all do ad hoc polymorphism, and sometimes tools like pattern matching are used to allow for the multiple behaviors that polymorphism solves for. That being said, there's yet another type of polymorphism, and this one is most apparent in statically typed functional programming languages. This type is called parametric polymorphism, and it is a bit of a mind-bender coming from an OOP world. 

Ad hoc and subtype polymorphism follow the rule that there's a certain amount known about the data types implementing the method -- that they share an interface or that they have a shared parent type -- but the body of the method is up to each of those types. In parametric polymorphism, it's kind of reversed. It's the idea that you have a function and that the types can be just about anything. 

A great example of this that bridges gaps between the OOP and FP worlds is `map` - sometimes called `Select` in languages like C#. Its type signature, following FP conventions using lower-case letters for "any" types, the way someone might use `<T>` in Java, and `f` standing for "a collection of" looks like this: 

```
 f a  -> (a -> b) -> f b
```

That is, it takes in a collection that is of type `a`, and `a` can be _anything_ and also takes a function that takes `a` (though now this is the same type as what we originally passed in) and returns... something. Really, in this instance `b` means anything. It could be our original value, it could be an entirely new data type -- it doesn't matter. Then the whole function returns a collection of type `b`. Again, it's `map`: it takes a collection of a thing and then turns it into a collection of another thing.

So how does this help? Well if you can write functions that have signatures that are this generic, then you can reuse the same function with a lot of different types.  And if they're a higher-order function like `map` (a function that takes another function as an argument), then you can also vary the behavior of the function based on what gets passed in, again achieving the goal of writing useful code that isn't 100% dependent on strict implementation. 

To sum up, in ad hoc and subtype polymorphism there are methods defined by the type that calls them, and in parametric polymorphism there is a defined function that can be called by many types. In both cases, the "form" of the function is defined by the context, of which there are many, hence poly (many) morph (form) ism (action). Or, you know, a thing that lets us do a lot of other things.
