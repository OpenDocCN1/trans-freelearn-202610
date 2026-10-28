
## Summary

We introduced the idea of an S4 class and provided several examples. The S4 class is constructed in at least two stages. The first stage is to define the name of the class and the associated data components. The methods associated with the class are then defined in a separate step.

In addition to defining a class and its method, the idea of inheritance was explored. A partial example was given in this chapter; it built on a base class defined in the first section of the chapter. Additionally, the method to call associated methods in parent classes was also explored, and the example made use of the constructor (or initialize method) to demonstrate how to build a chain of constructors.

Finally, four useful commands were explained. The four commands offered different ways to get information about a class or about an object of a given class.

For more information, you can refer to *Mobile Cellular Automata Models of Ant Behavior: Movement Activity of Leptothorax allardycei, Blaine J. Cole and David Cheshire, The American Naturalist.*

## Chapter 10. Case Study – Course Grades

This chapter is our first case study. We bring together the ideas from the previous chapters and provide an extended example. Some new ideas are introduced, and they should make more sense in the context of a full example.

This chapter is roughly divided into four parts:

- **The Course class**: This section gives you an overview of the S4 class that will contain a list of grades for a course. The grades are kept in a list with each graded task as a separate object.
- **The assignment classes**: This section gives you an overview of the S4 classes used to keep the grades for a specific graded task. This class has two derived classes. One derived class is to keep track of grades that have a numeric score, and the other is to keep track of grades that consist of letters.
- **Extending existing functions**: This section includes a brief discussion of how existing functions can be extended to react in an appropriate way to an object that is one of the assignment classes. We focus on the summary, plot, and show commands.
- **Extending operations**: This section includes a discussion on how to extend arithmetic and access functions by building on existing methods.

### Overview

All the previous chapters focus on specific topics, and here we bring together a number of different topics to examine an extended example. All of the classes examined here are S4 classes. These classes are used to read in a CSV file that contains the grades for students in a class. There is one class that defines how to read a grade file and how to interpret a column. Another set of classes is defined and the classes are used to track the information for a single assignment. An object of this class will include all the scores for the assignments of all students.

We first discuss the new Course class, which is used to keep track of all the assignments. Next, we discuss the assignment classes that are used to keep track of the grades for a single assignment. Once the classes are defined, the details on extending the summary, plot, and show commands are given to demonstrate how common commands can be extended to react when passed a newly defined object. Finally, some basic arithmetic and accessor operators are redefined so that the R system will react in an appropriate manner when using familiar operations, such as addition or multiplication.

The Course class includes objects whose type is one of the assignment classes, and it seems more natural to define the assignment classes first. The actions of the assignment classes are more intricate and include more details, while the Course class is relatively straightforward. To keep the introduction to the classes more gentle, we’ll first discuss the Course class.

### The Course class

The Course class is used to keep track of all of the grades for a course. This class will read in the information from a file, decide whether grades are numeric grades or letter grades, and create an appropriate assignment object to hold the grades. The details for the class are shown in the following figure. We’ll first discuss the data and then the methods associated with the class. We’ll then give some details on how to define the class. Most of the code associated with the accessors is omitted for the sake of brevity, but the full code is available on the website associated with this book.

![](img/18eeabe9ab8c81c5a3f4040db5906c67_215_0.png)

First, there are three data structures (slots) associated with the class. The first is the name of the CSV file that contains all of the grades for the class. The second is a vector of prefixes used to determine what kind of graded tasks are in the file. The last is a list that contains all of the assignments.

There are seven methods. All but one are used to set and get the values for the three slots. The accessor routines to get and set the filename are given here, and the others are not included in the text here. The last routine is used to read the grades, and it is a complex method that reads the data from the file and then determines whether the assignment has numerical or letter grades. It then defines an appropriate assignment object.

### The definition of the Course class

The Course class definition is given and then the accessors for the slots are stated. It is an S4 class. In this case, we do not provide any checks to ensure that the information in the slots is consistent to reduce the complexity of the example:

```
###########################################
# Create the Course class to keep track of all grades
Course <- setClass(
    # Set the name for the class
    "Course",

    # Define the slots
    slots = c(
        GradesFile = "character",
        GradeTypes = "character",
        Grades     = "list"
    ),

    # Set the default values for the slots. (optional)
    prototype=list(
        GradesFile = "",
        GradeTypes = c("test","hw","quiz","project"),
        Grades     = list()
    ),

    # Make a function that can test to see if the data is consistent.
    # This is not called if you have an initialize function defined!
    validity=function(object)
    {
        return(TRUE)
    }
)
```

Each of the slots includes a set of accessor functions to assist in retrieving or setting information tracked by the class. We only provide the details for one set of accessors. The methods to set and get the filename are given. Neither of these methods exist in the default R environment so they must be created first, as follows:

```
# Define the methods used to retrieve or set the values within a Course object.
setGeneric(name="GetFileName",
           def=function(course)
           {
               standardGeneric("GetFileName")
           }
)

setMethod(f="GetFileName",
          signature="Course",
          definition=function(course)
          {
              return(course@GradesFile)
          }
)
```

### Reading grades from a file

The last method examined is the method to read the data from a given file. We will examine the `Grades` list within the `Course` class before providing a listing. This slot is a list, and each element within the list is an assignment object. (The assignment classes are discussed in the next section of this chapter.) The name of the object within the list is the same as the name found in the first row of the CSV file.

The `ReadGrades` method first reads the `csv` file. It then goes through the columns that were read from the file, and the names of the columns are assumed to be in the first row of the file. If the first letters in the name of a column match one of the strings that are in the vector (found in the `GradeTypes` slot), then it is assumed that the column represents a graded item. There are two kinds of assignments: numerical grades or letter grades. If a column from the data file is determined to be a graded item, then its type is checked. If the type is a numeric type, then it is assumed that the course type is for a numeric grade (an object from the `NumericGrade` class); otherwise, it is assumed to be a letter grade (an object from the `LetterGrade` class):

```r
setGeneric(name="ReadGrades",
           def=function(course)
           {
               standardGeneric("ReadGrades")
           }
)
```

```r
setMethod(f="ReadGrades",
          signature="Course",
          definition=function(course)
          {
              grades <- read.csv(GetFileName(course))
              convertedGrades <- list()
              courseTypes <- GetGradeTypes(course)
              for (gradeItem in names(grades))
              {
                  # Go through each column from the file.
                  for (type in courseTypes)
                  {
                      # go through each course type and determine if
                      # this column is a quiz/test/hw?
                      if(length(grep(type,gradeItem))>0)
                      {
                          # The prefix for the name matches one of
                          # the predefined types.
                          if((class(grades[[gradeItem]])=="numeric" ||
                             (class(grades[[gradeItem]])=="integer")) {
                              thisItem <- NumericGrade()
                              #print(paste("This item,",gradeItem,
                              #   ",is a numeric grade item.",
                              #   class(thisItem)))
                          } else {
                              thisItem <- LetterGrade()
```

