
TypeScript cannot guarantee that input is valid JSON. That is a runtime concern.

To summarize error handling:

* In JavaScript, you write code, you run it, but then you must handle errors when they happen.
* In TypeScript, you describe your assumptions in types, many mistakes are caught before running anything, but you still handle truly unpredictable failures at runtime.

**Good practice**: Use types to prevent predictable errors. Use `try` and `catch` for unpredictable ones. Do not use `try` and `catch` to hide type problems.

For example, this is a bad pattern:

```typescript
try {
  user.name.toUpperCase();
} catch {}
```

The better fix is to design the type correctly:

```typescript
if (user && user.name) {
  user.name.toUpperCase();
}
```

Or better yet:

```typescript
function formatUser(user: { name: string }) {
  return user.name.toUpperCase();
}
```

TypeScript does not make your code error-free. It changes when and how errors show up:

* Predictable errors are caught early by types.
* Unpredictable errors must be handled at runtime.

## Interfaces and the DOM

Interfaces become especially useful when dealing with structured data, such as form state or API responses:

```typescript
interface TodoItem {
  id: number;
  text: string;
  done: boolean;
}
const todos: TodoItem[] = [];
```

From this point on, your editor knows exactly what a to-do item looks like. Autocomplete becomes accurate, and mistakes become harder to make.

## Summary of basic types and interfaces

At this stage, the goal is not to memorize every possible type. It is to understand the mindset:

* Types describe intent.
* Interfaces describe shape.
* Errors move earlier in time.

TypeScript does not change how your code runs. It changes how confidently you can change it. Once you internalize that, types and interfaces become useful checks while you edit your code.

**Prompt**: Please explain union types and intersection types in TypeScript with simple, real-world examples.

Now that you’ve seen the basic types and how to define your own interfaces for your custom types, let’s practice what you’ve learned with some exercises.

# Hands-on exercise: making TypeScript work for you

This exercise stays small so you can see how types and interfaces guide you while you edit.

## Step 1: Initialize a TypeScript project

In the `Chapter06` folder, create a new folder named `TypesExercise`. In the folder, initialize a TypeScript project, as shown in the following command:

```bash
tsc --init
```

## Step 2: Create a new TypeScript file

In the `Chapter06` folder, in the `TypesExercise` folder, create a file called `profile.ts`.

We will start with a simple interface that describes a user profile:

```typescript
interface Profile {
  name: string;
  email: string;
  isActive: boolean;
}
```

This interface is your contract. Everything else in this file will revolve around it.

## Step 3: Create a value that must match the interface

Add the following below the interface:

```typescript
const profile: Profile = {
  name: "Mark",
  email: "mark@example.com",
  isActive: true
};
```

Now deliberately break it. For example, change the `isActive` property value to a string like `"true"`. Your editor should immediately complain, as shown in *Figure 6.3*:

![](Images/B34553_06_03.png)

*Figure 6.3: TypeScript in VS Code finding a type error*

Fix the error and move on.

TypeScript is enforcing the shape of your data before the code ever runs.

## Step 4: Add a function with typed parameters and a return value

Now write a function that works with a `Profile`:

```typescript
function describeProfile(profile: Profile): string {
  const status = profile.isActive ? "active" : "inactive";
  return `${profile.name} (${profile.email}) is ${status}`;
}
```

Call the function and log the result:

```typescript
console.log(describeProfile(profile));
```

Compile the project and then run it with Node.js, as shown in *Figure 6.4* and the following commands:

```bash
tsc
node profile.js
```

![](Images/B34553_06_04.png)

*Figure 6.4: Successfully running the profile project*

Open `profile.js` and confirm what you see. The interface is gone. The types are gone. Only JavaScript remains.

**Good practice**: Remember this: interfaces exist to help you write correct code, not to change runtime behavior.

Try passing a variable into `describeProfile` that does not match the interface, for example, a string. Watch where the error appears, as shown in *Figure 6.5*:

![](Images/B34553_06_05.png)

*Figure 6.5: Error when passing a wrong data type to a function*

## Step 5: Introduce an optional property

Extend the interface to support an optional last login date, as highlighted in the following code:

```typescript
interface Profile {
  name: string;
  email: string;
  isActive: boolean;
  lastLogin?: Date;
}
```

Update your data, as highlighted in the following code:

```typescript
const profile: Profile = {
  name: "Mark",
  email: "mark@example.com",
  isActive: true,
  lastLogin: new Date()
};
```

Now update the function so it handles both cases, as highlighted in the following code:

```typescript
function describeProfile(profile: Profile): string {
  const status = profile.isActive ? "active" : "inactive";
  if (profile.lastLogin) {
    return `${profile.name} (${profile.email}) is ${status}. Last login: ${profile.lastLogin.toDateString()}`;
  }
  return `${profile.name} (${profile.email}) is ${status}. Last login unknown.`;
}
```

**Warning!** TypeScript forces you to handle the missing case explicitly by checking for the existence of the optional property.

Recompile the project and rerun it with Node.

## Step 6: Work with arrays of typed objects

Now introduce a list of profiles, and then loop over the list and log descriptions:

```typescript
const profiles: Profile[] = [
  {
    name: "Ilya",
    email: "ilya@boston-raiders.com",
    isActive: true
  },
  {
    name: "Shane",
    email: "shane@montreal-voyageurs.com",
    isActive: false
  }
];
for (const p of profiles) {
  console.log(describeProfile(p));
}
```

Try pushing an invalid object into the array and see what happens.

## Optional challenges

If you want to go further, here are one or two small extensions for the profile project:

* Add a `role` field that only allows `admin`, `editor`, or `viewer`.
* Write a function that filters only active profiles.
* Change `email` to be optional and update the function accordingly.

Each task makes you use the type system to describe the data more carefully. If you get stuck, ask your AI coach to explain the error before asking it to fix the code.

By the end of this exercise, you have:

* Defined and used basic types.
* Created and enforced an interface.
* Worked with optional properties.
* Used typed functions and arrays.
* Seen exactly what disappears when TypeScript compiles to JavaScript.

By this point, TypeScript should look less abstract because you have seen it catch real mistakes.

**Prompt**: What are common mistakes beginners make when first using TypeScript?

# Practice, references, and resources

In this section, you will be tested on what you’ve learned in this chapter and be provided with useful links to reference materials and resources.

## Check your understanding

Answer the following questions:

1. What problem does TypeScript solve that JavaScript does not?
2. What does it mean that TypeScript is a development-time tool?
3. Why is TypeScript especially useful when working with AI-generated code?
4. What is type inference, and when should you rely on it?
5. Why is it recommended to be explicit with function parameters and return types?
6. What does it mean that interfaces describe the shape of data?
