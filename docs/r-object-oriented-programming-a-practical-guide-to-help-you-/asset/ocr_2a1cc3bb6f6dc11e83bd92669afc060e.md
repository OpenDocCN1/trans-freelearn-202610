# Reading grades from a file

The last method examined is the method to read the data from a given file. We will examine the `Grades` list within the `Course` class before providing a listing. This slot is a list, and each element within the list is an assignment object. (The assignment classes are discussed in the next section of this chapter.) The name of the object within the list is the same as the name found in the first row of the CSV file.

The `ReadGrades` method first reads the `csv` file. It then goes through the columns that were read from the file, and the names of the columns are assumed to be in the first row of the file. If the first letters in the name of a column match one of the strings that are in the vector (found in the `GradeTypes` slot), then it is assumed that the column represents a graded item. There are two kinds of assignments: numerical grades or letter grades. If a column from the data file is determined to be a graded item, then its type is checked. If the type is a numeric type, then it is assumed that the course type is for a numeric grade (an object from the `NumericGrade` class); otherwise, it is assumed to be a letter grade (an object from the `LetterGrade` class):

```
```r
setGeneric(name="ReadGrades",
           def=function(course)
           {
               standardGeneric("ReadGrades")
           }
)
```
```

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
```