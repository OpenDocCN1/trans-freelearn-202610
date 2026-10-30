
With this default configuration, the learner can view the course modules in any order, but all the modules must be marked as *completed* for the course to be considered *complete*.

- 3. Change the **Sequencing of Modules** option to **Ordered**.
- 4. Open the **Mandatory Modules** drop-down list and choose 6.
- 5. Click the **Save** button in the top-right corner of the page when you're done.

With this new configuration, the learner must review the modules in the order they appear in the course. Remember that you can drag and drop the modules in the correct order here on the course's **Overview** page when you are logged in as an Author.

You also decided that the course is considered *complete* when the learner has reviewed at least six modules. In this example, the course consists of eight modules, so the first six modules are required, while the last two modules are considered optional add-ons. These two options (**Sequencing of Modules** and **Mandatory Modules**) are very powerful and allow for many variations.

Before moving on, remember that the *completion* criteria are not the same as the *success* criteria. In other words, you can *complete* a course but *fail* some or all of the interactive modules within it. In such a case, you want to define how Captivate Prime manages multiple attempts.

## Allowing multiple attempts

Unfortunately, the **Multiple Attempts** option is *not enabled by default*. So, you need to go back to the Administrator role first to enable this option for your account. Then, you must return to the Author role to review it in detail. Let's take a look:

- 1. Click the user icon located in the top-right corner of the screen.
- 2. Select the **Administrator** role. After Captivate Prime reloads, you will be logged in as an Administrator.
- 3. From the Administrator home page, click the **Settings** link located at the end of the **Configure** section of the left sidebar.
- 4. Then, click the **General** link in the left sidebar. This action opens the **General** settings page.
- 5. Locate the **Multiple Attempts** option and select the **Enable** checkbox. This is the action that enables the advanced *success* and *completion* options for Authors creating courses.
- 6. Click the user icon located in the top-right corner of the screen and return to the **Author** role.

- 7. From the Author home page, click the **courses** link in the **Learning** section of the left sidebar.
- 8. Click the **Draft** tab to access the courses currently in the draft state.
- 9. Hover your course card and click the **View Course** link.
- 10. Notice the new **Allow multiple attempts** option at the top of the **Modules** section of the course's **Overview** page, as shown in the following screenshot:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_161_0.png)

Figure 4.18 – The Allow multiple attempts option appears at the top of the Modules section of the form

- 11. Toggle the **Allow multiple attempts** option on (this turns a **Retake** button on in the learner experience when the learner fails an interactive module).
- 12. Take some time to examine the available options. Make sure you do *not* save your changes when you're done.

While examining these options, keep in mind that they only apply to the *interactive* modules of your course. As discussed in *Chapter 3, Uploading Learning Content and Managing the Content Library*, interactive content modules are those that are created using specialized eLearning authoring tools (such as Adobe Captivate, Articulate Storyline, Lectora, and iSpring) and uploaded to Prime as AICC-, SCORM-, or xAPI-compliant packages:

- - The **Set Attempts at** drop-down list allows you to adjust these options at the *course* level or at the *module* level. If you choose to set these options at the course level, this means that you will apply the same values to all the interactive modules of your course. If you choose to set these options at the module level, you can adjust these options for each interactive module separately.
- - The **Stop new attempt once module is completed & passed** option hides the **Retake** button as soon as the learner has both *completed* and *passed* the interactive course module.

- - The **Lock module between attempts** option allows you to block access to a module for a certain amount of time. The learner is unable to start the next attempt until the lock time has elapsed.
- - The **Set time limit to complete module** option allows you to specify a time limit for completing the module. If the learner does not complete the module within this time limit, the attempt automatically ends with an *Incomplete* status.

These advanced options allow you to implement a very large array of instructional design strategies. Of course, you don't need to go through all these options every time you create a course. Sometimes, the default options will suit you just fine.

In this section, you learned how to specify the success and completion criteria of a course. Before moving on to publishing the course, you will learn how to define multiple language versions of the same course.

## Defining multiple language versions of a course

In *Chapter 1, Introduction to Adobe Captivate Prime*, you learned that the LMS is available in many languages and that it differentiates the *interface language* from the *content language* delivered to the learner.

In *Chapter 3, Uploading Learning Content and Managing the Content Library*, you learned how to upload multiple language versions of the same module and how Captivate Prime chooses which language version to deliver to each learner.

The same basic reasoning applies to courses. The original language of the course is the same as the default language of the platform (English (United States), in this example), but you can specify various information such as **Course Name**, **Short Description**, **Detailed Overview**, and more in different languages using a process very similar to the one we used to define multiple language versions for a module. Let's take a look:

- 1. Make sure you are logged in as an Author and return to the **Overview** page of the course you want to add language versions to.
- 2. Click the **Add New Language** link located next to the **English (United States)** tab at the very top of the form:

Fundamentals of cybersecurity

Fill in basic information to create Course overview. Click 'Save'. Proceed to modify Settings and publish Course

English (United States) Add New Language

Course Name *
Fundamentals of cybersecurity

Brief Description
This appears on the Course Card.
Learn the fundamentals of cybersecurity and the best practices in the workplace and at home.
0 characters left

Detailed Overview
This appears on your Course Page.
This course is designed to increase your awareness of the threats to your computer. You will learn the somewhat obscure vocabulary like "Ransomware", "Phishing attacks" or "Denial of Service" will be prepared to detect these threats before you become

Figure 4.19 – The Add New Language link

- 3. Select one or more additional languages from the list and click the Save button in the lower-right corner of the Select Languages box. This adds additional language tabs at the top of the course's Overview page, next to the English (United States) tab that was present by default.

- 4. Fill in the localized Course Name, Brief Description, and Detailed Overview details in every language tab.

- 5. Don't forget to click the Save button in the top-right corner of the Overview page when you're done.

With these additional language versions, each learner can browse the Course Catalog in his/her language. Remember that each learner can choose his/her preferred content language in his/her personal **Profile Settings** (see the *Modifying your account settings* section of *Chapter 1, Introduction to Adobe Captivate Prime*). With that in mind, imagine that a learner has chosen French as his/her preferred content language. In such a case, one of these situations can occur:

- - If a French version of the course is available, the French **Course Name, Short Description,** and **Detailed Overview** information will be presented to this learner when browsing the Course Catalog. When taking the course, the French version of the course module will be provided to this learner by default. If a French version is not available for a module, plan B is to serve the content in the default language of the platform (English (United States), in this example). In other words, for this learner to have a completely localized experience, you need to translate both the information of the course itself *and* all the modules included in the course.
- - If a French version of the course is not available, Captivate Prime falls back to providing the language version that corresponds to the default language of the platform (English (United States), in this example), so the learner sees **Course Name, Short Description, Detailed Overview,** and more in the default language. That being said, if one or more modules included in the course has a French version, that version of the module is presented to the learner.

After reading the section, your course is finally ready to be published, which is what you will review in the next section.

## Publishing a course

Normally, your course should still be in the *Draft* state. This means that it is currently invisible to learners searching the Course Catalog, so it is not yet open for enrollment. To make it an official component of the Course Catalog and to start accepting enrollments, you must *publish* the course by performing the following steps:

- 1. From the **Author** home page, click the **Courses** link in the **Learning** section of the left sidebar.
- 2. Click the **Draft** tab to access the courses currently in the draft state.
- 3. Hover over your course card and click the **Publish Course** link.
