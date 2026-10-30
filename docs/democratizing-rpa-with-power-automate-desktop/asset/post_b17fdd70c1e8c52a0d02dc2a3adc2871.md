
- Power Automate browser extensions: https://learn.microsoft.com/en-us/power-automate/desktop-flows/install-browser-extensions
- Automate on virtual desktops: https://learn.microsoft.com/en-us/power-automate/desktop-flows/virtual-desktops
- Automate on virtual desktops: https://learn.microsoft.com/en-us/power-automate/desktop-flows/virtual-desktops?source=recommendations

## Actions for UI Automation

In the previous chapter, we learned about UI elements and how they can identify areas an application interacts with. In this chapter, we will highlight the possibilities of PAD that can be accomplished with actions for UI elements. This includes covering the following:

- Exploring the different UI actions and their purpose
- Using UI actions to enable data exchange (extract data and form filling) and automation between two applications

By the end of this chapter, you will understand what is required to identify the necessary buttons and UI elements and trigger them with UI actions to safely and reliably repeat the desired tasks and configure them in a flow.

### Why do we need UI actions?

Over time, countless applications for Windows were created, of which a great many still exist to this day. Microsoft itself has created excellent options for tailoring your applications with tools such as Visual Basic and Visual Studio. However, many of these applications are still not accessible via an API and have to be operated manually – also known as the human interface.

As we will see later in this book, PAD can work directly with numerous and widely used standard applications such as Office and Outlook, but it would be impossible to cover the wealth of different applications with dedicated actions. And that is the reason why the actions for UI elements exist. The term **UI automation** refers to the general capability of PAD to work with any application window and therefore remote control any application and control element.

To enforce this very general approach, the actions in this action group are correspondingly general and can be divided into the following parts:

- Identify a window
- Interact with application controls
- Extract data from a window
- Fill in data in a window

Let’s take a closer look at each of these actions before we dive into another example to leverage these actions.

### Identify a window

The first thing we need to do is get the window of the application we want to control. We already know that there are several ways to start a program, such as by utilizing the *run application* action or using a dedicated action for a program such as Excel or Outlook. The following actions identify a window or make it accessible for PAD, especially in combination with other windows:

- **Get window/focus window**: This can be found in the **Windows** subgroup. You can get or focus a running window and return a title and/or Window handle.
- **Set window state/set window visibility**: This can be found in the **Windows** subgroup. You can set the state of a window to **restored**, **maximized**, or **minimized**, which hides a window or makes it visible again, respectively.
- **Move/resize/close window**: Also in the **Windows** subgroup, this action can move a window to a specific position of the desktop by providing *X* and *Y* coordinates, can change the size of a window to a specific width and height, or just close a window.
- **If window/wait for window/wait for window content**: This action executes actions in a conditional block, depending on whether a window is open/focused or not, waits until a window is opened/closed/becomes the focus/loses focus, and waits for a specific UI element that should be contained in a window.

These actions are especially helpful in situations where an application might have some latency when opening or during the operation. If PAD were to access a UI element that hasn’t been displayed yet, these actions could be used to wait for the element to appear.

### Interact with a window

PAD also needs to be able to remote control an application. This means clicking on a button or area or dragging and dropping an element. This is possible with the following actions, which are self-explanatory:

- Select a tab in a window
- Hover the mouse over a UI element in a window
- Click on a UI element in a window
- Drag and drop a UI element in a window
- Expand/collapse a tree node in a window

These actions work with UI elements that have been previously defined; for example, with the recorder. In the example for this chapter, we will make use of some of these actions to remote control the application.

### Extract data from a window

We used some of these actions in one of our previous examples where we extracted the values of checkboxes in a Word document. There are more possibilities when it comes to extracting data from an application:

- **Get the details of window**: This action produces a variable containing properties of a window such as its title or source text. The desired property can be chosen in the actions dialog.
- **Get the details of a UI element in a window**: This action is the same as the previous one, except it’s for a specific UI element and different database and a Windows Desktop application. This scenario is illustrated in the following figure:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_162_0.png)

Figure 6.1 – Orders to Invoices structural view

In this scenario, a database application has been created with MS Access. This application may be used by multiple users through network sharing and contains information about customers and suppliers. The users can also store orders from the customers in this database, but the invoices to customers need to be processed via another application for some reason. This application is a Windows executable program that is running on the desktop of a user.

### Microsoft Access database as a source system

The database we are using in this example is a sample database provided by Microsoft. Starting MS Access from the program taskbar in Windows also provides a lot of sample databases from which the *Northwind* database is one of them. We can use the search function to look for this example, double-click it, and store it locally in our `Documents` folder.

This database contains a lot of functionality, starting with the possibility to have different user roles and nice dialogs to input data (see the following figure):

Northwind Login

Select Employee
Andrew Cencini
Login

Northwind Traders

I am: Andrew Cencini

- Quick Links
- View Inventory
- View Orders
- View Customers
- View Purchase Orders
- View Suppliers
- View Employees
- View Shippers
- Sales Reports

Active Orders

| Status | Date | Customer |
| --- | --- | --- |
| New | 25/04/2006 | Company C |
| New | 25/04/2006 | Company D |

Inventory to Reorder

| Product | Qty Available | Reorder Level |
| --- | --- | --- |
| Northwind Traders Boysenberry Spread | 0 | 25 |
| Northwind Traders Dried Pears | 0 | 10 |
| Northwind Traders Curry Sauce | 0 | 10 |
| Northwind Traders Fruit Cocktail | 0 | 10 |
| Northwind Traders Scones | 0 | 5 |
| Northwind Traders Beer | 0 | 15 |
| Northwind Traders Clam Chowder | 0 | 10 |
| Northwind Traders Chocolate | 0 | 25 |
| Northwind Traders Gnocchi | 1 | 30 |

Order List

Add New Order

| Status | Salesperson | Customer | Ship Date | Shipping | Taxes | Total |
| --- | --- | --- | --- | --- | --- | --- |
| New | Nancy Freehafer | Company A |  | 0.00 € | 0.00 € | 1,674.75 € |
| New | Nancy Freehafer | Company A |  |  |  | 736.00 € |

Order # (New)

Order Details

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_165_0.png)

Figure 6.2 – Northwind Access application

In our example, we assume that multiple users will add their orders to the database. But to create invoices out of these, the data must be transferred to the invoicing application. Neither application has built-in capabilities to transport the data elsewhere.

Having an Access database as one of the central applications is a very common scenario in smaller and even sometimes bigger companies. This is also true for the second application we are going to use in our scenario.

### Windows executable as the target system

We are going to use a sample application provided by Microsoft called Contoso Invoicing as the target system. This is just an example of an ordinary Windows application with specific characteristics such as a navigation tree, a menu, a toolbar, and some forms to view and capture data. Please refer to the Further reading section for more details on how to install this application, which looks like this:
