worry about what goes on inside. Our code written before the update will still work after the update. If the manufacturers of a car get rid of the wheels and make it an electrically powered hover car, as long as it still has a steering wheel, accelerator, and brake pedal, driving it should not be a challenge.

When we use the classes of the Android API, we are doing so in the way the Android developers designed their classes to allow us to.

# Polymorphism

Polymorphism allows us to write code that is less dependent on the *types* we are trying to manipulate, making our code clearer and more efficient. Polymorphism means *different forms*. If the objects that we code can be more than one type of thing, then we can take advantage of this. Some examples later in the chapter will make this clear.

# Inheritance

Just like it sounds, inheritance means we can harness all the features and benefits of other people's classes, including encapsulation and polymorphism, while further refining their code specifically to our situation. Actually, we have done this already, every time we used the extends keyword:

```
public class MyActivity extends AppCompatActivity {
```

The AppCompatActivity class itself inherits from Activity. So, we inherited from Activity every time we created a new Android project. We can go further than this, and we will see how it is useful.