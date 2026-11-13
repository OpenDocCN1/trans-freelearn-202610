the palette, but doing things this way is much less flexible and controllable.
Also, flexibility and control are the big benefits to fragments, as we will see
throughout this chapter. By creating a class that extends Fragment, we can make
as many fragments from it as we like.

In the project explorer, right-click on the folder that contains the MainActivity
file. From the context menu, navigate to New | Java class. In the Name field,
type SimpleFragment and click on OK.

# Tip

Note that there are options to create Fragment classes in various precoded states
to implement Fragment more quickly, but at the moment, they will slightly cloud
the learning objectives of this mini app.

In our new SimpleFragment class, change the code to extend Fragment. As you
type the code, you will be asked to choose a specific Fragment class to import,
as shown in the next screenshot:

![](bbox=[0.114, 0.47, 0.883, 0.665])

Choose the top option (as shown in the previous screenshot), which is a regular
Fragment class.

Now, add a single string variable called mystring and a Button variable called
myButton as members and override the oncreate method. Inside the oncreate
method, initialize myString to Hello from SimpleFragment. Our code so far
(excluding the package declaration) will look exactly like this next code:

```
import android.app.Fragment;
```