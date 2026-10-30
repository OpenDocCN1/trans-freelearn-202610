
Given the complexity of maintaining an up-to-date user directory, it makes little sense to duplicate all this data in Captivate Prime. That's why Captivate Prime provides a large number of connectors that allow you to integrate the LMS with any directory solution your organization uses.

Captivate Prime is designed to authenticate users through an external **Single Sign-On (SSO)** provider. This can be the SSO provider of your organization or Adobe's own SSO solution, also known as Adobe ID. The only exception to this general rule is the possibility for external users to use a Captivate Prime ID to authenticate to the platform.

Finally, Captivate Prime adopts a role-based approach to authorization. Six roles are automatically defined by Captivate Prime: **Administrator**, **Author**, **Integration Admin**, **Instructor**, **Manager**, and **Learner**. However, you can create your own custom roles to address a very wide range of complex use cases.

Now that all of these ingredients are in place, it is possible to start enrolling learners in courses. Captivate Prime offers many options for this purpose, addressing numerous instructional design strategies and business requirements. This is what you will review in the next chapter.

## 6 Enrolling Learners to Courses

In general, a recipe has two main parts: The *ingredients* and a second part called *Instructions, or Method.* After reading the previous chapters, the ingredients section of your digital learning recipe is complete. In this chapter, it's finally time to move on to the *Method* section and see how these ingredients can be combined for learning to occur.

The first question you will explore in this chapter is, *What do learners actually enroll in?* At first glance, the answer to this question seems obvious: learners enroll in courses. But upon closer inspection, Captivate Prime offers many nuances and variations, such as **course instances**, **Learning Programs**, **Learning Plans**, and **certifications**, all of which will be discussed in this chapter.

The second question to consider is, *Who can enroll learners to courses?* Again, there is an obvious answer: as we discussed earlier, Captivate Prime is designed as a learner-centric LMS, so learners typically enroll themselves. Therefore, in this chapter, you will examine the enrollment experience from the perspective of the learner. That being said, organizations are subject to regulations that require learners to take compliance courses; some professions require constant recertification, not to mention each organization's training and development strategy, which also plays a role in determining who should learn what.

I like to say that a successful learning and development strategy is where the desires of learners meet the needs of organizations. This is why administrators and managers also have a role to play when enrolling users in courses. In this chapter, you will review several enrollment workflows, such as administrator enrollment and manager approval. Of course, these various enrollment methods can be combined, which offers unmatched flexibility to learners, managers, and organizations alike.

By the end of this chapter, you will have a solid understanding of how learners and learning content can come together and how this helps organizations implement their overall learning and development strategy. To achieve this goal, this chapter will cover the following topics:

- Reviewing the course options that affect enrollment
- Creating course instances
- Taking advantage of Learning Plans and Certifications
- Self-enrolling to a course as a learner
- Enrollment by Administrators and Managers
- Automatic enrollment using Learning Paths

These are a lot of exciting new features to discover, so let’s jump right in!

### Technical requirements

In order to perform the exercises in this chapter, you need to meet the following technical requirements:

- You need Administrator and Author access to a working Captivate Prime account (it can be a trial account).
- In addition to the administrator account mentioned previously, you need access to at least three different learner accounts and one manager account.
- There must be at least one course containing both synchronous (live) and asynchronous (on-demand) activities defined on your account. At least one of these activities must have multiple delivery methods enabled.

If you have completed the exercises in the previous chapters, you should be ready to begin this chapter immediately. If you have not read the previous chapters, make sure you meet the aforementioned requirements before proceeding. You can also read through the steps without doing the practical exercises.

The steps described in the exercises in this chapter are based on the learning content and user profiles defined in the previous chapters of this book. Feel free to adapt the steps to your situation if you have not read the previous chapters.

### Reviewing the course options that affect enrollment

Back in Chapter 4, Creating Skills and Courses, you created a custom course in your Captivate Prime account. At the time, you went through almost all the options relating to course creation, except for a few properties related to defining the preferred target audience for the course and to how learners enroll in the course. Let’s return to the Author role to explore these remaining options using the following steps:

1. Make sure you are logged in as an Author in your Captivate Prime account.
2. From the Author home page, click the Courses link located at the top of the Learning section in the left sidebar.
3. Hover over the card of the course you created Chapter 4, Creating Skills and Courses.
4. Click the View Course link to open the course's details.

> **Note**
> If you did not perform the exercises in Chapter 4, Creating Skills and Courses, you can use any custom course available, providing that the chosen course contains both synchronous and asynchronous activities, and that at least one of the course activities has multiple delivery methods enabled.

5. Click the Settings link located in the left sidebar.

This takes you to the settings page of your course, where the Enrollment Type, Unenrollment, and Who should take this Course fields are available. These are the three fields that were not discussed in Chapter 4, Creating Skills and Courses. Well, the time has come to discuss them in more detail.

6. Click the Edit link located in the top-right corner of the screen.
7. Open the Enrollment Type drop-down list and inspect the available options.

There are three options available in the Enrollment Type drop-down. Self Enrolled is the default value. Using this option, learners can browse the course catalog and enroll themselves in courses. As a learner-centric LMS, this is exactly the type of experience Captivate Prime aims to promote, so it’s not surprising that this is the default.

When using the **Manager Nominated** option, learners cannot enroll themselves because only managers can enroll their team members in the course.

Finally, with the **Manager Approved** option, learners can browse the course catalog to send an enrollment request to their manager for approval.

8. Choose **Self-enrolled** from the **Enrollment Type** dropdown.

### Changing the enrollment type

When changing **Enrollment Type**, you cannot change to a more restrictive option. For example, changing from **Self Enrolled** to **Manager Approved** is not possible. When in doubt, choose a restrictive **Enrollment Type** when you create the course, as this allows you to switch to a more permissive option later. As a workaround, you can also duplicate the course, choose a more restrictive **Enrollment Type** for the copy, and retire the original.

9. Leave the **Unenrollment** checkbox deselected.

10. Scroll down to the bottom of the page until you see the **Tags** and **Who should take this Course?** options.

As discussed in the *Working with tags* section of *Chapter 3, Uploading Learning Content and Managing the Content Library*, tags are keywords that help the Captivate Prime search engine index the course content to make it more discoverable. Just as you can associate tags with modules, you can also associate tags with courses.

In the **Who should take this course** field, you can define the preferred target audience of the course by supplying one or more profiles. As discussed in *Chapter 5, Managing Users*, this profile is one of the fields that can be found in the **Add User** form. Most of the time, the profile corresponds to the *job title* of the learner.

11. Start typing the name of an existing profile in the **Who should take this Course?** field. Click the desired profile when it becomes visible in the list. You can repeat this operation and add multiple profiles. You can also easily remove the chosen profiles.

For this example, make sure you leave the **Who should take this Course?** field empty.

12. Finally, click the **Republish** button in the top-right corner of the screen to save your changes.

The course **Settings** page should now look as follows:
