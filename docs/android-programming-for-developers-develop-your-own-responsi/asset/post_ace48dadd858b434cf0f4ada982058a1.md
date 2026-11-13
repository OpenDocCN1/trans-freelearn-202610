
## More code comments

As you become more advanced at writing Java programs, the solutions you use to create your programs will become longer and more complicated. Furthermore, as we will see in later chapters, Java was designed to manage complexity by having us divide up our code into separate classes, very often across multiple files.

**Code comments** are a part of the Java program that do not have any function in the program itself. The compiler ignores them. They serve to help the programmer document, explain, and clarify their code to make it more understandable to themselves at a later date, or to other programmers who might need to use or modify it.

We have already seen a single-line comment:

```
// this is a comment explaining what is going on
```

The previous comment begins with the two forward slash characters `//`. The comment ends at the end of the line. So, anything on that line is for humans only, whereas anything on the next line (unless it's another comment) needs to be syntactically correct Java code:

```
// I can write anything I like here
but this line will cause an error
```

We can use multiple single-line comments:

```
// Below is an important note
// I am an important note
// We can have as many single line comments like this as we like
```

Single-line comments are also useful if we want to temporarily disable a line of code. We can put `//` in front of the code and it will not be included in the program. Remember this code, which tells Android to load our layout?

```
// setContentView(R.layout.activity_main);
```

In the previous situation, the layout will not be loaded and the app will have a blank screen when run, as the entire line of code is ignored by the compiler.

There is another type of comment in Java known as the multi-line comment. The multi-line comment is useful for longer comments that span across multiple lines, and also for adding things such as copyright information at the top of a code file. Similar to the single-line comment, a multi-line comment can be used to temporarily disable code, in this case across multiple lines.

Everything in between the leading /* and the ending */ is ignored by the compiler. Here are some examples:

```
/*
You can tell I am good at this because my
code has so many helpful comments in it.
*/
```

There is no limit to the number of lines in a multi-line comment. Which type of comment is best to use will depend upon the situation. In this book, I will always explain every line of code explicitly in the text but you will often find liberally sprinkled comments within the code itself that add further explanation, insight, or context. So, it's always a good idea to read all the code thoroughly too:

```
/*
The winning lottery numbers for next Saturday are
9,7,12,34,29,22
But you still want to make Android apps?
*/
```

### Tip

All the best Java programmers liberally sprinkle their code with comments!

## Storing and using data with variables

We can think of a **variable** as a named storage box. We choose a name, perhaps variableA. These names are kind of like our programmer's window into the memory of the user's Android device.

Variables are values in memory ready to be used or altered when necessary by using the appropriate name.

Computer memory has a highly complex system of addressing, which fortunately we do not need to interact with. Java variables allow us to devise our own convenient names for all the data we need our program to work with. The DVM will handle all the technicalities of interacting with the operating system, and the operating system will, in turn, interact with the physical memory.

So, we can think of our Android device's memory as a huge warehouse just waiting for us to add our variables. When we assign names to our variables, they are stored in the warehouse, ready for when we need them. When we use our variable's name, the device knows exactly what we are referring to. We can then tell it to do things such as: get variableA and add it to variableC, delete variableB, and so on.

In a typical app, we might have a variable named unreadMessages, perhaps to hold the number of unread messages the user has. We could add to it when a new message arrives, take away from it when the user reads a message, and show it to the user somewhere in the app layout, so they know how many unread messages they have.

Some situations that might arise are:

- User gets three new messages so add three to the value of unreadMessages.
- User logs into app so use Toast to display a message along with the value stored in unreadMessages.
- User sees that a bunch of the messages are from someone she doesn't like and deletes six messages. We could then subtract six from unreadMessages.

These are fairly arbitrary examples of names for variables and as long as you don't use any of the characters or keywords that Java restricts, you can actually call your variables whatever you like.

In practice, however, it is best to adopt a **naming convention** so that your variable names will be consistent. In this book, we will use a loose convention of variable names starting with a lowercase letter. When there is more than one word in the variable's name, the second word will begin with an uppercase letter. This is called **camel casing**.

Something like:

- unreadMessages
- contactName
- isFriend

Before we look at some real Java code with some variables, we need to first look at the **types** of variables we can create and use.

## Types of variables

It is not hard to imagine that even a simple app will probably have quite a few variables. In the previous section, we introduced the unreadMessages variable as a hypothetical example. What if the app has a list of contacts and needs to remember each of their names? We might then need variables for each contact.

And what about when an app needs to know whether a contact is also a friend, or just a regular contact? We might need code that tests for friend status and then adds messages from that contact into an appropriate folder so the user knows whether they were messages from a friend or not.

Another common requirement in a computer program, including Android apps, is the right or wrong calculation. Computer programs represent right or wrong calculations using true or false.

To cover these and many other types of data you might want to store or manipulate, Java has types.

## Primitive types

There are many types of variables and we can even invent our own types as well. But for now, we will look at the most used built-in Java types. And to be fair, they cover just about every situation we are likely to run into for a while. These examples are the best way to explain types.

We have already discussed the hypothetical unreadMessages variable. This variable is of course a number, so we have to tell the Java compiler this by giving it an appropriate type. The hypothetical contactName will of course hold the characters that make up the contact name. Jumping ahead a couple of paragraphs, the type that holds a regular number is called int and the type that holds name-like data is called String. And if we try to store a contact name, perhaps "Ada Lovelace" in an int like unreadMessages, meant for numbers, we will certainly run into trouble, as we can see from the next screenshot:

![](img/35d01831c19688a44e50bce7c489007a_220_0.png)

As we can see, Java was designed to make it impossible for such errors to make it into a running program. With the compiler protecting us from ourselves, what could possibly go wrong?

Here are the main types in Java, then we will see how to start using them:

- int: The int type is for storing integers, whole numbers. This type uses 32 pieces (bits) of memory and can therefore store values with a magnitude a little in excess of two billion, including negative values too.
- long: As the name hints at, long data types can be used when even larger numbers are required. A long type uses 64 bits of memory and 2 to the power of 63 is what we can store in this. If you want to see what that looks like, here it is: 9,223,372,036,854,775,807. Perhaps, surprisingly, there are
