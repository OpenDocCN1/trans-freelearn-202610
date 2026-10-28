
## Matching Pictures

- 12. The three rendered 3D models will now appear in Inkscape's drawing area, as shown in the next screenshot:

  ![](img/a18a528f2a8e54d48b64e97eaf62e865_55_0.png)

- 13. Select File | Save from Inkscape's main menu. Save the file as `image010201.svg` in the previously created folder, `C:\School`.

### What just happened?

We combined three rendered 3D models in Inkscape's drawing area. We searched for 3D models in Google 3D Warehouse and we imported the bitmap files obtained from the desired views into Inkscape's drawing area.

We now have a picture with these three rendered 3D models, representing real-life shapes:

- A school bag
- A book
- A calculator

### Time for action – organizing composite 3D models into a rendered 2D box

We are now going to move, resize, and save the new version of the combined rendered 3D models for the exercise using Inkscape.

- 1. Open the vector graphic file in Inkscape (`C:\School\image010201.svg`).
- 2. Resize and move each picture to create a drawing, as shown in the following screenshot:

  ![](img/a18a528f2a8e54d48b64e97eaf62e865_56_0.png)

- 3. Select File | Save, to save the changes made to the original Inkscape SVG file.
- 4. Select File | Export Bitmap. A dialog box showing many export options will appear. Enter 90 in the first dpi (Dots Per Inch) text box.
- 5. Click on the Drawing button, and then on Export. Inkscape will export the drawing in PNG format. The exported bitmap graphic with a school bag, a book, and a calculator will be `C:\School\image010201.png`.

### What just happened?

We manipulated the bitmap images of the rendered 3D models using Inkscape. We could change their size and positions and we exported the resulting image to the PNG format.

The rendered 2D box is the exported bitmap graphic and represents a 3D scene.

### Time for action – grouping text blocks and changing colors

It is time to create the text blocks with the possible answers for a pop quiz related to the 3D scene.

- 1. Start Hot Potatoes and click on JQuiz. A new window with the JQuiz application will appear.
- 2. Enter `Discovering sentences related to 3D scenes` in the **Title** textbox.
- 3. Select **Multiple-choice** in the combo box on the upper-right corner.
- 4. Click on the textbox below the title's textbox on the right-hand side of question number 1.
- 5. Select **Insert | Picture | Picture from local file** from JQuiz's main menu. Select the previously exported bitmap image `image010201.png` in `C:\School` and click on the Open button. A dialog box will appear displaying the image preview and some options.
- 6. Click on OK. JQuiz will show the following HTML code in the textbox:

  ```html
  <img src="image010201.png" alt="image010201.png"
       title="image010201" width="640" height="504"></img>
  ```

- 7. Click on the corresponding **Answers** textbox and enter the text shown in the next table for each row, as seen in the next screenshot:

| Row label | Text to enter in the Right (jumbled) items textbox |
|---|---|
| A | `<h2 style="color:green"><br><br><em>There is a calculator and a book in the school bag.</em><br><br></h2>` |
| B | `<h2 style="color:green"><br><br><em>There is a calculator and a book under the school bag.</em><br><br></h2>` |
| C | `<h2 style="color:green"><br><br><em>There is a calculator and four books next to the school bag.</em><br><br></h2>` |

![](img/a18a528f2a8e54d48b64e97eaf62e865_58_0.png)

- 8. Activate the **Correct** checkbox corresponding to the row labeled with a B. This is the correct answer for the multiple-choice style quiz.
- 9. Select **File | Save** from JQuiz's main menu. Save the file as `quiz0102.jgz` in the previously created folder, `C:\School`.
- 10. Next, select **File | Create Web Page | Standard Format**. JQuiz will create a new web page. Save the new file as `quiz0102.htm` in the aforementioned folder.

### What just happened?

We inserted the image with the 3D scene and we prepared the sentences for our discovering sentences exercise, using a multiple-choice format.

We used JQuiz to edit and preview the following three sentences as possible answers below the 3D scene:

- There is a calculator and a book in the school bag.
- There is a calculator and a book under the school bag.
- There is a calculator and four books next to the school bag.

We used our well-known HTML tags again to improve the fonts and color used for the sentences.

### Time for action – adding the activity to a Moodle course

We now have to add the discovering sentences exercise to an existing Moodle course.

- 1. Log in to your Moodle server and make sure that the **Hot Potatoes Quiz** activity module isn't hidden.
- 2. Click on the desired course name (School) and then on **Turn editing on**.
- 3. Position the mouse pointer over the desired week under **Weekly outline** (a week later than the matching composite pictures exercise). Then, click on the **Edit summary** icon (a small hand with a pencil).
- 4. Enter *Exercise 2* in **Summary** and click on the **Save changes** button.
- 5. Click on the **Add an activity** combo box for the selected week and choose **Hot Potatoes Quiz**.
- 6. A new web page will appear displaying the title **Adding a new Hot Potatoes Quiz**. Click on the **Choose or upload a file...** button and a pop-up window displaying information about files and folders will appear.
- 7. Click on **chapter01** (the folder created in our previous exercise).
- 8. Click on the **Upload a file** button and then on **Browse**. Browse to the folder that holds the images and the files used in the exercise (C:\School) and select the file to upload, `image010201.png`. Then, click on **Open** and on the **Upload this file** button.
- 9. Repeat the aforementioned step for the file generated by JQuiz, `quiz0102.htm`.
- 10. Next, position the mouse pointer over the **quiz0102.htm** name and move it horizontally to the **Choose** action hyperlink in the same row. Then, click on **Choose**, as shown in the next screenshot:

  ![](img/a18a528f2a8e54d48b64e97eaf62e865_61_0.png)

- 11. Moodle will display `chapter01/quiz0102.htm` in the **File name** textbox. This is the web page that will run the exercise previously created using JQuiz.
- 12. Scroll down and click on the **Save and display** button, located at the bottom of the web page. The web browser will show the discovering exercise with a multiple-choice format, as shown in the next screenshot:

  ![](img/a18a528f2a8e54d48b64e97eaf62e865_61_1.png)

### What just happened?

We added the discovering sentences related to 3D scenes exercise to a Moodle course. Now, the students are going to be able to run the activity by clicking on its hyperlink on the corresponding week.

We used the previously created folder, `chapter01`. We uploaded the bitmap image with the rendered 3D scene and the HTML file created with JQuiz (`quiz0102.htm`).

### Organizing the exercises' files using folders

We created a folder, `chapter01`, in which we uploaded all the files for both the matching composite pictures and the discovering sentences exercises. However, sometimes, we need to organize the exercises' files using additional folders.

For example, we can organize the files using the following structure:

- ◆ chapter01
  - □ exercise01
  - □ exercise02

We can create two sub-folders, `exercise01` and `exercise02`, in the `chapter01` folder. This way, all the files related to the second exercise would have to be uploaded in the `chapter01/exercise02` folder.

> The organization scheme chosen for the files will depend on the number of exercises and their relationship with your courses. However, in order to simplify your work, it is a good practice to keep all the files related to an exercise in a single folder. You can work with files in different folders; however, it will require you to work with complex paths in HTML code.

### Time for action – running the discovering sentences activity

It is time to run the discovering sentences activity as a student.

- 1. Click on the course name (School) and switch your role to student.
- 2. Click on the **Discovering sentences related to 3D scenes** link on the corresponding week. The web browser will show the multiple-choice exercise.
