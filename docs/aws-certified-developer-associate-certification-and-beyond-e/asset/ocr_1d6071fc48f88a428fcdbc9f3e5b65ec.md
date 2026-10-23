3. Next, you must edit your CloudFront distribution ID to your distribution ID. You will find the ID in your Amazon CloudFront console, as per *Figure 12.41*:

![](bbox=[0.148, 0.154, 0.872, 0.342])

Figure 12.41 – Identifying your CloudFront distribution ID

+   4. Next, ensure that you save the file.
+   
+   5. Open your Terminal window (if you are using a Mac) or Command Prompt (if you are using Windows) and navigate to the frontend code folder. List your directories and files to ensure you are in the correct folder containing the `buildspec.yml` file you just updated.
+   
+   6. Next, type in the `git add .` command and press *Enter*.
+   
+   7. Next, type in the `git commit -m "buildspec file updated"` command and press *Enter*. Here, you are committing the changes to the buildspec file you performed moments ago. You should get a confirmation of the changes.
+   
+   8. Next, type in the `git push` command and press *Enter* to push the code to AWS CodeCommit. You may be prompted to enter your passphrase if you created one earlier. You should then see a confirmation of the `git push` operation, as per *Figure 12.42*:

![](bbox=[0.146, 0.644, 0.706, 0.904])

Figure 12.42 – Git push of updated code files