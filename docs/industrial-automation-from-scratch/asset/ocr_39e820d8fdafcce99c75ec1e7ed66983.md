be activated:

![](bbox=[0.12, 0.12, 0.85, 0.39])

Figure 8.6 – A program that combines both AND and OR logic

We will now briefly discuss the next programming language in our list.

# Instruction List (IL)

This is a text-based language that resembles assembly language. A series of instructions is listed just as assembly language. A program written using IL is processed faster than other programming languages defined by IEC for programming PLCs. One disadvantage of this programming language is that it's difficult to debug a large or complex program.

The following is a sample program written using IL. If input A or input B is true (ON) and input C is true (ON), output Y will be activated:

```
LD  A
OR  B
AND C
OUT Y
END
```