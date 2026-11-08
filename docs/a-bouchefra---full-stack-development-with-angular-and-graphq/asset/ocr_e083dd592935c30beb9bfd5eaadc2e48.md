3. Next, create a .gitignore file and add the following contents:

```
node_modules
.DS_Store
```

4. Next, open the lerna.json file and set version to "0.0.1":

```
{
  "packages": [
    "packages/*"
  ],
  "version": "0.0.1"
}
```

Then, open the package.json file and set version to "0.0.1":

```
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "lerna": "^4.0.0"
  },
  "version": "0.0.1"
}
```

For more information on Lerna, check out https://lerna.js.org/.

5. Next, head over to your GitHub account and create a new repository that you can push your project's code to. This process is simple, but if you encounter any issues, make sure that you consult the docs at https://help.github.com/articles/creating-a-new-repository/.

6. Then, you need to point your local repository to your remote repository using the following command:

```
git remote add origin <PUT_YOUR_REPO_URL_HERE>
```

7. Before progressing, let's create a branch so that we can differentiate between the code for each chapter. In your Terminal, run the following command:

```
git checkout -b chapter2
```

You should get the following output:

```
Switched to a new branch 'chapter2'
```