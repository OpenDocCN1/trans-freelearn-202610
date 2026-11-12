
- 5. Drag an Input Dialog activity to the project's sequence.

- 6. Do the following to accept and store InputNumber, as shown in the following screenshot:
  - Set Dialog Title to "Input Number".
  - Set Input Label to "Enter Input Number".
  - Set Input Type to Text Box.
  - Set Value entered to be stored in Counter:

![](img/84b06ad2832c53c5f677f8ce4c80f184_195_0.png)

Figure 6.40 – Updating the Input Dialog properties

- 7. Drag and drop the While activity from the Activities panel into the Designer panel, as shown in the following screenshot:

![](img/84b06ad2832c53c5f677f8ce4c80f184_195_1.png)

Figure 6.41 – Designer view of the While activity

- 8. In the **Condition** field, write `Counter < 10`.

- 9. In the **Body** branch, drag a **Write Line** activity and write `Counter.ToString`.

- 10. Drag an **Assign** activity and write `Counter = Counter + 1`.

The project's outline sequence should look as follows:

![](img/84b06ad2832c53c5f677f8ce4c80f184_196_0.png)

Figure 6.42 – Sequence outline

- 11. Click the **Run** button from the **Design** tab and follow the prompts to enter 1 as your input so that the final output looks as follows for a number that's less than 10:

![](img/84b06ad2832c53c5f677f8ce4c80f184_197_0.png)

Figure 6.43 – Output window for an input that's less than 10

- 12. Click the Run button from the Design tab and follow the prompts to enter 99 as your input so that the final output looks as follows for a number that's greater than 10, that is, 99:

![](img/84b06ad2832c53c5f677f8ce4c80f184_198_0.png)

Figure 6.44 – Output window for an input that's greater than 10

In this section, we have explored the Do While activity, which is used in many programmatic situations to loop through inputs or values until a specific condition is satisfied. In the next section, we will learn about the `For Each` activity to see how we can process each and every available piece of input data.

## For Each

The For Each activity helps you go through every item in your data collection defined in a list, array, or dictionary, as you learned in *Chapter 7, Manipulating Data using UiPath*. The statements inside the For Each activity are executed once for each item so that its operations are performed based on that item's information.

### Example of a For Each activity

In this example, we will create a sequence that performs actions on a list of numbers. Let’s get started:

- 1. Create a New Blank Process from the New Project section in UiPath Studio and name it For_Each_Example, as shown in the following screenshot:

![](img/84b06ad2832c53c5f677f8ce4c80f184_199_0.png)

Figure 6.45 – Creating a New Blank Process

- 2. Enter your preferred project Location to save the project.

- 3. Enter some details in the Description field and click Create to create the Designer panel.

- 4. Create a variable and name it Counters. Then, set Variable type to Array of [T] and select Int32 from the dropdown, as shown in the following screenshot:

| Name | Variable type | Scope | Default |
|---|---|---|---|
| Counters | Int32[] | Sequence | Enter a VB expression |

Figure 6.46 – Creating a variable

- 5. In the **Default** column of **Variables**, define the default list as {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}, as shown in the following screenshot:

| Name | Variable type | Scope | Default |
|---|---|---|---|
| Counters | Int32[] | Sequence | {1,2,3,4,5,6,7,8,9,10} |

Figure 6.47 – Setting the default values

- 6. Drag and drop the **For Each** activity from the **Activities** panel into the **Designer** panel, as shown in the following screenshot:

![](img/84b06ad2832c53c5f677f8ce4c80f184_200_0.png)

Figure 6.48 – Creating a For Each activity

- 7. Update the default value inside the **For Each** field to Counter and the **in** field to **Counters**, as shown in *Figure 6.49*. In the **Body** branch, drag a **Write Line** activity and write "The Current item in process is " + Counter.ToString. As the data type of each item of **Counters** is Int32, we need to convert them into String format for the item to be displayed in the **Write Line** activity.

The project's outline sequence should look as follows:

![](img/84b06ad2832c53c5f677f8ce4c80f184_201_0.png)

Figure 6.49 – Sequence outline

- 8. Click the **Run** button from the **Design** tab and follow the prompts to enter your input so that the final output looks as follows:

![](img/84b06ad2832c53c5f677f8ce4c80f184_201_1.png)

Figure 6.50 – Output window

In this section, we looked at the different types of loops that we can use to automate repeated tasks and enhance our automation effectively. These concepts can be applied to different situations, based on the automation requirements.

## Summary

In this chapter, we learned about the different types of control flow concepts, control flow statements such as loops, switches, and If statements, and their implementations. We also learned how to apply these control flow concepts. The examples that were provided in this chapter will help you decide which activity to use when you encounter similar data handling situations while creating robots.

In the next chapter, you will learn how to manipulate different types of data using control flow activities.

## 7 Manipulating Data Using UiPath

Today, every software application, business process, or human deals with data. Data is the most critical resource in every organization and it's available to us in many different forms. We need to modify, structure, format, or sort data to convert it into meaningful information in a business process.

This chapter focuses on introducing different methods of manipulating data based on various data types. Data used in a business process undergoes many transformational steps before it's ready for consumption at various stages of the process, hence it is crucial to understand how to perform such manipulations.

This chapter consists of the following sections:

- Explaining the concept of data manipulation
- Understanding string manipulation
- Understanding date and time manipulation
- Understanding list manipulation
- Understanding numeric calculations
- Understanding dictionary manipulation
- Understanding pattern-based data extraction using regular expressions (regexes)

### Technical requirements

As a prerequisite, please have UiPath Studio and UiPath Robot installed and connected with Orchestrator. Furthermore, it is essential to understand the different data types, control flows, and UiPath Studio features before starting this chapter.

### Explaining the concept of data manipulation

Data manipulation is a process that alters data through various operations to convert it into a usable format. It is a vital function for business operations and management as it provides many benefits, such as the following:

- Converts data into a more consistent format to improve reliability, readability, and organization
- Enables customization and generation of more meaningful data according to business requirements
- Removes unwanted data to ensure only the required information is processed

Data manipulation operations consist of four primary functions, as outlined here:

- Extracting data by specifying extraction conditions such as filtering, summarizing, and so on
- Adding new data
- Removing unwanted data
- Changing existing data by updating new values or converting unstructured data into a more meaningful and process-friendly format

There are many different methods of manipulating data, depending on the type of data. Let's now take a look at various data manipulation techniques.

### Understanding string manipulation

Strings are one of the most commonly used data types in any automation solution. A string data type refers to a series of text values that we frequently use to extract, display, process, and send data between multiple applications for further processing. String operations help perform business scenarios such as sending out emails for users, extracting a portion of text from a larger text, and so on.
