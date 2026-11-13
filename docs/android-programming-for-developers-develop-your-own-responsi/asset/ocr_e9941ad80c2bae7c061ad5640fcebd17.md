# # Variable declaration

That's enough theory. Let's see how we would actually use our variables and types. Remember that each primitive type requires a specific amount of real device memory. This is one of the reasons that the compiler needs to know what type a variable will be. So, we must first declare a variable and its type before we attempt to do anything with it.

To declare a variable of type `int` with the name `unreadMessages`, we would type:

```
int unreadMessages;
```

That's it, simply state the type, in this case `int`, then leave a space and type the name you want to use for this variable. Note also the semicolon `;` on the end of the line will tell the compiler that we are done with this line and what follows, if anything, is not part of the declaration.

Similarly, for almost all the other variable types, declaration would occur in the same way. Here are some examples. The variable names in the examples are arbitrary. This is like reserving a labeled storage box in the warehouse:

```
long millisecondsElapsed;
float accountBalance;
boolean isFriend;
char contactFirstInitial;
String messageText;
```