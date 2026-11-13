# # Abstract classes

An abstract class is a class that cannot be instantiated and therefore cannot be
made into an object. So, it's a blueprint that will never be used, then? But that's
like paying an architect to design your home and then never building it! You
might be saying to yourself, "I kind of got the idea of an abstract method, but
abstract classes are just silly."

If we, or the designer of a class, wants to force us to inherit _before_ we use their
class, they can declare a class **abstract**. Then, we cannot make an object from it;
therefore, we must extend it first and make an object from the subclass.

We can also declare a method abstract and then that method must be
overridden in any class that extends the class with the abstract method.

Let's look at an example; it will help. We make a class abstract by declaring it
with the abstract keyword, like this:

```
abstract class someClass{
/*
All methods and variables here.
As usual!
Just don't try and make
an object out of me!
*/
}
```

Yes, but why?

Sometimes, we want a class that can be used as a polymorphic type, but we need
to guarantee it can never be used as an object. For example, Animal doesn't
really make sense on its own.

We don't talk about animals; we talk about types of animals. We don't say, "ooh,
look at that lovely, fluffy, white animal!" Or, "yesterday we went to the pet shop
and got an animal and an animal bed." It's just too, well, _abstract_.

So, an abstract class is kind of like a template to be used by any class that
extends it (inherits from it).