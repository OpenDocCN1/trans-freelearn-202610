So, you start with the singers: you set up a singers’ queue and a not-singers’ queue, which includes all the other acts. The singers perform, and the judges evaluate them. Once done, you switch the panel to judge dancers. Now, the dancers’ queue is formed, and everyone else, including the previously judged singers and the yet-to-be-judged magicians and comedians, goes into the not-dancers’ queue. This process repeats until each category of performers has been judged by their specialized panel.

This approach mirrors the OvR classification strategy used in machine learning. In a multi-class classification problem, the OvR method involves training a separate binary classifier for each class to distinguish that class from all other classes. For instance, one classifier might be trained to identify singers by distinguishing them from non-singers (dancers, magicians, comedians), another to identify dancers from non-dancers, and so on. Each classifier’s task is to focus on separating its target class from the rest, much like setting up a dedicated judging panel for each type of talent show act. This way, despite having multiple categories, you efficiently manage to evaluate each one using a series of binary, or two-group, decisions:

![](bbox=[0.235, 0.365, 0.765, 0.585])

Figure 5.8 – Working mechanism of the OvR classifier

# Applications

Here are three notable applications of the OvR classifier:

+   - **Text classification**: OvR is commonly used in classifying documents or emails into multiple categories based on their content. Each category is treated as a separate class, and OvR classifiers are trained to identify documents belonging to each category against all others.
+   - **Image recognition**: In scenarios where each image needs to be classified into one of multiple categories (for example, animals, vehicles, and landscapes), OvR can be applied to train individual classifiers to recognize each category against all other images.
+   - **Medical diagnosis**: OvR classifiers can be employed to diagnose multiple types of diseases from medical images or patient data, where each disease represents a distinct class, and the classifier for each disease distinguishes it from healthy conditions and other diseases.