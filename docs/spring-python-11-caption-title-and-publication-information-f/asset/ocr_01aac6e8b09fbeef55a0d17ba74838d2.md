# Creating Skeleton Apps with Coily

3. Let's use the plugin to create a CherryPy application called `sample_app`. Since we just installed it, the plugin now shows up on `coily`'s help menu. With that in place, we can then run the command to create `sample_app`.

![](bbox=[0.175, 0.205, 0.825, 0.525])

`gen-cherrypy-app` creates a directory named `sample_app` and copies the files listed above into it. It also does some transformations of the files, based on the argument `sample_app`.

+   - It replaces all instances of `${name}` in each file with `sample_app`
+   - It replaces all instances of `${properName}` in each file with `Sample_app`
+   - It renames `cherrypy-app.py` as `sample_app.py`