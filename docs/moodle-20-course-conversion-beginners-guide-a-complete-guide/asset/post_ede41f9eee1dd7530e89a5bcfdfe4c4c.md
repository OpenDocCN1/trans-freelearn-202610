
## Enhancing your Teaching

I'm not going to enforce course (rather than activity—we'll come on to that shortly) completion tracking on my course, put simply; mine isn't that kind of course. There are, however, good examples of where you would want course completion tracking enabled by default: one is any Moodle course in work-based learning (rather than in a school). Imagine my course was on manual handling—a mandatory course that all new employees had to sit through... and pass. That our students had completed every activity and had indicated that they had read and digested every static resource (through the completion tracking feature) then becomes an important part of their employee record.

Scroll down to the bottom of the Edit course settings page and press the Save changes button.

In the Settings block, under Course administration a new link has been added: Completion tracking. Click on that now to display the Edit course completion settings page.

Because I'm not going to force the completion tracking feature on my students from the moment they enroll, I'm going to enable the Manual self completion feature:

![](img/1529021fde33ef551603bbc71587313e_307_0.png)

and that's it for this screen. For more information on the many other settings on this page, remember to check out the Moodle docs at http://docs.moodle.org/20/en/course/completion. I'm going to scroll down to the bottom of the page and press the Save changes button.

The final step is to add the Self completion block onto the course page, because without it course completion tracking isn't fully enabled the way I want it. Ensure you have editing turned on and from the Add a block select Self completion from the drop-down list:

![](img/1529021fde33ef551603bbc71587313e_307_1.png)

Once added, I'm going to move the Completion tracking block to the left of the page, just under the Navigation block:

![](img/1529021fde33ef551603bbc71587313e_308_0.png)

That now allows students to state (in Moodle) when they consider that they have finished the course. We'll come back to this block later in this chapter. But what about individual activities? That needs to be configured on a per activity basis. For example, how do I allow students to mark that they have read the Sources of Energy handout?

### Time for action – enabling completion tracking on an activity

1. With editing turned on, click on the Update icon next to the Sources of Energy link.
2. Now that we have completion tracking enabled on our course, we can now scroll down to the **Activity completion** section:

   ![](img/1529021fde33ef551603bbc71587313e_308_1.png)

3. From the Completion tracking drop-down menu, I'm going to select **Students can manually mark the activity as completed**:

   ![](img/1529021fde33ef551603bbc71587313e_309_0.png)

4. Scroll down to the bottom of the page and press the Save and return to course button.

5. That's it! We're done.

### What just happened?

Now we've returned to the course page, track across to the right of the Sources of Energy resource and you'll see a little gray check mark:

![](img/1529021fde33ef551603bbc71587313e_309_1.png)

This is what completion tracking looks like from a student's point of view. For example, once he considers it read, John Smith can click on the checkbox:

![](img/1529021fde33ef551603bbc71587313e_309_2.png)

And I can track which students have indicated they have read the Sources of Energy handout through course reports. In the Navigation block, under My courses, click on the short name for your course and select Reports. Then, click on Activity completion:

![](img/1529021fde33ef551603bbc71587313e_310_0.png)

You'll see a list of your enrolled students and an indication of who has completed which activity:

![](img/1529021fde33ef551603bbc71587313e_310_1.png)

Check out the activity completion report download options (see previous screenshot). These are really useful if you need to analyse activity completion data in more detail, for example in Excel.

### Automatic activity completion

In my course, I'm happy for my students to indicate that they have completed an activity. The dynamic in my group of students is such that it quickly becomes obvious when someone hasn't done their homework (mainly due to the fact that it's a small group). What about having Moodle mark an activity as complete? Moodle can automatically mark an activity as complete but, obviously, what you define as "complete" depends on the activity. For example, for my Sources of Energy handout, Moodle can record the activity as complete when a student has viewed the handout:

![](img/1529021fde33ef551603bbc71587313e_311_0.png)

However, for a forum activity we can specify the number of posts a student must make in order to "complete" the task. We can also distinguish between starting new discussions and replying to other posts:

![](img/1529021fde33ef551603bbc71587313e_311_1.png)

Note that we can also specify a completion deadline to really keep our students on their toes.

Rather than attempting to describe the raft of different completion options for all of the resources and activities available in Moodle, I'm going to direct you to the Moodle docs at http://docs.moodle.org/20/en/Teacher_documentation.

### More on course completion

I have my course configured so that students can indicate when they think they have completed it—by clicking on **Complete course** in the **Self completion** block:

![](img/1529021fde33ef551603bbc71587313e_312_0.png)

Clicking on the link allows the student to specify whether they have completed the course (two buttons are displayed, "Yes" and "No"). But now that I have started to configure activity completion on individual tasks in my course (for example, reading the Sources of Energy handout and posting to a forum), now I can, if I wish, configure automatic course completion. In the **Settings** block, under **Course administration**, click on **Completion tracking**. Scroll down to the **Activities completed** and you will see listed those tasks that have activity completion enabled:

![](img/1529021fde33ef551603bbc71587313e_312_1.png)

Note that students can still record that they have completed the course if you leave the Self completion block on your course page.

### Conditional activities

Of course, guided learning isn't just about us insisting that our learners have to understand one concept before being allowed to attempt an activity (for example, there's no point trying to learn about calculus if you don't understand rates of change). In much the same way that in the classroom we guide our students to other activities they might find interesting—or will enrich their current understanding—we can do the same in Moodle. Also, don't necessarily think of conditional activities as being a replacement for the lesson activity. Lessons are primarily for exploring scenarios. Conditional activities are for learning journeys.

So, how do we configure access to one activity being conditional on completing another? Firstly, you have to ensure that conditional activities is enabled on your Moodle. Check out the Ask the admin section at the end of this chapter. Let's assume I want to ensure students have read the Sources of Energy handout before they get access to the Firing a bullet from a gun link:

![](img/1529021fde33ef551603bbc71587313e_313_0.png)

We can control access to tasks based on grade, date, or completion. I want to ensure that the Sources of Energy handout has (at the very least) been clicked on to view so I'm going to configure access to Firing a bullet from a gun based on the student having viewed Sources of Energy. Note that I'll need Activity completion enabled on the Sources of Energy handout to be able to follow the steps outlined in the next Time for action.

### Time for action – configuring conditional activities

1. With editing turned on, click on the Update icon next to the Firing a bullet from a gun resource.
