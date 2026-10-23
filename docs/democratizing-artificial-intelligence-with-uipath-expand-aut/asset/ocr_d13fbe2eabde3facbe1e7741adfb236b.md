When presented with the Classification Station, the user will have the ability to review and correct classifications performed by automation. In the Classification Station, the user can rearrange pages by clicking and dragging pages, and validate classifications performed by classifiers (Figure 6.24):

![](bbox=[0.13, 0.17, 0.87, 0.48])

Figure 6.24 – Validating the classified invoice using the Classification Station

Important Note
More information on the Classification Station can be found in the UiPath docs: https://docs.uipath.com/activities/docs/present-classification-station#using-classification-station.

With our classification validation completed with the Classification Station, we can leverage our human validation to retrain and strengthen the classifiers used. In the next section, we'll discuss how to train classifiers.

# Training classifiers

One of the great features of the Document Understanding framework is the ability to close the feedback loop and retrain classifiers after validation. Leveraging human input is a great way for automation to teach itself and improve performance for future executions. The training of classifiers is performed through the Train Classifiers Scope activity, as shown in the following screenshot: