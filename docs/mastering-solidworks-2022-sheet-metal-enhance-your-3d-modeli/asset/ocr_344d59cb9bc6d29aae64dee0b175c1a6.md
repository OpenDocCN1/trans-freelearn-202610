# Using the Insert Bends tool with Shelled parts

So far in the book, we have covered most of the SolidWorks Sheet Metal tools, and in almost every case, we have started our models using the Base Flange feature. However, it's also possible to convert existing solid models into Sheet Metal models. This can be done in two main ways: by using the Insert Bends tool or by using the Convert to Sheet Metal tool. While these two tools are similar, they do have some important differences, which will be explained in this chapter and Chapter 12, Building Sheet Metal Parts Using the Convert to Sheet Metal Tool, respectively.

Should I Use a Base Flange or Convert a Solid Model to Sheet Metal?
Generally, starting a Sheet Metal model using a Base Flange is the better option, because it gives you flexibility if changes are required at a later stage. At times, it may seem like creating a solid model and then converting it to Sheet Metal will be faster, but in the long run, once any future modifications are taken into account, it is often better to start with a Base Flange.

The Insert Bends and Convert to Sheet Metal tools are generally more useful when working with pre-existing models that you may have received from a different source, or previously made yourself.

The Insert Bends tool allows us to convert solid models – such as Shelled or Thin Feature parts – to Sheet Metal parts. It works best with parts that have a uniform (that is, constant) wall thickness. If your model is completely solid or has a variable wall thickness then it is usually better to use the Convert to Sheet Metal tool, which is covered in Chapter 12, Building Sheet Metal Parts Using the Convert to Sheet Metal Tool.

To demonstrate the use of the Insert Bends tool, first, we are going to create a simple Shelled model:

+   1. Start a new part document and create a solid cube that is 100 mm long on each side. This can be made by sketching a Center Rectangle on the Top Plane, then using the Extruded Boss/Base tool and the Mid Plane End Condition.