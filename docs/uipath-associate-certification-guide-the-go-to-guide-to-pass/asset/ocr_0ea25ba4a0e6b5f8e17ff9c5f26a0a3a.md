Let's take a look at how to loop through items and update each item in a list. In our scenario, we will update items in the list by converting them to uppercase letters. Proceed as follows:

+   1. Search for a **For Each** activity from the **Activities** panel and place it below the **Add To Collection** activities.

+   2. Change the `TypeArgument` property in the **Properties** panel of the **For Each** activity to `String`. This property indicates the data type of each element within the list. Hence, our list is a **String** list, so we have to change the property to **String**.

+   3. Configure the `Values` property of the **For Each** activity by indicating the `CombinedPeopleList` list variable. It also updates the **In** segment of the **For Each** activity with the same configuration. The `item` variable specified in the **ForEach** segment of the loop is a particular variable that is only applicable within the **For Each** loop. The `item` variable holds the data element of the current iteration during execution.

+   4. Search for an **Assign** activity from the **Activities** panel and place it inside the **Body** container of the **For Each** activity.

+   5. Since we need to update the item in the list, let's place the `item` variable in the **To** segment of the **Assign** activity.

+   6. Configure the **Value** segment of the **Assign** activity using the following code, which converts the value in the `item` variable to uppercase:

```
StrConv(item, VbStrConv.Uppercase)
```

+   7. Create another list variable of type `String` through the **Variables** panel and name it `ModifiedList`.

+   8. Change the **Scope** field of the `ModifiedList` variable to the outermost sequence.

+   9. Change the default value of the `ModifiedList` variable to `New List (Of String)` to initialize the `List` variable as a string list.

+   10. Place another **Add To Collection** activity below the **Assign** activity in the **For Each** loop.

+   11. Configure the `Collection` property of the activity by specifying the newly created `ModifiedList` variable.

+   12. Configure the `Item` property of the activity by providing the `item` variable. The **Assign** activity above the **Add to Collection** activity converts the value in the `item` variable to uppercase and overwrites its value. Hence, we are using the same variable to add the updated value to the new collection variable.