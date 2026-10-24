In the TypeScript file of ListComponent, we have created the definition of the headers and an example list of antiHeroes for the anti-hero list to display. This will only be temporary, as we just want to test whether our presentation component displays the list of anti-heroes successfully. We have also created the selectAntiHero() function for future implementation once a specific anti-hero is selected.

Let’s now define the input and output properties for AntiHeroList. Let’s have a look at the following code:

# list.component.html

```
<!-- Dumb component anti hero list -->
<app-anti-hero-list [antiHeroes]="antiHeroes" [headers]="headers" (antiHero)="selectAntiHero($event)"></app-anti-hero-list>
```

Now, in the HTML file of ListComponent, we have bound headers and antiHeroes to the properties of app-anti-hero-list. We have also used the selectAntiHero() function to catch the event once antiHero has emitted an action.

After successfully implementing our presentation component, we can run the application and open the application in the browser. We should see the following results:

![](bbox=[0.33, 0.5, 0.7, 0.565])

We can see in the results that our presentation component has successfully displayed the data coming from the parent component. For the form component, we will implement its functionalities in the next chapter, as creating forms will be another different topic.

Now, we have learned about the concepts, structure, and implementation of smart and dumb components. In the next section, we will now use a UI framework that will help us improve the interface of our Angular application.

# Adding Angular Material

We have already organized our Angular application using the core, feature, and shared architecture at the module level and the smart and dumb architecture at the component level. Now, we are ready to customize and improve the look and UI by styling our components. We all know that writing CSS code from scratch and developing a base style is another challenge for us developers. It creates additional effort for us beyond just worrying about the logical side of the code. This is where Angular Material saves the day!