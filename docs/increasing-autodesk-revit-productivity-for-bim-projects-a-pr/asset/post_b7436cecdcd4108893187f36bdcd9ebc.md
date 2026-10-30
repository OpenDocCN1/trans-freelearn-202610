
When we have a list of drawings, the project team or the BIM coordinator can quickly create all the sheets with the correct names, numbers, statuses, and other information. **This process is essential for gaining productivity in delivering drawings.** We can automate this process to create multiple sheets directly in Revit, or we can use the Dynamo and Revit plugins to speed up this process.

The key point is to make it clear what deliverables will be produced to the design team and assign team members responsibilities regarding drawing production.

Once the drawings sheets have been created, the project team or the BIM coordinator can populate most of the sheets with Revit views, even if the views haven’t been completed or are very basic. As the project develops, the drawing sheets will start to take form, and the project manager can print the set of drawings to verify your progress.

This initial drawing set is called **Cartoon Set**, which helps you plan project deliverables. The Cartoon Set increases efficiency as we can ensure that the drawing set includes all the necessary information to be delivered.

As the projects are developed, the Cartoon Set will become a complete set of drawings, which helps ensure that we meet our deadlines smoothly and efficiently.

> **Tip**
> Creating multiple drawing sheets at the last minute before the deadline is a bad idea!

## Work with a Good Revit Template

The drawing's production efficiency will depend on the Revit template's quality, which will include line weight, view templates, title sheets, and other Revit settings.

In *Chapter 3, Revit Templates and Standards*, we learned how to create and improve the Revit template.

The industry and organization standards included in the template will promote consistency across the project(s) and align them with the graphical and technical standards.

## Control Drawing Visibility

We can control the drawing visibility in multiple ways, such as by using **View Templates** and other techniques, but a few processes can help improve productivity so that we can create drawings in PDF and DWG format.

- Shadows will increase the time to create PDF files. Turn off shadows if they are not required.
- Check if the linked files are necessary for a specific view. Revit’s performance will increase if we unload the unnecessary links.
- Ensure we have installed the latest printer driver or plugin installed to print PDF files. In *Chapter 9, Enhancing Productivity with Plugins*, we will learn more about Revit plugins.

In this section, we learned about three factors that can affect productivity when we're producing drawings in Revit.

## Summary

In this chapter, we learned about the Revit core functionalities and best practices to improve efficiency when working with schedules and keynotes for construction specifications. The schedule workflows were divided into Basic and Advanced to help cover the essential processes for managing schedules.

In Revit, keynotes are used to assign construction specifications to Revit components, thus increasing productivity and consistency when we're annotating drawings.

We then explained the best practices and workflows for Design Options, Phasing, and Scope Boxes, which help us to be more efficient in our daily activities when we're working with Revit. We also learned about workflows for Worksharing so that we can collaborate with multiple users working simultaneously in the same file.

After that, we presented the three main factors for increasing productivity during drawing production, including defining the drawing list, working with a good Revit template, and controlling the drawing's visibility.

In the next chapter, we will cover visual programming with Dynamo and learn how to automate specific tasks in Revit.

## Section 3:
Adding
Visualizations and
Dashboards to
Your Data

In this section, there are the following chapters:

- *Chapter 6, Visual Programming with Dynamo*
- *Chapter 7, Dashboard Analyses with Power BI*

In these chapters, you will gain the basic knowledge of Dynamo you need to increase your productivity in Revit, and you will also learn how to run multiple analyses in Power BI with Revit data.

The following topics will be covered:

- The basics of Dynamo BIM
- The importance of mastering Dynamo to increase efficiency in Revit
- How to export and import data from Revit to Power BI
- Analyzing Revit data with Power BI

## 6
Visual Programming with Dynamo

Visual programming enables users to create a script by changing graphical elements rather than text programming, which is the typical process for developing a code. Basically, we are creating algorithms without writing code:

![](img/1961add582dd4a589b2eac3dda140e38_315_0.png)

Figure 6.1 – Dynamo graphical elements

Users don’t require any programming experience to start using visual programming applications, such as Dynamo, and automate Revit tasks, but it requires dedication to learn and master a new process.

Dynamo is an open source software that enables visual programming for Revit, Civil 3D, and other software.

The principles of Dynamo follow three basic processes:

**INPUT + FUNCTIONS AND OPERATIONS + OUTPUT**

We will be able to increase productivity in Revit by using Dynamo to automate multiple workflows and complete repetitive tasks quickly.

This chapter will provide you with a basic knowledge of Dynamo and demonstrate the importance of mastering this application to increase efficiency in Revit.

The chapter will also provide you with an introduction to Dynamo, show the most commonly used Dynamo packages, and explain how to use certain workflows with Dynamo.

The following topics will be covered in this chapter:

- Dynamo
- Expanding Dynamo
- Best practices

## Technical requirements

In this chapter, you will need access to Autodesk Revit. The version used to describe all of these exercises is Revit 2021, but you can follow along with another version. It's important to take the following into consideration:

- Different versions might be slightly different from each other.
- Older versions of Revit cannot open files from an earlier version.

All datasets used for this chapter can be found in the book's GitHub repository at
`https://github.com/PacktPublishing/-Increasing-Autodesk-Revit-Productivity-for-BIM-Projects`.

## Introducing Dynamo

Productivity in business is a significant factor for a company's performance. Increasing productivity is a process for working smarter in a pre-defined time frame to match your project deadline.

Productivity methodologies are as follows:

- Time management
- Tasks completed by the workforce
- Personal performance
- Ratio to manufacture a product

Dynamo will increase productivity by improving your performance to complete a repetitive task and work with complex geometry.

Dynamo is an open source application that can be used as a standalone *Sandbox* or connected with other software, such as Revit, Maya, Civil3D, or any other application that has an **API** (short for **Application Programming Interface**) available.

It is designed to expand Revit's capability by automating processes and workflows. It will not add new tools to Revit, but it will help to automate existing tools for almost unlimited tasks and modeling processes.

Let's start by accessing Dynamo from Revit:

1. Open a new Revit project.
2. From the **Manage** tab, go to the **Visual Programming** panel.

![](img/1961add582dd4a589b2eac3dda140e38_317_0.png)

Figure 6.2 – Visual Programming panel

3. Click on **Dynamo.**

After clicking, the **Dynamo** window will appear:

![](img/1961add582dd4a589b2eac3dda140e38_318_0.png)

Figure 6.3 – Dynamo canvas

For more information about Dynamo, please refer to the following link:

```
https://bit.ly/3b9vxxa
```

QR code:

![](img/1961add582dd4a589b2eac3dda140e38_318_1.png)
