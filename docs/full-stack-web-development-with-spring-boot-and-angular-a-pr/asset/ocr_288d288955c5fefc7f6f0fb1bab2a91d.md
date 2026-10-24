After successfully creating the form arrays, we will now have a view of the form:

![](bbox=[0.254, 0.142, 0.734, 0.659])

Figure 11.1 – Hero form with a form group and form arrays

We have successfully created reactive forms using form groups and form arrays. Now, we will use the FormBuilder service to simplify the syntax in creating forms in our application.

# Using the FormBuilder service to generate controls

In the previous section, we successfully created reactive forms using form groups, form arrays, and form controls. However, as we can see from the syntax, creating forms becomes repetitive. We are always instantiating new instances of form controls, form arrays, and form groups, and this is not ideal in larger forms. FormBuilder provides the solution for this issue.