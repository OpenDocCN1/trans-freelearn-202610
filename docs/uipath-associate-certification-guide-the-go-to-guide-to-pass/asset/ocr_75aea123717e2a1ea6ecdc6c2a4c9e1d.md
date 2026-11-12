16. Run the workflow and observe the results in the **Output** panel. You should see the total shows up as a whole number, whereas the average consists of decimal values.

There are many functions available in .NET Framework to perform numeric calculations. The example provided here is fundamental. It is intended to give you a high-level idea of performing some numeric manipulations such as rounding up values, calculating totals, and deriving averages. The knowledge you already gained through this chapter will give you the foundation to explore other functions in the Math library and the different other functions available for numeric calculations. Explore other methods and familiarize yourself with those functions as they will be useful when building automation solutions for your organization.

While numeric data types have their unique capabilities, `Dictionary`-type variables offer a unique way of managing data using the key-value pair concept. The next section of the chapter explains how to manipulate data using `Dictionary` variables.

# Understanding dictionary manipulation

The `Dictionary` type has a key difference compared with `List` and `Array` types. While lists and arrays hold multiple data elements of the same data type, a dictionary can hold each data element along with a unique key. This combination is named **key-value** pairs. The user chooses the data type of the key and the value during variable creation.

The `Dictionary` type is mainly used for storing configuration details or other similar information. You may wonder why you should not use a `List` type to store and access configuration data. In real-world scenarios, each configuration data element has a descriptive name that describes what the configuration represents. For example, if the configuration file has data on **Uniform Resource Locators (URLs)**, timeouts, and file paths, the file may look like this:

| Key | Value |
| :--- | :--- |
| BusinessURL | https://www.google.com |
| InputExcelFolderPath | C:\Invoice Files\ |
| ProcessTimeout | 10000 |

Figure 7.15 – Sample structure of a configuration file