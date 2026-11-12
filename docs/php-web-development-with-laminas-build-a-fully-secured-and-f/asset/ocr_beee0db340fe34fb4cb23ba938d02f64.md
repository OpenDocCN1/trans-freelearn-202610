Expressions such as ${...} are Eclipse variables. The ${workspace_loc} variable contains the complete path to the current Eclipse workspace. The ${string_prompt} variable, in turn, contains the text typed by a user in a dialog box. This dialog box opens when Eclipse runs the program. You can write a message to appear in this dialog box, putting text after the term string_prompt, preceded by a colon. So, for showing the Project Name message, you write ${string_prompt:Project name}. There are other variables, and you can see all of them by clicking on the Variables button.

The Main tab shows the working part of the program, but for the program to appear in the Eclipse toolbar, we have to select an option under the Common tab, as illustrated in the following screenshot:

![](bbox=[0.12, 0.27, 0.845, 0.705])

Figure 2.10 – Enabling the program as an item in the menu

As we can see in Figure 2.10, the Common tab has a frame named Display in favorites menu. You must check the External Tools option inside the frame for the program to be visible in the menu. After that, you can click on Apply to save the configuration.

Then, you can click on the External Tools button in the toolbar. This button's icon is a green play symbol with a red toolbox. As we can see in the following screenshot, the button will open a list with our Create Laminas Project program: