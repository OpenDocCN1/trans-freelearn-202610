# While loops

Java while loops have the simplest syntax. Think back to the if statements for a moment. We could put virtually any combination of operators and variables in the conditional expression of the if statement. If the expression evaluated to true then the code in the body of the if block is executed. With the while loop we also put an expression that can evaluate to true or false. Take a look at this code:

```
int x = 10;
while(x > 0){
x--;
// x decreases by one each pass through the loop
}
```

What happens here is this: outside of the while loop an int named x is declared and initialized to 10. Then, the while loop begins. Its condition is x > 0. So, the while loop will continue looping through the code in its body until the condition evaluates to false. So the previous code will execute 10 times.

On the first pass x = 10 then 9 then 8, and so on. But once x is equal to 0, it is of course no longer greater than 0, the program will exit the loop and continue with the first line of code after the while loop.

Just like an if statement, it is possible that the while loop will not execute even once. Take a look at this:

```
int x = 10;
while(x > 10){
// more code here.
// but it will never run unless x is greater than 10.
}
```

Moreover, there is no limit to the complexity of the conditional expression or the amount of code that can go into the loop body:

```
int newMessages = 3;
int unreadMessages = 0;
while(newMessages > 0 || unreadMessages > 0){
```