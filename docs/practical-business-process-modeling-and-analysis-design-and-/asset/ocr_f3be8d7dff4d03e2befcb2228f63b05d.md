A second, slightly less drastic occurrence of the error related to message exchanges is seen at the end of **Pool 1**, where **Task D** and **Task E**, both of which are connected with a sequence flow, are marked as send and receive, respectively.

This would only make sense if we were sending something to **Pool 2** and receiving from it, but we see nothing to indicate such use. Therefore, this isn’t correct – we aren’t allowed to send messages within a single pool, even if the reasoning goes: “But after all, I’m sending an email to colleagues in another department, and they receive it and get acquainted with its content.”

The pools and lanes that we’ve become better acquainted with in this section can sometimes be extremely useful for conveying very important information about a process – that is, who’s involved in the process and who’s responsible for each step of the process. However, we might want to include some more information in our diagram.

# # Adding more information to your diagram with artifacts

As we already know, artifacts are used to convey more information about our diagram without affecting the way it’s interpreted and executed. In this section, we’ll learn more about **text annotations** and **groups**.

# ## Text annotations

The most used type of artifact is text annotation, which allows you to insert text into your diagram.

This annotation is quite simple – it always has the shape of an opening square bracket, followed by text. Depending on our needs, we can only change the size of the object so that the text takes up more or less space.

![](bbox=[0.3945, 0.585, 0.6055, 0.6828])

Despite this simplicity, annotations can be very useful. First, the text they contain is always visible within the diagram (unless, of course, we shrink the object so that the text no longer fits in the available space), making it easy to spot on the printout and accessible to viewers who aren’t BPMN experts and don’t realize that many objects may have additional, invisible information (descriptions, but not only). Thus, annotations allow us to convey relevant information relating to the context of the process.