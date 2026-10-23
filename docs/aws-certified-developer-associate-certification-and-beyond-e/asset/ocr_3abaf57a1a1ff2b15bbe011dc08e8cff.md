Figure 12.31 – Navigating to the root of the frontend code folder

# Important Note

Before uploading this updated version of the frontend code, you must amend the contents of the index.html file. Open the HTML code in your favorite editor, such as Visual Studio Code, and navigate toward the end of the file. In my Visual Studio Code, these are line numbers 906 and 907, as per Figure 12.32.

```
iaasacademycreator@IaaSs-MacBook-Air codecommit % ls
backend        frontend
iaasacademycreator@IaaSs-MacBook-Air codecommit % cd frontend
iaasacademycreator@IaaSs-MacBook-Air frontend % ls
todo-plus-frontend  todo-plus-frontend.zip
iaasacademycreator@IaaSs-MacBook-Air frontend % cd todo-plus-frontend
iaasacademycreator@IaaSs-MacBook-Air todo-plus-frontend % ls
api.js         images          package-lock.json
assets         index.html      package.json
buildspec.yml  index.js        util-aws-sdk.js
constants.js   main.js
iaasacademycreator@IaaSs-MacBook-Air todo-plus-frontend %
```

```
Users > iaasacademycreator > Desktop > codecommit > frontend > todo-plus-frontend > <> index.html > body
313   <body>
899   <!-- Vendor JS Files -->
900
901
902   <!-- Template Main JS File -->
903   <script src="main.js"></script>
904
905   <script>
906     window.cognitoClientId = "2ip6ded8t1buteqdtu8isf7120" // Put your client id here
907     window.BASE_URL = "/development"; // Put your base path here (if different)
908   </script>
909
910   <script type="module" src="index.js"></script>
911
912   </body>
913
914   </html>
```

Figure 12.32 – The frontend index.html file

You must replace the existing Cognito sample ID provided in the quotation marks with the Cognito client ID you created in Chapter 2, Securing Access with AWS Identity and Access Management.