# There's more...

We will make a more realistic application using `om.next`.

# Task list using om.next

We will create a simple task list to see your tasks and due dates. A screenshot of the application is as follows:

![](bbox=[0.06, 0.28, 0.84, 0.61])

To create a new task, you will add a task description and due date. If you click a checkbox for the specific task, it means the task is finished. The **delete** buttons are to cancel tasks you have entered.

# Defining the namespace and libraries to use

Let's define the namespace and libraries to use:

```
(ns om-next-example.task
  (:require [goog.dom :as gdom]
            [om.next :as om :refer-macros [defui]]
            [om.dom :as dom]))
;;=> nil
```