# # Generic objects

If your goal when adopting TypeScript is to create an API for client consumption, then generics are the language construct you should become the most familiar with. They allow consumers of frameworks and APIs to agree on a specific contract to accomplish a designated goal without having to know specific object types. Generics define the third and final category of objects in TypeScript, type parameters. As we saw earlier in this chapter when we discussed arrays, type parameters are defined inside of < and > characters. They can be used on interfaces or class definitions to create functionality for a broad set of object types that may or may not be known at the time of development. Let’s say we wanted to create a generic task processor. We will assume the simplest possible implementation, which is just a sequential process that passes a list of generic tasks to a function to have them run:

```
```typescript
interface ITask {
    Id: number;
    Execute(): boolean;
    Error: string;
}
function ProcessTasks<T extends ITask>(tasks: T[]): T {
    for (var i = 0; i < tasks.length; i++) {
        if (tasks[i].Execute() == false) {
            return tasks[i];
        }
    }
    return null;
}
```
```

In this example, we have defined an interface that the task processor will rely on to execute a set of tasks sequentially. The function declaration for ProcessTasks now contains a type parameter, T, which must adhere to the ITask interface. This type parameter is used as a constraint for the objects that can be passed in as part of the tasks array, and will be the return type of the function. The tasks array is looped through one by one and the Execute method is called. If the Execute method returns a false value then the processing is stopped and the task is returned. Generics are built completely on type annotations, so they have no effect on the generated JavaScript. However, if the type constraints are not met, a compile error will be generated.

Interfaces and classes are capable of creating even more complex structures built around a very loose type definition. A type parameter provided to an interface or class can be used to provide type information for any member or function inside of the declaration body. In the next example, our ProcessTasks function has been moved into a class that will operate as the processor. A generic interface is created that will allow us to create any number of task processors with different implementations of the ProcessTasks function. Then, we see a class declaration that provides its own set of type parameters and implements the ITaskProcessor interface:

```
```typescript
interface ITask {
    Id: number;
```
```