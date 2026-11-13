
## Questions

If you have a problem with any aspect of this course, you can contact us at <questions@packtpub.com>, and we will do our best to address the problem.

## Part 1. Module 1

## *Android Programming for Beginners*

Learn all the Java and Android skills you need to start making powerful mobile applications

## Chapter 1. The First App

Welcome! In this chapter, we won't waste any time in getting started with developing Android apps.

We will look at what is so great about Android, what Android and Java are exactly, how they work and complement each other, and what this means to us as future developers.

After this, we will spend a little time setting up our development environment and then get straight to building and deploying our first app.

By the end of this chapter, we will have done the following:

- Set up the **Java Development Kit (JDK)**, part of the required Android development environment
- Installed Android Studio, the final part of our Android development environment
- Built our very first Android app
- Deployed an Android emulator
- Run our app on an Android emulator and a real device

### How Java and Android work together

After we write a program in Java for Android, we click on a button to change our code into another form that is understood by Android. This other form is called Dalvik Executable (DEX) code, and the transformation process is called compiling. Compiling takes place on the development machine after we click on that button. We will see this work right after we set up our development environment.

Android is a fairly complex system, but you do not need to understand it in depth to be able to make amazing apps. The part of the Android system that executes (runs) our compiled DEX code is called the Dalvik Virtual Machine (DVM). The DVM itself is a piece of software written in another language that runs on a specially adapted version of the Linux operating system. So what the user sees of Android, is itself just an app running on another operating system.

The purpose of the DVM is to hide the complexity and diversity of the hardware and software that Android runs on but, at the same time, its purpose is to expose all of its useful features. This exposing of features generally works in two ways. The DVM itself must have access to the hardware, which it does, but this access must be programmer friendly and easy to use. The way the DVM allows us access is indeed easy to use because of the Android Application Programming Interface (API).

### The Android API

The Android API is the code that makes it really easy to do exceptional things. A simple analogy could be drawn with a machine, perhaps a car. When you step on the accelerator, a whole bunch of things happen under the hood. We don't need to understand about combustion or fuel pumps because a smart engineer has provided an **interface** for us. In this case, a mechanical interface—the accelerator pedal.

Take the following line of Java code as an example; it will probably look a little intimidating if you are completely new to Android:

```
locationManager.getLastKnownLocation(LocationManager.GPS_PROVIDER);
```

However, once you learn that this single line of code searches for the available satellites and then communicates with them in orbit around the Earth while retrieving your precise latitude and longitude on the planet, it is easy to begin to glimpse the power and depth of the Android API in conjunction with the DVM. Even if that code does look a little challenging at the moment, imagine talking to a satellite in some other way!

The Android API is mainly a whole bunch of Java code. So, how do we use all this code to do cool stuff without getting swamped by its complexity? How do we find and manipulate the pedals, steering wheel, and sunroof of the Android API?

> **Note**
>
> There are many different estimates to the number of lines of code that have gone into Android. Some estimates are as low as 1 million, some as high as 20 million. What might seem surprising is that, despite this vast amount of code, Android is known in programming circles for being "lightweight".

### Java is object-oriented

Java is a programming language that has been around a lot longer than Android. It is an object-oriented language. This means that it uses the concept of reusable programming objects. If this sounds like technical jargon, another analogy will help. Java enables us and others (such as the Android development team) to write Java code that can be structured based on real-world "things" and, here is the important part, it can be reused.

So, using the car analogy, we could ask the question: if a manufacturer makes more than one car in a day, do they redesign each and every part for each and every car?

The answer, of course, is no. They get highly skilled engineers to develop exactly the right components that are honed, refined, and improved over years. Then, that same component is reused again and again, as well as occasionally improved. Now, if you are going to be picky about my analogy, then you can argue that each of the car's components still have to be built from raw materials using real-life engineers, or robots, and so on.

This is true. What the software engineers actually do when they write their code is build a blueprint for an object. We then create an object from their blueprint using Java code and, once we have that object, we can configure it, use it, combine it with other objects, and more. Furthermore, we can design blueprints and make objects from them as well. The compiler then translates (manufactures) our custom-built creation into DEX code.

In Java, a blueprint is called a class. When a class is transformed into a real working thing, we call it an object.

> **Note**
>
> **Objects in a nutshell**
>
> - Java is a language that allows us to write code once that can be used over and over again.
> - This is very useful because it saves us time and allows us to use other people's code to perform tasks we might otherwise not have the time or knowledge to write for ourselves.
> - Most of the time, we do not even need to see this code or even know how it does its work!

One last analogy. We just need to know how to use that code, just as we only need to learn to drive the car.

So, a smart software engineer up at Google HQ writes a desperately complex Java program that can talk to satellites. He then considers how he can make this code useful to all the Android programmers out there. One of the things he does is he makes features such as getting the device's location in the world a simple one-line task. So the one line of code we saw previously sets many more lines of code in action that we don't see. This is an example of using somebody else's code to make our code infinitely simpler.

### What exactly is Android?

We know that to get things done on Android, we write Java code of our own, which also uses the Java code of the Android API. This is then compiled into DEX code and run by the DVM, which in turn has connections to an underlying operating system called Linux.

Then the manufacturers of the Android devices and individual hardware components write advanced software called drivers, which ensure that their hardware (CPU, GPU, GPS receivers, and so on) can run on the underlying Linux operating system.

Our compiled Java code, along with some other resources, is placed in a bundle of files called an **Android application package (APK)**, and this is what the DVM needs to run our app. This process is explained in the following figure:

![](img/35d01831c19688a44e50bce7c489007a_46_0.png)

In summary, all we need to do is learn how to read and code Java, so we can begin to learn and take advantage of the Android API.

All these tools are free, so let's take a look at the development environment we will be using.
