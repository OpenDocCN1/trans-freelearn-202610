
Author(s)
作者姓名和个人资料图片会显示给访问此学习的全部学员。请确保您已获得作者的同意。
Damien Bruyndonckx

Enrollment Type *
自行报名

Unenrollment
学员可以自行取消报名

Prerequisites
推荐课程。非强制性。
Adobe Captivate Prime 学习者体验

Job Aids
快速参考资料，用于在课程期间和之后补充学习
未添加任何工作辅助

Tags
学员可以使用标签查找相关课程。
未添加任何标签

Who should take this Course?
指定个人资料/目标受众。
Profile

Figure 6.1 – 课程的报名相关选项在课程设置页面可见

Thanks to the above steps, you should have a better understanding of the enrollment options available. You also learned how to define a preferred target audience for each course. By doing so, Captivate Prime gives courses a more prominent place that matches the learner's profile when the learner browses the course catalog.

In the next section, we will discuss course instances and define the needed course instances for your custom course.

## Creating course instances

To understand course instances, it is necessary to remember that a Captivate Prime course can include both synchronous and asynchronous modules. As a reminder, asynchronous activities are self-paced modules such as videos, online quizzes, PowerPoint presentations, PDF files, and others that learners can review at their convenience, while synchronous activities require learners and instructors to be present together at the same time, either physically in a classroom or lecture hall or online using a video conferencing system.

But there is another major difference between *synchronous* and *asynchronous* activities: *synchronous* activities are time-bound, while *asynchronous* activities are not.

Let's look at a quick example. Imagine it's early April and you're creating a course that includes a virtual classroom activity scheduled for, say, mid-May. If learners register for the course before the end of April, all is well. But if they register later, they may not have enough time to complete the required prework before attending the mid-May class. Even worse: what happens if learners enroll in the course *after* the scheduled class date?

Another example is when your organization wants to offer the same synchronous activity in various countries and/or in different languages. In such a case, you need to schedule several instances of the same (live or virtual) class in different locations, time zones, languages, and, possibly, with different instructors. In the case of a virtual class, each instance can use a different (Zoom, Connect, Teams) URL and/or different instructor(s).

You can also use course instances with courses that consist of self-paced modules only. This can be very useful for reporting and feedback purposes. Feedback and reporting will be covered in *Chapter 9, Configuring and Using Feedback*, and *Chapter 10, Reporting in Captivate Prime*.

To address these use cases, you need to define **course instances**. Learners enroll in a course instance rather than the course itself.

In this section, you will create two instances for the custom course you defined in *Chapter 4, Creating Skills and Courses*, but first, let's remember what modules this course is made up of by performing the following steps:

- 1. Make sure you are logged in as an Author in your Captivate Prime account.
- 2. From the Author home page, click the **Courses** link located at the top of the **Learning** section in the left sidebar.
- 3. Hover over the card of the course you created *Chapter 4, Creating Skills and Courses*.
- 4. Click the **View Course** link to open the course's details.
- 5. Scroll down the **Overview** page until you see the **Modules** section.

As depicted in the following screenshot, the course is made up of several modules, one of which (identified by the arrow in the following screenshot) is a classroom module. Remember that classroom modules are *synchronous* activities. The other modules included in this course are all *asynchronous* (self-paced) activities:

Content: * Core Modules that make up the Course.

|  |  |  |
|---|---|---|
| Theory of file extensions (Part 1) Multiple Delivery Types |  |  |
| Learn the file extensions Self Paced | 4 mins 39 secs |  |
| Identifying Phishing Attempts Self Paced | 7 mins 53 secs |  |
| Combat Phishing - Check the email address of the sender Self Paced | 1 min 30 secs |  |
| Phishing explained - Youtube Video Activity | 3 mins |  |
| Cybersecurity Live class Classroom |  |  |
| Your cybersecurity essay Activity | 1 hr |  |
| Cybersecurity - Checklist Activity |  |  |

Figure 6.2 – 课程中包含的一个模块是同步课堂模块

Also, note the two activity modules included at the end of the course. Both of these activity modules require an instructor to either assess the assignment that's submitted by the learner or to fill out the checklist.

> **Note**
> File submissions and checklist activities were discussed in *Chapter 4, Creating Skills and Courses.*

Finally, the first course module in the preceding screenshot (identified by the oval shape) has **Multiple Delivery Types** assigned. To review which delivery types have been assigned to this particular module, it is necessary to perform the following steps:

- 6. Click the **Edit** button located in the top-right corner of the page.
- 7. Click the **Modify Delivery Types** link associated with the **Theory of file extensions (Part 1)** module.

As shown in the following screenshot, this module can be a **Self Paced Module** or a **Classroom Module**:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_236_0.png)

Figure 6.3 – 该模块可以是自定进度模块或课堂模块

To relate this situation to a real-world use case, imagine that you have only English-speaking instructors who can teach this particular class. English-speaking learners can therefore enjoy an instructor-led course module, while learners speaking other languages must watch a self-paced recording of the course with subtitles in their language. Or let’s imagine the instructor who used to teach the course no longer works for the organization, so what used to be an instructor-led module is now a self-paced module using a recording of an earlier lecture.

Whatever the actual reason, the point is that the same course module can be assigned multiple delivery types.

- 8. Before moving on to the next section, make sure you close the **Module Details** dialog and **Cancel** any changes you may have made to the course.

Now that you have the details of this course back in mind, it is time to turn your attention to creating course instances.

## Creating course instances

Creating course instances is something both administrators and authors can do. For this example, let's do this as an administrator by performing the following steps:

- 1. Make sure you are logged in as an Administrator.
- 2. From the Administrator home page, click the **Courses** link located in the **Learning** section of the left sidebar.
- 3. Hover over the card of the course you created *Chapter 4, Creating Skills and Courses*, and click the **View Course** link to open the course's page.
- 4. Click the **Instances** link located at the top of the **Manage** section of the left sidebar.

As shown in the following screenshot, a **Default instance** should already be defined. Depending on the situation, this default instance may already be *retired* (see the arrow in the following screenshot). This means that the date you chose for the classroom activity when creating the course has already passed, which is the case in the following screenshot:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_237_0.png)

Figure 6.4 – 应已定义默认实例

If the course only contains *asynchronous* self-paced modules, this **Default instance** is probably all you need. But since this course contains a time-bound *synchronous* classroom activity, it is necessary to create course instances before accepting enrollments.

You will now create two additional instances for this course: one for the North American region and one for the Europe Middle East Africa (EMEA) region. Perform the following steps:

- 5. Click the **Add New Instance** link located in the top-right corner of the **Instances** page.
