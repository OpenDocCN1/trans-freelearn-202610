# # Method overrides

Method overrides are a way to replace functionality that is inherited from super-classes. Method overrides must have the same call signature as the method that is being overridden. Take a look at the following example:

```
```typescript
interface IEmployee {
    name: string;
    email: string;
    work(tasks: string[]);
}
class Employee implements IEmployee {
    constructor(public name: string, public email: string) {
    }
    public work(tasks: string[]) {
        for (var i = 0; i < tasks.length; i++) {
            //perform task
        }
    }
}
interface IManager extends IEmployee {
    employees: IEmployee[];
}
class Manager extends Employee implements IManager {
    public employees: IEmployee[] = [];
    constructor(name: string, email: string, employees: IEmployee[]) {
        super(name, email);
        this.employees = employees;
    }
    public work(tasks: string[]) {
        for (var i = 0; i < this.employees.length; i++) {
            for (var j = 0; j < tasks.length; j++) {
                this.employees[i].work([tasks[j]]);
            }
        }
    }
}
```
```

In this example, you can see that we have two classes. First, a generic Employee class that represents the base class for any employee within an organization. This employee has a few properties such as a name and an e-mail address to contact them with. Then, second, there is a very simple method that allows the employee to be passed a list of tasks, and this task list will be executed sequentially until all of the tasks are complete. The next class is the Manager class, which is a subclass of Employee. The manager has all of the same functions as a normal employee, they just also have a list of employees that they are responsible for. The manager is handed a list of tasks, and rather than performing each one explicitly, it will delegate each task to each of its employees for execution. If we were to add or remove parameters from the work method, then the call signatures would be different between the subclass and superclass and would break the Liskov substitution principle. TypeScript will not stop you from changing the method signature, however, it will make your code harder to manage.