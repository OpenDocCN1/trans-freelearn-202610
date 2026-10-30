In the following code snippet, I have copied some code from the http://matplotlib.org/ website, which creates this incredible chart. I have also added some comments to the code. There are many examples available on this site, and I encourage you to try them out until you find the kind of charts you would like to create.

Here is the code to create the chart in less than 25 lines of Python code, including white spaces:

```
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()                                    # create a figure
ax = fig.gca(projection='3d')                        # create a 3-dimensional axis
X = np.arange(-5, 5, 0.25)                            # horizontal range
Y = np.arange(-5, 5, 0.25)                            # vertical range
X, Y = np.meshgrid(X, Y)                              # create a special grid
R = np.sqrt(X**2 + Y**2)                              # calculate square root
Z = np.sin(R)                                         # calculate sinus

## use #@UndefinedVariable below to ignore the error for cm.coolwarm
surf = ax.plot_surface(X, Y, Z, rstride=1, cstride=1,
                       cmap=cm.coolwarm, linewidth=0,
                       antialiased=False)
ax.set_zlim(-1.01, 1.01)                              # z-axis is third dimension

ax.zaxis.set_major_locator(LinearLocator(10))
ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))

fig.colorbar(surf, shrink=0.5, aspect=5)

plt.show()                                            # display the figure
```

![](bbox=[0.1, 0.7, 0.2, 0.78])

Running the code using Python 3.6 or above with the Eclipse PyDev plugin might show some unresolved import and variable errors. You can simply disable these in Eclipse via #@UnresolvedImport and #@UndefinedVariable.

Just ignore those errors if you are developing using Eclipse, as the code will run successfully.