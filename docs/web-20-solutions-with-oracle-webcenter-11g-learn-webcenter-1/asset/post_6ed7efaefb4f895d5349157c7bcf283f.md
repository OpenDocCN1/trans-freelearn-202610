

5. In the Login page, enter the username: eduardo.pulido and password: oracle. Click Login.

![](img/fccf6c511126cf79e98510edd3b343ac_247_0.png)

6. Next, you can see the Oracle Personal Spaces environment.

![](img/fccf6c511126cf79e98510edd3b343ac_247_1.png)

7. On the left side, Oracle Personal Spaces shows a toolbar called Sidebar. This tool provides quick access to resources that Oracle WebCenter Spaces offers.

![](img/fccf6c511126cf79e98510edd3b343ac_248_0.png)

8. The elements of the Sidebar are as follows:

- **Application**: Provides access to business applications used by the user. It includes Single Sign-On for saving passwords.
- **Worklist**: Often users participate in workflows, as approvals and reviews. This taskflow or portlet will provide access to the flow pending user approval.
- **Mail**: Link to user e-mail or third-party e-mail like Yahoo and Google along with their corporate e-mail.
- **Buddies**: Access to a list of their instant messaging contacts.
- **Notes**: Allows the user to create brief notes.
- **Recent Documents**: The documents that the user has accessed recently are shown in this region.
- **Discussions**: Access to hot and favorite topics associated.
- **Tags**: Gives access to pages through their tags and provides direct access to Tag Center.

9. On the right side, Oracle Personal Spaces shows the pages belonging to the connected user. Note that there are two pages by default, **Welcome** and **Documents**, which are shown by tabs.

10. The first page is an informative page, and the second allows us to access the document repository configured in the previous step.

![](img/fccf6c511126cf79e98510edd3b343ac_248_1.png)

## 11. To configure their pages, the users can use the option Page Actions | Manage Pages....

![](img/fccf6c511126cf79e98510edd3b343ac_249_0.png)

Note that this window allows you to manage all user pages. In addition, users also can access the Role Based Pages in WebCenter Spaces. These are pages that get assigned to a role within a company. Thus, if the user belongs to the sales role, then he or she will have automatic access to pages exclusively for this role.

## 12. Let us use this to create a new page. Click on the Create Page button.

![](img/fccf6c511126cf79e98510edd3b343ac_249_1.png)

## 13. Enter the name of the page as Custom Information and the scheme as Dunes. Click on the Create button.

![](img/fccf6c511126cf79e98510edd3b343ac_249_2.png)

# Oracle WebCenter Spaces Overview

14. Change the layout of the page by clicking on this link: **Change Layout | Two columns above....**

![](img/fccf6c511126cf79e98510edd3b343ac_250_0.png)

15. Note that we now have three regions to be used by the user, in order to place data associated with the purchase of natural fibers. To begin, in the upper left corner, click on the button **Add Content**.

![](img/fccf6c511126cf79e98510edd3b343ac_250_1.png)

16. In the catalog of window components, click on the **Layout** option.

![](img/fccf6c511126cf79e98510edd3b343ac_250_2.png)

17. In the next window, click on the **Add** button associated with the Text control and then press the **Close** button to close the catalog of components.

![](img/fccf6c511126cf79e98510edd3b343ac_251_0.png)

18. Note that you have created a text component on this new page. Next, click on the **Edit Text** link and add information on the types of natural fiber on the market. You can select and format any paragraph of text. Complete this change by clicking on the **Done Editing** link.

![](img/fccf6c511126cf79e98510edd3b343ac_251_1.png)

19. Now, in the upper right portion, we are going to put news on the availability of natural fibers in the world. To do this, we will use the RSS service. Next, click on the **Add Content** button in the upper right region.

![](img/fccf6c511126cf79e98510edd3b343ac_251_2.png)

20. Click on the **Add** link for the RSS control. Then, press the **Close** button to close the catalog.

![](img/fccf6c511126cf79e98510edd3b343ac_251_3.png)

# Oracle WebCenter Spaces Overview

21. Once the RSS control is created, you must configure it. To do this, click the **Edit** link (pencil icon).

![](img/fccf6c511126cf79e98510edd3b343ac_252_0.png)

22. Complete the **RSS Feed URL** field, with an address that contains the desired information. In our case, we use the following URL:
`http://rss.globalsources.com/I/Cotton-fiber-manufacturers/r/2000000003844/3000000179301/21806/RSS.xml.`

![](img/fccf6c511126cf79e98510edd3b343ac_252_1.png)

23. Press **OK** to apply the changes.
24. Finally, in the lower region of the page, the user will place files used in the purchase process that users carry out. To do this, click on the **Add Content** button at the bottom.

![](img/fccf6c511126cf79e98510edd3b343ac_252_2.png)

25. Choose the category **Documents**.

![](img/fccf6c511126cf79e98510edd3b343ac_252_3.png)

26. Click on the **Add** link for **Documents** and press the **Close** button.

![](img/fccf6c511126cf79e98510edd3b343ac_252_4.png)

# Chapter 10

27. Now customize this control to show only the user's directory. To do this, click on the **Edit** button associated with the Document control.

![](img/fccf6c511126cf79e98510edd3b343ac_253_0.png)

28. In **Start Folder Path** field, click on the **Edit** button.

![](img/fccf6c511126cf79e98510edd3b343ac_253_1.png)

29. Choose the values in the next screen to define, as the root of the repository, a directory with the username. Press **OK** in the open windows.

![](img/fccf6c511126cf79e98510edd3b343ac_253_2.png)

30. Finally, click on the **Close** button located on the top right of the page.

![](img/fccf6c511126cf79e98510edd3b343ac_253_3.png)

31. Accept the changes made on the page by clicking the **Save** button.

![](img/fccf6c511126cf79e98510edd3b343ac_253_4.png)

## 32. Finally, you will appreciate the following changes in the user page.

![](img/fccf6c511126cf79e98510edd3b343ac_254_0.png)

![](img/fccf6c511126cf79e98510edd3b343ac_254_1.png)

# Building a business role page

As we have mentioned in the introduction, the business role pages are a kind of pages that have the capability to be included in the work environment of all users who have a specific role. This way we can target information to a predetermined group of users, avoiding the need to allocate one by one.

In this example, we will define the role Purchaser and assign users to this role. Then we will create a business role page that lists the best suppliers of the company, which will be available to all users of the Purchaser role.

# Hands on

- 1. Go to the Oracle Spaces login page: `http://localhost:8888/webcenter`.
- 2. In the same way as you created the `eduardo.pulido` user, press the Register button and proceed to create users `martin.alban` and `diego.munoz`, both with password `oracle`.

![](img/fccf6c511126cf79e98510edd3b343ac_255_0.png)

- 3. After the registration, return to the initial page and log in as the admin user, in our case, the user **weblogic**.

![](img/fccf6c511126cf79e98510edd3b343ac_255_1.png)

# Oracle WebCenter Spaces Overview

4. Once in the main environment, click the **Administration** link.

![](img/fccf6c511126cf79e98510edd3b343ac_256_0.png)

5. In the window displayed, choose the option **Manage WebCenter Users and Roles**.

![](img/fccf6c511126cf79e98510edd3b343ac_256_1.png)

6. Then, it will show the security tab for defining roles and users used by Oracle Spaces. Press the **Create Role** link.

![](img/fccf6c511126cf79e98510edd3b343ac_256_2.png)

# Chapter 10

7. Next, enter **Purchaser** and choose the template **Spaces-User**. Press **OK**.

![](img/fccf6c511126cf79e98510edd3b343ac_257_0.png)

8. After creating the role, we will register the users who belong to that role. To do this, click on the **Users** tab.

![](img/fccf6c511126cf79e98510edd3b343ac_257_1.png)

9. This page defines the users who belong to a role. Enter **eduardo.pulido**, select the role **Purchaser**, and click **Grant Access**. In this way, the user is associated with the role.

![](img/fccf6c511126cf79e98510edd3b343ac_257_2.png)

10. Repeat for users **martin.alban** and **diego.munoz**.

11. After registering the users, it should display the following information at the bottom of the page.

![](img/fccf6c511126cf79e98510edd3b343ac_257_3.png)

